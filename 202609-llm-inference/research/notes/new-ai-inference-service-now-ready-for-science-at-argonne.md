---
title: New AI Inference Service Now Ready for Science at Argonne
id: new-ai-inference-service-now-ready-for-science-at-argonne
tags:
- onprem-llm-inference-stacks-859099
- hpc
created: '2026-10-23T18:15:48.165759Z'
updated: '2026-10-23T18:17:22.081380Z'
source: https://www.globus.org/news/new-ai-inference-service-for-science
source_domain: www.globus.org
fetched_at: '2026-10-23T18:15:48.144889Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'HPCwire/Globus (Jun 2026): ALCF Inference Service ~35 models, ~10 loaded
  at a time, across 12 DOE labs; no utilization figure.'
---

[Skip to main content](https://www.globus.org/news/new-ai-inference-service-for-science#main)  
# New AI Inference Service Now Ready for Science at Argonne
June 01, 2026 | HPCwire By Alex Woodie
AI can help accelerate scientific discovery, but setting up and running a foundation model is not a simple task. Thanks to the work of the Argonne Leadership Computing Facility, scientists affiliated with DOE National Labs and the Genesis Mission can now tap into a new AI inference service running on ALCF supercomputers.
Dubbed the ALCF Inference Service, the new service enables DOE scientists to interact with dozens of open foundation models in batch and interactive modes. This includes [Google’s](http://www.google.com/) Gemma series, [Meta’s](http://www.meta.com/) LLaMA models, and [OpenAI’s](http://www.openai.com/) GPT-OSS family, as well as domain-specific foundation models, computer vision models and in-house models developed at Argonne, like AuroraGPT.
“There are people doing science on it now,” Papka said. “What we really see is the scientific community building this into their workflows.”
The inference service will span roughly 35 AI models, with about 10 of them loaded at any given time. If a user requests a model that is not active there, they can make a request and the model will enter a queue to get spun up.
Dealing with a large number of concurrent users is no trivial task, but it’s something that the National Labs will need to address if it’s going to provide the sort of “dial tone” service for AI inference that the large commercial AI providers can deliver. The ALCF Inference Service is based on a [2025 paper](https://dl.acm.org/doi/epdf/10.1145/3731599.3767346) by ANL scientists and University of Chicago professors on a product dubbed Federated Inference Resource Scheduling Toolkit, or FIRST.
FIRST consists of three main components, including an Inference Gateway API (based on OpenAI’s API) to process user requests; [Globus Compute](https://www.globus.org/compute) to execute tasks on HPC resources; and Model Serving Tools to efficiently perform the LLM inference. “The framework addresses the growing demand for private, secure, and scalable AI inference in scientific workflows, allowing researchers to generate billions of tokens daily on-premises without relying on commercial cloud infrastructure,” the paper’s authors write.
The AI inference service isn’t open just to ALCF users, but any users across 12 DOE Labs as part of the Genesis Mission projects, including the American Science Cloud (AmSc) and Transformational AI Models Consortium (ModCon), Papka said.
“What we’ve been slowly doing with DOE as part of the Genesis Mission is opening this up to the ModCon team, so researchers that are part of ModCon can say ‘We want to use this’ and then they just get added to a list where we’re leveraging Globus authentication,” Papka said. “They authenticate to their local Globus accounts, so they don’t necessarily need an ALCF account.”
This opens the door for researchers from Brookhaven National Lab, Pacific Northwest National Laboratory, and other labs to get access to the ALCF chat interface as well as the API, Papka said. When they register using their DOE accounts, they get tokens to use on the new inference service.
Those tokens are good for a length of time, which means that people don’t need to continually re-log-in to the system, Papka said. That’s a little different than how commercial AI services work, he said.
ALCF Director Michael Papka
“We want to be very responsive to scientists. Their asks are likely to be different than what maybe the commercial vendors will be pursuing,” Papka said. “As an HPC facility, allowing for these long running tokens is something different and that’s how we’ve adapted to this new workflow.”
[Read the full original article](https://www.hpcwire.com/2026/06/01/new-ai-inference-service-now-ready-for-science-at-argonne/)
[ Older Free Globus Subscriptions Available to Research Non-Profits  ](https://www.globus.org/news/free-globus-subscriptions-available-to-research-non-profits)
