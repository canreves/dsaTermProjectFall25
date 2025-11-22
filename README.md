#  The Cinematic Economy: Analyzing the Impact of Budget, Star Prestige, Inflation and Seasonality on Film Success

## **Project Overview**

This project aims to analyze the factors influencing a film's success, defined by commercial revenue (Box Office) and User Ratings. The analysis moves beyond simple correlations by integrating five key dimensions: **economic context (Inflation & Unemployment)**, **prestigious achievements (Award Scores)**, and **temporal factors (Seasonality)**.

The core objective is to build a high-quality **predictive model for Box Office Revenue** using an enriched dataset and foundational Machine Learning techniques (Regression).

---

## **Hypotheses**

The following hypotheses will be tested using statistical methods:

1.  **Null Hypothesis ($H_0$):** Financial, temporal, and prestige factors do not significantly influence a film's box office revenue.
2.  **$H_1$—The Real Economic Effect:** Films released during periods of lower **Unemployment Rate** and higher **Inflation-Adjusted Budget** will exhibit a stronger positive correlation with Box Office Revenue.
3.  **$H_2$—Star Prestige Index:** Films whose lead actors or directors have a higher accumulated **Oscar/Award Prestige Score** (based on achievements *prior* to the film's release) will generate significantly higher revenue.
4.  **$H_3$—The "Holiday" Effect:** Films released during major **US Holiday windows** will achieve statistically higher revenue than those released during off-peak periods.

---

## **Data Sources and Collection Plan**

I will use a primary dataset from Kaggle and enrich it with **four external data sources** to add deep economic, prestige, and temporal context, fulfilling the course's data enrichment requirement.

| Dataset | Content | Source & Method | Role in Project |
| :--- | :--- | :--- | :--- |
| **Primary Data** | Budget, Revenue, Ratings, Release Date, Cast, Runtime | **Kaggle (IMDB or TMDb Movie Dataset).** Downloaded as `.csv`. | **Core Dataset** |
| **Enrichment 1** | US Consumer Price Index (CPI) | **US Bureau of Labor Statistics.** Static table, used for **yearly adjustment.** | **Economic Normalization** (Calculating "Real Budget") |
| **Enrichment 2** | US Unemployment Rate | **Federal Reserve Economic Data (FRED).** Monthly/yearly rates, merged on **Release Year.** | **Economic Health** (Disposable Income Proxy) |
| **Enrichment 3** | Academy Awards (Oscars) History | **Kaggle (The Oscar Award).** Requires **Feature Engineering** to calculate cumulative scores. | **Star Prestige Index** (The Basis of the Prestige Score) |
| **Enrichment 4** | **Major US Federal Holiday Calendar** | **Official US Govermentt Calendar Data (BLS, OPM, etc.).** Used to create a binary flag. | **Temporal Context** (Flagging Holiday Releases) |

---

## **Methods and Analysis Stages**

The project pipeline follows the standard data science workflow:

### 1. Data Processing & Feature Engineering

* **Merge & Clean:** Combine the movie dataset with CPI, Unemployment, and Awards data based on `Year` and `Film/Person ID`.
* **Inflation Adjustment:** Create the new feature `Real_Budget` using the CPI multiplier.
* **Economic Context Merge:** Incorporate the corresponding **Unemployment Rate** for the film's release year.
* **Feature Engineering (Crucial Steps):**
    * **Star Prestige Index:** Calculate the cumulative count of major awards (Oscars/Nominations) received by the film's lead actors and director **prior to the film's release date**. This score (`Star_Prestige_Index`) will be used as a predictor.
    * **Seasonality Flagging:** Create the binary feature `Is_Holiday` (0/1).

### 2. Exploratory Data Analysis (EDA) & Hypothesis Testing

* **Visualizations:**
    * Scatter plots comparing `Real Budget` vs. `Revenue`.
    * Bar charts comparing average revenue of **Holiday** vs. **Non-Holiday** releases.
    * Time-series plot showing **Unemployment Rate** vs. **Average Film Revenue** over the study period.
* **Statistical Testing:**
    * Perform **t-tests** to test the significance of differences in revenue (testing $H_3$).
    * Perform **Correlation Analysis** to test the strength of the relationship between `Real_Budget`, `Unemployment Rate`, and `Star_Prestige_Index` vs. `Revenue` (testing $H_1$ and $H_2$).

### 3. Machine Learning (ML) Implementation

* **Model:** I will implement a **Linear Regression** model (and potentially a Decision Tree Regressor for comparison) as the primary ML method.
* **Target:** `Box Office Revenue` (Continuous Variable).
* **Features:** `Real_Budget`, `Unemployment Rate`, `Star_Prestige_Index`, `Is_Holiday`, `Runtime`, and `Vote_Average` (Ratings).
* **Evaluation:** The model's performance will be evaluated using **R-Squared ($R^2$)** and **RMSE** (Root Mean Squared Error) to see how well financial, temporal, and prestige factors explain the variance in revenue.
