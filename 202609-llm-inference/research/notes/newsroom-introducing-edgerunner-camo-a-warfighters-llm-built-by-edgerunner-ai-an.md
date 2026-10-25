---
title: 'Newsroom | Introducing EdgeRunner-Camo: A Warfighter’s LLM Built by EdgeRunner
  AI and the U.S. Army Artificial Integration Center (AI2C)'
id: newsroom-introducing-edgerunner-camo-a-warfighters-llm-built-by-edgerunner-ai-an
tags:
- onprem-llm-inference-stacks-859099
- vendor-blog
- onprem-classified
created: '2026-10-23T18:18:26.857842Z'
updated: '2026-10-23T18:21:06.941993Z'
source: https://www.edgerunnerai.com/news/introducing-edgerunner-camo-a-warfighters-llm-built-by-edgerunner-ai-and-the-army-artificial-integration-center-ai2c
source_domain: www.edgerunnerai.com
fetched_at: '2026-10-23T18:18:26.838261Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'EdgeRunner AI blog (vendor, 2026): CamoGPT described in past tense, NIPRNet
  IL5 RAG chat, 75k users, 1.4M conversations; distilled Gemma 4 E4B student from
  Gemma 4 31B teacher. No serving engine/GPU info. Reliability: vendor-authored, medium.'
---

EdgeRunner now available for DoW users at no cost. Click [here](https://military.edgerunnerai.com/) to get started for free.
# Introducing EdgeRunner-Camo: A Warfighter’s LLM Built by EdgeRunner AI and the U.S. Army Artificial Integration Center (AI2C)
### _EdgeRunner and AI2C’s new EdgeRunner-Camo model achieves a 37% reduction in error rates across personnel, logistics, training, policy, weapons, and tactics, demonstrating how domain-specific LLMs can improve operational effectiveness_
### Background
CamoGPT was a chat-based generative AI platform launched in 2024 built by the U.S. Army's [_Artificial Intelligence Integration Center (AI2C)_](https://ai2c.army.mil/). It ran on NIPRNet, where it handled Impact Level 5 (IL-5) data, including Controlled Unclassified Information (CUI). Its core capability was retrieval-augmented generation (RAG) over Army-specific corpora such as doctrine, training materials, and connected sources like SharePoint and the Army Publishing Directorate. The platform reached [_75,000 users_](https://www.dvidshub.net/news/544837/army-intelligence) by mid-2025, and was used as an experimental testbed for agentic AI and emerging capabilities, informing Army requirements for generative AI.
At EdgeRunner AI, we build domain-specific AI for the warfighter that runs locally at the tactical edge. Since 2025, we have partnered with AI2C under a Cooperative Research and Development Agreement (CRADA) to advance state of the art military-specific AI. For one of our projects, we used AI2C’s historic CamoGPT data to train a new LLM that achieved superior performance across multiple military tasks. This serves as a case study for how other groups across the Department of War (DoW) can leverage their internal data to develop more performant domain-specific models for their key use cases.
### Data
We began by assessing 1.4M CamoGPT conversations composed of 17.7M total messages (user and model turns). Across model messages, 16.4k were given positive feedback from users (thumbs-up), and 5k were given negative feedback. For this initial set of experiments, we focused on first-turn messages without any user-uploaded content. Using Gemma 4 E4B, we assessed each example in the dataset to determine (a) if the query was military-related or not, (b) if any portion of the conversation contained Personally Identifiable Information (PII), (c) the category of the conversation, (d) the quality of a given sample for model training, and (e) if the message referenced anything unknowable by a standalone model not provided with any of the tools that have been used in CamoGPT over time (e.g. retrieval augmented generation). All subsequent data processing was conducted after filtering out non-military conversations, PII, and unknowable answers.
To make test data, we held out all the data with positive feedback and apportioned the data according to the categories identified by Gemma 4 E4B, as shown in Figure 1. Across both test and training data, conversations were heavily skewed toward the Personnel and Organization category, but we were still able to achieve statistical significance when examining results for Logistics and Supply; Training and Doctrine; Defense Policy and Strategy; Weapons and Platforms; and Tactics and Operations. To perform each test, we assume the answer provided to the user, to which the user provided a thumbs-up, is correct. We then feed the query, the model-under-test’s answer, and the ground truth answer to a judge model, which determines if the model-under-test’s answer sufficiently matches the thumbs-up answer from the user. In this work we used [Atla AI’s Selene 1 70B model](https://huggingface.co/AtlaAI/Selene-1-Llama-3.3-70B-GPTQ-W8A8/tree/main) as the judge, and our general evaluation methods are described further in [this blog post](https://www.edgerunnerai.com/news/the-edgerunner-model-evaluation-platform).
Next, we created the training data. We built several different data mixtures by adjusting (a) the training data quality as assessed by gemma 4 E4B and (b) the proportions of data from each category we identified. The aforementioned filtering operations, in which we identified cases in which answers were unknowable without the runtime tool suite, as well as cases in which the query was not directly military related, proved crucial. Our final dataset was based on category sampling matching that of the overall population.
All data processing and model training was conducted in an IL5-certified gov cloud environment.
### Methods
We performed training using on-policy distillation with our in-house training framework. Traditional LLM training relies on both queries and responses in order to conduct Supervised Fine Tuning (SFT). Later, various efforts emerged for conducting reinforcement learning, in which the model being trained generates one or more actions which are then graded, and the model’s weights are updated according to the returned reward. [On-policy distillation](https://arxiv.org/abs/2605.23493) is a type of reinforcement learning in which the student model receives a prompt, in this case being the filtered and sampled queries from the CamoGPT traffic. The student then generates one or more rollouts, or guesses, for that query. The teacher can then provide a score of the student’s answer. This score is determined not by looking at the generated tokens in the student’s response, but by comparing the probabilities for each candidate token at each sequence position to those of the teacher, which leads to faster convergence and richer representations that span synonyms and alternate responses.
In this work, we used Gemma 4 E4B as the student model and Gemma 4 31B as the teacher model. During training, we validated the model using EdgeRunner’s proprietary evaluation suite every 1,000 steps. After choosing the 4,000 step checkpoint, we then performed a full evaluation using the test sets described in the previous section.
### Results
Results are given in Figure 1. Across all held out test samples, we observe a 37% reduction in error rate. Error is reduced across all categories, ranging from a 16% reduction in error for Logistics and Supply to a 57% reduction in error for Training and Doctrine.
_Figure 1: Error rate reduction after training a Gemma 4 E4B model using On Policy Distillation (OPD) and training data extracted from the CamoGPT system. Overall error reduction was 37%._
Though these error reductions are substantial, they are only the result of our initial experimentation. We are confident that we can reduce error rates even further over time.
### Looking Ahead
Domain specialization, including for military functional areas, allows us to shrink model sizes by 10x and more while still achieving [frontier-level accuracy](https://www.edgerunnerai.com/news/edgerunner-2-a-new-state-of-the-art-for-military-llms-on-the-edge) on military tasks. Such efficiency is important not only for enabling edge deployment on each warfighter’s local hardware, but also for radically decreasing costs when hosting AI solutions in server-client architectures.
The work presented in this blog post represents our initial efforts with this dataset and use case. EdgeRunner-Camo is well suited as a generalist virtual assistant for the Army that can be deployed in-garrison on a wide variety of inference architectures or in the field at the tactical edge using EdgeRunner’s optimized inference runners. 
As future work, we plan to expand the training curriculum using EdgeRunner’s full post-training pipeline, which includes supervised fine tuning for knowledge acquisition, on-policy distillation with other prompts and teachers, grounded Group Relative Policy Optimization (GRPO), and Direct Preference Optimization (DPO). Moreover, modern AI systems do much more than Q&A, and we further train our models to be effective at tool calling, at agentic knowledge retrieval, and more.
### About the authors
_Jack FitzGerald is Chief Science Officer at EdgeRunner AI. He is a graduate of the US Air Force Academy and the Air Force Institute of Technology. Before joining EdgeRunner, he was a nuclear physicist and Air Force officer, after which he was a principal scientist at Amazon, where he worked on Amazon’s first LLMs._
_Brian King is Distinguished Scientist at EdgeRunner AI. He was previously a Senior Applied Scientist at Amazon where he worked on speech recognition for Alexa and multi-modal LLM’s. He holds 12 patents in applications in machine learning, speech recognition, and digital signal processing._
_CPT Aidan Doyle and SSG Ryan Cowgill are Data Engineers at the Artificial Intelligence Integration Center (AI2C) where they helped establish and deploy CamoGPT._
