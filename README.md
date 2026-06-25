# Bank Marketing: Classifier Comparison

An analytical evaluation of machine learning classifiers to optimize telemarketing campaign targeting for a Portuguese banking institution. 

## Project Links
* **Interactive Notebook:** [Link to Jupyter Notebook](https://github.com/KyungHee1251/BankMarketingClassifierComparison/blob/main/prompt_III.ipynb)

---

## Executive Summary

### The Core Challenge: The Accuracy Trap
The campaign dataset is heavily lopsided, with roughly 89% "no" answers and only 11% "yes" responses. In this scenario, standard accuracy is a highly misleading metric. A model could simply guess "no" for every single customer, score an impressive 88.7% accuracy, and completely fail the business. 

For a telemarketing team, the priority is finding the customers who will actually say "yes" (True Positives). Missing a willing subscriber means direct lost revenue. On the other hand, calling someone who says "no" only costs a few minutes of a representative's time. Because the penalty for missing a customer is so high, we must prioritize Recall (the total catch rate) over raw accuracy.

### Key Model Insights & Optimization Takeaways
Using targeted class balancing forced the models to prioritize the small subscriber group, breaking them out of their blind spot:

* **KNN failed entirely:** It scaled its neighbor count up to 21 just to protect its overall accuracy score. It missed almost every single subscriber and finished with the lowest overall score (0.6089 ROC AUC).
* **Class balancing corrected model behavior:** Enforcing balanced class weights forced Logistic Regression, Decision Trees, and SVC to prioritize the heavily outnumbered subscriber group. While this dropped global accuracy to the 58%–61% range, it successfully broke the models out of their blind spot so they could flag actual buyers.
* **Logistic Regression gave a clean baseline:** It delivered a top-tier score (0.6226 ROC AUC) with virtually no gap between training and testing performance, running near-instantly in just 0.33 seconds.
* **The Decision Tree was the top practical choice:** It caught **529 true subscribers**, beating Logistic Regression's 511. Capping the tree at `max_depth: 6` successfully controlled variance and kept overfitting to a minimum. Even though the absolute highest catch count was observed in SVC (539), its massive 700x training time slowdown (231.56 seconds) heavily outweighs that slight edge.

### Interpretation of Model Coefficients
To understand what truly drives a customer to subscribe to a term deposit, we analyzed the weights of the optimized Logistic Regression model:
* **Positive Multipliers:** Features with positive coefficients directly increase the model's log-odds calculation. As these traits increase or switch to true (such as specific receptive client occupations or higher education levels), they shift predictions toward a term deposit subscription ("yes").
* **Negative Constraints:** Features stretching in the negative direction generate lower predictive weights. These attributes act as structural barriers to conversions, indicating client segments that significantly minimize probability scores within the pipeline.
* **Feature Dominance:** The absolute distance of any feature coefficient from the baseline represents its statistical predictive power, providing a clear ranking of which specific traits hold the greatest mathematical influence over target audience behavior.

### Strategic Recommendation & Next Steps
The **Decision Tree** is the best model for immediate deployment because it maximizes customer acquisition without a massive time penalty. However, any model built purely on basic client demographics will eventually hit a performance ceiling. 

To significantly lift conversion rates, future modeling phases should expand beyond basic demographic profiles to fully incorporate the remaining behavioral and economic features already present in the dataset:

1. **Past Campaign Data:** Utilizing features like `pdays` (days since last contact) and `previous` (number of past interactions) will capture a customer's immediate interest or momentum.
2. **Economic Indicators:** Factoring in the existing social and economic attributes—such as interest rates and consumer price indices—will help the model adapt to real-world economic shifts, keeping marketing lists highly accurate over time.

---

## Repository Structure
* `prompt_III.ipynb`: Main Jupyter Notebook containing data exploration, preprocessing, scaling, hyperparameter grid searches, classifier comparisons, and coefficient interpretations.
* `data/`: Directory containing the bank marketing dataset files.

---

## How to Use
1. Clone this repository:

```
git clone https://github.com/KyungHee1251/BankMarketingClassifierComparison.git
```

2. Unzip ```data/bank-additional-full.csv.zip``` so that ```bank-additional-full.csv``` is present in the ```data/``` directory.
3. Open the notebook:

```
jupyter notebook prompt_III.ipynb
```

**Note** Problem 11 executiom will take long, approximately 4 min. 
