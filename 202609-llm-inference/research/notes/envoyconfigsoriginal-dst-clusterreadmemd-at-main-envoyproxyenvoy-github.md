---
title: envoy/configs/original-dst-cluster/README.md at main · envoyproxy/envoy · GitHub
id: envoyconfigsoriginal-dst-clusterreadmemd-at-main-envoyproxyenvoy-github
tags:
- llmd-router-nonk8s-backends
- envoy
- original-dst
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:16.985891Z'
updated: '2026-09-12T03:37:37.563968Z'
source: https://github.com/envoyproxy/envoy/blob/main/configs/original-dst-cluster/README.md
source_domain: github.com
fetched_at: '2026-09-11T19:44:16.966364Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Envoy upstream example/README for the ORIGINAL_DST cluster type using the
  classic iptables-REDIRECT pattern: a network namespace redirects traffic matching
  a CIDR to an Envoy listener (port 10000), and Envoy''s original-destination cluster
  forwards each connection to whatever destination address the connection was originally
  addressed to before redirection (observed via SO_ORIGINAL_DST). Demonstrates the
  setup/cleanup scripts (netns_setup.sh/netns_cleanup.sh) and expected log lines (''Adding
  host ip:port'', ''Removing stale host ip:port''). This document covers only the
  iptables/connection-based original-destination discovery mechanism, NOT the request-level
  ''header override'' variant (setting the original destination per-HTTP-request via
  a header rather than via the pre-redirect socket address) that gateways use to honor
  an out-of-band EPP decision — that header-based override behavior lives in Envoy''s
  cluster/original_dst proto config (original_dst_lb_config / x-envoy-original-dst-host)
  and is not documented on this page, which is a documentation gap worth flagging
  for anyone using this page as the sole reference for EPP-driven routing.'
utility_score: 12.0
---

[Skip to content](https://github.com/envoyproxy/envoy/blob/main/configs/original-dst-cluster/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/envoyproxy/envoy/blob/main/configs/original-dst-cluster/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/envoyproxy/envoy/blob/main/configs/original-dst-cluster/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/envoyproxy/envoy/blob/main/configs/original-dst-cluster/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/blob/main/configs/original-dst-cluster/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fenvoyproxy%2Fenvoy) You must be signed in to change notification settings
  * [ Fork 5.6k ](https://github.com/login?return_to=%2Fenvoyproxy%2Fenvoy)
  * [ Star  28.9k ](https://github.com/login?return_to=%2Fenvoyproxy%2Fenvoy)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
Sunil Narasimhamurthy
[docs: Fix few typos in a README and a yaml (](https://github.com/envoyproxy/envoy/commit/379a7567b0d15cc9d0806052b82670c8cad76fc5)[#19924](https://github.com/envoyproxy/envoy/pull/19924)[)](https://github.com/envoyproxy/envoy/commit/379a7567b0d15cc9d0806052b82670c8cad76fc5)
Open commit details
Feb 14, 2022
[379a756](https://github.com/envoyproxy/envoy/commit/379a7567b0d15cc9d0806052b82670c8cad76fc5) · Feb 14, 2022
## History
[History](https://github.com/envoyproxy/envoy/commits/main/configs/original-dst-cluster/README.md)
Open commit details
History
76 lines (56 loc) · 2.52 KB
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

76 lines (56 loc) · 2.52 KB
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Original destination cluster configuration and testing
An original destination cluster forwards requests to the same destination the request was going to before being redirected to Envoy using an iptables REDIRECT rule. `proxy_config.yaml` contains an example Envoy configuration demonstrating the use of an original destination cluster. `netns_setup.sh` and `netns_cleanup.sh` are provided as examples for setting up and cleaning up, respectively, a network namespace and the required iptables rule to redirect traffic to Envoy.
# Setting up
`netns_setup.sh` takes two arguments: the name of the new network namespace and the prefix that is to be redirected. Envoy listener port is set to 10000, which matches the configuration in `proxy_config.yaml`.
This creates a network namespace `ns1` and redirects traffic from there to Envoy listening on port 10000 if the destination address of the traffic matches `173.194.222.0/24` :

```
sudo ./configs/original-dst-cluster/netns_setup.sh ns1 173.194.222.0/24

```

# Building and running Envoy
Build Envoy with debug options, so that the behavior can be better observed from the logs:

```
bazel build //source/exe:envoy-static -c dbg

```

Then you should run Envoy with the provided example configuration:

```
bazel-out/local-dbg/bin/source/exe/envoy-static -c configs/original-dst-cluster/proxy_config.yaml -l debug

```

When running you should see periodical messages like `Cleaning up stale original dst hosts.`
# Generating traffic
Next we generate traffic from the new network namespace hitting the redirect rule. Run this from another terminal:

```
sudo ip netns exec ns1 curl -v 173.194.222.106:80

```

Most likely you'll see `301 Moved` in the curl response. In the rare case of upstream connection timeout you'll see `503 Service Unavailable` instead. The connection timeout setting on the proxy_config.yaml is set to 6 seconds to make this less likely, but if no host with the destination address exist then you will get this response no matter how long the timeout setting.
You should see lines with `Adding host 173.194.222.106:80` being logged by each Envoy thread, followed by `Keeping active host 173.194.222.106:80` and eventually `Removing stale host 173.194.222.106:80`, again multiple times, once from each Envoy thread.
# Cleaning up
To properly remove the added network namespace and the iptables configuration run `netns_cleanup.sh` with the same arguments as the setup before:

```
sudo ./configs/original-dst-cluster/netns_cleanup.sh ns1 173.194.222.0/24

```

Finally, stop Envoy with `^C`.
You can’t perform that action at this time. 
