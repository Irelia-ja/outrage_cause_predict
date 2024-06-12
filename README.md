# Prediction of the Cause of Major Outage
## Framing a Prediction Problem
In this project, we will make a model to predict the category of an outage. The predictive model will help the utility company to save resources and time on fixing outages by determining the causes of them. The model will be a multiclass classification that predicts the column “CAUSE.CATEGORY.” 

At the time of our prediction, we will have the information about the climate category of the region, total sales, total price, the relative GDP between the state and the country (PC.REALGSP.REL), the total customers, the month the outage happened, and the state utility sectors’ income over the total earnings of the country utility sectors’ income (PI.UTIL.OFUSA). 

We will fit our model by using random forest classification because it is the best fit for classification prediction and generally provides a better fit than decision trees. We will use the f1 score to analyze our model’s performance because we weigh recall and precision equally. 
## 
