# DEGO Project- Team 13

## Team Members
* Artur Bastos (Student_Id: 52011)
* Vanessa Weiss (Student_Id: 73217)
* Ana Reis (Student_Id: 70047)
* Sara Martins Fernandes (Student_Id: 51489)


## Project Description
The goal of this project is to analyze a dataset of credit loan applications. The focus is on evaluating the quality of the data and exploring whether any bias can be observed in the loan approval decisions.

Automated credit systems rely heavily on the quality of the data they use.
Therefore, understanding data quality problems and possible bias is an important step before using the data for analysis or decision making.

## Structure
project/
│
├── data/  #contains the datasets used in this project
│   ├── raw_credit_applications.json  #the original dataset with some potential inconsistencies and/or missing values
│   └── clean_credit_applications.json  #cleaned dataset generated after the data quality analysis
│
├── notebooks/  #contains the notebooks used for the analysis
│   ├── 01-data-quality.ipynb  #performs the data quality assessment and the cleaning process
│   └── 02-bias-analysis.ipynb  #explores potential bias in the loan approval decisions
│
├── README.md
└── LICENSE

## Dataset Description
The dataset contains records of credit loan applications. Each one includes information about the applicant, its financial situation, spending behaviour, and the final loan decision.

## Data Quality Analysis (DQA)
### Executive Summary
The first stage of the project consisted on evaluating the quality of the raw dataset, where several data quality issues were identified during the analysis.
Starting with some inconsistent formats, followed by missing values, and irregular entries. These issues were addressed in order to create a clean dataset, more suitable for further analysis, which ensured that the dataset could be used more reliably for the pattern exploration and potential bias.

### Key Findings
#### Consistency Issues
