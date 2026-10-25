---
title: 'Best Air-Gapped LLM Stack: Apache Weights on vLLM Beat NVIDIA''s Fee | THE
  D*AI*LY BRIEF'
id: best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- secondary-analysis
- reliability-medium
created: '2026-10-23T17:21:53.787148Z'
updated: '2026-10-23T17:25:06.673060Z'
source: https://www.beri.net/article/best-air-gapped-llm-stack-apache-weights-vllm-vs-nvidia-ai-enterprise-2026
source_domain: www.beri.net
fetched_at: '2026-10-23T17:21:53.761583Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Sept 28 2026 opinionated trade piece (Rajesh Beri, beri.net; reliability
  medium: analyst/SEO-ish, but well-cited to primary docs). Compares DIY vLLM+Apache
  weights, Red Hat AI, NVIDIA AI Enterprise ($4,500/GPU/yr; DLS licence server with
  manual uploads), Gemini on GDC, Cohere North for air-gap. Offline update = docker
  save, pip download, hash-verified removable media, HF_HUB_OFFLINE=1; telemetry kill
  switches; 8-GPU H200 node ~$370k vs hosted gpt-oss-120b ~$3,400/yr at 1B tokens/mo;
  licence clauses (Llama 4, Gemma, NVIDIA Open Model License, Command A CC-BY-NC).'
---

[air-gapped deployment](https://www.beri.net/tag/air-gapped%20deployment)[vLLM](https://www.beri.net/tag/vLLM)[open-weight models](https://www.beri.net/tag/open-weight%20models)[NVIDIA AI Enterprise](https://www.beri.net/tag/NVIDIA%20AI%20Enterprise)[Red Hat AI](https://www.beri.net/tag/Red%20Hat%20AI)[model licensing](https://www.beri.net/tag/model%20licensing)[self-hosted inference](https://www.beri.net/tag/self-hosted%20inference)[Google Distributed Cloud](https://www.beri.net/tag/Google%20Distributed%20Cloud)
# Best Air-Gapped LLM Stack: Apache Weights on vLLM Beat NVIDIA's Fee
For air-gapped LLMs, run Apache-2.0 weights like gpt-oss-120b on vLLM, via Red Hat AI if you need a vendor. NVIDIA AI Enterprise's per-GPU fee is the loser, and an 8-GPU air-gapped node costs 30-40x the hosted API at 1B tokens a month, and even two GPUs cost about 9x.
By Rajesh Beri·September 28, 2026·14 min read
Share:
Illustration generated using AI
**The best air-gapped LLM stack for most defence, health and industrial buyers is Apache-2.0 open weights — gpt-oss-120b, Qwen3 or Mistral Small — served by vLLM.** Buy it through Red Hat AI if you need a support contract; run it yourself if you have a platform team. Pay NVIDIA AI Enterprise's $4,500 per GPU per year only if you are all-NVIDIA and want its pre-optimised containers. Buy Gemini on Google Distributed Cloud only for a closed frontier model with an accreditation someone else paid for. At the workload below, an 8-GPU air-gapped node costs roughly 30-40x the hosted equivalent, and even a minimal two-GPU deployment is around 10x. You air-gap because you are not allowed to call an API, never to save money.  
| **vLLM + Apache weights (DIY)**  | **Red Hat AI (vLLM, supported)**  | **NVIDIA AI Enterprise (NIM)**  | **Gemini on GDC air-gapped**  | **Cohere North (private)**  |  
| --- | --- | --- | --- | --- |  
| Verdict  | **Default if you have a platform team**  | **Default if you need a vendor on the hook**  | Loser for most buyers — fee without a moat  | Only for frontier-quality closed models  | An app layer, not an inference platform  |  
| Software price (checked 2026-09-28)  | $0  | Per physical accelerator, contact sales  | $4,500/GPU/yr; $22,500/GPU perpetual  | Contact sales (seat, token or per-appliance)  | Contact sales  |  
| Models  | Any weights you are licensed to run  | Any vLLM-supported weights  | NVIDIA-packaged NIM profiles  | Gemini only  | Cohere Command family  |  
| Offline update path  |  `docker save`, wheel mirror, weights by hash  | Mirror images to your registry  |  `download-to-cache`, copy cache  | Google-operated appliance  | Vendor-delivered  |  
| Extra thing inside the air gap  | Nothing  | OpenShift, if you use OpenShift AI  | A licence server (DLS) with manual uploads  | Google's hardware and control plane  | Cohere's stack  |  
| Who should NOT pick it  | Teams with no one on call for GPU drivers  | Shops with no OpenShift or RHEL skills  | Mixed-accelerator fleets; anyone budget-bound  | Anyone who wants weights they can inspect  | Anyone who only needs an endpoint  |  
## What does "air-gapped LLM" actually mean for the buyer?
**An air-gapped LLM deployment is a model, its serving runtime and every dependency they touch running on a network with no route to the internet — no API calls, no weight downloads, no telemetry, no licence check-in.** That last clause is where most stacks fail. The model is the easy part. The runtime, the Python packages, the tokenizer, the embedding model and the licence server all assume egress by default.
One practitioner blueprint puts it bluntly: teams going through the exercise typically discover "[five to fifteen distinct egress dependencies they did not know they had](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment)" — and one it singles out is a retrieval pipeline quietly sending text to a hosted embedding API. Treat that as a practitioner's estimate, not a measured industry figure. It matches what anyone who has run `tcpdump` on a "sealed" GPU node will recognise.
So the comparison below is not "which model is smartest." It is: **which stack leaves the fewest things inside your perimeter that want to phone home, and the cheapest path to getting a new model across the gap.**
## Which model weights are actually licensed for an air gap?
**Pick weights under Apache 2.0, because every other licence in this category carries a clause a lawyer in a regulated buyer will want to talk about.** The permissive short-list today:
  * **gpt-oss-120b and gpt-oss-20b** — [Apache 2.0, per OpenAI's repository](https://github.com/openai/gpt-oss), plus a one-line usage policy that asks only for compliance with applicable law; and the 120b model fits on "a single 80GB GPU" while the 20b runs within 16GB.
  * **Qwen3-235B-A22B** — [Apache 2.0 on its model card](https://huggingface.co/Qwen/Qwen3-235B-A22B).
  * **Mistral Small 3.2 (24B)** — [Apache 2.0](https://huggingface.co/mistralai/Mistral-Small-3.2-24B-Instruct-2506), needing about 55GB of GPU memory in bf16.


The steel-man for the others is real: Llama, Gemma and Nemotron are good models and plenty of enterprises run them. But read what you are signing.
  * **Llama 4** incorporates Meta's Acceptable Use Policy "[by reference into this Agreement](https://dev.meta.ai/llama/llama4/license/)", requires you to display "Built with Llama", and sends anyone over 700 million monthly active users back to Meta for a separate licence. For most buyers the MAU cap is irrelevant; the by-reference policy is a document that can change after you have certified your system.
  * **Gemma** 's terms say Google "[reserves the right to restrict (remotely or otherwise)](https://ai.google.dev/gemma/terms)" usage it believes violates the agreement, and also pull in a prohibited-use policy by reference. In an air gap "remotely" is moot — but your accreditor will still ask what it means.
  * **NVIDIA's Open Model License** is commercial and royalty-free, but [revocable, and your rights "automatically terminate"](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) if you bypass or reduce the efficacy of a model's safety guardrail without a substantially similar replacement. A fine-tuning team in a defence programme needs to know that clause exists before it starts.
  * **Cohere's Command A** open weights are [CC-BY-NC — non-commercial](https://huggingface.co/CohereLabs/c4ai-command-a-03-2025). The download is a research artefact. Commercial use in your air gap means buying Cohere's private deployment.


The rule: **the licence you can hand to an accreditor without a cover memo is Apache 2.0.** Everything else is workable but costs you a legal review per model version.
## How much hardware does each concurrency target need?
**The memory floor is set by the model; the concurrency ceiling can only be set by load-testing your own prompts — do not trust anyone's tokens-per-second table, including this one.** What you can plan from:
  * **Pilot, up to ~10 concurrent users.** One published [reference build on a Dell R740 with two 24GB Turing-era GPUs](https://github.com/wu840407/enterprise-airgapped-llm) served a 4-bit Qwen3-Coder-30B-A3B at 80-120 tokens/s for one user and about 30 tokens/s per user at 10 concurrent, with time-to-first-token under 500ms warm. That is a 2018-era server. A pilot does not need Blackwell.
  * **Department, dozens of concurrent users on a frontier-class open model.** gpt-oss-120b's single-80GB-GPU footprint means one H100/H200 per replica, and you want at least two replicas so a driver update does not take the service down. Plan for 2-4 GPUs.
  * **Enterprise, hundreds of concurrent users or long contexts.** This is where buyers end up at an 8-GPU HGX node, which resellers put at [$320,000-$420,000 for an H200 system, typically ~$370,000](https://www.mercatus-ai.com/blog/h200-server-price) in 2026 — a reseller's estimate.


Two things change the answer. Mixture-of-experts models load _all_ their weights even when few are active, so size memory on total parameters, not active ones ([we worked through that trap here](https://www.beri.net/article/tencent-hy4-moe-total-vs-active-parameters-gpu-memory-sizing)). And old hardware costs engineering time: the R740 build had to force float16, avoid FP8 and run `--enforce-eager` because Turing lacks BF16 — each one a day someone spent reading stack traces offline.
## How do you patch and update with no internet?
**Every update becomes a release: download on a connected machine, verify, carry it across on approved media, re-validate, promote.** The stacks differ only in how many artefacts you carry and whether the vendor has scripted it.
  * **vLLM DIY.** The reference build's procedure is the honest template: pull everything while connected, then `docker save` the images, `pip download` the wheels, tar the configs, and [move them across on removable media with hash verification](https://github.com/wu840407/enterprise-airgapped-llm). Set `HF_HUB_OFFLINE=1`, which makes the Hugging Face libraries [make no HTTP calls and read only cached files](https://huggingface.co/docs/huggingface_hub/package_reference/environment_variables). Mirror your weights into a private registry rather than a shared folder ([our registry comparison](https://www.beri.net/article/hugging-face-alternatives-private-model-registry-air-gapped-2026) covers that).
  * **Red Hat AI.** OpenShift AI has a [documented disconnected install](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/2.25/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/installing-the-single-model-serving-platform_component-install): mirror the operator and serving images to your internal registry, and serve models through KServe. It is the same vLLM, packaged with a lifecycle you can put in a change ticket.
  * **NVIDIA AI Enterprise.** NIM's [air-gap guide](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html) has you run `download-to-cache` for a specific model profile on a connected system, copy the cache across, or point `NIM_MODEL_PATH` at a local directory. Profiles are tuned per GPU, so a hardware change can mean a new download. And the licence itself needs infrastructure: the [Delegated License Service](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html) is "fully disconnected from the NVIDIA Licensing Portal", so you download licences and upload them manually. That is one more appliance inside your perimeter and one more thing that can expire.
  * **Gemini on GDC.** Google operates the stack. That removes the update engineering and replaces it with a dependency on Google's release cadence for your model.


Budget for size. Model artefacts run [10-400GB each](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment), and the same blueprint's warning is the one to pin on the wall: air-gapped releases "are slower by design", and teams find out when a regulator asks for the change-control evidence.
## How do you evaluate and monitor with no telemetry egress?
**Turn off every default phone-home, then build the eval loop entirely inside the gap — including the judge model.**
Start with the defaults. vLLM [collects anonymous usage data by default](https://docs.vllm.ai/en/latest/usage/usage_stats.html) — hardware, model architecture, runtime settings — and it is disabled with `VLLM_NO_USAGE_STATS=1` or `DO_NOT_TRACK=1`. The Hugging Face libraries send telemetry too, disabled with [`HF_HUB_DISABLE_TELEMETRY=1`](https://huggingface.co/docs/huggingface_hub/package_reference/environment_variables). In an air gap these fail rather than leak, but failing calls cost timeouts, and a node that is ever briefly connected for patching will send them. Set them in the base image.
Then the monitoring stack. Hosted observability does not survive isolation; the [practitioner pattern](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment) is self-hosted Prometheus, Grafana and Loki, with the logs classified at the same level as the inference cluster — because the logs contain the prompts.
Evaluation is the underestimated piece. An **offline eval** is a fixed test set, a harness and a judge that all run inside the perimeter. Bake the benchmark data into the harness rather than pulling it at runtime, and run a smaller local judge model calibrated against your human graders. The long pole, per the same blueprint, is ground-truth annotation on on-prem tools — start that before the hardware arrives. Every model update then reruns the same suite before promotion, which is the change-control evidence your auditor wants anyway.
## What does air-gapping cost versus the hosted equivalent?
**Roughly 30-40x more on an enterprise node, and still around 10x on the smallest redundant deployment, before you count a single engineer — which is why cost cannot be the reason you do it.**
The defined workload: **gpt-oss-120b, 1 billion tokens a month (700M input, 300M output), on one 8-GPU H200 node.** That node is sized for peak concurrency and headroom, not the average: 1 billion tokens a month is under 400 tokens a second on average, which two GPUs can serve. Right-sized to two GPUs — roughly a quarter of the node, ~$31,000 a year — the gap narrows to about 9x. It does not close.
  * **Hosted.** Together AI lists gpt-oss-120b at [$0.15 per million input tokens and $0.60 per million output tokens](https://www.together.ai/pricing) (checked 2026-09-28). That is $105 + $180 = **$285 a month, about $3,400 a year.**
  * **Air-gapped hardware.** A ~$370,000 H200 node straight-lined over three years is **~$123,000 a year** , before power, cooling, spares and rack space.
  * **Air-gapped software.** Free for DIY vLLM. Red Hat is per physical accelerator and [unpriced publicly](https://www.redhat.com/en/resources/ai-subscription-guide-detail) — get a quote. NVIDIA AI Enterprise lists [$4,500 per GPU for one year, $18,000 for five, or $22,500 perpetual with five years of support](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html) — **$36,000 a year for this node.**


Look at that last line against the hosted bill. At ten times the volume — 10 billion tokens a month — hosted gpt-oss-120b costs about $34,200 a year. **The NVIDIA licence alone, for one node, costs more than the entire hosted service at 10x the workload.** And that is before the people: GPU driver lifecycles, offline release engineering, the eval pipeline and the on-call rotation.
The working title of this piece was that air-gapped LLMs cost more in operations than they save in licences. The numbers say it is worse: there are no licence savings to set against the operations bill, because the hosted open-weight API was never expensive. The only lever you control is the recurring software line — which is why the verdict pushes that line to zero or near it.
## Where each option wins, and who should not buy it
**vLLM with Apache weights, self-run.** The default for any team that already runs GPUs. vLLM is also [our default inference runtime](https://www.beri.net/article/vllm-vs-tensorrt-llm-vs-sglang-inference-runtime-2026) outside air gaps, so you are not maintaining an exotic stack. **Do not pick it** if nobody on staff will own GPU drivers, CUDA versions and a pip mirror at 02:00 with no internet to search.
The same vLLM with a support contract, a documented disconnected installer and a subscription counted [per physical accelerator — eight GPUs, eight subscriptions](https://www.redhat.com/en/resources/ai-subscription-guide-detail); OpenShift AI additionally needs base OpenShift subscriptions. For a hospital or a defence integrator whose procurement rules require a named vendor, this is the pick. **Do not pick it** if you have no OpenShift or RHEL skills — you will be buying a platform to host an endpoint.
**NVIDIA AI Enterprise (NIM).** The steel-man: pre-optimised containers per GPU, one throat to choke on an all-NVIDIA estate, and a scripted air-gap procedure. **This is the loser for most buyers.** It charges the largest published recurring fee in the comparison for packaging open runtimes and open weights you can mirror yourself, and it adds a licence server with manual uploads to the one environment where every extra component is a liability. **Do not pick it** if you run mixed accelerators, if your budget is fixed, or if your accreditor counts appliances.
**[Gemini](https://www.beri.net/tools/gemini) on Google Distributed Cloud air-gapped.** Google made Gemini on GDC air-gapped [generally available in August 2025](https://www.googlecloudpresscorner.com/2025-08-28-Google-Cloud-Makes-Gemini-Everywhere-Vision-a-Reality,-Doubles-Down-on-Enterprise-AI-Commitment-to-Singapore), says the product is [authorised for US Government Secret and Top Secret missions](https://cloud.google.com/blog/products/ai-machine-learning/run-gemini-and-ai-on-prem-with-google-distributed-cloud), and at Next '26 partner Cirrascale announced a [single-server Dell appliance with eight NVIDIA GPUs](https://venturebeat.com/technology/googles-gemini-can-now-run-on-a-single-air-gapped-server-and-vanish-when-you-pull-the-plug), priced by seat, by token or flat per appliance — contact sales. It is the only way to get a closed frontier model behind a gap. **Do not pick it** if you need weights you can inspect, fine-tune or keep after the contract ends; the model leaves when the appliance does.
**[Cohere North](https://www.beri.net/tools/cohere-north).** Cohere offers [air-gapped deployment behind your firewall](https://cohere.com/private-deployments), and North was launched as runnable on [as few as two GPUs](https://techcrunch.com/2025/08/06/coheres-new-ai-agent-platform-north-promises-to-keep-enterprise-data-secure/). It is an agent-and-search application with models attached, priced through sales. **Do not pick it** if what you need is an OpenAI-compatible endpoint for your own applications — you would be buying an application to get a model whose open weights you [cannot use commercially](https://huggingface.co/CohereLabs/c4ai-command-a-03-2025).
## The decision: criteria that predict regret
**The buyers who regret an air-gapped LLM purchase almost always under-weighted operations and over-weighted the model.** Score these before you sign:
  1. **Can you name the person who carries the next model across the gap?** If not, buy Red Hat or Google, not DIY.
  2. **Does the licence survive your accreditor without a memo?** Apache 2.0 does. Anything with a policy incorporated by reference costs a legal review per version.
  3. **How many components phone home by default?** Count them in a connected staging environment with egress logging on. Every one you find there is one you will not find in production.
  4. **What is your recurring software line per GPU?** At air-gap volumes it is the biggest cost you can still change.
  5. **Do you need a closed frontier model, or just a good one?** If an open 120B model passes your offline eval, the Gemini appliance is a premium for a gap you have not measured.


What changes the answer: a mandate for a closed frontier model (Gemini on GDC); a procurement rule requiring a single hardware-and-software vendor on an all-NVIDIA estate (NIM becomes defensible); or a use case that is really search-and-agents over internal documents with no engineering team (North).
**This Week:** stand up gpt-oss-20b on one 16GB GPU in a staging enclave with egress logging on, and write down every blocked connection.
**This Month:** build the offline eval suite — 200 real prompts, graded by your people — and run gpt-oss-120b, Qwen3 and one paid option against it.
**Before Budget Close:** get a Red Hat per-accelerator quote and an NVIDIA AI Enterprise quote for the same node, and put both next to the $0 line.
## The Bottom Line
Air-gapped AI repeats the pattern of every sovereign-IT cycle before it: the hardware is a one-time decision, and the software subscription and the release process are the cost that compounds. The hosted open-weight API is cheap enough that nothing inside your perimeter will ever beat it on price — so stop trying, and make the perimeter as cheap to operate as possible. Apache weights, vLLM, a support contract only if your rules require one.
The gap is the requirement. The fee is optional.
## Continue Reading
  * [Hugging Face Alternatives: Cache the Hub, Sign at the Door](https://www.beri.net/article/hugging-face-alternatives-private-model-registry-air-gapped-2026)
  * [vLLM vs TensorRT-LLM vs SGLang: Default to vLLM](https://www.beri.net/article/vllm-vs-tensorrt-llm-vs-sglang-inference-runtime-2026)
  * [Tencent Says 49B Active. Your Node Loads All 770B.](https://www.beri.net/article/tencent-hy4-moe-total-vs-active-parameters-gpu-memory-sizing)
  * [7 OpenAI Alternatives. Only 3 Clear a Sovereignty Rule.](https://www.beri.net/article/openai-alternatives-data-sensitive-workloads-2026)
  * [Best MLOps for Regulated AI: Domino, Then a Sign-Off Layer](https://www.beri.net/article/best-mlops-platforms-regulated-industries-model-risk-2026)
  * [Self-Host the Vector DB for Residency. Not for the Bill.](https://www.beri.net/article/self-hosted-vector-database-alternatives-true-cost-2026)
  * [NVIDIA's Kumo Tabular Undercuts TabPFN's License Below 60,000 Rows](https://www.beri.net/article/nvidia-kumo-tabular-open-tabular-foundation-model-tabarena-commercial-license-vs-tabpfn-3-5)
  * [Cursor Alternatives That Survive Security Review Start With Copilot](https://www.beri.net/article/cursor-alternatives-enterprise-security-review-data-path-copilot-claude-code)


Share:
## Frequently Asked Questions
### Which open-weight models are licensed for air-gapped commercial use?
The cleanest are Apache 2.0 models: gpt-oss-120b and gpt-oss-20b, Qwen3-235B-A22B and Mistral Small 3.2. Llama 4 and Gemma incorporate usage policies by reference, NVIDIA's Open Model License terminates if you circumvent guardrails, and Cohere's Command A open weights are non-commercial (CC-BY-NC).
### How much does NVIDIA AI Enterprise cost for an air-gapped deployment?
NVIDIA lists AI Enterprise at $4,500 per GPU for one year, $18,000 per GPU for five years, or $22,500 per GPU perpetual with five years of support (checked 2026-09-28). An 8-GPU node is $36,000 a year, and air-gapped sites also run a Delegated License Service with manually uploaded licences.
### Is running an LLM air-gapped cheaper than a hosted API?
No. Hosted gpt-oss-120b costs $0.15 per million input and $0.60 per million output tokens on Together AI, about $3,400 a year at 1 billion tokens a month. An 8-GPU H200 node amortised over three years is roughly $123,000 a year before power and staff.
### How do you update an air-gapped LLM without internet access?
Treat each update as a release: download images, wheels and weights on a connected machine, verify hashes, carry them across on approved media, re-run your offline eval suite, then promote. NIM uses download-to-cache; OpenShift AI mirrors images to an internal registry; DIY vLLM uses docker save and HF_HUB_OFFLINE=1.
### Does vLLM send telemetry by default?
Yes. vLLM collects anonymous usage data such as hardware, model architecture and runtime settings by default. Disable it with VLLM_NO_USAGE_STATS=1 or DO_NOT_TRACK=1, and set HF_HUB_DISABLE_TELEMETRY=1 for the Hugging Face libraries, in the base image.
Newsletter
## More on Models & LLMs, every Sunday
The week's enterprise AI news, Models & LLMs included, in one Sunday email.
### Mentioned Tools
#### [Cohere North Privately deployable enterprise AI workspace for agents, search and workflow automation — run it in your VPC, on-prem or air-gapped ](https://www.beri.net/tools/cohere-north)#### [Gemini Google's frontier model family, sold to enterprises as a metered API, a per-seat agent workspace (Gemini Enterprise) and a full agent-building platform. ](https://www.beri.net/tools/gemini)#### [Red Hat AI Run any model on any accelerator across hybrid cloud, with safety evidence and cost attribution built in ](https://www.beri.net/tools/red-hat-ai)
### Related Articles
[self-hosted inferenceSelf-Hosted gpt-oss-120b on H100 Costs 2x the API at 9-to-5 Load An H100 running gpt-oss-120b at Lambda's $4.29 an hour must sustain 5,958 tokens per second every hour of the month to match a $0.20 blended API rate. Business-hours traffic leaves it at about twice the API's cost. October 23, 2026](https://www.beri.net/article/self-hosted-llm-vs-api-break-even-gpt-oss-120b-h100-b200-deepseek-v4-utilization-2026)[Chinese open-weight alternativesKimi and Qwen Alternatives: Meta's Muse Trails Kimi K3 by 5 Points Muse Spark Open is the closest US replacement for Kimi K3, five index points behind and cheaper per task. Defense, nuclear and critical-infrastructure buyers should pick Apache 2.0 Reflection Beam instead. October 19, 2026](https://www.beri.net/article/kimi-qwen-alternatives-us-open-weights-muse-spark-open-reflection-beam-gpt-oss-chinese-origin-policy-2026)[Hex-RaysHex-Rays Buys HexAI and Ships Its New AI Tools Outside IDA Hex-Rays, maker of IDA Pro, is buying HexAI (formerly Ventris) for a reported $30M-$35M. Its new AI research tools are currently available only through HexAI, so IDA customers should settle model choice, air-gap support and pricing before they renew. October 18, 2026](https://www.beri.net/article/hex-rays-hexai-ventris-acquisition-ida-pro-ai-security-research-agents-air-gapped-model-agnostic-customer-continuity)[DeepSeek V4.1 FlashDeepSeek's $0.75 Search API Undercuts Tavily From Servers in China DeepSeek's new Search & Fetch API costs $0.75 per 1,000 calls, about a tenth of Tavily's basic search, with billing from October 31. Queries go to a service that stores data in China under PRC-governed terms. October 17, 2026](https://www.beri.net/article/deepseek-v4-1-flash-1017-search-fetch-api-0-75-per-1000-mit-weights-deepswe-vs-tavily-exa-brave-data-path)
### Latest Articles
[View All →](https://www.beri.net/articles)
#### [Self-Hosted gpt-oss-120b on H100 Costs 2x the API at 9-to-5 Load October 23, 2026](https://www.beri.net/article/self-hosted-llm-vs-api-break-even-gpt-oss-120b-h100-b200-deepseek-v4-utilization-2026)#### [Claude Duet Ships on Pro and Max With No Admin Console for IT October 23, 2026](https://www.beri.net/article/claude-duet-launch-pro-max-research-preview-no-team-enterprise-admin-controls-personal-account-connectors-vs-openai-dots)#### [OpenAI on Google Cloud Draws Down Commits Without the FSP Discount October 22, 2026](https://www.beri.net/article/openai-gpt-6-astra-sol-luna-google-cloud-model-garden-private-preview-commit-drawdown-no-fsp-discount-zdr-coming-soon)#### [Microsoft 365 Price Increase: Negotiate From Last Year's Bill October 22, 2026](https://www.beri.net/article/microsoft-365-google-workspace-salesforce-ai-bundle-price-increase-renewal-negotiation-2026)
