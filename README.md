# Tiki-Sales-Prediction

**Table of Contents** <a id="intro"></a>
1.  [Problem Statement Formulation](#1-report)
2.  [Exploratory Data Analysis and Data Cleaning](#2-report)
3.  [Data Wrangling and Transformation](#3-report)
4.  [Machine Learning Modelling](#4-report)
5.  [Model Evaluation and Selection](#5-report)
6.  [Summary and Further Improvements](#6-report)

## 1. Problem Statement:  <a id="1-report"></a>  
[Top](#intro)  

The e-commerce sector is rapidly growing, influenced by numerous factors that affect sales and demand. With the rising popularity of various trends, it has become challenging to determine a product's popularity accurately. This creates a need for effective inventory management, ensuring that supply aligns with demand.

The goal of this project is to leverage the power of data analytics to build a predictive model that forecasts sales demand. This model will help optimize inventory management for online retail sellers by predicting the `quantity_sold` of a product. 

The data that we will be analysing on contains `18` variables and `44804` observations. <br>
Link to dataset: https://www.kaggle.com/datasets/michaelminhpham/vietnamese-tiki-e-commerce-dataset

Information available: 
* Product related: `id`, `name`, `description`, `brand`, `product_type`
* Sales metrics: `quantity_sold`, `original_price`, `vnd_cashback`
* User interaction: `review_count`, `rating_average`, `favourite_count`
* Metadata: `date_created`, `number_of_images`, `has_video`

Using available information on current products, sales metrics, and user interactions, the project aims enable sellers to manage inventory efficiently and prioritize popular products. 

## 2. Exploratory Data Analysis and Data Cleaning (key insights)  <a id="2-report"></a>  
[Top](#intro)  
**Overview of NULL values**: <br>
<img src="https://github.com/user-attachments/assets/7be7b740-0d26-47d9-8fe6-818ae302a2f2" width="200"> <br>
> Only `rating_average`contains NULL.


**Duplicacted Rows**: <br>
<img src="https://github.com/user-attachments/assets/05cabe48-495c-4edd-8712-c02576118670" width="200"> <br>
> Since IDs are unique identifiers, duplicated IDs should be removed.

### **Categorical Data Analysis** <br>
<img src="https://github.com/user-attachments/assets/99cd790e-2c9a-4007-ad57-884f9154589a" width="1000"> <br>
> * Most common fulfillment type is through ` dropship` , but `tiki_delivery` products consistently have a higher `quantity_sold` through higher mean and median.
> * Some items in `dropship` have a high `quantity_sold`.  <br>
 <br>
 <br>

![image](https://github.com/user-attachments/assets/0baf687f-4306-4ac0-96eb-163833326841)
> * `has_videos` products have a higher `quantity_sold`.
 <br>
 <br>
 
![image](https://github.com/user-attachments/assets/c65eb8aa-9f18-46de-abfb-1376c5b81197)
> * `fashion_accesssories` has many products but moderate `quantity_sold`.
> * The median `quantity_sold` for `men_bags` is 0, hence the product with extremely high `quatity_sold` is an exception in `men_bags`.
<br>
 <br>


<br>
<br>

### **Numerical Data Analysis**
![image](https://github.com/user-attachments/assets/29663f80-69e9-4992-a599-467ce5b50153)
> * `quantity_sold` increases as `review_count` increases
<br>
<br>

<img src="https://github.com/user-attachments/assets/92f03cf0-fea4-49cd-b361-41e9b7bfddaa" width="600"> <br>
> * `Original_price` and `price` are highly correlated
> * `Review count` has the highest correlation with `quantity_sold`
> * Weakly correlated variables with `quantity_sold` may need further transformations to uncover patterns.
<br>
<br>

![image](https://github.com/user-attachments/assets/fedbe6fc-6ce0-4bb2-a7b8-0b2780e69d3e)
> Difference in `original_price` and `price` does not affect the quantity_sold
<br>
<br>

#### **Data Errors**
![image](https://github.com/user-attachments/assets/dd930fc2-b0de-46e7-b2d8-175db61c0078)
![image](https://github.com/user-attachments/assets/a542e8b0-fae6-42c9-8986-a9f59bd7e45b)
> `date_created` with `738076` is a potential error as it is unrealistic to have a difference of 2022 years between the last update date and the created date, hence outlier trimming should be conducted.
>
 <br>
<br>

![image](https://github.com/user-attachments/assets/1ee27590-3f84-4351-88d7-c0aff19df517)
> `favourite_count` has only one category, it is not meaningful and can be removed.

<br>
<br>


#### **Examining `current_sellers` and `brands`**
![image](https://github.com/user-attachments/assets/3769b4a2-091e-4ed2-acc8-fc0ba20767e3)
![image](https://github.com/user-attachments/assets/5710807b-9267-4f1b-9985-879857374e70)

> * Most `sellers` and `brands` show consistent sales with low standard deviation.
> * Apply target-mean encoding to these variables to explore potential relationships with the target.
<br>
<br>

![image](https://github.com/user-attachments/assets/977fcbf8-fd04-4e06-831a-daaee0510b2a)
![image](https://github.com/user-attachments/assets/002c4583-ce51-4b2b-a063-0c1422d0012d)

> * `quantity_sold` is unevenly distributed, with many sellers and brands having low sales.
> * Group low-performing sellers and brands to reduce noise and prevent overfitting.

## 3. Data Wrangling and Transformation  <a id="3-report"></a>  
[Top](#intro)  
![image](https://github.com/user-attachments/assets/5ca154fe-539a-434c-8a8f-8bdaa124b672)

>The distribution and relation between `date_created` and `quantity_sold` is now clearer. 
<br>
<br>

#### **Missing Data Imputation**
> Missing `rating_average` values are imputed with the mean value from the training data. 
<br>
<br>

#### **Categorical Data Encoding**<br>
Categorical variables’ cardinality:<br>
<img src="https://github.com/user-attachments/assets/213d48b5-f6a4-4060-bf8d-a831185797722" width="200"> <br>


> * `id`,`name`, `description` are dropped as they have high cardinality and no direct relationship with the target variable
> *  `pay_later` and `has_video` are converted from Boolean to binary columns.
> * **One hot encoding** was applied to `product_type` and `fulfillment_type` to preserve their independence. 
> * After various testing on encoding `current_sellers` and `brand`, model performs better with dropping `current_sellers`, and conducting **target-mean encoding** to `brand` without grouping less popular categories.
<br>
<br>
#### **Numerical Data Transformation** <br>
> * Transformations are done to highly skewed features (`original_price`, `price`, `review_count`, `vnd_cashback`,` brand_mean_encoded`) as they can cause imbalanced splits for random forest regressor, affecting predictive accuracy 

After transformation: <br>
1. More normalized distribution: 
>![image](https://github.com/user-attachments/assets/70f4f1e8-2f07-4a67-bd04-c4a376f1f276)
>![image](https://github.com/user-attachments/assets/a767d8f2-92c4-4101-8555-c34b7f754547)
>![image](https://github.com/user-attachments/assets/2616d13f-a91e-4f64-b471-f20f067d28db)
>![image](https://github.com/user-attachments/assets/d9361c88-631f-4878-b3e2-7e210664da4d)
>![image](https://github.com/user-attachments/assets/5a110b18-a097-421e-a797-b990f499b91c)

<br>
2. Better model performance: <br>
<img src="https://github.com/user-attachments/assets/a704c31c-b8b6-4740-b3cd-7b251c4d038f" width="300"> <br>


<br>
<br>
 
#### **Correlation  Analysis**
Correlations between the variables are reviewed again:  <br>
<img src="https://github.com/user-attachments/assets/79f138c8-7424-44ae-b83e-adf6b18b5b15f" width="600"> <br>

>**multicollinearity:**
>* `original_price` & `price`
>* `review_count` & `rating_average`
>
> **low correlations with the target:**
>* `vnd_cashback`
>* `number_of_images`

> Multicollinear variables affect model performance and are dropped. 

<br>
<br>
 

#### **Feature Scaling**
Scaling features ensures that features with larger scales don't dominate the decision-making process of regression models, allowing for accurate identification of important features. 

 * Both standard and robust scaling were tested.  <br>
<img src="https://github.com/user-attachments/assets/6285923d-59e8-4f51-a278-feba784fdd67" width="300"> <br>

> From the results, the model performs better with robust scaling. 

## 4. Machine Learning Modelling <a id="4-report"></a>  
[Top](#intro)  

Rows and column for final data: <br>
<img src="https://github.com/user-attachments/assets/4bff7375-56f7-4fd5-9764-9e19f9925b29" width="300"> <br>

 > After transformations, the final training set contains 26064 observations and the test set includes 11062 observations.
<img src="https://github.com/user-attachments/assets/2f89b8de-1edb-4641-a475-8feee2cf45fe" width="500"> <br>
 The variable used to train the model is `features_sscaled`, a vector containing all the features after scaling is applied. <br>


#### **Building and testing different models**
* Various regression models are built and trained on a dataset with minimal cleansing (only missing and categorical features handled) to determine the best model with the current data. 
* After various testing and evaluations (to be discussed in further details in the next section), I have chosen to work with the **Random Forest Regressor**, and conducted further data wrangling to fine-tune the model (outlier and numerical transformations on the data)

#### **Features Importances**
* After wrangling the data and training the model, the `.featureImportance` attribute in the model was used to find out the relative importance of each feature to the model's predictions.
 <br>
<img src="https://github.com/user-attachments/assets/80885dbf-1bfb-482b-a317-eada2a9c3bcf" width="300"> <br>
 Based on the results, variables `has_video`, `brand_mean_encoded` and `fulfillment_type_one_hot` have low importance to the model’s predictions.  <br>
* Hence, they should be removed to reduce complexity and improve model performance.
<br>

<img src="https://github.com/user-attachments/assets/8346b6d4-64f9-470b-8111-ac4acd4aa7db" width="500">  <br>
> Better model performance is observed after removing redundant features.
<br>
<br>

## 5. Model Evaluation and Selection <a id="5-report"></a>  
[Top](#intro)  

#### **Model Comparison**
1.	Linear Regression
> * The baseline linear regression models showed limited predictive capability, with moderate training R^2 (~0.32) and testing R^2 (~0.51).
> * Applying scaling and regularization (Lasso, Ridge, Elastic Net) did not significantly improve the performance, with Testing R^2 remaining around ~0.54.

2.	Gradient Boosted Trees (GBTRegressor)
> * While GBT performed well on the training data (R^2 of 0.97), its testing performance dropped significantly (R^2 of 0.31), indicating overfitting.
> * GBT’s higher training complexity and sensitivity to overfitting make it less reliable for this dataset. 

3.	Decision Tree Regressor
> * With a maximum depth of 5, Decision Trees provided decent training performance (R^2 of 0.88) but relatively lower generalization on the test set (R^2 of 0.34).

4.	Random Forest Regressor
> * Random Forest models showed the best balance between training and testing performance, especially after hyperparameter tuning.
> * The final configuration (numTrees=200, maxDepth=7, maxBins=120, seed=42) and further data transformation achieved a test R^2 of 0.7247, with an RMSE of 64.97 and MAE of 11.91. This indicates a significant improvement over other models.
> * Adding minInstancesPerNode=2 slightly reduced overfitting, maintaining test performance (R^2 of 0.7245) while controlling training complexity.

**Model Selection**
> * The Random Forest Regressor is chosen due to its strong generalization ability and relatively low error overall.

#### **Model Evaluation**
![image](https://github.com/user-attachments/assets/4603feff-a6af-46e7-824f-95858b0aaa03)

> This plot shows the model's prediction errors for quantity_sold on the test set. Most residuals are centered around zero, indicating unbiased predictions, but accuracy declines as quantity_sold increases.


### 6. Summary and Further Improvements <a id="6-report"></a>  
[Top](#intro)  

**Key Insights**
1. Exploratory Data Analysis
> * The number of reviews shows a strong relationship with the `quantity_sold`.
> * For most sellers and brands, the `quantity_sold` is relatively consistent across different products. However, some brands and sellers achieve higher popularity and higher overall sales, while the majority have low overall quantities sold.
> * The `men_bags` product category includes a product with an exceptionally high quantity sold compared to others.
> * Errors are identified in the `date_created` field, and missing values are observed in `rating_average`.
2. Data Wrangling Process
> * Most variables were highly skewed, and their skewness was addressed using numerical transformations.
> * After transformations, variables exhibiting multicollinearity were identified and removed.
> * Based on the feature importance, the top 3 most importance variables in predicting the `quantity_sold` is `review_count`, `date_created` and `original_price`
> * Variables such as `has_video`, `fulfillment_type` has low importance in predicting the `quantity_sold`

**Further Improvements**
> Based on the residual plot, the model struggles to predict products with high quantities sold. To improve performance, I can 
> * focus on analyzing the subset of data with products with higher sold quantity, examining patterns among sellers or brands that consistently achieve high sales. This analysis could help the model capture hidden relationships, potentially enhancing its predictive accuracy.
> * Examine temporal features such as seasonality or locations as the popularity of products could vary in different times of the year or different regions in Vietnam. 



