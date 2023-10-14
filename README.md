# Stroke_Prediction


### Business Case

<p style="text-align:justify;color:#2E4057;font-family:Arial, sans-serif;font-size:1.1em">
<b>Introduction</b> An ACO - Accountable Care Organization has developed an outreach program where a team of health coaches will work with members to reduce their risk of stroke. However, they do not have enough resources to engage every member of their population. Therefore, they would like us to develop a model that can be used to match the health coaches with those individuals who have the highest risk of stroke.  <br>
<br>    
Business Process: Currently, a manual approach is used to engage health coaches with members of ACO. <br>
<br>
Limitations: Do not have enough resources to engage every member of their population. <br>
<br>
Business Requirement: Predicting highest stroke risk based on various factors like lifestyle and  clinical data
<br>
<br>
</p>


### Dataset

* **id:** Unique identifier
* **gender:** Gender of the patient (Male, Female, Other)
* **age:** Age of the patient
* **hypertension:** **0** if the patient doesn't have hypertension, **1** if the patient has hypertension
* **heart_disease:** **0** if the patient doesn't have any heart diseases, **1** if the patient has a heart disease
* **married:** **Yes** if the patient is married, **No** if the patient is not married 
* **occupation:** Profession of the patient (A,B,C,D,E)
* **residence:** Residence category of the patient (Rural, Urban)
* **metric_1:** clinical data
* **metric_2:** clinical data
* **metric_3:** clinical data
* **metric_4:** clinical data
* **metric_5:** clinical data
* **smoking_status:** Smoking status of the patient (formerly smoked, never smoked, smokes, Unknown). **Unknown** in **smoking_status** means that the information is unavailable for this patient
* **stroke:** **1** if the patient had a stroke or **0** if not


### Exploratory Data Analysis

<p style="font-weight:bold;font-size:1.1em ">Insights from Data Quality Assessment

1. **It is evident that data set is "largely imbalanced" towards "No stroke" category.**

2. **Age column contains extreme values like -10, 999,1000 which need to be removed.**

3. **metric_1,metric_2,metric_4 contains outliers**

3. **metric_2 and smoking_status contains null values**

4. **There are no duplicates in the data set**

5. **metric_1 and metric_5 columns seems to be duplicated - both contains exactly same data(one can be removed)**

6. **Low information column like ID can be remove**

7. **Mixed data types**

8. **Label encoding would be required for categorical column**



### Resampling

<b> Given dataset is highly imbalanced and skewed towards Negative stroke cases where the positive class (stroke in this case) is a small minority. </b> 

In order to handling the imbalance in the data various techniques like oversampling, undersampling, Synthetic sampling can be used.

- Undersampling: This involves removing some of the majority class (no stroke) samples to balance the class distribution. However, this can lead to loss of information and may not always be the best approach.

- Oversampling: This involves replicating some of the minority class (stroke) samples to balance the class distribution. This can lead to overfitting, as the model may become too familiar with the minority class samples.

- Synthetic Sampling: This involves generating new samples in the minority class (stroke) by using techniques like SMOTE (Synthetic Minority Over-sampling Technique), which generates synthetic samples by interpolating between existing samples.


<b> Hence, I intend to use Synthetic Sampling using SMOTE </b>


### Data Analysis Results

- Categorical Features (Order) :
    - gender : female > male
    - hypertension : no hypertension > hypertension
    - heart_disease : no heart disease > heart disease
    - married : married > no married
    - occupation : B > D > E > A
    - residence : Urban > Rural
    - smoking_status : never smoked > formerly smoked > smokes
- Discrete Features (Range) :
    - age : 55 - 80
    - metric_1 : 80 - 200
    - metric_2 : 20 - 40
    - metric_4 : 96 - 100
            
<b>From the data, these order/range leads to stroke in members of ACO</b>

- Target (after resampling)
    - No stroke : 52% of data
    - Stroke: 48% of data


### Feature Engineering

## __Scaling__

Feature selection methods often rely on the assumption that features are on the same scale. In order to identify important features in the given dataset, it is neccesary to scale the data evenly. Because, if the features are on different scales, then some features may have more impact on the model than others due to their scale, even though they may not be the most important predictors.

By scaling the features, we can bring them to the same scale and ensure that each feature has an equal opportunity to impact the model. This can help feature selection methods to identify the most important predictors more accurately and lead to a more robust model.

With scikitlearn's ColumnTransformer buildng a pipeline to scale numerical features using StandardScalar and leaving categorical as-is.

## __Feature Selection__

After scaling the data performed feature selection using RandomForest's feature_importances_ method.
    
'feature_importances_' is calculated based on the RandomForest model's prediction performance, which is trained on the input data. The importance of a feature is determined by measuring the reduction in the model's performance when that feature is removed from the data. This model-based approach is efficient because it can capture the nonlinear relationships between features and the target variable.

     

From the above process, it shows that <b> age, metric_1,metric_2,metric_4, occupation, smoking_status </b> plays a significant role in predicting stroke risk.


### Modeling

__For this business problem, we are building a model to predict the highest risk for strokes.__
    
To start, we used a Generalized Linear Model (GLM) as a base model to model the risk of stroke. GLM is a flexible and powerful modeling technique that can handle a wide range of data distributions and can be used for both binary and continuous outcomes. Specifically, we use binomial, which is a type of GLM that is well-suited for binary outcomes.
    
GLM, to establish a baseline performance and gain insights into the relationships between the predictors and the outcome. This will help us identify the most important predictors and assess the overall predictive power of the model. To improve the performance of the GLM, we performed hyperparameter tuning on alpha and regularization parameters which did not result in any significant improvement (i.e) default hyperparameters of the model were well-suited for the data. 
    
Once we have established the baseline performance, we explored more on advanced machine learning algorithms, such as XGBoost, to improve the performance set by base model and identify individuals with the highest risk of stroke.
    
To further improve the performance of the XGBoost model, we performed hyperparameter tuning using RandomizedSearchCV by tuning the 'learning rate', 'max_depth', and 'n_estimators' hyperparameters to find the optimal values that maximize the model's performance.
    
with optimal hyperparameters for the XGBoost model, we retrain the model and evaluate its performance on a holdout dataset to ensure its generalizability to new data. 
    
By performing hyperparameter tuning on the XGBoost model, we achieved even better performance than the baseline GLM model.
 

### Model Evaluation

The GLM model has a higher number of Type 1 and Type 2 errors compared to XGBoost. This suggests that the GLM model may not be as accurate in its predictions as the XGBoost model.

Both models have a high number of True Positives and True Negatives, indicating that they are effective in correctly classifying positive and negative cases.

The XGBoost model has a higher overall accuracy, as it has a lower number of Type 1 and Type 2 errors compared to GLM.

In the context of predicting the highest risk of strokes, it would be important to minimize False Negatives (Type 2 errors), as these represent cases where the model incorrectly predicts that a patient is not at high risk of strokes when in fact they are. This is because a False Negative could result in a patient not receiving appropriate medical attention or interventions that could prevent or reduce the risk of stroke.

In ROC curves, a higher TPR (true positive rate) and a lower FPR (false positive rate) is considered better performance.

While comparing XGBoost and GLM models, it appears that the XGBoost model achieves a higher TPR (0.98) at a lower FPR (0.0) compared to the GLM model, which reaches a TPR of 0.82 at a higher FPR of 0.25.


### Results
__Pros__ 
    
XGBoost performed incredibly well on the given dataset in identifying highest risk for stroke with a F1 score of 98%. This suggests that the XGBoost model is performing better than the GLM model in terms of its ability to correctly identify positive cases while minimizing false positives.
    
while comparing the results of confusion matrix, XGBoost has recorded minimum Type 2 error (Actual 1, Predicted 0)  than GLM which suggests that it is more efficient in identifying stroke risk.
    
__Cons__
    
Eventhough GLM model is easy to implement, It almost classified 8% of the actual stroke data as No Stroke which is 
7.1% higher than XGBoost. 
    
GLM doesn't show any significant improvement even after performing hyperparamenter tuning. 


Therefore, We select XGBoost to predict highest stroke risk for the members of ACO


