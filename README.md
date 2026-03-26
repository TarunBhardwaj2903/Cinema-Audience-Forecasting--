# Cinema Audience Forecasting Challenge 🎬📊

## 🏆 Achievements & Academic Context
* **Performance:** Achieved a rank within the **Top 20% of Candidates** in the competition.
* **Institution:** This project was developed as part of the **IIT Madras BS in Data Science and Applications**.
* **Supervision:** Conducted under the guidance and supervision of **IIT Madras Professors**.

---

## Project Overview
This repository contains a machine learning pipeline designed to forecast the daily audience count for various movie theaters. The solution integrates multiple data sources—including online booking platforms, point-of-sale (POS) systems, and theater metadata—to predict attendance numbers.

The project employs advanced time-series feature engineering and compares multiple regression models, ultimately selecting a **Random Forest Regressor** to generate final predictions.

## 📂 Dataset Description
The solution utilizes a relational dataset comprising the following files:

* **Theater Metadata:**
    * `CinePOS_theaters.csv`: Information on theaters using the POS system.
    * `BookNow_theaters.csv`: Information on theaters using the online booking platform.
    * `movie_theater_id_relation.csv`: Mapping table to link POS IDs with Online IDs.
* **Transactional Data:**
    * `CinePOS_booking.csv`: Granular walk-in/offline booking transactions.
    * `BookNow_booking.csv`: Granular online booking transactions.
    * `BookNow_visits.csv`: Daily historical audience counts (Target variable).
* **Auxiliary Data:**
    * `date_info.csv`: Calendar metadata (day of week, holidays, etc.).

## 🛠️ Methodology & Pipeline

### 1. Data Cleaning
* **Sparsity Pruning:** Dropped high-sparsity columns (e.g., `latitude`, `longitude` in CinePOS data) to reduce noise.
* **ID Matching:** Filtered records with missing theater identifiers that could not be mapped.
* **Categorical Filling:** Imputed missing theater types/areas with an `'Unknown'` placeholder and created binary flags for missing data.

### 2. Feature Engineering
The "Secret Sauce" of this model lies in the robust feature extraction:
* **Temporal Features:** Extracted Year, Month, Day, Week of Year, and created an `Is_Weekend` boolean flag.
* **Booking Signals:**
    * **Lead Time:** Calculated the average number of days in advance tickets were booked online.
    * **Channel Volume:** Aggregated total tickets sold via POS vs. Online for every show date.
* **Time-Series Features (Lag & Rolling):**
    * **Lags:** Created features representing audience counts from 7, 14, and 28 days prior.
    * **Rolling Means:** Calculated 7-day and 14-day moving averages to capture recent trends.

### 3. Model Selection
Three regression models were trained and evaluated using **R-Squared ($R^2$)** on a time-based validation split (training on 2023 data, validating on early 2024 data).

| Model | Validation $R^2$ Score |
| :--- | :--- |
| LightGBM | 0.3031 |
| XGBoost | 0.3617 |
| **Random Forest** | **0.5281** |

*The Random Forest model significantly outperformed gradient boosting methods on this specific dataset layout, likely due to its ability to handle the high cardinality of theater IDs and non-linear temporal relationships without extensive tuning.*

### 4. Final Training
The final model was retrained on the full dataset (Train + Validation). For the test set, historical lag features were propagated using the last known values to generate the final submission.

## 💻 Requirements
To run this notebook, you will need Python 3.x and the following libraries:

```txt
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
lightgbm
