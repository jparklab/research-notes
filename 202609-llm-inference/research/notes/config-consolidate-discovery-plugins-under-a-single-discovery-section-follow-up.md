---
title: 'config: consolidate discovery plugins under a single discovery section (follow
  up #2341) by pazqo · Pull Request #2434 · llm-d/llm-d-router · GitHub'
id: config-consolidate-discovery-plugins-under-a-single-discovery-section-follow-up
tags:
- llmd-router-nonk8s-backends-54b774
- discovery-config
- migration-shim
- unreleased-change
created: '2026-09-11T21:47:04.622307Z'
updated: '2026-09-12T03:37:37.833211Z'
source: https://github.com/llm-d/llm-d-router/pull/2434
source_domain: github.com
fetched_at: '2026-09-11T21:47:04.594952Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub PR #2434, ''config: consolidate discovery plugins under a single
  discovery section (follow up #2341)'', author pazqo. Opened 18 Aug 2026, MERGED
  into llm-d/llm-d-router:main on 25 Aug 2026 (commit 950c963, by elevran, auto-merge
  squash, 39 checks passed) -- i.e. eight days AFTER the v0.10.0 tag (17 Aug 2026),
  so this change ships in NO tagged release as of the 11 Sep 2026 fetch date; it exists
  only on main/HEAD, targeting the yet-unreleased v0.11 line. Exact mechanism, quoted
  from the PR description: groups endpoint discovery and peer discovery under dataLayer.discovery
  with endpoints and peers sub-fields, replacing two separate top-level fields (dataLayer.discovery
  and dataLayer.peerDiscovery). A migration shim in the config loader preserves backward
  compatibility: dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef;
  dataLayer.peerDiscovery migrates to discovery.peers. Test coverage cited: pkg/epp/config/loader/configloader_test.go,
  tests named Deprecated_discovery, TestLoadRawConfig, Deprecated_dataLayer. Official
  release-note text drafted in the PR: ''The dataLayer.discovery.pluginRef config
  field is deprecated. Use dataLayer.discovery.endpoints.pluginRef instead. The old
  field is still accepted and automatically migrated. Peer discovery is now configured
  exclusively via dataLayer.discovery.peers.'' This confirms the migration-shim mechanism
  the target position describes is real and merged, but the version floor of ''v0.9.0''
  in the target position is wrong for the NEW endpoints.pluginRef key -- it does not
  exist in any release yet, only on unreleased main.'
---

*Suggested by [[releases-llm-dllm-d-router]] — PR referenced in target position as source of dataLayer.discovery.endpoints.pluginRef change*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/2434#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2434) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2434) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2434) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Aug 18, 2026](https://github.com/llm-d/llm-d-router/pull/2434#issue-5180116866) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
Copy link  Copy Markdown 
Contributor
Group endpoint discovery and peer discovery under dataLayer.discovery with endpoints and peers sub-fields instead of two separate top-level fields (dataLayer.discovery and dataLayer.peerDiscovery).
A migration shim in the config loader preserves backward compatibility:
  * dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef
  * dataLayer.peerDiscovery migrates to discovery.peers


An example can be seen in `pkg/epp/config/loader/configloader_test.go`: `Deprecated_discovery`, `TestLoadRawConfig`, `Deprecated_dataLayer`.
Follows the existing deprecation pattern used for parser and saturationDetector fields.
**What type of PR is this?** /kind cleanup /kind deprecation
**What this PR does / why we need it** : Consolidates the two discovery plugin config fields (dataLayer.discovery and dataLayer.peerDiscovery) into a single dataLayer.discovery section with endpoints and peers sub-fields. A migration shim preserves backward compatibility for the old config format, following the existing deprecation pattern used for parser and saturationDetector.
**Which issue(s) this PR fixes** : Follow-up to [#2341](https://github.com/llm-d/llm-d-router/pull/2341), as discussed with [@evacchi](https://github.com/evacchi) and [@elevran](https://github.com/elevran)
**Release note** _(write`NONE` if no user-facing change)_:

```
The `dataLayer.discovery.pluginRef` config field is deprecated. Use `dataLayer.discovery.endpoints.pluginRef` instead. The old field is still accepted and automatically migrated. Peer discovery is now configured exclusively via `dataLayer.discovery.peers`.

```

` config: consolidate discovery plugins under a single discovery section[](https://github.com/llm-d/llm-d-router/pull/2434/commits/88bc31361d0c823474021f90201c8e28985654aa "config: consolidate discovery plugins under a single discovery section

Group endpoint discovery and peer discovery under dataLayer.discovery
with endpoints and peers sub-fields instead of two separate top-level
fields \(dataLayer.discovery and dataLayer.peerDiscovery\).

A migration shim in the config loader preserves backward compatibility:
- dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef
- dataLayer.peerDiscovery migrates to discovery.peers

Follows the existing deprecation pattern used for parser and
saturationDetector fields.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[pazqo](https://github.com/pazqo) Stefano Pascolutti 
SSH Key Fingerprint: mwgPqeAFzir5KT87TfNQTpPsVMyY6u9Iqx1JmMeYtWg
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).

```
Group endpoint discovery and peer discovery under dataLayer.discovery
with endpoints and peers sub-fields instead of two separate top-level
fields (dataLayer.discovery and dataLayer.peerDiscovery).

A migration shim in the config loader preserves backward compatibility:
- dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef
- dataLayer.peerDiscovery migrates to discovery.peers

Follows the existing deprecation pattern used for parser and
saturationDetector fields.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
```

[github-actions](https://github.com/apps/github-actions) Bot added the [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. label [Aug 18, 2026](https://github.com/llm-d/llm-d-router/pull/2434#event-29608989314)
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/ba7bdc1b99f17d54e62363a14503b331dbb66449 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
[pazqo](https://github.com/pazqo) marked this pull request as ready for review [August 18, 2026 09:13](https://github.com/llm-d/llm-d-router/pull/2434#event-29609219978)
[pazqo](https://github.com/pazqo) requested review from a team and [shmuelk](https://github.com/shmuelk) as [code owners](https://github.com/llm-d/llm-d-router/blob/dd65342ee538ab76d4b9f7e1efb5d8e95385d06e/CODEOWNERS#L14) [August 18, 2026 09:13](https://github.com/llm-d/llm-d-router/pull/2434#event-29609220687)
[pazqo](https://github.com/pazqo) requested review from [elevran](https://github.com/elevran) and [hexfusion](https://github.com/hexfusion) [August 18, 2026 09:13](https://github.com/llm-d/llm-d-router/pull/2434#event-29609221148)
[pazqo](https://github.com/pazqo) changed the title ~~config: consolidate discovery plugins under a single discovery section~~ config: consolidate discovery plugins under a single discovery section (follow up #2341) [Aug 18, 2026](https://github.com/llm-d/llm-d-router/pull/2434#event-29609270136)
[elevran](https://github.com/elevran) self-assigned this [Aug 18, 2026](https://github.com/llm-d/llm-d-router/pull/2434#event-29614422380)
[pazqo](https://github.com/pazqo) added 4 commits [August 18, 2026 13:29](https://github.com/llm-d/llm-d-router/pull/2434#commits-pushed-46b4997)
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/46b4997e8dec2c8b24a10f5cb4a8ed2727946c62 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/bf5bc5acf8943ff9a2b275b158e6259c46c701a5 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/c44d8a920c4f47e101ca7157886fcdd7a58ec10b "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/0bd88b448242c348ac1ebcee18b6d63d7b75ee05 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
reviewed  [ Aug 19, 2026 ](https://github.com/llm-d/llm-d-router/pull/2434#pullrequestreview-4970551523)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2434/files/0bd88b448242c348ac1ebcee18b6d63d7b75ee05)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks for following-up on [#2341](https://github.com/llm-d/llm-d-router/pull/2341) and consolidating the two discovery fields under one section. The endpoint-discovery migration path (old bare `pluginRef` -> `endpoints.pluginRef`) correctly preserves back-compat with a deprecation log, matching the existing parser/saturationDetector pattern.
Since peer discovery hasn't shipped in any release yet and has no real users, treating its rename as a "deprecation" (kept field, migration shim, log line, release note) seems like more machinery than needed for a pre-release rename - see inline comments. Suggest just renaming it outright and dropping the compat path for that one field, keeping the shim scoped to the endpoint-discovery field that's actually released.
This affects the PR body's release-note section as well and needs correcting to scope backward compatibility for `endpoints` field only.
Outside this PR diff: `pkg/epp/framework/interface/datalayer/peer.go`:69 still documents the old dataLayer.peerDiscovery.pluginRef path - worth updating for consistency once the field is renamed.
Comment thread [apix/config/v1alpha1/endpointpickerconfig_types.go](https://github.com/llm-d/llm-d-router/pull/2434/files/c44d8a920c4f47e101ca7157886fcdd7a58ec10b#diff-b0ac55cea5177bf6d95d92857360cd09545672ee55c54014376c3f72050f6257) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
Comment thread [pkg/epp/config/loader/configloader.go](https://github.com/llm-d/llm-d-router/pull/2434/files/c44d8a920c4f47e101ca7157886fcdd7a58ec10b#diff-186930e69c48cf7f3fa08f1c3e0fc3841be3eae4afd76c0d77bb13c17a4a313e) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
` config: remove pre-release peerDiscovery deprecation shim[](https://github.com/llm-d/llm-d-router/pull/2434/commits/6f11b89ba56520dc4bb5a34f0806c8154be10580 "config: remove pre-release peerDiscovery deprecation shim

The dataLayer.peerDiscovery field was introduced in #2341 but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration \(which IS released\)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[pazqo](https://github.com/pazqo) Stefano Pascolutti 
SSH Key Fingerprint: mwgPqeAFzir5KT87TfNQTpPsVMyY6u9Iqx1JmMeYtWg
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).

```
The dataLayer.peerDiscovery field was introduced in llm-d#2341[](https://github.com/llm-d/llm-d-router/pull/2341) but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration (which IS released)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
```

[github-actions](https://github.com/apps/github-actions) Bot added [ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ area/datalayer ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdatalayer) [ area/docs ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdocs) labels [Aug 20, 2026](https://github.com/llm-d/llm-d-router/pull/2434#event-29735276124)
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/ba802ab47babcbbdb35d8b2720a5a7ab1bc386ce "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
### 
commented [Aug 20, 2026](https://github.com/llm-d/llm-d-router/pull/2434#issuecomment-5354248368)
Copy link  Copy Markdown 
Contributor Author  
| 
> Thanks for following-up on [#2341](https://github.com/llm-d/llm-d-router/pull/2341) and consolidating the two discovery fields under one section. The endpoint-discovery migration path (old bare `pluginRef` -> `endpoints.pluginRef`) correctly preserves back-compat with a deprecation log, matching the existing parser/saturationDetector pattern. Since peer discovery hasn't shipped in any release yet and has no real users, treating its rename as a "deprecation" (kept field, migration shim, log line, release note) seems like more machinery than needed for a pre-release rename - see inline comments. Suggest just renaming it outright and dropping the compat path for that one field, keeping the shim scoped to the endpoint-discovery field that's actually released. This affects the PR body's release-note section as well and needs correcting to scope backward compatibility for `endpoints` field only. Outside this PR diff: `pkg/epp/framework/interface/datalayer/peer.go`:69 still documents the old dataLayer.peerDiscovery.pluginRef path - worth updating for consistency once the field is renamed.
Yeah, that makes sense. I'll remove the un-necessary deprecation things and keep them on already shipped features. Thanks for the review. I pushed an updated version addressing your comments.  |  
| --- |  
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/0e7e48251f185393f492901ff8340e74acccdba5 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
[pazqo](https://github.com/pazqo) requested a review from [elevran](https://github.com/elevran) [August 20, 2026 19:23](https://github.com/llm-d/llm-d-router/pull/2434#event-29764314487)
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/26b3189c453564de255b33405ac2f1b57df31014 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
[evacchi](https://github.com/evacchi) mentioned this pull request [ Aug 21, 2026 ](https://github.com/llm-d/llm-d-router/pull/2434#ref-pullrequest-5216539302)
[ feat: pod watch peer discovery plugin #2501 ](https://github.com/llm-d/llm-d-router/pull/2501)
[pazqo](https://github.com/pazqo) added 4 commits [August 22, 2026 15:06](https://github.com/llm-d/llm-d-router/pull/2434#commits-pushed-83131f9)
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/83131f9003965d54672e196957482c2e8ca2f038 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/481edb490780ee2955e9a42d436ccfb13f341dc9 "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/bdeab567bc86a81c0b87fef7ac1726debf9710dd "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/54ed0da4761411c6aedd5af48139a7b22422135a "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
reviewed  [ Aug 25, 2026 ](https://github.com/llm-d/llm-d-router/pull/2434#pullrequestreview-5020313660)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2434/files/54ed0da4761411c6aedd5af48139a7b22422135a)
### 
left a comment  •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
One consistency gap in the new migration's log message; otherwise looks good.
Comment thread [pkg/epp/config/loader/configloader.go](https://github.com/llm-d/llm-d-router/pull/2434/files/54ed0da4761411c6aedd5af48139a7b22422135a#diff-186930e69c48cf7f3fa08f1c3e0fc3841be3eae4afd76c0d77bb13c17a4a313e) Outdated   
|  //nolint:staticcheck // SA1019: dl.Discovery.PluginRef is deprecated: use discovery.endpoints instead.  |  
| --- |  
|  if dl.Discovery != nil && dl.Discovery.PluginRef != "" {  |  
|  logger.Info("DEPRECATION: dataLayer.discovery.pluginRef is deprecated, use dataLayer.discovery.endpoints.pluginRef instead.")  |  
### 
[Aug 25, 2026](https://github.com/llm-d/llm-d-router/pull/2434#discussion_r3854158916) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
the existing saturationDetector and parser migrations both end their log message with "If both are set, the new field is used.". This migration should match. The code silently prefers `endpoints.pluginRef` when both are populated, so a user with both fields set gets no signal that one of them was ignored.
### 
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Should be fixed now, thanks!
Comment thread [cmd/epp/runner/runner.go](https://github.com/llm-d/llm-d-router/pull/2434/files/54ed0da4761411c6aedd5af48139a7b22422135a#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
### 
commented [Aug 25, 2026](https://github.com/llm-d/llm-d-router/pull/2434#issuecomment-5412192005)
Copy link  Copy Markdown 
Collaborator  
|  LGTM can merge after fixing deprecation message.  |  
| --- |  
[pazqo](https://github.com/pazqo) added 2 commits [August 25, 2026 17:30](https://github.com/llm-d/llm-d-router/pull/2434#commits-pushed-cbe7ff2)
` address review feedback on discovery config migration[](https://github.com/llm-d/llm-d-router/pull/2434/commits/cbe7ff2c5d40a9b430a2c4bdf06d91211ff5b968 "address review feedback on discovery config migration

Add "If both are set, the new field is used." to the discovery
pluginRef deprecation message, matching the saturationDetector and
parser migration patterns. Extract a local variable in runner.go
to reduce repetition of the DataLayer field path.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[pazqo](https://github.com/pazqo) Stefano Pascolutti 
SSH Key Fingerprint: mwgPqeAFzir5KT87TfNQTpPsVMyY6u9Iqx1JmMeYtWg
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).

```
Add "If both are set, the new field is used." to the discovery
pluginRef deprecation message, matching the saturationDetector and
parser migration patterns. Extract a local variable in runner.go
to reduce repetition of the DataLayer field path.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
```

` Merge branch 'main' into consolidate-discovery-config[](https://github.com/llm-d/llm-d-router/pull/2434/commits/88486e58f88c940dd4986eb7f03c271d474a233b "Merge branch 'main' into consolidate-discovery-config") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
[pazqo](https://github.com/pazqo) requested a review from [elevran](https://github.com/elevran) [August 25, 2026 16:17](https://github.com/llm-d/llm-d-router/pull/2434#event-29989934748)
approved these changes  [ Aug 25, 2026 ](https://github.com/llm-d/llm-d-router/pull/2434#pullrequestreview-5022345283)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2434/files/88486e58f88c940dd4986eb7f03c271d474a233b)
[elevran](https://github.com/elevran) enabled auto-merge (squash) [August 25, 2026 18:13](https://github.com/llm-d/llm-d-router/pull/2434#event-29995822805)
Hide details View details
[elevran](https://github.com/elevran) merged commit [`950c963`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) into  llm-d:main [Aug 25, 2026](https://github.com/llm-d/llm-d-router/pull/2434#event-29996528279)
39 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).
[llm-d-router-release-notes](https://github.com/apps/llm-d-router-release-notes) Bot mentioned this pull request [ Aug 25, 2026 ](https://github.com/llm-d/llm-d-router/pull/2434#ref-pullrequest-5249219911)
[ docs: add release-note fragment for PR #2434 #2551 ](https://github.com/llm-d/llm-d-router/pull/2551)
Merged 
[ahg-g](https://github.com/ahg-g) pushed a commit that referenced this pull request [ Aug 25, 2026 ](https://github.com/llm-d/llm-d-router/pull/2434#ref-commit-c3b6040)
` docs: add release-note fragment for PR[](https://github.com/llm-d/llm-d-router/commit/c3b60408e11615988452d177c4ed53b8e28f37a7 "docs: add release-note fragment for PR #2434 \(#2551\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>") #2434[](https://github.com/llm-d/llm-d-router/pull/2434) ([](https://github.com/llm-d/llm-d-router/commit/c3b60408e11615988452d177c4ed53b8e28f37a7 "docs: add release-note fragment for PR #2434 \(#2551\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>")#2551[](https://github.com/llm-d/llm-d-router/pull/2551))[](https://github.com/llm-d/llm-d-router/commit/c3b60408e11615988452d177c4ed53b8e28f37a7 "docs: add release-note fragment for PR #2434 \(#2551\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2434).

```
Signed-off-by: llm-d-router-release-notes[bot] <287676111+llm-d-router-release-notes[bot]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes[bot] <287676111+llm-d-router-release-notes[bot]@users.noreply.github.com>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F2434)
###  Labels 
[ area/datalayer ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdatalayer) [ area/docs ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdocs) [ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files.
###  2 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
