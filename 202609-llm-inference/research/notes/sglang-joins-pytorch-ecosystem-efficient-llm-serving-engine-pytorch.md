---
title: 'SGLang Joins PyTorch Ecosystem: Efficient LLM Serving Engine - PyTorch'
id: sglang-joins-pytorch-ecosystem-efficient-llm-serving-engine-pytorch
tags:
- internal-llm-inference-stacks-7f0c90
- sglang
- production-adoption
- radixattention
created: '2026-10-09T22:54:11.247389Z'
updated: '2026-10-10T14:52:17.017412Z'
source: https://pytorch.org/blog/sglang-joins-pytorch/
source_domain: pytorch.org
fetched_at: '2026-10-09T22:54:11.225978Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PyTorch Foundation blog (March 19, 2025) announcing SGLang''s integration
  into the PyTorch ecosystem. First-party description of SGLang''s architecture: RadixAttention
  for prefix caching, zero-overhead CPU scheduler, continuous batching, token (paged)
  attention, speculative decoding, tensor parallelism, chunked prefill, structured
  outputs, and FP8/INT4/AWQ/GPTQ quantization, plus a separate ''flexible frontend
  language'' for programmatic LLM app construction. States named production adoption:
  xAI uses SGLang to serve Grok 3; Microsoft Azure uses SGLang to serve DeepSeek R1
  on AMD GPUs. Claims SGLang is ''deployed to large-scale production, generating trillions
  of tokens every day'' with 300+ GitHub contributors, and lists institutional backers/users:
  AMD, Atlas Cloud, Baseten, Cursor, DataCrunch, Etched, Hyperbolic, iFlytek, Jam
  & Tea Studios, LinkedIn, LMSYS, Meituan, Nebius, Novita AI, NVIDIA, RunPod, Stanford,
  UC Berkeley, UCLA, xAI, 01.AI. Includes a reference Docker launch command for serving
  DeepSeek-V3 with tensor-parallel=8.'
utility_score: 13.0
---

[Close Search ](https://pytorch.org/blog/sglang-joins-pytorch/)
# SGLang Joins PyTorch Ecosystem: Efficient LLM Serving Engine
March 19, 2025September 4th, 2025
We’re thrilled to announce that the SGLang project has been integrated into the PyTorch ecosystem! This integration ensures that SGLang aligns with PyTorch’s standards and practices, providing developers with a reliable and community-supported framework for fast and flexible serving of LLMs.
To view the PyTorch Ecosystem, see the [PyTorch Landscape](https://landscape.pytorch.org/) and learn more about how projects can [join the PyTorch Ecosystem](https://github.com/pytorch-fdn/ecosystem).
## About SGLang
SGLang is a fast-serving engine for large language models and vision language models. It makes the interaction with models faster and more controllable by co-designing the backend runtime and frontend language.
The core features include:
  * Fast Backend Runtime: Provides efficient serving with RadixAttention for prefix caching, zero-overhead CPU scheduler, continuous batching, token attention (paged attention), speculative decoding, tensor parallelism, chunked prefill, structured outputs, and quantization (FP8/INT4/AWQ/GPTQ).
  * Flexible Frontend Language: Offers an intuitive interface for programming LLM applications, including chained generation calls, advanced prompting, control flow, multi-modal inputs, parallelism, and external interactions.
  * Extensive Model Support: Supports a wide range of generative models (Llama, Gemma, Mistral, Qwen, DeepSeek, LLaVA, etc.), embedding models (e5-mistral, gte, mcdse) and reward models (Skywork), with easy extensibility for integrating new models.
  * Active Community: SGLang is open source and backed by an active community with industry adoption.


SGLang is famous for its fast speed. It can often significantly outperform other state-of-the-art frameworks in terms of serving throughput and latency. You can learn more about the underlying techniques from the past release blog posts: [v0.2 blog](https://lmsys.org/blog/2024-07-25-sglang-llama3/), [v0.3 blog](https://lmsys.org/blog/2024-09-04-sglang-v0-3/), [v0.4 blog](https://lmsys.org/blog/2024-12-04-sglang-v0-4/).
SGLang has been widely adopted by leading industry companies and frontier research labs. For example, xAI uses SGLang to serve its flagship model, [Grok 3](https://grok.com/), which is currently the best model according to the Chatbot Arena leaderboard. Microsoft Azure uses SGLang to serve [DeepSeek R1](https://techcommunity.microsoft.com/blog/azurehighperformancecomputingblog/running-deepseek-r1-on-a-single-ndv5-mi300x-vm/4372726) on AMD GPUs, which is currently the best open source model.
## Serving DeepSeek Models
You can easily launch a Docker container to serve a DeepSeek model with the following command:

```
# Pull the latest image
docker pull lmsysorg/sglang:latest

# Launch a server
docker run --gpus all --shm-size 32g -p 30000:30000 -v ~/.cache/huggingface:/root/.cache/huggingface --ipc=host --network=host --privileged lmsysorg/sglang:latest \
    python3 -m sglang.launch_server --model deepseek-ai/DeepSeek-V3 --tp 8 --trust-remote-code --port 30000

```

Then you can query the server with the OpenAI-compatible API

```
import openai
client = openai.Client(base_url=f"http://127.0.0.1:30000/v1", api_key="None")

response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-V3",
    messages=[
        {"role": "user", "content": "List 3 countries and their capitals."},
    ],
    temperature=0,
    max_tokens=64,
)

```

The server launch command above works for 8xH200. You can find detailed instructions for other hardware (MI300X, H100, A100, H20, L40S) at https://docs.sglang.ai/references/deepseek.html.
SGLang integrates DeepSeek-specific optimizations, such as MLA throughput optimizations, MLA-optimized kernels, data-parallel attention, multi-token prediction, and DeepGemm, making it the top choice for serving DeepSeek models by dozens of [companies](https://x.com/lmsysorg/status/1887262321636221412), including AMD, NVIDIA, and many cloud providers. The team is actively working on integrating more optimizations following the 2025 H1 roadmap below.
## Serving Llama Models
Similarly, you can launch the server for a Llama 3.1 text model with:

```
python -m sglang.launch_server --model-path meta-llama/Meta-Llama-3.1-8B-Instruct

```

Or a Llama 3.2 multimodal model with:

```
python3 -m sglang.launch_server --model-path meta-llama/Llama-3.2-11B-Vision-Instruct  --chat-template=llama_3_vision

```

## Roadmap
This year, the SGLang team will continue to push the boundaries of system efficiency. You can find the roadmap of 2025H1 [here](https://github.com/sgl-project/sglang/issues/4042). The focus is
  * Throughput-oriented large-scale deployment similar to the DeepSeek inference system
  * Long context optimizations
  * Low latency speculative decoding
  * Reinforcement learning training framework integration
  * Kernel optimizations


## Community
SGLang has been deployed to large-scale production, generating trillions of tokens every day. It has an active community with over three hundred contributors on GitHub. It is supported by the following institutions: AMD, Atlas Cloud, Baseten, Cursor, DataCrunch, Etched, Hyperbolic, iFlytek, Jam & Tea Studios, LinkedIn, LMSYS, Meituan, Nebius, Novita AI, NVIDIA, RunPod, Stanford, UC Berkeley, UCLA, xAI, and 01.AI.
## Conclusion
We’re excited to welcome SGLang to the PyTorch ecosystem. SGLang accelerates the serving of large language and vision language models. It’s widely adopted by industry, powering the large-scale online serving of frontier models like Grok and DeepSeek.
We invite you to explore the [SGLang GitHub repo](https://github.com/sgl-project/sglang/tree/main), join the [community on Slack](https://slack.mindee.com/), and reach out to contact@sglang.ai for inquiries or collaboration opportunities. Together, we can make powerful AI models accessible to everyone.
### Docs
Access comprehensive developer documentation for PyTorch
### Tutorials
Get in-depth tutorials for beginners and advanced developers
[View Tutorials ›](https://pytorch.org/tutorials)
### Resources
Find development resources and get your questions answered
[View Resources ›](https://pytorch.org/resources)
##  **Stay in touch** for updates, event info, and the latest news
By submitting this form, I consent to receive marketing emails from the LF and its projects regarding their events, training, research, developments, and related announcements. I understand that I can unsubscribe at any time using the links in the footers of the emails I receive. [Privacy Policy](https://www.linuxfoundation.org/privacy/?__hstc=132719121.beecd1747181d248a94c08fb4c366ad1.1791586450592.1791586450592.1791586450592.1&__hssc=132719121.1.1791586450592&__hsfp=71fa762034c573b129d4ad707c755f41).


© 2026 PyTorch. Copyright © The Linux Foundation®. All rights reserved. The Linux Foundation has registered trademarks and uses trademarks. For more information, including terms of use, privacy policy, and trademark usage, please see our [Policies](https://www.linuxfoundation.org/legal/policies?__hstc=132719121.beecd1747181d248a94c08fb4c366ad1.1791586450592.1791586450592.1791586450592.1&__hssc=132719121.1.1791586450592&__hsfp=71fa762034c573b129d4ad707c755f41) page. [Trademark Usage](https://www.linuxfoundation.org/trademark-usage?__hstc=132719121.beecd1747181d248a94c08fb4c366ad1.1791586450592.1791586450592.1791586450592.1&__hssc=132719121.1.1791586450592&__hsfp=71fa762034c573b129d4ad707c755f41). [Privacy Policy](http://www.linuxfoundation.org/privacy?__hstc=132719121.beecd1747181d248a94c08fb4c366ad1.1791586450592.1791586450592.1791586450592.1&__hssc=132719121.1.1791586450592&__hsfp=71fa762034c573b129d4ad707c755f41)
[Close Menu ](https://pytorch.org/blog/sglang-joins-pytorch/)
  * [Learn](https://pytorch.org/blog/sglang-joins-pytorch/)
    * [Intro to PyTorch – YouTube Series](https://pytorch.org/tutorials/beginner/introyt.html)
    * [PyTorch Certification](https://pytorch.org/pytorch-certification/)
  * [Community](https://pytorch.org/blog/sglang-joins-pytorch/)
    * [Join the Ecosystem](https://pytorch.org/join-ecosystem)
    * [Developer Resources](https://pytorch.org/resources/)
    * [Contributor Awards](https://pytorch.org/contributor-awards/)
  * [Projects](https://pytorch.org/projects/)
    * [PyTorch](https://pytorch.org/projects/pytorch/)
  * [Docs](https://pytorch.org/blog/sglang-joins-pytorch/)
  * [Blog & News](https://pytorch.org/blog/sglang-joins-pytorch/)
    * [Announcements](https://pytorch.org/announcements)
  * [About](https://pytorch.org/blog/sglang-joins-pytorch/)
    * [PyTorch Foundation](https://pytorch.org/foundation/)
    * [Technical Advisory Council](https://pytorch.org/tac/)
    * [Cloud Credit Program](https://pytorch.org/credits/)


