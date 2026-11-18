# AI Usage and Impact on Students and Professionals — Data Analytics Project

An end-to-end data analytics project examining the real-world adoption, productivity gains, tool dominance, and subscription ROI of generative AI among students and professionals.

🔗 **[View Dashboard Suite](https://datastudio.google.com/reporting/aa87d77b-b34c-4883-89a4-7f4305cbb91f)**

![image-Data Visualization](![alt text](image.png)) 
---

## 📌 Project Overview
As generative AI transforms modern education and workplaces, understanding *how* users engage with these tools is crucial. This project analyzes a refined survey dataset of **288 user records** using **Python (Pandas)** for exploratory data analysis and cleaning, **Google BigQuery SQL** for relational analysis, and **Looker Studio** for executive dashboarding.

---

## 📊 Dataset Description
* **Total Records:** 288 cleaned entries (after handling missing values and removing 12 duplicate records from 300 raw entries).
* **Columns (19 total):**
  * **Demographics:** `User_ID`, `Age`, `Gender`, `Country`, `User_Type` (Student vs. Professional), `Education_Level`, `Profession`, `Monthly_Income`.
  * **AI Tool Usage:** `AI_Tool`, `AI_Purpose`, `AI_Usage_Hours_Per_Day`, `Tasks_Performed`, `Monthly_AI_Cost`.
  * **Impact & ROI Metrics:** `Productivity_Score`, `Accuracy_Rating`, `Satisfaction_Score`, `Time_Saved_Hours_Per_Week`, `Work_or_Study_Hours_Per_Day`, `Would_Recommend`.

---

## 🛠️ Project Architecture & Workflow

### 1. Data Cleaning & Transformation (Python / Pandas)
* **Data Hygiene:** Inspected raw data structures, imputed missing categorical values with `'Unknown'` and numerical scores with median values to protect distributions.
* **Text & Format Standardization:** Stripped text suffixes (`"hrs"`, `"yrs"`), converted columns into proper `int64` and `float64` types, harmonized messy casing across gender and education levels, and standardized tool name variations (e.g., merging `"Chat GPT"` into `"ChatGPT"`).
* **Feature Engineering:** Created behavioral usage segments (e.g., *Casual*, *Moderate*, *Power User*) and calculated efficiency ratios (*Time Saved per Usage Hour*).

### 2. Relational Querying & SQL Analysis (BigQuery)
The project addresses **5 core analytical questions** using advanced SQL scripts (CTEs, conditional `CASE` statements, and aggregations):
1. **Efficiency Comparison:** Do students and professionals differ in daily AI usage and productivity?
2. **Tool Dominance & Purpose Matrix:** Which tools dominate specific use cases and accuracy?
3. **Usage vs. Time Saved:** Is higher daily AI usage associated with more weekly time saved?
4. **Pricing Tier ROI:** Does paid investment in AI tools lead to higher satisfaction and recommendation rates?
5. **Demographic Dependency:** How does AI adoption and daily dependency vary across age cohorts?

### 3. Business Intelligence & Dashboarding (Looker Studio)
* Designed a multi-page executive dashboard featuring:
  * **Top-Level KPI Scorecards:** Total users, average daily usage hours, and weekly hours saved.
  * **Tool vs. Purpose Matrix:** Pivot table with a heatmap highlighting tool dominance across tasks like coding and writing.
  * **Scatter Plot with Trendline:** Visualizing the positive correlation between daily usage and weekly time savings.
  * **Dual-Axis Combo Charts:** Evaluating monthly subscription costs against satisfaction and recommendation rates.

---

## 📈 Key Insights & Findings
* **Massive Time Savings:** Users save an average of over 5 hours per week, with heavy users (>5 hours daily) experiencing exponential returns ($r = 0.67$ correlation).
* **Tool Specialization:** ChatGPT, Claude, and Gemini capture the vast majority of coding, writing, and research workflows.
* **The Cost-Satisfaction Paradox:** Higher monthly AI costs do not automatically guarantee higher user satisfaction, as free and pro tiers perform competitively in loyalty and recommendation rates.

---

## 🧠 Skills Demonstrated
* **Data Cleaning & Preprocessing:** EDA, Missing Value Imputation, Deduplication, Text Standardization.
* **Database & SQL Querying:** Google BigQuery, Complex Scripting (CTEs), Conditional Aggregations (`CASE WHEN`).
* **Business Intelligence:** Looker Studio Layout Design, Interactive Filtering, Heatmap & Dual-Axis Configuration.
* **Analytical Modeling:** KPI & ROI Metric Design, Efficiency Ratios, Cohort Analysis.