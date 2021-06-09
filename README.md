# FF-STM
Web Appendix - Methodology for Structural Topic Modeling

This repository serves as a web appendix for the paper 'Wrinkles in the CSR story: CSR and the service brand experience in fast fashion retailing by Haren, van, Mickelsson, and Lemmink (2021)

The repository contains a complementary and more extensive discussion of the methodology from the article and all the code that is used in the paper to perform 
Structural Topic Modeling (STM).

# Methodology
## Natural Language processing

For this study we used Natural Language Processing (NLP), which is an umbrella term for various computational techniques for analyzing and visualizing naturally occuring texts with the purpose of achieving human-like language processing (Liddy, 2001). Some of these techniques include sentiment analysis and topic modeling; this paper focuses on the latter. Topic modeling is an unsupervised machine learning (ML) content analysis technique aiming at uncovering hidden latent structures from large text corpora; it analyzes the words of a text and discovers the themes that are present (Blei, 2012). Unsupervised means that the topics do not have to be known a priori and therefore no training dataset is needed. Instead raw texts are the input for the unsupervised text analysis and therefore it can easily analyze corpora on a scale that is impossible using human labour (Blei, 2012; Chae & Park, 2018). 

## Latent Dirichlet Allocation

For our topic model we use Latent Dirichlet Allocation (LDA; Blei, 2012; Blei, Ng, and Jordan, 2003) because it is the simplest and therefore most popular model used in the literature. LDA uses a ‘reversed generative process’ answering the question: ‘What is the hidden structure that likely generated the observed collection?’ (Blei, 2012, p.10). The documents and its content (words) are observed, but the topics, per-document topic distributions and per-document per-word topic assignments are hidden structures. The model tries to deduce the hidden topic structure, based on the observed documents and words inside the documents. Figure 1. visualizes this process. 



Figure 1. A graphical representation for LDA (Adapted from Blei, 2012 in Chae &  Park, 2018, p.4). The documents and its content (words; w) are observed. On the basis of this the hidden structures are deduced: topic distributions per document (θ), per-word topic assignments (z), and topic word distribution (β).  K = topic; D = Document.
