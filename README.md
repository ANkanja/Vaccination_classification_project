# Vaccination_classification_project

## Business Understanding
#### Problem Statement
In the wake of the 2009 H1N1 influenza pandemic, public health officials faced challenges in achieving widespread vaccination coverage. Understanding the factors that influence individuals' decisions to get vaccinated is crucial for developing effective public health strategies, especially for future pandemic scenarios. This project aims to identify and analyze the demographic, behavioral, and attitudinal factors that predict whether individuals received the H1N1 vaccination during the 2009 pandemic.

#### Stakeholder
The primary stakeholder for this analysis is the Department of Health and Human Services (HHS), specifically their immunization campaign strategists. The insights gained from this analysis will help them:

1. Design more targeted vaccination campaigns
2. Allocate resources more efficiently
3. Address specific barriers to vaccination in different population segments
4. Improve overall vaccination rates in future public health emergencies

#### Business Objective
Develop a predictive model that can accurately identify individuals who are likely or unlikely to get vaccinated based on their demographic information, behaviors, and attitudes. 
##### For this project, we'll focus on predicting the H1N1 vaccine
This will enable public health officials to:

1. Identify high-risk populations with low vaccination likelihood
2. Tailor communication strategies to address specific concerns
3. Implement targeted interventions to increase vaccination rates
4. Optimize resource allocation for vaccination campaigns

#### Success Metrics

1. Model accuracy in predicting vaccination status
2. Identification of key factors influencing vaccination decisions
3. Actionable insights for public health campaign design
4. Ability to segment population by vaccination likelihood


### Data Understanding
Dataset Overview
The data comes from the National 2009 H1N1 Flu Survey (NHFS), which was conducted by the Centers for Disease Control and Prevention (CDC) to monitor vaccination rates during the H1N1 pandemic. The dataset contains information about respondents' demographics, opinions about vaccines, and health behaviors.


#### Key aspects of the dataset include:
- Target Variable: h1n1_vaccine (binary: 0 = No, 1 = Yes)
- Demographic Features: `age_group`, `education`, `race`, `sex`, `income_poverty`, `marital_status`, `rent_or_own`, `employment_status`.
- Attitude and Opinion Features: Beliefs about vaccine effectiveness and safety for both H1N1 and seasonal flu, perceived risk of infection.
- Behavioral and Health-Related Features: Doctor recommendations for vaccination, presence of chronic medical conditions, whether the respondent is a healthcare worker, has children under 6 months, and has health insurance.

Initial data exploration revealed:

* **Missing Values:** Several columns have missing data, with the percentage of missingness varying across features (e.g., `income_poverty` has a significant percentage of missing values).
* **Target Imbalance:** The distribution of the target variable `h1n1_vaccine` shows that approximately **21.26%** of respondents received the H1N1 vaccine, indicating an imbalanced classification problem.
* **Correlations:** Initial correlation analysis suggests that factors like doctor's recommendation for H1N1 vaccine, opinions on vaccine effectiveness and risk, and being a healthcare worker have a positive correlation with H1N1 vaccination.

## Modeling

The modeling process involved the following steps:

1.  Data Preparation:
    - Separated features (X) and the target variable (y).
    - Identified categorical and numerical features.
    - Split the data into training (80%) and testing (20%) sets using stratified sampling to maintain the proportion of the target variable.
    - Created preprocessing pipelines using `ColumnTransformer` to handle different feature types:
    - Categorical Features: Imputed missing values with the most frequent value and then applied one-hot encoding.
    - Numerical Features: Imputed missing values with the median and then applied standard scaling.

2.  Baseline Model (Logistic Regression):
    * Created a pipeline combining the preprocessor and a Logistic Regression model.
    * Trained the baseline model on the training data.
    * Evaluated the model's performance on both the training and testing sets using metrics like accuracy, ROC-AUC, precision, recall, and F1-score.
    * Identified the top 15 most important features based on the absolute values of the Logistic Regression coefficients.

3.  Tuned Logistic Regression:
    * Defined a hyperparameter grid for Logistic Regression, including different values for the regularization strength (C) and penalty (l1, l2), and the 'liblinear' solver.
    * Used `GridSearchCV` with 5-fold cross-validation and the ROC-AUC score as the evaluation metric to find the best combination of hyperparameters.
    * Trained the best Logistic Regression model with the identified optimal hyperparameters.
    * Evaluated the performance of the tuned model on the training and testing sets.

4.  Non-Parametric Model (Decision Tree):
    * Created a pipeline with the preprocessor and a Decision Tree classifier.
    * Trained and evaluated the baseline Decision Tree model.

5.  Tuned Decision Tree:
    * Defined a hyperparameter grid for the Decision Tree, including `max_depth`, `min_samples_split`, `min_samples_leaf`, and `criterion`.
    * Used `GridSearchCV` to find the best hyperparameters using ROC-AUC as the scoring metric.
    * Trained and evaluated the tuned Decision Tree model.

6.  Ensemble Model (Random Forest):
    * Created a pipeline with the preprocessor and a Random Forest classifier.
    * Trained and evaluated the Random Forest model.
    * Identified the top 15 most important features based on the Gini importance scores from the Random Forest model.

## Evaluation

The performance of each model was evaluated using the following metrics on both the training and testing datasets:

* Accuracy: The proportion of correctly classified instances.
* ROC-AUC: The Area Under the Receiver Operating Characteristic curve, indicating the model's ability to distinguish between the two classes.
* Precision: The proportion of correctly predicted positive instances out of all instances predicted as positive.
* Recall: The proportion of correctly predicted positive instances out of all actual positive instances.
* F1-Score: The harmonic mean of precision and recall.

A summary of the model performance on the testing data is as follows:

| Model                       | Test Accuracy | Test ROC-AUC | Test Precision | Test Recall | Test F1 |
| :-------------------------- | :------------ | :----------- | :------------- | :---------- | :------ |
| Baseline Logistic Regression | 0.8371        | 0.8364       | 0.6667         | 0.3993      | 0.4989  |
| Tuned Logistic Regression   | 0.8375        | 0.8366       | 0.6704         | 0.3993      | 0.5007  |
| Decision Tree               | 0.7747        | 0.6949       | 0.4455         | 0.4571      | 0.4512  |
| Tuned Decision Tree         | 0.8149        | 0.7857       | 0.5769         | 0.4228      | 0.4902  |
| Random Forest               | 0.8278        | 0.8136       | 0.6216         | 0.4128      | 0.4961  |

### Conclusions and Business Recommendations
#### Key Findings
Our analysis of the National 2009 H1N1 Flu Survey (NHFS) data yielded several important insights about factors influencing H1N1 vaccination decisions:

1. Model Performance: Our best-performing model is the Tuned Logistic Regression model delivering the highest test accuracy (83.75%) and F1 score (0.537), while showing virtually no overfitting (-0.007%)

2. Key Predictors of Vaccination:
    - Doctor's recommendation appears to be the strongest predictor of vaccination status
    - Positive opinions about vaccine effectiveness strongly correlate with vaccination
    - Healthcare workers and those with chronic medical conditions were more likely to be vaccinated
    - Higher education and income levels are associated with higher vaccination rates
    - Geographic location plays a role in vaccination likelihood


3. Demographic Insights:

- Age groups show different vaccination patterns, with older individuals are more likely to be vaccinated
- Education level has a strong positive correlation with vaccination rates
- Income level correlates with vaccination likelihood


4. Attitude Factors:

- Concerns about vaccine side effects significantly decrease vaccination likelihood
- Perceived risk of H1N1 infection correlates with higher vaccination rates
- Those who believe the vaccine is effective are much more likely to get vaccinated
