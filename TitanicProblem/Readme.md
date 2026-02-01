# Titanic Survival Prediction (Kaggle)

## Problem Statement
The goal of this project is to predict whether a passenger survived the Titanic disaster based on available passenger information such as socio-economic class, gender, and family structure.

This is a **binary classification problem**, where:
- `1` → Survived  
- `0` → Did not survive

---

## Objective and Evaluation Focus
While Kaggle evaluates submissions using **accuracy**, this project intentionally focuses on **recall** for the *survived* class.

### Why Recall?
In this context, predicting a survivor as non-survived (false negative) is treated as more costly than predicting a non-survivor as survived (false positive).

Therefore, the objective was:
- To **reduce false negatives**
- Even if it meant accepting more false positives

This aligns with real-world scenarios where missing a positive case can be more harmful than raising false alarms.

---

## Modeling Approach
- Models used:
  - Random Forest Classifier
  - XGBoost (explored earlier)
- Validation strategy:
  - Stratified K-Fold Cross-Validation
- Feature set (kept intentionally simple):
  - `Pclass`
  - `Sex`
  - `SibSp`
  - `Parch`

Feature engineering was explored, but analysis showed that model performance plateaued, indicating limited additional signal in the available data.

---

## Threshold Tuning Strategy
By default, classifiers use a decision threshold of **0.5** to convert probabilities into class predictions.  
Instead of changing the model or adding potentially leaky features, the decision threshold was tuned to control the **recall–precision trade-off**.

### Rationale
- Lowering the threshold increases recall
- This allows the model to predict “Survived” more often
- Precision decreases as a trade-off

Thresholds evaluated:
- `0.3`
- `0.4`
- `0.5` (default)

---

## Cross-Validation Results (Recall vs Precision)

| Threshold | Mean Recall | Mean Precision |
|---------|------------|----------------|
| 0.3 | High | Low |
| 0.4 | Balanced | Balanced |
| 0.5 | Low | High |

Based on consistent performance across folds, a threshold of **0.4** was selected as it provided a meaningful increase in recall with an acceptable and stable drop in precision.

---

## Key Takeaways
- Improving model behaviour does not always improve Kaggle leaderboard accuracy
- Threshold tuning is a **policy decision**, not a model optimization trick
- Error analysis showed that remaining false negatives were not cleanly separable with available features
- This highlights the limits of the dataset rather than shortcomings of the model

---

## Conclusion
This project demonstrates a complete ML workflow:
- Proper cross-validation
- Error analysis
- Metric-driven decision making
- Honest trade-offs instead of leaderboard-driven hacks

The focus was on **learning correct ML engineering practices**, not maximizing Kaggle score.
