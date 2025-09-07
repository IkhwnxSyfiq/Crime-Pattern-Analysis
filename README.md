# Crime Pattern Analysis & Prediction

## Project Overview

This project analyses and models crime data from a major city (2014-2017) to identify patterns and predict the type of Major Crime Incident (MCI) based on temporal and geographical features. Using machine learning classification techniques, the goal is to build a model that can accurately categorise crimes, which could aid in resource allocation and preventive policing strategies.

**Key Questions:**
- Can we predict the type of crime based on when and where it occurred?
- What are the temporal trends for different types of major crimes?
- How do different machine learning models perform on this predictive task?

## Dataset

The analysis is based on the `MCI_2014_to_2017.csv` dataset, which contains records of Major Crime Incidents.

**Relevant Features Used:**
- `occurrenceyear`, `occurrencemonth`, `occurrenceday`, `occurrencehour`
- `occurrencedayofyear`, `occurrencedayofweek`
- `MCI` (Target Variable): The type of major crime.
    - `Assault`
    - `Auto Theft`
    - `Break and Enter`
    - `Robbery`
    - `Theft Over`
- `Division`, `Hood_ID` (Geographical identifiers)

## Methodology

### 1. Data Preprocessing
- **Filtering:** Data was filtered to include only records from 2014 onwards.
- **Encoding:** Categorical variables (like `MCI`, `Division`) were factorised into numerical labels for model ingestion.
- **Train-Test Split:** The data was split into a 75% training set and a 25% testing set.
- **Feature Encoding:** Two encoding strategies were tested:
    - **Label Encoding:** Simple numerical conversion of categories.
    - **One-Hot Encoding:** Creating binary columns for each category level.

### 2. Modeling
Several classification models were implemented and evaluated:

- **Random Forest Classifier:** The primary model used, chosen for its robustness and ability to handle complex relationships.
    - Tested with both Label Encoded and One-Hot Encoded features.
    - Tested with and without `class_weight='balanced'` to handle the imbalanced dataset.
- **Gradient Boosting Classifier:** Evaluated but performed significantly worse than Random Forest for this task.

### 3. Evaluation
Models were evaluated based on:
- **Accuracy:** Overall correctness of predictions.
- **Confusion Matrix:** Detailed breakdown of correct and incorrect predictions per class.
- **Classification Report:** Precision, Recall, and F1-score for each crime type.

## Results

### Model Performance Summary
| Model | Encoding | Class Weighting | Accuracy | Key Findings |
| :--- | :--- | :--- | :--- | :--- |
| Random Forest | Label | None | 57.8% | Good at predicting Assault, poor on minority classes. |
| Random Forest | One-Hot | None | **59.1%** | **Best overall model.** Modest improvement over label encoding. |
| Random Forest | Label | Balanced | 57.5% | Slightly worse accuracy, no significant gain in minority class prediction. |
| Gradient Boosting | One-Hot | N/A | 54.4% | Poor performance. Default parameters predicted only the majority class. |

### Key Findings
1.  **Class Imbalance:** The dataset is highly imbalanced. "Assault" is the majority class, which the models learn to predict well, while minority classes like "Theft Over" and "Auto Theft" have very low recall and precision.
2.  **Best Model:** The **Random Forest model with One-Hot Encoding** achieved the highest accuracy (~59.1%).
3.  **Performance Breakdown:** The model is effective at predicting frequent crimes (e.g., Assault) but struggles with rare events. The F1-score for "Theft Over" is very low (0.03), meaning it is rarely correctly identified.

### Visualization
The project includes a stacked bar chart visualisation showing the distribution of different crime types by year, highlighting trends and the inherent class imbalance.
![Pattern of Crime by Year](crime_trends_plot.png) *(Note: Replace with the actual path or description of your saved plot)*

### Conclusions and Recommendations
Conclusion: While the model can predict the most common crime type with reasonable accuracy, the severe class imbalance makes it unreliable for predicting specific, less frequent crimes based solely on the provided temporal and geographical features.

Next Steps:

Address Imbalance: Use techniques like SMOTE (Synthetic Minority Over-sampling Technique) or ADASYN to generate synthetic samples for the minority classes.

Advanced Models: Experiment with more sophisticated models like XGBoost or LightGBM, and perform rigorous hyperparameter tuning.

Feature Engineering: Create new features, such as "is_weekend", "season", "time_of_day_bins", or "crime_rate_in_hood", to provide more signal to the models.

Alternative Metrics: Focus on precision-recall curves and the F1-score for the minority classes instead of overall accuracy.

Spatial Analysis: Incorporate more detailed geographical data or use clustering algorithms to identify crime hotspots.
