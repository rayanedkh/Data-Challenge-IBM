# IBM Hackathon
## Dataset Overview

For this data challenge, we initially received three datasets in CSV format:
- *cards_data.csv*, containing information related to cards (type, issue date, limits, etc.)
- *users_data.csv*, containing user attributes (age, gender, location, status)
- *transactions_train.csv*, listing the operations performed (amount, date, payment method, merchant)

Each of these files included complementary columns, so we merged them using the common keys (Card ID and User ID) to create a single unified dataset ready for analysis and preprocessing.

## Fraud Labeling

Fraud labels (fraud/no-fraud) were provided in a separate JSON file named *train_fraud_labels.json*.
This file maps each transaction ID to its fraud label, allowing us to supervise the training of the detection models.

# Performance Summary of the LGBM Model – Fraud Detection

**Number of evaluated instances**: 21,000  
**Algorithm**: LightGBM (LGBMClassifier)  
**Number of features**: 46  
**Target column**: `target` (1 = fraud, 0 = non-fraud)

---

## Feature Engineering

- Removal of unnecessary columns such as **cvv** (confidentiality risk) and other low-information attributes
- Use of **One-Hot Encoding** for categorical variables, turning each category into a distinct binary column

---

## Evaluation Scores

| **Metric**                | **Holdout** | **Cross-Validation** |
|--------------------------|-------------|---------------------|
| **Accuracy**             | 0.999       | 0.999               |
| **AUC-ROC**              | 0.996       | 0.985               |
| **Precision**            | 0.767       | 0.687               |
| **Recall**               | 0.742       | 0.743               |
| **F1-Score**             | 0.754       | 0.711               |
| **Average Precision (AP)** | 0.786     | 0.749               |
| **Log Loss**             | 0.003       | 0.004               |

---

## Confusion Matrix

| Observed       | Predicted 1 | Predicted 0 | % Correct |
|---------------|-------------|-------------|----------|
| 1 (Fraud)     | 23          | 8           | 74.2%    |
| 0 (Non-fraud) | 7           | 20,962      | 100.0%   |

---

## Interpretation of Results

- **High precision**: The model achieves 76.7% precision for the fraud class, indicating a strong ability to identify fraudulent transactions.
- **Solid recall**: With a recall of 74.2%, the model successfully detects a significant proportion of actual fraud cases.
- **High AUC-ROC**: A holdout AUC-ROC score of 0.996 suggests excellent class discrimination capability.
- **Low Log Loss**: A logarithmic loss of 0.003 indicates that the model is well-calibrated and confident in its predictions.

---

## Recommendations

- **Error analysis**: Review false positives and false negatives to identify patterns and improve model robustness.
- **Threshold adjustment**: Consider adjusting the decision threshold depending on risk tolerance to further balance precision and recall.
- **Continuous monitoring**: Set up ongoing monitoring to detect performance drift or changes in incoming data behavior.

[LightGBM Model evaluation metrics - GeeksforGeeks](https://www.geeksforgeeks.org/lightgbm-model-evaluation-metrics/)
