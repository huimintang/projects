# Project 3: Web Application Programming Interface (API) & Natural Language Processing (NLP)
---




# Subreddit Classification 

## Problem Statement

We are a team of data analysts working in a leading tech magazine that strives to be a trusted source of latest technology trends and electronic brand reviews.

To help the publication team come up with a big year-end story on “The Brutal War for Brand Dominance between 2 consumer electronic giants - Samsung and Apple”, we have been approached by the Editor-in-chief to develop a text classifier where the technology journalists can leverage to have a quick sense of the online chatters of the 2 top tech-brands, instead of looking through the forums manually.

The text classifer will be built using supervised learning techniques. It will be capable of distinguishing whether an online post is about Samsung or Apple, and be able to surface some meaningful insights on the most talked-about topics which hopefully could provide the technology journalists some inspiration for their article.


### Problem Statement : 
To create a text classifier to determine whether a Reddit post submitted by a user would be classified under the Subreddit groups "Samsung" or "Apple" (i.e. binary classification problem) using supervised learning techniques and derive meaningful insights on the most talked-about topics relating to Samsung and Apple.


## Executive Summary and Recommendations



### Content: 

[**Part 1**](./code/1_data_scraping.ipynb):

1. Data Scraping

[**Part 2**](./code/2_data_cleaning_eda.ipynb):

2. Data Cleaning
3. Preprocessing
4. Exploratory Data Analysis

[**Part 3**](./code/3_modelling.ipynb):

5. Model Selection
6. Conclusion and Recommendations

### Methodology:

Textual data is first scraped from the two subreddits, r/Apple and r/Samsung, using Pushshift's API and selecting the respective endpoints. With a comprehensive dump of data, we have conducted data cleaning, pre-processing, exploratory data analysis, and modelling to develop a text classifier of high accuracy. 

From the data inspection and exploratory data analysis, we observed that the posts consist of 'selftext' and 'title' columns, both of which contain useful text of predictive value for our model. As such, we combined them into one single 'text' column, which is assigned as the predictor X-variable.  

The data is then pre-processed through a series of routine cleaning steps. Punctuation, emojis, hyperlinks and whitespaces are also removed from the text before tokenizing and application of standard stopwords in order to facilitate the development of a high-quality machine learning algorithm and generate better insights. The eventual dataset that is fed to the model consisted of 14,359 unique posts across both r/Apple and r/Samsung. 

As this is a binary classification problem, we employed a range of supervised learning techniques below with data vectorized using **CountVectorizer** and **TfidVectorizer** (Term Frequency Inverse Document Frequency Vectorizer), starting with the default hyperparameters before performing tuning via Grid Search CV: 
- **Logistic Regression**;
- **Multinomial Naive Bayes Classifier**; and
- **Random Forest Classifier**

We measured the performance of our classifier model by looking at several metrics, including, precision, recall, F1-score, Receiver Operating Characteristic-Area Under Curve (ROC-AUC), with a primary focus on:
- Accuracy (ideally > 0.8 accuracy score and minimally better than baseline accuracy); and
- Generalisability (Little signs of overfitting; variance between the 'train' and 'test' scores is low)

All models provided similar accuracy of between 0.92 to 0.94, though the Random Forest Classifers were found to be significantly overfitting to the data. 

### Evaluation : 

We had selected **Logistic Regression with TF-IDF Vectorizer with hyperparameter tuning** as our best model for deployment to classify Samsung and Apple posts after scoring and evaluating all the models. 

The test dataset performed well, returning a accuracy score of 0.948468. It can also generalize unseen data well with the small difference in accuracy rate of 0.029710 between the 'train' and 'test' scores. 

There is also a strong improvement over the baseline accuracy score of 0.507799 by using the normalised prediction of all posts. 

However, the model has its limitations. As we deep-dived into the wrongly classified posts (i.e. false positives and false negatives), posts mentioning switching of brand loyalty from Apple to Samsung or vice versa, are often misclassified. This shows that while r/Apple and r/Samsung are fairly different, they still hold some similarities.  

### Recommendations : 

**1. Consider discussing Apple/ Samsung products in the magazine article**

Based on the model's coefficients, the common words that have the strongest predicting power in distinguishing the class are the brand names itself - 'Apple' and 'Samsung', followed by the words that are product-specific. These insights are also similarly observed in the EDA. The Editor-in-chief could consider including some of the latest products that are gaining traction in the magazine article since they are found to be widely discussed. 

**2. Re-run the model by including additional stopwords like brand names**

One of the future considerations is to re-run the model by including the brand names (i.e. 'Apple' and 'Samsung') as stopwords. It would be interesting to see how the model will perform if it disregards the brand names. The model could also surface other strong predictive words that were previously overshadowed by the brand names. 

**3. Deploy the text classifier to other tech forums**

As our model has learnt how to differentiate between 'Apple' and 'Samsung' posts on Reddit, the model can be deployed to other tech forums to help classify posts into the 2 classes. This can ease the burden of the technology journalists to a certain extent as it can help organise their research materials. 

**4. Perform sentiment analysis**

It is not enough to just listen to what people discuss online; it is more important to comprehend the meaning of the online texts. As seen from the Ngrams illustrations, the strong predictive words are primarily related to the product names, which are relatively neutral. More advanced text analytics or sentiment analysis could be conducted to uncover the hidden sentiments behind the online texts. The insights could provide some useful inspiration for the technology journalists' articles.

## Data Used in Analysis





### Datasets:
* [`df_apple.csv`](../data/df_apple.csv): Subreddits scraped from [`r/apple`](https://www.reddit.com/r/apple)
* [`df_samsung.csv`](../data/df_samsung.csv): Subreddits scraped from [`r/samsung`](https://www.reddit.com/r/samsung)

### Data Dictionary : 

The key features used in the project are listed below:

|Feature|Type|Dataset|Description|
|:---|:---:|:---:|:---|
|author|*object*|ap_clean/ss_clean|creator of the post| 
|num_com|*int*|ap_clean/ss_clean|number of comments in the post|
|selftext|*object*|ap_clean/ss_clean|body text of the post| 
|title|*object*|ap_clean/ss_clean|title of the post| 
|date|*object*|ap_clean/ss_clean|date in string format| 
|datetime|*datetime*|ap_clean/ss_clean|datetime of post| 
|title_len|*int*|ap_clean/ss_clean|length of title| 
|selftext_len|*int*|ap_clean/ss_clean|length of selftext| 
|is_samsung|*int*|ap_clean/ss_clean|mapped as 1 for r/Samsung and 0 for r/Apple| 
|text|*object*|ap_clean/ss_clean|combination of processed title and selftext| 

