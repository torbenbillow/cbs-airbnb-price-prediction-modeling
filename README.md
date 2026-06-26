# Airbnb Price Prediction Modeling

## Overview

This project explores how machine learning can be used to estimate nightly Airbnb listing prices in Copenhagen, Denmark. The goal was to develop a regression-based price recommendation concept that could help Airbnb hosts set a more evidence-based starting price when creating or updating a listing.

The project was completed as part of the Applied Machine Learning course at Copenhagen Business School.

Rather than attempting to prescribe a final price, the model is intended to provide a benchmark based on observable listing characteristics such as location, capacity, amenities, host-related information, and property attributes.

## Business Problem

Airbnb hosts often need to set prices without historical booking data or detailed knowledge of local market conditions. This can lead to listings being underpriced, overpriced, or misaligned with comparable properties.

This project addresses the following question:

> Can machine learning models use observable Airbnb listing features to estimate a competitive nightly price for Copenhagen listings?

A practical version of this model could support hosts during the listing creation process by providing an initial price recommendation, while still allowing hosts to adjust the final price themselves.

## Dataset

The project uses a point-in-time extract of Airbnb listings in Copenhagen from Inside Airbnb.

* Source: Inside Airbnb
* Location: Copenhagen, Denmark
* Snapshot date: June 27, 2025
* Raw dataset size: 22,684 listings and 79 attributes
* Final cleaned dataset size: 7,567 listings and 40 attributes
* Target variable: `price`, representing nightly listing price in DKK

The dataset included listing-level information such as:

* Property type
* Room type
* Neighborhood
* Latitude and longitude
* Number of guests accommodated
* Bedrooms, beds, and bathrooms
* Amenities
* Host characteristics
* Review-related attributes
* Nightly price

## Methodology

The project followed a structured machine learning workflow:

1. Data loading and initial audit
2. Data cleaning and type correction
3. Feature engineering
4. Train-test split
5. Preprocessing with scikit-learn pipelines
6. Baseline model training
7. Hyperparameter tuning with GridSearchCV
8. Final evaluation on a holdout test set
9. Residual analysis and interpretation

## Data Cleaning and Feature Engineering

The raw dataset required substantial cleaning before modeling. Key steps included:

* Dropping irrelevant metadata columns
* Removing rows with missing target values
* Standardizing Boolean fields
* Cleaning and transforming price values
* Handling missing values carefully to avoid data leakage
* Creating missingness indicators for selected fields
* Engineering host tenure from the host signup date
* Transforming text fields into length-based proxy features
* Creating binary indicators for selected high-value amenities
* Creating hexagonal spatial bins from latitude and longitude to capture more granular location effects

Examples of engineered features include:

* `host_tenure_days`
* `description_length`
* `description_missing`
* `host_about_length`
* `host_about_missing`
* `neighborhood_overview_length`
* `neighborhood_overview_missing`
* `host_location_missing`
* Hex-bin location indicators

## Models Compared

The project compared several regression models to evaluate different assumptions, levels of complexity, and bias-variance trade-offs.

Models included:

* Linear Regression
* Ridge Regression
* Lasso Regression
* Decision Tree Regressor
* Random Forest Regressor
* Gradient Boosting Regressor
* K-Nearest Neighbors Regressor

Baseline models were first evaluated using cross-validation. Selected models were then tuned using `GridSearchCV`.

## Evaluation Metrics

Model performance was evaluated using:

* Mean Absolute Error (MAE)
* Root Mean Squared Error (RMSE)
* R² score

MAE was used as the primary evaluation metric because it is directly interpretable in Danish kroner and aligns well with the business use case of providing a practical pricing recommendation.

## Results

| Model               | Test MAE | Test RMSE | Test R² |
| ------------------- | -------: | --------: | ------: |
| Linear Regression   |    385.7 |     563.7 |    0.55 |
| Ridge Regression    |    377.0 |     554.5 |    0.57 |
| Lasso Regression    |    378.1 |     559.9 |    0.56 |
| Random Forest       |    345.2 |     524.6 |    0.61 |
| Gradient Boosting   |    348.3 |     541.8 |    0.59 |
| K-Nearest Neighbors |    374.8 |     601.7 |    0.49 |

The Random Forest model achieved the strongest overall test performance, with the lowest MAE and RMSE and the highest R² score. This suggests that non-linear ensemble models are better suited to modeling Airbnb prices than linear models in this dataset.

## Key Findings

The results suggest that Airbnb prices are shaped by complex, non-linear relationships between listing characteristics. Ensemble models performed better than linear models, likely because they can capture interactions between features such as location, capacity, amenities, and host-related variables.

The model performed well as a proof of concept for a pricing recommendation tool, but it should not be considered production-ready. A real-world implementation would require additional data, ongoing monitoring, and further validation against actual booking behavior.

## Limitations

Several limitations should be considered:

* The dataset is a single point-in-time snapshot.
* The target variable reflects host-listed prices, not actual booked prices.
* The dataset does not include occupancy, booking conversion, seasonality, or revenue outcomes.
* Some potentially valuable features, such as listing photos and full text descriptions, were excluded due to scope.
* Amenities were simplified to a smaller set of selected features.
* Some host-related features were excluded due to ethical concerns around bias and discrimination.
* Scraped secondary data may contain inconsistencies, outdated values, or self-reported inaccuracies.

## Ethical Considerations

Certain host-related attributes, such as names, profile photos, and other personal identifiers, may influence booking behavior due to bias or discrimination. These features were intentionally excluded from the model.

However, removing sensitive attributes does not guarantee that all bias is removed, since proxy variables may still reflect underlying patterns in the data. Any production version of this type of model would require additional fairness testing, monitoring, and governance.

## Technologies Used

* Python
* pandas
* NumPy
* scikit-learn
* GeoPandas
* Shapely
* Matplotlib
* Seaborn
* Folium
* Google Colab

## Repository Structure

```text
airbnb-price-prediction/
│
├── README.md
├── notebooks/
│   └── Airbnb_Price_Prediction_Modeling_Notebook.ipynb
│
├── report/
│   └── Airbnb_Price_Prediction_Modeling_Report.pdf
│
├── requirements.txt
└── .gitignore
```

## How to Run

1. Clone this repository:

```bash
git clone https://github.com/YOUR-USERNAME/airbnb-price-prediction.git
cd airbnb-price-prediction
```

2. Install required packages:

```bash
pip install -r requirements.txt
```

3. Open the notebook:

```bash
jupyter notebook notebooks/Airbnb_Price_Prediction_Modeling_Notebook.ipynb
```

Alternatively, open the notebook in Google Colab.

## Data Access

The original dataset was sourced from Inside Airbnb. The raw dataset is not included in this repository due to size and external data ownership considerations.

To reproduce the project, download the Copenhagen listings dataset from Inside Airbnb and update the file path in the notebook accordingly.

## What I Learned

This project strengthened my understanding of:

* End-to-end machine learning workflows
* Regression modeling
* Feature engineering
* Data leakage prevention
* Train-test splitting and preprocessing pipelines
* Cross-validation and hyperparameter tuning
* Model evaluation using MAE, RMSE, and R²
* The practical trade-offs between interpretability and predictive performance
* Ethical considerations in predictive modeling

## Academic Context

This project was completed as part of the Applied Machine Learning course at Copenhagen Business School.

The project should be viewed as a proof of concept for a machine learning-based pricing recommendation tool, not as a production-ready pricing system.
