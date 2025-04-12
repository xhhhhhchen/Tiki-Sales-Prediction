# Tiki-Sales-Prediction

### Problem Statement: 

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

## Exploratory Data Analysis and Data Cleaning (key insights) 
### Overview of NULL values: 
<img src="https://github.com/user-attachments/assets/7be7b740-0d26-47d9-8fe6-818ae302a2f2" width="400">
> Only `rating_average`contains NULL.


