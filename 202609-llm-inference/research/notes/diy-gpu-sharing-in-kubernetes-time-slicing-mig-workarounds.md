---
title: 'DIY GPU Sharing in Kubernetes: Time-Slicing, MIG & Workarounds'
id: diy-gpu-sharing-in-kubernetes-time-slicing-mig-workarounds
tags:
- onprem-llm-inference-stacks-859099
- gpu-sharing
- provenance-vendor-blog
- multi-tenancy
created: '2026-10-23T17:22:55.371331Z'
updated: '2026-10-23T17:25:04.842145Z'
source: https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes
source_domain: www.vcluster.com
fetched_at: '2026-10-23T17:22:55.343169Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'vCluster (vendor) explainer on K8s GPU sharing: time-slicing via GPU Operator
  ConfigMap (replicas:4, no memory/fault isolation, time split per process not per
  pod), MIG via mig.strategy single/mixed and node label nvidia.com/mig.config=all-1g.5gb
  (up to 7 instances, hardware fault isolation, DCGM per-slice metrics, reconfig operationally
  disruptive), MIG+time-slicing combined (7 x 4 = 28 requests per GPU), and DIY fallbacks
  (taints/affinity, ResourceQuota on requests.nvidia.com/gpu, MPS/GVirtuS, cgroup
  hacks) judged fragile. Ends in vCluster sales pitch (virtual clusters as tenancy
  layer over MIG/time-slicing). No measurements. Vendor blog; config accurate to NVIDIA
  docs.'
---

This website uses cookies
Our site uses, and allows third parties to use, cookies and other tracking technologies to enable and improve site functionality, remember your preferences, analyze site use, and facilitate advertising. As explained in our [Privacy Policy](https://www.vcluster.com/legal/privacy) and [Cookie Policy](https://www.vcluster.com/legal/cookies), we may transfer data to third parties through use of these tracking technologies. Click ‘Allow All’ to agree. Click “Reject/Decline All” to decline these activities.
[#GPC_BANNER_ICON#]
[#GPC_TOAST_TEXT#]
Consent Selection
**Necessary**
**Preferences**
**Statistics**
**Marketing**
Details
  * Necessary  37
Necessary cookies help make a website usable by enabling basic functions like page navigation and access to secure areas of the website. The website cannot function properly without these cookies.
    * [Cookiebot2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://www.cookiebot.com/goto/privacy-policy/ "Learn more about this provider Cookiebot's privacy policy - opens in a new window")
**CookieConsent [x2]** Stores the user's cookie consent state for the current domain
**Maximum Storage Duration** : 1 year**Type** : HTTP Cookie
    * [Github2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://docs.github.com/en/github/site-policy/github-privacy-statement "Learn more about this provider Github's privacy policy - opens in a new window")
**_gh_sess** Preserves users states across page requests.
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**logged_in** Registers whether the user is logged in. This allows the website owner to make parts of the website inaccessible, based on the user's log-in status. 
**Maximum Storage Duration** : 1 year**Type** : HTTP Cookie
    * [Google2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://business.safety.google/privacy/ "Learn more about this provider Google's privacy policy - opens in a new window")
Some of the data collected by this provider is for the purposes of personalization and measuring advertising effectiveness. The provider may use the IP Addresses for ads measurement and ads personalization.
**test_cookie** Used to check if the user's browser supports cookies.
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
**_GRECAPTCHA** This cookie is used to distinguish between humans and bots. This is beneficial for the website, in order to make valid reports on the use of their website.
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
    * [HubSpot6](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://legal.hubspot.com/privacy-policy "Learn more about this provider HubSpot's privacy policy - opens in a new window")
**cookietest [x2]** This cookie is used to determine if the visitor has accepted the cookie consent box.
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**rc::a** This cookie is used to distinguish between humans and bots. This is beneficial for the website, in order to make valid reports on the use of their website.
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
**rc::b** This cookie is used to distinguish between humans and bots. 
**Maximum Storage Duration** : Session**Type** : HTML Local Storage
**rc::c** This cookie is used to distinguish between humans and bots. 
**Maximum Storage Duration** : Session**Type** : HTML Local Storage
**rc::f** This cookie is used to distinguish between humans and bots. 
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
    * [LinkedIn2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://www.linkedin.com/legal/privacy-policy "Learn more about this provider LinkedIn's privacy policy - opens in a new window")
**bcookie** Used in order to detect spam and improve the website's security. 
**Maximum Storage Duration** : 1 year**Type** : HTTP Cookie
**li_gc** Stores the user's cookie consent state for the current domain
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
    * [app.lemlist.com1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**__cflb** Registers which server-cluster is serving the visitor. This is used in context with load balancing, in order to optimize user experience. 
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
    * [cdn.jsdelivr.net1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**68994d4ecac027f80ff70510#pages** Pending
**Maximum Storage Duration** : Persistent**Type** : IndexedDB
    * [digitaloceanspaces.comhsadspixel.neths-banner.comlinkedin.comt.co  13](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**__cf_bm [x13]** This cookie is used to distinguish between humans and bots. This is beneficial for the website, in order to make valid reports on the use of their website.
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
    * [do.featurebase.app1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**featurebaseGlobalAuth** Pending
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
    * [medium.comwebsite.devpod.shwebsite.vcluster.com  3](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**_cfuvid [x3]** This cookie is a part of the services provided by Cloudflare - Including load-balancing, deliverance of website content and serving DNS connection for website operators. 
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [tb.featurebaseapp.com1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**SESS#** Preserves users states across page requests.
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [us-assets.i.posthog.com2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**dmn_chk_# [x2]** Used to maintain the overall functionality of the website: Assigns the user to a server and detects potential errors on specific servers, allowing the website to reassign the users to another server. 
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [www.vcluster.com1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**wf_auth_page** This cookie is necessary for the login function on the website. 
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
  * Preferences  3
Preference cookies enable a website to remember information that changes the way the website behaves or looks, like your preferred language or the region that you are in.
    * [LinkedIn1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://www.linkedin.com/legal/privacy-policy "Learn more about this provider LinkedIn's privacy policy - opens in a new window")
**lidc** Registers which server-cluster is serving the visitor. This is used in context with load balancing, in order to optimize user experience. 
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
    * [www.vcluster.com2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**docusaurus.announcement.dismiss** Pending
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
**docusaurus.announcement.id** Pending
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
  * Statistics  20
Statistic cookies help website owners to understand how visitors interact with websites by collecting and reporting information anonymously.
    * [Github1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://docs.github.com/en/github/site-policy/github-privacy-statement "Learn more about this provider Github's privacy policy - opens in a new window")
**_octo** Pending
**Maximum Storage Duration** : 1 year**Type** : HTTP Cookie
    * [HubSpot8](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://legal.hubspot.com/privacy-policy "Learn more about this provider HubSpot's privacy policy - opens in a new window")
**__hssc [x2]** Identifies if the cookie data needs to be updated in the visitor's browser.
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
**__hssrc [x2]** Used to recognise the visitor's browser upon reentry on the website.
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**__hstc [x2]** Sets a unique ID for the session. This allows the website to obtain data on visitor behaviour for statistical purposes.
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
**hubspotutk [x2]** Sets a unique ID for the session. This allows the website to obtain data on visitor behaviour for statistical purposes.
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
    * [Twitter Inc.1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://twitter.com/en/privacy "Learn more about this provider Twitter Inc.'s privacy policy - opens in a new window")
**personalization_id** This cookie is set by Twitter - The cookie allows the visitor to share content from the website onto their Twitter profile. 
**Maximum Storage Duration** : 400 days**Type** : HTTP Cookie
    * [cdn.ashbyprd.com1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**dd_cookie_test_#** Registers data on visitors' website-behaviour. This is used for internal analysis and website optimization. 
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
    * [cdn.cr-relay.com2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**cookiecookie [x2]** This cookie is used by the website’s operator in context with multi-variate testing. This is a tool used to combine or change content on the website. This allows the website to find the best variation/edition of the site.
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [loft.featurebase.app1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**featurebase-anon-id** Pending
**Maximum Storage Duration** : 1 year**Type** : HTTP Cookie
    * [us-assets.i.posthog.com5](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**ph_#_posthog [x3]** Registers statistical data on users' behaviour on the website. Used for internal analytics by the website operator. 
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**ph_#_primary_window_exists** Registers statistical data on users' behaviour on the website. Used for internal analytics by the website operator. 
**Maximum Storage Duration** : Session**Type** : HTML Local Storage
**ph_#_window_id** Registers statistical data on users' behaviour on the website. Used for internal analytics by the website operator. 
**Maximum Storage Duration** : Session**Type** : HTML Local Storage
    * [widget.senja.io1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**sj_v_id** Pending
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
  * Marketing  55
Marketing cookies are used to track visitors across websites. The intention is to display ads that are relevant and engaging for the individual user and thereby more valuable for publishers and third party advertisers.
    * [Google8](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://business.safety.google/privacy/ "Learn more about this provider Google's privacy policy - opens in a new window")
Some of the data collected by this provider is for the purposes of personalization and measuring advertising effectiveness. The provider may use the IP Addresses for ads measurement and ads personalization.
**_ga [x2]** Used to send data to Google Analytics about the visitor's device and behavior. Tracks the visitor across devices and marketing channels.
**Maximum Storage Duration** : 2 years**Type** : HTTP Cookie
**_ga_# [x2]** Used to send data to Google Analytics about the visitor's device and behavior. Tracks the visitor across devices and marketing channels.
**Maximum Storage Duration** : 2 years**Type** : HTTP Cookie
**_gcl_au [x2]** Used by Google AdSense for experimenting with advertisement efficiency across websites using their services. 
**Maximum Storage Duration** : 3 months**Type** : HTTP Cookie
**IDE** Used by Google DoubleClick to register and report the website user's actions after viewing or clicking one of the advertiser's ads with the purpose of measuring the efficacy of an ad and to present targeted ads to the user.
**Maximum Storage Duration** : 400 days**Type** : HTTP Cookie
**pagead/1p-user-list/#** Tracks if the user has shown interest in specific products or events across multiple websites and detects how the user navigates between sites. This is used for measurement of advertisement efforts and facilitates payment of referral-fees between websites.
**Maximum Storage Duration** : Session**Type** : Pixel Tracker
    * [HubSpot1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://legal.hubspot.com/privacy-policy "Learn more about this provider HubSpot's privacy policy - opens in a new window")
**__ptq.gif** Sends data to the marketing platform Hubspot about the visitor's device and behaviour. Tracks the visitor across devices and marketing channels.
**Maximum Storage Duration** : Session**Type** : Pixel Tracker
    * [Liveintent1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://liveintent.com/privacy-policy/ "Learn more about this provider Liveintent's privacy policy - opens in a new window")
**lidid** Collects data on visitors' behaviour and interaction - This is used to make advertisement on the website more relevant. The cookie also allows the website to detect any referrals from other websites. 
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [Reddit4](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://www.redditinc.com/policies/privacy-policy "Learn more about this provider Reddit's privacy policy - opens in a new window")
**rp.gif** Necessary for the implementation of the Reddit.com's share-button function.
**Maximum Storage Duration** : Session**Type** : Pixel Tracker
**_rdt_uuid [x3]** Used to track visitors on multiple websites, in order to present relevant advertisement based on the visitor's preferences. 
**Maximum Storage Duration** : 3 months**Type** : HTTP Cookie
    * [Twitter Inc.6](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://twitter.com/en/privacy "Learn more about this provider Twitter Inc.'s privacy policy - opens in a new window")
**1/i/adsct [x2]** Collects data on user behaviour and interaction in order to optimize the website and make advertisement on the website more relevant. 
**Maximum Storage Duration** : Session**Type** : Pixel Tracker
**muc_ads** Collects data on user behaviour and interaction in order to optimize the website and make advertisement on the website more relevant. 
**Maximum Storage Duration** : 400 days**Type** : HTTP Cookie
**guest_id** Collects data related to the user's visits to the website, such as the number of visits, average time spent on the website and which pages have been loaded, with the purpose of personalising and improving the Twitter service.
**Maximum Storage Duration** : 400 days**Type** : HTTP Cookie
**guest_id_ads** Collects information on user behaviour on multiple websites. This information is used in order to optimize the relevance of advertisement on the website.
**Maximum Storage Duration** : 400 days**Type** : HTTP Cookie
**guest_id_marketing** Collects information on user behaviour on multiple websites. This information is used in order to optimize the relevance of advertisement on the website.
**Maximum Storage Duration** : 400 days**Type** : HTTP Cookie
    * [YouTube18](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://business.safety.google/privacy/ "Learn more about this provider YouTube's privacy policy - opens in a new window")
**LAST_RESULT_ENTRY_KEY [x2]** Used to track user’s interaction with embedded content.
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**TESTCOOKIESENABLED [x2]** Used to track user’s interaction with embedded content.
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
**yt-icons-last-purged [x2]** Necessary for the implementation and functionality of YouTube video-content on the website. 
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
**ytidb::LAST_RESULT_ENTRY_KEY [x2]** Used to track user’s interaction with embedded content.
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
**YtIdbMeta#databases [x2]** Used to track user’s interaction with embedded content.
**Maximum Storage Duration** : Persistent**Type** : IndexedDB
**__Secure-BUCKET** Pending
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
**__Secure-ROLLOUT_TOKEN** Used to track user’s interaction with embedded content.
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
**__Secure-YEC** Stores the user's video player preferences using embedded YouTube video
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**__Secure-YNID** Used to track user’s interaction with embedded content.
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
**LogsDatabaseV2:V#||LogsRequestsStore** Used to track user’s interaction with embedded content.
**Maximum Storage Duration** : Persistent**Type** : IndexedDB
**ServiceWorkerLogsDatabase#SWHealthLog** Necessary for the implementation and functionality of YouTube video-content on the website. 
**Maximum Storage Duration** : Persistent**Type** : IndexedDB
**VISITOR_INFO1_LIVE** Tries to estimate the users' bandwidth on pages with integrated YouTube videos.
**Maximum Storage Duration** : 180 days**Type** : HTTP Cookie
**YSC** Registers a unique ID to keep statistics of what videos from YouTube the user has seen.
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [cdn.cr-relay.com4](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**signals-sdk-session-id [x2]** Pending
**Maximum Storage Duration** : 1 day**Type** : HTTP Cookie
**signals-sdk-user-id [x2]** Pending
**Maximum Storage Duration** : 1 year**Type** : HTTP Cookie
    * [cdn.vector.co7](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**_li_duid** Collects information on user behaviour on multiple websites. This information is used in order to optimize the relevance of advertisement on the website.
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
**_lc2_fpi [x2]** Registers user behaviour and navigation on the website, and any interaction with active campaigns. This is used for optimizing advertisement and for efficient retargeting. 
**Maximum Storage Duration** : 2 years**Type** : HTTP Cookie
**_li_dcdm_c [x2]** Registers user behaviour and navigation on the website, and any interaction with active campaigns. This is used for optimizing advertisement and for efficient retargeting. 
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**_lc2_fpi_js [x2]** Pending
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [loft.featurebase.appwww.googletagmanager.com  2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**_gcl_ls [x2]** Tracks the conversion rate between the user and the advertisement banners on the website - This serves to optimise the relevance of the advertisements on the website. 
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
    * [static.ads-twitter.com2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**_twpid [x2]** Pending
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [www.vcluster.comdevpod.sh  2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**KnockDB#kv [x2]** Pending
**Maximum Storage Duration** : Persistent**Type** : IndexedDB
  * Unclassified 11
Unclassified cookies are cookies that we are in the process of classifying, together with the providers of individual cookies.
    * [Reddit1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)[Learn more about this provider](https://www.redditinc.com/policies/privacy-policy "Learn more about this provider Reddit's privacy policy - opens in a new window")
**_rdt_cfg** Pending
**Maximum Storage Duration** : Session**Type** : HTML Local Storage
    * [api.vector.co1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**pixel/phoenix/rp/p** Pending
**Maximum Storage Duration** : Session**Type** : Pixel Tracker
    * [ca.knock-ai.com1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**knock_vid_bce03148-269d-4e21-b4f1-0bf4ef74f4f6** Pending
**Maximum Storage Duration** : 1 year**Type** : HTTP Cookie
    * [js.knock-ai.com3](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**knock_storage_cookie** Pending
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
**knock_storage_local** Pending
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage
**knock_storage_session** Pending
**Maximum Storage Duration** : Session**Type** : HTML Local Storage
    * [static.ads-twitter.com1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**_twsid** Pending
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
    * [us-assets.i.posthog.com3](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**ph_phc_zFBrPQEeLstf4d8HEAcKeMZY4JxLJ8RzYa7NB8byMtsS_posthog_cpm [x2]** Pending
**Maximum Storage Duration** : Session**Type** : HTTP Cookie
**ph_phc_zFBrPQEeLstf4d8HEAcKeMZY4JxLJ8RzYa7NB8byMtsS_session_registered_properties** Pending
**Maximum Storage Duration** : Session**Type** : HTML Local Storage
    * [www.vcluster.com1](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
**ph_phc_zFBrPQEeLstf4d8HEAcKeMZY4JxLJ8RzYa7NB8byMtsS_posthog** Pending
**Maximum Storage Duration** : Persistent**Type** : HTML Local Storage


[Cross-domain consent2](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes)
Your consent applies to the following domains:
List of domains your consent applies to:
Cookie declaration last updated on 9/30/26 by [Cookiebot](https://www.cookiebot.com)
[#IABV2_TITLE#]
[#IABV2_BODY_INTRO#]
[#IABV2_BODY_LEGITIMATE_INTEREST_INTRO#]
[#IABV2_BODY_PREFERENCE_INTRO#]
[#IABV2_LABEL_PURPOSES#]
[#IABV2_BODY_PURPOSES_INTRO#]
[#IABV2_BODY_PURPOSES#]
[#IABV2_LABEL_FEATURES#]
[#IABV2_BODY_FEATURES_INTRO#]
[#IABV2_BODY_FEATURES#]
[#IABV2_LABEL_PARTNERS#]
[#IABV2_BODY_PARTNERS_INTRO#]
[#IABV2_BODY_PARTNERS#]
About
Cookies are small text files that can be used by websites to make a user's experience more efficient.This site uses different types of cookies. Some cookies are placed by third party services that appear on our pages.You can at any time change or withdraw your consent from the Cookie Declaration on our website.Learn more about who we are, how you can contact us and how we process personal data in our Privacy Policy.Please state your consent ID and date when you contact us regarding your consent.[Privacy Policy](https://www.vcluster.com/legal/privacy) and [Cookie Policy](https://www.vcluster.com/legal/cookies)
**Do not sell or share my personal information**
Deny Allow selection Customize Allow all
[NEW: Introducing vMetal — turn your GPU racks into a cloud platform →](https://www.vmetal.ai/)
[Tech Blog by vCluster](https://www.vcluster.com/blog)[Press and Media Resources](https://www.vcluster.com/press-releases)
# DIY GPU Sharing in Kubernetes
[ James Walker](https://www.vcluster.com/authors/james-walker)
Jan 6, 2026
|
9
min Read
**_Editor's note(August 2026):_**_This article covers techniques for sharing GPUs among multiple tenants on common infrastructure. For tenants that can't share such as external customers, untrusted workloads, or compliance-bound AI infrastructure,_[_vCluster Private Nodes_](https://www.vcluster.com/docs/vcluster/production-guide/choose-worker-node-model) _provides dedicated GPU nodes per tenant, and is the recommended model for customer-facing and hard-isolation production workloads. To find out more, see our_[solutions page](https://www.vcluster.com/solutions) _for the full picture._
GPUs are now commonplace in Kubernetes environments, thanks to the rise of AI and ML workloads. Yet GPUs are expensive and not always used to capacity, so implementing an effective sharing strategy that enables multiple workloads to run on one GPU is also increasingly important. With individual GPUs typically costing thousands of dollars, it's crucial to make the most of the hardware you have available.
Natively, Kubernetes allows GPUs to be assigned to only a single pod at a time. This is highly inefficient as pods must be allocated an entire GPU even if they only need a fraction of its capacity. GPU-sharing techniques address this limitation, allowing you to improve GPU utilization and reduce your operating costs. Sharing enables you to safely use a single GPU for several apps, teams, or customers instead of needing a distinct device to be assigned to each deployment.
This article examines the problems involved in Kubernetes GPU sharing. We'll then discuss the key ways to implement GPU sharing using standard and DIY approaches, along with their benefits and pitfalls.
## Understanding the Kubernetes GPU-Sharing Problem
Kubernetes has [built-in support](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus) for allocating GPUs to your workloads. It relies on device plugins provided by your GPU manufacturer—NVIDIA, AMD, or Intel.
With a device plugin installed in your cluster, you can assign GPUs to your pods using the Kubernetes [resource requests and limits system](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers). The following example demonstrates how a pod can claim an NVIDIA GPU via the `[nvidia.com/gpu`](https://nvidia.com/gpu`) resource. The resource type is provided by the [NVIDIA Kubernetes device plugin](https://github.com/NVIDIA/k8s-device-plugin):

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-gpu
spec:
  containers:
    - name: my-container
      image: my-image:latest
      resources:
        limits:
          # Request a single NVIDIA GPU instance
          nvidia.com/gpu: 1

Copy

```

This Kubernetes-native mechanism is simple and predictable, but it has a big drawback: You can only assign whole GPU instances to your pods. Fractional requests, such as `[nvidia.com/gpu:](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes) 0.5`, aren't supported, so each pod that requests a GPU must consume an entire physical instance. This prevents efficient GPU utilization when operating smaller workloads that only need a fraction of an instance each.
GPU sharing solves this problem. It enables a single GPU to be allocated to more than one Kubernetes pod. This improves operating efficiency by reducing the number of GPUs you need to procure. Depending on the sharing mechanism you use, you can also ensure different workloads remain isolated at the GPU level.
The following sections explore the main strategies that allow you to share GPUs between multiple pods in your Kubernetes cluster. Because of the absence of GPU-sharing capabilities in native Kubernetes, each of these options requires a degree of manual configuration. They also have differing implications for workload isolation, so you should check the notes for each solution before you get started.
## Approach 1: Time-Slicing with the NVIDIA GPU Operator
GPU time-slicing is an NVIDIA GPU-sharing feature available when you have the [NVIDIA GPU Operator](https://github.com/NVIDIA/gpu-operator) installed in your Kubernetes cluster. The GPU Operator automates in-cluster management of the NVIDIA device plugin, drivers, and container tools.
Time-slicing allows you to share a GPU between multiple pods by creating logical replicas of the device. Activity on any of the replicas runs on the same physical GPU. The GPU's available compute time is sliced evenly between the running processes.
Time-slicing provides a relatively thin layer of abstraction over the physical GPU. It does not provide private memory, so you can't restrict the memory consumption of individual pods. It also doesn't provide fault isolation, meaning a problem caused by one pod could potentially impact other pods that are sharing the same GPU.
To configure time-slicing, you must first [install](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html) the NVIDIA GPU Operator using Helm:

```
$ helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
$ helm repo update
helm install \
  --wait \
  --generate-name \
  -n gpu-operator --create-namespace \
  --version=v25.3.2 \
  nvidia/gpu-operator

Copy

```

Once you've installed the GPU Operator, you can enable time-slicing for your GPUs by creating a ConfigMap in the GPU Operator's namespace (`gpu-operator` in the example above). The following example specifies that each of your GPUs will be split into four time-sliced replicas:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-slicing-configmap
  namespace: gpu-operator
data:
  any: |-
    version: v1
    sharing:
      timeSlicing:
        resources:
          - name: nvidia.com/gpu
            replicas: 4

Copy

```

To activate time-slicing, you must first apply the ConfigMap to your cluster:

```
$ kubectl apply -f time-slicing-configmap.yaml
Copy

```

Next, patch the NVIDIA device plugin's default Cluster Policy to reference the ConfigMap:

```
$ kubectl patch clusterpolicies.nvidia.com/cluster-policy \
  -n gpu-operator \
  --type merge \
  -p \'{"spec": {"devicePlugin": {"config": {"name": "time-slicing-configmap", "default": "any"}}}}\'

Copy

```

Inspecting the `[nvidia.com/gpus`](https://nvidia.com/gpus`) annotation on your nodes should now report the configured number of time-sliced GPU replicas:

```
$ kubectl describe node | grep nvidia.com
nvidia.com/gpu.count=1
nvidia.com/gpu.family=pascal
nvidia.com/gpu.memory=11264
nvidia.com/gpu.present=true
nvidia.com/gpu.product=NVIDIA-GeForce-GTX-1080-Ti-SHARED
nvidia.com/gpu.replicas=4
nvidia.com/gpu.sharing-strategy=time-slicing
nvidia.com/gpu:  4

Copy

```

While the output above has been condensed, it demonstrates four key details:
- `[nvidia.com/gpu.count=1`:](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes) There is a single physical GPU attached to the node.
- `[nvidia.com/gpu.replicas=4`:](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes) Four time-sliced replicas are being exposed per physical GPU.
- `[nvidia.com/gpu.sharing-strategy=time-slicing`:](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes) The NVIDIA device plugin is configured to share GPUs using time-slicing.
- `[nvidia.com/gpu:](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes) 4`: There are four GPU instances available for pods to request.
You can now go ahead and deploy multiple pods that request GPU access, up to a maximum of the four GPU replicas configured. Here's a demo that runs NVIDIA's CUDA sample image, as detailed in the [documentation](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html#time-slicing-verify):

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-deployment
spec:
  # The 3 replicas will create successfully
  # Each of the 3 Pods will be able to claim a time-sliced GPU replica
  replicas: 3
  selector:
    matchLabels:
      app: demo-deployment
  template:
    metadata:
      labels:
       app: demo-deployment
    containers:
      - name: nvidia-cuda-sample
        image: nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubuntu20.04
        command: ["/bin/bash", "-c", "--"]
        args:
          - while true; do /cuda-samples/vectorAdd; done
        resources:
          limits:
            nvidia.com/gpu: 1

Copy

```

Now that we've seen how to implement GPU time-slicing, let's break down the pros and cons of this approach. As we've already outlined above, time-slicing is versatile and easy to configure, but it also lacks robust isolation so it's not suitable for every use case.
### Time-Slicing Pros
- **Simplicity** : Time-slicing's biggest advantage is its ease of configuration. You only need the NVIDIA GPU Operator and a simple ConfigMap to slice your GPU into multiple replicas.
- **Supports very large replica counts** : Time-slicing allows you to create as many GPU replicas as you need. A single GPU could be sliced into hundreds of replicas to serve many small or infrequently accessed apps.
- **Flexible configuration** : Despite its simplicity, time-slicing can be customized for advanced scenarios. You can specify different sharing configurations on a per-node or per-GPU basis, for example.
### Time-Slicing Cons
- **No private memory** : Memory is shared between each time-sliced GPU replica. The inability to set pod-level memory usage constraints can lead to resource contention and noisy-neighbor problems.
- **No fault isolation or scheduling guarantees** : Time-slicing provides a bare minimum level of abstraction. GPU replicas aren't isolated at the hardware level, so faults in one task may have knock-on impacts on others. Further, the GPU time available to each pod isn't guaranteed to be proportional to the number of GPU replicas it requests. This limitation can prevent you from precisely optimizing workload performance.
- **Potential performance issues** : Time-slicing can potentially add small performance overheads that are hard to diagnose. The GPU must continually context-switch between tasks assigned to its replicas.
- **Allocated GPU time isn't proportional to the number of GPU replicas requested** : Pods may request two or more time-sliced replicas (*eg* `[nvidia.com/gpu:](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes) 2`) to run multiple processes, but this does not guarantee the pod will receive proportionally more compute time than other pods sharing the same GPU. The GPU's time is shared equally among all the _processes_ using the GPU, across all replicas and pods—it's not divided between the pods themselves.
## Approach 2: NVIDIA MIG (Multi-Instance GPU)
[MIG (Multi-Instance GPU)](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/index.html) is a newer GPU-sharing capability available within the NVIDIA GPU Operator. It lets you partition GPUs into up to seven self-contained instances. Each instance acts as though it's a new physical GPU attached to the host node. You can then assign the partitioned instances to your Kubernetes pods using a standard `[nvidia.com/gpu:](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes) <instance-count>` resource request.
MIG partitions operate independently of each other, with full hardware-level fault isolation. Each partition is allocated its own private share of the GPU's memory. Partitions are also assigned dedicated paths to GPU resources, such as caches, memory controllers, and compute components. This enhances security and prevents high utilization in one partition from impacting the others.
MIG is the most powerful GPU-sharing strategy when you need strong isolation guarantees. However, it's a relatively inflexible tool that only works in certain configurations. The partitioning options available [depend on your hardware](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/index.html#supported-mig-profiles) and may not always allow you to use your GPU's entire capacity.
For instance, if you partition a 96GB H100 card into three partitions, then each partition will be assigned 24GB of memory, leaving 24GB unallocated. It's not possible to assign differing amounts of compute resources to individual partitions. Moreover, MIG's only supported on select NVIDIA GPUs, starting from the Ampere generation introduced in 2020.
Assuming MIG works with your GPUs, you can activate the feature in your Kubernetes cluster by installing the NVIDIA GPU Operator with a MIG strategy configured. The following command enables MIG for all GPUs on your nodes:

```
$ helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
$ helm repo update
helm install \
  --wait \
  --generate-name \
  -n gpu-operator --create-namespace \
  --set mig.strategy=single \
  --version=v25.3.2 \
  nvidia/gpu-operator

Copy

```

You can optionally set the `mig.strategy` option to `mixed` instead of `single` to selectively activate MIG for specific GPUs.
Once the GPU Operator's installed, you should find your node's `[nvidia.com/mig.capable`](https://nvidia.com/mig.capable`) label has been set to `true`. You can also check the `[nvidia.com/mig.strategy`](https://nvidia.com/mig.strategy`) and `[nvidia.com/mig.config.state`](https://nvidia.com/mig.config.state`) labels to ensure the MIG configuration has applied successfully:

```
$ kubectl describe node | grep nvidia.com/gpu
nvidia.com/gpu.present=true
nvidia.com/gpu.count=1
nvidia.com/gpu.replicas=1
nvidia.com/mig.capable=true
nvidia.com/mig.config=all-disabled
nvidia.com/mig.config.state=success
nvidia.com/mig.strategy=single

Copy

```

Next, you must choose a MIG profile to partition your GPU with. As noted above, the available GPU profiles are documented in the [MIG user guide](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/index.html#supported-mig-profiles). As an example, you can use the `all-1g.5gb` profile if you have an A100 GPU that you'd like to partition into the maximum of seven MIG instances.
Once you've selected a MIG profile to use, update your Kubernetes node's `[nvidia.com/mig.config`](https://nvidia.com/mig.config`) label to reference the profile's name:

```
$ kubectl label nodes my-node nvidia.com/mig.config=all-1g.5gb --overwrite
Copy

```

The NVIDIA GPU Operator will then partition the GPU. Wait a few moments, then check the node's labels again. The `[nvidia.com/gpu.count`](https://nvidia.com/gpu.count`) label should now state the expected number of partitioned replicas, while `[nvidia.com/mig.config`](https://nvidia.com/mig.config`) will show the profile you've configured.

```
$ kubectl describe node | grep nvidia.com/gpu
nvidia.com/gpu.count=7
nvidia.com/mig.capable=true
nvidia.com/mig.config=all-1g.5gb
nvidia.com/mig.config.state=success
nvidia.com/mig.strategy=single

Copy

```

There are now effectively seven distinct GPUs available in your cluster. You can use the standard `[nvidia.com/gpu`](https://nvidia.com/gpu`) resource to assign the partitioned GPUs to your Kubernetes workloads:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-deployment
spec:
  # The 3 replicas will create successfully
  # Each of the 3 Pods will be able to claim a partitioned GPU instance
  replicas: 3
  selector:
    matchLabels:
      app: demo-deployment
  template:
    metadata:
      labels:
       app: demo-deployment
    containers:
      - name: nvidia-cuda-sample
        image: nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubuntu20.04
        command: ["/bin/bash", "-c", "--"]
        args:
          - while true; do /cuda-samples/vectorAdd; done
        resources:
          limits:
            nvidia.com/gpu: 1

Copy

```

You'll notice that this example is the same as the one shown above for time-slicing. Whether you use time-slicing or MIG, you should configure your Kubernetes pods in the same way. But it's important to realize that whereas time-slicing shares physical GPU access between pods with `[nvidia.com/gpu`](https://nvidia.com/gpu`) requests, MIG assigns each request exclusive access to a private partitioned instance. If a pod's assigned an MIG partition, then its GPU access is fully isolated from other pods also accessing that GPU. Pods that are assigned a time-sliced replica do not benefit from this isolation.
Although using MIG to share GPUs gives the strongest possible isolation, it's also a relatively inflexible solution. Let's take a closer look at its pros and cons.
Get started with the#1 platform for AI infra.
Trusted by today’s fastest-growing AI cloud builders.
[Chat with Sales](https://start-chat.com/slack/Loft-Labs/zQfdtP)[Try vCluster Free](https://www.vcluster.com/free)
### NVIDIA MIG Pros
-**Improved isolation** : Compared with time-slicing, MIG offers greatly enhanced isolation. Each workload using MIG gets its own private memory and fault isolation, preventing noisy-neighbor issues and security threats.
- **Hardware-enforced partitioning** : MIG partitioning is enforced at the hardware level, with each instance having its own path through the GPU's entire memory system.
- **Enhanced observability** : MIG is fully compatible with NVIDIA's [DCGM-Exporter](https://github.com/NVIDIA/dcgm-exporter), a tool that makes GPU utilization data available as Prometheus metrics. You can individually monitor utilization stats for each of your MIG partitions, allowing you to see precisely which apps and teams are occupying your GPUs.
### NVIDIA MIG Cons
- **Requires specific hardware** : MIG only works with modern high-end NVIDIA GPUs, so it won't necessarily work with your current infrastructure. Upgrading to MIG-compatible hardware could be a significant upfront investment.
- **Partitioning options are preconfigured** : MIG only supports up to seven partitions per GPU. The compute time and memory available to each partition are fixed, with no ability to arbitrarily assign resources. You must choose from one of the available profiles for your GPU.
- **Reconfiguring profiles can be operationally complex** : You can switch between MIG profiles by simply updating the `[nvidia.com/mig.config`](https://nvidia.com/mig.config`) label on the affected Kubernetes node. However, as this will change the number of partitions available, activating a new profile is often operationally complex. You must check that your workloads can still schedule and run performantly with the new configuration applied.
### Approach 3: MIG and Time-Slicing
MIG and time-slicing are independent mechanisms that each implement a form of GPU sharing. You can use either approach individually, but [combining MIG and time-slicing](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html#comparison-time-slicing-and-multi-instance-gpu) can help partially address the weaknesses of both.
To enable this strategy, you must first set up MIG by following the steps shown above. Once MIG is enabled, you can then configure time-slicing to provide shared access to the partitioned GPU instances created by MIG. The following time-slicing ConfigMap demonstrates how to time-slice the MIG partitions created in the previous example so there are four replicas of each partition:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-slicing-configmap
  namespace: gpu-operator
data:
  any: |-
    version: v1
    flags:
      migStrategy: single
    sharing:
      timeSlicing:
        resources:
          - name: nvidia.com/gpu
            replicas: 4

Copy

```

With this configuration enabled, up to twenty-eight GPU resource requests (seven MIG partitions times four time-sliced replicas) can be granted to pods—all from a single physical GPU. For advanced use cases, you can also configure MIG time-slicing [in `mixed` mode](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html#applying-multiple-node-specific-configurations) (`flags.migStrategy: mixed`) to configure a different number of time-sliced replicas for each active MIG profile.
Combining MIG and time-slicing is useful when you want to share GPU access across multiple isolated workloads, with multiple replicas running for each workload. By combining MIG and time-slicing, you can run up to seven distinct workloads per GPU—each with its own isolated GPU partition—but allow multiple pods per workload by time-slicing the MIG partitions into replicas.
## Alternative Work-Arounds and Custom Solutions
MIG and time-slicing are the leading technologies for Kubernetes GPU sharing. However, it's also possible to build DIY workarounds in case these solutions don't work for your use case. Perhaps you don't have MIG-compatible hardware and need stronger isolation than time-slicing alone, or are using non-NVIDIA GPUs. Here are a few different strategies you can choose.
### Manually Assigning GPUs Using Node Labeling and Taints
You can match Kubernetes workloads to GPUs using node labels and taints. While this doesn't let you share GPUs between multiple workloads, it can help ensure only approved pods get access to a GPU.
For example, you could apply a `has-gpu` [taint](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration) to nodes with available GPUs:

```
$ kubectl taint nodes my-node-with-gpu has-gpu=true:NoSchedule-
Copy

```

This prevents pods from scheduling to the node unless they tolerate the `has-gpu` taint. A pod manifest like the following one can then be used to schedule a pod to a GPU-enabled node while matching on a specific GPU type using the `[nvidia.com/gpu.product`](https://nvidia.com/gpu.product`) node label:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: demo-deployment
  template:
    metadata:
      labels:
       app: demo-deployment
    affinity:
      nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
            - key: nvidia.com/gpu.product
              value: A100-SXM4-40GB
    tolerations:
      - key: has-gpu
        operator: Exists
        effect: NoSchedule
    containers:
      - name: nvidia-cuda-sample
        image: nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubuntu20.04
        command: ["/bin/bash", "-c", "--"]
        args:
          - while true; do /cuda-samples/vectorAdd; done
        resources:
          limits:
            nvidia.com/gpu: 1

Copy

```

Now, the node is effectively reserved for pods that need the GPU—expressed via the `has-gpu` toleration—while the pod can guarantee it accesses an NVIDIA A100-SXM4-40GB device. You could expand this strategy by creating a custom Kubernetes [admission controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers) to reject pods that aren't authorized to use GPUs.
### Enforcing GPU Quotas via Resource Requests and Quotas
Kubernetes [resource quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas) allow you to control the maximum amount of a resource available to individual Kubernetes namespaces. You can use them to specify the maximum number of GPU instances available to individual tenants in your cluster.
Resource quotas allow you to fairly share physical GPUs—when no other GPU-sharing mechanism is available—or divide time-sliced or partitioned GPU replicas between your cluster's namespaces. The following example specifies that pods in the `my-namespace` namespace can request up to four `[nvidia.com/gpu`](https://nvidia.com/gpu`) resources:

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: gpu-resource-quota
  namespace: my-namespace
spec:
  hard:
    requests.nvidia.com/gpu: 4

Copy

```

### User-Space GPU-Multiplexing Tools
User-space GPU-multiplexing tools—like [GVirtuS](https://github.com/gvirtus), CUDA MPS—virtualize access to GPUs while running within user space at the operating system level. Each of these tools provides a mechanism to share GPUs between multiple processes, but using them with Kubernetes requires custom configuration.
GVirtuS is an experimental project, while CUDA MPS is another NVIDIA software feature. [LibVF.IO](https://LibVF.IO) implements vendor-agnostic multiplexing for NVIDIA, AMD, and Intel devices, but the project's last activity was over two years ago. It's unclear whether it's still maintained or compatible with current hardware.
### Container-Level GPU Cgroups and Runtime Hacks
You can use cgroups and container runtime configuration changes to manually implement an approximation of GPU sharing. If you've got multiple physical GPUs attached to your host, then cgroup hooks let you customize which devices are visible to the containers running within the cgroup. This lets you enforce that only specific containers can use the GPU, based on the cgroup they're assigned to.
### Why These Strategies Fall Short
Using any of these four strategies is likely to produce a fragile system that's difficult to configure, maintain, and secure. The operational overheads involved may surpass the cost of purchasing new hardware that supports stable features like MIG and time-slicing. You'll also lack debugging and monitoring capabilities, making it hard to investigate failures.
## How vCluster Helps Simplify GPU Sharing
Running multitenant AI/ML workloads with shared GPU access involves more than just the GPUs themselves. Kubernetes includes basic built-in multitenancy features via namespaces and RBAC, but these don't go far enough to ensure full isolation.
[vCluster](https://www.vcluster.com/solutions/internal-gpu-platform) solves this problem by enabling you to create fully functioning virtual clusters inside your physical Kubernetes environment. Each virtual cluster is fully isolated from its neighbors and operates independently. It has its own control plane with full RBAC and resource quota management.
vCluster doesn't natively implement GPU sharing, but it's fully compatible with existing technologies like NVIDIA MIG and time-slicing. It allows you to easily manage the tenanted workloads that access your GPU instances. vCluster also works with leading GPU-equipped workflow orchestration platforms, including [Run:ai](https://www.vcluster.com/solutions/run-ai), [Volcano](https://volcano.sh/en), and [Kubeflow](https://www.kubeflow.org), enabling you to fully isolate your AI/ML workloads and environments.
We'll take a closer look at using vCluster for GPU-equipped workflows in the next article in this series.
## Conclusion
GPU sharing maximizes GPU utilization in your Kubernetes clusters. You can reduce operating costs and improve efficiency by allowing multiple AI, ML, and big data workloads to access the same physical GPU.
NVIDIA's MIG and GPU time-slicing mechanisms provide the basics needed to successfully share your GPUs. With MIG, you can partition your hardware into seven isolated instances, whereas time-slicing lets you create replicas that evenly divide the GPU's compute time between different processes. Alternative DIY approaches are also available, but they're generally complex to configure and brittle in use.
GPU sharing doesn't end with these technologies, though. Virtual clusters are the crucial final piece for building full multitenancy in Kubernetes. By combining vCluster, MIG, and time-slicing, you can safely use a small number of GPUs to serve many independent workloads. Your virtual clusters separate the different tenanted resources in your physical Kubernetes environment, while MIG guarantees hardware-level GPU isolation. This reduces GPU infrastructure requirements for AI/ML deployments, increasing your return on investment.
If you're building robust GPU-enabled platforms, we've created a comprehensive resource to help. Download our free ebook, "GPU-Enabled Platforms on Kubernetes," which explains how Kubernetes abstracts GPU resources, why traditional isolation fails, and what architectural patterns enable multi-tenant GPU platforms. This guide covers everything from how GPUs meet Kubernetes and why GPU multi-tenancy is hard, to orchestrating GPU sharing, hardware isolation and enforcement, and architecting GPU infrastructure with vCluster for optimal isolation and efficiency. Download the eBook [here](https://www.vcluster.com/ebook/gpu-enabled-platforms-on-kubernetes).
Share:
Copied!
Table of Contents
[Understanding the Kubernetes GPU-Sharing Problem](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#understanding-the-kubernetes-gpu-sharing-problem)
[Approach 1: Time-Slicing with the NVIDIA GPU Operator](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#approach-1-time-slicing-with-the-nvidia-gpu-operator)
[Approach 2: NVIDIA MIG (Multi-Instance GPU)](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#approach-2-nvidia-mig-multi-instance-gpu)
[Approach 3: MIG and Time-Slicing](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#approach-3-mig-and-time-slicing)
[Alternative Work-Arounds and Custom Solutions](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#alternative-work-arounds-and-custom-solutions)
[Manually Assigning GPUs Using Node Labeling and Taints](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#manually-assigning-gpus-using-node-labeling-and-taints)
[Enforcing GPU Quotas via Resource Requests and Quotas](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#enforcing-gpu-quotas-via-resource-requests-and-quotas)
[User-Space GPU-Multiplexing Tools](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#user-space-gpu-multiplexing-tools)
[Container-Level GPU Cgroups and Runtime Hacks](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#container-level-gpu-cgroups-and-runtime-hacks)
[Why These Strategies Fall Short](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#why-these-strategies-fall-short)
[How vCluster Helps Simplify GPU Sharing](https://www.vcluster.com/blog/diy-gpu-sharing-in-kubernetes#how-vcluster-helps-simplify-gpu-sharing)
Subscribe to newsletter
eu1
26230392
15314855-8a4c-4751-ad29-605050b96984
loft-sh
true
By subscribing you agree to our [Privacy Policy](https://www.vcluster.com/legal/privacy) and provide email consent.
Related blog posts
[vCluster Automates Private AI Cloud Services on Deutsche Telekom's Industrial AI Cloud With NVIDIA DSX OS Oct 21, 2026 | 7 min Read ](https://www.vcluster.com/blog/vcluster-deutsche-telekom-industrial-ai-cloud-nvidia-dsx-os)
[vCluster and Open-Source NVIDIA Run:ai Bring Tenant Isolation and Efficient GPU Utilization to NVIDIA DSX AI Factories Oct 13, 2026 | 10 min Read ](https://www.vcluster.com/blog/stronger-tenant-isolation-for-nvidia-run-ai)
[Introducing Stacks: ship AI environments as products, not projects Oct 8, 2026 | 6 min Read ](https://www.vcluster.com/blog/introducing-stacks-ship-ai-environments-as-products-not-projects)
Ready to take vCluster for a spin?
Deploy your first virtual cluster today.
