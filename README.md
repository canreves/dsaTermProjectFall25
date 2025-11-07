# 🎬 The Cinematic Economy: Analyzing the Impact of Budget and External Factors on Film Success

## **Project Overview**

This project aims to quantify the relationship between a film's production **budget** and its eventual success, defined by **commercial revenue (Box Office)** and **critical reception (User Ratings)**. The core objective is to move beyond the simple correlation between budget and revenue by integrating **enriched external factors** (such as public sentiment, star power, and seasonal effects).

We will develop a robust **Regression Model** to predict revenue, thereby fulfilling the requirement for applying Machine Learning techniques.

---

## **Hypotheses to be Tested**

The project will use statistical hypothesis testing to examine the following claims:

1.  **Null Hypothesis ($H_0$):** Budget and external factors do not significantly influence a film's box office revenue or rating.
2.  **$H_1$—The Budget Effect:** Films with higher **production budgets** will exhibit a **significantly positive correlation** with both higher Box Office Revenue and higher User Ratings.
3.  **$H_2$—Star Power/Director:** The inclusion of actors or directors with **high historical average popularity scores** (based on prior IMDB/TMDb data) will be positively correlated with higher revenue.
4.  **$H_3$—Public Sentiment (Enrichment):** Films that receive **more positive social media sentiment** during their pre-release/release window will achieve higher Box Office Revenue and Ratings compared to those with neutral or negative sentiment.

---

## **Data Sources and Collection Plan**

The project will combine a primary public dataset with an external dataset, fulfilling the requirement to **enrich** publicly available data.

| Dataset | Variables | Source & Collection Plan | Project Role |
| :--- | :--- | :--- | :--- |
| **Primary Data** | Budget, Box Office Revenue (Target), User Ratings (Target), Release Date, Genres, Cast & Crew | **Kaggle** (TMDb or IMDB Movie Dataset). Data will be downloaded directly via `pandas`. | **Core Financial & Rating Data** |
| **Enrichment Data** | Public Sentiment Score (or Seasonal/Holiday Context) | **Twitter/X API** or existing sentiment data sets, **OR** a self-compiled dataset of major holiday release dates. Data will be collected/compiled via a Python script and merged based on **Film Title** and **Release Date**. | **Socio-Cultural Enrichment** |

*The dataset will be filtered for films released between 2010 and 2024 (if available) to ensure relevance.*

---

## **Methods and Analysis Stages**

### 1. Data Processing and Feature Engineering
* Load and merge the Primary and Enrichment datasets.
* **Feature Engineering:** Calculate **Star/Director Popularity Index** and/or compute the **Public Sentiment Score**.
* Handle missing values and scale financial variables (Budget, Revenue).

### 2. Exploratory Data Analysis (EDA) and Hypothesis Testing
* **EDA:** Examine the distribution of Budget, Revenue, and Ratings using **Histograms** and **Box Plots**.
* **Hypothesis Testing:** Perform **t-tests** and **Correlation Analysis** to statistically test $H_1$ through $H_3$ against the Null Hypothesis ($H_0$).

### 3. Machine Learning (ML) Implementation
* **Prediction Task:** Apply **Regression Methods** (e.g., Multiple Linear Regression, Random Forest Regressor) to predict the **Box Office Revenue** (Target Variable).
* **Model Evaluation:** Evaluate model performance using metrics such as $R^2$ score and Root Mean Squared Error (RMSE).

