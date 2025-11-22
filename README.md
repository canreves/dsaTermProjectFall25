# The Cinematic Economy: Analyzing the Impact of Budget, Inflation, and Seasonality on Film Success

## **Project Overview**
This project analyzes the key factors influencing a film's financial success and critical reception. While production budget is often seen as the primary driver of box office revenue, raw financial data can be misleading without economic context.

**The goal of this project is to build a predictive model for Box Office Revenue** by moving beyond simple correlations. To achieve this, I will enrich a standard movie dataset with:
1.  **Inflation Data:** Adjusting historical budgets to 2024 values to allow for fair comparison across decades.
2.  **Seasonality & Holiday Data:** Analyzing the impact of releasing films during major holiday windows (e.g., Christmas, Summer Blockbuster season).

This approach fulfills the course requirement of applying Machine Learning techniques on an enriched, real-world dataset.

---

## **Hypotheses**

I will test the following hypotheses to understand the drivers of film success:

1.  **Null Hypothesis ($H_0$):** Adjusted budget and release timing do not significantly influence a film's box office revenue.
2.  **$H_1$—The Real Budget Effect:** When adjusted for **inflation**, films with higher production budgets will exhibit a stronger positive correlation with Box Office Revenue compared to raw nominal budgets.
3.  **$H_2$—Star Power:** Films featuring top-tier actors or directors (quantified by historical popularity) will generate higher revenue, partially independent of the budget.
4.  **$H_3$—The "Holiday" Effect:** Films released during major **US Holiday windows** (e.g., Thanksgiving, Christmas, July 4th) will achieve statistically higher revenue than those released during off-peak periods.

---

## **Data Sources and Collection Plan**

I will use a primary dataset from Kaggle and enrich it with two external data sources to add economic and temporal context.

| Dataset | Content | Source & Method | Role in Project |
| :--- | :--- | :--- | :--- |
| **Primary Data** | Budget, Revenue, Ratings, Release Date, Cast, Runtime | **Kaggle (IMDB or TMDb Movie Dataset).** Downloaded as `.csv`. | **Core Dataset** |
| **Enrichment 1** | US Consumer Price Index (CPI) | **US Bureau of Labor Statistics.** A static table of yearly inflation rates. | **Economic Normalization** (Calculating "Real Budget") |
| **Enrichment 2** | US Federal Holidays | **Python `holidays` library** or static calendar list. | **Temporal Context** (Flagging Holiday Releases) |

*Data filtering:* The analysis will focus on films released between **2010 and 2024** to ensure relevant market trends, though historical data may be used for inflation modeling.

---

## **Methods and Analysis Stages**

The project pipeline follows the standard data science workflow:

### 1. Data Processing & Enrichment
* **Merge & Clean:** Combine the movie dataset with CPI data based on the `Year`.
* **Inflation Adjustment:** Create a new feature `Real_Budget` by applying the CPI multiplier to the raw `Budget` column.
* **Feature Engineering:**
    * `Is_Holiday`: A binary feature (0/1) indicating if the release date falls within a 7-day window of a major holiday.
    * `Star_Score`: A simple index based on the cast's previous movie performance (if available in the dataset).

### 2. Exploratory Data Analysis (EDA)
* **Visualizations:**
    * Scatter plots comparing `Raw Budget` vs. `Revenue` and `Real Budget` vs. `Revenue`.
    * Bar charts comparing average revenue of **Holiday** vs. **Non-Holiday** releases.
* **Statistical Testing:** Perform a **t-test** to determine if the difference in revenue between holiday and non-holiday releases is statistically significant.

### 3. Machine Learning (ML)
* **Model:** I will implement a **Linear Regression** model (and potentially a Decision Tree Regressor for comparison).
* **Target:** `Box Office Revenue`.
* **Features:** `Real_Budget`, `Is_Holiday`, `Runtime`, `Vote_Average` (Ratings).
* **Evaluation:** The model's performance will be evaluated using **R-Squared ($R^2$)** and **RMSE** (Root Mean Squared Error) to see how well financial and temporal factors explain the variance in revenue.

