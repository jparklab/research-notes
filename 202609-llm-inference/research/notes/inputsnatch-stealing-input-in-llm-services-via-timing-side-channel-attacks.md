---
title: 'InputSnatch: Stealing Input in LLM Services via Timing Side-Channel Attacks'
id: inputsnatch-stealing-input-in-llm-services-via-timing-side-channel-attacks
tags:
- kv-cache-routing
- llmd-kv-cache-routing-c665d5
- kv-cache-security
- timing-side-channel
- prompt-stealing
- semantic-cache
created: '2026-09-14T11:57:55.742196Z'
updated: '2026-09-15T04:55:24.100950Z'
source: https://arxiv.org/abs/2411.18191
source_domain: arxiv.org
fetched_at: '2026-09-14T11:57:55.741926Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'Primary source for the InputSnatch attack cited by KVGov. Proposes a timing-based
  side-channel attack against two cache mechanisms (prefix/KV caching and semantic
  caching) using two components: an input constructor (ML models + LLM-based vocabulary-correlation
  learning with optimized search) that generates candidate inputs likely to hit the
  victim''s cached state, and a time analyzer that applies statistical time fitting
  with outlier elimination to distinguish cache hits from misses and feeds results
  back to refine the constructor''s search. Against prefix/KV caching, the timing
  analyzer achieves 87.13% accuracy in determining cache-hit prefix length. Against
  static-template medical question-answering systems, the attack achieves a 62% success
  rate extracting exact disease-name inputs and 13.5% for exact symptom descriptions;
  against legal-consultation RAG services using semantic caching, semantic-level extraction
  success rates range from 43% to 100% across thirteen legal domains. Overall the
  framework reports 62% success in exact partial input recovery, 12.5% in exact complete
  input extraction, and 79.5% effectiveness in semantic-level content reconstruction
  under realistic deployment constraints. Distinguishes itself from prior prompt-theft
  work (adversarial prompting, embedding inversion, next-token-probability exploitation)
  by requiring neither white-box model access nor unlimited querying across the full
  vocabulary, and by targeting structured/templated enterprise prompts where the unknown
  content is a bounded field rather than open vocabulary.'
raw_file: raw/inputsnatch-stealing-input-in-llm-services-via-timing-side-channel-attacks.pdf
doi: arXiv:2411.18191
citation_count: 32
venue: arXiv.org
is_retracted: false
---

*Suggested by [[governing-the-kv-cache]] — cited as one of three timing side-channel attacks KVGov defends against*

InputSnatch: Stealing Input in LLM Services via Timing Side-Channel Attacks
Xinyao Zheng1,2,3, Husheng Han1,2,3, Shangyi Shi1,2,3, Qiyan Fang1,3,6, Zidong Du1,5, Xing Hu1,4*, Qi Guo1
1SKLP, Institute of Computing Technology, Chinese Academy of Sciences
2University of Chinese Academy of Sciences
3Cambricon Technologies
4ZGC LAB
5Shanghai Innovation Center for Processor Technologies, SHIC
6University of Science and Technology of China
{zhengxinyao22s, hanhusheng20z, shishangyi22s}@ict.ac.cn
{qiyanfang}@mail.ustc.edu.cn
{duzidong, guoqi, huxing}@ict.ac.cn
Abstract—Large language models (LLMs) possess extensive
knowledge and question-answering capabilities, having been
widely deployed in privacy-sensitive domains like finance and
medical consultation. During LLM inferences, cache-sharing
methods are commonly employed to enhance efficiency by
reusing cached states or responses for the same or similar
inference requests. However, we identify that these cache
mechanisms pose a risk of private input leakage, as the caching
can result in observable variations in response times, making
them a strong candidate for a timing-based attack hint.
In this study, we propose a novel timing-based side-channel
attack to execute input theft in LLMs inference. The cache-
based attack faces the challenge of constructing candidate
inputs in a large search space to hit and steal cached user
queries. To address these challenges, we propose two primary
components. The input constructor employs machine learning
techniques and LLM-based approaches for vocabulary correla-
tion learning while implementing optimized search mechanisms
for generalized input construction. The time analyzer imple-
ments statistical time fitting with outlier elimination to identify
cache hit patterns, continuously providing feedback to refine
the constructor’s search strategy. We conduct experiments
across two cache mechanisms and the results demonstrate
that our approach consistently attains high attack success
rates in various applications. Our work highlights the secu-
rity vulnerabilities associated with performance optimizations,
underscoring the necessity of prioritizing privacy and security
alongside enhancements in LLM inference.
1. Introduction
Since the release of ChatGPT [86], large language
models (LLMs) have garnered widespread attention due to
their exceptional reasoning and natural language genera-
tion capabilities, leading to their extensive application in
privacy-sensitive areas such as medical [117], finance [131],
law [19], and office assistance [107]. These models demon-
strate remarkable proficiency in understanding complex
queries, generating contextually appropriate responses, and
providing domain-specific insights across various fields.
Users increasingly rely on these intelligent agents for
privacy-critical scenarios like medical advice, financial plan-
ning, and legal consultation tasks. This growing dependence
on LLM-based systems for processing sensitive personal
information raises significant privacy and security concerns.
LLMs’ privacy and security are rapidly gaining attention
as these models become increasingly integrated into com-
mercial applications. Building upon traditional deep learning
privacy concerns, the enhanced memorization capabilities of
LLMs exacerbate existing privacy risks such as member-
ship inference attacks (MIAs) [13], [71], [72], [106] and
personally identifiable information (PII) leakage [55], [66],
[108]. These risks scale proportionally with model size, as
larger models demonstrate stronger memorization of training
data [13].
Beyond these traditional concerns, LLMs introduce a
novel privacy challenge: prompt theft attacks, which threaten
intellectual property rights and personal privacy. These
prompt theft attacks have emerged in various forms, in-
cluding extracting system prompts by adversarial prompt-
ing [63], [91], [141], [144], inverting prompts from em-
bedding vectors [16], [16], [59], [79], [105] or model
responses [67], [97], [101], [138], and recovering in-
put prompts by exploiting next-token probability distribu-
tions [78] or token-length sequences [129]. These attacks
pose significant risks to the growing prompt marketplace and
user privacy in downstream LLM applications, particularly
when prompts contain sensitive information or proprietary
instructions.
Although previous works have demonstrated the prompt
theft attacks in LLM-based systems, they have several
limitations: 1) Inability to recover exact privacy: current
approaches rely on either response feature analysis [68],
[101], [138] or embedding vectors [16], [105], can only
achieve partial semantic recovery. This is attributed to the
complexity of establishing precise inverse mappings in high-
dimensional spaces, resulting in the loss of crucial infor-
mation. 2) Limited attack scenarios: existing attacks are
designed for specific applications based on task-specific
arXiv:2411.18191v2  [cs.CR]  29 Nov 2024


---

methodologies [68], [101], [138]. While effective in their
targeted domains, these specialized approaches can not
transfer to other LLM-based applications, leading to limited
attack scenarios. 3) Impractical assumptions: current attacks
often rely on unrealistic assumptions, such as white-box
access to models [105] or unlimited querying capabilities to
obtain next-token probability distributions across the entire
vocabulary [78].
In this work, we present InputSnatch, which addresses
the limitations of existing attacks by exploiting timing-based
side channels arising from the commonly used cache-sharing
optimization (prefix caching and semantic caching). Prefix
caching [52], [56], [56], [139] enables the reuse of cached
attention states in two different requests with the same
prefix prompts, having been widely deployed in the industry
environment. Semantic caching [7], [37], [61], [76] allows
requests with identical content or similar semantics to share
cached responses, especially in LLM applications equipped
with Retrieval-Augmented Generation(RAG). Our primary
observation is that different requests on the same computing
nodes share the same cache, which results in an observable
time reduction when meeting sharing criteria. Figure 1
illustrates a pronounced time difference between hits and
misses, despite fluctuations caused by schedule issues and
network variability. The observable time differences provide
attack hints for attackers to obtain cache states and infer
secret inputs of other users, potentially disclosing personal
privacy information, trade secrets, or other enterprise data.
Noted that, our proposed cache-based attack vector is pow-
erful because (1) it enables exact input reconstruction due
to the strict matching requirements in prefix caching, (2)
it generalizes well for its wide implementation in diverse
mainstream LLM inference backends [49], [49], [82] and
API providers [3], [4], [40], [54], [89], (3) it is practical
since no privileged access beyond standard API capabilities
is required.
(a) Prefix caching            
(b) Semantic caching          
Figure 1: The prefill time difference between cache hits
and misses for varying input lengths with OpenAI API
calls GPT-4o-mini LLM. (a) Prefix caching implemented
by OpenAI. (b) Semantic caching with GPTCache.
The cache-based attack requires input construction for
the desired cache hit. For this purpose, our attack framework
comprises two components: a Input Constructor, which gen-
erates inputs attempting to hit cached content, and the Time
Analyzer, which determines whether a match has occurred
based on the measured time. However, fully recovering
prompts present several significant challenges. First, the
complexity of input construction escalates exponentially as
the vocabulary size reaches 1000,000 tokens [115] and the
context window scales to 128K [84], substantially increas-
ing the search space. Second, the observed times are influ-
enced by noise coming from network latency and memory
scheduling delays, complicating accurately determining the
cache state. Moreover, practical deployment constraints such
as available memory, rate limit, and cache’s Time To Live
(TTL), pose additional challenges to our attack.
To address these challenges, we introduce a compre-
hensive timing analysis framework. First, the framework
establishes temporal patterns of target services by strate-
gic sampling method and mitigates noise interference with
proposed point processing algorithms. Second, to mitigate
the large search space issue, our framework leverages ad-
vanced machine learning techniques to extract contextual
information and semantic relationships from open-source
datasets, thereby enhancing construction efficiency. Besides,
we incorporate a multi-stage candidate evaluation process to
prioritize candidates exhibiting higher cache hit probabili-
ties. In the process, the constructed texts undergo adaptive
filtering mechanisms and probabilistic ranking algorithms
to optimize the selection of high-potential candidates while
maintaining computational efficiency.
Experimental evaluations across diverse deployment sce-
narios demonstrate the effectiveness of our attack frame-
work. For requests of specified lengths, our timing analyzer
achieves 87.13% accuracy in determining cache hit prefix
lengths. In medical question-answering systems with static
prompt engineering, our attacks achieve a 62% success
rate in extracting exact disease inputs and 13.5% for pre-
cise symptom descriptions. For legal consultation services
implementing RAG, the semantic extraction success rates
range from 43% to 100%. These results highlight significant
privacy vulnerabilities across deployment contexts, empha-
sizing the critical balance between performance optimization
and security in LLM services.
Contributions. The main contributions of this work are:
•
First systematic investigation of time-based side
channels in LLM inference caused by cache-
sharing optimization, analyzing privacy leakage risks
of two cache mechanisms and examining their
performance-privacy trade-offs.
•
Implementation of a comprehensive attack method-
ology combining diverse input construction strate-
gies (ML models, LLM-based analysis, and opti-
mized search) with robust timing analysis (statistical
fitting and anomaly detection), demonstrating effec-
tive input reconstruction across various deployment
scenarios.
•
Development of a robust attack framework that
achieves 62% success rate in exact partial input
recovery and 12.5% in exact complete input ex-
traction, while demonstrating 79.5% effectiveness
in semantic-level content reconstruction under real-
world deployment constraints.


---

2. Background
2.1. Transformer-Based LLM Inference
Transformer-based large language models (LLMs) such
as GPT-3 [11] and PaLM [17] have revolutionized natural
language processing by leveraging the transformer architec-
ture [121]. A core component of these models is the self-
attention mechanism, which enables the capture of long-
range dependencies and complex contextual relationships
within input sequences.
The self-attention mechanism mainly contains the pro-
jection and attention operations. Given a token sequence
X = [x1, . . . , xn], each token xi is transformed into query
(qi), key (ki), and value (vi) vectors through learned linear
projections as qi = Wqxi,
ki = Wkxi,
vi = Wvxi,
where Wq, Wk, and Wv are trainable weight matrices.
Then, the attention operations with Q, K, and V are pro-
cessed as shown in Euqation 1. After this, a linear layer
performs re-projection to generate the input for the next
layer. The computational complexity of self-attention in-
creases quadratically with the length of the tokens.
Attention(Q, K, V) = softmax
QK⊤
√dk

V
(1)
LLMs generate text autoregressively, producing one to-
ken at a time conditioned on the initial prompt and the
preceding generated tokens. The generation process typi-
cally involves two phases: prefill and decoding. The prefill
phase computes input prompts intermediate states (keys
and values) for input prompts and generates the first next
token. The decoding phase generates output tokens autore-
gressively one by one until a stopping token is met. Due
to data dependencies, the autoregressive generation cannot
be parallelized, resulting in the underutilization of GPU
resources and memory constraints, contributing significantly
to the latency of individual requests. It’s imperative to
enhance metrics like Time To First Token (TTFT) and Time
Per Output Token (TPOT), as latency can be calculated as
Latency = TTFT +(TPOT ∗N), where N represents the
number of generated tokens.
2.2. Cache Optimization in LLM Inference
LLM inference services deployed on the cloud resources
require managing a high volume of real-time requests while
ensuring high throughput and low latency. The concurrent
requests are scheduled across different computing nodes
and computed in optimized batching [20], [83] to increase
throughput. Modern LLMs utilize the Key-Value (KV)
Cache mechanism [92] to optimize TPOT by storing each
token’s key and value vectors after their initial computation.
As presented in Figure 2, when generating token vector
on+k+1, the attention states of previous tokens can be
cached to avoid recomputation. However, as the sequence
length and batch size increase, the KV cache consumes more
GPU memory. Many inference optimizations technologies
TABLE 1: Cache Mechanisms Comparison of Different
LLM API Vendors
Vendor
Stream
Caching Mechanisms
Cache Lifetime
OpenAI [89]
Y
Prefix Caching
5-10 minutes
DeepSeek [23]
Y
Prefix Caching
Hours to days
Anthropic Claude [3]
Y
Prefix Caching
5 minutes
Google Gemini [40]
Y
Prefix Caching
Default 1 hour
MoonShot Kimi [77]
Y
Prefix Caching
Customization
Portkey [93]
Y
Semantic Caching
Default 7 days
Google Cloud [98]
Y
Semantic Caching
Uncertain
Microsoft Azure [4]
Y
Semantic Caching
Uncertain
UnKey [32]
Y
Semantic Caching
Uncertain
Amazon [54]
Y
Semantic Caching
Uncertain
have been proposed to optimize KV Cache, including spar-
sity [28], [143], [145], quantization [24], [34], [65], [99],
[142], windowing [43], [134], and sharing [52], [56], [139],
[148]. Leading LLM API providers extensively implement
cache-sharing mechanisms (detailed in Table 1), which can
be classified into prefix and semantic caching approaches,
to optimize TTFT and memory utilization through cross-
request content reuse.
k 1
k 2
...
kn+k
×
Q
KT
×
v1
v2
...
vn+k
V
＝
k 1
k 2
...
kn+k
×
Q
KT
×
v1
v2
...
vn+k
V
＝
Values that will be taken from cache
Without Cache
With Cache
qn+k
Values that will be computed
＝
＝
qn+k
on+k+1
qn+kk1
qn+kk2
...
qn+kkn+k
on+k+1
qn+kk1
qn+kk2
...
qn+kkn+k
Longer Time
Shorter Time
Figure 2: Comparison of self-attention computation mecha-
nisms. The traditional approach (upper) performs full recom-
putation for each token, while the KV cache (lower) reuses
stored key-value vectors to accelerate inference. The KV
Cache reduces the computational complexity per decoding
step from O(n2) to O(n).
Prefix Caching. Prefix caching [52], [56], [139], [148]
reuses the KV cache with identical prefixes across different
requests or within multiple sequences generated from a
single request. For instance, vLLM [56] introduces non-
contiguous memory allocation and efficient prefix sharing
across different sequences. It is important to note that
only consistent content from the beginning qualifies as a
prefix; the same segments starting mid-sequence cannot
be matched. Prefix caching has been an industry norm
integrated into mainstream inference frameworks such as
HuggingFace TGI [49], NVIDIA TensorRT-LLM [82], and
LMDeploy TurboMind [51]. Other notable implementations
include SGLang [148], which employs a radix tree to reuse
the KV cache in various scenarios, ChunkAttention [139],
which introduces a prefix-aware KV cache partitioning
mechanism, and Hydragen [52], which efficiently manages
shared prefixes.
Semantic Caching. Semantic caching mechanism [7], [37],


---

[61], [76] enables consistent outputs for identical or se-
mantically similar queries. This approach optimizes server
resource utilization, minimizes data retrieval latency, reduces
API costs, and enhances system scalability. By leveraging
embedding similarity metrics [149], [150], responses can
be reused for semantically equivalent queries. A prominent
open-source implementation, GPTCache [7], demonstrates
this concept by caching request-response pairs. Upon cache
hits, the system directly retrieves stored responses, reducing
redundant LLM invocations and response latency. Recent
research efforts [61], [76] have focused on enhancing se-
mantic caching optimization strategies to improve cache hit
rates and minimize operational costs.
However, these caching mechanisms are potential attack
targets, as different users’ requests can share the same
caching infrastructure. Our focus is on streaming responses
supported by the APIs, where generated tokens are sent
back sequentially, thus enabling users to receive real-time
feedback and measure per-token generation times precisely.
We have designed attack strategies targeting two scenar-
ios, aiming to extract information about input prompts by
measuring timing side channels associated with the cache
mechanisms. Specifically, we focus on two application sce-
narios: static prompt engineering using prefix caching and
RAG employing semantic caching. By timing side-channel
attacks, we can infer partial or complete information about
user inputs, posing a security threat as unauthorized access
to sensitive input information.
2.3. Targeted Attack Scenarios
Many applications leverage LLMs’ advanced reasoning
and natural processing capabilities by customizing models
for specific tasks. They often combine technologies such
as prompt engineering or Retrieval-Augmented Generation
(RAG) at a high level, offering advanced functionality and
exceptional adaptability across various industries, including
healthcare [95], [103], [104], [120], [137], legal consult-
ing [19], [102], [110], [132], e-commerce [33], [42], [136],
[151], and education [12], [64], [73], [130].
Prompt Engineering. Prompt engineering has become in-
dispensable for extending the model’s capabilities through
task-specific instructions, allowing integration into down-
stream tasks
[94]. These techniques include few-shot
prompting [11], [35], [41], where examples guide the
model’s responses for new tasks without extensive train-
ing; chain-of-thought prompting [60], [126], [146], [147],
which encourages step-by-step reasoning. Recent studies
have shown that carefully crafted prompts can significantly
improve model performance across various tasks, from ques-
tion answering [100], [104], [113], [123] to code genera-
tion [15], [26]. For example, ChatGPT o1 [88] leverages
reinforcement learning to refine its CoT strategies, allowing
it to identify and correct errors effectively.
Our attack strategy focuses on applications that use static
prompt engineering, like GPT store [87], where users can
design system prompts for specific tasks to define their apps.
User input is usually embedded or concatenated into the
system prompt, which can be reused with prefix caching.
It’s possible to recover partial or complete input accurately
due to KVCache segmented management and precise prefix
matching deployed by mainstream inference frameworks (as
detailed in section 4.2).
RAG Databases
User
Embedding 
Model
Similarity>
Threshold?
Semantic Cache
request
Matched Files
LLM 
Service
File
Embeddings
Vector Search
response
request
<hash>: 
<requests>
Longer Time
Shorter Time
Requests
Cache Index
Vector Search
Y
N
Store
Cache Hit Flow
Cache Miss Flow
Time-consuming Process
<hash>: 
<responses>
response
response
Figure 3: Overview of the RAG-assisted LLM system with
the semantic caching mechanism. User queries are first
matched against cached requests based on semantic simi-
larity. Responses are retrieved directly from the cache if the
similarity score exceeds the threshold; otherwise, the system
proceeds with vector database retrieval and LLM inference.
Retrieval-Augmented Generation (RAG). RAG [58] en-
hances LLMs by retrieving relevant external knowledge to
address the inherent limitations of LLMs, including knowl-
edge constraints and hallucination issues. This approach
has demonstrated significant value across various domains,
including improving accuracy in medical diagnostics [133]
and legal analysis [53] and integrating external information
in open-domain questions [81], [124]. However, each inter-
action requiring data retrieval can be computationally inten-
sive and time-consuming, particularly with large datasets.
Developers exploit semantic caching to solve latency and
cost challenges. As illustrated in Figure 3, incoming user
requests are translated into vectors by embedding models
and compared against request embedding vectors through
semantic similarity computation. If the similarity score ex-
ceeds the predefined threshold, the system directly retrieves
the corresponding response from the semantic cache, by-
passing the time-consuming retrieval and inference stages.
When no semantically similar cached queries are found,
the system follows the traditional pipeline: retrieve relevant
passages from the vector database, which are combined with
the original query vectors and fed into LLMs for response
generation.
This semantic caching mechanism significantly reduces
response latency for frequently asked questions. The distinct
temporal patterns between cache hits and misses create an
observable timing differential, as caching hits mitigate costly
retrieval and generation operations latency. This discrepancy
in response times potentially reveals sensitive information
about the existence and nature of previously cached re-
quests.


---

3. Attack Hints and Threat Model
3.1. Attack Hints
LLM inference systems leverage caching mechanisms
to optimize computation, while the cache-sharing strategy
inadvertently introduces vulnerabilities for time-based side-
channel attacks. As demonstrated in Figure 1, despite signifi-
cant timing variations due to network latency and scheduling
delays, there exists evident time differences in real-world
API deployments. In this section, we analyze the timing
characteristics caused by two cache mechanisms at different
phases (prefill and decode phase) to determine the specific
attack vector and attack timing. We adopted two promi-
nent open-source implementations: vLLM [56] and GPT-
Cache [7] as our experiment platforms, which were chosen
based on their widespread adoption, active development, and
robust technical implementations.
Our empirical analysis revealed distinct temporal dis-
parities between prefix cache hits and misses in the prefill
and decode phases. Based on this observation, we selected
the prefill time as an effective attack vector as it enables
early termination of the inference process after the first token
generation, significantly reducing the overall attack duration.
As illustrated in Figure 4, our experiments reveal distinct
timing patterns between cache hits and misses with no
overlapping or ambiguous cases observed. Figure 4(a) shows
that the timing difference in the prefill phase positively
correlates with the length of cached requests. Figure 4(b)
demonstrates that the timing difference in the decode phase
primarily stems from the time saved by reusing input token
computations.
(a)Prefill phase          
(b) Decode phase        
Figure 4: Time difference between hits and misses for prefix
caching: 100 experiments in vLLM by a local API deploy-
ment using the LLaMa-2 70B model. (a)Time for input with
varying lengths taken to generate one token. (b)Time for
input with the same length to generate different numbers of
tokens.
Similarly, we identify distinguishable temporal patterns
in semantic caching operations and select prefill time as
the attack vector. When cache hits occur, the responses are
retrieved from the cache, eliminating the need for token-
by-token generation. This results in negligible prefill time
and overall latency, as demonstrated in Figure 5. For cache
misses, the response latency varies dramatically for different
generated tokens, while the prefill time fluctuates slightly
due to network latency.
(a)Prefill time
(b) Response Latency   
Figure 5: Time difference between hits and misses for
semantic caching: conducted in GPTCache by invoking API
to access GPT-4o-mini. (a)Time for different inputs with
varying lengths to generate one token. (b)Time for input
with varying lengths to generate complete responses.
Given the clear time difference in both prefix and se-
mantic caching mechanisms, we selected prefill time as
our attack vector, allowing us to minimize the total attack
duration. We conducted further experiments across various
configurations as Figure 6, with each data point representing
the average of 30 runs on vLLM. While our results indicate
that TTFT correlates with model size, hardware specifica-
tions, tensor parallelism scale, and sampling parameters, a
significant timing differential between hit and miss persists
across all experimental configurations. This insight enables
us to construct a robust time analyzer that can effectively
characterize and differentiate caching behaviors, regardless
of the underlying model architecture, hardware configura-
tions, or sampling parameters in inference deployments.
3.2. Threat Model
Attack Scenario. Our attack scenario is presented as Fig-
ure 7, the application was customized by developers to
invoke the LLMs inference service provided by the cloud
service vendor through the API. Ordinary users interact with
the inference service through a web browser or other appli-
cation interface, sending their privacy input and receiving
generated responses. The communication channels can be
encrypted to ensure data security.
We consider the cloud infrastructure often implements
shared caching mechanisms in each computing node, where
requests from multiple users share cached states or re-
sponses. When cache hits occur, the computing nodes can
bypass computation and directly use existing cached values
to improve throughput and reduce latency. The experimental
analysis demonstrates that systems with long shared prefixes
achieve a 70%-90% reduction in memory utilization [139]
and semantic caching can effectively handle 20-30% of user
queries [109] in production deployments.
Attacker’s Objective. The primary objective of the attacker
is to construct input to match inputs from normal users on
the same computing node. With measured response latency,
the attacker can determine the occurrence of cache hits,
indicating the successful match of another user’s input. For
context retention and reuse maximization, requests from
the same user are typically routed to the same computing
node, which enables multiple attempts for attackers. In cases


---

(a)Model Size
(b)Tensor Parallel
(c)Different GPUs
(d)Different Parameters 
Figure 6: Prefill time characteristics under different configurations. The horizontal axis represents the number of input tokens,
and the vertical axis represents the prefill time(s). (a) Three models of different parameter scales (LLaMa-3 8B [29], Qwen
14B [5], and LLaMa-2 70B [119]) deployed on eight A30 GPUs. (b) LLaMa-3 8B under varying tensor parallelism sizes
(1, 4, and 8) across different numbers of A30 GPUs. (c) LLaMa-2 70B deployed across eight different GPU architectures
(A30, A100, and L40). (d) LLaMa-2 70B deployed across eight A30 with different sampling diversity: low (temperature=0,
top k=1, top p=1), medium (temperature=0.2, top k=2, top p=0.3), and high (temperature=1.0, top k=100, top p=1.0).
"Age":** ,
"Gender":** ,
"Disease 
History":**,
"Symptoms":**,
Victim
Applications
Cache 
Search
Secret Inputs
USER
LLM SERVICES
ATTACKER
Prompt 
Constructor 
Time 
Analyzer
Prompt
Times
Feedback 
information
Hit
Miss
Reuse
Compute
API
API
Figure 7: Overview of our attack scenario. Users interact
with the LLMs cloud service through the interaction inter-
face, and different user requests are routed to share the cache
through encrypted API channels.
of cache misses, the attacker employs an iterative request
modification and resubmission strategy. To maintain attack
accuracy and prevent false positives, avoiding interactions
with previously cached content from the attacker’s requests
is crucial. Therefore, each constructed input query must
incorporate unique prefixes, ensuring distinctive cache sig-
natures across iterations. Notably, the attackers focus on
extracting private inputs from users sharing the same cache
and do not target the victim users’ identification information.
Attacker’s Capability. We consider a realistic attacker who
accesses the LLM service as an ordinary user through the
public interfaces provided by the cloud service. The attacker
does not possess special privileges and is unaware of the
training data, model parameters, or specific hardware config-
urations. The attacker’s capabilities are confined to sending
requests and measuring the response times, as permitted by
the API.
3.3. Challenges
The controlled experiments enable us to analyze cache-
related timing characteristics in an environment free from
remote API and establish a foundation for understanding
exploitable timing patterns in real-world scenarios. While
adversaries can theoretically craft input prompts to align
with cached content from other users, several significant
challenges emerge in real-world environments.
Expansive Search Space. Modern language models may in-
corporate vocabulary sizes exceeding 100,000 tokens [115],
with context windows extending to two million tokens [25].
This exponential growth in search space with increasing
input length renders accurate input reconstruction compu-
tationally intractable, presenting a fundamental challenge
to effective input construction. Moreover, the diversity of
natural language expressions and the potential variations
in user prompts further expand this search space, making
exhaustive exploration impractical even for relatively short
prompts.
Interference from Time Noise. Response time measure-
ments are subject to various environmental factors, includ-
ing network latency and memory scheduling delays. These
external influences introduce noise that obscures the true re-
sponse time, potentially compromising the accuracy of cache
status judgment. Additional factors such as load balancing,
resource contention, and system load variations can further
complicate timing measurements in distributed systems. The
challenge becomes particularly acute in cloud-based deploy-
ments where multiple layers of virtualization and shared
resources introduce additional timing uncertainties.
Real-world Constraints.
Several constraints complicate
our attack. Firstly, rate limits are commonly imposed to
prevent API abuse and ensure fair service for all users,
restricting the number of attempts we can make when inter-
acting with the API. Secondly, limited GPU memory and the
costs associated with API usage constrain the total number
of tokens an attacker can try, preventing continuous attempts
that might evict the target request from memory. Finally, as
noted by OpenAI, cached content typically remains active
during inactivity periods lasting from 5 to 10 minutes and
can persist for up to one hour during off-peak periods [89].
Consequently, the duration of the end-to-end attack is lim-
ited by the cache’s Time To Live (TTL). Additionally,
dynamic pricing models and usage quotas implemented by


---

service providers further restrict the feasibility of large-scale
attack attempts, while sophisticated rate-limiting algorithms
may detect and block suspicious patterns of API usage.
4. Attack 1: Prompt Engineering
This section introduces our input theft attacks in ap-
plications assisted by prompt engineering optimized with
the prefix caching mechanism. We will demonstrate the ef-
fectiveness of our attacks through experiments, highlighting
the privacy information leakage risks associated with LLM-
based applications.
4.1. Introduction to Attack Scenario
Prompt engineering may influence the generative per-
formance of LLMs
[70], [96]. The parameterized prompt
is a format of prompt template that enables dynamic value
substitution based on user input [38], which has been advo-
cated in applications such as GPTforWork [112] and various
prompt generation utilities [2], [36]. In our attacks, the
systems integrate user-provided information into predefined
positions within hidden prompts, facilitating personalized
response generation.
User interaction involves sensitive data submissions,
such as medical records, personal preferences, financial
information, and proprietary business data. We focus on
a medical consultation system inspired by the Zuoshou
doctor platform [116], where users provide sensitive input
across six fields illustrated as Figure 8. While Age and Gen-
der fields have numerical constraints, the Disease History
and Symptoms fields have unlimited amounts with user-
customizable items. The system maintains consistent item
ordering when populating the prompt template based on
research indicating the impact of input sequencing on gen-
eration quality [18]. Duration field accepts free-form input.
Chief Complaints offers ten predetermined options based on
the model’s capabilities, covering aspects such as treatment
approaches, medication management, dietary considerations,
etc. For standardization purposes, both Disease History and
Symptoms are restricted to 100 characters, with placeholder
text automatically filling any unused capacity.
4.2. Attack Methodology
Our attack framework comprises two primary compo-
nents: the time analyzer and the input constructor, as illus-
trated in Figure 9. During the offline attack phase, the input
constructor learns the relationships between input fields
from open-source datasets, and the timing analyzer obtains
the association between response time and hit ratios by the
appropriate query. In the online attack phase, the constructor
generates the inputs embedded into the system prompt for
LLM inference. The response time is then measured and fed
into the time analyzer to determine whether the current field
is hit.
Medical Services   Please enter your consultation information
Age
Gender
Disease History
Symptoms
Duration
Custom input
Chief Complaints
     Please select from the drop-down menu
     Please select from the drop-down menu
Custom input
▽
     Please select from the drop-down menu
Custom input
▽
▽
Male   ×
33       ×
(Up to 5 inputs)
(Up to 5 inputs)
Input completed, please generate a response to my situation
Figure 8: Medical consultation interface. Collecting user in-
formation through six fields, featuring structured selections
and flexible custom inputs, supporting predefined options
and free-form text entry for friendly user interaction.
Opensource 
Datasets
Input Dataset 
Learning
Victim 
Service
TTFT
Processing
Sampling
Time Fitting
Online Attack Stage
Time Analyzer
Attack Input
Inference Prompt
Measured
Time
Hit Ratio
Offline Attack Stage
Victim 
Service
Input Constructor
Figure 9: Attack overview. The input constructor leverages
field correlation learning to optimize online input genera-
tion, while the timing analyzer establishes temporal patterns
during the offline phase to facilitate online timing analysis.
The inference backend, exemplified by vLLM, employs
block-level memory management for the KV Cache. Specif-
ically, vLLM allocates and manages KV Cache memory
in fixed-size blocks, enabling efficient memory utilization
and cache management. Different LLM service providers
implement varying block sizes, OpenAI utilizes 128-token
blocks [89], while DeepSeek employs 64-token blocks [23].
The standardized block-based cache allows us to verify
cache hits on a block granularity, making it possible to
conduct inputs field by field sequentially. Once we success-
fully hit one input field, the next field can be constructed
given the context, significantly reducing the search space
and improving attack efficiency.
Time Analyzer. Through 30 experimental trials, we ana-
lyzed temporal distribution patterns, revealing distinct re-
sponse time variations corresponding to different cache
block hit counts. As illustrated in Figure 10, the blue regions


---

represent temporal distributions for current hit blocks, with
distinguishable time intervals enabling hit ratio estimation
for field-level construction. The red regions highlight tempo-
ral overlaps between different hit ratios, presenting a signif-
icant challenge as similar response times may correspond
to multiple distinct hit ratios. These overlapping patterns
predominantly occur between adjacent hit block counts and
are exacerbated by temporal noise arising from concurrent
request processing and hardware scheduling variations. This
temporal ambiguity constitutes a fundamental challenge to
our attack methodology, necessitating the development of
robust prediction strategies.
Figure 10: Relationship between TTFT and hit blocks. A
total of 30 experiments were conducted with a prompt length
of 800 tokens, where each block consists of 16 tokens.
The gray line indicates the prefill time no block hits and
the blue section represents the time range for different hit
blocks obtained from multiple experiments. The red section
indicates the overlapping time intervals for different hit
block counts, which are prone to misjudgment.
The prefill computation time exhibits a relationship con-
cerning the prompt length and hit ratio, expressed as time
is proportional to (n-k) × n, where n denotes the total token
count and k represents hit token counts. Assuming minimal
system noise, this relationship enables precise prediction
of hit ratios based on observed response times. During
the offline phase, we conduct sampling of the target ser-
vice to get this functional relationship, employing post-
processing techniques to filter anomalous measurements.
The noise is inherent and unavoidable due to system varia-
tions and concurrent request interference. Instead of making
deterministic predictions, we maintain multiple candidate
hit ratios weighted by their likelihood given the observed
time. This statistical framework proves effective in practice
since adjacent input fields are separated by more than one
block length, and field content contains multiple blocks.
These structural input characteristics help isolate potential
misclassifications, preventing error propagation from blocks
to fields and ensuring that occasional block-level prediction
errors do not significantly impact the overall attack success
rate.
Input Constructor. The block-level management mecha-
nism enable input constructor to construct input field-by-
field, as illustrated in Figure 11. When constructing the
i −th field, the preceding fields remain unchanged, and
the following fields are randomly filled. Subsequent fields
are constructed if the preceding fields hit. If the current
field fails to hit, the input constructor continues to generate
different alternative content to avoid erroneous successful
judgments caused by hitting previous attack inputs. Upon a
successful hit of the current field, the process advances to
constructing the next field.
The attacker lacks fine-grained information correspond-
ing to user inputs, and exhaustively searching the vocabulary
space for each position would result in a prohibitively
large search space. To reduce this complexity, we leverage
open-source datasets to learn potential field contents and
their probabilities. We analyze these datasets to construct
a targeted vocabulary subset for each field based on its
semantic context and typical usage patterns. This context-
aware approach substantially narrows the search space by
considering only contextually relevant tokens rather than the
entire vocabulary. Furthermore, we can reduce the search
space for subsequent fields by combining hit inputs with the
learned field correlations, making the attack computationally
feasible. For example, a specific age or gender is often
strongly correlated with certain diseases, and known disease
information can aid in constructing subsequent symptoms.
③ The hit ratio is increased. Proceed to construct the next field.
1~i-1
i(miss)
i~N
① Predict the hit ratio based on time.
1~i-1
i(miss)
i~N
1~i-1
i(miss)
i~N
1~i-1
i(hit)
i~N
...
1~i
i+1
i+1~N
②Based on the time feedback and re-predict the current field.
Figure 11: Partial prediction mechanism with block ratio
estimation. The construction of the next field can only
proceed after hitting the current field.
4.3. Experimental Setup
Experimental Setup.
We conduct experiments on vLLM
0.6.2 as the LLM inference framework, which implements
block-based memory management and prefix caching. We
simulate multi-user scenarios by invoking the OpenAI-
compatible local API through various processes. The at-
tacker continuously attempts to construct input to hit cached
content from other users within the same computing node.
We deploy LLaMA-2 70B as the service model on an
8×A40 GPU cluster (40GB memory per GPU), representing
a typical production deployment. The model is configured
with deterministic sampling parameters: temperature=0,
topk=1, and topp=1.
Datasets.
We learn domain-specific vocabulary and inter-
field correlations from open-source Chatdoctor [62] datasets,
which comprise 110K real patient-doctor conversations. We
extract age-containing dialogue samples from the patient’s
input and employ ChatGPT-4o to extract and structure infor-
mation across six distinct fields to get our Formatted Input


---

Field datasets containing 16,276 samples. To facilitate large
language model fine-tuning, we processed three customized
fields, where the input consists of successfully predicted
fields and the output corresponds to the target field for
prediction. This processing resulted in three datasets of
equal size, each designed to train the model on specific
field prediction tasks while leveraging the information from
previously identified fields. Our complexity analysis reveals
that the search space of the Formatted Input Field dataset
exceeds 2×1042, highlighting the computational challenges
involved.
To ensure comprehensive evaluation, we implement
post-processing validation mechanisms for maintaining
structural consistency while preserving semantic integrity,
specifically addressing format inconsistencies and field
omissions in large language model outputs. The evaluation
methodology incorporates 200 randomly selected samples
from the Formatted Input Field dataset to simulate real-
world user inputs, with the remaining data allocated for
constructor training. Additionally, to enhance evaluation
fairness and assess generalization capabilities, we supple-
mented our test set with 200 ChatGPT-4 generated samples,
simulating diverse user expressions and ensuring unbiased
performance assessment.
Evaluation Metrics. Our evaluation framework encom-
passes both idealized scenarios and practical constraints.
The system operates within a memory allocation of 128GB
for model parameters, with 80% of the remaining memory
(153GB) dedicated to KV-cache storage, accommodating
250K tokens’ worth of KV vectors. This configuration
establishes our upper bound for total input token capac-
ity. We implement a pragmatic 5-minute attack duration
limit, which aligns with typical prefix cache lifetimes in
production environments [89]. For rate limiting, we adopt
the Tier2 user constraints from OpenAI’s specification of
5,000 requests per minute (RPM), while acknowledging that
rate limits vary substantially across different API providers,
with some offering unrestricted access. We evaluate steal-
ing success rates for disease (ASRdisease) and symptom
(ASRsymptoms) prediction, overall input stealing success
rates (ASRall), number of attempts(Attempts), required
memory utilization(Tokens) and time consumption(Time).
The experimental evaluation encompasses two distinct
scenarios: an unconstrained setting (Ideal) representing the-
oretical maximum performance, and a constrained setting
(All) that incorporates all three practical limitations (mem-
ory, time, and rate constraints). This dual-scenario evaluation
framework enables us to quantify both the theoretical upper
bounds of system performance and its practical efficacy
under real-world operational constraints, where the perfor-
mance delta between scenarios provides valuable insights
into constraint impacts and highlights potential optimization
opportunities.
4.4. Evaluation Results
Timing Analyzer Evaluation.
We evaluate our timing
analyzer on prompts of 800 and 1,600 tokens as shown in
Table 2 and Table 3. For each prompt length, we conduct
experiments with varying sampling quantities (10, 300, 600,
and 900 samples) and evaluate three machine learning meth-
ods: Gradient Boosting, Random Forest, and XGBoost. The
models are tested on a dataset of 1,000 samples, measuring
two key metrics: SRBlock (success rate in predicting the
number of cache-hit blocks) and SRF ield (success rate
in predicting hits for four fields, assuming field starting
positions are separated by at least 16 tokens, which aligns
with realistic usage patterns). This comprehensive evaluation
framework allows us to assess the impact of sampling quan-
tity and the effectiveness of different prediction methods.
The results demonstrate that our timing analyzer can
achieve high accuracy with moderate sampling overhead,
making it efficient for real-world attacks. As shown in
Table 2, for 800-token inputs, our analyzer achieves optimal
performance with 600 sampling points, demonstrating an
86.34% success rate in predicting cache-hit block counts and
a near-perfect 97.25% accuracy in field-level hit detection.
Notably, increasing sampling beyond this point yields dimin-
ishing returns, indicating that moderate sampling quantities
are sufficient for effective timing analysis. Similar patterns
emerge for 1600-token inputs as shown in Table 3, where
600 samples achieve optimal performance with 87.13%
accuracy in block hit prediction and 100% accuracy in field-
level hit detection.
TABLE 2: Performance of the Time Analyzer With Input
Token Number is 800
Query Numbers
Prediction Methods
SRBlock
SRF ield
100
Gradient Boosting
68.43%
95.42%
Random Forest
64.64%
94.58%
XGBoost
56.27%
95.88%
300
Gradient Boosting
81.83%
96.41%
Random Forest
78.76%
95.69%
XGBoost
72.55%
96.01%
600
Gradient Boosting
86.34%
97.25%
Random Forest
79.28%
96.21%
XGBoost
81.63%
96.67%
900
Gradient Boosting
80.13%
97.45%
Random Forest
81.18%
96.86%
XGBoost
82.35%
96.80%
TABLE 3: Performance of the Time Analyzer With Input
Token Number is 1600
Query Numbers
Prediction Methods
SRBlock
SRF ield
100
Gradient Boosting
48.51%
93.07%
Random Forest
43.47%
91.29%
XGBoost
36.04%
95.05%
300
Gradient Boosting
75.84%
100%
Random Forest
74.95%
100%
XGBoost
64.06%
100%
600
Gradient Boosting
87.13%
99.70%
Random Forest
84.65%
100%
XGBoost
79.31%
100%
900
Gradient Boosting
86.73%
100%
Random Forest
84.54%
100%
XGBoost
82.97%
100%
End-to-End Attack Evaluation. We integrate our input
constructor and time analyzer for end-to-end attack assess-
ment in a shared prefix KV cache, where regular users and


---

attackers coexist on the same node. Attackers can determine
cache hits and infer other users’ inputs by analyzing TTFT
patterns of constructed requests. The attack proceeds by
conducting exhaustive searches for basic fields like age and
gender, then leveraging these successfully matched fields to
sequentially predict subsequent entries, with custom content
fields presenting the highest complexity.
We implement four approaches to implement the input
constructor. (1) The baseline approach employs random con-
struction based on knowledge learned from Formatted Input
Field datasets. (2) Additionally, we developed a machine
learning method using Gaussian Naive Bayes, comprising
two random constructors for Age and Gender and four dis-
tinct models trained on Formatted Input Field for subsequent
field prediction. (3) The framework also incorporates prob-
abilistic learning implemented at fine-grained vocabulary
levels, focusing on vocabulary probabilities and combination
relationships. (4) We utilize fine-tuned LLaMA-3 8B models
trained on three Formatted Input Field datasets, specifically
targeting high-complexity fields (Disease History, Symp-
toms, Duration). Each model specializes in predicting its
designated field based on previously matched inputs. To
ensure diverse outputs match user input patterns, we con-
figure the models with high sampling parameters (temper-
ature=0.99, top p=0.99, max tokens=100) and generate 30
candidates per inference. Given the inherent output diversity
and potential duplications, we perform 50 inference itera-
tions for each test sample, remove duplicates, and employ
a ranking mechanism to prioritize the predictions. These
ranked outputs are then sequentially submitted to the victim
service for cache hit attempts.
The comprehensive results are presented in Table 4. The
experimental results demonstrate the performance variations
across different scenarios and methods. Under ideal con-
ditions, the Probability-based Vocabulary approach demon-
strates superior performance, achieving the highest attack
success rates across disease prediction (67.5%), symptom
prediction (53.75%), and overall field prediction (49%).
However, due to its word-level search mechanism and rela-
tively poor grasp of coarse-grained correlations, this method
incurs substantial computational overhead. In contrast, the
GaussianNB method strikes an optimal balance between re-
source utilization and performance, showing approximately
twofold efficiency improvement compared to the baseline
approach. When subjected to real-world constraints, while
the Probability-based Vocabulary method maintains its lead-
ership in disease prediction (62%), its advantages in symp-
tom and overall prediction become less pronounced. The
GaussianNB method exhibits remarkable stability, achieving
the highest overall prediction accuracy (12.50%).
The Finetuned LLM approach, hampered by output vari-
ability and uncertainty, requires extensive attack attempts,
resulting in excessive memory and time consumption, yield-
ing performance dropping from 54.00% in ideal conditions
to complete failure under comprehensive constraints. These
findings suggest that traditional machine learning methods
offer a more reliable and efficient solution for precise match-
ing tasks under practical constraints than large language
models, particularly when considering the trade-off between
accuracy, resource utilization, and operational stability. The
Finetuned LLM shows promising potential in unrestricted
environments, its substantial resource requirements and per-
formance instability under constraints make it less suitable
for real-world attacks.
5. Attack 2: Retrieval Augmented Generation
This section introduces our input theft attacks, which
target semantic caching in applications assisted by retrieval-
augmented generation. We will demonstrate the effective-
ness of our attacks through experiments, highlighting the
privacy information leakage risks associated with LLM-
based applications.
5.1. Introduction to Attack Scenario
Semantic caching represents a crucial optimization for
LLM and RAG applications, offering developers signifi-
cant practical advantages. Research indicates that up to
31% of LLM calls are redundant, which can be effectively
eliminated through semantic caching [37]. This optimiza-
tion is particularly vital in Retrieval-Augmented Genera-
tion (RAG) systems, where rapid and accurate response
generation is critical for performance. Intelligently caching
semantically similar queries substantially reduces database
retrieval and LLM inference while dramatically improving
response times. For application developers, this translates
into more cost-effective scaling, improved system reliability,
and enhanced user experience. The growing adoption of
semantic caching is evident in LLM-based tools like GPT
for Work [111] plugin, attracting 6.88 million users. As
LLM applications continue to grow in popularity, semantic
caching emerges as an essential architectural pattern that ef-
fectively bridges the gap between powerful LLM capabilities
and real-world deployment constraints.
We identify a novel privacy vulnerability in RAG-
assisted LLM systems with semantic caching. In contrast
to exact matching approaches, our study examines semantic
caching systems that leverage similarity matching mech-
anisms, where responses are cached and retrieved based
on semantic proximity rather than exact matches. Through
systematic probing with carefully crafted queries, attackers
can exploit the timing side-channel to infer the presence of
specific topics or questions in the cache, potentially reveal-
ing sensitive information about other users’ interactions with
the system.
We demonstrate our attack scenario in the context of on-
line legal consultation services, where RAG-enhanced LLM
systems are commonly deployed to supplement responses
with up-to-date regulations and relevant case law. These
services typically implement semantic caching to optimize
performance and reduce computational overhead when han-
dling similar inquiries from multiple users. By exploiting the
shared cache architecture, our attack framework targets legal
consultation queries spanning various domains, enabling
attackers to infer the semantic content and topical nature


---

TABLE 4: End-to-End Attack Performance Comparison Across Different Methods Under Various Constraints. We evaluate
success rates for disease prediction (ASRdisease), symptom prediction (ASRsymptoms), and overall field prediction
(ASRall), along with required attempt counts(Attempts), memory usage (Tokens), and time consumption (Time). The
evaluation is conducted under two scenarios: Ideal represents the theoretical upper bound without any practical limitations,
All reflects real-world performance under memory constraints (250K tokens), rate limits (5,000 RPM), and time restrictions
(5-minute duration).
Ideal
Methods
ASRdisease
ASRsymptoms
ASRall
Attempts
Tokens
Time
Baseline
60.00%
23.50%
22.00%
6868 ± 6855
283799 ± 273969
1751 ± 1747
GaussianNB
59.00%
27.50%
26.50%
2904 ± 2830
108650 ± 94450
730 ± 622
Prob vocabulary
67.50%
53.75%
49.00%
502720 ± 2426519
11507996 ± 56966635
77918 ± 376100
Finetuned LLM
54.00%
18.00%
10.00%
659920 ± 6817828
6599206 ± 6817828
101500 ± 101001
All
Methods
ASRdisease
ASRsymptoms
ASRall
Attempts
Tokens
Time
Baseline
45.50%
9.00%
8.00%
1350 ± 1173
55926 ± 48139
232 ± 166
GaussianNB
54.00%
13.50%
12.50%
1086 ± 898
40207 ± 33421
276 ± 228
Prob vocabulary
62.00%
12.50%
9.50%
2247 ± 983
57526 ± 26768
348 ± 152
Finetuned LLM
00.00%
00.00%
00.00%
0 ± 0
0 ± 0
0 ± 0
of other users’ legal inquiries. This represents a significant
privacy concern as it exposes sensitive information about
users’ legal circumstances and consultation topics.
5.2. Methodologies
Our attack methodology comprises two key components:
a input constructor and a time analyzer. Due to the reduction
in retrieval and LLM inference processes when semantic re-
sponses are cached, there is a significant temporal disparity
between cache hits and misses, as illustrated in Figure 5.
Through temporal feature extraction from sampled data, our
timing analyzer achieves 100% accuracy in distinguishing
between cache hits and misses. However, the input con-
structor faces substantial challenges in crafting inputs within
an infinite search space, as it must generate semantically
relevant queries that effectively probe the cache contents.
We propose an intelligent constructor that systemati-
cally explores an extensive, unknown input space to hit the
cached user input. Our approach begins with semantic space
partitioning through hierarchical clustering of the training
dataset, establishing a foundational understanding of the
input space structure. To efficiently navigate this partitioned
space, we implement a weighted binary tree whose weights
are derived from cluster cardinality, enabling depth-first ex-
ploration of semantic clusters. The search strategy carefully
balances exploitation and exploration. While focusing on
promising regions near cluster centroids to leverage learned
patterns, the constructor simultaneously conducts peripheral
exploration around clusters to maintain search breadth. This
dual-focus approach is governed by parameters that dynam-
ically adjust the exploration-exploitation trade-off based on
search progress.
In each iteration, the constructor generates multiple can-
didate inputs, subjecting them to a sophisticated ranking
process. This ranking mechanism integrates multiple fac-
tors: historical attempt patterns, similarity relationships with
previous attempts, and the candidate’s representativeness
within the current pool. To maintain attack efficiency and
prevent redundant attempts, the constructor enforces diver-
sity constraints through similarity thresholds, ensuring each
new attempt is sufficiently distinct from previous ones while
remaining representative of the current search region.
The effectiveness of our constructor lies in its ability
to systematically explore high-dimensional semantic spaces
while adapting its search patterns based on accumulated
knowledge. Through this balanced approach of focused
exploitation and strategic exploration, the constructor effi-
ciently navigates the challenge of matching user inputs in
an extensive unknown space while maintaining diversity in
its attack attempts.
5.3. Experimental Setup and Evaluation
Experimental Setup. Our experimental framework utilized
GPTCache’s semantic caching infrastructure. We config-
ured the semantic similarity thresholds as 0.9 using its
default similarity evaluation method SearchDistanceEvalu-
ation [50]. We tend to choose a deliberately conservative
threshold setting to ensure high-fidelity semantic match-
ing and to impose stringent quality requirements on our
constructor’s output. This high threshold helped eliminate
false positives in cache hits, enabling more precise mea-
surement of performance differences. The RAG database
was constructed by uploading legal corpus to the OpenAI
organization platform, where we measured and analyzed
retrieval latencies under various query inputs.
Input Constructor Evaluation. Our study employed a
large-scale legal consultation corpus CrimeKgAssitant [1]
consisting of 200,000 question-answer pairs, categorized
into 13 distinct legal domains. The dataset’s categorical dis-
tribution, training data sizes, and input length statistics are
detailed in Table 5. We extracted the user query components
from the dataset for input constructor training. To simulate
diverse real-world user inputs, we employ ChatGPT-4 to
generate 30 test queries and split 30 test data from the
training dataset, forming a test set that reflects varied user
expression patterns.


---

Empirical results demonstrate that our prompt construc-
tor can effectively capture semantic spaces from training
data. Through iterative probing of user input requests, it
achieves semantic extraction success rates ranging from 43%
to 100%, as presented in Table 5. This effectiveness in
generalizing across disparate datasets highlights the robust
learning capabilities in input reconstruction.
TABLE 5: Systematic evaluation of attack success rates
(ASR). Constructor learned from CrimeKgAssitant datasets
with 13 legal domains and tested with GPT-4 generated data
Category
Count
Len. Avg. ± Std.
ASR
Marriage and Family
39281
38.11 ± 344.29
93.33%
Labor Disputes
35011
38.79 ± 329.54
90.00%
Traffic Accidents
22646
39.45 ± 317.98
100.00%
Debt Disputes
21925
38.47 ± 364.97
90.00%
Criminal Defense
18314
35.96 ± 364.64
93.33%
Contract Disputes
13765
39.73 ± 340.30
96.67%
Property Disputes
12071
36.65 ± 337.62
100.00%
Infringement
10594
39.03 ± 426.91
43.33%
Company Law
10011
38.47 ± 340.90
70.00%
Medical Disputes
7285
39.07 ± 345.55
73.33%
Demolition and Resettlement
7022
40.91 ± 320.75
100.00%
Administrative Litigation
2776
36.14 ± 302.39
93.10%
Construction Projects
1610
42.11 ± 262.82
63.33%
6. Discussion and Defenses
6.1. Discussion
Trade-offs in Cloud-Deployed LLMs. While local LLM
deployment provides inherent privacy benefits, cloud de-
ployment remains inevitable due to the substantial com-
putational requirements of modern inference systems that
exceed edge device capabilities. This fundamental tension
between privacy and performance has led to various privacy-
preserving solutions, including homomorphic encryption,
TEEs, and data masking for cloud-based inference. How-
ever, our timing side-channel attack demonstrates that these
cryptographic approaches fail to address privacy leakage
arising by system-level optimizations like prefix cache-
sharing.
Our research exposes a critical vulnerability where per-
formance optimizations in cloud-deployed LLMs, while es-
sential for scalability, create exploitable privacy side chan-
nels. This finding reveals an important trade-off: shared
caching mechanisms that enhance system efficiency simul-
taneously introduce subtle yet significant privacy risks. The
implications extend beyond LLM systems, contributing to
the broader discourse on balancing security and performance
in cloud computing architectures and emphasizing the need
for comprehensive privacy-preserving designs that consider
cryptographic protections and system-level vulnerabilities.
Limitations of the Attack. One significant limitation of our
attack is the lack of fine-grained information for accurately
reconstructing the input. While our insights effectively cap-
ture secret information in the data, it does not provide the
detailed context required for precise recovery. To address
this, integrating techniques such as embedding inversion and
token-length side channels could enhance the attack’s ef-
fectiveness by offering additional granular information. Our
methods could serve as validation mechanisms, improving
the reliability of the input recovery process.
Another critical limitation is that our attack cannot link
the inferred inputs to specific users or input objects, prevent-
ing meaningful use of the stolen data in real-world scenarios.
This lack of user attribution significantly limits the attack’s
practical implications.
6.2. Potential Defenses
Our attack involves manipulating prompt sequences to
probe the KV Cache, allowing adversaries to determine
cache hits or misses based on the time taken to generate
responses. This section delineates strategic defense mecha-
nisms to mitigate such timing attacks.
User-Level Cache Isolation. Implementing distinct cache
namespaces prevents cross-user cache sharing, effectively
containing cache states within individual sessions. While
this approach strengthens security, it trades off system
efficiency. Major providers like OpenAI’s API [89] and
DeepSeek’s API [22] implement isolation for prefix caching
to protect user privacy and security.
Rate Limiting to Mitigate Frequent Sttacks. By restrict-
ing request frequency, rate limiting impedes rapid successive
probing necessary for timing analysis. This defense not only
prevents brute-force attempts but also maintains infrastruc-
ture stability. While OpenAI employs this approach [85],
careful calibration is needed to balance security with legit-
imate user access.
Complicate Time Analysis. Timing obfuscation approaches
can effectively prevent attackers from exploiting response
time variations. Similar techniques have been proven effec-
tive against model extraction attacks, where timing patterns
can reveal model parameters through correlations with acti-
vation functions [8], network depth [30], and computational
operations [27]. We propose two key obfuscation strategies:
response time homogenization through either constant-time
execution [69] or random delay injection [9] and disabling
streaming responses to eliminate measurable timing patterns
Our defense strategy emphasizes the critical balance
between security, performance, and user experience. While
each mechanism provides distinct protection against timing-
based attacks, their implementation requires careful consid-
eration of operational requirements and threat models. A
combined approach, tailored to specific deployment con-
texts, offers the most robust protection against our demon-
strated side-channel vulnerabilities.
7. Related Work
7.1. Side-channel Attacks on AI Systems
Side-channel attacks can exploit indirect information
leakage from AI systems, such as timing [27], [30],
[39], power consumption [118], [127], or electromagnetic


---

emissions [8], [45], [140], to infer sensitive model ar-
chitectures and parameters. Additionally, cache-based side
channels [44], [122], [135], memory access patterns [46],
[47], and resource contention information [31], [80], [114]
provide further avenues for gleaning model information.
Debenedetti et al. [21] explore privacy side-channel attacks
in machine learning systems, highlighting how system-level
components can be manipulated to leak private information
more effectively than standalone models.
However, due to the complexity of large model systems,
side-channel attacks against large model systems remain
relatively rare. Weiss et al. [128] introduce a novel token-
length side-channel vulnerability affecting AI assistants,
demonstrating how encrypted responses from AI Chatbots
can be partially reconstructed by analyzing the length of
transmitted tokens over the network. In contrast, our work
proposes a new timing-based side-channel attack to capture
user input and analyze cache behavior over time, revealing
the cache-sharing dynamics within LLM systems.
7.2. Prompt Text Leakage Attacks
Adversarial Prompts. The art of prompt engineering is
crucial and often regarded as proprietary because it opti-
mizes model performance for specific tasks [126], enhances
instruction-following capabilities [90], and aligns outputs
with human values [6], making system prompts valuable
assets [125]. Perez et al. [91] first explore prompt leakage in
LLMs by injecting crafted prompts that misalign the model’s
intended goals, allowing attackers to extract sensitive in-
formation embedded in the prompts. Recent studies [63],
[141], [144] primarily focus on developing more effective
adversarial prompts to expose the system prompts of LLMs.
Prompts Inversion. Another type of attack involves infer-
ring input prompts from generated content. Input text data is
often represented as embedding vectors in natural language
processing, especially with the proliferation of LLMs. Previ-
ous work [105] has demonstrated the possibility of obtaining
sensitive information of original inputs by inverting these
embedding vectors. Since then, numerous studies have fo-
cused on embedding inversion attacks on language models,
Morris et al. [79] recover significant personal information
from clinical datasets, Li et al. [59] generate coherent and
similar sentences to the original input, and Chen et al. [16]
investigate multilingual inversion attacks.
In addition to embedding vectors, other information can
also be used to reverse the input content. Morris et al. [78]
suggest that the next-token probability distribution contains
information about the preceding text, which can be exploited
to reconstruct input prompts, highlighting the significance
of residual information in model outputs. Similarly, images
generated by text-to-image diffusion models [67], [101] and
texts produced by LLMs [97], [138] can also be exploited
to reverse-engineer text prompts. These attack techniques
adversely affect the commercial benefits of the prompt mar-
ketplace and infringe on prompt engineers’ property rights.
7.3. Memorization and Privacy Risks in LLMs
Memorization of language models can remember parts
of the training data, which encompasses web crawls of
personal pages, social media messages, and internal email
databases, raising concerns about data copyright and privacy
breaches. Pre-trained and fine-tuned language models [48],
[57], [74], [75] also subject to these issues. Carlini et
al. [13] demonstrate that larger models tend to memorize
more information, highlighting the necessity of mitigating
memorization as the model scale keeps growing.
If the training data is leaked, it can result in the unau-
thorized exposure of confidential information, raising ethical
concerns regarding consent and data ownership. Training
data extraction attacks [10], [14] can recover sensitive train-
ing examples by querying the LLMs. Additionally, member-
ship inference attacks [13], [71], [72], [106] can determine
whether specific data was included in the training dataset,
potentially compromising the copyrights of the data owner.
Memorization significantly threatens personal privacy by
potentially retaining and recalling sensitive information from
training data. Lukas et al. [66] explore privacy-utility trade-
offs of using defenses such as PII scrubbing and Differ-
entially Private training when fine-tuning language models.
Kim et al. [55] enable data subjects to determine whether
their PII is at risk of disclosure through queries. Staab
et al. [108] indicates that LLMs can automatically reason
author attributes from unstructured text, greatly reducing the
cost associated with privacy violation.
References
[1]
GitHub
-
liuhuanyong/CrimeKgAssitant.
https://github.com/
liuhuanyong/CrimeKgAssitant. [Accessed 14-11-2024].
[2]
Online Tools and Utilities: Generators. https://webutility.io/.
[3]
Anthropic.
Prompt caching (beta).
https://docs.anthropic.com/en/
docs/build-with-claude/prompt-caching, 2024.
[4]
Microsoft Azure. Tutorial: Use Azure Cache for Redis as a seman-
tic cache. https://learn.microsoft.com/en-us/azure/azure-cache-for-
redis/cache-tutorial-semantic-cache, 2024.
[5]
Jinze Bai, Shuai Bai, Yunfei Chu, Zeyu Cui, Kai Dang, Xiaodong
Deng, Yang Fan, Wenbin Ge, Yu Han, Fei Huang, et al.
Qwen
technical report. arXiv preprint arXiv:2309.16609, 2023.
[6]
Yuntao Bai, Andy Jones, Kamal Ndousse, Amanda Askell, Anna
Chen, Nova DasSarma, Dawn Drain, Stanislav Fort, Deep Ganguli,
Tom Henighan, et al.
Training a helpful and harmless assistant
with reinforcement learning from human feedback. arXiv preprint
arXiv:2204.05862, 2022.
[7]
Fu Bang. Gptcache: An open-source semantic cache for llm appli-
cations enabling faster answers and cost savings. In Proceedings of
the 3rd Workshop for Natural Language Processing Open Source
Software (NLP-OSS 2023), pages 212–218, 2023.
[8]
Lejla Batina, Shivam Bhasin, Dirmanto Jap, and Stjepan Picek.
{CSI}{NN}: Reverse engineering of neural network architectures
through electromagnetic side channel.
In 28th USENIX Security
Symposium (USENIX Security 19), pages 515–532, 2019.
[9]
Jakub Breier, Dirmanto Jap, Xiaolu Hou, and Shivam Bhasin. A
desynchronization-based countermeasure against side-channel anal-
ysis of neural networks. In International Symposium on Cyber Secu-
rity, Cryptology, and Machine Learning, pages 296–306. Springer,
2023.


---

[10]
Hannah Brown, Katherine Lee, Fatemehsadat Mireshghallah, Reza
Shokri, and Florian Tram`er. What does it mean for a language model
to preserve privacy? In Proceedings of the 2022 ACM conference on
fairness, accountability, and transparency, pages 2280–2292, 2022.
[11]
Tom B Brown, Benjamin Mann, Nick Ryder, Melanie Subbiah, Jared
Kaplan, Prafulla Dhariwal, Arvind Neelakantan, Pranav Shyam,
Girish Sastry, Amanda Askell, et al. Language models are few-shot
learners. In Advances in Neural Information Processing Systems,
2020.
[12]
Tuan Bui, Oanh Tran, Phuong Nguyen, Bao Ho, Long Nguyen,
Thang Bui, and Tho Quan. Cross-data knowledge graph construc-
tion for llm-enabled educational question-answering system: A case
study at hcmut. In Proceedings of the 1st ACM Workshop on AI-
Powered Q&A Systems for Multimedia, pages 36–43, 2024.
[13]
Nicholas Carlini, Daphne Ippolito, Matthew Jagielski, Katherine
Lee, Florian Tramer, and Chiyuan Zhang. Quantifying memorization
across neural language models. arXiv preprint arXiv:2202.07646,
2022.
[14]
Nicholas Carlini, Florian Tramer, Eric Wallace, Matthew Jagielski,
Ariel Herbert-Voss, Katherine Lee, Adam Roberts, Tom Brown,
Dawn Song, Ulfar Erlingsson, et al. Extracting training data from
large language models.
In 30th USENIX Security Symposium
(USENIX Security 21), pages 2633–2650, 2021.
[15]
Angelica Chen, David Dohan, and David So. Evoprompting: lan-
guage models for code-level neural architecture search. Advances
in Neural Information Processing Systems, 36, 2024.
[16]
Yiyi Chen, Heather Lent, and Johannes Bjerva.
Text embedding
inversion security for multilingual language models. In Proceedings
of the 62nd Annual Meeting of the Association for Computational
Linguistics (Volume 1: Long Papers), pages 7808–7827, 2024.
[17]
Aakanksha Chowdhery, Sharan Narang, Jacob Devlin, Mikhail
Bosma, Adam Roberts, Maitreya Subbiah, Quoc V Le, and Ilya
Sutskever. Palm: Scaling language modeling with pathways. arXiv
preprint arXiv:2204.02311, 2022.
[18]
KuanChao Chu, Yi-Pei Chen, and Hideki Nakayama.
A better
llm evaluator for text generation: The impact of prompt output
sequencing and optimization.
arXiv preprint arXiv:2406.09972,
2024.
[19]
Jiaxi Cui, Zongjian Li, Yang Yan, Bohua Chen, and Li Yuan.
Chatlaw: Open-source legal large language model with integrated
external knowledge bases. arXiv preprint arXiv:2306.16092, 2023.
[20]
Cade Daniel, Chen Shen, Eric Liang, and Richard Liaw.
How
continuous batching enables 23x throughput in llm inference while
reducing p50 latency, 2023.
[21]
Edoardo Debenedetti, Giorgio Severi, Nicholas Carlini, Christo-
pher A Choquette-Choo, Matthew Jagielski, Milad Nasr, Eric Wal-
lace, and Florian Tram`er. Privacy side channels in machine learning
systems. In 33rd USENIX Security Symposium (USENIX Security
24), pages 6861–6848, 2024.
[22]
DeepSeek. DeepSeek API Docs: DeepSeek API introduces Context
Caching on Disk, cutting prices by an order of magnitude. https:
//api-docs.deepseek.com/news/news0802/.
[23]
DeepSeek.
DeepSeek API introduces Context Caching on Disk,
cutting prices by an order of magnitude. https://api-docs.deepseek.
com/news/news0802/, 2024.
[24]
Tim Dettmers, Mike Lewis, Younes Belkada, and Luke Zettlemoyer.
Gpt3. int8 (): 8-bit matrix multiplication for transformers at scale.
Advances in Neural Information Processing Systems, 35:30318–
30332, 2022.
[25]
Yiran Ding, Li Lyna Zhang, Chengruidong Zhang, Yuanyuan Xu,
Ning Shang, Jiahang Xu, Fan Yang, and Mao Yang.
Longrope:
Extending llm context window beyond 2 million tokens.
arXiv
preprint arXiv:2402.13753, 2024.
[26]
Jean-Baptiste D¨oderlein, Mathieu Acher, Djamel Eddine Khel-
ladi, and Benoit Combemale.
Piloting copilot and codex: Hot
temperature, cold prompts, or black magic?
arXiv preprint
arXiv:2210.14699, 2022.
[27]
Gaofeng Dong, Ping Wang, Ping Chen, Ruizhe Gu, and Honggang
Hu.
Floating-point multiplication timing attack on deep neural
network. In 2019 IEEE International Conference on Smart Internet
of Things (SmartIoT), pages 155–161. IEEE, 2019.
[28]
Harry Dong, Xinyu Yang, Zhenyu Zhang, Zhangyang Wang, Yuejie
Chi, and Beidi Chen. Get more with less: Synthesizing recurrence
with kv cache compression for efficient llm inference. arXiv preprint
arXiv:2402.09398, 2024.
[29]
Abhimanyu Dubey, Abhinav Jauhri, Abhinav Pandey, Abhishek
Kadian, Ahmad Al-Dahle, Aiesha Letman, Akhil Mathur, Alan
Schelten, Amy Yang, Angela Fan, et al. The llama 3 herd of models.
arXiv preprint arXiv:2407.21783, 2024.
[30]
Vasisht Duddu, Debasis Samanta, D Vijay Rao, and Valentina E
Balas. Stealing neural networks via timing side channels. arXiv
preprint arXiv:1812.11720, 2018.
[31]
Sankha Baran Dutta, Hoda Naghibijouybari, Arjun Gupta, Nael
Abu-Ghazaleh, Andres Marquez, and Kevin Barker.
Spy in the
gpu-box: Covert and side channel attacks on multi-gpu systems.
In Proceedings of the 50th Annual International Symposium on
Computer Architecture, pages 1–13, 2023.
[32]
Dom Eccleston.
Semantic caching.
https://www.unkey.com/blog/
semantic-caching, 2024.
[33]
Chenhao Fang, Xiaohan Li, Zezhong Fan, Jianpeng Xu, Kaushiki
Nag, Evren Korpeoglu, Sushant Kumar, and Kannan Achan. Llm-
ensemble: Optimal large language model ensemble method for e-
commerce product attribute value extraction.
In Proceedings of
the 47th International ACM SIGIR Conference on Research and
Development in Information Retrieval, pages 2910–2914, 2024.
[34]
Elias Frantar, Saleh Ashkboos, Torsten Hoefler, and Dan Alistarh.
Gptq: Accurate post-training quantization for generative pre-trained
transformers. arXiv preprint arXiv:2210.17323, 2022.
[35]
Tianyu Gao, Adam Fisch, and Danqi Chen.
Making pre-
trained language models better few-shot learners.
arXiv preprint
arXiv:2012.15723, 2020.
[36]
GenerateStory. Free AI Story Generator. https://generatestory.io/.
[37]
Waris Gill, Mohamed Elidrisi, Pallavi Kalapatapu, Ali Anwar, and
Muhammad Ali Gulzar.
Privacy-aware semantic cache for large
language models. arXiv preprint arXiv:2403.02694, 2024.
[38]
In Gim, Guojun Chen, Seung-seob Lee, Nikhil Sarda, Anurag Khan-
delwal, and Lin Zhong.
Prompt cache: Modular attention reuse
for low-latency inference. Proceedings of Machine Learning and
Systems, 6:325–338, 2024.
[39]
Cheng Gongye, Yunsi Fei, and Thomas Wahl. Reverse-engineering
deep neural networks using floating-point timing side-channels. In
2020 57th ACM/IEEE Design Automation Conference (DAC), pages
1–6. IEEE, 2020.
[40]
Google. Gemini API : Google AI for Developers. https://ai.google.
dev/gemini-api/docs/caching?lang=python, 2024.
[41]
Yuxian Gu, Xu Han, Zhiyuan Liu, and Minlie Huang. Ppt: Pre-
trained prompt tuning for few-shot learning.
In Proceedings of
the 60th Annual Meeting of the Association for Computational
Linguistics (Volume 1: Long Papers), pages 8410–8423, 2022.
[42]
Xinnan Guo, Wentao Deng, Yongrui Chen, Yang Li, Mengdi Zhou,
Guilin Qi, Tianxing Wu, Dong Yang, Liubin Wang, and Yong
Pan.
Comave: Contrastive pre-training with multi-scale masking
for attribute value extraction.
In Findings of the Association for
Computational Linguistics: ACL 2023, pages 6007–6018, 2023.
[43]
Chi Han, Qifan Wang, Wenhan Xiong, Yu Chen, Heng Ji, and Sinong
Wang. Lm-infinite: Simple on-the-fly length generalization for large
language models. arXiv preprint arXiv:2308.16137, 2023.


---

[44]
Sanghyun Hong, Michael Davinroy, Yiˇgitcan Kaya, Stuart Nevans
Locke, Ian Rackow, Kevin Kulda, Dana Dachman-Soled, and Tudor
Dumitras¸.
Security analysis of deep neural networks operating
in the presence of cache side-channel attacks.
arXiv preprint
arXiv:1810.03487, 2018.
[45]
Peter Horvath, Lukasz Chmielewski, Leo Weissbart, Lejla Batina,
and Yuval Yarom. Barracuda: Bringing electromagnetic side channel
into play to steal the weights of neural networks from nvidia gpus.
arXiv preprint arXiv:2312.07783, 2023.
[46]
Xing Hu, Ling Liang, Shuangchen Li, Lei Deng, Pengfei Zuo, Yu Ji,
Xinfeng Xie, Yufei Ding, Chang Liu, Timothy Sherwood, et al.
Deepsniffer: A dnn model extraction framework based on learning
architectural hints. In Proceedings of the Twenty-Fifth International
Conference on Architectural Support for Programming Languages
and Operating Systems, pages 385–399, 2020.
[47]
Weizhe Hua, Zhiru Zhang, and G Edward Suh. Reverse engineer-
ing convolutional neural networks through side-channel information
leaks.
In Proceedings of the 55th Annual Design Automation
Conference, pages 1–6, 2018.
[48]
Jie Huang, Hanyin Shao, and Kevin Chen-Chuan Chang. Are large
pre-trained language models leaking your personal information?
arXiv preprint arXiv:2205.12628, 2022.
[49]
HuggingFace. Large Language Model Text Generation Inference.
https://github.com/huggingface/text-generation-inference.
[50]
Zilliz Inc. How to better configure your cache; GPTCache. https:
//gptcache.readthedocs.io/en/latest/configure it.html, 2023.
[51]
InternLM. LMDeploy is a toolkit for compressing, deploying, and
serving LLMs. https://github.com/InternLM/lmdeploy/tree/main.
[52]
Jordan Juravsky, Bradley Brown, Ryan Ehrlich, Daniel Y Fu,
Christopher R´e, and Azalia Mirhoseini. Hydragen: High-throughput
llm inference with shared prefixes. arXiv preprint arXiv:2402.05099,
2024.
[53]
Rishi Kalra, Zekun Wu, Ayesha Gulley, Airlie Hilliard, Xin Guan,
Adriano Koshiyama, and Philip Treleaven.
Hypa-rag: A hybrid
parameter adaptive retrieval-augmented generation system for ai
legal and policy applications.
arXiv preprint arXiv:2409.09046,
2024.
[54]
Santiago Flores Kanter.
Improve speed and reduce cost for
generative AI workloads with a persistent semantic cache in Amazon
MemoryDB.
https://aws.amazon.com/cn/blogs/database/improve-
speed-and-reduce-cost-for-generative-ai-workloads-with-a-
persistent-semantic-cache-in-amazon-memorydb/, 2024.
[55]
Siwon Kim, Sangdoo Yun, Hwaran Lee, Martin Gubri, Sungroh
Yoon, and Seong Joon Oh.
Propile: Probing privacy leakage in
large language models. Advances in Neural Information Processing
Systems, 36, 2024.
[56]
Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin
Zheng, Cody Hao Yu, Joseph Gonzalez, Hao Zhang, and Ion Stoica.
Efficient memory management for large language model serving
with pagedattention.
In Proceedings of the 29th Symposium on
Operating Systems Principles, pages 611–626, 2023.
[57]
Eric Lehman, Sarthak Jain, Karl Pichotta, Yoav Goldberg, and
Byron C Wallace.
Does bert pretrained on clinical notes reveal
sensitive data? arXiv preprint arXiv:2104.07762, 2021.
[58]
Patrick Lewis, Ethan Perez, Aleksandra Piktus, Fabio Petroni,
Vladimir Karpukhin, Naman Goyal, Heinrich K¨uttler, Mike Lewis,
Wen-tau Yih, Tim Rockt¨aschel, et al.
Retrieval-augmented gen-
eration for knowledge-intensive nlp tasks.
Advances in Neural
Information Processing Systems, 33:9459–9474, 2020.
[59]
Haoran Li, Mingshi Xu, and Yangqiu Song. Sentence embedding
leaks more information than you expect: Generative embedding
inversion attack to recover the whole sentence. In Findings of the
Association for Computational Linguistics: ACL 2023, pages 14022–
14040, 2023.
[60]
Jia Li, Ge Li, Yongmin Li, and Zhi Jin. Structured chain-of-thought
prompting for code generation.
ACM Transactions on Software
Engineering and Methodology, 2023.
[61]
Jiaxing Li, Chi Xu, Feng Wang, Isaac M von Riedemann, Cong
Zhang, and Jiangchuan Liu. Scalm: Towards semantic caching for
automated chat services with large language models. arXiv preprint
arXiv:2406.00025, 2024.
[62]
Yunxiang Li, Zihan Li, Kai Zhang, Ruilong Dan, Steve Jiang, and
You Zhang. Chatdoctor: A medical chat model fine-tuned on a large
language model meta-ai (llama) using medical domain knowledge.
Cureus, 15(6), 2023.
[63]
Zi Liang, Haibo Hu, Qingqing Ye, Yaxin Xiao, and Haoyang
Li.
Why are my prompts leaked? unraveling prompt extrac-
tion threats in customized large language models. arXiv preprint
arXiv:2408.02416, 2024.
[64]
Anna Lieb and Toshali Goel. Student interaction with newtbot: An
llm-as-tutor chatbot for secondary physics education. In Extended
Abstracts of the CHI Conference on Human Factors in Computing
Systems, pages 1–8, 2024.
[65]
Ji Lin, Jiaming Tang, Haotian Tang, Shang Yang, Wei-Ming Chen,
Wei-Chen Wang, Guangxuan Xiao, Xingyu Dang, Chuang Gan, and
Song Han. Awq: Activation-aware weight quantization for on-device
llm compression and acceleration. Proceedings of Machine Learning
and Systems, 6:87–100, 2024.
[66]
Nils Lukas, Ahmed Salem, Robert Sim, Shruti Tople, Lukas
Wutschitz, and Santiago Zanella-B´eguelin.
Analyzing leakage of
personally identifiable information in language models.
In 2023
IEEE Symposium on Security and Privacy (SP), pages 346–363.
IEEE, 2023.
[67]
Shweta Mahajan, Tanzila Rahman, Kwang Moo Yi, and Leonid
Sigal. Prompting hard or hardly prompting: Prompt inversion for
text-to-image diffusion models. In Proceedings of the IEEE/CVF
Conference on Computer Vision and Pattern Recognition, pages
6808–6817, 2024.
[68]
Shweta Mahajan, Tanzila Rahman, Kwang Moo Yi, and Leonid
Sigal. Prompting hard or hardly prompting: Prompt inversion for
text-to-image diffusion models. In 2024 IEEE/CVF Conference on
Computer Vision and Pattern Recognition (CVPR), pages 6808–
6817, 2024.
[69]
Saurav Maji, Utsav Banerjee, and Anantha P Chandrakasan. Leaky
nets: Recovering embedded neural network models and inputs
through simple power and timing side-channels—attacks and de-
fenses. IEEE Internet of Things Journal, 8(15):12079–12092, 2021.
[70]
Ggaliwango Marvin, Nakayiza Hellen, Daudi Jjingo, and Joyce
Nakatumba-Nabende. Prompt engineering in large language mod-
els. In International conference on data intelligence and cognitive
informatics, pages 387–402. Springer, 2023.
[71]
Justus Mattern, Fatemehsadat Mireshghallah, Zhijing Jin, Bernhard
Schoelkopf, Mrinmaya Sachan, and Taylor Berg-Kirkpatrick. Mem-
bership inference attacks against language models via neighbour-
hood comparison. In The 61st Annual Meeting Of The Association
For Computational Linguistics, 2023.
[72]
Matthieu Meeus, Shubham Jain, Marek Rei, and Yves-Alexandre
de Montjoye.
Did the neurons read your book? document-level
membership inference for large language models. In 33rd USENIX
Security Symposium (USENIX Security 24), pages 2369–2385, 2024.
[73]
Silvia Milano, Joshua A McGrane, and Sabina Leonelli.
Large
language models challenge the future of higher education. Nature
Machine Intelligence, 5(4):333–334, 2023.
[74]
Fatemehsadat Mireshghallah, Archit Uniyal, Tianhao Wang, David
Evans, and Taylor Berg-Kirkpatrick. Memorization in nlp fine-tuning
methods. arXiv preprint arXiv:2205.12506, 2022.
[75]
Fatemehsadat
Mireshghallah,
Archit
Uniyal,
Tianhao
Wang,
David K Evans, and Taylor Berg-Kirkpatrick. An empirical analy-
sis of memorization in fine-tuned autoregressive language models.
In Proceedings of the 2022 Conference on Empirical Methods in
Natural Language Processing, pages 1816–1826, 2022.


---

[76]
Ramaswami Mohandoss. Context-based semantic caching for llm
applications.
In 2024 IEEE Conference on Artificial Intelligence
(CAI), pages 371–376. IEEE, 2024.
[77]
MoonShot.
Using the Context Caching Feature of the Kimi
API. https://platform.moonshot.cn/docs/guide/use-context-caching-
feature-of-kimi-api, 2024.
[78]
John X Morris, Wenting Zhao, Justin T Chiu, Vitaly Shmatikov,
and Alexander M Rush. Language model inversion. arXiv preprint
arXiv:2311.13647, 2023.
[79]
John Xavier Morris, Volodymyr Kuleshov, Vitaly Shmatikov, and
Alexander M Rush. Text embeddings reveal (almost) as much as
text.
In The 2023 Conference on Empirical Methods in Natural
Language Processing, 2023.
[80]
Hoda Naghibijouybari, Ajaya Neupane, Zhiyun Qian, and Nael Abu-
Ghazaleh. Rendered insecure: Gpu side channel attacks are practical.
In Proceedings of the 2018 ACM SIGSAC conference on computer
and communications security, pages 2139–2153, 2018.
[81]
Xuan-Phi Nguyen, Shrey Pandit, Senthil Purushwalkam, Austin Xu,
Hailin Chen, Yifei Ming, Zixuan Ke, Silvio Savarese, Caiming
Xong, and Shafiq Joty. Sfr-rag: Towards contextually faithful llms.
arXiv preprint arXiv:2409.09916, 2024.
[82]
NVIDIA.
NVIDIA TensorRT-LLM.
https://docs.nvidia.com/
tensorrt-llm/index.html.
[83]
NVIDIA.
Ragged Batching; NVIDIA Triton Inference Server.
https://docs.nvidia.com/deeplearning/triton-inference-server/user-
guide/docs/user guide/ragged batching.html,
2024.
[Accessed
13-11-2024].
[84]
OpenAI. Gpt-4 turbo in the openai api. https://help.openai.com/en/
articles/8555510-gpt-4-turbo-in-the-openai-api.
[85]
OpenAI.
Openai
developer
platform,
Rate
limits.
https://platform.openai.com/docs/guides/rate-limits/what-are-
the-rate-limits-for-our-api.
[86]
OpenAI. Introducing chatgpt. https://openai.com/blog/chatgpt, 2022.
[87]
OpenAI. Introducing the GPT Store We’re launching the GPT Store
to help you find. https://chat.openai.com/gpts, 2024.
[88]
OpenAI.
Learning to Reason with LLMs We are introducing
OpenAI o1, a new large language model trained with LLMs.
https://openai.com/index/learning-to-reason-with-llms/, 2024.
[89]
OpenAI.
Prompt caching: Reduce latency and cost with prompt
caching.
https://platform.openai.com/docs/guides/prompt-caching,
2024.
[90]
Long Ouyang, Jeffrey Wu, Xu Jiang, Diogo Almeida, Carroll Wain-
wright, Pamela Mishkin, Chong Zhang, Sandhini Agarwal, Katarina
Slama, Alex Ray, et al.
Training language models to follow in-
structions with human feedback. Advances in neural information
processing systems, 35:27730–27744, 2022.
[91]
F´abio Perez and Ian Ribeiro.
Ignore previous prompt: Attack
techniques for language models. arXiv preprint arXiv:2211.09527,
2022.
[92]
Reiner Pope, Sholto Douglas, Aakanksha Chowdhery, Jacob Devlin,
James Bradbury, Jonathan Heek, Kefan Xiao, Shivani Agrawal, and
Jeff Dean. Efficiently scaling transformer inference. Proceedings of
Machine Learning and Systems, 5:606–624, 2023.
[93]
Portkey. Cache (Simple & Semantic) - Portkey Docs. https://portkey.
ai/docs/product/ai-gateway/cache-simple-and-semantic, 2024.
[94]
Pranab Sahoo, Ayush Kumar Singh, Sriparna Saha, Vinija Jain,
Samrat Mondal, and Aman Chadha. A systematic survey of prompt
engineering in large language models: Techniques and applications.
arXiv preprint arXiv:2402.07927, 2024.
[95]
Malik Sallam. Chatgpt utility in healthcare education, research, and
practice: systematic review on the promising perspectives and valid
concerns. In Healthcare, volume 11, page 887. MDPI, 2023.
[96]
Melanie Sclar, Yejin Choi, Yulia Tsvetkov, and Alane Suhr. Quan-
tifying language models’ sensitivity to spurious features in prompt
design or: How i learned to start worrying about prompt formatting.
arXiv preprint arXiv:2310.11324, 2023.
[97]
Zeyang Sha and Yang Zhang. Prompt stealing attacks against large
language models. arXiv preprint arXiv:2402.12959, 2024.
[98]
Arun
Shankar.
Implementing
Semantic
Caching:
A
Step-
by-Step
Guide
to
Faster,
Cost-Effective
GenAI
Workflows.
https://medium.com/google-cloud/implementing-semantic-caching-
a-step-by-step-guide-to-faster-cost-effective-genai-workflows-
ef85d8e72883, 2024.
[99]
Wenqi Shao, Mengzhao Chen, Zhaoyang Zhang, Peng Xu, Lirui
Zhao, Zhiqian Li, Kaipeng Zhang, Peng Gao, Yu Qiao, and Ping
Luo. Omniquant: Omnidirectionally calibrated quantization for large
language models. arXiv preprint arXiv:2308.13137, 2023.
[100] Zhenwei Shao, Zhou Yu, Meng Wang, and Jun Yu. Prompting large
language models with answer heuristics for knowledge-based visual
question answering. In Proceedings of the IEEE/CVF Conference
on computer vision and pattern recognition, pages 14974–14983,
2023.
[101] Xinyue Shen, Yiting Qu, Michael Backes, and Yang Zhang. Prompt
stealing attacks against {Text-to-Image} generation models. In 33rd
USENIX Security Symposium (USENIX Security 24), pages 5823–
5840, 2024.
[102] Juanming Shi, Qinglang Guo, Yong Liao, Yuxing Wang, Shijia Chen,
and Shenglin Liang. Legal-lm: Knowledge graph enhanced large
language models for law consulting. In International Conference
on Intelligent Computing, pages 175–186. Springer, 2024.
[103] Karan Singhal, Shekoofeh Azizi, Tao Tu, S Sara Mahdavi, Jason
Wei, Hyung Won Chung, Nathan Scales, Ajay Tanwani, Heather
Cole-Lewis, Stephen Pfohl, et al. Large language models encode
clinical knowledge. Nature, 620(7972):172–180, 2023.
[104] Karan Singhal, Tao Tu, Juraj Gottweis, Rory Sayres, Ellery Wul-
czyn, Le Hou, Kevin Clark, Stephen Pfohl, Heather Cole-Lewis,
Darlene Neal, et al. Towards expert-level medical question answer-
ing with large language models. arXiv preprint arXiv:2305.09617,
2023.
[105] Congzheng Song and Ananth Raghunathan.
Information leakage
in embedding models. In Proceedings of the 2020 ACM SIGSAC
conference on computer and communications security, pages 377–
390, 2020.
[106] Congzheng Song and Vitaly Shmatikov. Auditing data provenance
in text-generation models. In Proceedings of the 25th ACM SIGKDD
International Conference on Knowledge Discovery & Data Mining,
pages 196–206, 2019.
[107] Jared
Spataro.
Introducing
microsoft
365
copilot.
https://blogs.microsoft.com/blog/2023/03/16/introducing-microsoft-
365-copilot-your-copilot-for-work/, 2023.
[108] Robin Staab, Mark Vero, Mislav Balunovi´c, and Martin Vechev.
Beyond memorization: Violating privacy via inference with large
language models. arXiv preprint arXiv:2310.07298, 2023.
[109] Sudarsan.
Optimize
Azure
OpenAI
Applications
with
Semantic
Caching.
https://techcommunity.microsoft.com/blog/
azurearchitectureblog/optimize-azure-openai-applications-with-
semantic-caching/4106867, 2024. [Accessed 15-11-2024].
[110] Jingyun Sun, Chengxiao Dai, Zhongze Luo, Yangbo Chang, and
Yang Li. Lawluo: A chinese law firm co-run by llm agents. arXiv
preprint arXiv:2407.16252, 2024.
[111] Talarian.
GPT for Microsoft Excel or Google Sheets.
https:
//gptforwork.com/, 2024.
[112] Talarian. OpenAI GPT prompt generator. https://gptforwork.com/
tools/prompt-generator, 2024.
[113] Chuanyuan Tan, Yuehe Chen, Wenbiao Shao, and Wenliang Chen.
Make a choice! knowledge base question answering with in-context
learning. arXiv preprint arXiv:2305.13972, 2023.


---

[114] Mingtian Tan, Junpeng Wan, Zhe Zhou, and Zhou Li.
Invisible
probe: Timing attacks with pcie congestion side-channel. In 2021
IEEE Symposium on Security and Privacy (SP), pages 322–338.
IEEE, 2021.
[115] Chaofan Tao, Qian Liu, Longxu Dou, Niklas Muennighoff, Zhong-
wei Wan, Ping Luo, Min Lin, and Ngai Wong.
Scaling laws
with vocabulary: Larger models deserve larger vocabularies. arXiv
preprint arXiv:2407.13623, 2024.
[116] Zuoyi Technology. Zuoshouyisheng open platforms. https://open.
zuoshouyisheng.com/, 2024.
[117] Arun James Thirunavukarasu, Darren Shu Jeng Ting, Kabilan Elan-
govan, Laura Gutierrez, Ting Fang Tan, and Daniel Shu Wei Ting.
Large language models in medicine. Nature medicine, 29(8):1930–
1940, 2023.
[118] Shanquan Tian, Shayan Moini, Adam Wolnikowski, Daniel Hol-
comb, Russell Tessier, and Jakub Szefer.
Remote power attacks
on the versatile tensor accelerator in multi-tenant fpgas. In 2021
IEEE 29th Annual International Symposium on Field-Programmable
Custom Computing Machines (FCCM), pages 242–246. IEEE, 2021.
[119] Hugo Touvron, Louis Martin, Kevin Stone, Peter Albert, Amjad
Almahairi, Yasmine Babaei, Nikolay Bashlykov, Soumya Batra,
Prajjwal Bhargava, Shruti Bhosale, et al. Llama 2: Open foundation
and fine-tuned chat models. arXiv preprint arXiv:2307.09288, 2023.
[120] Tao Tu, Shekoofeh Azizi, Danny Driess, Mike Schaekermann,
Mohamed Amin, Pi-Chuan Chang, Andrew Carroll, Charles Lau,
Ryutaro Tanno, Ira Ktena, et al. Towards generalist biomedical ai.
NEJM AI, 1(3):AIoa2300138, 2024.
[121] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit,
Llion Jones, Aidan N Gomez, Łukasz Kaiser, and Illia Polosukhin.
Attention is all you need. Advances in neural information processing
systems, 30, 2017.
[122] Han Wang, Syed Mahbub Hafiz, Kartik Patwari, Chen-Nee Chuah,
Zubair Shafiq, and Houman Homayoun. Stealthy inference attack
on dnn via cache-based side-channel attacks.
In 2022 Design,
Automation & Test in Europe Conference & Exhibition (DATE),
pages 1515–1520. IEEE, 2022.
[123] Wenjin Wang, Yunhao Li, Yixin Ou, and Yin Zhang. Layout and task
aware instruction prompt for zero-shot document image question
answering. arXiv preprint arXiv:2306.00526, 2023.
[124] Zhiruo Wang, Jun Araki, Zhengbao Jiang, Md Rizwan Parvez, and
Graham Neubig. Learning to filter context for retrieval-augmented
generation. arXiv preprint arXiv:2311.08377, 2023.
[125] Tom Warren. These are microsoft’s bing ai secret rules and why it
says it’s named sydney. The Verge, 14, 2023.
[126] Jason Wei, Xuezhi Wang, Dale Schuurmans, Maarten Bosma, Fei
Xia, Ed Chi, Quoc V Le, Denny Zhou, et al.
Chain-of-thought
prompting elicits reasoning in large language models. Advances in
neural information processing systems, 35:24824–24837, 2022.
[127] Lingxiao Wei, Bo Luo, Yu Li, Yannan Liu, and Qiang Xu. I know
what you see: Power side-channel attack on convolutional neural
network accelerators. In Proceedings of the 34th Annual Computer
Security Applications Conference, pages 393–406, 2018.
[128] Roy Weiss, Daniel Ayzenshteyn, Guy Amit, and Yisroel Mirsky.
What was your prompt? a remote keylogging attack on ai assistants.
arXiv preprint arXiv:2403.09751, 2024.
[129] Roy Weiss, Daniel Ayzenshteyn, and Yisroel Mirsky.
What was
your prompt? a remote keylogging attack on AI assistants. In 33rd
USENIX Security Symposium (USENIX Security 24), pages 3367–
3384, Philadelphia, PA, August 2024. USENIX Association.
[130] Qingsong Wen, Jing Liang, Carles Sierra, Rose Luckin, Richard
Tong, Zitao Liu, Peng Cui, and Jiliang Tang.
Ai for education
(ai4edu): Advancing personalized education with llm and adaptive
learning. In Proceedings of the 30th ACM SIGKDD Conference on
Knowledge Discovery and Data Mining, pages 6743–6744, 2024.
[131] Shijie Wu, Ozan Irsoy, Steven Lu, Vadim Dabravolski, Mark Dredze,
Sebastian Gehrmann, Prabhanjan Kambadur, David Rosenberg, and
Gideon Mann. Bloomberggpt: A large language model for finance.
arXiv preprint arXiv:2303.17564, 2023.
[132] Yang Wu, Chenghao Wang, Ece Gumusel, and Xiaozhong Liu.
Knowledge-infused legal wisdom: Navigating llm consultation
through the lens of diagnostics and positive-unlabeled reinforcement
learning. arXiv preprint arXiv:2406.03600, 2024.
[133] Peng Xia, Kangyu Zhu, Haoran Li, Hongtu Zhu, Yun Li, Gang
Li, Linjun Zhang, and Huaxiu Yao. Rule: Reliable multimodal rag
for factuality in medical vision language models.
arXiv preprint
arXiv:2407.05131, 2024.
[134] Guangxuan Xiao, Yuandong Tian, Beidi Chen, Song Han, and Mike
Lewis. Efficient streaming language models with attention sinks.
arXiv preprint arXiv:2309.17453, 2023.
[135] Mengjia Yan, Christopher W Fletcher, and Josep Torrellas. Cache
telepathy: Leveraging shared resource attacks to learn {DNN} ar-
chitectures. In 29th USENIX Security Symposium (USENIX Security
20), pages 2003–2020, 2020.
[136] Li Yang, Qifan Wang, Jingang Wang, Xiaojun Quan, Fuli Feng,
Yu Chen, Madian Khabsa, Sinong Wang, Zenglin Xu, and Dongfang
Liu.
Mixpave: Mix-prompt tuning for few-shot product attribute
value extraction. In Findings of the Association for Computational
Linguistics: ACL 2023, pages 9978–9991, 2023.
[137] Xi Yang, Aokun Chen, Nima PourNejatian, Hoo Chang Shin,
Kaleb E Smith, Christopher Parisien, Colin Compas, Cheryl Martin,
Mona G Flores, Ying Zhang, et al.
Gatortron: A large clinical
language model to unlock patient information from unstructured
electronic health records. arXiv preprint arXiv:2203.03540, 2022.
[138] Yong Yang, Xuhong Zhang, Yi Jiang, Xi Chen, Haoyu Wang, Shoul-
ing Ji, and Zonghui Wang. Prsa: Prompt reverse stealing attacks
against large language models. arXiv preprint arXiv:2402.19200,
2024.
[139] Lu Ye, Ze Tao, Yong Huang, and Yang Li. Chunkattention: Efficient
self-attention with prefix-aware kv cache and two-phase partition.
arXiv preprint arXiv:2402.15220, 2024.
[140] Honggang Yu, Haocheng Ma, Kaichen Yang, Yiqiang Zhao, and
Yier Jin. Deepem: Deep neural networks model recovery through
em side-channel information leakage. In 2020 IEEE International
Symposium on Hardware Oriented Security and Trust (HOST), pages
209–218. IEEE, 2020.
[141] Jiahao Yu, Yuhang Wu, Dong Shu, Mingyu Jin, and Xinyu Xing.
Assessing prompt injection risks in 200+ custom gpts. arXiv preprint
arXiv:2311.11538, 2023.
[142] Yuxuan Yue, Zhihang Yuan, Haojie Duanmu, Sifan Zhou, Jianlong
Wu, and Liqiang Nie. Wkvquant: Quantizing weight and key/value
cache for large language models gains more.
arXiv preprint
arXiv:2402.12065, 2024.
[143] Amir Zandieh, Insu Han, Vahab Mirrokni, and Amin Karbasi. Sub-
gen: Token generation in sublinear time and memory. arXiv preprint
arXiv:2402.06082, 2024.
[144] Yiming Zhang, Nicholas Carlini, and Daphne Ippolito.
Effective
prompt extraction from language models. In First Conference on
Language Modeling, 2024.
[145] Zhenyu Zhang, Ying Sheng, Tianyi Zhou, Tianlong Chen, Lianmin
Zheng, Ruisi Cai, Zhao Song, Yuandong Tian, Christopher R´e, Clark
Barrett, et al.
H2o: Heavy-hitter oracle for efficient generative
inference of large language models. Advances in Neural Information
Processing Systems, 36, 2024.
[146] Zhuosheng Zhang, Aston Zhang, Mu Li, and Alex Smola. Automatic
chain of thought prompting in large language models. arXiv preprint
arXiv:2210.03493, 2022.
[147] Xufeng Zhao, Mengdi Li, Wenhao Lu, Cornelius Weber, Jae Hee
Lee, Kun Chu, and Stefan Wermter. Enhancing zero-shot chain-of-
thought reasoning in large language models through logic. arXiv
preprint arXiv:2309.13339, 2023.


---

[148] Lianmin Zheng, Liangsheng Yin, Zhiqiang Xie, Jeff Huang, Chuyue
Sun, Cody Hao Yu, Shiyi Cao, Christos Kozyrakis, Ion Stoica,
Joseph E Gonzalez, et al. Efficiently programming large language
models using sglang. arXiv preprint arXiv:2312.07104, 2023.
[149] Banghua Zhu, Ying Sheng, Lianmin Zheng, Clark Barrett, Michael I
Jordan, and Jiantao Jiao. On optimal caching and model multiplex-
ing for large model inference.
arXiv preprint arXiv:2306.02003,
2023.
[150] Hanlin Zhu, Banghua Zhu, and Jiantao Jiao.
Efficient prompt
caching via embedding similarity, 2024.
[151] Henry Peng Zou, Vinay Samuel, Yue Zhou, Weizhi Zhang,
Liancheng Fang, Zihe Song, Philip S Yu, and Cornelia Caragea.
Implicitave: An open-source dataset and multimodal llms bench-
mark for implicit attribute value extraction.
arXiv preprint
arXiv:2404.15592, 2024.
