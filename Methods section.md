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

	Figure 2. A graphical representation of STM (Adapted from Roberts et al. 2014 in 
	in Chae & Park, 2018, p.5). The topic prevalence covariate (X) influences the topic  proportions (θ). Topic content covariates (Υ) influence the per-topic word 	
	distributions (β).

Content analysis is utilised in CSR research to analyze e.g. company CSR reports (Tate, Ellram, and Kirchoff, 2010), newspaper articles (Lee & Carroll, 2011) and academic articles (Lee, 2017), but these studies used manual coding which has the disadvantage that only relatively small datasets can be analyzed. On the other hand, a growing body of literature (e.g. Chae & Park, 2018; Benites-Lazaro, Giatti and Giarolla, 2018; Goloshchapova, Poon, Pritchard , and Reed, 2019) uses ML techniques for this purpose and the advantage of ML is that huge datasets can be easily analyzed. 

**STM process**

Figure 3. provides an overview of the STM process. First, text data (and accompanying metadata) is collected and loaded into R. Second, the data is processed, so it has the correct format to serve as input for the model. Third, an analysis is run to select the best fitting model. Lastly, the best fitting model is estimated, additional analysis is performed to better understand the output of the model and the output of the model is visualized. All of the data preparation, analysis and visualization is done with the R package ‘STM’ (NOTE:  https://www.structuraltopicmodel.com/ ) (Roberts et al. 2014; Roberts et al. 2019). 

![STM_workflow](https://user-images.githubusercontent.com/79323371/121392333-912a4d00-c94f-11eb-9e4c-3a4999b08cf6.png)


	Figure 3. Workflow of the STM process.

**Data collection**

In this study our dataset (text corpus) consists of three streams of information, collected between 01-01-2018 and 31-12-2020. First, CSR reports and press releases from three fast fashion companies (H&M, Zara, and Primark) were collected from their corporate websites. In these texts the companies try to create a CSR identity for themselves. Second, tweets from consumers about these fast fashion companies were scraped from Twitter using Python and the snscrape package (NOTE:  https://github.com/JustAnotherArchivist/snscrape ). Third, news articles from the news media were collected from the LexisNexis database. Here, consumers and the news media create a CSR reputation about the fast fashion companies. 

**Text pre-processing**

All non-English texts were removed from the corpus and only tweets and news articles containing CSR keywords, as defined by Sarkar & Searcy (2016), were included. The reason for this is twofold: practically, there is much ‘garbage’ in these two data streams, e.g. teen girls tweeting a picture of themselves in a H&M dress is not very relevant considering the scope of this research. Theoretically, it might seem like cherry picking, but in this study we don’t want to answer the question ‘if CSR topics are discussed in relation to these companies’, but ‘which topics are discussed in relation to these fast fashion companies’, so only including texts containing CSR keywords is in line with our research question. Search terms were stemmed (e.g. ‘sustainab*’). News articles were also stripped from unnecessary metadata, only leaving plain text. Furthermore, HTML tags, URLs, punctuation, special characters, numbers and stop words were removed from the texts. Words were stemmed, converted to lower cases, and infrequent words were removed. 

The resulting text corpus consists of 89 CSR reports and press releases (H&M: 52; Zara: 19; Primark: 18), 57,414 tweets (H&M: 19,050; Zara: 15,445; Primark: 22,919), and 5,351 news articles (H&M: 2,227; Zara: 734; Primark: 2,390) for a total of 62,854 documents.

**Selecting a model**

The STM model is an unsupervised ML technique, but needs one input from the researcher: the number of topics. There is no right or wrong answer to how many topics should be used (Roberts et al. 2019), but using the searchK function in the STM package a data-driven decision can be made on the number of topics. According to the official documentation, 60 - 100 topics works well for corpora with 10,000 to 100,000 documents (Roberts et al. 2017); therefore diagnostics were estimated for 60, 70, 80, 90, and 100 topics. Diagnostics include residuals, semantic coherence and held-out likelihood estimations. A mixed approach was used to choose the optimal number of topics. First, residuals of the different models were compared. Then, for models with the lowest residuals, topic quality was measured. Topic quality consists of two dimensions. Topic coherence, which is maximized when the most probable words within a topic co-occur together and topic exclusivity, which means that top words for one topic do not appear as top words for other topics (Roberts et al. 2019, Chae & Park, 2018). 

**Estimating the best model**

**Understanding the model**

**Bibliography**

Benites-Lazaro, L. L., Giatti, L., & Giarolla, A. (2018). Sustainability and governance of sugarcane ethanol companies in Brazil: Topic modeling analysis of CSR reporting. Journal of Cleaner Production, 197, 583-591.

Blei, D. M. (2012). Probabilistic topic models. Communications of the ACM, 55(4), 77-84.

Blei, D. M., & Laﬀerty, J. D. (2009). Topic models. In Text mining (pp. 101-124). Chapman and Hall/CRC.

Blei, D. M., Ng, A. Y., & Jordan, M. I. (2003). Latent dirichlet allocation. the Journal of machine Learning research, 3, 993-1022.

Goloshchapova, I., Poon, S. H., Pritchard, M., & Reed, P. (2019). Corporate social responsibility reports: topic analysis and big data approach. The European Journal of Finance, 25(17), 1637-1654.

Lee, T. H. (2017). The status of corporate social responsibility research in public relations: A content analysis of published articles in eleven scholarly journals from 1980 to 2015. Public Relations Review, 43(1), 211-218.

Lee, S. Y., & Carroll, C. E. (2011). The emergence, variation, and evolution of corporate social responsibility in the public sphere, 1980–2004: The exposure of firms to public debate. Journal of Business Ethics, 104(1), 115-131.

Liddy, E. D. (2001). Natural language processing.

Roberts, M. E., Stewart, B. M., & Tingley, D. (2019). Stm: An R package for structural topic models. Journal of Statistical Software, 91(1), 1-40.

Roberts, M., Stewart, B., Tingley, D., Benoit, K., Stewart, M. B., Rcpp, L., ... & KernSmooth, N. L. P. (2020). Package ‘stm’. Imports matrixStats, R. & KernSmooth, (2017).

Roberts, M. E., Stewart, B. M., Tingley, D., Lucas, C., Leder‐Luis, J., Gadarian, S. K., ... & Rand, D. G. (2014). Structural topic models for open‐ended survey responses. American Journal of Political Science, 58(4), 1064-1082.

Sarkar, S., & Searcy, C. (2016). Zeitgeist or chameleon? A quantitative analysis of CSR definitions. Journal of Cleaner Production, 135, 1423-1435.

Tate, W. L., Ellram, L. M., & Kirchoff, J. F. (2010). Corporate social responsibility reports: a thematic analysis related to supply chain management. Journal of supply chain management, 46(1), 19-44.

