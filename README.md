# DEGO Project- Team 13

## Team Members
* Ana Reis (Student_Id: 70047)
* Artur Bastos (Student_Id: 52011)
* Sara Martins Fernandes (Student_Id: 51489)
* Vanessa Weiss (Student_Id: 73217)


## Project Description
In this project our team acts as a Data Governance Task Force for the fintech company NovaCred.
The company uses automated systems to evaluate credit applications and recently faced regulatory concerns regarding possible discrimination in its lending decisions.
The goal was to analyse the provided dataset of credit applications in order to identify data quality issues, investigate potential bias in the approval decisions, and assess privacy and governance risks associated with the handling of sensitive personal data.
Additionaly, it was proposed a set of governance recommendations that could help improve the reliability, transparency, and regulatory compliance of NovaCred's data practices.

## Structure
project-team13/
│
├── data/  #contains the datasets used in this project
│   ├── raw_credit_applications.json
│   └── clean_credit_applications.json
│
├── notebooks/  #contains the notebooks used for the analysis
│   ├── 01-data-quality.ipynb
│   ├── 02-bias-analysis.ipynb
│   ├── 03-proxy-analysis.ipynb
│   └── 04-privacy-demonstration.ipynb
│
├── presentation/  #contains the material used for the final presentation
│
├── README.md
└── LICENSE

## Dataset Description
The dataset consists of credit loan applications stored in nested JSON format.
Each record includes personal information about the applicant, financial attributes related to creditworthiness, behavioural spending data, and the final loan decision produced by NovaCred's automated system.

The raw dataset contains 502 application records. After identifying and removing duplicate entries and invalid records during the data quality assessment, the final cleaned dataset used for analysis contains 499 unique applications.

## Data Quality Analysis (DQA)
### Executive Summary
The first stage of the project consisted on evaluating the quality of the raw dataset, where several data quality issues were identified during the analysis.
Starting with some inconsistent formats, followed by missing values, negative values, and irregular entries. These issues were addressed in order to create a clean dataset, more suitable for further analysis, which ensured that the dataset could be used more reliably for the pattern exploration and potential bias.

### Key Metrics
The first step in the project involved auditing the data and addressing any issues, such as data consistency, completeness, validity, and uniqueness.

There are some inconsistencies in the data, as observed from the data analysis. The date_of_birth field has inconsistent data formats in the data set. The data is expected to be in the standard ISO format (YYYY-MM-DD), but 161 data points, approximately 32.3% of the data, have different data formats such as DD/MM/YYYY and YYYY/MM/DD.
There is also inconsistency in the gender attribute. The data is expected to have the categorical values "Male" and "Female", but 114 data points, corresponding to 22.7% of the data have "M" and "F", which are abbreviations. Additionally, 2 records had empty gender values, which were set to "Unknown".

There is also a data type inconsistency in the annual_income field. The data is expected to be in a numeric data type, but some data points have string data, meaning the field was found with 3 different types (float, int, str), which were all standardised to float.
There are 2 duplicate application records in the data (app_001 and app_042), which were flagged with a DUPLICATE_ENTRY_ERROR and addressed in the data cleaning step.
There is also some missing data in the dataset, such as in the attributes "email" (7 records), "date_of_birth" (4 records), "ip_address" (4 records), "ssn" (4 records), and "zip_code" (1 record).

Finally, invalid values were also identified: 2 records had negative credit_history_months values (app_043 with -10 and app_156 with -3), which were corrected to 0, and 1 record had a negative savings_balance (app_290 with -5000), which was removed.
All the steps in data preprocessing and validation are included in the notebook *01-data-quality.ipynb*.

### Bias and Fairness Analysis
After resolving the data quality issues, we analysed the dataset to investigate potential bias in loan approval decisions. Approval rates were compared across gender groups and the Disparate Impact (DI) ratio was calculated to evaluate potential discrimination.
The Disparate Impact (DI) ratio compares the approval rate of the unprivileged group with that of the privileged group. According to the four-fifths rule, values below 0.8 may indicate potential disparate impact in automated decision systems.

The analysis revealed that male applicants had an approval rate of 66.0% compared to 50.4% for female applicants, resulting in a DI ratio of 0.76, which falls below the 0.8 threshold and indicates potential disparate impact. Age-based patterns were also identified, with applicants aged 18-24 showing an approval rate of only 41.7% compared to 65.3% for the 35-44 age group (DI = 0.556).

In addition to direct bias detection, we explored whether certain variables could act as proxy attributes for protected characteristics. The zip_code field was found to have a strong correlation with gender (r = -0.806), indicating it acts as a geographic proxy for a protected attribute. Geographic disparities were also identified, with New York City applicants showing a 64.5% approval rate compared to 51.5% for Los Angeles applicants (DI = 0.515). 

Furthermore, sensitive spending categories such as Gambling, Adult Entertainment, and Alcohol were flagged as potential prohibited model inputs under EU AI Act Article 10. The methodology and results of this analysis are presented in both *02-bias-analysis.ipynb* and *03-proxy-analysis.ipynb*.


## Privacy and Governance
There are several types of personally identifiable information present in the dataset, which were classified into three tiers: direct identifiers (full_name, email, ssn, ip_address), indirect or special identifiers (spending_behavior, gender, date_of_birth, zip_code), and financial personal data (credit_history_months, debt_to_income, savings_balance). This demonstrates the need for privacy and proper data governance.
To show the effectiveness of data privacy, we have used the pseudonymization technique on the data. SHA-256 hashing was applied to the full_name, email, and ssn fields, replacing the original values while maintaining referential integrity for audit purposes. Additionally, the ip_address and spending_behavior columns were fully removed to demonstrate data minimisation. This is demonstrated in *04-privacy-demonstration.ipynb*.

Regarding the data governance, the results emphasize the need for robust data validation, data protection, and the development of audit trails for automated credit approvals. Specific GDPR concerns identified include the absence of a documented lawful basis for data collection (Art. 6), no right-to-erasure mechanism (Art. 17), and the risk of opaque automated decision-making given that over 80% of rejections were attributed to an algorithm_risk_score with no meaningful explanation (Art. 22). Moreover, monitoring fairness metrics and data governance policies can also ensure data governance regulations like GDPR and EU AI Act are followed.

## Governance Recommendations
Based on the findings of the analysis, the following governance recommendations are proposed for NovaCred:

### 1. Data Validation Pipeline
Enforce schema types, date format standards, and categorical value constraints at data ingestion. Automated anomaly alerts should be implemented for future data loads to prevent the issues identified in this analysis from recurring.

### 2. Fairness Monitoring 
Compute the Disparate Impact ratio on every model retrain and trigger a manual review whenever Disparate Impact falls below 0.8. Monitoring should also cover intersectional effects such as gender combined with age, as the analysis revealed compounding disparities across these groups.

### 3. Proxy Attribute Audit
Remove or control the zip_code variable from model features, given its strong correlation with gender (r = -0.806). 
All geographic and spending-related variables should be evaluated for protected-characteristic correlation before being included in model training. Sensitive spending categories (Gambling, Adult Entertainment, Alcohol) should be explicitly excluded per EU AI Act Art. 10.

### 4. Privacy-by-Design
Pseudonymise all Personally Identifiable Information fields at rest using strong hashing algorithms. Implement consent tracking, data retention schedules, and GDPR Art. 17 erasure workflows across all systems handling applicant data.

### 5. Audit Trail and Explainability
Log all automated credit decisions with the model version, input features used, and outcome. Replace opaque algorithm_risk_score rejections with meaningful, human-readable explanations, and implement human review escalation for borderline cases as required by EU AI Act Art. 14.

### 6. Model Governance Board
Establish a governance board responsible for quarterly fairness reporting to senior management, a documented approval process before any credit model goes live, and role-based data access controls to limit exposure of sensitive applicant information.

### Technologies used
Python was used as the programming language, and the required calculations were done with the help of Pandas, NumPy, Matplotlib, and Seaborn libraries, running on the Jupyter Notebook.

## Running the Project
To reproduce the analysis, install the required dependencies:
pip install pandas numpy matplotlib seaborn

Then run the notebooks in the following order:
01-data-quality.ipynb
02-bias-analysis.ipynb
03-proxy-analysis.ipynb
04-privacy-demonstration.ipynb
