---
title: ALCF launches first large-scale AI inference service for open science | Argonne
  Leadership Computing Facility
id: alcf-launches-first-large-scale-ai-inference-service-for-open-science-argonne-le
tags:
- onprem-llm-inference-stacks-859099
- hpc-inference
- national-lab
- government-sovereign
created: '2026-10-23T17:21:39.726035Z'
updated: '2026-10-23T17:24:43.562889Z'
source: https://www.alcf.anl.gov/news/alcf-launches-first-large-scale-ai-inference-service-open-science
source_domain: www.alcf.anl.gov
fetched_at: '2026-10-23T17:21:39.691369Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: policy
deprecated: false
summary: 'Argonne press release (26 May 2026): ALCF Inference Service hosts Gemma,
  Llama, GPT-OSS, AuroraGPT and domain/vision models on Sophia, Metis (SambaNova),
  Aurora; home-institution credentials for 8 DOE labs; positioned as stable, transparent
  alternative to vendor-managed frontier models that change without notice.'
---

*Suggested by [[argonne-launches-high-performance-computing-backed-ai-research-service-nextgovfc]] — press release cited by Nextgov*

[ Skip to main content ](https://www.alcf.anl.gov/news/alcf-launches-first-large-scale-ai-inference-service-open-science#main-content)
#  ALCF launches first large-scale AI inference service for open science
announcements
Author Jim Collins
Published 05/26/2026
Argonne's inference service is powered by ALCF systems including Sophia (left) and Metis (right). (Image: Argonne National Laboratory)
A key enabler for DOE’s Genesis Mission, the ALCF Inference Service helps researchers accelerate the path from data to insight to discovery.
The U.S. Department of Energy’s Argonne National Laboratory has launched a first-of-its-kind AI inference service to help researchers across the nation accelerate discovery and innovation.
The service offers cloud-like access to a range of large language models (LLMs) and science foundation models running on Argonne’s high-performance computing (HPC) systems. This gives researchers a powerful and secure resource for analyzing large datasets and testing new ideas.
“Our inference service helps close the gap between developing AI models and putting them to work in scientific research,” said Michael Papka, director of the Argonne Leadership Computing Facility (ALCF). “By offering AI inference as a shared resource, we enable researchers to apply AI at scale to their data, simulations, and experiments, without the burden of building and maintaining their own infrastructure.”
Inference is the process of using trained AI models to analyze data, identify patterns and make predictions. AI chatbots like ChatGPT use inference to answer questions in real time. In research, the same capability can help scientists guide experiments, make sense of complex data and perform other analytical tasks more efficiently.
“Inference services allow researchers to spend less time managing models and more time testing hypotheses,” said Venkat Vishwanath, AI and machine learning lead at the ALCF. “Instead of taking days or weeks to analyze data, scientists can rapidly interpret results, refine experiments and explore complex systems in ways that weren’t practical before.”
#### **Providing scalable AI inference for scientific research**
The service grew out of a [2025 paper](https://dl.acm.org/doi/10.1145/3731599.3767346) that outlined a framework for providing secure, scalable AI inference across distributed HPC systems. The goal was to give researchers the ability to run parallel inference workloads on different models without relying on commercial cloud infrastructure.
This work led to the creation of the [ALCF Inference Service](https://docs.alcf.anl.gov/services/inference-endpoints/). It provides access to a range of LLMs, including Google’s Gemma series, Meta’s LLaMA models, and OpenAI’s GPT-OSS family, as well as domain-specific foundation models, computer vision models, and in-house models developed at Argonne, such as AuroraGPT. The service hosts these models on ALCF computing systems, giving researchers more flexibility in how models are used and how results are produced. Unlike vendor-managed frontier AI models, which can change without notice, the ALCF service offers a more stable and transparent environment for scientific research. The ALCF is a DOE Office of Science user facility.
The service is being used by a growing and diverse set of researchers. In addition to a substantial base of Argonne and ALCF users, it is actively supporting users across the DOE national laboratory ecosystem, enabling seamless access for researchers from several labs using their home institution credentials. These include Brookhaven National Laboratory, Fermi National Accelerator Laboratory, Los Alamos National Laboratory, Lawrence Berkeley National Laboratory, Lawrence Livermore National Laboratory, Oak Ridge National Laboratory, Sandia National Laboratories, and Thomas Jefferson National Accelerator Facility. This expanding cross-lab adoption underscores the service’s role in enabling integrated, multi-institutional research workflows.
Researchers can access a broad range of AI models through the service, from popular LLM families like Gemma, LLaMA, and GPT-OSS to domain-specific scientific foundation models and computer vision models. (Image: Argonne National Laboratory)
#### **Accelerating research across diverse fields**
Among its users, the ALCF Inference Service is supporting teams working on [DOE’s Genesis Mission](https://www.energy.gov/undersecretaryforscience/genesis-mission/genesis-mission), a national AI initiative to build the world's most powerful scientific platform to accelerate discovery science, strengthen national security, and drive energy innovation. It will also be a key tool for the [American Science Cloud (AmSC)](https://amsc.energy.gov/), the Genesis Mission’s integrated environment connecting DOE supercomputers, experimental facilities, and data resources.
Beyond the Genesis Mission, the ALCF’s inference capabilities enable scientists to tackle complex challenges across many fields. In fusion energy research, for example, AI models can analyze streams of experimental data in real time and predict plasma disruptions before they occur, providing insights to enable safer and more efficient control of fusion reactions. In high energy physics and astronomy, inference helps scientists sift through massive volumes of collider and telescope data to identify rare events and new phenomena more quickly.
The service is also being used to advance work in chemistry and materials science. A recent example is [ChemGraph](https://www.nature.com/articles/s42004-025-01776-9), an AI framework that helps researchers simplify and automate molecular simulation workflows and other computational chemistry tasks. By using the ALCF Inference Service for LLM-driven tasks, ChemGraph lets researchers manage complex, multi-step simulation workflows interactively.
“This allows scientists to explore more candidate molecules, iterate on designs faster and manage large-scale calculations as an integrated process rather than a series of disconnected jobs,” said Murat Keçeli, an Argonne computational scientist who helped develop ChemGraph.
ChemGraph relies on repeated interactions between the AI and simulation tools, known as “tool calling,” to break complex workflows into smaller steps and manage large amounts of data effectively. Each interaction requires the AI model to process information in units referred to as tokens. These can add up quickly and increase computational costs.
“Since tool calls can be frequent and might involve large inputs and outputs, token consumption can be much higher than conversation style usage and cost a lot for frontier AI models,” Keçeli said. “Therefore, the inference service is very useful for agentic AI applications to minimize the cost.”
To support these scientific workflows, the service runs on dedicated ALCF systems, including Sophia, an NVIDIA DGX A100 cluster, and Metis, a SambaNova Systems platform designed for high-throughput, low-latency AI inference workloads. In the near future, the service will also run on the facility’s new NVIDIA-based systems, Tara and Minerva. These machines work alongside the facility’s other powerful computing resources, such as the Aurora exascale supercomputer and the ALCF AI Testbed, giving researchers a broad set of capabilities for simulation, data-intensive, and AI-driven science.
The service relies on [Globus Compute](https://www.globus.org/compute) and [Globus Auth](https://www.globus.org/globus-auth-service) to enable secure, federated access to AI inference capabilities on ALCF systems. This allows researchers from different institutions to use their existing organizational credentials while supporting inference workflows across distributed computing resources.
#### **Expanding from computing to service-enabled science**
The service itself is part of the ALCF’s broader [Service-Enabled Science program](https://www.alcf.anl.gov/service-enabled-science-program), which brings together HPC and AI resources, integrated workflow tools, AI model training capabilities, and large-scale data sharing and analysis. This approach provides scientists with a complete suite of tools and services, supporting every stage of their research projects.
“ALCF’s shift from providing raw computing power to delivering integrated services gives researchers a connected environment that supports data generation, simulation, and AI inference, accelerating scientific discovery,” Papka said.
For details on using the ALCF Inference Service, see the [ALCF user guide](https://docs.alcf.anl.gov/services/inference-endpoints/) or watch the webinar, “[Deploying Inference Services at ALCF](https://www.youtube.com/watch?v=22caQwCemug).”
[ Opens in a new window  ](https://www.facebook.com/sharer/sharer.php?u=https://www.alcf.anl.gov/news/alcf-launches-first-large-scale-ai-inference-service-open-science&title=ALCF%20launches%20first%20large-scale%20AI%20inference%20service%20for%20open%20science "Share to Facebook") [ Opens in a new window  ](https://www.linkedin.com/sharing/share-offsite/?url=https://www.alcf.anl.gov/news/alcf-launches-first-large-scale-ai-inference-service-open-science "Share to LinkedIn")
Menu
