# Prediction of the Cause of Major Outage

### Data Frame




## Hypothesis Testing
We will be performing a permutation test on the relationship between **CAUSE.CATEGORY** and **TOTAL.SALES**. Specifically, we will examine the difference between the mean total sales of two variables in the **CAUSE.CATEGORY** - severe weather and intentional attack. 
	- Null Hypothesis: The mean total sales between severe weather and intentional attack is roughly the same.
	- Alternative Hypothesis: The mean total sales of severe weather is greater than the one of intentional attack. 

We will use the difference of mean as our test statistics because we want to know which category has a higher mean total sales. 

We generate our test group with 1000 trials, and we get a p value of 0.0. Therefore, at a significance level of 1%, we reject the null hypothesis.

<iframe
  src="plt/hypo_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Framing a Prediction Problem
In this project, we will make a model to predict the category of an outage. The predictive model will help the utility company to save resources and time on fixing outages by determining the causes of them. The model will be a multiclass classification that predicts the column **CAUSE.CATEGORY**. 

At the time of our prediction, we will have the information about the climate category of the region, total sales, total price, the relative GDP between the state and the country (**PC.REALGSP.REL**), the total customers, the month the outage happened, and the state utility sectors’ income over the total earnings of the country utility sectors’ income (**PI.UTIL.OFUSA**). 

We will fit our model by using random forest classification because it is the best fit for classification prediction and generally provides a better fit than decision trees. We will use the f1 score to analyze our model’s performance because we weigh recall and precision equally. 

### Basic Model
For our basic model, we will be using columns **CLIMATE.CATEGOR** (nominal), **TOTAL.PRICE** (quantitative), and **PC.REALGSP.REL** (quantitative). 

We choose **CLIMATE.CATEGORY** because warm and cold climates may result in a more extreme climate that will cause an outage. **TOTAL.PRICE** and **PC.REALGSP.REL** together can help us determine the state's economy behind the outage: if **TOTAL.PRICE** is high but **PC.REALGSP.REL** is low, there may be more chance to have a public appeal as the cause of the outage. 

We will perform a one-hot encoder to the **CLIMATE.CATEGORY** column. Our basic model has a f1 score of **0.58**, which is kind of low. Therefore, we want to explode more into other data columns.  

### Final Model
Our final model will contain new data columns: **MONTH**, **PI.UTIL.OFUSA**, and **a_tsalses**. **MONTH** (quantitative) gives us information about the time the outage happened: there could be some period of the month when one particular cause of the outage may have happened. For example, severe weather often occurs around 11-12 months in Minnesota. The column **a_tsalses** (quantitative) represents the average consumption of electricity per customer, which implies the electrical burden on the equipment. Higher **a_tsalses** may cause the utility equipment to deteriorate more quickly than the low one, and may help us in predicting the cause of the outage. Lastly, **PI.UTIL.OFUSA** (quantitative), in short, tells us about how the workers in the utility company are treated. We think that if workers are being treated nicely (larger **PI.UTIL.OFUSA**), they will be more enthusiastic about their jobs and prevent some causes of the outage - for instance, the equipment failure. 
In addition, we will transform our two old data columns - **CLIMATE.CATEGORY** and **TOTAL.PRICE** - in our final model. Instead of using a one hot encoder, we transform the *warm* and *cold* value in the **CLIMATE.CATEGORY** to 1 and normal to 0. We think that warm and cold climates will result in the same severe weather, so we should treat them equally. Then, we apply standard scaler transformations to the column **TOTAL.PRICE**. 
We use GridSearchCV to help us determine the hyperparameter in the random forest classifier. The hyperparameters are:
    - Criterion = gini
    - Max_depth = 202
    - Min_samples_split = 10

Our final model has a f1 score of **0.61**, which is better than our basic model. Therefore, we conclude that the final model performs better than our basic model

### Fairness Analysis
We will perform a fairness analysis on the column **CUSTOMERS.AFFECTED**, and analyze if our model’s performance depends on the number of customers affected in an outage. We binarize the columns by setting numbers greater than the median of the columns equal to 1 and others to 0. 
	- Null Hypothesis: The model is fair. The f1 score between large and small number of customers affected in an outage is roughly the same. 
	- Alternative Hypothesis: The model is unfair. The f1 score of the large number of customers affected in an outage is significantly greater than the smaller one. 
We will use the difference in f1 score as our test statistic.

We perform 1000 permutations to create our test group, and we get a p-value of **0.0**. At a significance level of 1%, we reject our null hypothesis and conclude that the f1 score for a large number of customers affected in an outage is significantly greater than the low one. 

<iframe
  src="plt/fairness.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>



