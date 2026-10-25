---
title: 'LLM Model Serving: Faster Inference on Snowflake'
id: llm-model-serving-faster-inference-on-snowflake
tags:
- internal-llm-inference-stacks
- snowflake
- vllm
- pagedattention
- model-serving
- first-person-account
created: '2026-10-09T22:53:31.439533Z'
updated: '2026-10-10T14:52:16.990524Z'
source: https://www.snowflake.com/en/blog/engineering/llm-model-serving-vllm-inference/
source_domain: www.snowflake.com
fetched_at: '2026-10-09T22:53:31.416848Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Snowflake engineering blog (Apr 17 2026), vendor-authored product-launch
  post for Snowflake Model Serving''s LLM support (extending Snowflake ML''s Model
  Registry to bring your own fine-tuned/domain-specific/OSS models, distinct from
  Snowflake''s managed Cortex AI for frontier models). Engine: vLLM, explicitly named
  and credited by mechanism -- "continue to double down on vLLM ... leveraging vLLMs
  PagedAttention and continuous batching capabilities" for dynamic GPU memory management
  and streaming output. Exposes models via the standard OpenAI protocol (chat completions,
  multimodal base64 input) callable from SQL, Python, or REST, using a reg.get_model(name).version(v).list_services()
  pattern and direct openai Python client usage against a Snowflake-issued endpoint.
  Supports one-click Hugging Face model import via a Model Garden and remote model
  logging via file I/O so GPUs are not needed just to log a model. Runs inside Snowpark
  Container Services (SPCS), an airgapped container runtime within the customers Snowflake
  account perimeter; co-engineered internal network routing solved an egress-proxy
  bottleneck, achieving a measured 700GB model download in 83 seconds at 67.5 Gbps.
  Claimed benchmark: 2.2x-2.3x P99 latency/TTFT improvement on average vs Databricks
  across Qwen3-4B, Gemma3-1B, Medgemma-27B, and favorable high-concurrency scaling
  vs Baseten; benchmark code published at github.com/Snowflake-Labs/sf-samples. Caveat
  stated by Snowflake itself: benchmarks were single-node, and GLM 4.5 (700GB model)
  scalability was tested in Snowflakes own internal environment -- self-reported,
  not third-party verified. Does not name the orchestration/gateway layer around vLLM
  beyond SPCS; flagged as an unstated component.'
utility_score: 14.0
---

[Blog](https://www.snowflake.com/en/blog/engineering/)/[Machine Learning](https://www.snowflake.com/en/blog/engineering/machine-learning/)/LLM Serving Made Easy: High-Throughput Inference with vLLM
Apr 17, 2026/6 min readMachine Learning
Copy post link
Open in ClaudeOpen in ChatGPT
# LLM Serving Made Easy: High-Throughput Inference with vLLM
[ Vivek Alamuri +5 ](https://www.snowflake.com/#snowflake-blog-author-chip-title)
Generative AI is transforming how organizations extract value from their data. They're GPU-hungry, generate variable-length responses, and require specialized engines to serve efficiently. However, customers consistently tell us that deploying, managing and scaling large language models (LLMs) in production introduces significant friction. Builders want the flexibility to use state-of-the-art open source models, but they want to avoid the undifferentiated heavy lifting of managing GPU infrastructure, optimizing complex inference engines and securing data perimeters.
Snowflake offers [Cortex AI](https://www.snowflake.com/en/product/features/cortex/) for managed access to frontier models. [Model Serving](https://docs.snowflake.com/en/developer-guide/snowflake-ml/inference/real-time-inference-rest-api) from [Snowflake ML](https://www.snowflake.com/ml) is for teams that want to bring their own models — fine-tuned, domain-specific or open source — with full control over the serving infrastructure, model weights and inference parameters. We are excited to share how we have expanded Snowflake Model Serving from the [Model Registry](https://docs.snowflake.com/en/developer-guide/snowflake-ml/model-registry/overview) to natively support all of these cases. Now builders can securely deploy and scale generative AI applications using the exact same intuitive APIs they already use for traditional machine learning models, all while achieving industry-leading price-performance and strict data governance. In this blog, we show you how we architected Model Serving so that our customers can consistently take advantage of a **2.2x–2.3x P99 latency and TTFT on average in Snowflake over competing managed inference products**.
## Eliminating the heavy lifting of LLM inference
In previous posts ([architecture](https://www.snowflake.com/en/engineering-blog/scalable-model-serving-architecture/), [scaling real-time inference](https://www.snowflake.com/en/engineering-blog/scale-real-time-model-serving/)), we explored how Snowflake Model Serving turns logged models into highly scalable inference services. However, LLMs introduce unique bottlenecks. They are GPU-hungry, requiring massive matrix multiplication operations, and their autoregressive nature produces large, dynamic responses that complicate memory management and latency.
To help solve this for our customers, we continue to double down on **vLLM** , a high-throughput, memory-efficient inference engine. By leveraging vLLM's PagedAttention and continuous batching capabilities, Snowflake Model Serving dynamically manages GPU memory, allowing builders to optimize throughput and support streaming outputs without sacrificing performance.
Figure 1: Snowflake Model Serving architecture optimized with vLLM for high-throughput continuous batching.
## Key features for builders
We built Snowflake Model Serving with the developer experience in mind, enabling you to go from prototype to production with minimal friction.
  * **Familiar, standardized APIs:** We know builders don't want to rewrite their application logic. That's why Snowflake Model Serving exposes LLMs via the standard OpenAI protocol, including support for multimodal input specified inline with `base64` encoding. Whether you are building in SQL, Python or calling a REST API, you can interface with your hosted models seamlessly.


_Python example (OpenAI API):_

```
from openai import OpenAI
from snowflake.ml.registry import Registry

reg = Registry(session=session)

mv = reg.get_model("QWEN3_4B").version("V1")
ENDPOINT = mv.list_services()["inference_endpoint"].iloc[0]
client = OpenAI(
    base_url=f"https://{ENDPOINT}/v1",
    api_key="unused",  # sometimes required but ignored
    default_headers={"Authorization": f'Snowflake Token="{SNOWFLAKE_PAT_TOKEN}"'},
)

response = client.chat.completions.create(
    model="QWEN3_4B",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "Explain vector databases in one sentence."},
            ],
        },
    ],
)

print(response.choices[0].message.content)
```

_SQL example:_

```
SELECT
    QWEN3_4B_SERVICE!__CALL__(
        [
            {
                'role': 'user',
                'content': [
                 {
                    'type': 'text',
                    'text': 'Summarize following support ticket: ' || ticket_text
                 }
               ]
            }
        ]
    ) AS response
FROM support_tickets
WHERE status = 'OPEN';
```

  * **One-click model onboarding and remote logging:** Customers can now import models directly from Hugging Face with a single click via our curated Model Garden. Additionally, we introduced a mechanism to remotely log massive models using file I/O. This means builders no longer need to provision scarce, expensive GPUs just to download and log a model into the Snowflake Model Registry.


_Python example (remote logging):_

```
from snowflake.ml.model.models import huggingface
from snowflake.ml.registry import Registry

reg = Registry(session=session)

model = huggingface.TransformersPipeline(
    task="text-generation",
    model="Qwen/Qwen3-4B-Instruct-2507",
    compute_pool_for_log = "SYSTEM_COMPUTE_POOL_CPU"
)

mv = reg.log_model(model, model_name="QWEN3_4B", version_name="V1")
```

_From UI example (remote logging):_
Figure 2: Easily import state-of-the-art open source models directly into your secure Snowflake environment.
  * **Inference-time parameters:** To give builders full control over model behavior, we separated [inference parameters](https://docs.snowflake.com/en/developer-guide/snowflake-ml/model-registry/model-signature#constructing-a-signature-with-parameters) (like `temperature` and `max_tokens`) from input data, allowing dynamic configuration at runtime without altering the core model signature.


## Secure by design: Airgapped execution and optimized startup time
Security is our top priority. Enterprise customers want certainty that their proprietary data will not be exfiltrated during inference.
Snowflake Model Serving runs on the robust foundation of [**Snowpark Container Services (SPCS)**](https://docs.snowflake.com/en/developer-guide/snowpark-container-services/overview). Inference workloads are executed within an airgapped container runtime securely inside your Snowflake account perimeter.
During cold-start, downloading massive LLM weights into these secure environments historically presented a networking bottleneck due to required egress proxies. By co-engineering optimized, secure internal routing pathways with Snowflake SPCS, we drastically improved startup times. In our stress tests, this architecture successfully downloaded a 700GB model in just 83 seconds, achieving sustained network speeds of 67.5 Gbps, enabling us to quickly start inferencing.
## Unmatched price-performance
When evaluating LLM serving, stable Time to First Token (TTFT) and tail latency under load are critical for production-grade user experiences. We benchmarked Snowflake Model Serving against Databricks and Baseten using diverse architectures like Qwen3-4B, Gemma3-1B and Medgemma-27B.
**The results demonstrate a clear architectural advantage:**
Figure 3: Snowflake demonstrates a consistent 2.2x–2.3x performance advantage over Databricks.
  * **Consistent superiority:** Snowflake Model Serving demonstrated a **2.2x to 2.3x P99 latency and TTFT improvement on average** over Databricks. This advantage remains remarkably constant across concurrency levels and becomes even more prominent as scale increases.
  * **High-concurrency scalability:** When tested against Baseten, Snowflake outperformed across the spectrum.


Figure 4: Predictable tail-latency scaling under heavy concurrency against Baseten.
  * **Large model support:** Finally the platform is tested against a very large model (GLM 4.5, a 700G model on a H200 GPU node) and observed that the system is able to scale and leverage the high end GPU really well.


Figure 5: System scalability test with large model on H200 GPU.
## Best practices for productionization
Snowflake empowers you to rethink how you pay for and structure your AI workloads:
  1. **Pay by resource, not by token:** Hosting a model on Snowflake Model Serving shifts the financial model from pay-per-token pricing to paying only for the fixed infrastructure consumed. Paired with auto-suspend and auto-scaling, this managed service offers a massive ROI for high-volume workloads when compared to competition. Snowflake's managed Model Serving reduces your operational expenses so that you can focus on your workload and not on managing infrastructure.
  2. **Optimize with specialist models:** Instead of sending every request to an expensive, massive commercial model, builders can route specific tasks to highly optimized, fine-tuned "expert" models (like MedGemma) to drastically reduce costs and improve speed.
  3. **Protect sensitive data:** Sending data to third-party APIs introduces breach risks. By keeping inference inside Snowflake, all data sent to your endpoint remains strictly within your secure deployment.


To learn more and start building, check out more [Snowflake ML](https://www.snowflake.com/ml) resources and visit the [Snowflake Model Registry](https://docs.snowflake.com/en/developer-guide/snowflake-ml/model-registry/overview) documentation today.
_**Note:** All benchmarks were run on single-node services to evaluate baseline compute efficiency. Real-world deployments can utilize multi-node scaling. Some of the evaluations demonstrated (Figures 4 and 5) in this blog were achieved in an internal Snowflake testing environment using Nvidia H200 GPUs. This demonstration is intended to showcase the hardware-agnostic scalability and maximum architectural capabilities of Snowpark Container Services for ML workloads, and actual results may vary based on your workload, network and regions. The H200 is not currently available as a standard compute pool, but may be available upon request. Please reach out to your account team to discuss the highly performant GPU instances options currently available for your region to accelerate your ML workloads today. You can find our code to benchmark inference services [here](https://github.com/Snowflake-Labs/sf-samples/blob/main/samples/ml/model_serving/benchmarks/llm_streaming_benchmark.ipynb#L4)._
### Learn more about the authors
#### [Vivek Alamuri Software Engineer ](https://www.snowflake.com/en/blog/authors/vivek-alamuri/)#### [Pavithran Ramachandran Senior Software Engineer ](https://www.snowflake.com/en/blog/authors/pavithran-ramachandran/)#### [Sumit Das Principal Software Engineer, Snowflake ](https://www.snowflake.com/en/blog/authors/sumit-das/)#### [Goutam Murlidhar Senior Software Engineer ](https://www.snowflake.com/en/blog/authors/goutam-murlidhar/)#### [Sasank Chindirala Software Engineer ](https://www.snowflake.com/en/blog/authors/sasank-chindirala/)#### [Seth Mason Senior Product Manager ](https://www.snowflake.com/en/blog/authors/seth-mason/)
Share this post
Subscribe to our blog newsletter
Get the best, coolest and latest delivered to your inbox each week
By submitting this form, I understand Snowflake will process my personal information in accordance with their Privacy Notice.
##  Where Data Does More
**Subscribe to our monthly newsletter**
Stay up to date on Snowflake’s latest products, expert insights and resources—right in your inbox!
Product
  * [Snowflake CoWork](https://www.snowflake.com/en/product/snowflake-cowork/)
  * [Applications & Collaboration](https://www.snowflake.com/en/product/applications-and-collaboration/)


Support


[Industries](https://www.snowflake.com/en/solutions/industries/)
  * [Advertising, Media & Entertainment](https://www.snowflake.com/en/solutions/industries/advertising-media-entertainment/)
  * [Financial Services](https://www.snowflake.com/en/solutions/industries/financial-services/)
  * [Healthcare & Life Sciences](https://www.snowflake.com/en/solutions/industries/healthcare-and-life-sciences/)
  * [Retail & Consumer Goods](https://www.snowflake.com/en/solutions/industries/retail-consumer-goods/)


Company
  * [Investor Relations](https://investors.snowflake.com/overview/default.aspx)
  * [Environmental, Social & Governance](https://www.snowflake.com/en/company/overview/esg/)
  * [Snowflake Summit 26](https://www.snowflake.com/en/summit/)


Learn
  * [Resource Library](https://snowflake.com/en/resources/)
  * [Snowflake University](https://learn.snowflake.com/en/)
  * [Data Governance](https://www.snowflake.com/en/data-governance/)


  * © 2026 Snowflake Inc. All Rights Reserved
  * [Communication Preferences](https://info.snowflake.com/Preference-center.html)
  * Cookies Settings
  * [Do Not Share My Personal Information](https://www.snowflake.com/en/legal/privacy/privacy-policy/#12)


## Snowflake's Use of Cookies
We use cookies to enhance your experience and to analyze site traffic as described in our Cookie Statement. By accepting, you consent to our use of cookies.[Cookie Statement.](https://www.snowflake.com/privacy-policy/cookie-statement/)
Cookies Settings Reject All Accept All Cookies
## Privacy Preference Center
Opt-Out Request Honored
## Privacy Preference Center
  * ### Your Privacy
  * ### Strictly Necessary Cookies
  * ### Performance Cookies
  * ### Functional Cookies
  * ### Targeting Cookies


#### Your Privacy
When you visit any website, it may store or retrieve information on your browser, mostly in the form of cookies. This information might be about you, your preferences, or your device, and is mostly used to make the site work as you expect. The information does not usually identify you directly, but it can give you a more personalized web experience. Because we respect your right to privacy, you can choose not to allow some types of cookies. Click on the different category headings to learn more and change our default settings. Blocking some types of cookies may impact your experience of the site and the services we are able to offer. [More information](https://cookiepedia.co.uk/giving-consent-to-cookies)
#### Strictly Necessary Cookies
Always Active
These cookies are necessary for the website to function and cannot be switched off. They are usually only set in response to actions made by you which amount to a request for services, such as setting your privacy preferences, logging in or filling in forms. You can set your browser to block or alert you about these cookies, but some parts of the site will not then work. These cookies do not store any personally identifiable information.
Cookies Details
#### Performance Cookies
Performance Cookies
These cookies allow us to count visits and traffic sources so we can measure and improve the performance of our site. They help us to know which pages are the most and least popular and see how visitors move around the site. All information these cookies collect is aggregated and therefore anonymous. If you do not allow these cookies we will not know when you have visited our site, and will not be able to monitor its performance.
Cookies Details
#### Functional Cookies
Functional Cookies
These cookies enable the website to provide enhanced functionality and personalisation. They may be set by us or by third party providers whose services we have added to our pages. If you do not allow these cookies then some or all of these services may not function properly.
Cookies Details
#### Targeting Cookies
Targeting Cookies
These cookies may be set through our site by our advertising partners. They may be used by those companies to build a profile of your interests and show you relevant adverts on other sites. They do not store directly identifiable personal information, but are based on uniquely identifying your browser and internet device. If you do not allow these cookies, you will experience less targeted advertising.
Cookies Details
Back Button
### Cookie List
Filter Button
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Clear
  * checkbox label label


Apply Cancel
Confirm My Choices
Allow All
