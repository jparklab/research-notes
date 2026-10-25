---
title: Health - HTTP API | Consul | HashiCorp Developer
id: health-http-api-consul-hashicorp-developer
tags:
- llmd-router-nonk8s-backends-54b774
- locus-custom-discovery-plugin-design
- consul
- health-filtering
created: '2026-09-11T21:27:21.242055Z'
updated: '2026-09-15T04:55:23.948453Z'
source: https://developer.hashicorp.com/consul/api-docs/health
source_domain: developer.hashicorp.com
fetched_at: '2026-09-11T21:27:21.220521Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Official Consul Health HTTP API docs, fetched as a companion to the blocking-queries
  page (cross-referenced from it) specifically for health-check filtering semantics.
  Key parameter, on /v1/health/service/:service (and related health endpoints): ''passing
  (bool: false) - Specifies that the server should return only nodes with all checks
  in the passing state. This can be used to avoid additional filtering on the client
  side.'' This is a server-side filter -- the client does not need to fetch all instances
  and filter locally; it maps directly onto llm-d-router''s Kubernetes-discovery behavior
  of only Upserting pods that satisfy the Ready condition. A Consul-backed EndpointDiscovery
  plugin should therefore call /v1/health/service/:service?passing=true (combined
  with blocking-query index/wait params from the companion blocking-queries note)
  to receive only healthy endpoints on each poll/watch cycle, mirroring the ''only
  Upsert healthy endpoints'' contract. Also documents /v1/health/state/:state (state
  = any|passing|warning|critical) as an alternative check-centric view, and per-endpoint
  filter expressions (Status: Equal/Not Equal/In/Not In/Matches/Not Matches) for finer-grained
  filtering beyond the boolean passing shortcut.'
---

[Skip to main content](https://developer.hashicorp.com/consul/api-docs/health#main)
[HashiConf 2025 Don't miss the live stream of HashiConf Day 2 happening now View live stream](https://www.hashicorp.com/conferences/hashiconf#livestream)
Sign In[Sign Up](https://developer.hashicorp.com/sign-up)
Theme
Dark Light System
# Health HTTP Endpoint
The `/health` endpoints query health-related information. They are provided separately from the `/catalog` endpoints since users may prefer not to use the optional health checking mechanisms. Additionally, some of the query results from the health endpoints are filtered while the catalog endpoints provide the raw entries.
To modify health check registration or information, use the [`/agent/check`](https://developer.hashicorp.com/consul/api-docs/agent/check) endpoints.
##  List Checks for Node
This endpoint returns the checks specific to the node provided on the path.
The HTTP response includes the `X-Consul-Results-Filtered-By-ACLs: true` header if the response array excludes results due to ACL policy configuration. Refer to the [HTTP API documentation](https://developer.hashicorp.com/consul/api-docs/api-structure#results-filtered-by-acls) for more information.  
| Method  | Path  | Produces  |  
| --- | --- | --- |  
| `/health/node/:node`  | `application/json`  |  
The table below shows this endpoint's support for [blocking queries](https://developer.hashicorp.com/consul/api-docs/features/blocking), [consistency modes](https://developer.hashicorp.com/consul/api-docs/features/consistency), [agent caching](https://developer.hashicorp.com/consul/api-docs/features/caching), and [required ACLs](https://developer.hashicorp.com/consul/api-docs/api-structure#authentication).  
| Blocking Queries  | Consistency Modes  | Agent Caching  | ACL Required  |  
| --- | --- | --- | --- |  
| `node:read,service:read`  |  
###  Path Parameters
  * `(string: <required>)` - Specifies the name or ID of the node to query.


###  Query Parameters
  * `(string: "")` - Specifies the datacenter to query. This will default to the datacenter of the agent being queried.
  * [`filter`](https://developer.hashicorp.com/consul/api-docs/health#filter) `(string: "")` - Specifies the expression used to filter the queries results prior to returning the data.
  * `(string: "")`
Enterprise
- Specifies the namespace of the checks you lookup. You can also [specify the namespace through other methods](https://developer.hashicorp.com/consul/api-docs/health#methods-to-specify-namespace).


###  Sample Request

```
$ curl \
    --header "X-Consul-Namespace: *" \
    http://127.0.0.1:8500/v1/health/node/my-node

```

```
$ curl \
    --header "X-Consul-Namespace: *" \
    http://127.0.0.1:8500/v1/health/node/my-node

```

###  Sample Response

```


    "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
    "Node": "foobar",
    "CheckID": "serfHealth",
    "Name": "Serf Health Status",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "",
    "ServiceName": "",
    "ServiceTags": [],
    "Namespace": "default"


    "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
    "Node": "foobar",
    "CheckID": "service:redis",
    "Name": "Service 'redis' check",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "redis",
    "ServiceName": "redis",
    "ServiceTags": ["primary"],
    "Namespace": "foo"



```

```
    "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
    "Node": "foobar",
    "CheckID": "serfHealth",
    "Name": "Serf Health Status",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "",
    "ServiceName": "",
    "ServiceTags": [],
    "Namespace": "default"
    "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
    "Node": "foobar",
    "CheckID": "service:redis",
    "Name": "Service 'redis' check",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "redis",
    "ServiceName": "redis",
    "ServiceTags": ["primary"],
    "Namespace": "foo"

```

###  Filtering
The filter will be executed against each health check in the results list with the following selectors and filter operations being supported:  
| Selector  | Supported Operations  |  
| --- | --- |  
| `CheckID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Notes`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Output`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceName`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceTags`  | In, Not In, Is Empty, Is Not Empty  |  
| `Status`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
##  List Checks for Service
This endpoint returns the checks associated with the service provided on the path.
The HTTP response includes the `X-Consul-Results-Filtered-By-ACLs: true` header if the response array excludes results due to ACL policy configuration. Refer to the [HTTP API documentation](https://developer.hashicorp.com/consul/api-docs/api-structure#results-filtered-by-acls) for more information.  
| Method  | Path  | Produces  |  
| --- | --- | --- |  
| `/health/checks/:service`  | `application/json`  |  
The table below shows this endpoint's support for [blocking queries](https://developer.hashicorp.com/consul/api-docs/features/blocking), [consistency modes](https://developer.hashicorp.com/consul/api-docs/features/consistency), [agent caching](https://developer.hashicorp.com/consul/api-docs/features/caching), and [required ACLs](https://developer.hashicorp.com/consul/api-docs/api-structure#authentication).  
| Blocking Queries  | Consistency Modes  | Agent Caching  | ACL Required  |  
| --- | --- | --- | --- |  
| `node:read,service:read`  |  
###  Path Parameters
  * [`service`](https://developer.hashicorp.com/consul/api-docs/health#service) `(string: <required>)` - Specifies the service to list checks for.


###  Query Parameters
  * `(string: "")` - Specifies the datacenter to query. This will default to the datacenter of the agent being queried.
  * `(string: "")` - Specifies a node name to sort the node list in ascending order based on the estimated round trip time from that node. Passing `?near=_agent` uses the agent's node for the sort. This is specified as part of the URL as a query parameter.
  * [`node-meta`](https://developer.hashicorp.com/consul/api-docs/health#node-meta) `(string: "")` - Specifies a desired node metadata key/value pair of the form `key:value`. This parameter can be specified multiple times, and filters the results to nodes with the specified key/value pairs. This is specified as part of the URL as a query parameter.
  * [`filter`](https://developer.hashicorp.com/consul/api-docs/health#filter-1) `(string: "")` - Specifies the expression used to filter the queries results prior to returning the data.
  * `(string: "")`
Enterprise
- Specifies the namespace of the service. You can also [specify the namespace through other methods](https://developer.hashicorp.com/consul/api-docs/health#methods-to-specify-namespace).


###  Sample Request

```
$ curl \
    http://127.0.0.1:8500/v1/health/checks/my-service?ns=default

```

```
$ curl \
    http://127.0.0.1:8500/v1/health/checks/my-service?ns=default

```

###  Sample Response

```


    "Node": "foobar",
    "CheckID": "service:redis",
    "Name": "Service 'redis' check",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "redis",
    "ServiceName": "redis",
    "ServiceTags": ["primary"],
    "Namespace": "default"



```

```
    "Node": "foobar",
    "CheckID": "service:redis",
    "Name": "Service 'redis' check",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "redis",
    "ServiceName": "redis",
    "ServiceTags": ["primary"],
    "Namespace": "default"

```

###  Filtering
The filter will be executed against each health check in the results list with the following selectors and filter operations being supported:  
| Selector  | Supported Operations  |  
| --- | --- |  
| `CheckID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Notes`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Output`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceName`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceTags`  | In, Not In, Is Empty, Is Not Empty  |  
| `Status`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
##  List Service Instances for Service
This endpoint returns the service instances providing the service indicated on the path. Users can also build in support for dynamic load balancing and other features by incorporating the use of health checks.
The HTTP response includes the `X-Consul-Results-Filtered-By-ACLs: true` header if the response array excludes results due to ACL policy configuration. Refer to the [HTTP API documentation](https://developer.hashicorp.com/consul/api-docs/api-structure#results-filtered-by-acls) for more information.  
| Method  | Path  | Produces  |  
| --- | --- | --- |  
| `/health/service/:service`  | `application/json`  |  
The table below shows this endpoint's support for [blocking queries](https://developer.hashicorp.com/consul/api-docs/features/blocking), [consistency modes](https://developer.hashicorp.com/consul/api-docs/features/consistency), [agent caching](https://developer.hashicorp.com/consul/api-docs/features/caching), and [required ACLs](https://developer.hashicorp.com/consul/api-docs/api-structure#authentication).  
| Blocking Queries  | Consistency Modes  | Agent Caching  | ACL Required  |  
| --- | --- | --- | --- |  
| `background refresh`  | `node:read,service:read`  |  
1some query parameters will use the [streaming backend](https://developer.hashicorp.com/consul/api-docs/features/blocking#streaming-backend) for blocking queries.
###  Path Parameters
  * [`service`](https://developer.hashicorp.com/consul/api-docs/health#service-1) `(string: <required>)` - Specifies the service to list services for.


###  Query Parameters
  * `(string: "")` - Specifies the datacenter to query. This will default to the datacenter of the agent being queried.
  * `(string: "")` - Specifies a node name to sort the node list in ascending order based on the estimated round trip time from that node. Passing `?near=_agent` uses the agent's node for the sort. ~> **Note:** Using `near` will ignore [`use_streaming_backend`](https://developer.hashicorp.com/consul/docs/reference/agent/configuration-file/general#use_streaming_backend) and always use blocking queries, because the data required to sort the results is not available to the streaming backend.
  * `(string: "")` **Deprecated** - Use `filter` with the `Service.Tags` selector instead. This parameter will be removed in a future version of Consul. Specifies the tag to filter the list. You can use this parameter multiple times to return only the results that include all of the tag values provided.
  * [`node-meta`](https://developer.hashicorp.com/consul/api-docs/health#node-meta-1) `(string: "")` **Deprecated** - Use `filter` with the `Node.Meta` selector instead. This parameter will be removed in a future version of Consul. Specifies a desired node metadata key/value pair of the form `key:value`. This parameter can be specified multiple times, and filters the results to nodes with the specified key/value pairs.
  * [`passing`](https://developer.hashicorp.com/consul/api-docs/health#passing) `(bool: false)` - Specifies that the server should return only nodes with all checks in the `passing` state. This can be used to avoid additional filtering on the client side.
  * [`filter`](https://developer.hashicorp.com/consul/api-docs/health#filter-2) `(string: "")` - Specifies the expression used to filter the queries results prior to returning the data.
  * `(string: "")` - Specifies the imported service's peer. Applies only to imported services.
  * [`merge-central-config`](https://developer.hashicorp.com/consul/api-docs/health#merge-central-config) - Include this flag in a request for `connect-proxy` kind or `*-gateway` kind services to return a fully resolved service definition that includes merged values from the [proxy-defaults/global](https://developer.hashicorp.com/consul/docs/reference/config-entry/proxy-defaults) and [service-defaults/:service](https://developer.hashicorp.com/consul/docs/reference/config-entry/service-defaults) config entries. Returning a fully resolved service definition is useful when a service was registered using the [/catalog/register](https://developer.hashicorp.com/consul/api-docs/catalog#register_entity) endpoint, which does not automatically merge config entries.
  * `(string: "")`
Enterprise
- Specifies the namespace of the service. You can also [specify the namespace through other methods](https://developer.hashicorp.com/consul/api-docs/health#methods-to-specify-namespace).
  * `(string: "")`
Enterprise
- Specifies the sameness group the service is a member of to facilitate requests to identical services in other peers or partitions.


###  Sample Request

```
$ curl \
    http://127.0.0.1:8500/v1/health/service/my-service?ns=default

```

```
$ curl \
    http://127.0.0.1:8500/v1/health/service/my-service?ns=default

```

###  Sample Response

```


    "Node": {
      "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
      "Node": "foobar",
      "Address": "10.1.10.12",
      "Datacenter": "dc1",
      "TaggedAddresses": {
        "lan": "10.1.10.12",
        "wan": "10.1.10.12"

      "Meta": {
        "instance_type": "t2.medium"


    "Service": {
      "ID": "redis",
      "Service": "redis",
      "Tags": ["primary"],
      "Address": "10.1.10.12",
      "TaggedAddresses": {
        "lan": {
          "address": "10.1.10.12",
          "port": 8000

        "wan": {
          "address": "198.18.1.2",
          "port": 80


      "Meta": {
        "redis_version": "4.0"

      "Port": 8000,
      "Weights": {
        "Passing": 10,
        "Warning": 1

      "Namespace": "default"

    "Checks": [

        "Node": "foobar",
        "CheckID": "service:redis",
        "Name": "Service 'redis' check",
        "Status": "passing",
        "Notes": "",
        "Output": "",
        "ServiceID": "redis",
        "ServiceName": "redis",
        "ServiceTags": ["primary"],
        "Namespace": "default"


        "Node": "foobar",
        "CheckID": "serfHealth",
        "Name": "Serf Health Status",
        "Status": "passing",
        "Notes": "",
        "Output": "",
        "ServiceID": "",
        "ServiceName": "",
        "ServiceTags": [],
        "Namespace": "default"





```

```
    "Node": {
      "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
      "Node": "foobar",
      "Address": "10.1.10.12",
      "Datacenter": "dc1",
      "TaggedAddresses": {
        "lan": "10.1.10.12",
        "wan": "10.1.10.12"
      "Meta": {
        "instance_type": "t2.medium"
    "Service": {
      "ID": "redis",
      "Service": "redis",
      "Tags": ["primary"],
      "Address": "10.1.10.12",
      "TaggedAddresses": {
        "lan": {
          "address": "10.1.10.12",
          "port": 8000
        "wan": {
          "address": "198.18.1.2",
          "port": 80
      "Meta": {
        "redis_version": "4.0"
      "Port": 8000,
      "Weights": {
        "Passing": 10,
        "Warning": 1
      "Namespace": "default"
    "Checks": [
        "Node": "foobar",
        "CheckID": "service:redis",
        "Name": "Service 'redis' check",
        "Status": "passing",
        "Notes": "",
        "Output": "",
        "ServiceID": "redis",
        "ServiceName": "redis",
        "ServiceTags": ["primary"],
        "Namespace": "default"
        "Node": "foobar",
        "CheckID": "serfHealth",
        "Name": "Serf Health Status",
        "Status": "passing",
        "Notes": "",
        "Output": "",
        "ServiceID": "",
        "ServiceName": "",
        "ServiceTags": [],
        "Namespace": "default"

```

###  Filtering
The filter will be executed against each entry in the top level results list with the following selectors and filter operations being supported:  
| Selector  | Supported Operations  |  
| --- | --- |  
| `Checks`  | Is Empty, Is Not Empty  |  
| `Checks.CheckID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Checks.Name`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Checks.Node`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Checks.Notes`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Checks.Output`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Checks.ServiceID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Checks.ServiceName`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Checks.ServiceTags`  | In, Not In, Is Empty, Is Not Empty  |  
| `Checks.Status`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Node.Address`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Node.Datacenter`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Node.ID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Node.Meta`  | Is Empty, Is Not Empty, In, Not In  |  
| `Node.Meta.<any>`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Node.Node`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Node.TaggedAddresses`  | Is Empty, Is Not Empty, In, Not In  |  
| `Node.TaggedAddresses.<any>`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Address`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Connect.Native`  | Equal, Not Equal  |  
| `Service.EnableTagOverride`  | Equal, Not Equal  |  
| `Service.ID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Kind`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Meta`  | Is Empty, Is Not Empty, In, Not In  |  
| `Service.Meta.<any>`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Port`  | Equal, Not Equal  |  
| `Service.Proxy.DestinationServiceID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.DestinationServiceName`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.LocalServiceAddress`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.LocalServicePort`  | Equal, Not Equal  |  
| `Service.Proxy.Mode`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.TransparentProxy.OutboundListenerPort`  | Equal, Not Equal  |  
| `Service.Proxy.MeshGateway.Mode`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.Upstreams`  | Is Empty, Is Not Empty  |  
| `Service.Proxy.Upstreams.Datacenter`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.Upstreams.DestinationName`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.Upstreams.DestinationNamespace`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.Upstreams.DestinationType`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.Upstreams.LocalBindAddress`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Proxy.Upstreams.LocalBindPort`  | Equal, Not Equal  |  
| `Service.Proxy.Upstreams.MeshGateway.Mode`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.Service`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.TaggedAddresses`  | Is Empty, Is Not Empty, In, Not In  |  
| `Service.TaggedAddresses.<any>.Address`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Service.TaggedAddresses.<any>.Port`  | Equal, Not Equal  |  
| `Service.Tags`  | In, Not In, Is Empty, Is Not Empty  |  
| `Service.Weights.Passing`  | Equal, Not Equal  |  
| `Service.Weights.Warning`  | Equal, Not Equal  |  
##  List Service Instances for Mesh-enabled Service
This endpoint returns the service instances providing a [mesh-capable](https://developer.hashicorp.com/consul/docs/connect) service in a given datacenter. This will include both proxies and native integrations. A service may register both mesh-capable and incapable services at the same time, so this endpoint may be used to filter only the mesh-capable endpoints.
The HTTP response includes the `X-Consul-Results-Filtered-By-ACLs: true` header if the response array excludes results due to ACL policy configuration. Refer to the [HTTP API documentation](https://developer.hashicorp.com/consul/api-docs/api-structure#results-filtered-by-acls) for more information.  
| Method  | Path  | Produces  |  
| --- | --- | --- |  
| `/health/connect/:service`  | `application/json`  |  
Parameters and response format are the same as [`/health/service/:service`](https://developer.hashicorp.com/consul/api-docs/health#list-nodes-for-service).
##  List Service Instances for Ingress Gateways Associated with a Service
**1.8.0+:** This API is available in Consul versions 1.8.0 and later.
This endpoint returns the service instances providing an [ingress gateway](https://developer.hashicorp.com/consul/docs/north-south/ingress-gateway) for a service in a given datacenter.
The HTTP response includes the `X-Consul-Results-Filtered-By-ACLs: true` header if the response array excludes results due to ACL policy configuration. Refer to the [HTTP API documentation](https://developer.hashicorp.com/consul/api-docs/api-structure#results-filtered-by-acls) for more information.  
| Method  | Path  | Produces  |  
| --- | --- | --- |  
| `/health/ingress/:service`  | `application/json`  |  
Parameters and response format are the same as [`/health/service/:service`](https://developer.hashicorp.com/consul/api-docs/health#list-nodes-for-service).
**Note:** Unlike `/health/connect/:service` and `/health/service/:service` this endpoint does not support the `peer` query parameter and the [streaming backend](https://developer.hashicorp.com/consul/api-docs/features/blocking#streaming-backend).
##  List Checks in State
This endpoint returns the checks in the state provided on the path.
The HTTP response includes the `X-Consul-Results-Filtered-By-ACLs: true` header if the response array excludes results due to ACL policy configuration. Refer to the [HTTP API documentation](https://developer.hashicorp.com/consul/api-docs/api-structure#results-filtered-by-acls) for more information.  
| Method  | Path  | Produces  |  
| --- | --- | --- |  
| `/health/state/:state`  | `application/json`  |  
The table below shows this endpoint's support for [blocking queries](https://developer.hashicorp.com/consul/api-docs/features/blocking), [consistency modes](https://developer.hashicorp.com/consul/api-docs/features/consistency), [agent caching](https://developer.hashicorp.com/consul/api-docs/features/caching), and [required ACLs](https://developer.hashicorp.com/consul/api-docs/api-structure#authentication).  
| Blocking Queries  | Consistency Modes  | Agent Caching  | ACL Required  |  
| --- | --- | --- | --- |  
| `node:read,service:read`  |  
###  Path Parameters
  * [`state`](https://developer.hashicorp.com/consul/api-docs/health#state) `(string: <required>)` - Specifies the state to query. Supported states are `any`, `passing`, `warning`, or `critical`. The `any` state is a wildcard that can be used to return all checks.


###  Query Parameters
  * `(string: "")` - Specifies the datacenter to query. This will default to the datacenter of the agent being queried.
  * `(string: "")` - Specifies a node name to sort the node list in ascending order based on the estimated round trip time from that node. Passing `?near=_agent` uses the agent's node for the sort.
  * [`node-meta`](https://developer.hashicorp.com/consul/api-docs/health#node-meta-2) `(string: "")` - Specifies a desired node metadata key/value pair of the form `key:value`. This parameter can be specified multiple times, and filters the results to nodes with the specified key/value pairs.
  * [`filter`](https://developer.hashicorp.com/consul/api-docs/health#filter-3) `(string: "")` - Specifies the expression used to filter the queries results prior to returning the data.
  * `(string: "")`
Enterprise
- Specifies the namespace to query. You can also [specify the namespace through other methods](https://developer.hashicorp.com/consul/api-docs/health#methods-to-specify-namespace).


###  Sample Request

```
$ curl \
    http://127.0.0.1:8500/v1/health/state/passing?ns=default

```

```
$ curl \
    http://127.0.0.1:8500/v1/health/state/passing?ns=default

```

###  Sample Response

```


    "Node": "foobar",
    "CheckID": "serfHealth",
    "Name": "Serf Health Status",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "",
    "ServiceName": "",
    "ServiceTags": [],
    "Namespace": "default"


    "Node": "foobar",
    "CheckID": "service:redis",
    "Name": "Service 'redis' check",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "redis",
    "ServiceName": "redis",
    "ServiceTags": ["primary"],
    "Namespace": "default"



```

```
    "Node": "foobar",
    "CheckID": "serfHealth",
    "Name": "Serf Health Status",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "",
    "ServiceName": "",
    "ServiceTags": [],
    "Namespace": "default"
    "Node": "foobar",
    "CheckID": "service:redis",
    "Name": "Service 'redis' check",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "redis",
    "ServiceName": "redis",
    "ServiceTags": ["primary"],
    "Namespace": "default"

```

###  Filtering
The filter will be executed against each health check in the results list with the following selectors and filter operations being supported:  
| Selector  | Supported Operations  |  
| --- | --- |  
| `CheckID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Notes`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `Output`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceID`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceName`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
| `ServiceTags`  | In, Not In, Is Empty, Is Not Empty  |  
| `Status`  | Equal, Not Equal, In, Not In, Matches, Not Matches  |  
##  Methods to specify namespace 
Enterprise
The health endpoints support several methods for specifying the namespace of resources with the following order of precedence:
  1. query parameter
  2. [`X-Consul-Namespace`](https://developer.hashicorp.com/consul/api-docs/health#x-consul-namespace) request header
  3. Namespace is inherited from the namespace of the request's ACL token (if any)
  4. The `default` namespace


[Edit this page on GitHub](https://github.com/hashicorp/web-unified-docs/blob/main/content/consul/v2.0.x/content/api-docs/health.mdx)
