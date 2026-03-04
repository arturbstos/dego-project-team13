# DEGO Project- Team 13

## Team Members
* Artur Bastos (Student_Id: 52011)
* Vanessa Weiss (Student_Id: 73217)
* Ana Reis (Student_Id: 70047)
* Sara Martins Fernandes (Student_Id: 51489)


## Project Description
In this project our team acts as a Data Governance Task Force for the fintech company NovaCred. 
The company uses automated systems to evaluate credit applications and recently faced regulatory concerns regarding possible discrimination in its lending decisions.
The goal was to analyse the provided dataset of credit applications in order to identify data quality issues, investigate potential bias in the approval decisions, and assess privacy and governance risks associated with the handling of sensitive personal data. 
Additionaly, it was proposed a set of governance recommendations that could help improve the reliability, transparency, and regulatory compliance of NovaCred’s data practices.

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
Each record includes personal information about the applicant, financial attributes related to creditworthiness, behavioural spending data, and the final loan decision produced by NovaCred’s automated system.

The raw dataset contains 502 application records. After identifying and removing duplicate entries during the data quality assessment, the final cleaned dataset used for analysis contains 500 unique applications.

## Data Quality Analysis (DQA)
### Executive Summary
The first stage of the project consisted on evaluating the quality of the raw dataset, where several data quality issues were identified during the analysis.
Starting with some inconsistent formats, followed by missing values, and irregular entries. These issues were addressed in order to create a clean dataset, more suitable for further analysis, which ensured that the dataset could be used more reliably for the pattern exploration and potential bias.

### Key Metrics
The first step in the project involved auditing the data and addressing any issues, such as data consistency, completeness, validity, and uniqueness.

There are some inconsistencies in the data, as observed from the data analysis. The date_of_birth field has inconsistent data formats in the data set. The data is expected to be in the standard ISO format (YYYY-MM-DD), but 157 data points, approximately 31.3% of the data, have different data formats such as DD/MM/YYYY and YYYY/MM/DD.
There is also inconsistency in the gender attribute. The data is expected to have the categorical values "Male" and "Female", but 114 data points, corresponding to 22.7% of the data have "M" and "F", which are abbreviations.

Lastly, there is a data type inconsistency in the annual_income field. The data is expected to be in a numeric data type, but 8 data points, which corresponds to 1.6% of the data have string data.
There are 2 duplicate application records in the data, which is addressed in the data cleaning step.

There is also some missing data in 1% of the data, such as in the attributes "email", "gender" "date_of_birth", and "ZIP Code".
All the steps in data preprocessing and validation are included in the notebook *01-data-quality.ipynb*.


### Bias and Fairness Analysis
After resolving the data quality issues, we analysed the dataset to investigate potential bias in loan approval decisions. Approval rates were compared across gender groups and the Disparate Impact (DI) ratio was calculated to evaluate potential discrimination.

The Disparate Impact (DI) ratio compares the approval rate of the unprivileged group with that of the privileged group. According to the four-fifths rule, values below 0.8 may indicate potential disparate impact in automated decision systems.

In addition to direct bias detection, we explored whether certain variables could act as proxy attributes for protected characteristics. Variables related to geographic information, financial attributes, or spending behaviour may indirectly encode demographic information and influence automated decisions. The methodology and results of this analysis are presented in both *02-bias-analysis.ipynb* and *03-proxy-analysis.ipynb*.

## Privacy and Governance
There are several types of personally identifiable information present in the dataset, including names, email addresses, Social Security numbers, IP addresses, and dates of birth, which demonstrates the need for privacy and proper data governance.

To show the effectiveness of data privacy, we have used the pseudonymization technique on the data. This technique replaces the original data with pseudo data while maintaining the effectiveness of the data. This is demonstrated in *04-privacy-demonstration.ipynb*.

Regarding the data governance, the results emphasize the need for robust data validation, data protection, and the development of audit trails for automated credit approvals. Moreover, monitoring fairness metrics and data governance policies can also ensure data governance regulations like GDPR and EU AI Act are followed.

## Technologies used
Python was used as the programming language, and the required calculations were done with the help of Pandas, NumPy, Matplotlib, and Seaborn libraries, running on the Jupyter Notebook.

## Running the Project
To reproduce the analysis, install the required dependencies:

pip install pandas numpy matplotlib seaborn

Then run the notebooks in the following order:
1. 01-data-quality.ipynb
2. 02-bias-analysis.ipynb
3. 03-proxy-analysis.ipynb
4. 04-privacy-demonstration.ipynb
