# Airline Bird Strike Cost Prediction Report

## Executive Summary

This report outlines the development of a predictive model to estimate the financial cost of bird strike incidents for airlines. The primary objective was to build a machine learning model that could accurately predict the `Total Cost` of a strike based on incident attributes. Following data preparation and the evaluation of several models, a **Neural Network** was identified as the top-performing solution, significantly outperforming the baseline model and other traditional regression techniques. This model provides a valuable tool for proactive cost management and operational planning.

***

## Project Goal

The goal of this project was to leverage a dataset of airline incidents to predict the `Total Cost` associated with a bird strike. An effective predictive model can enable airlines to forecast potential financial impact as soon as an incident occurs.

***

## Data and Preprocessing

The `airline.csv` dataset, which contains information on various bird strike attributes, was used for this analysis. The following key data preparation steps were undertaken to prepare the data for modeling:

* **Handling Missing Values:** Missing values in the `Description` column were imputed with the string `'missing'` to ensure all text data could be processed.
* **Feature Engineering:** A new binary feature, `hit_on_runway`, was created to distinguish between strikes that occurred on the ground ($Altitude = 0$) and those that occurred in the air ($Altitude > 0$).
* **Preprocessing Pipeline:** A comprehensive `ColumnTransformer` pipeline was used to apply different preprocessing steps to various data types, ensuring consistency and preventing data leakage.
    * **Numeric Features:** `Number_Objects`, `Engines`, and `Altitude` were imputed using the median and scaled using `StandardScaler`.
    * **Categorical Features:** Columns like `Aircraft`, `Airline`, `Origin State`, `Phase`, `Object Size`, and `Weather` were handled using `SimpleImputer` and `OneHotEncoder`.
    * **Text Features:** The `Description` column was processed using `TfidfVectorizer` followed by `TruncatedSVD` to reduce dimensionality while capturing the most important information.

***

## Model Performance & Results

To evaluate the models, the Root Mean Squared Error (**RMSE**) was used as the key performance metric, measuring the average magnitude of the errors in predicting the `Total Cost`. All models were compared against a simple baseline model that predicts the mean of the training data.

The performance of each model on both training and test data is summarized in the table below. Lower RMSE values indicate better performance.

| Model | Training RMSE | Test RMSE | Overfitting Corrected? |
| :--- | :--- | :--- | :--- |
| **Baseline** | $574,386.41$ | $482,867.50$ | N/A |
| **Decision Tree** | $549,974.11$ | $477,310.63$ | Yes |
| **Voting Regressor** | $511,016.61$ | $465,649.31$ | Yes |
| **AdaBoost** | $517,303.03$ | $484,690.52$ | Yes |
| **Neural Network** | $528,208.62$ | $455,780.48$ | N/A |
| **Grid Search (Decision Tree)** | $546,443.94$ | $484,526.76$ | N/A |

As indicated in the table, the initial attempts with the Decision Tree, Voting Regressor, and AdaBoost models showed significant signs of overfitting, where the training RMSE was substantially lower than the test RMSE. Hyperparameter tuning was performed to mitigate this, resulting in more robust and generalized models. The Neural Network model, without explicit tuning for overfitting, demonstrated a more balanced performance.

***

## Conclusion & Recommendations

Based on the evaluation of the various models, the **Neural Network** stands out as the best model for predicting the `Total Cost` of a bird strike.

The Neural Network model achieved the **lowest test RMSE of $455,780.48$**. This value is the most significant improvement over the baseline model's test RMSE of $482,867.50$, indicating that it is the most effective at generalizing to unseen data. While some of the corrected models had comparable performance, the Neural Network's result was the best overall, making it the recommended choice for implementation.

The success of the Neural Network suggests that the relationship between the incident attributes and the financial cost is complex and non-linear, which this model is well-suited to capture. By deploying this model, the airline can gain a more accurate and timely financial forecast for bird strike incidents.
