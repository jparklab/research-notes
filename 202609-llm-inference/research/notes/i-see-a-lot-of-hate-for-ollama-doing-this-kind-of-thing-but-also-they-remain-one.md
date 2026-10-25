---
title: I see a lot of hate for ollama doing this kind of thing but also they remain
  one... | Hacker News
id: i-see-a-lot-of-hate-for-ollama-doing-this-kind-of-thing-but-also-they-remain-one
tags:
- internal-llm-inference-stacks-7f0c90
- ollama
- llamacpp
- gray-literature
- named-practitioner
created: '2026-10-09T22:55:06.739444Z'
updated: '2026-10-10T14:52:17.064003Z'
source: https://news.ycombinator.com/item?id=44803629
source_domain: news.ycombinator.com
fetched_at: '2026-10-09T22:55:06.717645Z'
fetch_provider: crawl4ai
status: review
type: note
tier: practitioner
content_type: forum
deprecated: false
summary: 'Hacker News comment thread (on the ''Ollama Turbo'' submission, item 44802414)
  containing substantive first-person technical claims from named/pseudonymous practitioners,
  including Ollama maintainer ''mchiang'' (Michael Chiang, Ollama co-founder) responding
  directly. Treat all claims as gray literature from pseudonymous or semi-identified
  commenters, not verified fact.\n\nKey technical claims (with speaker attribution;
  evidentiary caveats noted):\n- mchiang (Ollama maintainer): ''Since the new multimodal
  engine, Ollama has moved off of llama.cpp as a wrapper. We do continue to use the
  GGML library... Ollama might look like a toy... to keep its simplicity, we go through
  a deep amount of struggles.'' He also states Ollama implemented its own MXFP4 kernel
  for gpt-oss and benchmarked against OpenAI''s reference implementation and against
  vLLM''s published harmony-format benchmarks to validate parity -- this is a first-party
  maintainer claim, not independently verified in-thread.\n- Commenter ''buyucu''
  disputes mchiang''s framing as ''gaslighting,'' asserting ''GGML library is llama.cpp.
  They are one and the same... developed by the same people.'' Another commenter ''scosman''
  flatly rejects this (''Nope...''). This is an active, unresolved technical disagreement
  about whether Ollama''s GGML dependency still constitutes a ''llama.cpp wrapper''
  -- both sides are pseudonymous/semi-identified, so the underlying technical reality
  (not just the framing dispute) should be checked against Ollama''s own source tree
  (linked in-thread: https://github.com/ollama/ollama/tree/main/model/models and https://github.com/ggml-org/ggml)
  rather than taken from either side''s assertion.\n- Commenter ''dcreater'': explicit
  production positioning claim -- ''AWS, vLLM, SGLang etc would be the solutions for
  enterprise'' vs. Ollama being ''meaningful for hobbyists and individuals.'' Cites
  an anecdote (unverifiable, pure anecdote) of a startup deploying Ollama on customer
  premises ''because it was easy'' with ''no good reason.''\n- Commenter ''evilduck'':
  concrete friction claims for llama.cpp as the ''easy'' alternative -- no prebuilt
  CUDA binaries for Fedora-based distros (''losing an hour or more''), llama-swap
  requiring a ''new custom yaml config,'' and manual systemd/user/group setup for
  boot-time startup, each costing ''another hour.''\n- Commenter ''steren'' (claims
  internal benchmarking, unpublished at time of posting): ''We benchmarked vLLM and
  Ollama on both startup time and tokens per second. Ollama comes at the top'' --
  caveat: no published data, no stated concurrency/multi-user condition, and ''apitman''
  immediately asks whether multi-user cases were tested (unanswered in-thread).\n-
  Commenter ''sbinnee'': architectural framing claim -- ''vllm and ollama assume different
  settings and hardware. vLLM backed by paged attention expect a lot of requests from
  multiple users whereas ollama is usually for single user on a local machine'' --
  this is the clearest single-sentence articulation in the thread of why Ollama is
  considered unsuited to concurrent/production serving.\n- Commenter ''miki123211'':
  niche production counter-case -- Ollama is ''your best bet'' when an org has no
  datacenter GPU access and must do desktop/clientside deploys (''a desktop with a
  4090 or two is all you can get in some organizations'').'
utility_score: 14.0
---

|   
 | **[Hacker News](https://news.ycombinator.com/news)**[new](https://news.ycombinator.com/newest) | [past](https://news.ycombinator.com/front) | [comments](https://news.ycombinator.com/newcomments) | [ask](https://news.ycombinator.com/ask) | [show](https://news.ycombinator.com/show) | [jobs](https://news.ycombinator.com/jobs) | [submit](https://news.ycombinator.com/submit)  |   |  
| --- | --- |  
 |  
|   
 |  [jnmandal](https://news.ycombinator.com/user?id=jnmandal) | [parent](https://news.ycombinator.com/item?id=44802414) | [context](https://news.ycombinator.com/item?id=44802414#44803629) | [favorite](https://news.ycombinator.com/fave?id=44803629&auth=433624751798277a33c17281056a9d44953e3e70) | on: [Ollama Turbo](https://news.ycombinator.com/item?id=44802414 "Ollama Turbo") I see a lot of hate for ollama doing this kind of thing but also they remain one of the easiest to use solutions for developing and testing against a model locally.Sure, llama.cpp is the real thing, ollama is a wrapper... I would never want to use something like ollama in a production setting. But if I want to quickly get someone less technical up to speed to develop an LLM-enabled system and run qwen or w/e locally, well then its pretty nice that they have a GUI and a .dmg to install.  |  
| --- |  
|   
 |  [mchiang](https://news.ycombinator.com/user?id=mchiang) Thanks for the kind words.Since the new multimodal engine, Ollama has moved off of llama.cpp as a wrapper. We do continue to use the GGML library, and ask hardware partners to help optimize it. Ollama might look like a toy and what looks trivial to build. I can say, to keep its simplicity, we go through a deep amount of struggles to make it work with the experience we want. Simplicity is often overlooked, but we want to build the world we want to see.  |  
| --- |  
 |  
|   
 |  [dcreater](https://news.ycombinator.com/user?id=dcreater) But Ollama is a toy, it's meaningful for hobbyists and individuals to use locally like myself. Why would it be the right choice for anything more? AWS, vLLM, SGLang etc would be the solutions for enterpriseI knew a startup that deployed ollama on a customers premises and when I asked them why, they had absolutely no good reason. Likely they did it because it was easy. That's not the "easy to use" case you want to solve for.  |  
| --- |  
 |  
|   
 |  [mchiang](https://news.ycombinator.com/user?id=mchiang) I can say trying many inference tools after the launch, many do not have the models implemented well, and especially OpenAI’s harmony.Why does this matter? For this specific release, we benchmarked against OpenAI’s reference implementation to make sure Ollama is on par. We also spent a significant amount of time getting harmony implemented the way intended. I know vLLM also worked hard to implement against the reference and have shared their benchmarks publicly.  |  
| --- |  
 |  
|   
 |  [jnmandal](https://news.ycombinator.com/user?id=jnmandal) Honestly, I think it just depends. A few hours ago I wrote I would never want it for a production setting but actually if I was standing something up myself and I could just download headless ollama and know it would work. Hey, that would also be fine most likely. Maybe later on I'd revisit it from a devops perspective, and refactor deployment methodology/stack, etc. Maybe I'd benchmark it and realize its fine actually. Sometimes you just need to make your whole system work.We can obviously disagree with their priorities, their roadmap, the fact that the client isn't FOSS (I wish it was!), etc but no one can say that ollama doesn't work. It works. And like mchiang said above: its dead simple, on purpose.  |  
| --- |  
 |  
|   
 |  [dcreater](https://news.ycombinator.com/user?id=dcreater) But its effectively equally easy to do the same with llama.cpp, vllm or modular..(any differences are small enough that they either shouldn't cause the human much work or can very easily be delegated to AI)  |  
| --- |  
 |  
|   
 |  [evilduck](https://news.ycombinator.com/user?id=evilduck) Llama.cpp is not really that easy unless you're supported by their prebuilt binaries. Go to the llama.cpp GitHub page and find a prebuilt CUDA enabled release for a Fedora based linux distro. Oh there isn't one you say? Welcome to losing an hour or more of your time.Then you want to swap models on the fly. llama-swap you say? You now get to learn a new custom yaml based config file syntax that does basically nothing that the Ollama model file already does so that you can ultimately... have the same experience as Ollama but now you've lost hours just to get back to square one. Then you need it to start and be ready with the system reboot? Great, now you get to write some systemd services, move stuff into system-level folders, create some groups and users and poof, there goes another hour of your time.  |  
| --- |  
 |  
|   
 |  [jnmandal](https://news.ycombinator.com/user?id=jnmandal) Sure but if my some of the development team is using ollama locally b/c it was super easy to install, maybe I don't want to worry about maintaining a separate build chain for my prod env. Many startups are just wrapping or enabling LLMs and just need a running server. Who are we to say what is right use of their time and effort?  |  
| --- |  
 |  
|   
 |  [leopoldj](https://news.ycombinator.com/user?id=leopoldj) > Ollama has moved off of llama.cpp as a wrapper. We do continue to use the GGML libraryWhere can I learn more about this? llama.cpp is an inference application built using the ggml library. Does this mean, Ollama now has it's own code for what llama.cpp does?  |  
| --- |  
 |  
|   
 |  [guipsp](https://news.ycombinator.com/user?id=guipsp) <https://github.com/ollama/ollama/tree/main/model/models>  |  
| --- |  
 |  
|   
 |  [buyucu](https://news.ycombinator.com/user?id=buyucu) This kind of gaslighting is exactly why I stopped using Ollama.GGML library is llama.cpp. They are one and the same. Ollama made sense when llama.cpp was hard to use. Ollama does not have value preposition anymore.  |  
| --- |  
 |  
|   
 |  [mchiang](https://news.ycombinator.com/user?id=mchiang) It’s a different repo. <https://github.com/ggml-org/ggml>The models are implemented by Ollama <https://github.com/ollama/ollama/tree/main/model/models> I can say as a fact, for the gpt-oss model, we also implemented our own MXFP4 kernel. Benchmarked against the reference implementations to make sure Ollama is on par. We implemented harmony and tested it. This should significantly impact tool calling capability. Im not sure if im feeding here. We really love what we do, and I hope it shows in our product, in Ollama’s design and in our voice to our community. You don’t have to like Ollama. That’s subjective to your taste. As a maintainer, I certainly hope to have you as a user one day. If we don’t meet your needs and you want to use an alternative project, that’s totally cool too. It’s the power of having a choice.  |  
| --- |  
 |  
|   
 |  [mark_l_watson](https://news.ycombinator.com/user?id=mark_l_watson) Hello, thanks for answering questions here.Is there a schedule for adding additional models to Turbo mode plan, in addition to gpt-oss 20/120b? I wanted to try your $20/month Turbo plan, but I would like to be able to experiment with a few other large models.  |  
| --- |  
 |  
|   
 |  [buyucu](https://news.ycombinator.com/user?id=buyucu) This is exactly what I mean by gaslighting.GGML is llama.cpp. It it developed by the same people as llama.cpp and powers everything llama.cpp does. You must know that. The fact that you are ignoring it very dishonest.  |  
| --- |  
 |  
|   
 |  [scosman](https://news.ycombinator.com/user?id=scosman) > GGML library is llama.cpp. They are one and the same.Nope…  |  
| --- |  
 |  
|   
 |  [steren](https://news.ycombinator.com/user?id=steren) > I would never want to use something like ollama in a production setting.We benchmarked vLLM and Ollama on both startup time and tokens per seconds. Ollama comes at the top. We hope to be able to publish these results soon.  |  
| --- |  
 |  
|   
 |  [ekianjo](https://news.ycombinator.com/user?id=ekianjo) you need to benchmark against llama.cpp as well.  |  
| --- |  
 |  
|   
 |  [apitman](https://news.ycombinator.com/user?id=apitman) Did you test multi-user cases?  |  
| --- |  
 |  
|   
 |  [jasonjmcghee](https://news.ycombinator.com/user?id=jasonjmcghee) Assuming this is equivalent to parallel sessions, I would hope so, this is like the entire point of vLLM  |  
| --- |  
 |  
|   
 |  [sbinnee](https://news.ycombinator.com/user?id=sbinnee) vllm and ollama assume different settings and hardware. Vllm backed by the paged attention expect a lot of requests from multiple users whereas ollama is usually for single user on a local machine.  |  
| --- |  
 |  
|   
 |  [romperstomper](https://news.ycombinator.com/user?id=romperstomper) It is weird but when I tried new gpt-oss:b20 model locally llama.cpp just failed instantly for me. At the same time under ollama it worked (very slow but anyway). I didn't find how to deal with llama.cpp but ollama definitely doing something under the hood to make models work.  |  
| --- |  
 |  
|   
 |  [miki123211](https://news.ycombinator.com/user?id=miki123211) > I would never want to use something like ollama in a production settingIf you can't get access to "real" datacenter GPUs for any reason and essentially do desktop, clientside deploys, it's your best bet. It's not a common scenario, but a desktop with a 4090 or two is all you can get in some organizations.  |  
| --- |  
 |  
 |  
|  [Consider applying for YC's Winter 2027 batch! _Applications_ are open till November 2.](https://www.ycombinator.com/apply/) [Guidelines](https://news.ycombinator.com/newsguidelines.html) | [FAQ](https://news.ycombinator.com/newsfaq.html) | [Lists](https://news.ycombinator.com/lists) | [API](https://github.com/HackerNews/API) | [Security](https://news.ycombinator.com/security.html) | [Legal](https://www.ycombinator.com/legal/) | [Apply to YC](https://www.ycombinator.com/apply/) | Contact  |
