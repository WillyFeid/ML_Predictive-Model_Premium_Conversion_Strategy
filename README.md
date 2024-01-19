# HighNote Freemium to Premium Conversion Strategy

## Project Overview
You are all familiar with the HighNote freemium monetization case. HighNote is an online
community similar to Spotify (and LinkedIn and OkCupid) that allows both free users and
premium users to co-exist. The key question is, Who are the top 1000 most likely free users to convert to premium?

This project aims to develop a data-driven strategy to convert free users to premium subscribers within the HighNote online community. Using advanced machine learning techniques, we identify the top 1,000 free users who are most likely to upgrade to a premium subscription when targeted with a specific promotional offer.

## Data Preprocessing and Preparation
In this phase, we performed extensive data cleaning and normalization to ensure accurate and meaningful model training and evaluation. The steps included:
- Handling NA values across multiple features with tailored imputation strategies.
- Balancing the dataset using Synthetic Minority Over-sampling Technique (SMOTE).
- Normalizing data to bring all variables to a similar scale, crucial for models like Neural Networks and to ensure fair comparison across different models.

## Model Training and Evaluation
Several models were trained and evaluated to determine their effectiveness in predicting user conversion. The models include:
- Logistic Regression (with delta variables and full set of variables)
- XGBoost (with delta variables and full set of variables)
- Decision Tree (with delta variables and full set of variables)
- Neural Network (with delta variables and full set of variables)

## Results Analysis and Model Selection
When evaluating these models, I only looked at performance based on the top 1000 probabilities that the model chose in order to better
understand the model's impact on the actual promotion. the task is to determine which model is most suited for identifying the top 1,000 free
users who are most likely to convert to premium if targeted with the promotion. The goal is to maximize the number of actual conversions out
of these 1000 offers.

In model selection, I focused mainly on 4 factors:

  - True Positives: Since the models were evaluated and measured only on the top 1000 users they selected from the validation set, the
number of true positives was the most important metric during this process. The number of true positives gives us the clearest estimate
of how many conversions we can expect to get with the 1000 user promotion.

  - Recall:Out of all the actual positives, how many did the model predict? This was the second most important metric in this process
because it represents the ability of the model to capture all potential converters. We do not want to miss any would-be converters if
they had received the promotion.

  - Precision:Out of all the positive predictions the model made, how many were actually correct. This is a valuable metric in this context
because we want to ensure that those we target are indeed likely to convert and we are not wasting promotions. Since we only
measured on the top 1000 predictions from the validation set, the precision is just a percentage of the number of true positives out of 1000.
    
  - F1: This is the harmonic mean of precision and recall and gives us an overall measure of model performance.

Given the prioritized metrics, the chosen model for this task is the **XG Boost** model, that was trained on the entire data set. The `XG Boost - Full` model predicted the highest number of true positives out of 1000 (298), and produced the highest ***Precision (0.298)***, ***Recall (0.221)***, and ***F1 (0.254)*** scores. This model gives us the best chance of producing the highest number of conversions. In addition, the XG Boost model is able to handle non-linear relationships and is less prone to over fitting which provides more confidence when deploying it in the real world scenario.

*The performance of models with full variables (both previous and current period) consistently outperforms models built only on the delta (previous period's change) variables. This indicates that current period data provides significant and valuable predictive power. Therefore, XG Boost trained on the full data set was selected over the XG Boost model trained only on delta variables.*

---

## Visualizations from the Project

**Decision Tree Structure**: Visual representation of the decision-making process in the tree model.

<img src="unnamed-chunk-38-1" width="600">

---

**Neural Network Structure**: A diagram showcasing the architecture of the neural network model.

<img src="NN-Train-1.png" width="600">

---

**Model Performance Table**: A detailed table displaying the TP, FN, FP, and TN for each model.

<img src="Table_Model_performance.png" width="600">

---

**Model Evaluation Metrics Table**: A comprehensive table showing the Accuracy, Precision, Recall, and F1 Scores for each model.

<img src="Table_Accuracy_Recall.png" width="600">

---

**Performance Metrics Bar Chart**: A bar chart comparing the Accuracy, Precision, Recall, and F1 Scores across all models.

<img src="Plot-Accuracies-1.png" width="600">

---


## Strategies to estimate ROI of the model

**Comparing XGB Model to Randomized Promotion**
Generating a scenario where 1000 people have been randomly chosen to receive the proportion (this is
assumed to be the alternative procedure if predictive models were not being used)

<img src="Table-XGB-Random-1" width="600">

<img src="Table-XGB-Random-2" width="600">

Here, we can see that the use of predictive modeling to select the top 1000 users would result in 221 additional premium users than if you had
randomly targeted.This generates almost 4x additional revenue, and is more cost-efficient by not wasting the two-month trial on as many
people that won’t ultimately sign up for premium.

**Estimating Cost-Benefit Matrix**

<img src="Profit comparison of XGB vs Random" width="600">

---

## Strategy for Deployment

**1. A/B Testing**
Before fully deploying the promotion to a wider audience, they should first run a controlled A/B test. Offer the promotion to a subset of users selected by the XG Boost model (treatment group) and to another subset of users that have been randomly selected (control group). The hypothesis is that the results of the treatment group would result in *size of subset* × 0.298 conversions which we derive from the XGB Model performance on validation set, and the results of the control group would be *size of subset* × 0.077 conversions (derived from the generated randomized scenario above). We can then compare the conversion rates to validate the model's effectiveness. 


**2. Deployment**
If the null hypothesis (H0: predictive conversions = randomized conversions) from our A/B test can be rejected, then we can deploy the promotion and target 1000 customers with the offer.

**3. Evaluate Real Findings and Impove Model**
After deployment, we can monitor the results and continue to tune and adjust the model. We can also try to improve our data collection process so that we have cleaner data to train our models with and there is less of a dependence on imputation. Additionally, we can attempt to collect new data (i.e. web search data, clicked on premium links before, better location data, etc.) which may improve our model. Furthermore, we may adjust our decision logic afer the initial offer. For example, we may want to avoid targeting people that have such a high probability of converting to premium that they would convert no matter what - avoiding these people would save use the cost of the two-month promotion (during which they may have otherwise been a paying premium user). Changes in decision logic can also be implemented into the updated model.


**4. Deploy on a Larger Set**
We can then run another A/B test with the improved model (trained on more thorough data with additional features) vs original model, and then re-deploy the promotion on an even larger group.

---

## Implementation and Conclusion
Using the selected XGBoost model, we identified the top 1,000 users most likely to convert to premium subscriptions. This targeted approach is expected to maximize actual conversions from the promotional offer, demonstrating the value of a data-driven strategy in enhancing subscription conversions.

---

*For detailed analysis, methodologies, and the source code, please refer to the individual sections in this repository.*
