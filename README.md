# Prediction of the Cause of Major Outage
DSC80 Final Project in UCSD

Conducted by Shangqi Liu and Zihan Liu
## Introduction ##
### Project Background ###
In recent years, with the rapid socio-economic development and accelerated urbanization, the demand for electricity has been growing. However, frequent and irregular power outages not only cause great inconvenience to residents' living and production activities, but also have a significant socio-economic impact. The causes of power outages are varied and may include natural disasters (e.g., storms, earthquakes), equipment failures, human factors, and overloading of the power system. In order to better understand and respond to these outages, the study of past outage data and causes of outages is particularly important.

### Project Significance ###
The aim of this project is **to explore the main causes of outage and find a reasonable way to predict them through in-depth analysis of outage data from various states in the U.S..** At the micro level, this will help electric utilities improve their emergency response to outages. By optimizing the maintenance and management of the power system, this improves the reliability of the power system. At the macro level, this can provide a scientific basis for policy makers to effectively reduce the frequency and duration of blackout events caused by human factors. Ultimately, this will help promote the construction of smart grids and save socio-economic costs.

### Introduction to the dataset ###
In this project, we use a dataset from the Laboratory for Advancing Sustainable Critical Infrastructure at Purdue University (https://engineering.purdue.edu/LASCI/research- data/outages) This dataset includes the major outages witnessed by different states in the continental U.S. Besides major outages, this data contains information on geographical location of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages. 

The dataset contains a total of 1,534 outage data from 2000 to 2016 and 56 columns of related data information. The following are the columns that are the focus of this study:

| Columns                 |   Description |
|:------------------------|:--------------|
| CLIMATE.CATEGORY        |Categorizes climate conditions as "Warm," "Cold," or "Normal" based on deviations from normal temperatures.|
| TOTAL.PRICE             |Represents average monthly electricity prices in cents per kilowatt-hour across various sectors.|
| PC.REALGSP.REL          |Indicates the per capita real gross state product relative to the overall U.S. per capita real GDP|
| PI.UTIL.OFUSA           |Reflects the state's utility sector income as a percentage of the total U.S. utility sector income.|
| MONTH                   |Specifies the month when the data or event occurred.|
| TOTAL.CUSTOMERS         |             Total number of customers served across all sectors in the state.|
| TOTAL.SALES             |             Total electricity sales in megawatt-hours (MWh) across all sectors in the state.|
| CAUSE.CATEGORY          |             Broad categories describing the causes of major power outages.|
| CAUSE.CATEGORY.DETAIL   |             Detailed descriptions providing specific information about outage causes.|
| OUTAGE.DURATION         |             Duration of power outages in minutes.|
| CUSTOMERS.AFFECTED      |             Number of customers affected by power outages.|
| U.S._STATE              |             Name of the U.S. state where the outage occurred.|
| CLIMATE.REGION          |             Geographic region within the U.S. characterized by similar climate conditions.|
| POPULATION              |             Total population residing in the U.S. state.|
| PC.REALGSP.STATE        |             Per capita real gross state product, indicating economic output per person in the state.|
| OUTAGE.START.DATE       |             Date when the outage event began.|
| OUTAGE.START.TIME       |             Time of day when the outage event started.|
| OUTAGE.RESTORATION.DATE |             Date when power was fully restored to all affected customers.|
| OUTAGE.RESTORATION.TIME |             Time of day when power was fully restored.|


## Data Cleaning and Exploratory Data Analysis ##
### Data Cleaning ###
1. In this project, we extract the columns that we care about, which are **"CLIMATE.CATEGORY", "TOTAL.PRICE", "PC.REALGSP.REL", "PI.UTIL.OFUSA", "MONTH", "TOTAL.CUSTOMERS", "TOTAL.SALES", "CAUSE.CATEGORY", "CAUSE.CATEGORY.DETAIL", "OUTAGE.DURATION", "CUSTOMERS.AFFECTED", "U.S._STATE", "CLIMATE.REGION", "POPULATION", "PC.REALGSP.STATE", "OUTAGE.START.DATE", "OUTAGE.START.TIME", "OUTAGE.RESTORATION.DATE", "OUTAGE.RESTORATION.TIME"**.

2. We combine the **OUTAGE.START.DATE** and **OUTAGE.START.TIME** columns into one Timestamp object in an **OUTAGE.START** column. Also, we combine the **OUTAGE.RESTORATION.DATE** and **OUTAGE.RESTORATION.TIME** into **OUTAGE.RESTORATION**. At last, we dropped the old columns.

3. We divided **TOTAL.SALES** by **TOTAL.CUSTOMERS** into one column **a_tsalses**, which is the per capita consumption of electricity.

4. The **OUTAGE.DURATION** and **CUSTOMERS.AFFECTED** for values of 0 have no practical significance. Thus, we replace 0 values in these columns with np.nan.

| CAUSE.CATEGORY | CLIMATE.CATEGORY | a_tsalses | OUTAGE.START | OUTAGE.RESTORATION | 
|:-------------------|:-------------------|------------:|:--------------------|:---------------------| 
| severe weather | normal | 2.52823 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 | 
| intentional attack | normal | 2.00104 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 | 
| severe weather | cold | 2.01867 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 | 
| severe weather | normal | 2.21998 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 | 
| severe weather | warm | 2.23313 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |

### Univariate Analysis ###

<iframe
  src="plt/fig1.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

The Figure 1 shows the relationship between the average power outage duration and the month. The average power outage duration from April to August is short, and the average power outage duration in September is the longest.

<iframe
  src="plt/fig2.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

The Figure 2 shows the number of power outages in different months. There are more power outages from June to August, and fewer power outages from September to December. We find that this is the opposite of the results shown above.

<iframe
  src="plt/fig3.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

The Figure 3 shows the number of power outages in different states. It's clear that California had the most power outages, more than 200. It was followed by Texas, Washington, Michigan and New York. We can find that the larger the city, the more power outages.

<iframe
  src="plt/fig4.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

The Figure 4 shows the number of power outages corresponding to different climate regions. The northeast had the most power outages and the northwest the least.

### Bivariate Analysis ###
We need to look not only at the relationship between power outages and climate data, but also at the relationship between power outages and socio-economic data.

<iframe
  src="plt/fig5.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

The Figure 5 shows the length of the outage in relation to the state population. We found that there was no significant difference in the duration of power outages for different populations.

<iframe
  src="plt/fig6.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

The Figure 6 shows the relationship between outage duration and real gross state product per capita. We found that there was no significant difference in outage duration for different GSP.

This can simply show that whether the power outage occurs has little to do with the socio-economic situation.



### Interesting Aggregates ###
This pivot table includes columns such as **CUSTOMERS.AFFECTED, CLIMATE.CATEGORY, and CAUSE.CATEGORY**. It summarizes the average number of affected customers (CUSTOMERS.AFFECTED) across different categories of climate (CLIMATE.CATEGORY) and causes of outages (CAUSE.CATEGORY). Each cell in the table represents this average value, rounded to two decimal places, providing a structured overview for analysis and comparison of outage impacts based on climate and cause categories.

Understanding how different climate conditions influence outage causes and subsequently affect customer numbers is vital for resilience planning and disaster response strategies. It enables us to identify patterns and correlations that may exist between climate types and specific outage causes, thereby informing more accurate predictive models for outage causes. This knowledge can lead to improved preparedness measures, more targeted mitigation strategies, and better allocation of resources during outage events.

| CLIMATE.CATEGORY   |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| cold               |             90160.4 |                     0   |                76.27 |     6615.29 |         7016.67 |           165690 |                        214297   |
| normal             |            126292   |                     0.5 |              1538.43 |     9255.83 |         7859.6  |           193217 |                        250940   |
| warm               |             74708.3 |                     0   |              5256.98 |      758.12 |            0    |           211223 |                         86869.8 |



## Assessment of Missingness ##
### NMAR Analysis ###
**IND.PRICE** column in the dataset is NMAR ("Industrial Sector Monthly Electricity Prices (cents/kWh)").NMAR implies that the probability of a missing data point is related to the value of the missing data itself. In other words, the missingness depends on unobserved data and cannot be explained by other observed variables in the data set.

In the case of the index. price column, the missing values may be NMAR for several reasons.
1. Regulation and policy impacts: Changes in energy policy or regulation may affect reporting practices. For example, reporting may be avoided if prices are unfavorable during a regulatory review or policy change affecting energy prices.
2. Data reporting issues: In some cases, data may not be recorded due to errors or omissions related to pricing data. This is due to omissions on the part of the data collection company.

In order to convert an NMAR miss to a MAR,  we need to collect additional data, including:
1. Regulatory information: Records of any regulatory changes or policies affecting the energy sector during the missing period. This can help determine if there were specific events or policies that affected electricity price reporting.
2. Information related to the data collection company: First, operational logs and system status of the data collection company. The company's operating logs, employee work records, and relevant internal reports are collected. These records can reveal whether there were operational omissions or human errors when data were missing. Second, system status and maintenance records. Collect status records and maintenance records of the data collection system, including system crashes, server maintenance, software updates, and so on. These records can reveal whether technical problems existed at the time the data were missing.


### Missingness Dependency ###
To test missingness dependency, I will focus on the distribution of **CAUSE.CATEGORY.DETAIL**. I will test this against the columns **CAUSE.CATEGORY** and **OUTAGE.DURATION**.

#### CAUSE.CATEGORY ####
- Null Hypothesis: The distribution of Cause Category is the same when Cause Category detail is missing vs not missing.
- Alternate Hypothesis: The distribution of Cause Category is different when Cause Category detail is missing vs not missing.

<iframe
  src="plt/fig7.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

After the permutation test, we find that the observed TVD corresponds to a **p-value of 0.0**. Its distribution is shown in the figure. Since the p-value is less than 0.01, we reject the null hypothesis, that is, the distribution of Cause Category is significantly different when Cause Category detail is missing vs not, indicating that the missingness of Cause Category detail is dependent on Cause Category.

#### OUTAGE.DURATION ####
- Null Hypothesis: The distribution of outage.duration is the same when Cause Category detail is missing vs not missing.
- Alternate Hypothesis: The distribution of outage.duration is different when Cause Category detail is missing vs not missing.

<iframe
  src="plt/fig8.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

After the permutation test, we find that the observed TVD corresponds to a **p-value of 0.896**. Its distribution is shown in the figure. Since the p-value is greater than 0.1, We cannot reject the null hypothesis, that is, the distribution of outage.duration is the same when Cause Category detail is missing vs not, indicating that the missingness of  Cause Category detail is not dependent on outage.duration.

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

We will perform a one-hot encoder to the **CLIMATE.CATEGORY** column. Our basic model has a f1 score of **0.57**, which is kind of low. Therefore, we want to explode more into other data columns.  

### Final Model
Our final model will contain new data columns: **MONTH**, **PI.UTIL.OFUSA**, and **a_tsalses**. **MONTH** (quantitative) gives us information about the time the outage happened: there could be some period of the month when one particular cause of the outage may have happened. For example, severe weather often occurs around 11-12 months in Minnesota. The column **a_tsalses** (quantitative) represents the average consumption of electricity per customer, which implies the electrical burden on the equipment. Higher **a_tsalses** may cause the utility equipment to deteriorate more quickly than the low one, and may help us in predicting the cause of the outage. Lastly, **PI.UTIL.OFUSA** (quantitative), in short, tells us about how the workers in the utility company are treated. We think that if workers are being treated nicely (larger **PI.UTIL.OFUSA**), they will be more enthusiastic about their jobs and prevent some causes of the outage - for instance, the equipment failure. 
In addition, we will transform our two old data columns - **CLIMATE.CATEGORY** and **TOTAL.PRICE** - in our final model. Instead of using a one hot encoder, we transform the *warm* and *cold* value in the **CLIMATE.CATEGORY** to 1 and normal to 0. We think that warm and cold climates will result in the same severe weather, so we should treat them equally. Then, we apply standard scaler transformations to the column **TOTAL.PRICE**. 
We use GridSearchCV to help us determine the hyperparameter in the random forest classifier. The hyperparameters are:
- Criterion = Entropy
- Max_depth = 102
- Min_samples_split = 10

Our final model has a f1 score of **0.62**, which is better than our basic model. Therefore, we conclude that the final model performs better than our basic model

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





