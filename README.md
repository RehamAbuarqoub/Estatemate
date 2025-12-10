# House Price Prediction – AI for Business Decisions and Transformation

This project details the development and validation of a machine learning system designed to estimate house prices. It was built as part of the **Artificial Intelligence for Business Decisions and Transformation** course, demonstrating the use of modern data science to enhance traditional real estate and financial processes.

## Project Overview

Accurate house price prediction is a necessity for all market participants. Our objective was to construct an AI model that delivers **reliable price estimates** based on core property attributes.

## Dataset

The dataset utilizes a comprehensive collection of traditional real estate features derived from property transaction records:
`price` (the target variable), `sqft_living`, `sqft_lot`, `bedrooms`, `bathrooms`, `floors` (`stories`), `yr_built`, `yr_renovated`, and high-impact locational and quality factors like `waterfront`, `view`, `condition`, `grade`, `zipcode`, `lat`, and `long`.

## Feature Explanations (Input to Model 1)

The valuation model (Model 1) relies on the following key features, grouped by their impact on traditional valuation principles:

### I. Core Property Attributes (The Traditional Standards)

| Feature | Description | Valuation Impact |
| :--- | :--- | :--- |
| `price` | **Target Variable:** The final sale price of the house. | N/A |
| `sqft_living` | **Square Footage of Living Space.** The interior area of the home. | The most significant traditional driver of value. |
| `bedrooms` | The total number of bedrooms. | Primary factor in utility and family accommodation. |
| `bathrooms` | The total number of bathrooms (including half-baths). | Key utility feature, highly correlated with sale price. |
| `floors` | The number of stories/levels in the house. | Influences floor plan and build complexity. |
| `sqft_lot` | The total area of the property lot (land). | Value of the underlying land. |
| `yr_built` | The year the house was originally constructed. | Impacts depreciation and need for maintenance. |

### II. Quality and Condition Factors (Perceived Value)

| Feature | Description | Valuation Impact |
| :--- | :--- | :--- |
| `grade` | **Quality of Construction and Design (1-13).** 1 being low, 13 being best. | A core factor reflecting finish quality and materials. |
| `condition` | **Condition of the Home (1-5).** 5 is excellent, 1 is poor. | Reflects current maintenance status. |
| `view` | Quality of the view (e.g., mountains, city, none). | Significant driver of aesthetic and perceived value. |
| `yr_renovated` | The year the house was last significantly renovated. | Indicates recent capital expenditure and modernization. |

### III. Locational and Premium Factors (The Highest Impact Drivers)

| Feature | Description | Valuation Impact |
| :--- | :--- | :--- |
| `zipcode` | The 5-digit postal code of the property. | **Crucial:** Acts as a proxy for neighborhood quality, school district, and local market demand. |
| `lat` / `long` | Latitude and Longitude coordinates. | Precise geographical location, highly important in micro-markets (e.g., proximity to water/downtown). |
| `waterfront` | **Boolean (0 or 1):** Whether the property borders a body of water. | **One of the strongest premium value drivers.** |

### IV. Engineered Features (Forward-Thinking Insights)

| Feature | Description | Valuation Impact |
| :--- | :--- | :--- |
| `house_age` | Calculated as the difference between the current year and `yr_built`. | Direct measure of physical obsolescence and depreciation. |
| `sqft_per_room` | Ratio of `sqft_living` to `bedrooms`. | Proxy for the *spaciousness* of the rooms, capturing modern preference for larger individual spaces. |

## Preprocessing

Preprocessing involved standard, reliable steps critical for model performance:
* **Feature Engineering:** Creation of features like `house_age` and `sqft_per_room`.
* **One-hot encoding** for categorical variables (e.g., `zipcode`).
* **Standardization** of numerical features.
* **Train/Validation/Test Split:** A robust 80% / 10% / 10% split was used for development and final validation.

## Models Trained

We tested three different regression models to establish performance benchmarks:
* Linear Regression (Baseline)
* Random Forest Regressor
* Gradient Boosting Regressor

The final system relies on a **Stacked Ensemble Model** (`final_model_stack`). This technique combines the predictions of the individual regressors (including LightGBM and XGBoost components) to achieve the highest possible accuracy, a standard practice for reducing overall error. 

## Model Evaluation

Models were assessed on the dedicated test set using the following key regression metrics:
* **RMSE (Root Mean Squared Error):** This was prioritized as it heavily penalizes large errors—a critical requirement for financial applications where high-cost mistakes must be avoided.
* **MAE (Mean Absolute Error):** Provides the average dollar magnitude of the prediction error.
* **R² (Coefficient of Determination):** Measures the model's overall explanatory power.



## Final Results: Stacked Ensemble Model (Model 1)

The final model used was the **Stacked Ensemble Regressor**, which achieved the following superior results on the test data:

| Metric | Result | Interpretation |
| :--- | :--- | :--- |
| **Test R² Score (Reliability)** | **0.8683** | $\mathbf{86.83\%}$ of the price variance is successfully captured by the features. |
| **Test MAE (Average Error)** | **$70,433.89** | The average magnitude of prediction error. |
| **Test RMSE (Penalty for Large Errors)** | **$141,109.33** | Key metric for managing risk from large estimation errors. |
| **Test MAPE (Percentage Error)** | $\mathbf{12.87\%}$ | The average percentage error. |

**Key Feature Insight:** The model validates the long-standing traditional drivers of home value: the strongest features impacting price were confirmed to be **`sqft_living`**, followed by premium attributes like **`waterfront`** and high **`grade`** scores, and localized factors derived from **`zipcode`**.

## Loan Decision System Validation

The high-accuracy valuation model (Model 1) is integrated with a **Rule-Based Affordability Model (Model 2)** to provide loan decision support based on traditional financial limits (28%/36% DTI/HER).

| Scenario | Input | Result |
| :--- | :--- | :--- |
| **Model 1 (Valuation) Price** | N/A | **$409,177** |
| **Applicant HER** | 14.00% | Well below the 28% limit. |
| **Applicant DTI** | 20.00% | Well below the 36% limit. |
| **Final Decision** | N/A | **APPROVE: Low Risk - Meets Standards** |
