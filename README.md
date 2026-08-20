HR Employee Attrition Analysis

Problem Statement



This analysis examines employee attrition patterns using HR records for 1,470 employees, in order to identify which employee groups are at highest risk of leaving and help HR proactively reduce turnover before it happens.



Dataset

Source: IBM HR Analytics Employee Attrition Dataset (Kaggle)

Size: 1,470 employee records

Key fields used: Age, OverTime, MonthlyIncome, JobSatisfaction, YearsAtCompany, DistanceFromHome, Department, Attrition

Tools \& Tech Stack

SQL (SQLite) — data exploration and aggregation

Python (Pandas) — data cleaning, feature engineering

Matplotlib / Seaborn — data visualization

Scikit-learn — predictive modeling (Logistic Regression)

Key Findings

Finding	Detail

Overtime impact	Employees working overtime leave at 30.5%, nearly 3x the rate of those who don't (10.4%)

Age impact	Employees under 25 have the highest attrition rate of any age group at 39.2%, declining steadily with age

Combined risk	Employees under 25 who also work overtime leave at roughly 67% — the single highest-risk segment identified

Modeling Approach



A Logistic Regression model was trained to predict individual attrition risk using age, overtime status, income, job satisfaction, tenure, and department.



Because only \~16% of employees in the dataset actually left (class imbalance), an initial unweighted model reached 87% accuracy but only caught 21% of actual leavers — a misleadingly high accuracy driven by majority-class bias.



Applying class\_weight='balanced' traded some precision for a substantial recall improvement:



Metric	Unbalanced Model	Balanced Model

Recall (class "Left")	21%	68%

Precision (class "Left")	91%	31%

Accuracy	87%	71%

ROC-AUC	0.759	0.761



Why the balanced model was chosen: for an HR use case, failing to flag a genuine flight risk is more costly than an unnecessary check-in with an employee who was actually fine. The balanced model catches roughly 2 out of every 3 employees who actually leave, versus 1 in 5 for the unbalanced model.



Business Recommendations

Prioritize workload review for employees under 25 working overtime — this is both the highest-risk group and one of the easiest to intervene on directly (e.g. reducing mandatory overtime).

Use model-generated risk scores to build a proactive check-in list for HR, rather than reacting only after resignation letters are submitted.

Follow up with a targeted survey for young, high-overtime employees to understand root causes (burnout, pay, lack of growth opportunities) — the model identifies who is at risk, not why.

Project Structure

hr-attrition-project/

├── data/                  # Raw dataset

├── notebook/

│   └── analysis.ipynb     # Full SQL + Python analysis

├── hr\_attrition.db        # SQLite database

└── README.md

Author



Hoysala K S

