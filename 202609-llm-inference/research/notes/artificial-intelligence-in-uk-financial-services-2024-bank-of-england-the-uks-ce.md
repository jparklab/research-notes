---
title: Artificial intelligence in UK financial services - 2024 | Bank of England –
  the UK's central bank
id: artificial-intelligence-in-uk-financial-services-2024-bank-of-england-the-uks-ce
tags:
- internal-llm-inference-stacks-7f0c90
- regulatory-disclosure
- regulatory-survey
- audited-absence
created: '2026-10-10T10:58:50.418103Z'
updated: '2026-10-10T14:52:17.277677Z'
source: https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024
source_domain: www.bankofengland.co.uk
fetched_at: '2026-10-10T10:58:50.396999Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Bank of England / FCA third joint survey (published 21 Nov 2024, 118 respondent
  firms across six FS sectors) on AI/ML adoption in UK financial services. Reports
  75% of firms already using AI (up from 58% in 2022), foundation models at 17% of
  all AI use cases, a third of AI use cases are third-party implementations (vs 17%
  in 2022), top-3 third-party model providers covering 44% of reported model-provider
  relationships, 46% of firms report only ''partial understanding'' of the AI they
  use. Entirely aggregate/anonymized statistics across the regulated population --
  zero mentions of ''inference'', ''self-host'', ''open-source'', ''on-premises'',
  ''GPU'', or ''vLLM'' anywhere in the 4,499-word report; no single institution''s
  serving architecture, model choice, or tech stack is named. Directly supports the
  cc-5 null result: even the regulator''s own flagship AI-adoption survey operates
  at the aggregate-statistic level and never surfaces an individual firm''s inference
  architecture.'
---

## Our use of cookies
We use necessary cookies to make our site work. We’d also like to use some non-essential cookies (including third-party cookies) to help us improve the site. By clicking ‘Accept recommended settings’ on this banner, you accept our use of analytics cookies. For more information on how these cookies work please see our [Cookie policy](https://www.bankofengland.co.uk/legal/privacy-and-cookie-policy).
Accept recommended cookies Proceed with necessary cookies only
[Skip to main content](https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024#main-content)
# Artificial intelligence in UK financial services - 2024
The Bank of England and Financial Conduct Authority conducted a third survey of artificial intelligence and machine learning in UK financial services.
Published on 21 November 2024 
## Executive summary
### Use and adoption
  * **75% of firms are already using artificial intelligence (AI), with a further 10% planning to use AI over the next three years.** This is higher than the figures in the 2022 joint Bank of England (Bank) and Financial Conduct Authority (FCA) [Machine learning in UK financial services](https://www.bankofengland.co.uk/report/2022/machine-learning-in-uk-financial-services), of 58% and 14% respectively.
  * **Foundation models form 17% of all AI use cases** supporting anecdotal evidence for the rapid adoption of this complex type of machine learning.


### Third-party exposure
  * **A third of all AI use cases are third-party implementations.[ footnote [1]](https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024#footnote-2) [footnote [2]](https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024#footnote-3)** This is greater than the 17% we found in the 2022 survey and supports the view that third-party exposure will continue to increase as the complexity of models increases and outsourcing costs decrease.
  * **The top three third-party providers account for 73%, 44%, and 33% of all reported cloud, model, and data providers respectively**.


### Automated decision-making
  * **Respondents report that 55% of all AI use cases have some degree of automated decision-making** with 24% of those being semi-autonomous ie while they can make a range of decisions on their own, they are designed to involve human oversight for critical or ambiguous decisions.
  * **Only 2% of use cases have fully autonomous decision-making**.


### Materiality
  * **62% of all AI use cases are rated low materiality by the firms that use them** with 16% rated high materiality.[footnote [3]](https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024#footnote-4)


### Understanding of AI systems
  * **46% of respondent firms reported having only ‘partial understanding’ of the AI technologies they use versus 34% of firms that said they have ‘complete understanding’**. This is largely due to the use of third-party models where respondent firms noted a lack of complete understanding compared to models developed internally.


### Benefits and risks of AI
  * **The highest perceived current benefits are in data and analytical insights, anti-money laundering (AML) and combating fraud, and cybersecurity**. The areas with the largest expected increase in benefits over the next three years are operational efficiency, productivity, and cost base. These findings are broadly in line with the findings from the 2022 survey.
  * **Of the top five perceived current risks, four are related to data:** data privacy and protection, data quality, data security, and data bias and representativeness.
  * **The risks that are expected to increase the most over the next three years are third-party dependencies, model complexity, and embedded or ‘hidden’ models.**
  * The increase in the average perceived benefit over the next three years (21%) is greater than the increase in the average perceived risk (9%).
  * **Cybersecurity is rated as the highest perceived systemic risk both currently and in three years**. The largest increase in systemic risk over that period is expected to be from critical third-party dependencies.


### Constraints
  * **The largest perceived regulatory constraint to the use of AI is data protection and privacy** followed by resilience, cybersecurity and third-party rules and the FCA’s Consumer Duty.
  * **The largest perceived non-regulatory constraint is safety, security and robustness of AI models** , followed by insufficient talent and access to skills.


### Governance and accountability
  * **84% of firms reported having an accountable person for their AI framework**. Firms use a combination of different governance frameworks, controls and/or processes specific to AI use cases – over half of firms reported having nine or more such governance components.
  * While **72% of firms said that their executive leadership were accountable for AI use cases** , accountability is often split with most firms reporting three or more accountable persons or bodies.


## 1: Introduction
### 1.1: Context and objectives
The use of AI has increased in UK financial services over the past few years, both in the number and types of use cases. While AI has many benefits, including improving operational efficiencies and providing customers with personalised services, it can also present challenges to the safety and soundness of firms, the fair treatment of consumers, and the stability of the financial system. Therefore, it is important that the Bank and the FCA maintain an understanding of the capabilities, development, deployment and use of AI in UK financial services.
The Bank and the FCA have undertaken a range of work aimed at furthering our understanding of the use of AI in UK financial services and its implications. This includes the machine learning in UK financial services surveys from [2019](https://www.bankofengland.co.uk/report/2019/machine-learning-in-uk-financial-services) and [2022](https://www.bankofengland.co.uk/report/2022/machine-learning-in-uk-financial-services), the [AI Public-Private Forum](https://www.bankofengland.co.uk/research/fintech/ai-public-private-forum.), a discussion paper [DP5/22 – Artificial Intelligence and Machine Learning](https://www.bankofengland.co.uk/prudential-regulation/publication/2022/october/artificial-intelligence), and the associated feedback statement [FS2/23 – Artificial Intelligence and Machine Learning](https://www.bankofengland.co.uk/prudential-regulation/publication/2023/october/artificial-intelligence-and-machine-learning). In response to the UK Government’s AI White Paper, the Bank published [The Bank and the PRA’s response to DSIT/HMT](https://www.bankofengland.co.uk/prudential-regulation/letter/2024/artificial-intelligence-and-machine-learning-letter) and the FCA an [AI Update Opens in a new window](https://www.fca.org.uk/publication/corporate/ai-update.pdf).
The Artificial Intelligence and Machine Learning Survey 2024 aims to build on existing work to further the Bank and FCA’s understanding of AI in financial services. Specifically, it aims to continue the 2019 and 2022 surveys by providing ongoing insight and analysis into AI use by Bank and/or FCA-regulated firms. In view of generative AI’s growth since the 2022 survey, the 2024 survey incorporated related questions. 
### 1.2: Methodology
The results presented in this report are anonymised and aggregated with respondents grouped into the sectors listed in the table below.
### Table A: Sector classification  
| **Sector**  | **Types of firms included**  |  
| --- | --- |  
| UK banks  | UK deposit-takers, retail banks, building societies  |  
| International banks ()  | International banks operating in the UK  |  
| Insurance  | General insurers, health insurers, life insurers, personal and commercial lines insurers, insurance services providers  |  
| Non-bank lending  | Credit brokers, consumer credit lender, non-bank lenders  |  
| Investment and capital markets  | Alternatives, asset managers, fund managers, wealth managers and stockbrokers, wholesale brokers  |  
| Financial market infrastructures, payments and other  | Financial market infrastructure firms, payments, credit reference agencies, e-money issuers, exchanges, multilateral trading facilities  |  
#### Footnotes
  * Sources: Bank of England and Financial Conduct Authority.
  * (a) Responses from international banks are with reference to their UK operations. [↩ ](https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024#label-table_a_a-0)


The survey received 118 responses with the number of respondents from each sector shown in the chart below.
### Chart 1: A total of 118 firms responded to the survey
Number of respondents by sector
### 1.3: Definitions
For the purposes of the survey and this report, **artificial intelligence is defined as the simulation of human intelligence by machines, including the use of computer systems, which have the ability to perform tasks that demonstrate learning, decision-making, problem solving, and other tasks which previously required human intelligence**.
[Definitions Opens in a new window](https://www.onlinesurveys.fca.org.uk/CP/File.php?F=F_eLJCcMAZjtxchMO) of this and other terms used in the survey and this report are available.
## 2: AI adoption and use
### 2.1: AI use and adoption
Overall, **75% of firms that responded to the survey said they are already using AI** with a further 10% planning to use AI over the next three years. This is an increase on the figures from our 2022 survey which showed that 58% of firms were using AI and a further 14% were planning to do so.
The insurance sector reported the highest percentage of firms currently using AI at 95%, closely followed by international banks at 94%. Data from financial market infrastructure firms responding to the survey, suggest that, at 57%, this is the sector with the lowest percentage of firms currently using AI.
### Chart 2: 85% of respondents are already using or planning to use AI
Percentage of firms using or planning to use AI
Respondents expect the median number of use cases to more than double over the next three years (from 9 to 21). However, there is a notable difference between the large[footnote [4]](https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024#footnote-5) UK and international banks (with a median of 39 and 49 use cases respectively) and the overall median of nine use cases.
In terms of the distribution of use cases, the majority of respondents (56% of those that currently use AI) reported having 10 or fewer use cases, with 10% having more than 50. This picture changes as firms look out to three years with 31% of firms saying they will have 10 or fewer use cases while nearly a quarter expect to have over 50 use cases.
### Chart 3: 56% of respondents currently using AI have 10 or fewer AI use cases
Distribution of firms by number of use cases
### 2.2: Use cases across business areas
Respondent firms are using and planning to use AI across a wide range of business areas. In aggregate, the largest such area, with around 22% of all reported use cases, is operations and IT. This is twice the proportion of the next largest area, namely, retail banking with 11%. General insurance is third with 10% of use cases. The full range of business areas is shown in Chart 4 below.
### Chart 4: Operations and IT is the area with largest percentage of AI use cases
Percentage of use cases by business area, materiality, and external versus internal
### 2.3: Use of foundation models
The survey asked firms for the number of foundation model use cases per business area. The results show that **foundation models account for 17% of all use cases**. Operations and IT is again the area with the largest number of such use cases, accounting for around 30% of all foundation model use cases.
In terms of use of foundation models for each business area, firms’ legal functions have the highest proportion of foundation models at 29% of all models, with the second highest area being human resources (28% of all use cases).
### Chart 5: Foundation models form 17% of all AI models
Percentage of foundation models (as percentage of all models) by business area
### 2.4: Range of use cases
The area with the highest percentage of respondents using AI is optimisation of internal processes (41% of respondents). This is followed by cybersecurity (37%) and fraud detection (33%). 
Over the next three years, an additional 36% of respondents expect to use AI for customer support (including chatbots), 32% for regulatory compliance and reporting, 31% for fraud detection, and 31% for optimisation of internal processes. Note that these are increases over the existing number of use cases.
### Chart 6: 41% of respondents are using AI for optimisation of internal processes
Percentage of firms currently using or planning to use AI
#### Footnotes
  * Note: AML/CFT is ‘anti-money laundering and combating the financing of terrorism’. 


### 2.5: Automated decision-making
Respondents report that 55% of all AI use cases have some degree of automated decision-making with 24% of those being semi-autonomous ie while they can make a range of decisions on their own, they are designed to involve human oversight for critical or ambiguous decisions.
Automation with dynamic models accounted for 2% of AI applications, and fully autonomous decision-making for a further 2%.
### Chart 7: 55% of use cases have some degree of automated decision-making
Percentage of firms with automated decision-making approaches
### 2.6: Use cases by model type
In terms of use cases by model type, gradient boosting models are by far the most popular model type, comprising 32% of all reported use cases. In second place overall are transformer-based models with 10%.
Beyond these there are a wide variety of other model types with a fairly even spread, the closest followers being random forests and decision trees.
Other notable examples include linear models (including generalised linear models); traditional neural network architectures (multilayer perceptron/feedforward neural networks, convolutional neural networks, and recurrent neural networks); clustering and prototype models (eg, K-means, K-Nearest Neighbours); and third-party or proprietary models.
### 2.7: Explainability 
A high proportion of firms currently using AI (81%) employ some kind of explainability method. Feature importance (72%) and Shapley additive explanations (64%) are the most popular explainability approaches. These both aim to explain how much each input variable (or feature) contributes to a machine learning model’s predictions. Feature importance provides a general ranking of features while Shapley values consider all possible combinations of features to assess the impact of each one.
### Chart 8: More than half of firms use three or more explainability methods
Number of explainability methods (percentage of firms)
### 2.8: Third-party implementation
**A third of all current AI use cases deployed by respondents are third-party implementations.** This is significantly higher than the 17% reported in 2022 and is in line with the increase in perceived third-party dependency risk reported by respondents (see Section 4.2 below). The business areas with the highest percentages of third-party implementations were human resources (65%), risk and compliance (64%), and operations and IT (56%).
### Chart 9: One third of use cases are third-party implementations
Percentage of third-party implementations by business area
The survey also asked respondents to list the top three third-party providers they use for each of cloud, models, and data. The survey found that **the top three third-party providers of cloud, models, and data accounted for 73%, 44%, and 33% of all named providers respectively**. While the percentage share of the top three providers for cloud is somewhat lower than it was in the 2022 survey, the share of the top three **model** providers is significantly higher than the 18% figure in 2022. The figure for data has also increased meaningfully from the 2022 percentage of 25%.
### Chart 10: Top three model providers account for 44% of all named providers
Percentage of all third-party providers for cloud, model and data
The survey asked firms how the evaluation and integration of third-party AI products or services into their existing systems differs from those for non-AI products and services. Some respondents reported use of existing frameworks for the evaluation and integration of third-party AI systems. Some also had additional conditions or considerations specific to AI systems.
### 2.9: Materiality of applications
The survey asked respondent firms to rate the number of use cases by level of materiality. We defined materiality as a rating of the use case's impact, which could include (a) quantitative size-based measures, for example, exposure, book or market value, or number of customers to which a model applies, and (b) qualitative factors relating to the purpose of the model and its relative importance to informing business decisions, and considering the potential impact on the firm's solvency and financial performance.
#### Materiality distribution
Of the total number of use cases reported by respondent firms (both internally developed and externally implemented with third parties), 62% were rated as low materiality, 22% as medium and 16% as high. Low and medium materiality use cases were most common in operations and IT. High materiality use cases were most common in general insurance, risk and compliance, and retail banking. 
### Chart 11: 62% of all use cases are rated low materiality
Percentage of use cases by materiality, all use cases and foundation model use cases
#### Materiality – use of external models
One third (33%) of AI use cases are reported as external or third-party implementations. Of these, 62% are rated as being low materiality, 22% as medium, and 16% as high materiality. Of the high materiality use cases, a significant proportion are in use in operations and IT, and in risk and compliance. 
### Chart 12: 62% of all use cases are rated low materiality
Percentage of all use cases by materiality and external versus internal
#### Materiality – use of foundation models
Of the total number of AI use cases, 17% are foundation models. Of these, 71% are rated as low materiality, 17% medium and 12% high materiality. Of the high-materiality foundation model use cases, the largest proportion are in operations and IT (25%), retail banking (18%) and 11% in each of research and risk and compliance.
### Chart 13: 71% of all foundation model use cases are low materiality
Percentage of foundation model use cases by materiality and external versus internal
## 3: Strategies and governance 
### 3.1: Governance and accountability
#### Range of governance frameworks
A variety of approaches to AI governance are used by respondent firms. We provided a list of 16 approaches, and asked firms which of them they used specifically for AI applications. Respondents could select multiple answers, and these were not connected to specific model types. The most commonly used governance framework, control or process specific to AI was to have an accountable person or persons with responsibility for the AI framework (84% of firms currently using AI). This was followed closely by the use of an AI framework, principles, guidelines or best practice (82%) and data governance (79%).
#### Accountability
In terms of accountability, 72% of firms using or planning to use AI stated that they allocate accountability for AI use cases and their outputs to executive leadership. This is followed by developers and data science teams (64%), and business area users (57%).
### 3.2: Data management
Responses show that data management and governance is a key concern for firms, and that in most cases data management practices are not AI-specific. Change management practices were cited by 87% of respondents with 71% being not AI-specific and 16% being AI-specific. Data privacy and security continues to be a priority, with 19% of respondent firms using AI-specific practices and 66% using non-AI-specific practices. Other areas include data architecture and infrastructure (16% use AI-specific practices, 68% non-AI specific). Data ethics, bias, and fairness is the area with the highest proportion of respondents citing AI-specific practices at 34%, with 40% using non-AI-specific practices.
### Chart 14: Change management practices were cited by 87% of respondents
Percentage of firms with particular data management practices
### 3.3: Firms’ assessments of their own models
Firms describe considering a broad range of factors when assessing **the complexity of their AI models**. These include business need, evaluating how appropriate a particular type of model is to the business objective. More than half of respondents use complexity tests, some of which are built into existing processes and some of which are AI-specific. AI-specific tests tend to include consideration of methodology, data, complexity of code, interpretability, parameter count and frequency of use. Complexity of data is also a central factor, particularly where large and multi-dimensional or multi-modal data sets are involved. 
Firms were asked to rate specific metrics for **monitoring model effectiveness** , with the most common being accuracy, precision, recall and sensitivity (reported by 88% of firms using AI), operational efficiency (74%), and model robustness and stability (72%).
### 3.4: Firms’ understanding of AI technologies
Respondents were asked how they would describe their firm’s understanding of the AI technologies implemented in their operations (whether developed in-house or procured externally).Of the firms using or planning to use AI over the next three years, **46% reported having only ‘partial understanding’ of the AI technologies they use versus 34% of firms that said they have ‘complete understanding’**. This is largely due to the use of third-party models where respondent firms noted a lack of complete understanding compared to models developed internally.
## 4: Benefits, risks and constraints
### 4.1: Benefits now and in three years
The survey asked firms to rate, on a scale of 1 to 5, the extent to which AI is or could be beneficial in a number of areas. The chart below summarises the responses and shows that **the areas with the highest perceived current benefit are data and analytical insights, AML and combating fraud, and cybersecurity**.
While benefits are expected to grow in all areas, the areas with the largest expected increase over the next three years are operational efficiency, productivity, and cost base. Across all areas, the average benefit is rated as slightly lower than medium and increasing over the next three years to slightly higher than medium.
### Chart 15: Data and analytical insights is the highest perceived benefit of AI
Perceived benefits of AI now and in three years
###  4.2: Risks for firms and consumers
The survey also asked respondents to rate a set of risks and drivers of risk on a scale of 1 to 5. The chart below summarises responses and shows that **four of the top five risks are related to the use of data**. The three biggest current risks are seen to be data privacy and protection, data quality, and data security. **The risks that are expected to increase the most over the next three years are third-party dependencies, model complexity, and embedded or ‘hidden’ models**. Across all risks, the average level is slightly above medium, with respondents judging the level of risk will increase somewhat over the next three years.
Note that the increase in average expected benefits over the next three years (21%) is greater than the increase in average expected risk (9%).
### Chart 16: Four of the top five risks are data-related risks
Perceived risks of AI now and in three years
### 4.3: Systemic risks related to the use of AI
**Cybersecurity was ranked by respondents as the highest potential systemic risk and respondents expected it to remain the highest in three years’ time**. Critical third-party dependencies were ranked the second highest risk with common data sets and/or models in third place. The greatest expected increase in perceived risk is in critical third-party dependencies.
### Chart 17: Cybersecurity is the highest systemic risk
Perceived systemic risks now and in three years
### 4.4: Regulatory constraints to the adoption of AI
Data protection and privacy was noted by respondents as the greatest regulatory constraint, with 23% identifying it as a large constraint, 29% as medium, and 10% as a small constraint. Other notable regulatory constraints include resilience and cyber security rules (12% of firms consider it a large constraint, 22% medium, 17% small), and FCA Consumer Duty and conduct (5% large, 21% medium, 23% small).
### Chart 18: Data protection and privacy is the top regulatory constraint
Regulatory constraints by extent of constraint
**High regulatory burden** is considered the main type of regulatory constraint, with 33% of firms noting it for data protection and privacy, 23% for the FCA’s Consumer Duty, and 20% for other FCA regulations. **Lack of clarity in current regulation** is seen by 18% of firms to be a type of regulatory constraint in relation to intellectual property rights, followed by lack of clarity in relation to the FCA Consumer Duty (13% of firms) and resilience and cyber security rules (11%). Only 5% of firms consider **lack of alignment between UK and international regulation** to be a type of constraint for data protection and privacy.
### Chart 19: Type of regulatory constraint
Regulatory constraint by type of constraint
### 4.5: Non-regulatory constraints to AI adoption
**The top three non-regulatory constraints were rated as safety, security and robustness, insufficient talent/access to skills, and appropriate transparency and explainability**. Safety, security and robustness was considered by 19% of firms to be a large constraint, by 32% to be medium, and by 30% to be a small constraint. Insufficient talent/access to skills was considered by 25% of firms to be a large constraint, by 32% to be medium, and by 24% to be a small constraint. Appropriate transparency and explainability was considered by 16% to be a large constraint, by 38% to be medium, and by 25% to be a small constraint.
### Chart 20: Safety, security and robustness is the greatest non-regulatory constraint
Non-regulatory constraints by extent of constraint
## 5: Acknowledgements
The authors of this report are Mohammed Gharbawi, Ewa Ward (Bank), Emelie Bratt, Laurence Diver, Henrike Mueller, Rocco Quartu, Haydn Robinson (FCA).
We are grateful to Tom Mutton, Amy Lee, Iremide Sonubi, Seema Visavadia (Bank), Jessica Rusu, Ian Phoenix and Edmund Towers (FCA) for their helpful comments and support.
We are also grateful to colleagues from across the Bank, FCA and PRA for their input, including the supervisors of surveyed firms for their support throughout the process. Finally, we would like to thank all of the firms that participated in the survey for their input.
  1. The case where most of the development or deployment processes of the AI application are implemented by a third party. 
  2. [Definitions Opens in a new window](http://www.onlinesurveys.fca.org.uk/CP/File.php?F=F_eLJCcMAZjtxchMO) of terms used in this report and the survey are available. 
  3. Materiality is a rating of the use case impact which could include quantitative and qualitative measures. The full definition can be found in the [definitions Opens in a new window](https://www.onlinesurveys.fca.org.uk/CP/File.php?F=F_eLJCcMAZjtxchMO). 
  4. ‘Large’ denotes Category 1 firms. The [PRA’s approach to banking supervision Opens in a new window](https://www.bankofengland.co.uk/-/media/boe/files/prudential-regulation/approach/banking-approach-2023.pdf) defines Category 1 firms as the most significant firms whose size, interconnectedness, complexity, and business type give them the capacity to cause very significant disruption to the UK financial system (and through that to economic activity more widely) by failing, or by carrying on their business in an unsafe manner. 


## Footnote [4]
‘Large’ denotes Category 1 firms. The [PRA’s approach to banking supervision Opens in a new window](https://www.bankofengland.co.uk/-/media/boe/files/prudential-regulation/approach/banking-approach-2023.pdf) defines Category 1 firms as the most significant firms whose size, interconnectedness, complexity, and business type give them the capacity to cause very significant disruption to the UK financial system (and through that to economic activity more widely) by failing, or by carrying on their business in an unsafe manner. 
Close
## Footnote [3]
Materiality is a rating of the use case impact which could include quantitative and qualitative measures. The full definition can be found in the [definitions Opens in a new window](https://www.onlinesurveys.fca.org.uk/CP/File.php?F=F_eLJCcMAZjtxchMO). 
Close
## Footnote [2]
[Definitions Opens in a new window](http://www.onlinesurveys.fca.org.uk/CP/File.php?F=F_eLJCcMAZjtxchMO) of terms used in this report and the survey are available. 
Close
## Footnote [1]
The case where most of the development or deployment processes of the AI application are implemented by a third party. 
Close
## Other publications
[ Publication // Working Paper 28 August 2026 How do firms cope with economic shocks in... How do firms cope with economic shocks in real time? ](https://www.bankofengland.co.uk/working-paper/2026/how-do-firms-cope-with-economic-shocks-in-real-time)
[ Publication // Working Paper 28 August 2026 Locking in the rate or staying flexible?... Locking in the rate or staying flexible? Mortgage refinancing around an interest rate shock ](https://www.bankofengland.co.uk/working-paper/2026/locking-in-the-rate-or-staying-flexible-mortgage-refinancing-around-an-interest-rate-shock)
[ Publication // Working Paper 28 August 2026 A welfare analysis of the central bank balance... A welfare analysis of the central bank balance sheet ](https://www.bankofengland.co.uk/working-paper/2026/a-welfare-analysis-of-the-central-bank-balance-sheet)
[ Publication // Weekly report 27 August 2026 Bank of England Weekly Report 26 August 2026 Bank of England Weekly Report 26 August 2026 ](https://www.bankofengland.co.uk/weekly-report/2026/26-august-2026)
[View more Other publications](https://www.bankofengland.co.uk/news?NewsTypes=ce90163e489841e0b66d06243d35d5cb&SearchTerm=&InfiniteScrolling=False&Direction=Latest)
[Back to top](https://www.bankofengland.co.uk/report/2024/artificial-intelligence-in-uk-financial-services-2024)
