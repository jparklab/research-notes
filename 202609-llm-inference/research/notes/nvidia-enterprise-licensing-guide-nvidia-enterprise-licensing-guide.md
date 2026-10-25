---
title: NVIDIA Enterprise Licensing Guide — NVIDIA Enterprise Licensing Guide
id: nvidia-enterprise-licensing-guide-nvidia-enterprise-licensing-guide
tags:
- onprem-llm-inference-stacks-859099
- vendor-onprem
- vendor-docs
created: '2026-10-23T17:23:47.130805Z'
updated: '2026-10-23T17:28:44.350349Z'
source: https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/index.html
source_domain: docs.nvidia.com
fetched_at: '2026-10-23T17:23:47.110609Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NVIDIA Enterprise Licensing Guide docs index (docs.nvidia.com): hub page
  for the licensing-guide set covering NVIDIA AI Enterprise and vGPU entitlement,
  the Cloud License Service (CLS) and the on-premises Delegated License Service (DLS).
  Fetched as the primary-source anchor for the per-GPU licensing and air-gapped node-locked
  licence claims that the reseller and analyst secondary sources make.'
---

*Suggested by [[eurokommerz]] — cited as primary vendor source*

[Skip to main content](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/index.html#main-content)
Back to top
[ NVIDIA Enterprise Licensing Guide ](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/index.html)


Light Dark System Settings
# NVIDIA Enterprise Licensing Guide[#](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/index.html#nvidia-enterprise-licensing-guide "Link to this heading")
Document History
SWE-NVAIE-014-APNT  
| Version  | Date  | Authors  | Description of Change  |  
| --- | --- | --- | --- |  
| 01  | September 2024  | AF  | Major document revision  |  
| 02  | October 2024  | DK, AH  | Major document revision, Omniverse inclusion  |  
NVIDIA AI Enterprise
  * [Platform Overview](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/platform-overview.html)
    * [Application Layer](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/platform-overview.html#application-layer)
      * [Application Layer Software](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/platform-overview.html#application-layer-software)
    * [Infrastructure Layer](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/platform-overview.html#infrastructure-layer)
      * [Infrastructure Layer Software](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/platform-overview.html#infrastructure-layer-software)
      * [Infra Software Branch Support Matrix](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/platform-overview.html#infra-software-branch-support-matrix)
      * [Infra Software Branch Release Notes](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/platform-overview.html#infra-software-branch-release-notes)


NVIDIA Enterprise Licensing Guide
  * [Overview](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/overview.html)
    * [NVIDIA AI Enterprise](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/overview.html#nvidia-ai-enterprise)
  * [NVIDIA AI Enterprise Licensing](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/licensing.html)
    * [Subscription License](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/licensing.html#subscription-license)
    * [Selected NVIDIA GPUs Include NVIDIA AI Enterprise](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/licensing.html#selected-nvidia-gpus-include-nvidia-ai-enterprise)
    * [NVIDIA DGX Software Bundle & NVIDIA AI Enterprise](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/licensing.html#nvidia-dgx-software-bundle-nvidia-ai-enterprise)
    * [Cloud Deployment of NVIDIA AI Enterprise](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/licensing.html#cloud-deployment-of-nvidia-ai-enterprise)
  * [Pricing](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html)
    * [Self-managed systems](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#self-managed-systems)
      * [Available via NVIDIA Partner Network](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#available-via-nvidia-partner-network)
      * [Available Support and Services Upgrades](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#available-support-and-services-upgrades)
    * [Cloud-hosted systems](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#cloud-hosted-systems)
      * [CSP Marketplace (public cloud) availability](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#csp-marketplace-public-cloud-availability)
    * [NVIDIA Education Pricing Program](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#nvidia-education-pricing-program)
    * [NVIDIA Inception Pricing Program](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#nvidia-inception-pricing-program)
    * [NVIDIA Connect Pricing Program](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html#nvidia-connect-pricing-program)
  * [NVIDIA License System](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/license-system.html)


Notices
  * [Notices](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/notices.html)


NVIDIA uses cookies to improve your experience on our web site. We and our third-party partners also use cookies and other tools to collect and record information you provide as well as information about your interactions with our websites for performance improvement, analytics, and to assist in marketing efforts. By clicking "Accept All", you consent to our use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/). You can manage your cookie settings by clicking on "Manage Settings." By continuing to use this site or by clicking one of the buttons below, you agree to our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control (GPC) signal and recorded your rejection of all optional cookies on this site for this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. To opt out of non-cookie personal information "sales" / "sharing" for targeted advertising purposes, please visit the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies. You can manage these settings in the NVIDIA [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies which overrides at least one of your previous settings. You can manage them in the [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
Manage Settings
Reject Optional Accept All
Cookie Settings
We and our third-party partners (including social media, advertising, and analytics partners) use cookies and other tracking technologies to collect, store, monitor, and process certain information about you when you visit our website. The information collected might relate to you, your preferences, or your device. We use that information to make the site work, analyze performance and traffic on our website, provide a more personalized web experience, and assist in our marketing efforts.Under certain privacy laws, you have the right to direct us not to "sell" or "share" your personal information for targeted advertising. To opt-out of the "sale" and "sharing" of personal information through cookies, you must opt-out of optional cookies using the toggles below. To opt out of the "sale" and "sharing" of data collected by other means (e.g., online forms) you must also update your data sharing preferences through the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/).Click on the different category headings below to find out more and change the settings according to your preference. You cannot opt out of Required Cookies as they are deployed to ensure the proper functioning of our website (such as prompting the cookie banner and remembering your settings, etc.). By clicking "Save and Accept" or "Decline All" at the bottom, you consent to the use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) in accordance with your settings and accept our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). For more information about our privacy practices, please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/).
Required Cookies
Always Active
These cookies enable core functionality such as security, network management, and accessibility. These cookies are required for the site to function and cannot be turned off.
Cookies Details
Performance Cookies
Performance Cookies
These cookies are used to provide quantitative measures of our website visitors, such as the number of times you visit, time on page, your mouse movements, scrolling, clicks and keystroke activity on the websites; other browsing, search, or product research behavior; and what brought you to our site. These cookies may store a unique ID so that our system will remember you when you return. Information collected with these cookies is used to measure and find ways to improve website performance.
Cookies Details
Personalization Cookies
Personalization Cookies
These cookies collect data about how you have interacted with our website to help us improve your web experience, such as which pages you have visited. These cookies may store a unique ID so that our system will remember you when you return. They may be set by us or by third party providers whose services we have added to our pages. These cookies enable us to provide enhanced website functionality and personalization as well as make the marketing messages we send to you more relevant to your interests. If you do not allow these cookies, then some or all of these services may not function properly.
Cookies Details
Advertising Cookies
Advertising Cookies
These cookies record your visit to our websites, the pages you have visited and the links you have followed to influence the advertisements that you see on other websites. These cookies and the information they collect may be managed by other companies, including our advertising partners, and may be used to build a profile of your interests and show you relevant advertising on other sites. We and our advertising partners will use this information to make our websites and the advertising displayed on it, more relevant to your interests.
Cookies Details
Back Button
Cookie List
Search Icon
Filter Icon
Clear
  * checkbox label label


Apply Cancel
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Decline All Save and Accept
