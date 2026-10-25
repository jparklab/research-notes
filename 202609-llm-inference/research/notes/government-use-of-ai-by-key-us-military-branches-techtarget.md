---
title: Government use of AI by key U.S. military branches | TechTarget
id: government-use-of-ai-by-key-us-military-branches-techtarget
tags:
- onprem-llm-inference-stacks-859099
- locus-sovereignty-regulation-forces-onprem
- camogpt
created: '2026-10-23T17:56:49.310603Z'
updated: '2026-10-23T17:58:39.485032Z'
source: https://techtarget.com/searchenterpriseai/news/366613198/Government-use-of-AI-by-key-US-military-branches
source_domain: techtarget.com
fetched_at: '2026-10-23T17:56:49.289840Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'TechTarget: CENTCOM team put an LLM on SIPRNet Jan-Feb; Army CamoGPT ~10,000
  users. No hardware/model/engine named.'
---

* [AI Ethics & Governance](https://www.techtarget.com/ai/resources/AI-Ethics-Governance)


Getty Images
Getty Images
By
  * [Esther Shittu,](https://www.techtarget.com/contributor/Esther-Ajao) News Writer


Published: 09 Oct 2024
On Oct. 7, 2023, the U.S. Central Command, which is responsible for defense operations in the Middle East and Central Asia, was stunned when Hamas attacked Israel, setting off a brutal war in Gaza.
With Iran-backed militias increasingly attacking U.S. military bases in Iraq and Syria, the government organization's operations were disrupted.
"Within a 24-hour period, the pace and operational tempo of our command changed," said Schuyler Moore, CTO of U.S. Central Command, during a panel discussion on Oct. 8 at the [Nvidia](https://www.techtarget.com/whatis/definition/NVIDIA) AI Summit in Washington D.C.
While this meant a big shift on the ground, it also led to a management change. Operationally, Central Command saw an increase in the number of meetings as well as the need for summaries of those meetings.
"Suddenly, this crushing weight descended on our staff for the requirement to suddenly participate, summarize and push all of this information that was related to critical events in a timely way not only within our own command but also outside of our command, to the National Security Council and more," Moore said.
## Finding a way with GenAI
One way Central Command explored dealing with this disruption was with [generative AI](https://www.techtarget.com/searchenterpriseai/definition/generative-AI).
With [speech recognition](https://www.techtarget.com/searchunifiedcommunications/answer/What-are-the-risks-and-benefits-of-AI-driven-speech-technology) technology such as Whisper from OpenAI, it seemed that the way to address this problem was to break apart audio recordings and generate summaries. However, that approach did not have the desired effect, Moore said.
Recipients of the summaries had different requirements about what they considered important. Through the experience, Central Command realized that using generative AI for summaries was not helpful. However, the failed use case was just the beginning for the government military agency.
From early January to February, the Command found a way to get an LLM available on the government's classified network, Secured Internet Protocol Router Network ([SIPRNet](https://www.caci.com/sipr)).
"Working on a classified network is like working in a barren wasteland," Moore said. "You are not able to access the open internet."
However, most of the work Moore's team does is on SIPRNet, with nearly no work on an unclassified classified network like Google.
"If we need applications built, we need them on [SIPRNet]," she said. "If we need tools available, we need them on [SIPRNet]."
An LLM on SIPRNet was useful for several applications. First, it helped with code augmentation and generation. Previously, programmers would need to use two different networks: SIPRNet and Google on another computer used for unclassified information.
With the LLM on SIPRNet, everything was in a single location.
"We think the obvious reason that it was particularly successful is it's quite easy to catch errors very candidly with quick utility, but also very quick to notice if your code just doesn't generate the output and low risk in general, if it is not doing it correctly," Moore said.
Another use of generative AI for Central Command is machine-assisted disclosure. This means that the LLM can help a human determine what documents are classified and cannot be disclosed and what documents can be disclosed.
Finally, Central Command found that generative AI could still help with summarization, but its capabilities were limited. Instead of feeding the LLM multiple documents and having the LLM spit out a "magic" summary, Central Command learned that the model could lead a user to a specific part of a document so that the user knows what they need to focus on.
"Treating large language models as a cog in a broader wheel -- it is not the wheel. It will not do the entire process for you and spit out your outcome," Moore said.
Esther Ajao/TechTarget
Participants in panel discussion at Nvidia AI Summit discuss ways U.S. Central Command, Air Force and Army use generative AI. 
## The Air Force and NIPRGPT
Central Command is not the only government agency using generative AI as a tool. The Air Force has also created its own AI chatbot named [NIPRGPT](https://www.af.mil/News/Article-Display/Article/3800809/department-of-the-air-force-launches-niprgpt/). NIPRGPT is part of the Dark Saber software platform developed at the Air Force Research Laboratory.
Dark Saber is a group of U.S. Air Force innovators and developers who create software systems.
NIPRGPT enables users to have human-like conversations with it. As a generative AI tool, it can answer questions and assist with tasks such as coding within a secure environment.
"When we originally set out to host this, in the beginning, we thought that most people wouldn't use it," said Collen Roller, a senior computer scientist at the Air Force Research Laboratory, during the panel discussion. He added that the hypothesis was that while there was demand for [ChatGPT](https://www.techtarget.com/searchenterpriseai/video/ChatGPT-explained-in-a-minute) and other generative AI chatbots in the consumer world, that might not be the case in the government sector.
However, that hypothesis proved to be wrong.
"The demand for GenAI in the Department of Defense is real," Roller said. "It's something that we collectively need to care about and focus on. And we had a lot of interest in NIPRGPT right out of the gate."
Many people used NIPRGPT for toil reduction tasks, such as creating presentation outlines or roll-ups of different meetings, he added.
Air Force officials also feared that NIPRGPT could be misused.
"We thought that when people first went to a government GPT, we thought that people were going to be asking maybe how to make something destructive or trying to ask some ridiculous questions," Roller said. "We have found that that's not the case. People are just trying to get time back."
## The Army and CamoGPT
Saving time is also important to the U.S. Army, which is testing out a GPT product named CamoGPT. CamoGPT currently is being used by about 10,000 Army members.
Testing this model has led the Army to ask hard questions about whether the technology is worth investing in and what soldiers will use it for, said Isaac Faber, director of the U.S. Army AI Center, during the same panel discussion with Moore and Roller.
"From the Army's perspective, we're going to experiment a lot on … [and look at] what it means to adopt some of these technologies," Faber said.
## GenAI risk
While the Army and others in the government sector are taking advantage of generative AI technology, they are doing so cautiously.
"There's a risk dimension of AI," said Young Bang, principal deputy assistant secretary of the Army for acquisition, logistics and technology, during a different panel exploring scaling generative AI technology.
> Treating large language models as a cog in a broader wheel -- it is not the wheel. It will not do the entire process for you and spit out your outcome.  **Schuyler Moore** CTO, U.S. Central Command 
Policies governing the use of the technology, including the use of watermarks, in the Army aim to ensure that data is protected. The Army is also aware of how AI technology is used against the U.S.
"How do we counter AI, or how are our peer threats looking at AI and using it on us?" Bang said. "Counter AI is critical for us."
The Army is also exploring what [artificial general intelligence (AGI)](https://www.techtarget.com/searchenterpriseai/definition/artificial-general-intelligence-AGI) -- a concept that defines AI as capable of doing anything a human can do -- will mean.
"When you get to AGI, it's really behaviors and outcomes, [and] we have no idea what the outcome should be," Bang said.
"We're trying to look two, three steps ahead of what are going to be the roadblocks for us to adopt at mass and scale [as well as] the volumes that we're talking about. We're trying to address that now," he added.
_Esther Ajao is a TechTarget Editorial news writer and podcast host covering artificial intelligence software and systems._
####  Dig Deeper on AI Ethics & Governance
  * ##### [Did an AI agent really break free and attack another company? By: Dan Raywood ](https://www.computerweekly.com/news/366646245/Did-an-AI-agent-really-break-free-and-attack-another-company)
  * ##### [What Amazon Q prompt injection reveals about AI security By: Beth Pariseau ](https://www.techtarget.com/it-infrastructure/news/366628167/What-Amazon-Q-prompt-injection-reveals-about-AI-security)
  * ##### [MoD set to develop £50m data analytics platform with Kainos By: Brian McKenna ](https://www.computerweekly.com/news/366618611/MoD-set-to-develop-50m-data-analytics-platform-with-Kainos)
  * ##### [ENIAC (Electronic Numerical Integrator And Computer) By: Robert Sheldon ](https://www.techtarget.com/whatis/definition/ENIAC)


  * [Max-severity GitLab flaw lets attackers run code on servers](https://www.techtarget.com/cybersecurity/news/366652429/Max-severity-GitLab-flaw-lets-attackers-run-code-on-servers)
A high-impact GitLab flaw lets unauthenticated attackers run code on self-managed servers. Every release since 2014 is affected.
  * [PHP2Shell disclosure unravels after leaks forced early patch drop](https://www.techtarget.com/cybersecurity/news/366652613/PHP2Shell-disclosure-unravels-after-leaks-forced-early-patch-drop)
Although website shutdowns were inevitable to patch critical PHP flaws, early leaks and a broken embargo turned routine ...
  * [Just-in-time access: A CISO's guide to reducing standing privilege](https://www.techtarget.com/cybersecurity/tip/Just-in-time-access-A-CISOs-guide-to-reducing-standing-privilege)
Adopting just-in-time access helps companies better control who is accessing corporate resources, dovetailing with other key ...


  * [How CarGurus cleared the roadblocks to its AI initiatives](https://www.techtarget.com/data-technologies/feature/How-CarGurus-cleared-the-roadblocks-to-its-AI-initiatives)
After the company found that its existing data environment was stalling its AI plans, it launched a multifaceted effort to update...
  * [Databricks' latest improves AI memory, infrastructure for agents](https://www.techtarget.com/data-technologies/news/366652428/Databricks-latest-improves-AI-memory-infrastructure-for-agents)
The vendor's new additions address some of the real-world problems customers are having as they attempt to deploy agents at scale.
  * [AgentSpot a means for ThoughtSpot users to build and share AI](https://www.techtarget.com/data-technologies/news/366652510/AgentSpot-a-means-for-ThoughtSpot-users-to-build-and-share-AI)
With business decisions often a collaborative process but AI tools frequently used in isolation, the vendor's new workspace ...


  * [How to fit customer experience security into your strategy](https://www.techtarget.com/enterprise-software/tip/How-to-fit-customer-experience-security-into-your-strategy)
Most organizations overlook security in their CX strategies. However, with collaboration, personalization, CIAM controls and more...
  * [Customer journey mapping tools: What they do and how to choose one](https://www.techtarget.com/enterprise-software/tip/9-customer-journey-mapping-tools-that-can-improve-your-CX)
Customers are more diverse and demanding than ever. Enterprises need sophisticated mapping tools to help them determine how to ...
  * [CX personalization: Why it's important and how to deliver it](https://www.techtarget.com/enterprise-software/tip/CX-personalization-Why-its-important-and-how-to-deliver-it)
The more personalized the message, the better chance a brand has to increase loyalty and build customer retention. But beware of ...


  * [How network FinOps can control connectivity and cloud costs](https://www.techtarget.com/it-infrastructure/tip/How-network-FinOps-can-control-connectivity-and-cloud-costs)
While networking costs are challenging to manage, applying FinOps principles to network management can help. It starts with ...
  * [AI factories package Nvidia reference architecture, but who's buying?](https://www.techtarget.com/it-infrastructure/news/366652549/AI-factories-package-Nvidia-reference-architecture-but-whos-buying)
Nvidia partners bundle the OpenShell Reference Architecture's infrastructure-level AI safeguards in AI factories, but enterprise ...
  * [IT leaders test the waters of autonomous network operations](https://www.techtarget.com/it-infrastructure/news/366652573/IT-leaders-test-the-waters-of-autonomous-network-operations)
AI agents largely remain in an advisory role for NetOps teams as they take a 'trust but verify' approach, but autonomy is growing...


  * [Why CIOs and CHROs should be forming strategic partnerships](https://www.techtarget.com/it-strategy/feature/Why-CIOs-and-CHROs-should-be-forming-strategic-partnerships)
Previously unlikely bedfellows, CIOs and CHROs are starting to realize how AI's impact on workforce management is turning HR into...
  * [Cloud ERP deployment models: How to choose the right option](https://www.techtarget.com/it-strategy/tip/Cloud-ERP-deployment-models-How-to-choose-the-right-option)
SaaS ERP is most often chosen by small and midsize enterprises as well as companies replacing a lightly modified legacy system. ...
  * [Gartner unveils top technology trends at Gartner IT Symposium](https://www.techtarget.com/it-strategy/feature/Gartner-unveils-top-technology-trends-at-Gartner-IT-Symposium)
Gartner unveils its top 10 strategic technology trends for 2027, including large quantitative models, AI agent identity and ...


[©2026 TechTarget](https://www.informatechtarget.com/terms-of-use), Inc. d/b/a Informa TechTarget. All Rights Reserved.  [Cookie Preferences ](https://www.techtarget.com/ai/news/366613198/Government-use-of-AI-by-key-US-military-branches) [Cookie Preferences ](https://www.techtarget.com/ai/news/366613198/Government-use-of-AI-by-key-US-military-branches) [Do Not Sell or Share My Personal Information](https://techtarget.zendesk.com/hc/en-us/requests/new?ticket_form_id=360004852434)
Close
