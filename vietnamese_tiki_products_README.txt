https://www.kaggle.com/datasets/michaelminhpham/vietnamese-tiki-e-commerce-dataset

LICENSE:
Product crawled from open public API from TIKI.
Dataset used for education purpose

DATASET CLARIFICATION
This data is crawled from TIKI e-commerce platform, one of the top online marketplaces also reknowned as the AMAZON website in Vietnam.
The main task of this dataset is to predict the QUANTITY SOLD of a product. You can also perform your own ideas on this dataset as well.
Because of huge number of items on TIKI and for education purpose, I crawled only popular items (based on product ranking of TIKI). This would not only minimize the size of the data but also excludes 'empty' or unwanted 'dead' product ID.
**Note: **

main language is Vietnamese UTF-8,
currency is vietnam dong (vnd). For reference, 1 usd = 24,000 vnd (as of Sep, 2023)

FILES ARRANGEMENT
This dataset contains a range of .csv files, each represents one root category of TIKI.
Each file have the same formats and same number of features as well as headers.

>> JC has combined them into a single file
>> simple translation needed to be applied on the vietnamese language


Features explainations
id: unique product ID
name: product title
description: product description
original_price: price without discount
price: current price (may or may not applied discount)
fulfillment_type: dropship / seller_delivery / tiki_delivery. Dropship products go frrom (seller - TIKI warehouse - customers), packaging by TIKI. seller_delivery products go from (seller - customers) directly, packaging by sellers. tiki_delivery TIKI-self-owned products go from (TIKI warehouse - customers)
brand: OEM or registered brands. OEM means original equiment manufacturer or briefly 'no brand'.
review_counts: number of reviews
rating_average: average product rating
favorite_count: number users add this product to their favorite items
pay_later: allow a purchase now pay later program
current_seller: name of the seller
date_created: days from last update date and created date
number_of_images: number of product images uploaded
vnd_cashback: amount of vnd cashback
has_video: True if it has a video clip else False
quantity_sold: historical total units sold