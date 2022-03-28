# Loan Default Prediction
## 1. Overview
### 1.1 Objective Statement
The project objective is to build a model that predicts whether a borrower defaults on their loan or not. This is based on given characteristics and lifestyle of debtors and finding their relationship with defaulting. The end goal is to help lenders minimize their risk and losses.

### 1.2 Methodology
- Exploratory Data Analysis
- Statistical Analysis
- Classification
### 1.3 Business Benefit
- Reducing losses for the banks by identifying potential defaulters
- Reducing bias in the lending process by incorporating historical data to predict potential default rather than human judgement.
## 2. Business Understanding
- Business Problem: 
	- predict whether a loan applicant will default
	- help lenders minimize risks and losses.
- Importance and Effects of Loan Default:
	- Diminishes Asset Quality
	- Increases Costs of Funds
	- Decreases Profitability of the Bank
	- Decreases Overall Credit Rating of the Bank
	- Decreases the Capacity of Loan Sanctioning

## 3. Data Understanding
- Dataset: LendingClub 2013-2018. 
	- The full dataset provided consists of roughly 2 million records with 152 columns from 2007 - 2018
	- Due to software limitations, initially the data from 2016 - 2018 was extracted for model building
	- The target variable of loan_status was unbalanced resulting in low scores in the first phase of model building. This resulted in poor performance of the base models. To rectify this additional data was added.  We added 3 additional years worth of data. The dataset includes information from 2013-2018.
- Scope:		
	- Characteristics and lifestyle of debtors (example: emp_length, annual_inc, home_ownership)
	- Loan application details (example: int_rate)
	- finding their relationship with defaulting. 
- Data Dictionary:

## 4. Data Cleaning
- Missing/Null Values:
Most records with null values were removed. However, in columns referring to months since the last incident, a null value signified the borrower had no incidents. 
- Redundant Columns
Columns like funded_amnt_inv, funded_amnt and loan_amnt have nearly identical values.
Issue_d is not relevant to the purpose of this study. 
Id column is unique for each row.
The above columns were hence dropped.
- We plotted boxplots  and Tried to understand why outliers may be present for a particular column first. Over 50% of the data contained outliers indicating that the data contained important information. Hence, we only dropped very extreme outliers.


## 5. Data Preparation
- The home_ownership column had two categories ‘ANY’ & ‘NONE’. These values are replaced with  ‘OTHERS’.
- The FICO_Average column was created by taking the average of FICO_range_high & FICO_range_low.
	- last_fico_range_high: The upper boundary range the borrower’s last FICO pulled belongs to.
	- last_fico_range_low: The first 3 numbers of the zip code provided by the borrower in the loan application.
- All the states were now bucketed into different regions like  ‘west’, ‘south_west’, ‘south_east’, ‘mid_west’ & ‘north_east’.
- There were many categories in the purpose column , these were now bucketed into major categories like ‘debt’, ‘Major_purchase’, ‘life_event’ &  ‘Others ’.
- Lastly, year was extracted from the column ‘earliest_cr_line’.


## 6. Exploratory Data Analysis
- After plotting histograms we saw that Majority of the columns were skewed and had outliers. Since most of our columns were skewed we scaled the data by standardizing it
- We used a heatmap to identify correlations and we dropped columns which were highly correlated .
- One of the key insights that we derived from our analysis was that our target variable data was highly imbalanced as seen from the countplot on the left we will see how to deal with this problem when we get to model building. 
- We also observe a that longer someone has been an employee the likeliness of them defaulting reduces. From the countplot below We see that most of the loans in our dataset are debt consolidation loans followed by credit card loans.
- Another observation we made was that as we lower the loan grade our percentage of charged off accs was increasing. For Grade A,B and C- the Proportion of charged off is small whereas for Grade D,E,F and G - the Proportion of defaulters is high. Loan grade is a quality score assigned to a loan based on the borrower's credit history

- The plot at the bottom shows that defaults are higher for loans with longer duration 60 months
- Our dataset also contained geographical information which we tried to visualise by plotting the data on the US map
Idaho has the highest debt to income ratio 


## 7. Feature Engineering
-  Encoding of categorical variables: We have used different encoding techniques for different features. 
	-  We have used Ordinal encoding for ‘Grade’ and ‘Emp_length’ columns. We have split the ‘purpose’ column into three levels: ‘debt_consolidation’, ‘credit card’ and ‘other’ as the first two were high in count. We have used Target encoding for ‘addr_state’ as it had too many levels.
	-  For the remaining features we have used dummy encoding.
Our final dataframe has 54 features.
-  Scaling: We have used Standard Scaler to scale our numeric variables.
 
- Feature Creation
	- The creation of the feature, fico_avg, improved results from the base model significantly. This was created taking the average of 'last_fico_range_high' + 'last_fico_range_low'.
	- Regions were added as West, Southwest, Southeast, Midwest, Northeast based on states.
	- Purpose was streamlined into debt, major purchase, life event or other.


## 8. Model Building 
### 8.1 Base Models
- Models Used:
- All the models performed poorly with low F1 Scores and 
### 8.2 Model Optimization
- New Models Added:
	- XGBoost
	- AdaBoost
- Model Improvement Strategies:
	- Added two years of additional data
	- Added new features like FICO score
	- Retaining outliers
	- Different encoding techniques for region and purpose
	- Undersampling
	- RFE was used to find the significant features, which returned 22 significant features.
	- Hyperparameter tuning was done using GridSearchCV which gave more balanced results between test and train.

### 8.3 Model Evaluation
- The model performances have improved significantly 
- There is low variance between train and test score for all the models.
- Based on the metrics XGBoost after RFE and HyperParameter Tuning  performs the best on train as well as test set
### 8.4 Final Model
- Results:
	- Train accuracy and F1-Score: 0.94 and 0.94
	- Test accuracy and F1-Score: 0.90 and 0.90
	- This shows that there is slight overfitting. RFE and Hyperparameter tuning can be performed.
- Accuracy: 90%. Out of 100 loans, the model can predict 90% of results correctly. However, due to the nature of the prediction, it is not the most important metric.
- Precision: ~89%. Quality of positive prediction made by the model. Refers to the number of true positives divided by total positive predictions. A high precision value indicates the model out of all the applicants the model predicted to be a defaulter, 89% of them were correct.
- Recall: 92%. Target for optimization. Gives an indicator on the number of false negatives. Number of true positives by true positive + false negatives. False negatives could be costly, as they will cost the lender if there is default. 
- F1 Score: A combination of Precision and Recall.  The high F1 score of 0.89 indicates that the model is good overall , since F1 score will only be high if both precision and recall are sufficiently high.

## 9. Deployment (loan-default-prediction-app.herokuapp.com)
- Used Streamlit and Heroku to deploy the final XGBoost model.
## 10. Conclusions
- We have successfully used the Extreme Gradient Boosting (XGBoost) for bank loan default prediction. 
- Probability of default depends heavily on the average FICO score of the applicant. 
- This model provides an effective basis for loan credit approval in order to identify risky customers from a large number of loan applicants using predictive modeling. 

