# Keyword-Auction Bidding Optimizer for Amazon Advertising

## Introduction

This project focuses on developing a **Keyword-Auction Bidding Optimizer for Amazon Advertising**. Amazon provides a **real-time bidding (RTB)-based display advertising arrangement** to enable sellers to advertise their products. This system uses a **Pay-per-click (PPC) model** based on **second-price auctions**.

The motivation behind this project is that Amazon, as an e-commerce platform, offers a **keyword-based RTB facility** for advertisers. Advertisers aim to **maximise revenue** by choosing optimal bid and daily budget values for their campaigns, subject to cumulative budget constraints. This is a challenging optimisation problem involving intricate subproblems such as **finding the best bidding in auctions, estimating value per click, estimating conversions and profits, and deducing an optimised ROAS (Return on Advertising Spend)**.

The concept of the **Marketing Funnel** is relevant, representing the journey a shopper takes, where customer traffic reduces through stages like Awareness, Consideration, and Conversion.

## Objective

The primary aim is to **identify and explain the changes of inputs (particularly Bid, Budget) in an advertising campaign on its output evaluative parameters** such as ROAS, Impressions, and CTR (Click-Through Rate). This project was further focused on developing a **prediction model system** that, given specific ad-campaign inputs, can predict the end-measure '**profitability**' of the campaign in a multi-class manner. An expansion includes a 'what if' analysis inspired decision tree to show the results of various input changes on profits.

The system is conceptualised as a Black-Box taking Bid and Budget as inputs and predicting Profitability.

## Related Work

The project draws on related work concerning factors influencing online consumer behaviour, internet marketing techniques, web document classification, bidding strategies for cloud instances, and mechanisms for auctions. Specifically relevant is work attempting to **predict CTR and average CPC for keywords** using machine learning techniques like Linear Regression, Random Forest, and Gradient Boosting on keyword-level data.

## Proposed Technique and Algorithm

Initially conceived as a multi-module system including Keyword Extraction, Keyword Matching, and Bid-Strategy Optimizer, the current phase focused on a sub-problem: **predicting campaign profitability given bids and budget**. This problem was divided into three parts, corresponding to the stages of the advertisement funnel: Awareness, Consideration, and Conversion phase.

The approach involves a multi-module system for prediction:

1.  **Impression Classification/Prediction**: This module predicts Impressions. An increased bid amount is expected to increase the chances of winning an auction, leading to more impressions and potentially inflated subsequent metrics and profits. The classification model takes **bid, Match-type, and SKU-rank** as input and predicts Impressions as a multi-class output. Decision Tree, Random Forest Classifier (with grid-search), and multilayer perceptron based classifiers were used. A Random Forest based regressor was also tested to forecast Impressions numerically based on bid and budgets as changing inputs and other control variables.
2.  **Click Classification/Prediction**: Following the intuition that increased Impressions drive more clicks, this module predicts Clicks. It uses **Campaign-Keyword level data** for keywords with 'Enabled' status. Among four classifiers applied, the Random Forest based classifier showed better accuracy. A similar module predicts Click values based on inputs and predicted impressions.
3.  **Cost Prediction**: Uses predicted impressions and clicks to deduce output, employing a **Random Forest based regressor**. No control variables were used in this module.
4.  **Sales Prediction**: Uses predicted values of impressions and clicks to deduce output, also employing a **Random Forest based regressor**. No control variables were used here either. This module was tested against actual attributed sales values.
5.  **Profitability Prediction**: Uses the estimated values of clicks, impressions, costs, and sales to predict profits from a given input set [8]. The formula used is: **Profit = (RPC - CPC) * impression * CTR / 100**.

## Datasets Used

The project utilises several datasets:

*   **Amazon-product review dataset** from Kaggle: Used for keyword extraction and sentimental analysis, containing reviews for various ASINs.
*   **Amazon-mobile-electronics dataset** from Kaggle: Used for sentimental analysis with DistilBert, containing reviews for products.
*   **Historical business data** hosted on a Snowflake data warehouse: This is the primary dataset, providing data at various granularities including keyword, ad-group, and campaign levels. It also includes data related to keyword searches and product ranks.
    *   Relevant tables mentioned include `keywords_report`, `keywords_headline_report`, `asins_report`, `campaigns_report`, `campaigns_headline_report`, `search_data_archive`, `search_data_info`, and `keyword_classification` [10].

## Experiments and Results

*   **Keyword Extraction**: Using TF-IDF, lessons learned included omitting negative words and understanding that users search for what they want, not what they say about a product.
*   **Sentiment Classification**: A Pretrained DistilBERT model gave an accuracy of **74.7%**. A BOW-based RF model over Electronic data yielded **82.2%** accuracy.
*   **Impression Classification**: Tested with Decision Tree, Random Forest Classifier, and NN-sgd (4-class classification) using inputs (keyword\_bid, search\_type, search\_rank). Accuracies were 54.6% (Dtree), 57.6% (RF), and **59.4%** (NN). Initial lower accuracies (0.37, 0.44) were improved by equally sampling records to address data skew.
*   **CTR Classification**: Tested with Decision Tree, Random Forest Classifier, and NN-sgd (4-class classification) using inputs (Match\_score, keyword\_bid, impressions, rank). Accuracies were 40% (Dtree), **45.85%** (RF), and 41.2% (NN).
*   **Regressor Prediction (Sales, Cost)**: Random Forest based regressors were used. Sales prediction based on impressions and clicks achieved **40.94%** accuracy. Cost prediction based on impressions and clicks achieved **99.9%** accuracy. Mean Absolute Percentage Error (MAPE) was used for evaluation to provide a relative scale for errors.
*   **Regressor Prediction (Impression, Click)**: Used Random Forest based regressors to predict intermediate values. Predicting Impression based on (BID, CAMPAIGN\_BUDGET, PROFIT, CTR, COST, SALES) achieved **85.51%** accuracy. Predicting Click based on (BID, CAMPAIGN\_BUDGET, Impression, PROFIT, CTR, COST, SALES) achieved **99.9%** accuracy. Initial low accuracy (11%) for Impression prediction was improved by filtering data by a single campaign ID to get a uniform trend.

## Conclusion and Future Work

An initial attempt has been made to solve the **profitability prediction problem**. Accuracy for some intermediate predictions has been acceptable, while others require verification and internal discussion.

Future work could explore introducing a **UI based solution** capable of predicting the profit class based on input bids and budgets for campaigns whose historical data is available for training.

## References

The work builds upon concepts and techniques discussed in various research papers.
*(Note: Specific references from the sources are listed in section, but not replicated here as full citations unless necessary)*

## Thank You
