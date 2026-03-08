# DEGO Project- Team 13

## Team Members
* Ana Reis (Student_Id: 70047) - Governance Officer
* Artur Bastos (Student_Id: 52011) - Data Engineer
* Sara Martins Fernandes (Student_Id: 51489) - Product Lead
* Vanessa Weiss (Student_Id: 73217) - Data Scientist


## Project Description
In this project our team acts as a Data Governance Task Force for the fintech company NovaCred.
The company uses automated systems to evaluate credit applications and recently faced regulatory concerns regarding possible discrimination in its lending decisions.
The goal was to analyse the provided dataset of credit applications in order to identify data quality issues, investigate potential bias in the approval decisions, and assess privacy and governance risks associated with the handling of sensitive personal data.
Additionaly, it was proposed a set of governance recommendations that could help improve the reliability, transparency, and regulatory compliance of NovaCred's data practices.

## Structure
```
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
```

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
There are several types of personally identifiable information present in the dataset, which were classified into three tiers: direct identifiers (full_name, email, ssn, ip_address), indirect identifiers (spending_behavior, gender, date_of_birth, zip_code), and financial personal data (credit_history_months, debt_to_income, savings_balance, annual_salary). This demonstrates the need for privacy and proper data governance.

To show the effectiveness of data privacy, we demonstrated pseudonymization, anonymization and data minimization techniques. SHA-256 hashing was applied to full_name, email, ssn, and ip_address, with ip_address also being removed afterwards. The date_of_birth field was generalized into age and the original column dropped. The spending_behavior field was fully removed as an additional minimization step. 
All of these steps are grounded in GDPR Art. 5(1)(c), which requires that personal data be adequate, relevant, and limited to what is necessary. This is demonstrated in *04-privacy-demonstration.ipynb*.

Regarding data governance, several compliance gaps were identified. There is no documented lawful basis for the collection and use of personal data (GDPR Arts. 6 and 13), and no data retention policy defining how long each data type is kept or when it should be deleted or anonymized (GDPR Arts. 5(1)(e) and 17). Access to direct identifiers is not restricted, meaning analysts work with raw personal data rather than pseudonymized datasets (GDPR Arts. 5(1)(f) and 32). 
Finally, over 80% of rejections are attributed to an opaque algorithm_risk_score with no meaningful explanation provided to applicants, raising concerns under GDPR Art. 5(2) and AI Act Arts. 11–12 on accountability and documentation.

## Governance Recommendations
Based on the findings of the analysis, the following governance recommendations are proposed for NovaCred:

**1. Audit Trails** — Record every credit decision with timestamp, model version, inputs used, outcome, and whether a human changed or confirmed the outcome (GDPR Art. 5(2); AI Act Arts. 11–12).

**2. Human Oversight** — The model should support, not replace, human decision-making, reducing the risk of unfair automated decisions (AI Act Art. 14).

**3. Lawful Basis** — Document why each category of personal data is collected and used, and inform applicants about data use, purpose, and retention (GDPR Arts. 6 and 13).

**4. Data Retention Policy** — Set retention periods for each data type and delete or anonymize identifiers when no longer needed (GDPR Arts. 5(1)(e) and 17).

**5. Access Control** — Restrict direct identifiers to authorized staff only. Analysts and data scientists should work mainly with pseudonymized datasets, not raw personal data (GDPR Arts. 5(1)(f) and 32).

**6. Data and Model Review** — Review each variable before use in analysis or modeling, checking necessity, justification, and proxy risk (GDPR Art. 5(1)(c); AI Act Arts. 9–10).

## Technologies used
Python was used as the programming language, and the required calculations were done with the help of Pandas, NumPy, Matplotlib, and Seaborn libraries, running on the Jupyter Notebook. MongoDB was used as the database to store and query the credit application records.

## Running the Project
To reproduce the analysis, install the required dependencies:
```
pip install pandas numpy matplotlib seaborn pymongo
```
Then run the notebooks in the following order:
* 01-data-quality.ipynb
* 02-bias-analysis.ipynb
* 03-proxy-analysis.ipynb
* 04-privacy-demonstration.ipynb


## Video Presentation
https://www.youtube.com/watch?v=FrWYiPiYawU
