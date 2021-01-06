# Topic Modelling using Latent Derelict Allocation (LDA) for PubMed Central (pmc) Papers Related to COVID-19 and Other Corona Viruses

The CORD-19 dataset (https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge) containts a large collection of literature on coronaviruses that are made avilable for data mining. Part of the data is 85,000+ PMC papers- the project aims to classify the papers according to their topics in alignment with WHO's Research and Development Blueprint (https://www.who.int/blueprint/priority-diseases/key-action/Global_Research_Forum_FINAL_VERSION_for_web_14_feb_2020.pdf?ua=1) research topics to tackle the spread of COVID-19. These are: virus:natural history, transmission and diagnostics; animal and environmental research on the virus origin, and management measures at the human-animal interface, epidemiological studies; clinical characterization and management, infection prevention and control, including health care workers’ protection; candidate therapeutics R&D; candidate vaccines R&D; ethical considerations for research and; integrating social sciences in the outbreak response.

The concept of LDA was first applied by Blei, Ng and Jordan in 2003; in their paper they describe it as a generative probabilistic model for a text corpora. It uses a hierarchical (corpus, document and word level) Bayesian model where each document in a corpus is modeled as a random mixture of underlying set of topic probabilities [1]. 

Genisim is a popular open source library, for Python and other programming languages, that can be used for unsupervised topic modelling using Latent Dirichlet Allocation (LDA). The core estimation code for the LdaMulticore method is based on the onlineldavb.py script, by Hoffman, Blei, Bach: Online Learning for Latent Dirichlet Allocation, NIPS 2010 [2]. Gensim also has a module, models.wrappers.ldamallet, that allows both LDA model estimation from a training corpus and inference of topic distribution on new, unseen documents, using an (optimized version of) collapsed gibbs sampling from MALLET [3]. This project will apply both methods to model the topics.

In order to lower the computational cost, the data is first sampled for 40,000 pmc articles out of the 85,000+ avialable from the source. The sample is then processed by parsing the introduction, background, discussion and conclusion sections only. In order to accept the article into the corpus, there has to have an introduction and/or background section as well as a discussion and/or conclusion section. The latter step further lowered the number of papers to be included into the corpus into 19,664 papers. Once papers are parsed, the text is processed in Spacey's NLP library where it is tokenized, lemmatized and filtered to exclude stopwords, punctuation, standalone numbers, URL links and other words/characters deemed unuseful.

The code and technique used for the next steps follows the guidance given by Prabhakaran [4]. The corpus is then transformed into a bag-of-words (bow) vector, then gensim.models.ldamulticore class’ LdaMulticore method is used to model the topics. 16 different models are first created by varying the number of topics parameter num_topics using multiples of the number 4 from 4 to 64. In order to choose the best model and in turn the best number of topics to classify the topics into, the topic coherence score is calculated for each model using gensim.models.coherencemodel class. The topic coherence score calculation is based on Roeder, Both and Hinneburg’s 2015 [5]. The score quantifies the coherence of topics the documents were classified into, it ranges from 0 to 1, the higher the score the better the topic number choice is. The coherence score for the modelled topic numbers is shown in the plot below:

![coh_1](images/coh_1.png)

The highest score is 0.57 for 28 topics. To ensure that is the optimal topic number is chosen, the corpus is also modelled for 25, 26, 27, 29, 30 and 31. The resulting coherence score plot is shown below:

![coh_2](images/coh_2.png)

Topic number equal to 28 is still the topic with highest coherence score.

Another method to evaluate the modes is to compare topics correlation with each other within one model using Jacard distance. The plot below shows this relationship where the dark red cells indicate strongly decorrelated topics and dark blue cells indicate strongly correlated topics. In an ideal world, it is prefered to see different topics decorrelated between themselves (i.e. all dark red).

![corr_plt.png](images/corr_plt.png)

The plot shows a majority of the cells colored moderately dark to very dark red which is a good indication that the topic number 28 chosen is a good.

The table below shows the topic numbers and the top 5 paper titles and pmc ID number where the topic is most dominante.




References:
[1] Blei, D., Ng, A. and Jordan, M. (2003). Latent Dirichlet Allocation. In Journal of Machine Learning Research 3 (2003) 993-1022.
[2] Gensim: Topic modelling for humans. (2021, Jan 06). Retrieved from https://radimrehurek.com/gensim/models/ldamodel.html
[3] Gensim: Topic modelling for humans. (2021, Jan 06). Retrieved from https://radimrehurek.com/gensim/models/wrappers/ldamallet.html
[4] Prabhakaran, S. (Retrieved on 2021, Jan 06) Topic Modeling with Gensim (Python) . https://www.machinelearningplus.com/nlp/topic-modeling-gensim-python/
[5] Röder, M., Both, A., & Hinneburg, A. (2015, February). Exploring the space of topic coherence measures. In Proceedings of the eighth ACM international conference on Web search and data mining (pp. 399-408).
