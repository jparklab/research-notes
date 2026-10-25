---
title: 'GitHub - llm-d/llm-d-latency-predictor: Latency prediction service for ML-model
  based scoring with llm-d-inference-scheduler · GitHub'
id: github-llm-dllm-d-latency-predictor-latency-prediction-service-for-ml-model-base
tags:
- llmd-router-nonk8s-backends-54b774
- latency-predictor
- fastapi
- portability
created: '2026-09-11T20:55:04.473391Z'
updated: '2026-09-15T04:55:23.894643Z'
source: https://github.com/llm-d/llm-d-latency-predictor
source_domain: github.com
fetched_at: '2026-09-11T20:55:04.449068Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Upstream source repo (llm-d/llm-d-latency-predictor, Python 92.8 percent
  / Shell 4.7 percent, latest release v0.9.0, Aug 11 2026; generated from the llm-d/llm-d-go-template
  scaffold despite being a Python project) for the training and prediction servers
  used by predicted-latency scheduling. Both servers are plain FastAPI apps with no
  Kubernetes API dependency visible in the README: prediction/prediction_server.py
  (serves latency predictions, runnable directly via uvicorn prediction.prediction_server:app
  --port 8001) and training/training_server.py (trains models from request traces
  via uvicorn training.training_server:app --port 8000) -- i.e., the reference implementation
  is a generic HTTP microservice pair that could in principle be run as bare processes
  or plain Docker containers outside Kubernetes, even though the repos deploy/ directory
  ships Kubernetes manifests + kustomization and a build-deploy.sh helper targeting
  GKE for the reference/tested deployment path. tests/test_dual_server_client.py is
  an integration/load-test client exercising both servers together, packaged as a
  Kubernetes Job image in the reference deployment. This is the concrete evidence
  that the predicted-latency EPP plugins external dependency (the latency-scorer calling
  out to a prediction server) is an HTTP call to a configurable address, not an in-process
  or k8s-API-mediated dependency -- making it one of the more portable EPP add-ons
  for a non-k8s backend, PROVIDED the EPPs plugin config accepts an arbitrary host:port
  rather than requiring a Kubernetes Service DNS name (not confirmed either way in
  this repo; would need to check the latency-scorer plugin config schema in llm-d-router).'
utility_score: 12.0
---

*Suggested by [[llm-dllm-d-router-deepwiki]] — gap-fill search for predicted-latency-based scheduling guide*

[Skip to content](https://github.com/llm-d/llm-d-latency-predictor#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-latency-predictor) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-latency-predictor) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-latency-predictor) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-latency-predictor).
/ **[llm-d-latency-predictor](https://github.com/llm-d/llm-d-latency-predictor) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-latency-predictor) You must be signed in to change notification settings
  * [ Fork 29 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-latency-predictor)
  * [ Star  8 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-latency-predictor)


[**22** Branches](https://github.com/llm-d/llm-d-latency-predictor/branches)[](https://github.com/llm-d/llm-d-latency-predictor/tags)
Go to file
Open more actions menu
## Latest commit
[[bugfix] remove leftover testing CI datestamp file (](https://github.com/llm-d/llm-d-latency-predictor/commit/0e39e360bcfe94ec1fc0a41c33a91f2cc9949c74)[#70](https://github.com/llm-d/llm-d-latency-predictor/pull/70)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/0e39e360bcfe94ec1fc0a41c33a91f2cc9949c74)
Open commit detailssuccess
Sep 8, 2026
[0e39e36](https://github.com/llm-d/llm-d-latency-predictor/commit/0e39e360bcfe94ec1fc0a41c33a91f2cc9949c74) · Sep 8, 2026
## History
[34 Commits](https://github.com/llm-d/llm-d-latency-predictor/commits/main/)
Open commit details
34 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [triggering image builds and functional tests again (](https://github.com/llm-d/llm-d-latency-predictor/commit/8b7f945d976eacfebcb9de16a1dc3bb34ffb975b "triggering image builds and  functional tests again \(#69\)

* triggering image builds and  functional tests again

Signed-off-by: greg pereira <grpereir@redhat.com>

* removing version mechanism since we have no common schema, capture status and logs of tests

Signed-off-by: greg pereira <grpereir@redhat.com>

* cleanup the output steps

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#69](https://github.com/llm-d/llm-d-latency-predictor/pull/69)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/8b7f945d976eacfebcb9de16a1dc3bb34ffb975b "triggering image builds and  functional tests again \(#69\)

* triggering image builds and  functional tests again

Signed-off-by: greg pereira <grpereir@redhat.com>

* removing version mechanism since we have no common schema, capture status and logs of tests

Signed-off-by: greg pereira <grpereir@redhat.com>

* cleanup the output steps

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 30, 2026  |  
|   |   |  [[bugfix] remove leftover testing CI datestamp file (](https://github.com/llm-d/llm-d-latency-predictor/commit/0e39e360bcfe94ec1fc0a41c33a91f2cc9949c74 "\[bugfix\] remove leftover testing CI datestamp file \(#70\)

* triggering image builds and functional tests again

Signed-off-by: greg pereira <grpereir@redhat.com>

* remove empty CI triggers

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#70](https://github.com/llm-d/llm-d-latency-predictor/pull/70)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/0e39e360bcfe94ec1fc0a41c33a91f2cc9949c74 "\[bugfix\] remove leftover testing CI datestamp file \(#70\)

* triggering image builds and functional tests again

Signed-off-by: greg pereira <grpereir@redhat.com>

* remove empty CI triggers

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Sep 8, 2026  |  
|   |   |  [triggering image builds and functional tests again (](https://github.com/llm-d/llm-d-latency-predictor/commit/8b7f945d976eacfebcb9de16a1dc3bb34ffb975b "triggering image builds and  functional tests again \(#69\)

* triggering image builds and  functional tests again

Signed-off-by: greg pereira <grpereir@redhat.com>

* removing version mechanism since we have no common schema, capture status and logs of tests

Signed-off-by: greg pereira <grpereir@redhat.com>

* cleanup the output steps

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#69](https://github.com/llm-d/llm-d-latency-predictor/pull/69)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/8b7f945d976eacfebcb9de16a1dc3bb34ffb975b "triggering image builds and  functional tests again \(#69\)

* triggering image builds and  functional tests again

Signed-off-by: greg pereira <grpereir@redhat.com>

* removing version mechanism since we have no common schema, capture status and logs of tests

Signed-off-by: greg pereira <grpereir@redhat.com>

* cleanup the output steps

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 30, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |  [Fixed Segmentation fault on DataFrame Construction (](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")[#58](https://github.com/llm-d/llm-d-latency-predictor/pull/58)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")  | Aug 10, 2026  |  
|   |   |  [Fixed Segmentation fault on DataFrame Construction (](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")[#58](https://github.com/llm-d/llm-d-latency-predictor/pull/58)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")  | Aug 10, 2026  |  
|   |   |  [Fixed Segmentation fault on DataFrame Construction (](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")[#58](https://github.com/llm-d/llm-d-latency-predictor/pull/58)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")  | Aug 10, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |   | Apr 20, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |  [Add BenjaminBraunDev to OWNERS (](https://github.com/llm-d/llm-d-latency-predictor/commit/e603977efe6b249fd51faf3065d3431641f9dde0 "Add BenjaminBraunDev to OWNERS \(#74\)

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>")[#74](https://github.com/llm-d/llm-d-latency-predictor/pull/74)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/e603977efe6b249fd51faf3065d3431641f9dde0 "Add BenjaminBraunDev to OWNERS \(#74\)

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>")  | Aug 3, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |   | Apr 13, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |  [restructuring source directories by component (](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#16](https://github.com/llm-d/llm-d-latency-predictor/pull/16)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/720d12ef6f906aeabf221349a98e52b127b02856 "restructuring source directories by component \(#16\)

* restructuring source directories by component

Changes:
- add more dependency tooling
- institute common version file
- build in a mechanism for compiling hashes / reproduceable builds

Signed-off-by: greg pereira <grpereir@redhat.com>

* fix CI import issue, long term fix coming later

Signed-off-by: greg pereira <grpereir@redhat.com>

* addressing kaushik's comments

Signed-off-by: greg pereira <grpereir@redhat.com>

* revert kaushik's suggestion, follow on later with real fix

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Jul 10, 2026  |  
|   |   |  [Fixed Segmentation fault on DataFrame Construction (](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")[#58](https://github.com/llm-d/llm-d-latency-predictor/pull/58)[)](https://github.com/llm-d/llm-d-latency-predictor/commit/04e44cd801d5d79a215d99109081b6afb870b87e "Fixed Segmentation fault on DataFrame Construction  \(#58\)

* Fall back happened even when LightGBM shared library fails

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* Updated review comments

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* rebased version to 0.0.2

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated the function

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

* updated requirements.txt for testing

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>

---------

Signed-off-by: Modassar-Rana <modassar.rana@ibm.com>")  | Aug 10, 2026  |  
| View all files |  
## Repository files navigation
# llm-d-latency-predictor
> **Latency prediction service for the llm-d ecosystem.**
## Overview
## Prerequisites
  * Python 3.11+
  * Docker (for container builds)
  * [pre-commit](https://pre-commit.com/) (for local development)


## Quick Start

```
# Clone the repo
git clone https://github.com/llm-d/llm-d-latency-predictor.git
cd llm-d-latency-predictor

# Install pre-commit hooks
pre-commit install

# Install Python dependencies
make install

# Run tests
make test

# Run linters
make lint
```

## Development
See [CONTRIBUTING.md](https://github.com/llm-d/llm-d-latency-predictor/blob/main/CONTRIBUTING.md) for development guidelines, coding standards, and how to submit changes.
### Common Commands

```
make help           # Show all available targets
make install        # Install Python dependencies
make test           # Run Python tests
make lint           # Run Python linter (ruff)
make fmt            # Format Python code
make image-build    # Build container images (prediction, training, test)
make pre-commit     # Run pre-commit hooks
```

### Layout

```
common/
  types.py                    # Shared enums and model types
prediction/
  prediction_server.py        # FastAPI prediction server (serves latency predictions)
  Dockerfile                  # Image for the prediction server
training/
  training_server.py          # FastAPI training server (trains models from request traces)
  Dockerfile                  # Image for the training server
tests/
  test_dual_server_client.py  # integration / load-test client exercising both servers
  Dockerfile                  # Image that runs the test client as a Job
deploy/                       # Kubernetes manifests and kustomization
build-deploy.sh               # Helper script for building images and deploying to GKE

```

After `make install`, the servers can be run with:

```
uvicorn prediction.prediction_server:app --port 8001
uvicorn training.training_server:app --port 8000
```

## Architecture
## Configuration
## Contributing
We welcome contributions! Please see [CONTRIBUTING.md](https://github.com/llm-d/llm-d-latency-predictor/blob/main/CONTRIBUTING.md) for guidelines.
All commits must be signed off (DCO). See [PR_SIGNOFF.md](https://github.com/llm-d/llm-d-latency-predictor/blob/main/PR_SIGNOFF.md) for instructions.
## Security
To report a security vulnerability, please see [SECURITY.md](https://github.com/llm-d/llm-d-latency-predictor/blob/main/SECURITY.md).
## License
This project is licensed under the Apache License 2.0 - see [LICENSE](https://github.com/llm-d/llm-d-latency-predictor/blob/main/LICENSE) for details.
## About
Latency prediction service for ML-model based scoring with llm-d-inference-scheduler
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**8** stars
### Watchers
**1** watching
### Forks
[ forks](https://github.com/llm-d/llm-d-latency-predictor/forks)
## 
[ v0.9.0 ReleaseLatest Aug 11, 2026 ](https://github.com/llm-d/llm-d-latency-predictor/releases/tag/v0.9.0)
## 
## Languages
  * [Python92.8%](https://github.com/llm-d/llm-d-latency-predictor/search?l=python)
  * [Shell4.7%](https://github.com/llm-d/llm-d-latency-predictor/search?l=shell)
  * [Makefile1.4%](https://github.com/llm-d/llm-d-latency-predictor/search?l=makefile)
  * [Dockerfile1.1%](https://github.com/llm-d/llm-d-latency-predictor/search?l=dockerfile)


Generated from [llm-d/llm-d-go-template](https://github.com/llm-d/llm-d-go-template)
You can’t perform that action at this time. 
