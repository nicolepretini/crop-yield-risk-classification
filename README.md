# Crop Yield Risk Classification (Wheat)

## Project Overview
This project reframes crop yield modeling as a **risk classification problem**.
Instead of predicting exact yields, the goal is to identify **country–year
observations at risk of low wheat yield**, a framing that is more directly useful
for decision-making in agriculture, insurance, and policy contexts.

The project uses **real FAOSTAT country–year wheat yield data** and applies
strict validation strategies to avoid information leakage across countries.

---

## Data
- Source: FAOSTAT (Food and Agriculture Organization of the United Nations)
- Coverage:
  - 133 countries
  - 1961–2024
  - 7,137 country–year observations
- Variable used in this phase:
  - Wheat yield (kg/ha)

The dataset is public, large-scale, and widely used in agricultural analysis.

---

## Target Definition: Low-Yield Risk
A binary target was constructed to represent **low-yield risk**:

> A country–year is labeled as *low yield* if its yield falls in the **bottom
25% of that country’s historical yield distribution**.

This definition:
- avoids cross-country scale effects
- adapts to long-term technological trends
- produces a decision-relevant risk label

Overall class balance:
- ~74% normal yield
- ~26% low-yield risk

---

## Feature Engineering
Only **yield-derived temporal features** were used:

- Yield lag (t−1)
- Yield lag (t−2)
- Rolling mean of yield (previous 3 years)
- Calendar year (trend proxy)

No climate or exogenous variables are included at this stage, making the model
intentionally conservative and interpretable.

---

## Modeling Approach
Two classifiers were evaluated:

### Dummy Baseline
- Predicts the most frequent class (no low-yield risk)
- Serves as a strict baseline

### Logistic Regression
- Standardized inputs
- Interpretable linear decision boundary
- Suitable for imbalanced binary classification

---

## Validation Strategy
Model performance was evaluated using **GroupKFold cross-validation**, grouping
by country.

This ensures that:
- all observations from a given country appear in either train or test, never both
- performance reflects generalization to **previously unseen countries**

This is a much stricter and more realistic evaluation than random splits.

---

## Results

| Model | ROC-AUC | Precision | Recall | F1 |
|------|--------|-----------|--------|----|
| Logistic Regression | ~0.79 | ~0.65 | ~0.38 | ~0.48 |
| Dummy (Most Frequent) | 0.50 | 0.00 | 0.00 | 0.00 |

Key observations:
- The baseline classifier fails to identify any low-yield events.
- Logistic regression shows strong discriminatory power under country-level
  cross-validation.
- Precision–recall trade-offs indicate a conservative model that prioritizes
  reliability of risk alerts.

---

## Key Takeaways
- Yield history alone contains substantial predictive signal for low-yield risk
- Group-aware validation is essential in country–year panel data
- Simple, interpretable models can perform well under strict evaluation
- Risk framing is often more actionable than point prediction

---

## Limitations
- No climate or management variables included
- Structural breaks and extreme events are not explicitly modeled
- Risk threshold selection is application-dependent

---

## Future Work
- Incorporate climate anomalies and weather extremes
- Evaluate alternative classifiers (tree-based, calibrated models)
- Explore region-specific risk models
- Add cost-sensitive decision thresholds

---

## Project Structure
