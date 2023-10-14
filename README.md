# Stroke_Prediction

<p style="padding:10px;background-color:#432C0A;margin:0;color:#c69c6c;font-family:Arial, sans-serif;font-size:1.5em;text-align:center;border-radius:10px;overflow:hidden;font-weight:500">Business Case</p>


<p style="text-align:justify;color:#2E4057;font-family:Arial, sans-serif;font-size:1.1em">
<b>Introduction</b> An ACO - Accountable Care Organization has developed an outreach program where a team of health coaches will work with members to reduce their risk of stroke. However, they do not have enough resources to engage every member of their population. Therefore, they would like us to develop a model that can be used to match the health coaches with those individuals who have the highest risk of stroke.  <br>
<br>    
<b>Business Process:</b> Currently, a manual approach is used to engage health coaches with members of ACO. <br>
<br>
<b>Limitations:</b> Do not have enough resources to engage every member of their population. <br>
<br>
<b>Business Requirement:</b> Predicting highest stroke risk based on various factors like lifestyle and  clinical data
<br>
<br>
</p>


<p style="padding:10px;background-color:#432C0A;margin:0;color:#c69c6c;;font-family:Arial, sans-serif;font-size:1.5em;text-align:center;border-radius:10px;overflow:hidden;font-weight:500">Dataset</p>

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


<p style="padding:10px;background-color:#432C0A;margin:0;color:#c69c6c;font-family:Arial, sans-serif;font-size:1.5em;text-align:center;border-radius:10px;overflow:hidden;font-weight:500">Exploratory Data Analysis</p>

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


