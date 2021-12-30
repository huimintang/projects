# Capstone Project :  
Skincare Product Analysis - Decoding your Skincare Product
-------------------------------------------------------


------------------------------

## Background

The global skin care market is projected to increase from USD 130 billion in 2020 to over USD 177 billion by 2025 [[Statista, 2021]](https://www.statista.com/topics/4517/us-skin-care-market/#dossierKeyfigures). The ongoing growth of the industry shows the consumers' sustained interest in beauty regimes, as well as in basic personal care and hygiene practices.

With the explosion of skincare brands around the world, consumers are faced with many choices, making it more difficult to make informed decisions of their purchases. Adding to information paralysis, there are costs increasing the barriers to the purchase. Time is spent on researching through a myriad of products and ingredients. Trial and error is also costly both in terms of time and money. Skincare products are not always cheap and it takes time to see results. There is also a risk that the product may cause adverse reactions which is a major concern especially for consumers with sensitive skin.

By helping consumers understand what makes the product cheap or expensive and be aware of what they could be paying for, we can help them make more informed decisions. 
Consumers with a particular skin type, e.g. sensitive skin may also want to know if there are similar alternatives to their tried-and-tested products that can help them avoid unnecessary allergies.
If the product is not worth the price tag, consumers may want to know if there is a cheaper alternative that promises similar effect.

### Objectives of Project

This project aims to:

1) Build  a classification model to identify the product characteristics that affect prices of skincare products by analysing the ingredients, skincare concerns addressed, skin type that the product is suitable for,  brand, popularity, etc. 

2)  Build a recommender to help consumer find the next closest product based on product similarity of the ingredients. 

### Problem Statement

1) Help consumers understand what makes a skincare product cheap or expensive so that they can make more informed decisions.

2) Help consumers discover similar alternatives to their tried-and-tested products or even ‘dupe products’.

## Executive Summary and Recommendations

### Content: 

**[Part 1](./code/1_data_cleaning.ipynb)**
1. **Data Cleaning** 

**[Part 2](./code/2_eda_preprocessing.ipynb)**

2. **Exploratory Data Analysis**  

**[Part 3](./code/3_data_cleaning_ingr.ipynb)**

3. **Data Cleaning of Ingredients**
4. **Exploratory Data Analysis of Ingredients**

**[Part 4](./code/4_recommender.ipynb)**

5. **Exploring Metrics for Recommendation System**
6. **Final Recommender System**

**[Part 5](./code/5_modelling_insights.ipynb)**

7. **Modelling**
8. **Insights**

### Methodology:

**Datset:**

The dataset used was from Kaggle, which was scrapped from Sephora’s website. Sephora’s data was used because it is one of the largest beauty retailers in the United States. Only skincare products were covered for this project as I wanted to be more targeted. Furthermore, skincare product made up the largest share (42 percent) of the global cosmetic market in 2020 [[Statista, 2021]](https://www.statista.com/statistics/243967/breakdown-of-the-cosmetic-market-worldwide-by-product-category/), making it a compelling area of research.


**Data Cleaning & Pre-processing:**

In terms of data cleaning efforts, products without ingredients were removed from the dataset. Products with no ‘size’ like gift or bundle sets were also removed because ingredients and  product volume were used as features. As the ‘size’ values were in different volumetric units like ounces, millilitres, etc, they had to be standardized. Duplicated products were discovered during cleaning and were removed accordingly. During the data inspection, product refills were also found in the dataset. They were removed since the ingredients would be the same as the main product.

Multicollinearity between the features were checked using pairwise correlation. One feature of each pair of the highly correlated features were dropped because highly correlated features bring no new information to the dataset and our model.
Features without much variance or variability in the data were also dropped because they provide little information to an model for learning the patterns.

As the ingredients list contained other unnecessary information like descriptions of the ingredients, Regex was used extensively in the text data pre-processing to extract only the ingredients and not the descriptions of the ingredients. The text data-preprocessing of the individual  ingredient terms was more challenging because  different products could be having the same ingredient but the ingredients were listed with slight variations  due to different nomenclatures used. For example, some products may have 'birch bark extract', while others list it as 'birch extract' or 'bark extract'. Before vectorization, we remapped such ingredients which are essentially the same, into a single ingredient term  so that we can reduce the total number of unique ingredient terms. 

In order to do this quickly and to a certain level of accuracy, Fuzzy Matching technique (also called Approximate String Matching)  from the FuzzyWuzzy Library was explored. This technique computes the standard Levenshtein distance similarity ratio between 2 ingredient terms and we get a score out of 100, based on the similarity of the strings of ingredient terms. 
The higher the score, the more similar the 2 ingredient terms.  Based on the fuzz.token_sort_ratio, ingredient terms that were above my threshold of 80% similarity score, were matched and remapped as a single ingredient term.
This technique helped to reduce the total number of unique ingredient terms from 7700 to 5100. 

After cleaning and processing, I divided the dataset into 3 equal-sized bins of different price range - 'cheap', 'average' and 'expensive' class, which will form the y-target variable. 

**Modelling Approach:**

The text data (consisting of the bag of ingredient terms) was passed through either the Countvectorizer or TfidVectorizer, while the numeric data was scaled simultaneously before the output was fed to the models. As this is a 3-class classification problem, a range of classifiers below with data vectorized using **CountVectorizer** and **TfidVectorizer** (Term Frequency Inverse Document Frequency Vectorizer) were employed, starting with the default hyperparameters before performing tuning via Grid Search CV: 

1) Multi-class Logistic Regression

2) K-nearest Neighbors

3) Random Forest Classifier

4) Support Vector Machine

5) Adaptive Boosting Classifier

6) Gradient Boosting Classifier

7) Extreme Gradient Boosting Classifier

**Recommender System:**

As an extension to the project, I also attempted to create a simple recommender system to help consumers find the next closest product based on product similarity of the ingredients using cosine similarity matrix. With the recommender,we can:
- Qeury for a product that is similar to a selected product.
- Compare similarity of two products.
- Use the recommender to surface cheaper alternatives of similar products.

### Evaluation : 

The evaluation metric that was used to evaluate model performance was Accuracy because we are more concerned with the true positives and true negatives. The baseline model used to provide the lower bound expected performance of the other models was the dummy classifier which predicts the majority class from the training dataset with an accuracy of 0.349705. My goal was to achieve at least 0.70 accuracy because the dataset was not very clean to begin with.  

All the tuned and un-tuned models had outperformed the baseline model. All models provided similar accuracy of between 0.595 to 0.745, though the XGBoost and K-Nearest Neighbors were found to be significantly overfitting to the data. Generally, the tuned models had improved accuracy scores on the unseen data ('test') and both the Support Vector Machine Classifier and Multi-class Logistic Regression performed fairly well. Our best performing models were as follows, with 2 models having a tie at second place:
   1) Multi-class Logistic Regression with TfidfVectorizer (accuracy score = 0.744597)
   
   2) Multi-class Logistic Regression with with CountVectorizer (accuracy score = 0.734774)  
   
   2) Support Vector Machine Classifier with TfidfVectorizer (accuracy score = 0.734774)   

There was also a lift of 0.394892 in terms of accuracy score of the Multi-class Logistic Regression with TfidfVectorizer from the baseline model of 0.349705. It was able to generalize unseen data better as indicated by the narrow difference between the 'train' and 'test' scores of 0.217612. 

As such, the **Multi-class Logistic Regression with TfidfVectorizer** was selected for the deployment to classify skincare products in price range classifications of 'cheap', 'average' and 'expensive'. 

### Results and Insights

**1) Identified determinants of product price**
- The 3-class logistic regression model (with TfidfVectorizer) revealed pricing patterns in the skincare products with it being able to predict price categories with an accuracy of 74.5%. 
- There are specific brands that are strong predictors of price for each class.
- Product category is more important in predicting cheaper products.
- Products with ingredients that are non-toxic or have anti-aging properties are more expensive.

**2) Analysed product ingredients**
- Plant-based ingredients are associated with average- expensive products.
- Cheaper products have more alcohol derivatives, which are known to be more harmful and drying to the skin. 

**3) Created a simple recommender system based on product ingredient similarity**
- Able to query for a product that is similar to a selected product.
- Able to compare similarity of two products.
- Possibility of using the recommender to surface cheaper alternatives of similar products.

### Limitations and Future Consideration  

- In establishing product similarity, the proportion of ingredients is also an important factor as the concentration can change the product’s effectiveness/ similarity. However, this level of detail is not publicly available and is considered out-of-scope for this project.

- Generalisability may be limited as only products available on Sephora US are covered. There is little coverage for Asian skincare products, which may contain other types of common ingredients like  snail mucin and ginseng. One future consideration would be to include products from Asia and other continents in the analysis. 

- Collaboration with cosmetic chemists could facilitate better analysis on the effectiveness of the ingredients. 


## Data Used in Analysis

- [`sephora.csv`](../data/sephora.csv): Dataset was from [`Kaggle`](https://www.kaggle.com/raghadalharbi/all-products-available-on-sephora-website) which was extracted from [Sephora's website](https://www.sephora.com/).

## Data Dictionary

The key features used in the project are listed below:

|Feature|Type|Description|
|:---|:---:|:---:|
|id|*int*|product id| 
|brand|*object*|product brand|
|category|*object*|product type|
|name|*object*|product name|
|size|*object*|volume of product|
|rating|*float*|ratings of product from 1 to 5| 
|number_of_reviews|*int*|number of reviews submitted| 
|love|*int*|number of times the product was loved|
|price|*float*|discounted price of product|
|value_price|*float*|original price of product|
|URL|*object*|link to product|
|MarketingFlags|*bool*|whether or not the product had marketing tags| 
|MarketingFlags_content|*object*|type of marketing tags|
|options|*object*|different product sizes available|
|details|*object*|product description| 
|how_to_use|*object*|instructions to use product|
|ingredients|*object*|product ingredients|
|online_only|*int*|1 if the product has online marketing tags and 0 for none|
|exclusive|*int*|1 if the product has exclusive marketing tags and 0 for none|
|limited_edition|*int*|1 if the product has limited edition marketing tags and 0 for none|
|limited_time_offer|*int*|1 if the product has limited time offer marketing tags and 0 for none|
|skintype|*int*|skintype that the product is suitable for|
|concerns|*int*|skincare concerns that the product is targeted for|
|pref|*int*|specific preferences of skincare product composition|
|skincareacids|*int*|specific acids present in skincare product|
|excluded|*int*|ingredients that are excluded in the product|
|formulation|*int*|product formulation type|
|award|*int*|whether or not a product had achieved allure awards or the likes|
|clinical_results|*int*|whether or not a product as clinically tested|
|size_ml|*int*|volume of product in ml|
|price_per_unit_vol|*int*|derived by dividing value_price with size_ml|
|price_range|*int*|whether the product is cheap, average or expensive|
|ingredients_list|*int*|list of ingredients in the product extracted from details|
