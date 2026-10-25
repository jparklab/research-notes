---
title: Reducing Cold Start Latency for LLM Inference with NVIDIA Run:ai Model Streamer
  | NVIDIA Technical Blog
id: reducing-cold-start-latency-for-llm-inference-with-nvidia-runai-model-streamer-n
tags:
- internal-llm-inference-stacks-7f0c90
- cold-starts
- weight-loading
- benchmark
- fabrication-check
created: '2026-10-09T23:06:44.886339Z'
updated: '2026-10-10T14:52:17.217301Z'
source: https://developer.nvidia.com/blog/reducing-cold-start-latency-for-llm-inference-with-nvidia-runai-model-streamer/
source_domain: developer.nvidia.com
fetched_at: '2026-10-09T23:06:44.867074Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'NVIDIA Developer Blog primary engineering post (Sep 16, 2025; Omer Dayan,
  Noa Neria, Ekin Karabulut, all ex-Run:ai/now NVIDIA) introducing and benchmarking
  NVIDIA Run:ai Model Streamer (github.com/run-ai/runai-model-streamer), an open-source
  Python SDK with a C++ backend that concurrently reads model weights from storage
  and streams them directly into GPU memory, bypassing the sequential read-to-CPU-then-copy-to-GPU
  path. PROVENANCE CORRECTION: the secondary dreaming.press article''s claim that
  Model Streamer ''sustains around 80 Gbps'' is NOT substantiated anywhere in this
  primary source -- this post reports only GiB/s storage-throughput figures (~1 GiB/s
  on GP3 SSD, ~2 GiB/s observed on IO2 SSD, close to but below its 4 GiB/s theoretical
  max) and absolute load times in seconds, never a Gbps figure; treat the 80 Gbps
  claim as unverified/likely fabricated by the secondary source. Benchmark setup:
  Llama 3 8B (15GB, single safetensors file) on an AWS g5.12xlarge (1x A10G GPU used),
  vLLM 0.5.5, Model Streamer 0.6.0, Tensorizer 2.9.0, compared against the default
  HF Safetensors Loader and CoreWeave Tensorizer across three storage tiers. Concrete
  results: on GP3 SSD, raising concurrency from 1 to 16 cut Model Streamer''s load
  time from 47.56s to 14.34s (saturating GP3''s ~1GiB/s ceiling, matched by Tensorizer
  at 16.11s/16 workers, both ~3.3x faster than Safetensors'' 47.99s baseline); on
  IO2 SSD, Model Streamer hit 7.53s at concurrency 8 (~6x faster than Safetensors''
  47s), Tensorizer 10.36s at 8 workers; on Amazon S3 (which the default Safetensors
  Loader cannot read at all), Model Streamer reached 4.88s at concurrency 32 vs Tensorizer''s
  best of 37.36s at 16 workers -- Model Streamer''s largest relative advantage is
  specifically on S3, where it is the only loader with native object-store support.
  End-to-end vLLM engine-ready time (not just weight load) showed Model Streamer roughly
  halving total readiness versus Safetensors: 35.08s vs 66.13s on GP3, 28.28s vs 62.69s
  on IO2, and 23.18s on S3 vs Tensorizer''s 65.18s. Notes a measurement artifact:
  repeated S3 benchmarks within the same session showed anomalously fast ''warm cache''
  results, so the authors enforced a 3-minute cooldown between S3 runs to preserve
  genuine cold-start conditions. Key architectural features: multi-threaded concurrent
  reading with balanced workload distribution across threads, native Safetensors format
  compatibility (no weight conversion/re-serialization step required, unlike Tensorizer
  which needs its own serialization step), and direct integration as a loader option
  inside vLLM and TGI.'
---

*Suggested by [[scale-to-zero-for-llm-inference-why-cold-starts-are-a-weight-loading-problem]] — cited as primary source for Run:ai Model Streamer ~80Gbps benchmark*

[Agentic AI / Generative AI](https://developer.nvidia.com/blog/reducing-cold-start-latency-for-llm-inference-with-nvidia-runai-model-streamer/)
English 中文
# Reducing Cold Start Latency for LLM Inference with NVIDIA Run:ai Model Streamer
Sep 16, 2025 
By [Omer Dayan](https://developer.nvidia.com/blog/author/omdayan/ "Posts by Omer Dayan"), [Noa Neria](https://developer.nvidia.com/blog/author/nneria/ "Posts by Noa Neria") and [Ekin Karabulut](https://developer.nvidia.com/blog/author/ekarabulut/ "Posts by Ekin Karabulut")
Like 


## AI-Generated Summary
  * [NVIDIA Run:ai Model Streamer](https://github.com/run-ai/runai-model-streamer) is an open source Python SDK that concurrently reads model weights from storage and streams them directly into GPU memory to reduce cold start latency.
  * Benchmarks on AWS g5.12xlarge instances with a Llama 3 8B model show Model Streamer loading the 15 GB model in 14.34 seconds on GP3 SSD at concurrency 16, 7.53 seconds on IO2 SSD at concurrency 8, and 4.88 seconds on Amazon S3 at concurrency 32.
  * Model Streamer outperforms the default Hugging Face Safetensors Loader and CoreWeave Tensorizer across local SSDs and cloud storage, with the largest gap on S3 where Safetensors Loader lacks support.
  * Integration with vLLM cuts total engine readiness time roughly in half on local SSDs — 35.08 seconds versus 66.13 seconds on GP3 and 28.28 seconds versus 62.69 seconds on IO2 — and reaches 23.18 seconds on S3 compared to Tensorizer's 65.18 seconds.
  * Key features include multi-threaded concurrent reading with balanced workload distribution, support for SSDs, remote storage, and cloud object stores, native Safetensors compatibility without format conversion, and a Python API that integrates with inference engines like vLLM and TGI.


### Next Steps
  * Explore the [Model Streamer documentation](https://github.com/run-ai/runai-model-streamer/tree/master/docs) for setup and usage details.
  * Boost model loading performance with the [NVIDIA Run:ai Model Streamer](https://github.com/run-ai/runai-model-streamer) to reduce cold-start latency and accelerate time-to-inference.


Powered by NVIDIA Nemotron. AI-generated content may summarize information incompletely. Verify important information. [Learn more](https://www.nvidia.com/en-us/agreements/trustworthy-ai/terms/)
Deploying [large language models (LLMs)](https://www.nvidia.com/en-us/glossary/large-language-models/) poses a challenge in optimizing inference efficiency. In particular, cold start delays—where models take significant time to load into GPU memory—can impact user experience and scalability. Increasingly, complex production environments highlight the need for efficient model loading. These models often require tens to hundreds of gigabytes of memory, causing latency and resource challenges when scaling to meet unpredictable demand. Cold start delays impact end-user experience and operational efficiency.
This post introduces the [NVIDIA Run:ai Model Streamer](https://github.com/run-ai/runai-model-streamer), an open source Python SDK designed to mitigate these issues by concurrently reading model weights from storage and streaming them directly into GPU memory. We benchmarked it against the vLLM default [Hugging Face (HF) Safetensors Loader](https://huggingface.co/docs/safetensors/index) and [CoreWeave Tensorizer](https://www.coreweave.com/blog/coreweaves-tensorizer-decrease-pytorch-model-load-times) on local SSDs and Amazon S3.
The experiments explained in this post show that the NVIDIA Run:ai Model Streamer significantly reduces model loading times, lowering cold start latency even in cloud environments. It also remains compatible with the Safetensor format, avoiding weight conversion. Our findings emphasize storage choice and concurrent streaming for efficient LLM deployment. Specifically, to improve inference performance, use the NVIDIA Run:ai Model Streamer to reduce cold-start latency, saturate your storage throughput, and accelerate time-to-inference. 
## How is a model loaded to a GPU for inference? 
To provide some background information, this section explains the two main steps involved in loading a machine learning model into GPU memory for inference: reading weights from storage into CPU memory, and transferring them to the GPU. Understanding this process is key to optimizing inference latency, especially in large-scale or cloud-based deployments.
  * **Reading weights from storage to CPU memory:** The model’s weights are loaded from storage into CPU memory. Weights can be in various formats such as .pt, .h5, and .safetensors, or in custom formats; storage can be local, cluster-wide, or in the cloud. Note that the .safetensors format is used for the purposes of this post due to its wide adoption. However, other formats may be used elsewhere.
  * **Moving the model to GPU:** The model’s parameters and relevant tensors are transferred to GPU memory.


Loading models from cloud-based storage such as Amazon S3 often involves an extra step: the weights are first downloaded to local disk before being moved into CPU and then GPU memory. 
Traditionally, these steps occur sequentially, making model loading times one of the most significant bottlenecks when scaling inference. 
### How does the Model Streamer work? 
Model Streamer is an SDK with a high-performance C++ backend designed to accelerate model loading into GPUs from various storage sources (for example, network file systems, cloud, local disks, and so on). It uses multiple threads to read tensors concurrently from a file in object or file storage, to a dedicated buffer in the CPU memory. Each tensor has an identifier, enabling simultaneous reading and transfer: while some tensors are read from storage to CPU, others are moved from CPU to GPU.
The tool takes full advantage of the fact that GPU and CPU have separate subsystems. GPUs access CPU memory directly through PCIe without CPU intervention, allowing real-time overlap of storage reads and memory transfers. Experiments were run on an AWS g5.12xlarge instance with NVIDIA A10G GPUs and 2nd Gen AMD EPYC CPUs—offering balanced architecture for high-throughput parallel data handling.
Key features of the Model Streamer include:
  * **Concurrency:** Multiple threads read model weight files in parallel, including support for splitting large tensors.
  * **Balanced workload for reading:** Work is distributed based on tensor size to saturate storage bandwidth.
  * **Support for multiple storage types:** Works with SSDs, remote storage, and cloud object stores like S3.
  * **No tensor format conversion:** Supports Safetensors natively, avoiding conversion overhead.
  * **Easy integration:** Offers a Python API and an iterator similar to Safetensors but with concurrent background reading. Integrates easily with inference engines like vLLM and TGI.


For more details about setup and usage, see the [Model Streamer documentation](https://github.com/run-ai/runai-model-streamer/tree/master/docs).
### How does the HF Safetensors Loader work? 
The [HF Safetensors Loader](https://huggingface.co/docs/safetensors/index) is an open source utility that provides a safe and fast format for saving and loading multiple tensors. It uses a memory-mapped file system to minimize data copying. On a CPU, tensors are directly mapped into memory. On a GPU, it creates an empty tensor with PyTorch, then moves the tensor data using cudaMemcpy, facilitating a zero-copy loading process.
### How does the CoreWeave Tensorizer work? 
[CoreWeave Tensorizer](https://www.coreweave.com/blog/coreweaves-tensorizer-decrease-pytorch-model-load-times) is an open source tool that serializes model weights and their corresponding tensors into a single file. Instead of loading an entire model into RAM before moving it to the GPU, Tensorizer streams the model data tensor by tensor from an HTTP/HTTPS or S3 source. 
### Where loading meets inference engines: Loading weights with vLLM 
Model serving is not complete without an inference engine. There are many inference engines and servers that one can utilize. This post focuses on vLLM and its model loading capabilities. For the benchmarking study, we only utilized vLLM. 
The vLLM framework uses the HF safetensors model loading as default. Additionally, it supports CoreWeave Tensorizer to load models from S3 endpoints. However, note that the Tensorizer library requires converting weights from safetensors format to tensorizer format. 
## Comparing model loader performance across three storage types 
We compared the performance of different model loaders (NVIDIA Run:ai Model Streamer, CoreWeave Tensorizer, and HF Safetensors Loader) across three storage types:
  1. **Experiment #1: GP3 SSD** – Measured model loading times with various loaders. 
  2. **Experiment #2: IO2 SSD** – Tested the same loaders on IO2 SSD to evaluate the impact of higher IOPS and throughput. 
  3. **Experiment #3: Amazon S3** – Compared loaders in a cloud storage; Safetensors Loader excluded as it does not support S3. 
  4. **Experiment #4: vLLM with different loaders** – Integrated Model Streamer into vLLM to measure full load and readiness times across storage types, comparing it to default HF Safetensors Loader and Tensorizer. Safetensors Loader excluded from S3 tests. 


All tests ran under cold-start conditions to avoid cache effects. For S3, a minimum two-minute wait between tests ensured accuracy. Tensorizer experiments used models serialized per the [**Tensorizer recipe**](https://github.com/coreweave/tensorizer/blob/main/examples/serialize.py), and benchmarking followed their [**benchmarking recipe**](https://github.com/coreweave/tensorizer/tree/main/examples/benchmark), both without optional hashing.
### Experiment setup 
The experiments were conducted using the setup outlined in Table 1.   
| **Model** |  
| --- |  
| Llama 3 8B, an LLM weighing 15 GB, stored in a single Safetensors format |  
| **Hardware** |  
| AWS g5.12xlarge instance featuring four NVIDIA A10G GPUs (only one GPU was used for all tests to maintain consistency) |  
| **Software stack** |  
| CUDA 12.4vLLM 0.5.5 (Transformers 4.44.2)NVIDIA Run:ai Model Streamer 0.6.0Tensorizer 2.9.0Transformers 4.45.0.dev0Accelerate 0.34.2 |  
| **Storage types** |  
| GP3 SSD: 750 GB, 16K IOPS, 1,000 MiB/sIO2 SSD: 500 GB, 100K IOPS, 4,000 MiB/sAmazon S3: Same AWS region as instance to minimize latency |  
_Table 1. Summary of experimental setup_
For the experiments involving Tensorizer, the same model was serialized into the Tensorizer proprietary tensor format using the recipe provided by the Tensorizer framework. 
### Experiment #1 results: GP3 SSD 
In this initial experiment, we compared the loading performance of different model loaders using GP3 SSD storage. We evaluated the impact of concurrency on the Model Streamer (Figure 1) and examined how the number of workers affected Tensorizer. For Model Streamer, increasing concurrency—the number of concurrent threads reading from storage into CPU memory—led to a notable decrease in model loading time.
At concurrency 1, Model Streamer loaded the model in 47.56 seconds, slightly slower than HF Safetensors Loader at 47.99 seconds. With concurrency 16, loading time dropped to 14.34 seconds, maintaining throughput of ~1 GiB/s, the max for GP3 SSD. Beyond that, storage throughput limited further gains.
Tensorizer showed similar behavior. With one worker, loading time was 50.74 seconds, close to Safetensors Loader. With 16 workers, it achieved 16.11 seconds and 984.4 MiB/s throughput—also nearing GP3 SSD bandwidth.
The storage throughput limit of GP3 SSD became the bottleneck for both Model Streamer and Tensorizer, limiting performance. This motivated testing a higher-throughput storage solution in Experiment #2.  
| **Model Streamer**  | **Safetensors Loader**  |  
| --- | --- |  
| **Concurrency**  | **Time to load model to GPU (sec.)**  | **Time to load model to GPU (sec.)**  |  
| 47.56  | 47.99  |  
| 14.43  |  
| 14.42  |  
| 14.34  |  
_Table 2. Experiment #1 GP3 SSD results_  
| **Tensorizer** |  
| --- |  
| **Number of readers**  | **Time to load model to GPU (sec.)**  |  
| 50.74  |  
| 17.38  |  
| 16.49  |  
| 16.11  |  
| 17.18  |  
| 16.44  |  
| **100**  | 16.81  |  
_Table 3. Experiment #1 Tensorizer results_
__Figure 1. Higher concurrency significantly reduces model loading time, reaching peak SSD throughput at 16 streams__
_Figure 2. Model Streamer and Tensorizer achieve faster model loading than Safetensors on AWS GP3 SSD_
### Experiment #2: IO2 SSD 
For the second experiment, we used IO2 SSD, which offers significantly higher throughput than GP3 SSD. As before, we analyzed the effect of concurrency on Model Streamer (Figure 3) and the number of workers on Tensorizer.
At concurrency 1, Model Streamer and HF Safetensors Loader showed similar loading times of 43.71 seconds and 47 seconds, respectively. However, as we increased concurrency, Model Streamer showed much more pronounced gains compared to GP3 SSD. With concurrency 8, the model was loaded in just 7.53 seconds, making it around 6x faster than the HF Safetensors Loader, which took 47 seconds.
For Tensorizer, the performance also improved significantly. The optimal result was observed with eight workers, achieving a model loading time of 10.36 seconds (Figure 4). Beyond that, adding more workers did not yield further performance improvements, likely due to storage throughput limitations.
Despite the theoretical maximum throughput of 4 GiB/s for IO2 SSD, our experiments consistently hit a ceiling at around 2 GiB/s with Model Streamer and 1.6 GiB/s with Tensorizer. This suggests practical throughput limitations on the AWS infrastructure, rather than the loaders themselves.  
| **Model Streamer**  | **Safetensors Loader**  |  
| --- | --- |  
| **Concurrency**  | **Time to load model to GPU (sec.)**  | **Time to load model to GPU (sec.)**  |  
| 43.71  | 47  |  
| 11.19  |  
| 7.53  |  
| 7.61  |  
| 7.62  |  
_Table 4. Experiment #2 IO2 SSD results_  
| **Tensorizer** |  
| --- |  
| **Number of readers**  | **Time to load model to GPU (sec.)**  |  
| 43.85  |  
| 14.44  |  
| 10.36  |  
| 10.61  |  
| 10.95  |  
_Table 5. Experiment #2 Tensorizer results_
_Figure 3. Model loading time with Model Streamer drops sharply as concurrency increases on IO2 SSD_
_Figure 4. Model Streamer and Tensorizer outperform Safetensors Loader on AWS IO2 SSD at optimal concurrency_
### Experiment #3: S3 
For cloud storage, Experiment #3 compared the performance of Model Streamer and Tensorizer using Amazon S3 as the storage medium. Since HF Safetensors Loader does not support S3, it was not included in this benchmarking experiment. For the Tensorizer experiments, we used different numbers of workers and chose the best result for Figure 6, which was achieved with 16 workers in this case.
The results showed that Model Streamer outperformed Tensorizer at all tested concurrency levels. At concurrency 4, Model Streamer loaded the model in 28.24 seconds. As concurrency increased, Model Streamer continued to improve, reaching a load time of 4.88 seconds at concurrency 32, compared to 37.36 seconds for Tensorizer’s best result with 16 workers. This shows that the Model Streamer demonstrates superior efficiency in loading from cloud-based storage.
Note that during these experiments, we observed unexpected caching behavior on AWS S3. When experiments were repeated in quick succession, the model load times significantly improved, likely due to some form of S3 caching mechanism. To ensure consistency and avoid benefiting from this “warm cache,” we introduced at least a 3-minute wait between each test run. The results presented here reflect the times recorded after these intervals, ensuring they represent cold-start conditions.  
| **Model Streamer** |  
| --- |  
| **Concurrency**  | **Time to load model to GPU (sec.)**  |  
| 4  | 28.24  |  
| 16  | 8.45  |  
| 32  | 4.88  |  
| 64  | 5.01  |  
_Table 6. Experiment #3 S3 results_  
| **Tensorizer** |  
| --- |  
| **Number of readers**  | **Time to load model to GPU (sec.)**  |  
| 86.05  |  
| 37.36  |  
| 48.67  |  
| 41.49  |  
| 41.43  |  
__Table 7. Experiment #3 Tensorizer results__
_Figure 5. Model loading time with Model Streamer decreases sharply as concurrency increases on S3 bucket storage_
_Figure 6. Model Streamer outperforms Tensorizer in model loading from AWS S3 at optimal concurrency_
### ‍Experiment #4: vLLM with all loaders 
This experiment integrated different model loaders into vLLM to measure the total time from model loading to readiness for inference. Model Streamer, Safetensors Loader, and Tensorizer were tested on local storage (GP3 SSD and IO2 SSD), while Hugging Face Safetensors was excluded from S3 since it doesn’t support S3 loading. Tensorizer was tested with vLLM on S3 and compared to Model Streamer.
For each vLLM plus Model Streamer experiment, we used the most optimal concurrency levels determined from earlier experiments. Specifically:
  * For GP3 SSD, a concurrency level of 16 was used (Figure 1).
  * For IO2 SSD, the concurrency level was also 8 (Figure 3).
  * For S3 storage, a higher concurrency level of 32 was used (Figure 5).


Similarly, for the Tensorizer plus vLLM integration, we used the most optimal number of workers determined in previous experiments. Specifically:
  * **GP3 SSD** : 16 workers 
  * **IO2 SSD** : 8 workers 
  * **S3** : 16 workers 


Model Streamer reduced total readiness time to 35.08 seconds on GP3 SSD and 28.28 seconds on IO2 SSD, compared to HF Safetensors Loader at 66.13 seconds and 62.69 seconds, respectively. Tensorizer took 36.19 seconds on GP3 and 30.88 seconds on IO2 SSD, similarly cutting times roughly in half versus Safetensors. On S3, Model Streamer achieved 23.18 seconds total readiness, while Tensorizer required 65.18 seconds.  
| **vLLM with different loaders** |  
| --- |  
| **Loader**  | **Total time until vLLM engine is ready for requests (sec.)**  |  
| **Safetensors Loader**  | 66.13  |  
| **Model Streamer**  | 35.08  |  
| **Tensorizer**  | 36.19  |  
_Table 8. Experiment #4 vLLM results GP3 SSD storage_  
| **vLLM with different loaders** |  
| --- |  
| **Loader**  | **Total time until vLLM engine is ready for requests (sec.)**  |  
| **Safetensors Loader**  | 62.69  |  
| **Model Streamer**  | 28.28  |  
| **Tensorizer**  | 30.88  |  
_Table 9. Experiment #4 vLLM results IO2 SSD storage_  
| **vLLM with different loaders** |  
| --- |  
| **Loader**  | **Total time until vLLM engine is ready for requests (sec.)**  |  
| **Model Streamer**  | 23.18  |  
| **Tensorizer**  | 65.18  |  
_Table 10. Experiment #4 vLLM results S3 storage_
__Figure 7. Model Streamer and Tensorizer reduce total vLLM readiness time across storage types, especially on local SSDs__
## Get started with NVIDIA Run:ai Model Streamer 
Cold start latency remains a key bottleneck in delivering responsive, scalable LLM inference, especially in dynamic or cloud-native environments. Our benchmarks demonstrate that the [NVIDIA Run:ai Model Streamer](https://github.com/run-ai/runai-model-streamer) significantly accelerates model loading times across local and remote storage, outperforming other common loaders. By enabling concurrent weight loading and GPU memory streaming, it offers a practical and high-impact solution for production-scale inference workloads.
If you’re building or scaling inference systems, especially with large models or cloud-based storage, these results offer immediate takeaways: use the Model Streamer to reduce cold-start latency, saturate your storage throughput, and accelerate time-to-inference. With easy integration into frameworks like vLLM and support for high-concurrency, multi-storage environments, it’s a drop-in optimization that can yield measurable gains. Boost your model loading performance with the [NVIDIA Run:ai Model Streamer](https://github.com/run-ai/runai-model-streamer).
Like 
## Tags
[Agentic AI / Generative AI](https://developer.nvidia.com/blog/category/generative-ai/) | [Data Center / Cloud](https://developer.nvidia.com/blog/category/data-center-cloud/) | [General](https://developer.nvidia.com/blog/recent-posts/?industry=General) | [Run:ai](https://developer.nvidia.com/blog/recent-posts/?products=Run%3Aai) | [Intermediate Technical](https://developer.nvidia.com/blog/recent-posts/?learning_levels=Intermediate+Technical) | [Deep dive](https://developer.nvidia.com/blog/recent-posts/?content_types=Deep+dive) | [featured](https://developer.nvidia.com/blog/tag/featured/) | [Inference Performance](https://developer.nvidia.com/blog/tag/inference-performance/) | [LLMs](https://developer.nvidia.com/blog/tag/large-language-models/) | [Low-Latency Inference](https://developer.nvidia.com/blog/tag/low-latency-inference/)
##  About the Authors 
**About Omer Dayan** Omer Dayan is a software developer at RunAI, a startup acquired by NVIDIA, specializing in AI infrastructure. Omer’s research focuses on improving the efficiency and performance of inference software. Omer is a maintainer of the open-source KAI Scheduler project and has a deep technical interest in Nintendo Game Boy architecture. 
[ View all posts by Omer Dayan ](https://developer.nvidia.com/blog/author/omdayan/)
**About Noa Neria** Noa Neria is a senior system software engineer at NVIDIA, currently focusing on building open source infrastructure for LLM inference. Her primary technology interests are LLM inference, GPU virtualization, and distributed systems. She built this expertise by creating core GPU virtualization technologies, including fractional GPUs, at Run:ai (acquired by NVIDIA) and through her development of patented distributed NAS technologies at Dell. Dr. Neria holds a PhD Summa Cum Laude in Computational Chemical Physics from Tel Aviv University. 
[ View all posts by Noa Neria ](https://developer.nvidia.com/blog/author/nneria/)
**About Ekin Karabulut** Ekin Karabulut is a data scientist and developer advocate previously at Run:ai, now at NVIDIA, exploring the efficient usage of large models in different production scenarios. Previously she worked on privacy implications of federated learning, focused on distributed training techniques and got fascinated by inefficiencies in GPU usage in research and industry settings. She established the AI Infrastructure Club and is based in Munich, Germany. 
[ View all posts by Ekin Karabulut ](https://developer.nvidia.com/blog/author/ekarabulut/)
## Comments
## Notable Replies
  1. How to enable run:ai model steamer with run:ai memory swap, e.g. NIM hosting?


###  Continue the discussion at [forums.developer.nvidia.com](https://forums.developer.nvidia.com/t/reducing-cold-start-latency-for-llm-inference-with-nvidia-run-ai-model-streamer/345156)
#### Participants
Close Previous Next
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
