# Citation Network Analysis

## Introduction
In this project, we aim to analyze the interconnections between different fields of research and understand which fields are merging and which are diverging over time. We are using a citation dataset extracted from DBLP, ACM, MAG (Microsoft Academic Graph), which spans from 1936 to early 2022 and contains 5,259,858 papers before cleaning, in JSON format hosted on aminer.com, with a size of approximately 18 GB unprocessed.

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
