---
title: Chapter 7. Configuring scheduler settings for LLM inference services | Deploy
  models using Distributed Inference with llm-d | Red Hat OpenShift AI Self-Managed
  | 3.4 | Red Hat Documentation
id: chapter-7-configuring-scheduler-settings-for-llm-inference-services-deploy-model
tags:
- llmd-router
- endpointpickerconfig
- openshift-ai
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:46:06.961642Z'
updated: '2026-09-15T04:55:23.786267Z'
source: https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler
source_domain: docs.redhat.com
fetched_at: '2026-09-11T19:46:06.934665Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Red Hat OpenShift AI 3.4 docs (KServe LLMInferenceService CRD layer, not
  raw llm-d) on configuring EPP scheduler settings two ways: inline (router.scheduler.config.inline.<EndpointPickerConfig>)
  or via a Kubernetes ConfigMap referenced from the LLMInferenceService spec (router.scheduler.config.ref.{name,key})
  -- the two are mutually exclusive. Confirms the EPP''s two sequential layers --
  Flow Control (when/order: priority queuing, saturation detection, load shedding)
  and Scheduling (where: routing plugins choosing the backend pod) -- and that Scheduling
  is always on while Flow Control is opt-in via the flowControl feature gate. Documents
  the OpenShift-default scheduling profile: queue-scorer (weight 2) + prefix-cache-scorer
  (weight 3), prioritizing KV-cache reuse over even load. Note: this is a downstream
  KServe-wrapped EPP config surface, one layer removed from raw llm-d-router EndpointPickerConfig
  YAML.'
utility_score: 10.0
---

1. [Red Hat OpenShift AI Self-Managed](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/)
  2. [Deploy models using Distributed Inference with llm-d](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/)
  3. Chapter 7. Configuring scheduler settings for LLM inference services 


Ask AI
# Chapter 7. Configuring scheduler settings for LLM inference services
You can configure custom scheduler settings for large language model (LLM) inference services to optimize request routing and improve performance characteristics such as cache efficiency and load distribution. 
##  [7.1. Configure scheduler settings for LLM inference services](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#understanding-scheduler-configuration_configuring-llm-scheduler)
Copy linkLink copied to clipboard!
You can configure custom scheduler settings for large language model (LLM) inference services to optimize request routing and improve performance characteristics, such as prefix cache hit rates and request distribution patterns. 
The EndpointPicker determines how incoming requests are routed to available model endpoints in your LLM inference service deployment. When multiple replicas of a model are running, the scheduler decides which specific replica handles each request. 
If you do not specify custom scheduler configuration, the system uses a default profile that balances requests across replicas by combining queue depth, for example with a `weight` value of `2`, and a prefix cache hit scoring, for example with a `weight` value of `3`, then selects the highest-scoring replica. This default prioritizes KV cache reuse over even load distribution to minimize redundant computation. 
###  [7.1.1. Configuration methods](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#configuration_methods)
Copy linkLink copied to clipboard!
You can configure scheduler settings using two methods:  

Inline configuration
     Inline configuration embeds the scheduler settings directly in the `LLMInferenceService` resource specification. This method is suitable for simple configurations or when you have unique scheduler settings for a specific service. It keeps all settings for a service in one place for easy viewing.  

ConfigMap-based configuration
     ConfigMap-based configuration stores scheduler settings in a Kubernetes ConfigMap and references it from the `LLMInferenceService` resource. This method is ideal when you want to reuse the same scheduler configuration across multiple LLM inference services, or when you prefer to manage configurations separately from service definitions. It also enables centralized configuration management and easier updates to multiple services. 
The two configuration methods are mutually exclusive. You cannot use both inline and ConfigMap-based configuration in the same `LLMInferenceService` resource. 
##  [7.2. Endpoint Picker architecture](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#llm-d-endpoint-picker-architecture_configuring-llm-scheduler)
Copy linkLink copied to clipboard!
The Endpoint Picker (EPP) is the request orchestration component in Distributed Inference with llm-d that sits between the Inference Gateway and backend model servers. The Endpoint Picker contains two distinct layers that control how requests flow through the system: the flow control layer and the Scheduling layer. 
###  [7.2.1. Request flow through the Endpoint Picker](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#request_flow_through_the_endpoint_picker)
Copy linkLink copied to clipboard!
When an inference request arrives at the Inference Gateway, it passes through the Endpoint Picker before reaching a backend model server. The Endpoint Picker processes each request through two sequential layers:  

Flow Control layer
     Controls **when** and **in what order** requests are dispatched to backends. This layer manages priority-based queuing, saturation detection, and load shedding to enforce service-level objectives and prevent pool overload.  

Scheduling layer
     Controls **where** each request is routed among available backends. This layer uses routing plugins to select the optimal backend pod based on factors such as prefill versus decode capacity, prefix cache affinity, and load distribution. 
###  [7.2.2. Complete request path](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#complete_request_path)
Copy linkLink copied to clipboard!

```
Client Request
    ↓
Inference Gateway (entry point)
    ↓
EPP Flow Control Layer
  • Extract priority from `InferenceObjective`
  • Enqueue request in priority-aware queue
  • Check saturation (queue depth, KV cache utilization)
  • Dispatch when not saturated
    ↓
EPP Scheduling Layer
  • Apply routing plugins (prefill-filter, decode-filter, etc.)
  • Select specific backend pod
  • Forward request to chosen backend
    ↓
Backend Model Server (vLLM, TGI, etc.)
  • Execute inference
  • Return response
```

The Scheduling layer is always enabled. Flow Control is optional and can be enabled through the `flowControl` feature gate for priority-based queuing and saturation detection. 
##  [7.3. Configuring scheduler inline in LLMInferenceService](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#configuring-scheduler-inline_configuring-llm-scheduler)
Copy linkLink copied to clipboard!
You can configure scheduler settings directly within your LLMInferenceService resource specification by using inline configuration. This approach embeds the scheduler configuration in the same YAML file as your service definition, making it suitable for service-specific settings. 
**Prerequisites**
  * You have cluster administrator privileges or namespace administrator privileges for the namespace where you want to deploy the LLM inference service. 
  * You have installed OpenShift AI and configured model serving. 
  * You have a model that you want to deploy as an LLM inference service, or you have an existing `LLMInferenceService` resource that you want to update. 


**Procedure**
  1. Create a YAML file for your `LLMInferenceService` resource or open an existing service definition for editing. 
  2. Add the `router.scheduler.config.inline` section to the service specification: 

```
apiVersion serving.kserve.io/v1alpha1
 LLMInferenceService
metadata
   llmservicewithscheduler  # the namespace where you want to deploy the service
  namespace <your_namespace

  model
     hf//<your_model_uri
     <your_model_name
  router
    scheduler
      config
        inline
          apiVersion inference.networking.xk8s.io/v1alpha1
           EndpointPickerConfig
          plugins
              prefixcachescorer
              queuescorer
          schedulingProfiles
            default
            plugins
             pluginRef queuescorer
              weight 
             pluginRef prefixcachescorer
              weight 
```

     * `router.scheduler.config.inline.plugins.type`: This example enables prefix cache scoring and queue scoring to route similar prompts to the same replica. You can configure the scheduler plugins according to your requirements. 
  3. Apply the configuration to your cluster: 

```
$ oc apply -f llm-service.yaml
```

  4. Verify that the LLM inference service is running with the custom scheduler configuration: 

```
$ oc get llminferenceservice llm-service-optimized -n model-serving
```

The output shows the service status. Wait until the service reaches the `Ready` state. 


**Verification**
  * Send test requests to your LLM inference service to verify that it is handling requests correctly with the configured scheduler settings. 


##  [7.4. Configuring scheduler using ConfigMap references](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#configuring-scheduler-configmap_configuring-llm-scheduler)
Copy linkLink copied to clipboard!
You can configure scheduler settings by storing the configuration in a Kubernetes ConfigMap and referencing it from your LLMInferenceService resource. This approach is ideal for sharing configurations across multiple services or managing configurations separately from service definitions. 
**Prerequisites**
  * You have cluster administrator privileges or namespace administrator privileges for the namespace where you want to deploy the LLM inference service. 
  * You have installed OpenShift AI and configured model serving. 
  * You have a model that you want to deploy as an LLM inference service, or you have an existing `LLMInferenceService` resource that you want to update. 
  * You are familiar with Kubernetes ConfigMaps. 


**Procedure**
  1. Create a ConfigMap that contains your scheduler configuration. Create a YAML file named `scheduler-config.yaml` with the following content: 

```
apiVersion v1
 ConfigMap
metadata
   llmschedulerconfig
  namespace <your_namespace # Replace with your namespace

  default-scheduler 
    apiVersion: serving.kserve.io/v1alpha1
    kind: EndpointPickerConfig
    plugins:
      - type: prefix-cache-scorer
      - type: queue-scorer
    schedulingProfiles:
    - name: default
      plugins:
      - pluginRef: queue-scorer
        weight: 2
      - pluginRef: prefix-cache-scorer
        weight: 3
```

     * `data`: This section contains a key,`default-scheduler`, with the scheduler configuration as its value. 
     * `data.default-scheduler`: The scheduler configuration. You can include multiple configurations by adding more keys to the `data` section. 
  2. Apply the ConfigMap to your cluster: 

```
$ oc apply -f scheduler-config.yaml
```

  3. Verify that the ConfigMap was created successfully: 

```
$ oc get configmap llm-scheduler-config -n your_namespace
```

  4. Create or edit your `LLMInferenceService` resource to reference the ConfigMap. For example, create a YAML file named `llm-service.yaml`: 

```
apiVersion serving.kserve.io/v1alpha1
 LLMInferenceService
metadata
   llmservicewithconfigmap
  namespace <your_namespace

  model
     hf//<your_model_uri
     <your_model_name
  router
    scheduler
      config
        
           llmschedulerconfig
           defaultscheduler
```

     * `ref.name`: Specifies the ConfigMap name 
     * `ref.key`: Specifies which key within the ConfigMap contains the scheduler configuration. 
  5. Apply the LLMInferenceService configuration: 

```
$ oc apply -f llm-service.yaml
```

  6. Verify that the LLM inference service is running with the scheduler configuration from the ConfigMap: 

```
$ oc get llminferenceservice llm-service-with-configmap -n your-namespace
```

Wait until the service reaches the `Ready` state. 


**Verification**
  * Send test requests to your LLM inference service to verify that it handles requests correctly with the configured scheduler settings. 


##  [7.5. Scheduler plugin reference](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#scheduler-plugins_configuring-llm-scheduler)
Copy linkLink copied to clipboard!
Scheduler plugins implement specific routing logic that determines how the EndpointPicker distributes requests across model replicas. You can configure these plugins in the `EndpointPickerConfig` to customize request routing behavior. 
When multiple plugins are configured, the scheduler combines their scores to make the final routing decision. The scheduler evaluates each plugin for every request and selects the endpoint with the highest combined score. 
###  [7.5.1. Default plugins](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploy_models_using_distributed_inference_with_llm-d/configuring-llm-scheduler#default_plugins)
Copy linkLink copied to clipboard! 

queue-scorer
     The `queue-scorer` plugin scores replicas based on queue depth. Replicas with shorter queues score higher, helping to distribute requests evenly across available replicas and prevent overloading individual instances.  

prefix-cache-scorer
     The `prefix-cache-scorer` plugin optimizes request routing for improved prefix cache hit rates. It routes requests with similar prompt prefixes to the same model replica, increasing the likelihood that the replica can reuse previously computed results for common prompt prefixes. 
### Learn
  * [Developer resources](https://developers.redhat.com/learn)
  * [Cloud learning hub](https://docs.redhat.com/learn/learning-paths)
  * [Training and certification](https://www.redhat.com/services/training-and-certification)
  * [Customer support](https://access.redhat.com/support)
  * [See all documentation](https://docs.redhat.com/en/products)


### Try, buy, & sell
  * [Product trial center](https://redhat.com/en/products/trials)
  * [Red Hat Ecosystem Catalog](https://catalog.redhat.com/)
  * [Buy online (Japan)](https://www.redhat.com/about/japan-buy)


### Communities
  * [Customer Portal Community](https://access.redhat.com/community)


### About Red Hat
We deliver hardened solutions that make it easier for enterprises to work across platforms and environments, from the core datacenter to the network edge.
### Making open source more inclusive
Red Hat is committed to replacing problematic language in our code, documentation, and web properties. For more details, see the [Red Hat Blog](https://www.redhat.com/en/blog/making-open-source-more-inclusive-eradicating-problematic-language). 
### About Red Hat Documentation
We help Red Hat users innovate and achieve their goals with our products and services with content they can trust. [Explore our recent updates](https://www.redhat.com/en/blog/whats-new-docsredhatcom). 
### Theme
System default 
### Red Hat legal and privacy links
  * [Inclusion at Red Hat](https://redhat.com/en/about/our-culture/diversity-equity-inclusion)
  * [Cool Stuff Store](https://coolstuff.redhat.com/)


[All systems operational](https://status.redhat.com)
© 2026 Red Hat
### Red Hat legal and privacy links
  * [Privacy statement](https://redhat.com/en/about/privacy-policy)
  * [All policies and guidelines](https://redhat.com/en/about/all-policies-guidelines)
  * [Digital accessibility](https://redhat.com/en/about/digital-accessibility)
  * Cookie Preferences and Opt-Out Rights

Back to top
