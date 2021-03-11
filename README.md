# Plagiarism-Detection

## Introduction: 

Plagiarism is the “wrongful appropriation” and “stealing and publication” of another author’s “language, thoughts, ideas, or expressions” and representing them as one’s own original work. Checking assignments for plagiarism is essential in academics as assignments are used to evaluate students in their courses.. If a course consists of a large number of students, it is impractical to check each assignment by a human inspector. Therefore it is essential to have automated tools in order to assist detection of plagiarism in assignments.  

## Business Applications: 

- Students may plagiarize by copying ideas from friends,web or private tutors. Most courses in universities evaluate students based on the marks of their assignments.  
- A quality plagiarism detector has a strong impact on law suit prosecution.  
- To create websites quickly with less effort and to follow the quality content of a reputed website people commit plagiarism. 
- Some level plagiarism checkers also provide the accuracy of grammar and paragraphing. 

## Dataset: 

The dataset is taken from a freely available Clough-Stevenson corpu	s  . The corpus consists of answers to five short questions on a variety of topics in the Computer Science field. The five short questions are: 

- What is inheritance in object oriented programming?	  
- Explain the PageRank algorithm that is used by the Google search engine.	  
- Explain the Vector Space Model that is used for Information Retrieval.	 
- Explain Bayes Theorem from probability theory.	  
- What is dynamic programming?	 
	
These questions are represented as tasks(a-e) in the dataset where each task maps to each of the five questions respectively. 

Each question has 19 students answers and 1 corresponding wikipedia answer.  
We are given the extent of plagiarism for every answer in a separate file_information file. We use this file to check accuracy and precision of the results achieved by our algorithm. 

There are four different levels of plagiarisms. They are cut, light, non and heavy. Cut refers to answers that are cut, copy, pasted from Wikipedia answers. Heavy refers to answers that are heavily copied from Wikipedia answers, light refers to answers which are slightly copied from Wikipedia answers with extreme changes to the structure of answers. Non refers to non-plagiarised work.  

## Project Goal: 

The goal of this project is to find the extent of plagiarism in each student’s submission for every question using unsupervised machine learning methods. 
For this, we have Proposed two methods. 
 
### Method 1 Overview: 

- **Text Cleaning:** Initial step for building text models. Accuracy of machine learning	 models on textual data depends heavily on text pre-processing. 
- **Glove Embeddings:** GloVe is a log-bilinear model that is trained on the non-zero entries	 of a global word-word co-occurrence matrix, which tabulates how frequently words co-occur with one another in a given corpus. Output of this step is vector representation for each unique word in the corpus.  
- **K-Means Clustering:** Cluster these word vectors into K clusters based on their	 contextual meaning. The output of this step is K word clusters. 
- **Document Level Clustering:** From the obtained clusters and its respective labels from	 the K-means clustering, we now use these clusters to convert our data in each document to vectors. 
- **FP-Growth Algorithm:** In this step, we mine frequent patterns in documents based on	 sentence similarities which outputs a list of items which were plagiarised. 
- **Model Evaluation:** We evaluated the results manually calculating accuracy, precision,	 F1- score . 

## Text Pre-Processing: 

![image](https://user-images.githubusercontent.com/37929675/110738711-c662ba00-81fd-11eb-84b5-3d30e3879487.png)


For pre-processing we have to make sure that all the words are lowercase with all the punctuations removed. We use the NLTK sentence tokenizer to get a list of sentences so that we know where each sentence begins and ends. There are various latin words in our answers as well so we use ”latin1” encoding for reading the text. Implemented EDA on text data to get insights like top most used words in each question, Bigram representation in word clouds etc., 
 
## Glove Embeddings: 

For creating vector word embeddings, we used a method called GloVe. GloVe is an unsupervised learning algorithm for obtaining vector representations for words. List of words in sentences of the documents is fed to the Algorithm. After performing GloVe on our preprocessed data we get vectors of each unique word in our corpus. We can use this representation for clustering and further processing. 
 
## K-means clustering: 

K-means clustering is a type of unsupervised learning, which is used when we have unlabelled data. The goal of this algorithm is to nd groups in the data, with the number of groups represented by the variable K. We determined K value using Grid Search, a hyperparameter selection method. The output of this step is K (=25) word clusters.  


## Vector Representation for document-level embeddings:  

![image](https://user-images.githubusercontent.com/37929675/110738689-bcd95200-81fd-11eb-9d45-69ce937ee95c.png)


**Sentence Vector:** Each entry is a word representing the cluster label it belongs to.	 
Document Vector: Each entry represents sentence number according to the following rule.	 
From the obtained word clusters and its respective labels from the K-means clustering, we now use these clusters to convert data in each document to vectors. For this, we do the following:  
 
- Determine the cluster label for each of the words in sentences and substitute the word with its respective cluster label.  
- From the above step,we merge all the words and form sentence vectors.  
- From these sentence vectors, we form an answer(document) vector by assigning a unique number if two sentence vectors are different. If two sentence vectors are the same, we assign each of them the same number.  
  
Thus we get a vector for each answer(document).This vector can be viewed as an item-set and each answer can be viewed as a transaction. Thus we get a list of transactions which contain item-sets.We can feed this into the FP-Growth Algorithm to get frequent item sets,i.e., plagiarised answers.  
 
## FP-Growth Algorithm: 

The FP-growth algorithm is currently one of the fastest approaches to frequent itemset mining. We can nd which documents are plagiarised by just looking at the transactions which in our case are student answers. Here each student answer is one transaction and their vector representation of sentences are itemsets. 
 
## Model Evaluation: 

![image](https://user-images.githubusercontent.com/37929675/110738664-b519ad80-81fd-11eb-8484-0e563aab3866.png)


Based on Support counts output is classified into 4 classes. For heavy, minsup is 8, for cut its 6, for light its 4 , for non its 3. 
Multi-Class Classification: Evaluated final results with actual labels given in dataset i.e., light,	 heavy, cut, non. Accuracy is as low as 60% but high precision is to be noted. This is usually due to class imbalances. 
Binary Classification: To Tackle Class Imbalance problems	, Binary Classification is applied.	 
Documents with light,cut,heavy labels are categorized as Plagiarized and with non as 
Non-Plagiarized. Accuracy is as high as 80% and precision is 90%  

 
