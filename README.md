# The Cinematic Economy: Analyzing the Impact of Budget, Star Prestige, Inflation, Genre and Seasonality on Film Success

## **Project Overview**

This project aims to analyze the factors influencing a film's success, defined by commercial revenue (Box Office) and user ratings. The analysis moves beyond simple correlations by integrating **four** comprehensive dimensions:

* **Economic Context:** Examining the impact of macro factors like **Inflation-Adjusted Budget** ($H_1$) and **Unemployment Rate**.
* **Star Prestige:** Quantifying the effect of **Accumulated Award Scores (Oscar History)** of the director and lead cast.
* **Temporal Factors:** Testing the revenue differential based on release timing, such as **major US Holiday windows**.
* **Categorical Attributes:** Determining if the film's **Genre** creates a statistically significant difference in mean box office revenue.

The core objective is to build a high-quality **predictive model for Box Office Revenue** using an enriched dataset and foundational Machine Learning techniques (Regression).

---

## **Hypotheses**

The following hypotheses will be tested using statistical methods:

1.  **Null Hypothesis ($H_0$):** Financial, temporal, and prestige factors do not significantly influence a film's box office revenue.
2.  **$H_1$—The Real Economic Effect:** Films released during periods of lower **Unemployment Rate** and higher **Inflation-Adjusted Budget** will exhibit a stronger positive correlation with Box Office Revenue.
3.  **$H_2$—Star Prestige Index:** Films whose lead actors or directors have a higher accumulated **Oscar/Award Prestige Score** (based on achievements *prior* to the film's release) will generate significantly higher revenue.
4.  **$H_3$—The "Holiday" Effect:** Films released during major **US Holiday windows** will achieve statistically higher revenue than those released during off-peak periods.
5.  **H₄—Genre Effect:** The category to which the films belong (e.g. Action, Comedy, Drama) creates a **statistically significant** difference on the average gross.

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

* **Initial Merge & Cleaning:** Primary Movie Data (`tmdb_5000_movies`) is combined with actor/crew data (`tmdb_5000_credits`) and filtered for valid financial entries (Budget & Revenue > $1000$). Complex JSON columns (Genres, Companies) are parsed into simple comma-separated strings.
* **Inflation Adjustment ($H_1$):** **CPI (Consumer Price Index)** data is merged by `Year` to calculate **`Real_Budget`** and **`Real_Revenue`**. This removes financial distortion over time.
* **Logarithmic Transformation:** Due to the right-skewed nature of financial data (where outliers like blockbusters distort the scale), a **$\log_{10}$ transformation** is applied to both `Real_Budget` and `Real_Revenue`. This normalization allows for clearer visualization of orders of magnitude and improves regression performance.
* **Economic Context Merge ($H_1$):** **Unemployment Rate** data (FRED) is incorporated by `Release Year`.
* **Feature Engineering:**
    * **Star Prestige Index ($H_2$):** Calculated by counting the cumulative **Oscar/Award Wins** of the film's director and top actors **prior to the movie's release date**. This ensures no look-ahead bias.
    * **Seasonality Flagging ($H_3$):** Creation of the binary feature **`Is_Holiday`** (checking the release date against the Holiday Calendar).
    * **Categorical Transformation ($H_4$):** Creation of the **`Genre Explosion`** dataset, where multi-genre films are duplicated per genre to enable valid comparative analysis via **ANOVA**.

---

## 2. Exploratory Data Analysis (EDA) & Hypothesis Testing

This stage validates the hypotheses ($H_1$ - $H_4$) using visual analysis and statistical testing.

* **EDA Visualizations:**
    * **Trend Analysis:** Time-series plot showing the yearly trend of Average **Log10 Revenue** and **Log10 Budget** to visualize relative growth.
    * **Economic Context:** Scatter plot comparing **Unemployment Rate** vs. **Log10 Revenue** to visualize the macro-economic impact ($H_1$).
    * **Categorical Comparison:** Box plots comparing the distribution of `Log_Real_Revenue` earned by major `Genre` categories.
    * **Distribution Analysis:** Box plots comparing the distribution of log-revenue for **Holiday** vs. **Non-Holiday** releases ($H_3$).
    * **Correlation Heatmap:** Visual inspection of relationships between all key numeric features (`Real_Budget`, `UNRATE`, `Star_Prestige_Index`, etc.).

* **Statistical Testing (Hypothesis Validation):**
    * **$H_1$ & $H_2$ (Correlation):** Perform **Pearson Correlation Analysis** to measure the strength of the relationship between continuous predictors and Revenue.
    * **$H_3$ (Seasonality):** Perform **Independent T-tests** to test the significance of the difference in mean revenue between Holiday and Non-Holiday releases.
    * **$H_4$ (Genre Effect):** Perform **One-Way ANOVA** (Analysis of Variance) to statistically determine if there is a significant difference in mean revenue across the top 5 major film genres.

---

## 3. Machine Learning (ML) Implementation

This stage focuses on building a predictive framework for financial success using the enriched features derived from Step 1.

* **Model:** I will implement a **Linear Regression** model as the primary benchmark, with a **Decision Tree Regressor** used for comparison and feature importance analysis.
* **Target:** `Log_Real_Revenue` (Using the log-transformed variable to satisfy the normality assumption of Linear Regression).
* **Features:** The model will be trained on the final, enriched set of predictors:
    * `Log_Real_Budget`
    * `Unemployment Rate`
    * `Star_Prestige_Index`
    * `Is_Holiday`
    * `Runtime`
    * `Vote_Average`
* **Evaluation:** The model's performance will be evaluated using standard regression metrics: **R-Squared ($R^2$)** for overall fit and **RMSE** (Root Mean Squared Error) for error quantification.
