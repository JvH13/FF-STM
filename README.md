This Github page will serve as a webappendix for the paper 'Wrinkles in a CSR Story: CSR and Service Brand Experience in Fast Fashion Retailing by Van Haren, J.G.M., Mickelsson, J, and Lemmink, J.G.A.M. (2021).

In this webappendix you will find a more comprehensive discussion of the methodology and all the code that was used for the analyses in this paper.

**Methodology**

**Natural Language processing**

For this study we used Natural Language Processing (NLP), which is an umbrella term for various computational techniques for analyzing and visualizing naturally occuring texts with the purpose of achieving human-like language processing (Liddy, 2001). Some of these techniques include sentiment analysis and topic modeling; this paper focuses on the latter. Topic modeling is an unsupervised machine learning (ML) content analysis technique aiming at uncovering hidden latent structures from large text corpora; it analyzes the words of a text and discovers the themes that are present (Blei, 2012). Unsupervised means that the topics do not have to be known a priori and therefore no training dataset is needed. Instead raw texts are the input for the unsupervised text analysis and therefore it can easily analyze corpora on a scale that is impossible using human labour (Blei, 2012; Chae & Park, 2018). 

**Latent Dirichlet Allocation**

For our topic model we use Latent Dirichlet Allocation (LDA; Blei, 2012; Blei, Ng, and Jordan, 2003) because it is the simplest and therefore most popular model used in the literature. LDA uses a ‘reversed generative process’ answering the question: ‘What is the hidden structure that likely generated the observed collection?’ (Blei, 2012, p.10). The documents and its content (words) are observed, but the topics, per-document topic distributions and per-document per-word topic assignments are hidden structures. The model tries to deduce the hidden topic structure, based on the observed documents and words inside the documents. Figure 1. visualizes this process. 

![LDA_visual](https://user-images.githubusercontent.com/79323371/121391579-cf733c80-c94e-11eb-966c-2b2e5b243a35.png)

    Figure 1. A graphical representation for LDA (Adapted from Blei, 2012 in Chae &  Park, 2018, p.4). The documents and its content (words; w) are observed. On the basis of
    this the hidden structures are deduced: topic distributions per document (θ), per-word topic assignments (z), and topic word distribution (β).  K = topic; D = Document.

**Structural Topic Modeling**

Because of LDA’s simplicity it provides a viable foundation for building more complex topic models like the correlated topic model (Blei & Lafferty, 2009) and the structural topic model (STM; Roberts, Stewart, Tingley, Lucas, Leder-Luis, Gadarian, Albertson, and Rand, 2014; Roberts, Stewart, and Tingley, 2019). STM facilitates the use of covariates and other document-level metadata like year, company name and document source. Here it differentiates from other topic models which only permit the estimation of the overall topics in a corpus and therefore it is perfect for researchers in the social and business sciences who want to study the relationships between topics and metadata (Chae & Park, 2018, Roberts et al. 2014,  Roberts et al. 2019). 

STM builds on LDA by adding two extra components: topic prevalence covariates and topic content covariates. Topic prevalence means how much of a document is associated with a certain topic and topic content is about the words used in a certain topic (Chae & Park, 2018; Roberts et al. 2019). For example, in this research ‘Company source’ is a topic prevalence covariate and allows the comparison between tweets about H&M and H&M’s CSR reports. The covariate makes it possible to see whether a certain topic is discussed more often in tweets than in CSR reports (or vice versa). Figure 2. extents Figure 1. with the two types of covariates.

![STM_visual](https://user-images.githubusercontent.com/79323371/121391819-13fed800-c94f-11eb-9085-94cc7436c9a2.png)

	Figure 2. A graphical representation of STM (Adapted from Roberts et al. 2014 in Chae & Park, 2018, p.5). The topic prevalence covariate (X) influences the topic
	proportions (θ). Topic content covariates (Υ) influence the per-topic word distributions (β).

**STM process**

Figure 3. provides an overview of the STM process. First, text data (and accompanying metadata) is collected and loaded into R. Second, the data is processed, so it has the correct format to serve as input for the model. Third, an analysis is run to select the best fitting model. Lastly, the best fitting model is estimated, additional analysis is performed to better understand the output of the model and the output of the model is visualized. All of the data preparation, analysis and visualization is done with the R package [‘STM’](https://www.structuraltopicmodel.com/)  (Roberts et al. 2014; Roberts et al. 2019). 

![STM_workflow](https://user-images.githubusercontent.com/79323371/121392333-912a4d00-c94f-11eb-9e4c-3a4999b08cf6.png)

	Figure 3. Workflow of the STM process.

**Data collection**

In this study our dataset (text corpus) consists of three streams of information, collected between 01-01-2018 and 31-12-2020. First, CSR reports and press releases from three fast fashion companies (H&M, Zara, and Primark) were collected from their corporate websites. In these texts the companies try to create a CSR identity for themselves. Second, tweets from consumers about these fast fashion companies were scraped from Twitter using Python and the [snscrape package](https://github.com/JustAnotherArchivist/snscrape). The Python code used can be found [here](https://github.com/JvH13/FF-STM/blob/main/Twitter_scraper) Third, news articles from the news media were collected from the LexisNexis database. Here, consumers and the news media create a CSR reputation about the fast fashion companies. 

**Text pre-processing**

For indepth information about the STM package please also see [this website](https://www.rdocumentation.org/packages/stm/versions/1.3.6)

All non-English texts were removed from the corpus and only tweets and news articles containing CSR keywords, as defined by Sarkar & Searcy (2016), were included. The reason for this is twofold: practically, there is much ‘garbage’ in these two data streams, e.g. teen girls tweeting a picture of themselves in a H&M dress is not very relevant considering the scope of this research. Theoretically, it might seem like cherry picking, but in this study we don’t want to answer the question ‘if CSR topics are discussed in relation to these companies’, but ‘which topics are discussed in relation to these fast fashion companies’, so only including texts containing CSR keywords is in line with our research question. Search terms were stemmed (e.g. ‘sustainab*’). News articles were also stripped from unnecessary metadata, only leaving plain text. Furthermore, HTML tags, URLs, punctuation, special characters, numbers and stop words were removed from the texts. Words were stemmed, converted to lower cases, and infrequent words were removed. The code used for all of this can be found [here](https://github.com/JvH13/FF-STM/blob/main/Clean_data). The [textProcessor](https://www.rdocumentation.org/packages/stm/versions/1.3.6/topics/textProcessor) function in the STM package also performs many of these factions.

The resulting text corpus consists of 89 CSR reports and press releases (H&M: 52; Zara: 19; Primark: 18), 57,414 tweets (H&M: 19,050; Zara: 15,445; Primark: 22,919), and 5,
51 news articles (H&M: 2,227; Zara: 734; Primark: 2,390) for a total of 62,854 documents.

**Selecting a model**

The STM model is an unsupervised ML technique, but needs one input from the researcher: the number of topics. There is no right or wrong answer to how many topics should be used (Roberts et al. 2019), but using the [searchK](https://www.rdocumentation.org/packages/stm/versions/1.3.6/topics/searchK) function in the STM package a data-driven decision can be made on the number of topics. According to the official documentation, 60 - 100 topics works well for corpora with 10,000 to 100,000 documents (Roberts et al. 2017); therefore diagnostics were estimated for 60, 70, 80, 90, and 100 topics. Diagnostics include residuals, semantic coherence and held-out likelihood estimations. A mixed approach was used to choose the optimal number of topics. First, the diagnostics were analyzed. Important to remember is that there is no ‘best’ answer (Roberts et al., 2019). It is always a trade-off between different metrics. Second, topic quality was measured. Topic quality consists of two dimensions. Topic coherence, which is maximized when the most probable words within a topic co-occur together and topic exclusivity, which means that top words for one topic do not appear as top words for other topics (Roberts et al. 2019, Chae & Park, 2018). Figure 4. shows the output of the searchK function and Figure 5. shows topic quality (the trade-off between semantic coherence and exclusivity). To calculate topic quality the [topicQuality](https://www.rdocumentation.org/packages/stm/versions/1.3.6/topics/topicQuality) function from the STM package was used. The code for the searchK function can be found [here](https://github.com/JvH13/FF-STM/blob/main/Select_model) and the code for topicQuality [here](https://github.com/JvH13/FF-STM/blob/main/topicQuality)

![searchK_diagnostics](https://user-images.githubusercontent.com/79323371/124749384-71714f00-df24-11eb-813e-5f9ee0147514.png)


	Figure 4. Diagnostic statistics: Held-out likelihood, residuals, semantic coherence, and lower bound

**Estimating the best model**

We choose a model with 80 topics, because it appears to be a good trade-off between the various metrics: average values for residuals, semantic coherence and lower bound and the second-highest held-out likelihood value. Inspecting figure 5, the majority of the topics appear near the top right corner, which was true for all the different amounts of topics, which means all models are equally qualified with regards to topic quality. The code used to estimate the model can be found [here](https://github.com/JvH13/FF-STM/blob/main/EstimateModel)

![TopicQuality_model80](https://user-images.githubusercontent.com/79323371/124749497-982f8580-df24-11eb-9b6a-0adf7a32b4b7.png)

	Figure 5. Topic quality  for a topic model with 80 topics.

**Understanding the model**

The next step is to interpret the outcomes of the topic model. First of all, the topics have to be interpreted. Using ‘labelTopics’, four different methods of devising the words that form a topic are presented (Roberts et al., 2019). After interpreting the topics, we conclude that 23 out of 80 topics are CSR related. Many topics are about customer service and opening times of stores. Sarkar & Searcy (2016)’s CSR dimensions indeed contain the keyword ‘customers’, but we think that in the context of this paper these topics are not very relevant. 

With the 23 topics that are left we used the ‘estimateEffect’ function to regress the topic proportions on the documents, with the document meta-data as the covariates (Roberts et al., 2019). The results show, with a 95% confidence interval, whether a topic is discussed more or less in certain documents or not. Figure 6., 7., and 8. show how topic proportions vary between different news sources (Twitter, news media and company communication). For example in Figure 6., ‘hospital donation’, ‘data protection’, and ‘consciousness fashion’ are discussed significantly more often in the company reports compared to the news media. ‘government’, ‘factory worker union’, and ‘fair wages’ are not discussed significantly more in the news media compared to the company reports and vice versa. ‘ABF Food profits’, racist H&M ad’, and ‘furloughing employees’ are discussed significantly more often in the news media compared to the company reports. In the same manner Figure 7. and 8. can be read. 	
	
![model80_News_Company_reports](https://user-images.githubusercontent.com/79323371/124749862-0d02bf80-df25-11eb-83dc-d44a1a4ea0d7.png)

![model80_News_Twitter](https://user-images.githubusercontent.com/79323371/124749926-2441ad00-df25-11eb-9faa-05fd4ab2df0b.png)

![model80_Twitter_Company_reports](https://user-images.githubusercontent.com/79323371/124750008-39b6d700-df25-11eb-92eb-ec0f2193c261.png)


	Figure 6. Topic proportion differences between (1) news articles and company reports, (2) news articles and Tweets, (3) Tweets and company reports
	The black dots represent the proportion values and the black lines are the 95% confidence intervals. 

**Bibliography**

Blei, D. M. (2012). Probabilistic topic models. Communications of the ACM, 55(4), 77-84.

Blei, D. M., & Laﬀerty, J. D. (2009). Topic models. In Text mining (pp. 101-124). Chapman and Hall/CRC.

Blei, D. M., Ng, A. Y., & Jordan, M. I. (2003). Latent dirichlet allocation. the Journal of machine Learning research, 3, 993-1022.

Lee, T. H. (2017). The status of corporate social responsibility research in public relations: A content analysis of published articles in eleven scholarly journals from 1980 to 2015. Public Relations Review, 43(1), 211-218.

Lee, S. Y., & Carroll, C. E. (2011). The emergence, variation, and evolution of corporate social responsibility in the public sphere, 1980–2004: The exposure of firms to public debate. Journal of Business Ethics, 104(1), 115-131.

Liddy, E. D. (2001). Natural language processing.

Roberts, M. E., Stewart, B. M., & Tingley, D. (2019). Stm: An R package for structural topic models. Journal of Statistical Software, 91(1), 1-40.

Roberts, M., Stewart, B., Tingley, D., Benoit, K., Stewart, M. B., Rcpp, L., ... & KernSmooth, N. L. P. (2020). Package ‘stm’. Imports matrixStats, R. & KernSmooth, (2017).

Roberts, M. E., Stewart, B. M., Tingley, D., Lucas, C., Leder‐Luis, J., Gadarian, S. K., ... & Rand, D. G. (2014). Structural topic models for open‐ended survey responses. American Journal of Political Science, 58(4), 1064-1082.

Sarkar, S., & Searcy, C. (2016). Zeitgeist or chameleon? A quantitative analysis of CSR definitions. Journal of Cleaner Production, 135, 1423-1435.

