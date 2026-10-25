---
title: 'Both Endpoints and EndpointSlices not working with Service abstraction for
  external IPs · Issue #123954 · kubernetes/kubernetes'
id: both-endpoints-and-endpointslices-not-working-with-service-abstraction-for-exter
tags:
- llmd-router-nonk8s-backends
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:45:41.996484Z'
updated: '2026-09-15T04:55:23.767112Z'
source: https://github.com/kubernetes/kubernetes/issues/123954
source_domain: github.com
fetched_at: '2026-09-11T19:45:41.973744Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #123954 (kubernetes/kubernetes, opened Mar 15 2024, closed
  Apr 18 2024) is direct adversarial evidence of a real failure mode in the selectorless-Service
  + manual-EndpointSlice pattern. A user created a selectorless Service (port name
  "mysql") and a matching EndpointSlice, but kube-proxy silently rejected all traffic
  to it (iptables rule: "has no endpoints", REJECT with icmp-port-unreachable) even
  though kubectl get endpointslice showed the correct backend IP. Root cause identified
  by maintainer aroradaman: the EndpointSlice port was named ("mysql") while the Service
  port was unnamed, breaking the internal Service-to-endpoint port-name mapping kube-proxy
  relies on; the fix is to make both named or both unnamed consistently. A second,
  still-present quirk: kubectl describe service continues to show "Endpoints: <none>"
  even after the fix, because kubectl only reads the legacy Endpoints object, which
  is never auto-populated when EndpointSlices are created manually (only the reverse
  Endpoints-to-EndpointSlice mirroring exists) -- so manually-managed EndpointSlice
  backends can look empty/broken in standard tooling even when actually routing correctly.
  This is a concrete, code-level gotcha anyone using selectorless Service + manual
  EndpointSlice as a router backend must account for.'
utility_score: 11.0
---

[Skip to content](https://github.com/kubernetes/kubernetes/issues/123954#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes/kubernetes/issues/123954) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes/kubernetes/issues/123954) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes/kubernetes/issues/123954) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes/kubernetes/issues/123954).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes%2Fkubernetes) You must be signed in to change notification settings
  * [ Fork 44k ](https://github.com/login?return_to=%2Fkubernetes%2Fkubernetes)
  * [ Star  127k ](https://github.com/login?return_to=%2Fkubernetes%2Fkubernetes)


#  Both Endpoints and EndpointSlices not working with Service abstraction for external IPs #123954
Copy link
Copy link
Closed
Closed
[Both Endpoints and EndpointSlices not working with Service abstraction for external IPs](https://github.com/kubernetes/kubernetes/issues/123954#top)#123954
Copy link
Assignees
Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[sig/networkCategorizes an issue or PR as relevant to SIG Network.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22sig%2Fnetwork%22)Categorizes an issue or PR as relevant to SIG Network.[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
## Description
opened [on Mar 15, 2024](https://github.com/kubernetes/kubernetes/issues/123954#issue-2189285844)
Last edited by PMacho
Issue body actions
### What happened?
As recommend in <https://kubernetes.io/docs/concepts/services-networking/service/#services-without-selectors> I did try to abstract access to an externally hosted mariaDB. However, both the recommend approach and the legacy approach with Endpoints did not work as expected. There is a slight difference however, in the EndpointSlice case the service description showed `Endpoints: <none>` while in the endpoints case it was empty, i.e. `Endpoints: `.
### What did you expect to happen?
The external database should be reachable through the service abstraction.
### How can we reproduce it (as minimally and precisely as possible)?
It is a clean K3s with --disable traefik, nginx ingress as well as cert-manager installed via helm, plus GitLab Agent for Kubernetes also using Helm.
The Service:

```
apiVersion: v1
kind: Service
metadata:
  name: mariadb
spec:
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
```

I've tried both, EndpointSlice:

```
apiVersion: discovery.k8s.io/v1
kind: EndpointSlice
metadata:
  name: mariadb-1
  labels:
    kubernetes.io/service-name: mariadb
addressType: IPv4
ports:
  - name: mysql
    port: 3306
    protocol: TCP
endpoints:
  - addresses:
    - 10.0.0.5
```

, and Endpoints:

```
apiVersion: v1
kind: Endpoints
metadata:
  name: mariadb
subsets:
  - addresses:
      - ip: 10.0.0.5
    ports:
      - name: mysql
        port: 3306
```

Both were successfully deployed in the same namespace as the service and linked to the service.
## case: EndpointSlices

```
kubectl get endpointslice -l kubernetes.io/service-name=mariadb

NAME        ADDRESSTYPE   PORTS   ENDPOINTS   AGE
mariadb-1   IPv4          3306    10.0.0.5    3m29s

```


```
kubectl describe service mariadb

Name:              mariadb
Namespace:         keldysh-projects-workshop
Labels:            app.kubernetes.io/managed-by=Helm
Annotations:       meta.helm.sh/release-name: service
                   meta.helm.sh/release-namespace: keldysh-projects-workshop
Selector:          <none>
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.126.23
IPs:               10.43.126.23
Port:              <unset>  3306/TCP
TargetPort:        3306/TCP
Endpoints:         <none>
Session Affinity:  None
Events:            <none>

```

## case: Endpoints

```
kubectl describe endpoints mariadb

Name:         mariadb
Namespace:    keldysh-projects-workshop
Labels:       app.kubernetes.io/managed-by=Helm
Annotations:  meta.helm.sh/release-name: service
              meta.helm.sh/release-namespace: keldysh-projects-workshop
Subsets:
  Addresses:          10.0.0.5
  NotReadyAddresses:  <none>
  Ports:
    Name   Port  Protocol
    ----   ----  --------
    mysql  3306  TCP

Events:  <none>

```


```
kubectl describe service mariadb

Name:              mariadb
Namespace:         keldysh-projects-workshop
Labels:            app.kubernetes.io/managed-by=Helm
Annotations:       meta.helm.sh/release-name: service
                   meta.helm.sh/release-namespace: keldysh-projects-workshop
Selector:          <none>
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.126.23
IPs:               10.43.126.23
Port:              <unset>  3306/TCP
TargetPort:        3306/TCP
Endpoints:         
Session Affinity:  None
Events:            <none>

```

### Anything else we need to know?
DNS is working in principle:

```
kubectl exec -i -t dnsutils -- nslookup mariadb

Server:         10.43.0.10
Address:        10.43.0.10#53

Name:   mariadb.keldysh-projects-workshop.svc.cluster.local
Address: 10.43.19.67

```

I already submitted a bug report in the K3s repository. They however claim to not change the core kubernetes functionality: [k3s-io/k3s#9725](https://github.com/k3s-io/k3s/issues/9725) [k3s-io/k3s#9726](https://github.com/k3s-io/k3s/discussions/9726)
### Kubernetes version
K3s Version: k3s version v1.28.7+k3s1 ([k3s-io/k3s@`051b14b`](https://github.com/k3s-io/k3s/commit/051b14b248655896fdfd7ba6c93db6182cde7431)) go version go1.21.7
### Cloud provider
Self hosted K3s on Hetzner Cloud Servers
### OS version
Linux kubernetes-test-server 5.15.0-91-generic [k3s-io/k3s#101](https://github.com/k3s-io/k3s/issues/101) SMP Tue Nov 14 13:30:08 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
### Install tools
Details
### Container runtime (CRI) and version (if applicable)
Details
### Related plugins (CNI, CSI, ...) and versions (if applicable)
Details
## Activity
[PMacho](https://github.com/PMacho)
added 
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.
[on Mar 15, 2024](https://github.com/kubernetes/kubernetes/issues/123954#event-12135652608)
[k8s-ci-robot](https://github.com/k8s-ci-robot)
added 
[needs-sigIndicates an issue or PR lacks a `sig/foo` label and requires one.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22needs-sig%22)Indicates an issue or PR lacks a `sig/foo` label and requires one.
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Mar 15, 2024](https://github.com/kubernetes/kubernetes/issues/123954#event-12135654232)
### aojea commented on Mar 16, 2024 
Last edited by aojea
Member
More actions
Can you explain exactly what you expect to happen? is not very clear from your description,
do you expect that a pod that tries to connect to the Service ClusterIP `10.43.0.10` ends connected to the IP `10.0.0.5` ?
~~Have you verified all connectivity works without services too?~~
[@PMacho](https://github.com/PMacho) assuming you are using kube-proxy, can you please paste an `iptables-save` from one of the worker nodes ?
### PMacho commented on Mar 16, 2024 
Author
More actions
The documentation of "Services without Selectors" <https://kubernetes.io/docs/concepts/services-networking/service/#service-no-selector-access> explicitly states
> Accessing a Service without a selector works the same as if it had a selector. In the [example](https://kubernetes.io/docs/concepts/services-networking/service/#services-without-selectors) for a Service without a selector, traffic is routed to one of the two endpoints defined in the EndpointSlice manifest: a TCP connection to 10.1.2.3 or 10.4.5.6, on port 9376.
Where the "example" defines:

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

and

```
apiVersion: discovery.k8s.io/v1
kind: EndpointSlice
metadata:
  name: my-service-1 # by convention, use the name of the Service
                     # as a prefix for the name of the EndpointSlice
  labels:
    # You should set the "kubernetes.io/service-name" label.
    # Set its value to match the name of the Service
    kubernetes.io/service-name: my-service
addressType: IPv4
ports:
  - name:  # empty because port 9376 is not assigned as a well-known
             # port (by IANA)
    appProtocol: http
    protocol: TCP
    port: 9376
endpoints:
  - addresses:
      - "10.4.5.6"
  - addresses:
      - "10.1.2.3"
```

Therefore, in my configuration I expect `r2dbc:mariadb://mariadb` to be routed to `10.0.0.5`, instead of complaining `Connection refused: mariadb.debug-endpoints.svc.cluster.local/10.43.202.125:3306`.
For the record, defining the service to be headless, i.e. `spec.clusterIP: "None"`, does route to `10.0.0.5`. However, <https://kubernetes.io/docs/concepts/services-networking/service/#headless-services> states:
> Sometimes you don't need load-balancing and a single Service IP. Considering that the Database may very well be a cluster, this does not necessarily apply.
The requested output of `iptables save`, with the above configuration deployed to namespace `debug-endpoints`: [iptables-save.txt](https://github.com/kubernetes/kubernetes/files/14624940/iptables-save.txt)
### aojea commented on Mar 16, 2024 
Member
More actions
interesting, it seems you are using kube-proxy and kube-proxy does not process those endpoints
> -A KUBE-SERVICES -d 10.43.202.125/32 -p tcp -m comment --comment "debug-endpoints/mariadb has no endpoints" -m tcp --dport 3306 -j REJECT --reject-with icmp-port-unreachable
I'm in my way to kubecon so no much time until the week after, but this is a good issue for networking contributors to help, it seems pretty simple to reproduce as you are already providing the steps ... once reproduced is a matter of understanding why the kube-proxy does not see those EndpointSlices
[@aroradaman](https://github.com/aroradaman) [@uablrek](https://github.com/uablrek) you are quite familiar with the proxy code, maybe you can take a look? /cc [@danwinship](https://github.com/danwinship)
👍React with 👍1Reacted by Lan
### aojea commented on Mar 16, 2024 
Member
More actions
/sig network /triage accepted
[k8s-ci-robot](https://github.com/k8s-ci-robot)
added 
[sig/networkCategorizes an issue or PR as relevant to SIG Network.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22sig%2Fnetwork%22)Categorizes an issue or PR as relevant to SIG Network.
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
and removed 
[needs-sigIndicates an issue or PR lacks a `sig/foo` label and requires one.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22needs-sig%22)Indicates an issue or PR lacks a `sig/foo` label and requires one.
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Mar 16, 2024](https://github.com/kubernetes/kubernetes/issues/123954#event-12140778263)
### PMacho commented on Mar 16, 2024 
Author
More actions
> interesting, it seems you are using kube-proxy and kube-proxy does not process those endpoints
>> -A KUBE-SERVICES -d 10.43.202.125/32 -p tcp -m comment --comment "debug-endpoints/mariadb has no endpoints" -m tcp --dport 3306 -j REJECT --reject-with icmp-port-unreachable
> I'm in my way to kubecon so no much time until the week after, but this is a good issue for networking contributors to help, it seems pretty simple to reproduce as you are already providing the steps ... once reproduced is a matter of understanding why the kube-proxy does not see those EndpointSlices
Thank you for the positive validation. I wish you a great time in Paris.
### aroradaman commented on Mar 17, 2024 
Member
More actions
/assign
[k8s-ci-robot](https://github.com/k8s-ci-robot)
assigned [on Mar 17, 2024](https://github.com/kubernetes/kubernetes/issues/123954#event-12142141974)
### aroradaman commented on Mar 17, 2024 
Member
More actions
[@PMacho](https://github.com/PMacho) I see you are defining ports with a name in the endpointslice but without a name in the service which is breaking our internal mapping of service to endpoints.
Making both the ports either named or un-named should solve the issue.
quick fix:

```
kubectl patch service mariadb --type='json' -p='[{"op": "add", "path": "/spec/ports/0/name", "value": "mysql"}]'

```

### sftim commented on Mar 17, 2024 
Contributor
More actions
Maybe the docs could use a tweak to recommend setting a name for port 80?
### aroradaman commented on Mar 17, 2024 
Member
More actions
> Maybe the docs could use a tweak to recommend setting a name for port 80?
yes, will update the doc.
### PMacho commented on Mar 17, 2024 
Author
More actions
[@aroradaman](https://github.com/aroradaman)
Your quick fix indeed did the job. It will certainly be useful for other users to update the docs.
For the record, I do not know if this is intended but to my understanding the description of the service does not reflect its behavior since `Endpoints` are still `<none>` (even though the routing actually works now):

```
kubectl describe service mariadb -n debug-endpoints               
Name:              mariadb
Namespace:         debug-endpoints
Labels:            app.kubernetes.io/managed-by=Helm
Annotations:       meta.helm.sh/release-name: kubernetes-debug-service
                   meta.helm.sh/release-namespace: debug-endpoints
Selector:          none
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.202.125
IPs:               10.43.202.125
Port:              mysql  3306/TCP
TargetPort:        3306/TCP
Endpoints:         none
Session Affinity:  None
Events:            none
```

### sftim commented on Mar 17, 2024 
Contributor
More actions
If you manually create EndpointSlices, the associated Endpoints for the same Service doesn't get autopopulated (and that's where `kubectl` is looking).
### aroradaman commented on Mar 17, 2024 
Member
More actions
If you create endpoint instead of endpointslice then you can see the desired endpoints in kubectl describe output. And endpointslice mirror controller will create one endpointslice for your endpoint.
### danwinship commented on Mar 20, 2024 
Contributor
More actions
> If you manually create EndpointSlices, the associated Endpoints for the same Service doesn't get autopopulated (and that's where `kubectl` is looking).
oh, that's no good. It should look at the EndpointSlices. Hey [@aroradaman](https://github.com/aroradaman) wanna fix that too? 🙂
### aroradaman commented on Mar 20, 2024 
Member
More actions
> > If you manually create EndpointSlices, the associated Endpoints for the same Service doesn't get autopopulated (and that's where `kubectl` is looking).
> oh, that's no good. It should look at the EndpointSlices. Hey [@aroradaman](https://github.com/aroradaman) wanna fix that too? 🙂
sure, will look into this.
### aroradaman commented on Apr 3, 2024 
Member
More actions
> > If you manually create EndpointSlices, the associated Endpoints for the same Service doesn't get autopopulated (and that's where `kubectl` is looking).
> oh, that's no good. It should look at the EndpointSlices. Hey [@aroradaman](https://github.com/aroradaman) wanna fix that too? 🙂
[@danwinship](https://github.com/danwinship) do we need a new endpointMirroring controller for this?
### danwinship commented on Apr 4, 2024 
Contributor
More actions
oh, sorry, I just meant to fix `kubectl describe service` to show EndpointSlices rather than Endpoints
👍React with 👍1Reacted by Tim Bannister
### sftim commented on Apr 4, 2024 
Contributor
More actions
💭 should `kubectl` fall back to looking for an Endpoints if there are no matching EndpointSlices?
### aroradaman commented on Apr 4, 2024 
Member
More actions
> 💭 should `kubectl` fall back to looking for an Endpoints if there are no matching EndpointSlices?
endpoints will be mirrored by eps mirror controller, right?
### sftim commented on Apr 4, 2024 
Contributor
More actions
Usually, but you can [opt out](https://kubernetes.io/docs/reference/labels-annotations-taints/#endpointslicekubernetesioskip-mirror).
### aroradaman commented on Apr 18, 2024 
Member
More actions
/close
[k8s-ci-robot](https://github.com/k8s-ci-robot)
closed this as [completed](https://github.com/kubernetes/kubernetes/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Apr 18, 2024](https://github.com/kubernetes/kubernetes/issues/123954#event-12522937202)
### k8s-ci-robot commented on Apr 18, 2024 
Contributor
More actions
[@aroradaman](https://github.com/aroradaman): Closing this issue.
Details
In response to [this](https://github.com/kubernetes/kubernetes/issues/123954#issuecomment-2063784778):
> /close
Instructions for interacting with me using PR comments are available [here](https://git.k8s.io/community/contributors/guide/pull-requests.md). If you have questions or suggestions related to my behavior, please file an issue against the [kubernetes/test-infra](https://github.com/kubernetes/test-infra/issues/new?title=Prow%20issue:) repository.
[Sign up for free](https://github.com/signup?return_to=https://github.com/kubernetes/kubernetes/issues/123954)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kubernetes/kubernetes/issues/123954)
## Metadata
## Metadata
### Assignees
  * [aroradaman](https://github.com/aroradaman)


### Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[sig/networkCategorizes an issue or PR as relevant to SIG Network.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22sig%2Fnetwork%22)Categorizes an issue or PR as relevant to SIG Network.[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes/kubernetes/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
