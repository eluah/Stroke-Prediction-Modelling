## 1. Introduction
Some of the common warning signs before a stroke happen are sudden numbness on one side of the body, sudden confusion and difficulty in speech (Centers for Disease Control and Prevention, 2020). Most of the time, other health-related issues would be present before having a stroke. Therefore, it is important to understand the factors behind a stroke so that one can have appropriate and timely treatment to prevent it from happening.

In this project, a dataset containing medical information about patients is used, including their medical history, age, gender and stroke history. 

Based on this dataset, I aim to verify common knowledge about stroke.
1.	Does gender, age and BMI affect the chances of getting a stroke?
2.	Do medical conditions (hypertension & heart disease) affect the chances of getting a stroke?
3.	Is there any variable that is most significant in predicting a stroke?

This report will cover the data descriptions and analysis using R programming. For research objectives, statistical analysis will be performed and we aim draw conclusions in the most appropriate approach, together with explanations and elaborations.

## 2. Data Description
The dataset, titled “Stroke Prediction Dataset”, is obtained from the online data science community Kaggle and open to the public. The original data is named “healthcare-dataset-stroke-data.csv” and sourced from https://www.kaggle.com/fedesoriano/stroke-prediction-dataset.

Before proceeding with data analysis, first a preliminary data cleaning was performed to ensure that:
-	Irrelevant columns were removed, such as “id”, “ever_married”, “work_type” and “residence”
-	Any observations with NA records are removed
-	Data in the “gender” column is changed to 1 for male and 2 for female

After the preliminary cleaning, 3425 observations (patients) with 8 variables for analysis are left:
1.	gender: 1 (Male) or 2 (Female)
2.	age: age of the patient
3.	hypertension: 0 (no hypertension) or 1 (has hypertension)
4.	heart_disease: 0 (no heart disease) or 1 (has heart disease)
5.	avg_glucose_level: average glucose level in blood
6.	bmi: body mass index
7.	smoking_status: 0 (non smoker), 1(former smoker), or 2 (current smoker)
8.	stroke: 0 (no stroke history) or 1 (has stroke history)

## 3. Description and Cleaning of Dataset
In this section, We investigate each variable in detail to find possible outliers and possibly apply data transformations to fix skewed data. 

### 3.1 Summary statistics for the main variable of interest, stroke
We observe that the variable stroke is highly skewed towards patients with no stroke history. There are 3245 observations of no stroke history and 180 observations of stroke history. This data bias will be addressed later in the report through oversampling.

### 3.2 Summary statistics for gender
There are 1339 observations of males and 2086 observations of females. The difference in observations is not huge so we will not perform any data manipulations here.

### 3.3 Summary statistics for age

![age_hist](./img/age_hist.png)

The histogram seems to follow a normal distribution so no transformation is needed.

### 3.4 Summary statistics for hypertension
There are 3017 observations of no hypertension history and 408 observations of hypertension history. 

### 3.5 Summary statistics for heart_disease
There are 3219 observations without heart disease history and 206 observations with heart disease history.

### 3.6 Summary statistics for avg_glucose_level

![glu1](./img/glu1.png)
![glu2](./img/glu2.png)

The histogram seems to vary from a normal distribution, so we perform the Shapiro-Wilk test to check for normality. 
At level of significance 0.05, we reject the null hypothesis and conclude that the data does not follow normal distribution. Hence, we will perform a log transformation.

![glu3](./img/glu3.png)
![glu4](./img/glu4.png)

After taking the log transformation, the histogram seems to approach a normal distribution. Although the boxplot still shows quite a few outliers, high glucose levels are common in the early phases of stroke (Lindsberg & Roine, 2004) and since we are using it as a predictor for stroke, we will leave the outliers in. 

### 3.7 Summary statistics for bmi

![bmi1](./img/bmi1.png)
![bmi2](./img/bmi2.png)

The histogram is right-skewed hence we apply a (log) transformation to deal with skewed data. However the boxplot shows 3 extreme outliers which we remove for bmi>65. After removing the 3 extreme outliers, we check for normality using the Shapiro-Wilk test.
At level of significance 0.05, we reject the null hypothesis and conclude that the data does not follow normal distribution. Hence, we perform a log transformation.

![bmi3](./img/bmi3.png)
![bmi4](./img/bmi4.png)

After performing log transformation, the data now follows a normal distribution and is not skewed. The boxplot has quite a few outliers but since patients with extreme BMI values could lead to having a higher chance of stroke, we leave them in the dataset.

### 3.8 Summary statistics for smoking_status
There are 1850 observations of non-smokers, 836 observations of former smokers and 736 observations of current smokers. We have a roughly even number of observations for non-smokers and smokers.

### 3.9 Final dataset
Based on the above analysis, the dataset is reduced to 3422 observations.

## 4. Statistical Analysis
### 4.1 Visualize the correlation between the response variable, stroke and each predictor variables

![Correlation Matrix](./img/corrplot.png)

The correlation matrix highlights how correlated the variables are in the dataset. From the corrplot, one can see that stroke is positively correlated to age, hypertension, heart_disease and avg_glucose_level. 
We will perform some statistical tests to further verify our observations which is illustrated in the next section.

### 4.2 Statistical Tests
#### 4.2.1 Relation between stroke and age

![Age Box Plot](./img/age_box.png)

From the boxplot above, we observe that the median age for stroke patients is much higher than non-stroke patients. We proceed to test if the mean age for both stroke and non-stroke groups are equal. 

![Age Box Plot](./img/age_test.png)

The ANOVA test returns an extremely small p-value which is smaller than the significance level of 0.05. Thus we reject the null hypothesis and conclude that the mean age for stroke patients and non-stroke patients are not equal. We then conclude that the older you get, the higher the chance of getting a stroke.

#### 4.2.2 Relation between stroke and hypertension
First we create a table with the summary counts then perform a chi-square test to test for association between stroke and hypertension.
H0: There is no association between stroke and hypertension
H1: Some association exists between stroke and hypertension

![Hypertension Test](./img/hypertension_test.png)

The chi-square test returns an extremely small p-value which is smaller than the significance level of 0.05. Thus we reject the null hypothesis and conclude that some association exists between stroke and hypertension.

#### 4.2.3 Relation between stroke and heart_disease
Frist we create a table with the summary counts then perform a chi-square test to test for association between stroke and heart_disease.
H0: There is no association between stroke and heart_disease
H1: Some association exists between stroke and heart_disease

![Heart Test](./img/heart_test.png)

The chi-square test returns an extremely small p-value which is smaller than the significance level of 0.05. Thus we reject the null hypothesis and conclude that some association exists between stroke and heart_disease.

#### 4.2.4 Relation between stroke and avg_glucose_level

![Glucose Box](./img/glucose_box.png)

From the boxplot above, we observe that the median average glucose level for stroke patients is higher than non-stroke patients. We proceed to test if the mean average glucose level for both stroke and non-stroke groups are equal. 

![Glucose Test](./img/glucose_test.png)

The ANOVA test returns an extremely small p-value which is smaller than the significance level of 0.05. Thus we reject the null hypothesis and conclude that the mean average glucose level for stroke patients and non-stroke patients are not equal. We can then infer that having higher average glucose level increases the odds of getting a stroke.

#### 4.2.5 Relationship between hypertension and heart_disease
First we create a table with the summary counts then perform a chi-square test to test for association between stroke and heart_disease.
H0: There is no association between hypertension and heart_disease
H1: Some association exists between hypertension and heart_disease

![Hypertension-Heart Test](./img/hypertension_heart_test.png)

The chi-square test returns an extremely small p-value which is smaller than the significance level of 0.05. Thus we reject the null hypothesis and conclude that some association exists between hypertension and heart_disease.

### 4.3 Logistic Regression
First we perform oversampling to balance out our dataset and have the equal number of observations for stroke and non-stroke patients. Next split the data into training and test sets. Then we perform logistic regression using age, hypertension, heart disease and average glucose level as predictor variables to develop a model to predict stroke. 

![logreg1](./img/logreg1.png)

From the R output above, we can observe that all 4 predictor variables are significant as they have p-values below the significance level of 0.05.

![logreg2](./img/logreg2.png)

#### 4.3.1 Prediction
In order to test how accurate the model is, we perform predictions and compare the results with the test set data. A threshold value of 0.5 is used for our predictions and to generate a confusion matrix. A confusion matrix is a table used to describe the performance of a classification model on a set of test data for which the true outcomes are known. It compares the actual outcomes in the test data set to the predicted outcomes. 

![cmatrix](./img/cmatrix.png)

Our model is 76.01% accurate at predicting stroke using age, hypertension, heart disease and average glucose level as predictor variables. Thus we conclude that these variables are significant for predicting stroke in humans.

### 5. Conclusion
Stroke is one of the main causes of death. In order to prevent a stroke from happening, it is important to understand the chances of having a stroke based on the health issues that a person has. In this report, we attempted to verify common knowledge about stroke factors. 

From the analysis we conclude that:

-	Stroke is positively correlated to age, hypertension, heart disease, average glucose level
-	Age is highly correlated to hypertension and heart disease
-	Age is the most significant factor influencing the chances of getting a stroke
-	Hypertension and heart disease are linked to each other

In addition, gender and smoking status did not seem to affect the chances of getting a stroke significantly which is interesting to note. It should be mentioned that since oversampling used to balance the dataset, it might lead to overfitting. However, since the accuracy of the model is relatively high, it should not be a significant issue.

### 6. References
Better Health Channel. (2013, July 31). Stroke risk factors and prevention.
https://www.betterhealth.vic.gov.au/health/ConditionsAndTreatments/stroke-risk-f
actors-and-prevention

Centers for Disease Control and Prevention. (2020, August 28). Stroke Signs and
Symptoms. https://www.cdc.gov/stroke/signs_symptoms.htm

Lindsberg, P. J., & Roine, R. O. (2004). Hyperglycemia in Acute Stroke. Stroke, Vol
35(No. 2), 363–364.
https://www.ahajournals.org/doi/full/10.1161/01.str.0000115297.92132.84

World Health Organization. (2020, December 9). The top 10 causes of death.
https://www.who.int/news-room/fact-sheets/detail/the-top-10-causes-of-death
