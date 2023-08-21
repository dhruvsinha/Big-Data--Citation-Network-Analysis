# Citation Network Analysis
## Collaborators: Kaveri Chhikara and Summer Long
## Introduction
In this project, we aim to analyze the interconnections between different fields of research and understand which fields are merging and which are diverging over time. We are using a citation dataset extracted from **[DBLP, ACM, MAG](https://www.aminer.org/citation)**, which spans from 1936 to early 2022 and contains 5,259,858 papers before cleaning, in JSON format hosted on aminer.com, with a size of approximately 18 GB unprocessed.

We have developed three models to achieve our research objectives:
1. Abstract-Based Clustering: This model is designed to cluster papers based on their abstracts, which can help identify emerging research topics and trends across different fields.
2. Multiclass Text Classification: We used this model to classify papers into different research fields based on their abstracts.
3. Citation count prediction: With this model, we aimed to predict the number of citations a paper would receive based on its title and abstract

Our analysis has revealed that the intersections between computer science and healthcare, econometrics, and meteorology have been increasing over time. This indicates that researchers in these fields are collaborating and sharing their knowledge to solve complex problems that require interdisciplinary approaches.

Overall, our research provides valuable insights into the evolving landscape of research fields and highlights the importance of interdisciplinary collaboration in advancing scientific knowledge.

## Files contained
The project has been structured into three main folders, one for each model. This organizational approach facilitates modularity and enhances the overall manageability of the project. Each folder comprises several PySpark notebooks, which contain detailed comments explaining their purpose.

## Project Set up
The project was implemented on Google Cloud due to the large size of the original dataset, which was approximately 18 GB. PySpark was used for exploratory data analysis and building machine learning models on the data. To make the data more manageable, we initially converted the original dataset into Parquet files of 1 GB each. All subsequent analyses were performed on these Parquet files.

By converting the original dataset into smaller Parquet files, we could more easily process and manipulate the data. This also allowed us to take advantage of PySpark's distributed computing capabilities, which enabled us to process the data more efficiently. Furthermore, working with Parquet files instead of the original dataset made the analysis more manageable and reduced the risk of data corruption or loss.

Overall, using Google Cloud and PySpark to analyze the Parquet files enabled us to efficiently and effectively conduct exploratory data analysis and build machine learning models on a large and complex dataset. This approach has helped to improve the quality and accuracy of our results while reducing the time required for analysis.

### Model 1: Abstract Based Clustering

To identify emerging research topics and trends across different fields, we vectorize the abstract of each paper using Word2Vec. Once we have obtained the abstract vectors, we perform a KNN clustering to group the papers based on their semantic similarity. Within each cluster, we analyze the most popular research categories and track their changes over the years.

By using Word2Vec to vectorize the abstracts, we can capture the semantic meaning of the text, making it easier to cluster papers based on their topic. Additionally, KNN clustering allows us to group similar papers together, which can reveal hidden patterns and relationships in the data. By analyzing the most popular categories within each cluster and tracking their changes over time, we can gain insights into how different research fields are evolving and merging over time.
Overall, the approach of vectorizing the abstracts using Word2Vec and clustering them using KNN has enabled us to identify emerging research topics and trends across different fields. This approach has helped us to gain a deeper understanding of the interconnections between different research areas, and the changes that are taking place over time.

The model can be visualized in the diagram below: 

| ![model.jpg](/images/model.jpg) | 
|:--:| 
||

Some noteworthy observations from our analysis include the fact that fields such as oncology, healthcare, meteorology, medical imaging, and social psychology are increasingly sharing the same cluster with Data Science and Computer Science over time. We get the following result when we cluster for 2018. You can play with these results in clustering_final/res_plot.ipynb. 


| ![clustering_result.jpg](/images/clustering_result.jpg) | 
|:--:| 
||

The original dataset contained information on which papers were cited by other papers, which we used to construct an adjacency matrix. Using this matrix, we generated visual graphs where each node represents a paper, and the edges between nodes indicate that one paper cites another. 

By visualizing the citation relationships between papers in this way, we can gain insights into how different research fields are connected and how they evolve over time. This approach enables us to identify important papers, as well as the most influential authors and research groups in specific areas.

Overall, constructing the adjacency matrix from the citation data and generating visual graphs has provided us with a powerful tool for analyzing the connections between papers and identifying patterns in the data. The resulting visualizations allow us to explore the complex relationships between different research fields and better understand how they have evolved over time.

| ![graph_2000.jpg](/images/graph_2000.jpg) | 
|:--:| 
||

As we observe the graph, we can see that in the years 2000-2001, the edges between the nodes are relatively short, and the nodes are self-contained, indicating that there were not many interdisciplinary citations during that time period. This finding suggests that there may have been less collaboration between different research fields during the years 2000-2001. 

| ![graph_2010.jpg](/images/graph_2010.jpg) | 
|:--:| 
||

In the graph above, we can observe a significant increase in interconnections between different research fields. The edges between the nodes are longer, and we can see that fields such as Biomedical Engineering, Oncology, Healthcare, Social Psychology, and Econometrics are increasingly interacting with Data Science, Computer Vision, and Computer Science. This finding indicates that there has been a growing trend towards interdisciplinary collaboration between different research fields in recent years. 

### Model 2: Multiclass text classification
Here, we were trying to predict paper category from using abstracts. The model pipeline has four important steps- 
1. regexTokenizer: Tokenization with regular expression 
2. stopwordsRemover: Remove stop words 
3. StringIndexer that encodes a string column of labels to a column of label indices. 
4. Count vectors ("document term vectors") or TF-IDF

After partitioning the dataset into training and test (70:30), we trained the following models and made predictions and score on the test set

| Model | Accuracy |
| --- | --- |
| Logistic Regression with Count Vectorizer | 80.60% |
| Logistic Regression with TF-IDF |79.77% |
| Cross Validation (Tune LR with Countvectorizer) | 81.12% |
| Naive Bayes |78.89% |
| Random Forest |31.88% |

Random forest is an excellent and versatile method, but it may not be the best choice for high-dimensional sparse data. In this experiment, it is clear that Logistic Regression with cross-validation will be the ideal model for our needs. 

Logistic Regression is a popular classification method that is well-suited for high-dimensional data. It can handle both binary and multiclass classification problems and is particularly useful for datasets with sparse features. Additionally, by using cross-validation, we can ensure that our model is both robust and generalizable to new data.

Overall, using Logistic Regression with cross-validation is a reliable and effective approach for our experiment and is likely to yield accurate results.

### Model 3: Citation count prediction

In this part, we are trying to predict the number of citations by looking at paper abstract and titles. The pipeline for this part contains 6 major steps
1. Tokenizer: Converts abstract/title into a list of words 
2. StopWordsRemover: Removes stop words from list 
3. CountVectorizer: Converts words to sparse vector of term frequency counts 
4. IDF: Computes inverse document frequency weights 
5. VectorAssembler: Combines into single dense vector column 
6. GBTRegressor: Performs Gradient-Boosted Tree Regression

After partitioning the dataset into training and test (80:20), we trained the following models and made predictions and score on the test set:


| Feature used| RMSE |
| --- | --- |
|Title| 169.60 |
|Abstract|215.84|

This result is surprising. Abstract contains more information than the title. Therefore, we were expecting that RMSE for Abstract will be lower than RMSE for title. 
