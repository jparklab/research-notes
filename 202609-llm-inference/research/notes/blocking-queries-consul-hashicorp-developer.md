---
title: Blocking Queries | Consul | HashiCorp Developer
id: blocking-queries-consul-hashicorp-developer
tags:
- llmd-router-nonk8s-backends-54b774
- locus-custom-discovery-plugin-design
- consul
- reference-design
created: '2026-09-11T21:25:40.957314Z'
updated: '2026-09-12T03:37:37.781189Z'
source: https://developer.hashicorp.com/consul/api-docs/features/blocking
source_domain: developer.hashicorp.com
fetched_at: '2026-09-11T21:25:40.933402Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Official Consul blocking-queries semantics. Mechanism: endpoints that support
  blocking return header X-Consul-Index (a raft-index snapshot marker); client resends
  the same request with query param index=<last X-Consul-Index> plus wait=<duration>
  (max 10m, default 5m, plus up to wait/16 jitter) and the server holds the HTTP request
  open until a change occurs past that index or the timeout elapses -- long polling,
  not a persistent stream. Quote: ''the return of a blocking request is no guarantee
  of a change'' (timeout or an idempotent write can return with no actual change).
  Implementation-detail requirements a client MUST follow: (1) reset index to 0 if
  a returned index is LOWER than the previous value (raft snapshot restore, KV list
  edge cases, Consul upgrades changing watch granularity) -- failing this can miss
  updates indefinitely or busy-loop; (2) sanity-check index > 0 after each response,
  since index=1 is always safe to request for not-yet-existent data; (3) rate-limit
  the blocking loop client-side (token-bucket, burst ~2) to avoid degrading into a
  busy loop when data churns fast, rather than sleeping (which delays delivery). Streaming
  backend (Consul 1.10+) is a materialized-view alternative to long polling, engaged
  automatically when index or cached params are set if the endpoint supports it; it
  omits X-Consul-LastContact/X-Consul-KnownLeader and marks responses with X-Consul-Query-Backend:
  streaming. A minority of agent-local endpoints (state not raft-indexed) instead
  use hash-based blocking: header X-Consul-ContentHash + query param hash=<value>,
  opaque (not orderable), otherwise same wait/timeout contract. RELEVANCE TO LOCUS:
  this is the direct analog to a poll-vs-watch decision for a Consul-backed EndpointDiscovery
  plugin -- a plugin would issue a blocking GET against /v1/health/service/:service
  with index tracking (see companion note health-http-api-consul-hashicorp-developer
  for the passing=true health-filter parameter), not a bare interval poll, to get
  near-real-time updates without tight polling.'
---

[Skip to main content](https://developer.hashicorp.com/consul/api-docs/features/blocking#main)
[HashiConf 2025 Don't miss the live stream of HashiConf Day 2 happening now View live stream](https://www.hashicorp.com/conferences/hashiconf#livestream)
Sign In[Sign Up](https://developer.hashicorp.com/sign-up)
Theme
Dark Light System
# Blocking Queries
Many endpoints in Consul support a feature known as "blocking queries". A blocking query is used to wait for a potential change using long polling. Not all endpoints support blocking, but each endpoint uniquely documents its support for blocking queries in the documentation.
Endpoints that support blocking queries return an HTTP header named `X-Consul-Index`. This is a unique identifier representing the current state of the requested resource.
On subsequent requests for this resource, the client can set the `index` query string parameter to the value of `X-Consul-Index`, indicating that the client wishes to wait for any changes subsequent to that index.
When this is provided, the HTTP request will "hang" until a change in the system occurs, or the maximum timeout is reached. A critical note is that the return of a blocking request is **no guarantee** of a change. It is possible that the timeout was reached or that there was an idempotent write that does not affect the result of the query.
In addition to `index`, endpoints that support blocking will also honor a `wait` parameter specifying a maximum duration for the blocking request. This is limited to 10 minutes. If not set, the wait time defaults to 5 minutes. This value can be specified in the form of "10s" or "5m" (i.e., 10 seconds or 5 minutes, respectively). A small random amount of additional wait time is added to the supplied maximum `wait` time to spread out the wake up time of any concurrent requests. This adds up to `wait / 16` additional time to the maximum duration.
##  Implementation Details
While the mechanism is relatively simple to work with, there are a few edge cases that must be handled correctly.
  * **Reset the index if it goes backwards**. While indexes in general are monotonically increasing(i.e. they should only ever increase as time passes), there are several real-world scenarios in which they can go backwards for a given query. Implementations must check to see if a returned index is lower than the previous value, and if it is, should reset index to `0` - effectively restarting their blocking loop. Failure to do so may cause the client to miss future updates for an unbounded time, or to use an invalid index value that causes no blocking and increases load on the servers. Cases where this can occur include:
    * If a raft snapshot is restored on the servers with older version of the data.
    * KV list operations where an item with the highest index is removed.
    * A Consul upgrade changes the way watches work to optimize them with more granular indexes.
  * **Sanity check index is greater than zero**. After the initial request (or a reset as above) the `X-Consul-Index` returned _should_ always be greater than zero. It is a bug in Consul if it is not, however this has happened a few times and can still be triggered on some older Consul versions. It's especially bad because it causes blocking clients that are not aware to enter a busy loop, using excessive client CPU and causing high load on servers. It is _always_ safe to use an index of `1` to wait for updates when the data being requested doesn't exist yet, so clients _should_ sanity check that their index is at least 1 after each blocking response is handled to be sure they actually block on the next request.
  * **Rate limit**. The blocking query mechanism is reasonably efficient when updates are relatively rare (order of tens of seconds to minutes between updates). In cases where a result gets updated very fast however - possibly during an outage or incident with a badly behaved client - blocking query loops degrade into busy loops that consume excessive client CPU and cause high server load. While it's possible to just add a sleep to every iteration of the loop, this is **not** recommended since it causes update delivery to be delayed in the happy case, and it can exacerbate the problem since it increases the chance that the index has changed on the next request. Clients _should_ instead rate limit the loop so that in the happy case they proceed without waiting, but when values start to churn quickly they degrade into polling at a reasonable rate (say every 15 seconds). Ideally this is done with an algorithm that allows a couple of quick successive deliveries before it starts to limit rate - a [token bucket](https://en.wikipedia.org/wiki/Token_bucket) with burst of 2 is a simple way to achieve this.


##  Streaming backend
The streaming backend, first introduced in Consul 1.10, is a replacement for the long polling backend. If streaming is supported by an endpoint, it will be used when either the `index` or `cached` query parameters are set.
When streaming is used, the Consul servers publish change events to a topic. Clients subscribe to a topic to receive these change events. The clients use the change events to build a local "materialized view". Clients are then able to handle requests using that view. Streaming improves on the traditional long polling between clients and servers by significantly reducing the quantity of data sent between them, while still allowing clients to handle read requests.
While streaming is a significant optimization over long polling, it will not populate the `X-Consul-LastContact` or `X-Consul-KnownLeader` response headers, because the required data is not available to the client.
When the streaming backend is used, API responses will include the `X-Consul-Query-Backend` header with a value of `streaming`.
##  Hash-based Blocking Queries
A limited number of agent endpoints also support blocking however because the state is local to the agent and not managed with a consistent raft index, their blocking mechanism is different.
Since there is no monotonically increasing index, each response instead contains a header `X-Consul-ContentHash` which is an opaque hash digest generated by hashing over all fields in the response that are relevant.
Subsequent requests may be sent with a query parameter `hash=<value>` where `value` is the last hash header value seen, and this will block until the `wait` timeout is passed or until the local agent's state changes in such a way that the hash would be different.
Other than the different header and query parameter names, the biggest difference is that hash values are opaque and can't be compared to see if one result is older or newer than another. In general hash-based blocking will not return too early due to an idempotent update since the hash will remain the same unless the result actually changes, however as with index-based blocking there is no strict guarantee that clients will never observe the same result delivered before the full timeout has elapsed.
[Edit this page on GitHub](https://github.com/hashicorp/web-unified-docs/blob/main/content/consul/v2.0.x/content/api-docs/features/blocking.mdx)
