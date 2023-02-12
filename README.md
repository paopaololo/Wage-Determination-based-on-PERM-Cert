# Wage Determination based on The Permanent Labor Certification

*updated on 2/9/2023 to ver.3* 

![image](https://user-images.githubusercontent.com/107201347/216098246-399d5c2a-f182-4bc6-9648-500ac283b2ac.png)
*(The map of salary distribution of NYC based on zip codes. Plotted with Folium.)*

This is the code I used to predict the salaries using the "PERM Case" data published by the US Department Of Labour (DOL).
This readme aims to give the reader a sense of:
1. The background about the permanent labor certification
2. The Gradient-boosted decision trees algorithms (xgboost)
3. The Data Cleaning process
4. Discussion about the accuracy of the prediction model

# The Permanent Labor Certification

![image](https://user-images.githubusercontent.com/107201347/215913154-17004608-aebc-4592-a2e0-f8fa32cd7304.png)

*The process of any foreign labor certification. from https://www.foreignlaborcert.doleta.gov/about.cfm*

The type of foreign labor certification depends on the nature of the visa being requested: Permanent, H-1B, H-2A, H-2B, D-1.
In this project, we are only focusing on the **Permenant** cases.

A permanent labor certification issued by the Department of Labor (DOL) allows an employer to hire a foreign worker to work permanently in the United States. Once a permanent labor certification application has been approved by the DOL, the employer can seek immigration authorization from USCIS and initiate the I-140 process, which is the petition for a noncitizen worker to become a permanent resident in the United States.

DOL approves and issues permanent labor certification applications only after the petitioner has complied with DOL advertising and recruiting requirements and has established that there are no able, qualified, and available U.S. workers for the position and has rejected any U.S. job applicants for valid job-related reasons. The DOL will also look into the employer's wage offer range for the foreign worker and compare the offer to the prevailing wage based on the profession and job level.

## Prevailing Wage

A prevailing wage is the rate of wages and benefits paid to a number of similarly employed workers in a given geography. Prevailing wage laws can ensure that government dollars do not undercut local wage and benefit standards, prevent a race to the bottom among publicly funded contractors, support good jobs, and provide good value to taxpayers.

The prevailing wage is used for assessment in the permanent labor certification process. Therefore, the prevailing wage of an approved "Permenant" case is reflecting the labor market tied to the geopgraphical area. This is a key feature in the prediction model.

##  Standard Occupational Classification (SOC) System

The 2018 Standard Occupational Classification (SOC) system is a federal statistical standard used by federal agencies to classify workers into occupational categories for the purpose of collecting, calculating, or disseminating data. All workers are classified into one of 867 detailed occupations according to their occupational definition. To facilitate classification, detailed occupations are combined to form 459 broad occupations, 98 minor groups, and 23 major groups. Detailed occupations in the SOC with similar job duties, and in some cases skills, education, and/or training, are grouped together.

DOL classifies the job type of the foreign worker with the Standard Occupational Classification. Within the classification, the job levels are further divided into 4 levels. The system is another key input for the predication model.

### **Since the permanent labor certification process approves cases based on the salary data of local US citizens or US persons, it is an appropriate indication of the salaries in the local labor market. The salary offer to the foreign worker is also validated and audited throughout the process. The SOC system further classifies different professions with job level hierarchies. It further improves the standardization and the accuracy of the prediction.** 

# Gradient-boosted decision trees algorithms (XGBoost)
Gradient boosting is a machine learning technique used in regression and classification tasks, among others. It gives a prediction model in the form of an ensemble of weak prediction models, which are typically decision trees.

Gradient boosting combines weak "learners" into a single strong learner in an iterative fashion. The goal is to predict values by minimizing the mean square error:

![image](https://user-images.githubusercontent.com/107201347/215950121-235681a6-e638-40c2-a57b-52bdedd14385.png)

where i indexes over some training set of size n of actual values of the output variable y:
- ![image](https://user-images.githubusercontent.com/107201347/215950816-5a564160-13fd-438d-99be-cb5b29e1ddf9.png) the predicted value ![image](https://user-images.githubusercontent.com/107201347/215950719-ed488dac-ecb3-4652-bfa0-5cf4c840adec.png)
- ![image](https://user-images.githubusercontent.com/107201347/215951046-0cd96f24-2873-4302-bd3b-35c21ce84a7a.png) the observed value
- ![image](https://user-images.githubusercontent.com/107201347/215951147-650f56bf-d45f-4831-8fd6-fef0bede14fa.png) the number of samples in y

Since it is an iterative process, we now consider there are M stages. 
For each stage m of the gradient boosting, consider there is an intermediate and imperfect model ![image](https://user-images.githubusercontent.com/107201347/215952169-114e88ab-deb9-4b37-933b-380c1b8110b5.png). In order to improve the imperfect model, we need to fill the gap with an estimator ![image](https://user-images.githubusercontent.com/107201347/215952413-f0f4213f-2d19-48fd-a16c-0dd6e3b91f7d.png), so that:

![image](https://user-images.githubusercontent.com/107201347/215952511-34bf26cc-f093-4a9c-8d05-cb4ad53d62c5.png)

or equivalently 

![image](https://user-images.githubusercontent.com/107201347/215953381-45b8f0b5-2489-45e0-bd0a-0c14ea98d17a.png)

Minimizing ![image](https://user-images.githubusercontent.com/107201347/215953519-2d51da1f-4bce-4f05-bf93-43c075385552.png) is the goal of the gradient boosting.

To achieve that, the algorithm uses the gradient descent optimization to find local minima. 

The goal of XGBoost is to use gradient boosting on decision trees. This readme will not go into the explanation of decision trees, which is the basic tool for decision making. 

For details about decision trees, I recommend checking out https://github.com/SilverDecisions/SilverDecisions/wiki/Gallery.

# Dataset
The dataset used in this project is of the permanent labor certification in fiscal year 2021. 

*The link to the dataset: https://www.dol.gov/sites/dolgov/files/ETA/oflc/pdfs/PERM_Disclosure_Data_FY2021.xlsx*

*The metadata information of the dataset: https://www.dol.gov/sites/dolgov/files/ETA/oflc/pdfs/PERM_Record_Layout_FY2021.pdf*

![image](https://user-images.githubusercontent.com/107201347/215943184-21174fac-abd2-4dd4-b934-d5a631db5acd.png)

# Input (features) for the predication model
The following features are used as inputs for the prediction model:

**1. PW_SOC_CODE**

Occupational code associated with the job being requested for permanent labor certification, as classified by the Standard Occupational Classification (SOC) System.

**2. PW_SKILL_LEVEL**

Level of the prevailing wage determination.

**3. PW_WAGE**

Prevailing wage for the job being requested for permanent labor certification.

**4. WORKSITE_POSTAL_CODE**

Primary worksite address or location.

**5. FOREIGN_WORKER_EDUCATION**

Highest Education achieved by the foreign worker.

# Data Cleaning workflow

**1. Filter for "Certified-Expired" and "Certified"**

We do not need distinguish between "Certified-Expired" and "Certified". They are both approved by the DOL. The expiration indicates no consequent I-140 was filed after receiving the certification.

**2. Filter for Full-time Yearly Salary jobs**

**3. Cleaning and Manipulating the wages**
    
Change the prevailing wage back to float. Taking the midpoint of the wage-offer to target_wage. Target wage is the target of the predication model.

**4. Cleaning postal code**
    
Some of the postal codes in the data are 9 digits. The 9 digits zip helps mail carriers identify the correct address quickly. We do not need it for the prediction model. The 9 digits zip is divided into 5 digits and 4 digits by "-". (i.e. 12345-6789) The data cleaning process is stripping the last 4 digits along with the dash.

**5. Cleaning PW SOC**
    
Remove all NaNs and convert everything into string. Each code represent a job class. It cannot be numerical.
       
**6. Clean up missing values for remaining fields**

# Encode all the categorical columns 
There are several categorical columns need to be enocded for the input of the 

1. PW_SOC_CODE
2. PW_SKILL_LEVEL
3. WORKSITE_POSTAL_CODE
4. FOREIGN_WORKER_EDUCATION

Because there are certain hierachy with the columns, I encoded some of them manually.

# Discussion: Improvement and reflections

The accuracy of the model is not good:

![image](https://user-images.githubusercontent.com/107201347/216785821-e7f09aec-a73e-4d6f-a7ee-377cace1a1d7.png)

The fundamental problem of the model is due to the small sample size. If we consider Data Scientists data living in NYC:

![image](https://user-images.githubusercontent.com/107201347/218332954-ab7ce731-4d53-4786-9d1d-93565b893930.png)


There are a few issues I can control when cleaning and looking through the data. 

1. isnull() sometimes cannot detect the NaN in the dataset. I have to transform the column to another column to force the empty values into NaN.
2. There are other factors that drive the wage offer. I only incorporated a few factors.

I have the following proposal for improvements:
1. Include columns like:
   - FOREIGN_WORKER_REQ_EXPERIENCE (whether the foreign worker has the experience as required for the requested job opportunity)
   - MINIMUM_EDUCATION (The minimum U.S. diploma or degree required by the employer for the position)
   - REQUIRED_EXPERIENCE_MONTHS (If experience in the job offered is required, the number of months required)
2. Sort the SOC and the zip codes columns before encoding. If accuracy does not imrpove, use One Hot encoder.

The improvement is still under work. It will be updated shortly.

*update 2/4/2023

I tried including 
   - FOREIGN_WORKER_REQ_EXPERIENCE (whether the foreign worker has the experience as required for the requested job opportunity)
   - MINIMUM_EDUCATION (The minimum U.S. diploma or degree required by the employer for the position)
   - REQUIRED_EXPERIENCE_MONTHS (If experience in the job offered is required, the number of months required)

It produces higher errors:

![image](https://user-images.githubusercontent.com/107201347/216783398-525ec8ce-abdc-4ce3-a3a1-3f0ab195303d.png)

However, including the worksite city improves the accuracy a bit:

![image](https://user-images.githubusercontent.com/107201347/216785781-d6a7f925-c5c2-4823-830c-989bb34f7361.png)

The fundamental solution to this is to expand the input data. However, there are only limited permenant certification being processed by DOL a year.
My next step is to see if I can incoorprate other foreign work certifications data in the same year, like working visa H1B. 




