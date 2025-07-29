# Telecom Customer Churn Analysis
![image alt](https://github.com/SirSahilSingh/telecomm-churn-prediction-and-analysis/blob/9015d2d853a230884b9b551daafa49b6ddd22ff0/image_telecomm/telecomm_banner.png)
--
**Objective**:   
A comprehensive data-driven analysis to understand and reduce customer churn within a telecom business context.

This project involves exploratory data analysis (EDA) on a telecom customer dataset to uncover trends, identify key churn indicators, and deliver actionable insights. The analysis aims to support business stakeholders in implementing effective customer retention strategies.


## 📑 Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Dataset Description](#dataset-description)
- [Tools & Technologies](#tools--technologies)
- [Project Workflow](#project-workflow)
- [Key Insights](#key-insights)
- [License](#license)
- [Contact](#contact)
---
## 🧩 Problem Statement

Customer churn — the rate at which customers discontinue their service — is a critical challenge in the telecom industry. High churn rates directly impact recurring revenue and customer acquisition costs. This project aims to explore patterns in customer behavior that lead to churn and provide insights to proactively identify customers at risk.

By understanding the drivers behind churn, telecom providers can create data-informed retention strategies, reduce customer turnover, and improve lifetime value (LTV).
---
## 📦 Dataset Description

The analysis is based on a simulated telecom dataset containing approximately **240,000 customer records**. Each record includes:

- **Demographics**: Gender, Age, Location
- **Account Info**: Registration Date, Telecom Partner, Number of Dependents
- **Financial Data**: Estimated Salary
- **Usage Patterns**: Number of Calls, SMS Sent, Data Used
- **Target Variable**: `churn` (1 = customer churned, 0 = retained)

The dataset was imported from a file named `updated_telecom_churn.csv`.
---
## 🧰 Tools & Technologies

The following tools and technologies were used in this project:

### Programming Language
- **Python 3.12**

### Libraries
- **pandas** – Data manipulation and cleaning
- **NumPy** – Numerical operations
- **matplotlib** & **seaborn** – Data visualization
- **scikit-learn** *(if applicable)* – Preprocessing and potential modeling
- **Jupyter Notebook** – Interactive development environment

### Development Environment
- JupyterLab / Jupyter Notebook
- Visual Studio Code (optional)

> All dependencies can be installed via `pip install -r requirements.txt` (file included in future steps).
---
## 🔄 Project Workflow (with Sample Code)

The analysis follows a structured and modular approach, organized as follows:

### 1. Data Loading & Initial Inspection
- Import the telecom churn dataset.
- Perform structural checks (columns, types, sample entries).
- Generate a high-level statistical summary to identify potential anomalies or data quality issues.

```python
import pandas as pd
import numpy as np

# Load dataset
df = pd.read_csv(r"C:\\Users\\sahil\\Downloads\\updated_telecom_churn.csv")
print("Dataset loaded successfully.")
```

### 2. Data Cleaning & Preprocessing
- Handle missing or inconsistent values.
- Address illogical data entries (e.g., negative values in usage metrics).
- Convert data types appropriately (e.g., dates, categorical variables).
- Normalize and standardize where needed for analysis clarity.

```python
# Replace negative 'data_used' values with NaN
df['data_used'] = pd.to_numeric(df['data_used'], errors='coerce')
df.loc[df['data_used'] < 0, 'data_used'] = np.nan

print("Negative 'data_used' values replaced with NaN.")
```

### 3. Feature Engineering
- Create new features (e.g., tenure, usage ratios) to enrich the dataset.
- Bin continuous variables for segmentation.
- Extract temporal patterns from dates (e.g., tenure groups).

```python
# Example (Feature Engineering reused from cleaning as no unique feature logic was present)
df['data_used'] = pd.to_numeric(df['data_used'], errors='coerce')
df.loc[df['data_used'] < 0, 'data_used'] = np.nan
```
### 4. Exploratory Data Analysis (EDA)
- Examine feature distributions and relationships.
- Compare behavioral patterns between churned and non-churned customers.
- Use visualizations (histograms, box plots, bar charts, heatmaps) to reveal trends and correlations.

```python
# Calculate churn rate by telecom partner, sorted by churn rate
churn_by_partner = df.groupby('telecom_partner')['churn'].agg(['count', 'sum'])
churn_by_partner['churn_rate'] = (churn_by_partner['sum'] / churn_by_partner['count']).round(2)
churn_by_partner = churn_by_partner.rename(columns={
    'count': 'Total Users',
    'sum': 'Churned Users',
    'churn_rate': 'Churn Rate'
}).sort_values(by='Churn Rate', ascending=False) # Sort for better visualization

# Plotting the churn rate by telecom partner
plt.figure(figsize=(8, 5))
ax = sns.barplot(x=churn_by_partner.index, y=churn_by_partner['Churn Rate'], palette='magma')
plt.title("Churn Rate by Telecom Partner", fontsize=16)
plt.ylabel("Churn Rate", fontsize=12)
plt.xlabel("Telecom Partner", fontsize=12)
plt.xticks(rotation=45, ha='right', fontsize=10) # Rotate labels for readability
plt.yticks(fontsize=10)
plt.ylim(0, 0.35) # Consistent y-limit for churn rates

# Add churn rate labels on bars
for p in ax.patches:
    ax.annotate(f'{p.get_height():.2f}',
                (p.get_x() + p.get_width() / 2., p.get_height()),
                ha='center', va='center', xytext=(0, 10),
                textcoords='offset points', fontsize=10, color='black')

plt.tight_layout()
plt.show()
```
![image alt](https://github.com/SirSahilSingh/telecomm-churn-prediction-and-analysis/blob/9015d2d853a230884b9b551daafa49b6ddd22ff0/image_telecomm/chart.png)

### 5. Churn Behavior Insights
- Quantify and visualize churn distribution across different segments.
- Highlight key factors correlated with churn behavior (e.g., partner, region, data usage).
- Provide initial interpretation and business implications of the findings.

```python
# High-level churn analysis
churn_rate = df['churn'].value_counts(normalize=True) * 100
print(churn_rate)
```
---
## 📈 Key Insights

### Exploratory Data Analysis (EDA) – Key Insights

Based on the detailed exploratory data analysis, the following key insights have been uncovered regarding customer churn:

---

### 🔹 1. Overall Churn Distribution
* Approximately **20% of customers are churning**, indicating a significant challenge for customer retention.
* The remaining **80% are retained**, providing a substantial base to focus retention efforts.

---

### 🔹 2. Churn by Gender
* The churn rate is remarkably **consistent across both male and female customers (20%)**.
* This suggests that **gender alone is not a strong differentiating factor** in churn behavior, pointing towards other, more influential drivers.

---

### 🔹3. Churn by Telecom Partner
* **Significant variations in churn rates are observed across different telecom partners**:
    * **BSNL** exhibits the highest churn rate at **30%**.
    * **Vodafone** follows with a **25%** churn rate.
    * **Reliance Jio** and **Airtel** demonstrate lower churn rates at **15%** and **10%** respectively.
* This is the **most influential factor identified in this EDA**, strongly indicating that the choice of telecom partner, likely due to variations in service quality, network reliability, pricing, or customer support, directly impacts customer loyalty.

---

### 🔹 4. Churn by Age Group
* The churn rate remains **consistently around 20% across all age segments** (Youth, Adult, Middle Age, Senior).
* This implies that **age, as a standalone demographic factor, does not significantly predict churn**. Retention strategies should therefore be age-agnostic and focus on service-related improvements.

---

### 🔹 5. Usage Behavior by Churn Status
* There is **no major difference in average usage metrics** (calls made, SMS sent, data used) between churned and non-churned customers.
* The distributions of these usage metrics also show **considerable overlap**.
* This suggests that **basic usage activity is not a primary indicator of churn** in this dataset. Customers are not necessarily churning due to low usage.

---

### 🔹 6. Tenure vs Churn
* The average customer tenure is **nearly identical for both churned and non-churned customers** (approx. 1422 days vs. 1424 days).
* This indicates that **customer loyalty, as measured by tenure, does not strongly influence churn** in this dataset. Customers might be churning regardless of how long they've been with the company.

---

### 🔹 7. Churn by Number of Dependents
* The distribution of dependents among churned and non-churned customers is **highly similar**, with most customers having 0-2 dependents regardless of their churn status.
* This feature does **not appear to be a significant driver of churn**.

---

### 🔹 8. Churn by Salary Bucket
* Churn rates remain **consistent (around 20%) across all salary buckets** ('Low', 'Mid', 'High').
* This suggests that **estimated salary, when categorized, does not have a direct impact on churn**. Affordability or economic standing might not be the primary reasons for customers leaving in this dataset.

---

### Final Synthesis: The Core Drivers of Churn

> Based on this comprehensive Exploratory Data Analysis, it's evident that **churn is not primarily driven by static demographic features like gender, age, or salary, nor by basic usage patterns (calls, SMS, data) or customer tenure.**
>
> Instead, the most significant factor influencing churn is the **telecom partner itself.** The substantial differences in churn rates among providers (BSNL and Vodafone having significantly higher churn) strongly suggest that **service quality, network performance, customer support, pricing structures, or specific value propositions offered by different partners** are the core drivers of customer dissatisfaction and eventual churn.
>
> Future efforts for customer retention should primarily focus on in-depth analysis of the service delivery and customer experience aspects of the high-churn telecom partners.
---
## 📬 Contact

For questions, feedback, or collaboration opportunities:

**SINGH SAHIL**  
Data Analyst  
📧 Email: [sahil040305@gmail.com]  
🔗 LinkedIn: [SINGH SAHIL](https://linkedin.com/in/sirsinghsahil)  
💻 GitHub: [SirSahilSingh](https://github.com/SirSahilSingh)

Feel free to reach out if you’re interested in improving this project or discussing data analytics opportunities.
