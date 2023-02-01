# Wage-Determination-based-on-PERM-Cert
This is the code I used to predict the salaries using the "PERM Case" data published by the US Department Of Labour (DOL).
This readme aims to give the reader a sense of:
1. The background about the permanent labor certification
2. The Gradient-boosted decision trees algorithms (xgboost)

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

### **Since the permanent labor certification process approves cases based on the salary data of local US citizens or US persons, it is an appropriate indication of the salaries in the local labor market. The SOC system further classifies different professions with job level hierarchies. It further improves the standardization and the accuracy of the prediction.** 

# Dataset
The dataset used in this project is of the permanent labor certification in fiscal year 2021. 

*The link to the dataset: https://www.dol.gov/sites/dolgov/files/ETA/oflc/pdfs/PERM_Disclosure_Data_FY2021.xlsx*

*The metadata information of the dataset: https://www.dol.gov/sites/dolgov/files/ETA/oflc/pdfs/PERM_Record_Layout_FY2021.pdf*
