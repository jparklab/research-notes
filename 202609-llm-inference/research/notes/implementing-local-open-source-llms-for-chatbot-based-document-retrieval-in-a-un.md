---
title: Implementing Local Open-Source LLMs for Chatbot-Based Document Retrieval in
  a University Hospital
id: implementing-local-open-source-llms-for-chatbot-based-document-retrieval-in-a-un
tags:
- onprem-llm-inference-stacks-859099
- locus-sovereignty-regulation-forces-onprem
- hospital-onprem
created: '2026-10-23T17:57:56.517464Z'
updated: '2026-10-23T17:58:40.142357Z'
source: https://conferences.publisso.de/en/publisso_gold/export/conference/abstract/blobs/38158
source_domain: conferences.publisso.de
fetched_at: '2026-10-23T17:57:56.499046Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'GMDS 2025 abstract, Univ. Hospital Halle: local Linux server with one Nvidia
  H100, Falcon-7B-Instruct + RAG (jina-embeddings-v3, BM25); 287 queries; 75% responses
  <5s. No serving engine named.'
doi: 10.5555/3495724.3496517
---

25gmds197 10.3205/25gmds197 urn:nbn:de:0183-25gmds1976 Meeting Abstract Implementing Local Open-Source LLMs for Chatbot-Based Document Retrieval in a University Hospital Demus Demus Christoph C Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany author Bossenz Bossenz Jan J Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany author Günzl Günzl Carlo C Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany author Weise Weise Annemarie A Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany author Berns Berns Fabian F medicalvalues GmbH, Karlsruhe, Germany author Jäger Jäger Christian C Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany author Weber Weber Thomas T University Hospital Halle (Saale), Department of Internal Medicine IV, Hematology and Oncology, Halle (Saale), Germany author Kirchhoff Kirchhoff Jan J medicalvalues GmbH, Karlsruhe, Germany author Christoph Christoph Jan J Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany author German Medical Science GMS Publishing House Düsseldorf 610 large language models (LLM) natural language processing artificial intelligence (AI) information retrieval retrieval-augmented generation 20260401 engl This is an Open Access article distributed under the terms of the Creative Commons Attribution 4.0 License. Dieser Artikel ist ein Open-Access-Artikel und steht unter den Lizenzbedingungen der Creative Commons Attribution 4.0 License (Namensnennung). M0631 197 Deutsche Gesellschaft für Medizinische Informatik, Biometrie und Epidemiologie 70. Jahrestagung der Deutschen Gesellschaft für Medizinische Informatik, Biometrie und Epidemiologie e. V. (GMDS) V: Large language models & medical texts 1 Jena 20250907 20250911 Abstr. 217 TextIntroduction: The integration of large language models (LLMs) into healthcare, with its strict data protection and security requirements, poses particular challenges, which is why powerful proprietary models such as ChatGPT are not suitable, even if they are increasingly being used informally. To close this gap, this project presents a locally hosted open-source LLM and retrieval-augmented generation (RAG) system that enables interaction with a locally stored, extensive internal document library at the University Hospital Halle via a chatbot.Methods: The system is hosted on a local Linux server equipped with an Nvidia H100 GPU and utilizes the Falcon-7B-Instruct model. Internal documents - primarily SOPs, procedural guidelines and forms - were cleaned, contextually chunked, and embedded using the jina-embeddings-v3 model. The system retrieves documents based on combined cosine similarity and BM25 scoring. Evaluation involved (1) subjective user feedback collected during a one-month test phase (2) expert annotation of chatbot responses to 38 manually crafted evaluation questions and (3) a technically analysis of the response time.Results: During the test phase the system handled 287 user queries, receiving 72 feedback entries (25.01% response rate). Feedback on document relevance was generally more positive (59.26%) than the generated textual answers (43.50%). Expert evaluation using the test dataset yielded an average helpfulness rating of 72%. On a scale from one (worse) to five (best) expert evaluation reached a score of 3.72 for textual answers and 3.89 for retrieved documents. Technical performance showed acceptable latency, with 75% of responses returned in under five seconds. First queries in a session averaged 3.71s, while follow-ups averaged with 4.28s slightly longer response times due to an additional LLM-request in the retrieval phase.Discussion: Initial results show that even small LLMs can yield useful results when coupled with well-designed retrieval systems. Although user feedback was mixed, it reflected the early prototype status and the subjective nature of expectations. Document retrieval quality was consistently rated higher than the textual synthesis, highlighting limitations in inter-document reasoning by smaller LLMs. Evaluation discrepancies between users and expert annotators likely stem from varied prompt wording, query complexity and expectations. Response time analysis confirmed that the system operates within acceptable bounds for real-world usage.Conclusion: This proof-of-concept demonstrates the viability of a locally hosted LLM-based chatbot for internal document retrieval in healthcare settings. While textual response quality can be further improved, this proof of concept system already offers a useful alternative to basic keyword search. Future improvements should focus on the improvement of the retrieval process, metadata enrichment, and advanced reasoning capabilities. Moreover, in order to facilitate a comprehensive introduction to the system, it would be essential to establish appropriate management protocols to regulate document access by various individuals. Overall, the project underscores the feasibility and potential of secure, on-premise AI tools in clinical environments.Jan Kirchhoff and Fabian Berns are employed by medicalvalues GmbH, which provided consulting support during the design and development of the system. Jan Kirchhoff is the managing director of medicalvalues GmbH.The authors declare that an ethics committee vote is not required. Lewis P Retrieval-augmented generation for knowledge-intensive NLP tasks Proceedings of the 34th International Conference on Neural Information Processing Systems NIPS 20 Lewis P, et al. Retrieval-augmented generation for knowledge-intensive NLP tasks. In: Proceedings of the 34th International Conference on Neural Information Processing Systems NIPS 20. Vancouver, BC, Canada: Curran Associates Inc., 2020. DOI: 10.5555/3495724.3496517 https://doi.org/10.5555/3495724.3496517 Muti HS Customizing GPT-4 for clinical information retrieval from standard operating procedures 2024 medRxiv Muti HS, et al. Customizing GPT-4 for clinical information retrieval from standard operating procedures. medRxiv. 2024. DOI: 10.1101/2024.06.24.24309221 http://dx.doi.org/10.1101/2024.06.24.24309221 Amugongo LM Mascheroni P Brooks SG Doering S Seidel J Retrieval Augmented Generation for Large Language Models in Healthcare: A Systematic Review 2024 Preprints Amugongo LM, Mascheroni P, Brooks SG, Doering S, Seidel J. Retrieval Augmented Generation for Large Language Models in Healthcare: A Systematic Review. Preprints. Jul 2024. DOI: 10.20944/preprints202407.0876.v1 http://dx.doi.org/10.20944/preprints202407.0876.v1 Huang L A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions 2025 ACM Transactions on Information Systems 1–55 Huang L, et al. A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions. ACM Transactions on Information Systems. 2025 Jan;43(2):1–55. DOI: 10.1145/3703155 http://dx.doi.org/10.1145/3703155
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<GmsArticle xmlns:xlink="http://www.w3.org/1999/xlink">
<MetaData>
<Identifier>25gmds197</Identifier>
<IdentifierDoi>10.3205/25gmds197</IdentifierDoi>
<IdentifierUrn>urn:nbn:de:0183-25gmds1976</IdentifierUrn>
<ArticleType>Meeting Abstract</ArticleType>
<TitleGroup>
<Title language="en">Implementing Local Open-Source LLMs for Chatbot-Based Document Retrieval in a University Hospital</Title>
...
</TitleGroup>
<CreatorList>
<Creator>
<PersonNames>
<Lastname>Demus</Lastname>
<LastnameHeading>Demus</LastnameHeading>
<Firstname>Christoph</Firstname>
<Initials>C</Initials>
...
</PersonNames>
<Address>
<Affiliation>Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Bossenz</Lastname>
<LastnameHeading>Bossenz</LastnameHeading>
<Firstname>Jan</Firstname>
<Initials>J</Initials>
...
</PersonNames>
<Address>
<Affiliation>Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Günzl</Lastname>
<LastnameHeading>Günzl</LastnameHeading>
<Firstname>Carlo</Firstname>
<Initials>C</Initials>
...
</PersonNames>
<Address>
<Affiliation>Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Weise</Lastname>
<LastnameHeading>Weise</LastnameHeading>
<Firstname>Annemarie</Firstname>
<Initials>A</Initials>
...
</PersonNames>
<Address>
<Affiliation>Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Berns</Lastname>
<LastnameHeading>Berns</LastnameHeading>
<Firstname>Fabian</Firstname>
<Initials>F</Initials>
...
</PersonNames>
<Address>
<Affiliation>medicalvalues GmbH, Karlsruhe, Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Jäger</Lastname>
<LastnameHeading>Jäger</LastnameHeading>
<Firstname>Christian</Firstname>
<Initials>C</Initials>
...
</PersonNames>
<Address>
<Affiliation>Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Weber</Lastname>
<LastnameHeading>Weber</LastnameHeading>
<Firstname>Thomas</Firstname>
<Initials>T</Initials>
...
</PersonNames>
<Address>
<Affiliation>University Hospital Halle (Saale), Department of Internal Medicine IV, Hematology and Oncology, Halle (Saale), Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Kirchhoff</Lastname>
<LastnameHeading>Kirchhoff</LastnameHeading>
<Firstname>Jan</Firstname>
<Initials>J</Initials>
...
</PersonNames>
<Address>
<Affiliation>medicalvalues GmbH, Karlsruhe, Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
<Creator>
<PersonNames>
<Lastname>Christoph</Lastname>
<LastnameHeading>Christoph</LastnameHeading>
<Firstname>Jan</Firstname>
<Initials>J</Initials>
...
</PersonNames>
<Address>
<Affiliation>Junior Research Group (Bio-)Medical Data Science, Faculty of Medicine, Martin-Luther-University Halle-Wittenberg, Halle (Saale), Germany</Affiliation>
...
</Address>
<Creatorrole corresponding="no" presenting="no">author</Creatorrole>
...
</Creator>
...
</CreatorList>
<PublisherList>
<Publisher>
<Corporation>
<Corporatename>German Medical Science GMS Publishing House</Corporatename>
...
</Corporation>
<Address>Düsseldorf</Address>
...
</Publisher>
...
</PublisherList>
<SubjectGroup>
<SubjectheadingDDB>610</SubjectheadingDDB>
<Keyword language="en">large language models (LLM)</Keyword>
<Keyword language="en">natural language processing</Keyword>
<Keyword language="en">artificial intelligence (AI)</Keyword>
<Keyword language="en">information retrieval</Keyword>
<Keyword language="en">retrieval-augmented generation</Keyword>
...
</SubjectGroup>
<DatePublishedList>
<DatePublished>20260401</DatePublished>
...
</DatePublishedList>
<Language>engl</Language>
<License license-type="open-access" xlink:href="http://creativecommons.org/licenses/by/4.0/">
<AltText language="en">This is an Open Access article distributed under the terms of the Creative Commons Attribution 4.0 License.</AltText>
<AltText language="de">Dieser Artikel ist ein Open-Access-Artikel und steht unter den Lizenzbedingungen der Creative Commons Attribution 4.0 License (Namensnennung).</AltText>
...
</License>
<SourceGroup>
<Meeting>
<MeetingId>M0631</MeetingId>
<MeetingSequence>197</MeetingSequence>
<MeetingCorporation>Deutsche Gesellschaft für Medizinische Informatik, Biometrie und Epidemiologie</MeetingCorporation>
<MeetingName>70. Jahrestagung der Deutschen Gesellschaft für Medizinische Informatik, Biometrie und Epidemiologie e. V. (GMDS)</MeetingName>
<MeetingTitle/>
<MeetingSession>V: Large language models & medical texts 1</MeetingSession>
<MeetingCity>Jena</MeetingCity>
<MeetingDate>
<DateFrom>20250907</DateFrom>
<DateTo>20250911</DateTo>
...
</MeetingDate>
...
</Meeting>
...
</SourceGroup>
<ArticleNo>Abstr. 217</ArticleNo>
...
</MetaData>
<OrigData>
<TextBlock name="Text" linked="yes">
<MainHeadline>Text</MainHeadline>
<Pgraph>
<Mark1>Introduction:</Mark1>
The integration of large language models (LLMs) into healthcare, with its strict data protection and security requirements, poses particular challenges, which is why powerful proprietary models such as ChatGPT are not suitable, even if they are increasingly being used informally. To close this gap, this project presents a locally hosted open-source LLM and retrieval-augmented generation (RAG) system that enables interaction with a locally stored, extensive internal document library at the University Hospital Halle via a chatbot.
...
</Pgraph>
<Pgraph>
<Mark1>Methods:</Mark1>
The system is hosted on a local Linux server equipped with an Nvidia H100 GPU and utilizes the Falcon-7B-Instruct model. Internal documents - primarily SOPs, procedural guidelines and forms - were cleaned, contextually chunked, and embedded using the jina-embeddings-v3 model. The system retrieves documents based on combined cosine similarity and BM25 scoring. Evaluation involved (1) subjective user feedback collected during a one-month test phase (2) expert annotation of chatbot responses to 38 manually crafted evaluation questions and (3) a technically analysis of the response time.
...
</Pgraph>
<Pgraph>
<Mark1>Results:</Mark1>
During the test phase the system handled 287 user queries, receiving 72 feedback entries (25.01% response rate). Feedback on document relevance was generally more positive (59.26%) than the generated textual answers (43.50%). Expert evaluation using the test dataset yielded an average helpfulness rating of 72%. On a scale from one (worse) to five (best) expert evaluation reached a score of 3.72 for textual answers and 3.89 for retrieved documents. Technical performance showed acceptable latency, with 75% of responses returned in under five seconds. First queries in a session averaged 3.71s, while follow-ups averaged with 4.28s slightly longer response times due to an additional LLM-request in the retrieval phase.
...
</Pgraph>
<Pgraph>
<Mark1>Discussion:</Mark1>
Initial results show that even small LLMs can yield useful results when coupled with well-designed retrieval systems. Although user feedback was mixed, it reflected the early prototype status and the subjective nature of expectations. Document retrieval quality was consistently rated higher than the textual synthesis, highlighting limitations in inter-document reasoning by smaller LLMs. Evaluation discrepancies between users and expert annotators likely stem from varied prompt wording, query complexity and expectations. Response time analysis confirmed that the system operates within acceptable bounds for real-world usage.
...
</Pgraph>
<Pgraph>
<Mark1>Conclusion:</Mark1>
This proof-of-concept demonstrates the viability of a locally hosted LLM-based chatbot for internal document retrieval in healthcare settings. While textual response quality can be further improved, this proof of concept system already offers a useful alternative to basic keyword search. Future improvements should focus on the improvement of the retrieval process, metadata enrichment, and advanced reasoning capabilities. Moreover, in order to facilitate a comprehensive introduction to the system, it would be essential to establish appropriate management protocols to regulate document access by various individuals. Overall, the project underscores the feasibility and potential of secure, on-premise AI tools in clinical environments.
...
</Pgraph>
<Pgraph>Jan Kirchhoff and Fabian Berns are employed by medicalvalues GmbH, which provided consulting support during the design and development of the system. Jan Kirchhoff is the managing director of medicalvalues GmbH.</Pgraph>
<Pgraph>The authors declare that an ethics committee vote is not required.</Pgraph>
...
</TextBlock>
<References linked="yes">
<Reference refNo="1">
<RefAuthor>Lewis P</RefAuthor>
<RefAuthor/>
<RefTitle>Retrieval-augmented generation for knowledge-intensive NLP tasks</RefTitle>
<RefYear/>
<RefBookTitle>Proceedings of the 34th International Conference on Neural Information Processing Systems NIPS 20</RefBookTitle>
<RefPage/>
<RefTotal>Lewis P, et al. Retrieval-augmented generation for knowledge-intensive NLP tasks. In: Proceedings of the 34th International Conference on Neural Information Processing Systems NIPS 20. Vancouver, BC, Canada: Curran Associates Inc., 2020. DOI: 10.5555/3495724.3496517</RefTotal>
<RefLink>https://doi.org/10.5555/3495724.3496517</RefLink>
...
</Reference>
<Reference refNo="2">
<RefAuthor>Muti HS</RefAuthor>
<RefAuthor/>
<RefTitle>Customizing GPT-4 for clinical information retrieval from standard operating procedures</RefTitle>
<RefYear>2024</RefYear>
<RefJournal>medRxiv</RefJournal>
<RefPage/>
<RefTotal>Muti HS, et al. Customizing GPT-4 for clinical information retrieval from standard operating procedures. medRxiv. 2024. DOI: 10.1101/2024.06.24.24309221</RefTotal>
<RefLink>http://dx.doi.org/10.1101/2024.06.24.24309221</RefLink>
...
</Reference>
<Reference refNo="3">
<RefAuthor>Amugongo LM</RefAuthor>
<RefAuthor>Mascheroni P</RefAuthor>
<RefAuthor>Brooks SG</RefAuthor>
<RefAuthor>Doering S</RefAuthor>
<RefAuthor>Seidel J</RefAuthor>
<RefTitle>Retrieval Augmented Generation for Large Language Models in Healthcare: A Systematic Review</RefTitle>
<RefYear>2024</RefYear>
<RefJournal>Preprints</RefJournal>
<RefPage/>
<RefTotal>Amugongo LM, Mascheroni P, Brooks SG, Doering S, Seidel J. Retrieval Augmented Generation for Large Language Models in Healthcare: A Systematic Review. Preprints. Jul 2024. DOI: 10.20944/preprints202407.0876.v1</RefTotal>
<RefLink>http://dx.doi.org/10.20944/preprints202407.0876.v1</RefLink>
...
</Reference>
<Reference refNo="4">
<RefAuthor>Huang L</RefAuthor>
<RefAuthor/>
<RefTitle>A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions</RefTitle>
<RefYear>2025</RefYear>
<RefJournal>ACM Transactions on Information Systems</RefJournal>
<RefPage>1–55</RefPage>
<RefTotal>Huang L, et al. A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions. ACM Transactions on Information Systems. 2025 Jan;43(2):1–55. DOI: 10.1145/3703155</RefTotal>
<RefLink>http://dx.doi.org/10.1145/3703155</RefLink>
...
</Reference>
...
</References>
<Media>
<Tables>
<NoOfTables>0</NoOfTables>
...
</Tables>
<Figures>
<NoOfPictures>0</NoOfPictures>
...
</Figures>
<InlineFigures>
<NoOfPictures>0</NoOfPictures>
...
</InlineFigures>
<Attachments>
<NoOfAttachments>0</NoOfAttachments>
...
</Attachments>
...
</Media>
...
</OrigData>
...
</GmsArticle>
