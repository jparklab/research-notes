---
title: E­cient Deployment of a 685B-Parameter Open-
id: ecient-deployment-of-a-685b-parameter-open
tags:
- onprem-llm-inference-stacks-859099
- hpc-llm-serving
- preprint-not-peer-reviewed
- onprem
- case-study
- slurm-hpc
- quantization
- preprint
- national-lab
created: '2026-10-23T18:11:25.681391Z'
updated: '2026-10-23T18:16:00.634611Z'
source: https://www.researchsquare.com/article/rs-6655308/v1.pdf
source_domain: www.researchsquare.com
fetched_at: '2026-10-23T18:11:25.680869Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'LNCC Santos Dumont (Brazil) serving DeepSeek V3-0324 (685B) on 16 H100s:
  Unsloth dynamic-quantized GGUF at 1.78-4.5 bits (186-405GB), served by llama.cpp
  llama-server (NOT vLLM), one independent full instance per node (4x H100, 360GB
  VRAM) across 4 nodes with no cross-node tensor or pipeline parallelism, Slurm-launched
  behind a Ray Serve round-robin router and a double SSH tunnel to an external web
  UI. About 25 tok/s single query and 10-12 tok/s per query at 8 concurrent; context
  ~20k at iq1_s. DeepSeek''s own report recommends a minimum 352 H800 GPUs for acceptable
  SLOs, so this is the corpus''s clearest quantization-for-hardware trade. Research
  Square preprint, not peer reviewed.'
raw_file: raw/ecient-deployment-of-a-685b-parameter-open.pdf
---

*Suggested by [[efficient-deployment-of-a-685b-parameter-open-source-llm-on-the-brazilian-santos]] — full PDF*

E­cient Deployment of a 685B-Parameter Open-
Source LLM on the Brazilian Santos Dumont
Supercomputer
Leon Sul«erry Corrêa Costa 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0003-0423-8043
Matheus Müller Pereira da Silva 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0002-0659-6365
Fábio Lima Custódio 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0001-5134-2340
José Renato Duarte Fajardo 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0002-0303-4033
Bruno Alves Fagundes 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0009-0003-9336-1154
Marcelo Monteiro Galheigo 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0009-0003-1736-7431
Vívian Medeiros 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI)
André Ramos Carneiro 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0002-2809-055X
Wagner Vieira Léo 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI)
Fábio André Machado Porto 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0002-4597-4832
Fábio Borges De Oliveira 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0001-5159-9517
Antônio Tadeu Azevedo Gomes 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0002-0746-4014
Laurent Emmanuel Dardenne 
Laboratório Nacional de Computação Cientí«ca (LNCC/MCTI) https://orcid.org/0000-0001-8518-8062


---

Short Report
Keywords: LLM, DeepSeek V3-0324, Quantized Models, Santos Dumont Brazilian Supercomputer, LNCC,
Carcará
Posted Date: May 14th, 2025
DOI: https://doi.org/10.21203/rs.3.rs-6655308/v1
License:   This work is licensed under a Creative Commons Attribution 4.0 International License.  
Read Full License
Additional Declarations: The authors declare no competing interests.


---

Eﬃcient Deployment of a 685B-Parameter Open-Source
LLM on the Brazilian Santos Dumont Supercomputer
Leon Sulﬁerry Corrˆea Costa*, Matheus M¨uller Pereira da Silva*, F´abio Lima
Cust´odio, Jos´e Renato Duarte Fajardo, Bruno Alves Fagundes, Marcelo
Monteiro Galheigo, V´ıvian Medeiros, Andr´e Ramos Carneiro, Wagner Vieira
L´eo, F´abio Andr´e Machado Porto, F´abio Borges De Oliveira, Antˆonio Tadeu
Azevedo Gomes†, and Laurent Emmanuel Dardenne†
Laborat´orio Nacional de Computa¸c˜ao Cient´ıﬁca (LNCC), Brazil
*These authors contributed equally to this work.
†Corresponding authors: dardenne@lncc.br, atagomes@lncc.br
Abstract
This brief communication presents the deployment of open-source large language mod-
els (LLMs) on the Santos Dumont Brazilian Supercomputer to support the Laborat´orio
Nacional de Computa¸c˜ao Cient´ıﬁca (LNCC) academic community.
The solution, named
Carcar´a, leverages key optimizations such as dynamic quantization techniques to enable an
accessible, scalable, and cost-eﬀective implementation. As a result, a single-instance state-
of-the-art independent model was deployed on a single node equipped with 4 × NVIDIA
H100 GPUs. The quantized models ﬁt entirely within the node’s VRAM, enabling horizontal
scaling without the need for inter-node synchronization. In this way, we could provide ac-
cess to these models to the entire LNCC academic community using only four computational
nodes, demonstrating the eﬃciency and scalability of this approach. Crucially, our approach
ensures data sovereignty, allowing LNCC researchers and postgraduate students to utilize
AI for sensitive research topics with full control over their data, free from the privacy risks
associated with proprietary solutions. This initiative strengthens national scientiﬁc auton-
omy while providing secure and eﬃcient AI tools for academic and research advancements.
The code to deploy this solution is openly available, and we encourage other institutions
to adapt it and support their own communities. Discussions with the Brazilian Ministry of
Science, Technology and Innovation are underway to expand this strategic solution to other
research centers and universities in Brazil.
1


---

1
Main
The rise of artiﬁcial intelligence (AI), particularly generative AI and large language models
(LLMs), is poised to profoundly impact society and reshape the labor market [1, 2]. According
to a report by the International Monetary Fund (IMF), nearly 40% of jobs could be aﬀected
by AI, with high-skilled occupations facing the highest exposure [2].
Moreover, AI is likely
to amplify existing global inequalities, disproportionately aﬀecting developing nations. These
countries often lack the digital infrastructure, institutional frameworks, and skilled workforce
required to adapt to the AI-driven economic shifts.
Left unaddressed, this divergence could
deepen economic disparities, as larger economies leverage AI for productivity gains while others
struggle to keep pace. Proactive measures, such as strategic investments in infrastructure and
workforce development, are essential to ensuring that the beneﬁts of AI are widely shared and
its potential harms eﬀectively mitigated [3, 2].
Science, in particular, is witnessing a growing inﬂuence from LLMs and conversational AI
tools like ChatGPT, aﬀecting researchers across disciplines [4, 5, 3]. According to a Nature survey
of over 1,600 researchers worldwide, AI is now widely used for tasks such as data processing, code
generation, and scientiﬁc writing [6]. These tools oﬀer particular beneﬁts to non-native English
speakers, assisting with grammar, style, and translation. In software development, AI program-
ming assistants have demonstrated measurable productivity gains: a study with GitHub Copilot
users found that nearly 30% of AI-generated code suggestions were accepted on average [7], while
another analysis revealed that AI-assisted programmers completed particular tasks up to 55.8%
faster than those working without AI support [8]. While valid concerns remain about the opaque
and sometimes unpredictable nature of these systems [5]—concerns that deserve careful study
and oversight—the growing body of evidence suggests that AI is not only boosting eﬃciency
but also expanding access, lowering barriers, and reshaping how researchers and students engage
with their work.
Modern state-of-the-art LLMs contain hundreds of billions to trillions of trainable parameters,
necessitating substantial computational infrastructure for both training and deployment [9, 10].
These systems rely heavily on specialized hardware, particularly high-performance graphics pro-
cessing units (GPUs), which represent a signiﬁcant ﬁnancial investment whether purchased or
leased. For instance, in January 2024, Meta AI announced that it would acquire 350,000 NVIDIA
H100 GPUs by the end of that year [11]. Similarly, DeepSeek’s technical report reveals that their
model, despite incorporating several computational optimizations, required 2,048 NVIDIA H800
GPUs for training [10]. The report further recommends a minimum of 352 H800 GPUs to meet
acceptable service-level objectives (SLOs) and maintain high throughput during model serving,
while acknowledging that this requirement presents signiﬁcant challenges, especially for smaller
research teams.
The substantial costs of training and deploying LLMs have led to market consolidation, with
a small number of entities dominating key aspects of the technology, including control over
user data.
This raises signiﬁcant privacy concerns, as sensitive information (such as unpub-
lished research ﬁndings or personal data) becomes vulnerable to potential exploitation [12]. To
mitigate these risks, two complementary approaches show promise: (1) open-source releases of
model architectures and weights, and (2) computational optimization techniques like dynamic
quantization and knowledge distillation that reduce infrastructure requirements. Together, these
strategies could help democratize access and address pressing privacy challenges by making the
deployment of LLMs achievable.
The Laborat´orio Nacional de Computa¸c˜ao Cient´ıﬁca (LNCC) is a renowned research unit
under Brazil’s Ministry of Science, Technology, and Innovation (MCTI), specializing in scientiﬁc
computing and innovation. LNCC employs a dedicated team of 81 researchers and technologists.
2


---

LNCC operates the Santos Dumont Brazilian supercomputer, the most powerful Latin-American
machine dedicated exclusively to scientiﬁc research.
As of November 2024, Santos Dumont
ranked 89th on the TOP500 list1 of the world’s fastest supercomputers, supporting studies from
researchers throughout Brazil in renewable energy, oil and gas, mining, astronomy, and compu-
tational biology, to mention the most prominent applications. Since 2000, LNCC has also oﬀered
advanced master’s and doctoral programs in Computational Modeling, with an interdisciplinary
structure centered on three core pillars: mathematics, computer science, and modeling. This
postgraduate program has earned the highest possible rating from the Coordena¸c˜ao de Aper-
fei¸coamento de Pessoal de N´ıvel Superior (CAPES) in Brazil, indicating international recognition.
As of today, this program has graduated around 400 students at the master’s and doctoral levels,
and currently enrolls 90 graduate students.
This work presents the successful deployment and provision of state-of-the-art LLMs on a
small fraction of the computational nodes of the Santos Dumont supercomputer, to support
LNCC’s academic community (researchers and students). These are dynamically quantized ver-
sions of LLMs published under permissive licenses, particularly DeepSeek V3-0324 (a 685B-
parameter model) [10]. Although these models are publicly accessible, deploying them or even
estimating the required resources remains challenging. In this work, we leverage open-source
tools, models, and packages designed to be accessible even to those without deep technical exper-
tise. This approach demonstrates that institutions can locally host powerful AI systems without
relying on external providers, even with limited resources. With this cost-eﬀective and eﬃcient
solution, we could deliver these models via a ChatGPT-like web interface while maintaining full
data sovereignty. We named our implementation Carcar´a, inspired by the South American bird
of prey that, in Brazilian culture, embodies resilience, cunning, and an indomitable spirit. By
sharing our implementation, we aim to lower barriers for similar deployments, fostering broader
adoption of institution-hosted AI tools in academic, research, or similar settings.
2
Results
We tested and deployed publicly available dynamically quantized versions of DeepSeek V3-0324
(see Methods). The models were made available internally to the LNCC academic community
via a web-based chat interface similar to other proprietary solutions.
Our implementation enables secure, remote access to distributed model servers running across
multiple computational nodes. An external server hosts a web interface through which users in-
teract, with requests routed via a centralized orchestration service. Access to backend model
resources is secured through a two-tier secure tunneling setup, ensuring controlled and scalable
multi-node model serving. A single independent model instance is deployed per node, with quan-
tized models ﬁtting entirely within a single node’s VRAM, enabling horizontal scaling without
inter-node synchronization. Figure 1 illustrates the implemented architecture with 4 computa-
tional nodes.
The DeepSeek V3-0324, a 685B-parameter model, is available in multiple dynamically quan-
tized formats, with sizes varying between 186GB and 405GB depending on the chosen bit preci-
sion (see Methods). While higher bit precision inevitably increases model size, it typically yields
better generation accuracy. When deploying these models, there is a need to balance several
conﬂicting factors, including: model size, the number of simultaneous queries supported, and the
maximum context length per conversation. Ultimately, token generation throughput (the rate
at which tokens are generated across all queries) determines whether the system remains viable
1https://top500.org/lists/top500/2024/11/
3


---

External server
Login node
Compute nodes
PORT_A→Node1:PORT_A
PORT_B→Node2:PORT_A
PORT_C→Node3:PORT_A
PORT-D→Node4:PORT_A
PORT_J→login:PORT_A
PORT_K→login:PORT_B
PORT_L→login:PORT_C
PORT_M→login:PORT_D
Redirects to PORT_J, ..., 
PORT_M
Queries
Node 1
llama-server:PORT_A
Node 2
llama-server:PORT_A
Node 3
llama-server:PORT_A
Node 4
llama-server:PORT_A
SSH tunneling:
PORT_A→Node 1:PORT_A
PORT_B→Node 2:PORT_A
PORT_C→Node 3:PORT_A
PORT_D→Node 4:PORT_A
SSH tunneling:
PORT_J→login:PORT_A
PORT_K→login:PORT_B
PORT_L→login:PORT_C
PORT_M→login:PORT_D
Ray Serve (port PORT_X)
Web UI (port PORT_Y)
Figure 1: Multi-node architecture for LLM model serving using double-layered secure tunneling
and centralized request routing.
for practical use, as a slow text generation can lead to a poor quality of experience and hinder
real-world applicability.
We evaluated four model versions with increasing bit precisions: iq1 s, iq1 m, iq2 xxs, and
q2 k xl (see Methods for detailed speciﬁcations). We measured token generation throughput for
each version across diﬀerent numbers of parallel prompts and batch sizes, employing continuous
batching to optimize inference. All evaluations were conducted on a single node. Results are
shown in Figure 2a.
Batch sizes had a negligible impact on total throughput for the tested number of parallel
prompts (see Supplementary Information).
However, minor diﬀerences were observed across
model versions.
In general, higher bit precision models showed slightly reduced throughput,
particularly at greater numbers of simultaneous prompts. However, the iq1 s version showed a
consistent slower throughput than all other tested variants, deviating from the otherwise consis-
tent trend.
4


---

Overall, throughput performance was satisfactory for practical use. Single queries achieved
approximately 25 tokens per second (t/s) across most versions, while maintaining reasonable
rates under parallel prompt loads. For 8 simultaneous queries, each query received 10-12 t/s
(calculated by dividing total throughput by the number of parallel queries).
Another critical factor in model deployment is context size. A large context window is essen-
tial for certain use cases, such as coding, debugging, and other complex problem-solving tasks,
to ensure the model can process as much relevant information as possible. However, due to com-
putational restraints, larger models reduce the available VRAM for storing conversation context.
Additionally, the maximum number of concurrent users must be speciﬁed when deploying the
model; as the number of users increases, the available context is divided equally among them,
further limiting the maximum context per user. To determine the practical limits of our compu-
tational setup, we incrementally increased the context size in steps of 128 tokens until reaching
an out-of-memory error. The results are shown in Figure 2b.
The smallest model variant, iq1 s, enables a maximum context of approximately 20,000
tokens, while the largest tested variant, q2 k xl, accommodates over 10,000 tokens.
These
values are suﬃcient for most applications. As a reference, for this model, 1 token corresponds
to approximately 3.3 English characters on average. When scaling to more users, the maximum
context is divided proportionally, so institutions and teams should carefully evaluate the trade-
oﬀs based on their speciﬁc needs.
iq1 s
iq1 m
iq2 xxs
q2 k xl
0
5k
10k
15k
20k
Max. context length (tokens)
1
2
4
8
Number of parallel prompts
0
20
40
60
80
100
Text generation throughput (t/s)
iq1 s
iq1 m
iq2 xxs
q2 k xl
a)
b)
Figure 2: (a) Text generation throughput in tokens per second (t/s) for 1, 2, 4 and 8 parallel
prompts across four quantized model variants: iq1 s, iq1 m, iq2 xxs, and q2 k xl. (b) Maxi-
mum context length supported by our computational setup for each model before running out
of memory. Both experiments were conducted on a single node with a batch size of 512.
3
Discussion
This work demonstrates the feasibility of deploying state-of-the-art, open-source LLMs on high-
performance computing (HPC) infrastructure with small to moderate resource requirements. By
leveraging dynamic quantization techniques, we successfully deployed a 685B-parameter model
(DeepSeek V3-0324) on just 4 computational nodes of the Santos Dumont supercomputer, pro-
viding an eﬃcient and scalable solution to LNCC’s academic community. Our approach ensures
5


---

data sovereignty, allowing researchers and students to utilize advanced AI tools without compro-
mising sensitive information to third-party providers.
The results highlight a practical balance between model performance, computational eﬃ-
ciency, and accessibility, making this solution adaptable for institutions with limited resources.
Users are encouraged to experiment with diﬀerent quantization constraints and alternative
lightweight models to optimize performance for lower-end hardware, tailoring the system to
their speciﬁc computational and accuracy requirements. By open-sourcing this implementation,
we aim to encourage broader adoption of locally hosted AI systems, fostering scientiﬁc autonomy
while mitigating privacy risks. This initiative strengthens Brazil’s computational capabilities and
establishes a model for democratizing access to AI in academic and research environments, with
guaranteed privacy for data, source code, intellectual property, and sensitive information. With
such assurance in place, it is reasonable to anticipate a signiﬁcant increase in the adoption of
these models within academia, leading to substantial gains in scientiﬁc productivity, beneﬁts
that would fully justify the investments made in the computational infrastructure required to
support large-scale model inference. In this context, discussions are currently underway with
the Brazilian Ministry of Science, Technology and Innovation to expand this strategic solution
to additional research centers and universities across the country.
4
Methods
This study utilizes dynamically quantized versions of the DeepSeek-V3-0324 language model,
publicly available in a third-party repository2 created by Unsloth AI, an open-source initiative
designed to accelerate and simplify the ﬁne-tuning of LLMs [13]. The models are provided in
GGUF,3 a ﬁle format devised for eﬃcient deployment with support for quantization. Quantiza-
tion is the process of reducing the numerical precision of a model’s parameters, i.e. converting
them from higher-precision formats (e.g. 16-bit ﬂoating-point numbers) into lower-precision for-
mats (such as 4-bit integers), to reduce the model’s size, speed up computations, and lower
energy consumption, while aiming to maintain the same level of accuracy [14, 15].
The dynamic quantization approach, in particular, enhances model accuracy by selectively
applying lower-bit quantization to speciﬁc components of models, rather than uniformly to all
layers [16, 17]. This method targets areas like Mixture-of-Experts (MoE) layers for aggressive
quantization while preserving higher precision in sensitive parts such as attention mechanisms
and certain linear projections. By doing so, it preserves model accuracy and prevents issues like
incoherent outputs or inﬁnite loops that can result from indiscriminate quantization.
The DeepSeek V3-0324 model with 685B trainable parameters needs around 700 GB of VRAM
in full 8-bit precision, but it is also available in multiple quantized formats.
These formats
oﬀer varying levels of compression depending on the number of bits used. The most aggressive
quantization, at 1.71 bits, reduces the model size to 186GB—just 26.6% of the original size. As
the bit precision increases, the model size increases, but so does the likelihood of retaining higher
accuracy. The least quantized version available is the 4.5-bit model at 405GB, which represents
approximately 57.9% of the original memory footprint. Table 1 summarizes all the available
conﬁgurations. The largest quantized model that a single computational node (with four NVidia
H100 GPUs) in the Santos Dumont supercomputer can accommodate is the 2.71-bit version at
248 GB.
For local model execution and inference, we employed the llama.cpp library, an open-source
optimized implementation for running LLM inference on a series of CPU and GPU architec-
2https://huggingface.co/unsloth/DeepSeek-V3-0324-GGUF
3https://github.com/ggml-org/ggml/blob/master/docs/gguf.md
6


---

Table 1: Quantized conﬁgurations of DeepSeek V3-0324 and corresponding approximate size
reductions.
Type
Quantization (bits)
Model Size (GB)
Reduction in size (%)
IQ1 S
1.78
186
73.4
IQ1 M
1.93
196
72.0
IQ2 XXS
2.42
219
68.7
Q2 K XL
2.71
248
64.6
Q3 K XL
3.5
321
54.1
Q4 K XL
4.5
405
42.1
tures [18]. This library supports quantized models, CUDA integration, and layer-wise paralleliza-
tion across multiple GPUs. Additionally, it oﬀers a web-based graphical interface, facilitating
user interaction through a browser. The adapted interface’s source code is included in the project
repository and can be modiﬁed as needed.
The system architecture is designed for deployment in a multi-node cluster environment, with
each node running an independent instance of the model. Given that the quantized models ﬁt en-
tirely within a single node’s VRAM, horizontal scaling is achieved by replicating the environment
across nodes, eliminating the need for model partitioning or inter-GPU state synchronization.
To manage request distribution among nodes, we implemented a routing component using
the Ray Serve framework [19]. This component acts as a central entry point, balancing incom-
ing requests across available nodes using a round-robin strategy, which sequentially distributes
requests to ensure even utilization of resources.
This modular architecture separates the routing logic from the inference at computational
nodes, allowing for isolated management of authentication, context length constraints, and ses-
sion handling. Communication between the router and inference nodes is conducted via a two-tier
secure tunneling setup, ensuring compliance with internal security policies.
Experiments were performed on the Santos Dumont supercomputer, running a Linux distri-
bution. Job submissions are managed using the Simple Linux Utility for Resource Management
(Slurm). Each node in the cluster is equipped with 4x NVIDIA H100 GPUs, totalling 360 GB of
VRAM per node. The nodes are conﬁgured identically so that each can independently execute
an entire instance of the quantized model.
5
Code Availability
All scripts, conﬁguration ﬁles, and deployment routines described in this work are openly avail-
able at the following repository: https://github.com/gmmsb-lncc/carcara-sdumont and archived
using Zenodo [20].
6
Funding
This work was supported by the Brazilian agencies Conselho Nacional de Desenvolvimento
Cient´ıﬁco e Tecnol´ogico (CNPq) (grant number 309744/2022-9); and the Funda¸c˜ao Carlos Chagas
Filho de Apoio `a Ciˆencia (FAPERJ) (grant numbers E-26/010.001415/2019, E-26/211.357/2021,
E-26/200.393/2023).
7


---

References
[1] Tyna Eloundou, Sam Manning, Pamela Mishkin, and Daniel Rock. GPTs are GPTs: Labor
market impact potential of LLMs. Science, 384(6702):1306–1308, June 2024.
[2] Mauro Cazzaniga, Florence Jaumotte, Longji Li, Giovanni Melina, Augustus J. Panton,
Carlo Pizzinelli, Emma J. Rockall, and Marina Mendes Tavares. Gen-ai: Artiﬁcial intelli-
gence and the future of work. IMF StaﬀDiscussion Notes, 2024. Accessed April 8, 2025.
[3] OECD. Artiﬁcial intelligence in science: Challenges, opportunities and the future of research.
Report, OECD Publishing, Paris, 2023.
[4] Pablo Picazo-Sanchez and Lara Ortiz-Martin. Analysing the impact of ChatGPT in research.
Applied Intelligence, 54(5):4172–4188, March 2024.
[5] Abeba Birhane, Atoosa Kasirzadeh, David Leslie, and Sandra Wachter. Science in the age
of large language models. Nature Reviews Physics, 5(5):277–280, May 2023.
[6] Richard Van Noorden and Jeﬀrey M. Perkel. AI and science: What 1,600 researchers think.
Nature, 621(7980):672–675, September 2023.
[7] Thomas Dohmke, Marco Iansiti, and Greg Richards. Sea change in software development:
Economic and productivity analysis of the ai-powered developer lifecycle. arXiv preprint
arXiv:2306.15033, 2023.
[8] Sida Peng, Eirini Kalliamvakou, Peter Cihon, and Mert Demirer. The Impact of AI on
Developer Productivity: Evidence from GitHub Copilot, February 2023.
[9] Meta AI.
The Llama 4 herd: The beginning of a new era of natively multimodal AI
innovation. https://ai.meta.com/blog/llama-4-multimodal-intelligence/.
[10] DeepSeek-AI, Aixin Liu, Bei Feng, Bing Xue, Bingxuan Wang, Bochao Wu, Chengda Lu,
and Chenggang et. al. Zhao. DeepSeek-V3 Technical Report, February 2025.
[11] Kevin Lee, Adi Gangidi, and Mathew Oldham. Building meta’s genai infrastructure, March
2024. Accessed: 2025-04-08.
[12] Yannis Bendi-Ouis, Dan Dutarte, and Xavier Hinaut. Deploying Open-Source Large Lan-
guage Models: A performance Analysis, September 2024.
[13] Daniel Han and Michael Han. Unsloth: Fast, memory-eﬃcient ﬁne-tuning of llms. https:
//github.com/unslothai/unsloth, 2025. Accessed: 2025-04-15.
[14] Benoit Jacob, Skirmantas Kligys, Bo Chen, Menglong Zhu, Matthew Tang, Andrew Howard,
Hartwig Adam, and Dmitry Kalenichenko. Quantization and training of neural networks
for eﬃcient integer-arithmetic-only inference.
In Proceedings of the IEEE conference on
computer vision and pattern recognition, pages 2704–2713, 2018.
[15] Abhinav Dutta, Sanjeev Krishnan, Nipun Kwatra, and Ramachandran Ramjee. Accuracy
is not all you need. arXiv preprint arXiv:2407.09141, 2024.
[16] Mengxia Yu, De Wang, Qi Shan, and Alvin Wan. The super weight in large language models.
arXiv preprint arXiv:2411.07191, 2024.
8


---

[17] Shuming Ma, Hongyu Wang, Lingxiao Ma, Lei Wang, Wenhui Wang, Shaohan Huang,
Lifeng Dong, Ruiping Wang, Jilong Xue, and Furu Wei. The era of 1-bit llms: All large
language models are in 1.58 bits. arXiv preprint arXiv:2402.17764, 1, 2024.
[18] Georgi Gerganov. llama.cpp: Llm inference in c/c++. https://github.com/ggml-org/
llama.cpp, 2025. Accessed: 2025-04-15.
[19] Ray Team. Ray serve: Scalable and programmable serving. https://docs.ray.io/en/
latest/serve/index.html, 2025. Accessed: 2025-04-15.
[20] Leon Sulﬁerry Corrˆea Costa and Matheus M¨uller Pereira da Silva. gmmsb-lncc/carcara-
sdumont: v1.0.0. Zenodo, May 2025. Version v1.0.0; DOI: https://doi.org/10.5281/
zenodo.15390358.
9


---

Supporting Information
For the article: Eﬃcient Deployment of a 685B-Parameter Open-Source LLM on
Brazil’s Santos Dumont Supercomputer
Impact of Batch Size on Text Generation Throughput
1
2
4
8
Number of parallel prompts
0
20
40
60
80
100
Text generation throughput (t/s)
iq1 s
iq1 m
iq2 xxs
q2 k xl
Figure S1: Text generation throughput (t/s) for a batch size of 128.
1
2
4
8
Number of parallel prompts
0
20
40
60
80
100
Text generation throughput (t/s)
iq1 s
iq1 m
iq2 xxs
q2 k xl
Figure S2: Text generation throughput (t/s) for a batch size of 256.
1


---

1
2
4
8
Number of parallel prompts
0
20
40
60
80
100
Text generation throughput (t/s)
iq1 s
iq1 m
iq2 xxs
q2 k xl
Figure S3: Text generation throughput (t/s) for a batch size of 1024.
1
2
4
8
Number of parallel prompts
0
20
40
60
80
100
Text generation throughput (t/s)
iq1 s
iq1 m
iq2 xxs
q2 k xl
Figure S4: Text generation throughput (t/s) for a batch size of 2048.
2
