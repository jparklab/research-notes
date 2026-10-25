---
title: ARMY INTELLIGENCE | Article | The United States Army
id: army-intelligence-article-the-united-states-army
tags:
- onprem-llm-inference-stacks-859099
- locus-sovereignty-regulation-forces-onprem
- camogpt
- onprem-classified
created: '2026-10-23T17:56:38.000307Z'
updated: '2026-10-23T18:21:08.228220Z'
source: https://www.army.mil/article/287605/army_intelligence
source_domain: www.army.mil
fetched_at: '2026-10-23T17:56:37.980986Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Army AI2C article: CamoGPT runs on NIPRNet and SIPRNet, ~75,000 users; Army
  ''simply host[s] published, open-source models ... Meta, Google and Mistral''. No
  GPU/engine/model-size disclosure.'
---

An official website of the United States government _Here's how you know_
  * **Official websites use .mil** A **.mil** website belongs to an official U.S. Department of Defense organization in the United States. 
  * **Secure .mil websites use HTTPS** A **lock ( )** or **https://** means you've safely connected to the .mil website. Share sensitive information only on official, secure websites. 


MENU SEARCH
ABOUT 


  * [Army Worldwide](https://www.army.mil/news)
  * [Press Releases](https://www.army.mil/news/newsreleases)
  * [Soldier Features](https://www.army.mil/soldiers)


MULTIMEDIA 


LEADERS 
  * [Vice Chief of Staff](https://www.army.mil/leaders/vcsa/)
  * [Sergeant Major of the Army](https://www.army.mil/leaders/sma/)


FEATURES 


  * [Official U.S. Army Facebook](https://www.facebook.com/USArmy "social media link - facebook")
  * [Official U.S. Army Twitter](https://twitter.com/USArmy "social media link - twitter")
  * [Official U.S. Army Youtube](https://www.youtube.com/USarmy "social media link - youtube")
  * [Official U.S. Army Instagram](https://www.instagram.com/usarmy/ "social media link - instagram")
  * [Official U.S. Army LinkedIn](https://www.linkedin.com/company/us-army "social media link - linkedin")




# ARMY INTELLIGENCE
By Holly Comanse for Army AL&T MagazineAugust 11, 2025


Collen Roller, a senior computer scientist at the Air Force Research Laboratory, at his desk. Roller is the creator of NIPRGPT, an AI platform utilized by the Air Force.  (Photo Credit: Albert P. Santacroce, Air Force Research Laboratory) [View original](https://api.army.mil/e2/c/images/2025/08/05/b8315f5d/original.jpg "View original")
> **WORK IS NEVER DONE -**_Collen Roller, a senior computer scientist at the Air Force Research Laboratory, at his desk. Roller is the creator of NIPRGPT, an AI platform utilized by the Air Force. (Photo by Albert P. Santacroce, Air Force Research Laboratory)_
##### **Inside the development and adoption of AI chatbots—most notably, the Army’s new CamoGPT.**
Artificial intelligence (AI) may be trending, but it’s nothing new. Alan Turing, an English mathematician said to be the “[father of theoretical computer science](https://www.britannica.com/biography/Alan-Turing/Computer-designer),” conceptualized the term algorithm to solve mathematical problems and [test machine intelligence](https://www.coursera.org/articles/history-of-ai?msockid=3c4d2a2344af69f436053fc9452068fc). Not long after, the very first AI chatbot, called ELIZA, was released in 1966. However, it was the generative pre-trained transformer (GPT) model, first introduced in 2018, that provided the foundation for modern AI chatbots.
Both AI and chatbots continue to evolve with many unknown variables related to accuracy and ethical concerns, such as privacy and bias. Job security and [the environmental impact of AI](https://www.unep.org/news-and-stories/story/ai-has-environmental-problem-heres-what-world-can-do-about) are other points of contention. While the unknown may be distressing, it’s also an opportunity to adjust and adapt to an evolving era.
The 1962 television cartoon “The Jetsons” presented the concept of a futuristic family in the year 2062, with high-tech characters that included Rosie the Robot, the family’s maid, and an intelligent robotic supervisor named Uniblab. Such a prediction isn’t much of a stretch anymore. Robotic vacuums, which can autonomously clean floors, and other personal AI assistant devices are now commonplace in the American household. A recent [report](https://www.marketsandmarkets.com/Market-Reports/smart-homes-and-assisted-living-advanced-technologie-and-global-market-121.html) valued the global smart home market at $84.5 billion in 2024 and predicted it would reach $116.4 billion by 2029.
ChatGPT, released in 2022, is a widely used AI chat-based application with [400 million active users](https://www.businessofapps.com/data/chatgpt-statistics/). It independently browses the internet, allowing for more up-to-date and automatic results, but it’s used for conversational topics, not industry-specific information. The popularity and limitations of ChatGPT led some organizations to develop their own AI chatbots with the ability to reflect current events, protect sensitive information and deliver search results for company-specific topics.
One of those organizations is the U.S. Army, which now has an Army-specific chatbot known as CamoGPT. Currently boasting 75,000 users, CamoGPT started development in the fall of 2023 and was first deployed in the spring of 2024. Live data is important for the Army and other companies that choose to implement their own AI chat-based applications. CamoGPT does not currently have access to the internet because it is still in a prototype phase, but connecting to the net is a goal. Another goal for the platform is to accurately respond to questions that involve current statistics and high-stakes information. What’s more, CamoGPT can process classified data on SIPRNet and unclassified information on NIPRNet.
**THE MORE THE MERRIER** Large language models (LLMs) are a type of AI chatbot that can understand and generate human language based on inputs. LLMs undergo extensive training, require copious amounts of data and can be tedious to create. They can also process and respond to information as a human would. Initially, the information fed to the bot must be input individually and manually by human beings until a pattern is established, at which point the computer can take over. Updating facts can be a daunting task when considering the breadth of data from around the world that AI is expected to process.
Work is increasingly completed using computers and digital apps, as demonstrated by Staff Sgt. Andrew Schede, 105th Communications Squadron cyber systems operations, in the communications building at Stewart Air National Guard Base, New York, on March 6, 2025.  (Photo Credit: Senior Airman Sarah Post, 105th Airlift Wing) [View original](https://api.army.mil/e2/c/images/2025/08/05/916a3a4e/original.jpg "View original")
> **TECH-FOCUSED -**_Work is increasingly completed using computers and digital apps, as demonstrated by Staff Sgt. Andrew Schede, 105th Communications Squadron cyber systems operations, in the communications building at Stewart Air National Guard Base, New York, on March 6, 2025. (Photo by Senior Airman Sarah Post, 105th Airlift Wing)_
Aidan Doyle, a data engineer at the Army Artificial Intelligence Integration Center (AI2C), works on a team of three active-duty service members, including another data engineer and a data analyst, as well as four contracted software developers and one contracted technical team lead. “It’s a small team, roles are fluid, [and] everyone contributes code to Camo[GPT],” Doyle said.
Doyle’s team is working to transition CamoGPT into a program of record and put more focus into developing an Army-specific LLM. “An Army-specific LLM would perform much better at recognizing Army acronyms and providing recommendations founded in Army doctrine,” Doyle said. “Our team does not train LLMs; we simply host published, open-source models that have been trained by companies like Meta, Google and Mistral.”
Civilian professionals learning to adapt to fast-evolving technology, including AI, during their final training session on April 17, 2025, at Fort Sill in Oklahoma.  (Photo Credit: Edward Muniz, Fort Sill Public Affairs) [View original](https://api.army.mil/e2/c/images/2025/08/05/c27950f7/original.jpg "View original")
> **CHANGE WITH THE TIMES -**_Civilian professionals learning to adapt to fast-evolving technology, including AI, during their final training session on April 17, 2025, at Fort Sill in Oklahoma. (Photo by Edward Muniz, Fort Sill Public Affairs)_
The process of training LLMs involves pre-training the model by showing it as many examples of natural language as possible from across the internet. Everything from greetings to colloquialisms must be input so it can mimic human conversation. Sometimes, supervised learning is necessary for specific information during the fine-tuning step. Then the model generates different answers to questions, and humans evaluate and annotate the model responses and flag problems that arise. Once preferred responses are identified, developers adjust the model accordingly. This is a post-training step called reinforcement learning with human feedback, or alignment. Finally, the model generates both the questions and answers itself in the self-play step. When the model is ready, it is deployed.
**A LITTLE TOO CREATIVE** The use of AI in creative fields faces significant challenges, such as the potential for plagiarism and inaccuracy. Artists spend a lot of time creating work that can easily be duplicated by AI without giving the artist any credit. AI can also repackage copyrighted material. It can be tough to track down the original source for something when it is generated with AI.
AI can—and sometimes does—introduce inaccuracies. When AI fabricates information unintentionally, it is called a hallucination. AI can make a connection between patterns it recognizes and pass them off as truth for a different set of circumstances. Facts presented by AI should always be verified, and websites such as CamoGPT often come with a disclaimer that hallucinations are possible. Journalists and content creators should be cautious with AI use as they run the risk of spreading misinformation.
Images and videos can also be intentionally manipulated. For example, AI-generated content on social media can be posted for shock value. Sometimes it’s easy to spot when something has been created by AI, and you can take it with a grain of salt. In other cases, social media trends can go viral before they are vetted.
For these reasons, the Army decided to implement CamoGPT. Not only does it currently have the ability to process classified information discreetly, but developmental advances will also ensure that CamoGPT provides minimal errors in its responses.
**CONCLUSION** It’s becoming clear that analog is on the way out. Even older websites like Google and other search engines have started to prioritize AI summary results. Utilizing technology like AI, LLMs and other chatbots can save time and automate tedious tasks, which increases productivity and efficiency. CamoGPT is still evolving, and the team at AI2C is working hard to improve its accuracy and abilities. Other AI systems within the Army are still being developed, but the potential is limitless. While we may not be living in the future that the creator of “The Jetsons” predicted, we’re getting closer. In another 37 years, when 2062 rolls around, we may all be using flying cars—and those vehicles just might drive themselves with the help of AI.
_For more information, go to_ _<https://www.camogpt.army.mil/camogpt>__._
Follow us @USAASC
_HOLLY COMANSE provides contract support to the U.S. Army Acquisition Support Center from Honolulu, Hawaii, as a writer and editor for Army AL &T magazine and TMGL, LLC. She previously served as a content moderator and data specialist training artificial intelligence for a news app. She holds a B.A. in journalism from the University of Nevada, Reno._
#### TOP STORIES
  * [October 16, 2026Generational food changes optimize warfighter performance](https://www.army.mil/article/296172/generational_food_changes_optimize_warfighter_performance "Generational food changes optimize warfighter performance")
  * [October 16, 2026Army task force partners with acquisition directorate to accelerate counter-drone tech in Africa ](https://www.army.mil/article/296161/army_task_force_partners_with_acquisition_directorate_to_accelerate_counter_drone_tech_in_africa "Army task force partners with acquisition directorate to accelerate counter-drone tech in Africa ")
  * [October 19, 2026Honoring their service: Soldier helps veterans receive overdue recognition](https://www.army.mil/article/296198/honoring_their_service_soldier_helps_veterans_receive_overdue_recognition "Honoring their service: Soldier helps veterans receive overdue recognition")
  * [October 16, 2026American ingenuity at work: The Army's Strategic Capital Initiative](https://www.army.mil/article/296173/american_ingenuity_at_work_the_armys_strategic_capital_initiative "American ingenuity at work: The Army&#39;s Strategic Capital Initiative")
  * [October 15, 2026Florida, Alabama National Guard support Isaias response and recovery efforts](https://www.army.mil/article/296149/florida_alabama_national_guard_support_isaias_response_and_recovery_efforts "Florida, Alabama National Guard support Isaias response and recovery efforts")
  * [October 15, 2026Army training site helps prepare astronauts for lunar landings](https://www.army.mil/article/296165/army_training_site_helps_prepare_astronauts_for_lunar_landings "Army training site helps prepare astronauts for lunar landings")
  * [October 14, 2026Vice Chief to Soldiers: 'I’m trusting you'](https://www.army.mil/article/296115/vice_chief_to_soldiers_im_trusting_you "Vice Chief to Soldiers: &#39;I’m trusting you&#39;")
  * [October 8, 2026Send Me: The making of a Soldier | Week 2](https://www.army.mil/article/296019/send_me_the_making_of_a_soldier_week_2 "Send Me: The making of a Soldier | Week 2")


  * [ WORK IS NEVER DONE ](https://www.army.mil/article/287605/army_intelligence)
  * [ TECH-FOCUSED ](https://www.army.mil/article/287605/army_intelligence)
  * [ CHANGE WITH THE TIMES ](https://www.army.mil/article/287605/army_intelligence)


## Social Sharing


