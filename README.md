# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)  Project 3 - Sentiment Analysis of Elon Musk using YouTube Comments
---
### Introduction & Problem Statement

Public image refers to the 'the character or attitudes that most people think' a person or organisation has ([source](https://www.ldoceonline.com/dictionary/public-image)). In most cases, a good public image is essential in:
1. Building trust, 
2. Attracting customers, and 
3. Fostering positive relationships with stakeholders.

As such, it is important for individuals and organisations to keep their finger on the pulse when it comes to understanding public sentiment around them at all times. In this project, I am going to focus on the public sentiment of a well-known and polarising figure the past few years, Elon Musk. In this scenario, I am part of a data science-backed PR agency to help Elon navigate the murky waters of media scrutiny and public perception, ensuring his actions and communications align with his goals.


The problem statement is:
**<center>Can we create an effective classification model to identify the level of negative sentiment in text comments?</center>**

The ideal outcome / goal is to help individuals and companies with understanding public sentiment around them in order to build their PR strategy. This will be done by creating a sentiment analysis model, with the dependent variable being a classifier whether a comment made about them is negative or not (where '1' refers to a negative comment and '0' refers to a non-negative comment).

Success of the model will be determined by the recall and accuracy scores of the model. The higher the recall and accuracy scores, the better it is at predicting the public sentiment around an individual or company. Between the two, we will prioritise the recall score in order to minimise missing negative sentiments as much as possible. This is so that we have more data to look through if we want to dive deeper into understanding the reasons behind negative sentiments.

The primary audience will be individuals and companies of interest, while the secondary audience will be  existing PR companies that we can explore partnerships with.

---

### Methodology

Our methodology is as follows:

1. Cleaning of Data
    1. Handling of null values
    2. Handling duplicated values
    3. Handling data types
    4. Handling of HTML-encoded entities
    5. Handling of Foreign Languages
2. Feature Engineering
3. Establishing Ground Truth
    1. Running VADER Sensitivity Analysis Tool
    2. Manual labeling of comments
4. Exploratory Data Analysis 
5. Pre-Modeling
    1. Tokenization + Lemmatization
    2. Stop-word removal
    3. Train-test-split
6. Modeling
    1. Model 1: TF-IDF vectorization + Multinomial Naive Bayes model
    2. Model 2: RandomOverSampler + TF-IDF vectorizer + Multinomial Naive Bayes model
    3. Model 3: SMOTE + TF-IDF vectorizer + Multinomial Naive Bayes model
    4. Model 4: RandomOverSampler + TF-IDF vectorizer + Random Forest model
    5. Model 5: RandomOverSampler + TF-IDF vectorizer + Logistic Regression model
    6. Model 6: RandomOverSampler + TF-IDF vectorizer + Support Vector Machine model (linear)
7. Running Production Model on Old and New YouTube Videos of Elon Musk
    1. Old Video Scores
    2. New Video Scores
    3. Comparison of Scores
8. Explanatory Data Analysis
    1. Most Influential Words from Training Video (Interview with Joe Rogan)
    2. Most Frequent Words from Test Video (Old YouTube Video)
    3. Most Frequent Words from Test Video (New YouTube Video)
    4. Comparison of 'is_negative' Proportions Between Old and New Videos
9. Conclusion and Future Works
---

### Model Evaluation

Based on all the models that we ran, we can say that all the models performed reasonably well. However, a majority of the models led to overfitting, with the original Multinomial Naive Bayes model and SVM being particularly strong culprits. The results are as follows:

| # | Model Type                      | Addressed Class Imbalance? | Recall Score                | Accuracy Score              | Review                                                             | Choose model? |
|---|---------------------------------|----------------------------|-----------------------------|-----------------------------|--------------------------------------------------------------------|---------------|
| 1 | Multinomial Naive Bayes         | No                         | Train: 0.85<br>Test: 0.346  | Train: 0.959<br>Test: 0.811 | Massively overfit. Test score for recall is much lower than train. |               |
| 2 | Multinomial Naive Bayes         | ROS                        | Train: 0.935<br>Test: 0.863 | Train: 0.806<br>Test: 0.742 | Mildly overfit.                                                    | ✓             |
| 3 | Multinomial Naive Bayes         | SMOTE                      | Train:0.860<br>Test: 0.760  | Train: 0.864<br>Test: 0.812 | Mildly overfit, lower score than ROS.                              |               |
| 4 | Random Forest                   | ROS                        | Train: 0.736<br>Test: 0.666 | Train: 0.889<br>Test: 0.839 | Lower scores, underfit.                                            |               |
| 5 | Logistic Regression             | ROS                        | Train: 0.885<br>Test: 0.771 | Train: 0.904<br>Test: 0.864 | Strongly overfit.                                                  |               |
| 6 | Support Vector Machine (Linear) | ROS                        | Train: 0.936<br>Test: 0.751 | Train: 0.935<br>Test: 0.860 | Very strongly overfit.                                             |               |

Overall, I would say that the Multinomial Naive Bayes (with RandomOverSampler) performed the best, even though there is still overfitting in the model.

When it comes to running the production model on the test sets (ie. old and new YouTube videos on Elon Musk), we can see that there has been a change in sentiment on Elon Musk. While 41.98% of comments on older videos about him are negative, we can see that it has increased to 64.49%. The difference is statistically significant based on our two-tailed proportion Z-test.

This test highlights the deteriorating sentiment towards Elon Musk, and would be a starting point for us to recommend a PR strategy for him.

---

### Recommendations and Next Steps

There are a few issues with this project that I feel could be addressed in order to create a better Sentiment Analysis classifier.

1. Due to recent changes in API limitations on other websites, the best way I could represent sentiment analysis of Elon Musk is through YouTube videos of him. However, the videos that we have gotten are not purely about Elon Musk, thus making the analysis not completely accurate. For future studies, I would recommend using Twitter's API, as they would give a better representation of one's opinion about him.
2. As the dataset is imbalanced, it can lead to a bias in labeling due to limited exposure to the minority class. While we addressed this using RandomOverSampler and SMOTE, a better strategy for future works would be to gather more balanced data.
3. While the VADER lexicon was a decent model for sentiment analysis as it could handle emojis and slang, it might make sense for us to do a deeper dive into the lexicon to see it is attuned to more recent slang (eg. 'W rizz', or 'slay').