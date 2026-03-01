# Predicting Attitudes Toward Democracy
## A Machine Learning Case Study Using World Values Survey Data (Argentina, Wave 7)

What distinguishes people who strongly value democracy from those who do not? This project uses survey microdata and a supervised classification pipeline to find out — and to do so in a way that is reproducible, interpretable, and honest about its limitations.

---

## The Question

The World Values Survey asks respondents to rate how important democracy is as a system of government, on a scale from 1 to 10. In the Argentine sample of Wave 7, the distribution is heavily skewed toward the high end: most people say democracy matters. But a meaningful minority does not — and the question is whether that difference can be predicted from other attitudes measured in the same survey.

This is not a causal claim. The analysis identifies associations, not mechanisms. The goal is to build a clean, well-documented pipeline that extracts signal from a high-dimensional survey dataset and produces interpretable results.

---

## Data

World Values Survey — Wave 7, Argentina (v5.0)

- 1,003 respondents
- ~295 variables after preprocessing (originally 395, reduced by dropping empty columns and encoding missing values)
- Target variable: Q250 — "How important is it for you to live in a country that is governed democratically?" (1–10 scale, binarized at threshold > 8)

Missing values in the original dataset are encoded as negative integers, following WVS survey design conventions. The preprocessing pipeline handles these before any modeling step.

---

## Approach

The workflow covers the full cycle from raw data to explainability:

**Exploratory Data Analysis** — univariate distributions, bivariate box plots, and a Spearman correlation matrix against the target variable, focused on a curated set of theoretically relevant variables (military rule attitudes, institutional trust, gender equality, government intrusion, corruption perception).

**Preprocessing pipeline** — negative-to-NaN conversion, mode imputation, standard scaling, all implemented inside a scikit-learn Pipeline to prevent data leakage across cross-validation folds.

**Feature selection** — SelectKBest with F-test scoring against the binarized target. Note: this differs from Spearman correlation used in the EDA, which is why some variables prominent in the EDA (e.g., Trust in free elections) do not appear among the final selected features.

**Model comparison** — GridSearchCV over four classifiers (Logistic Regression, Random Forest, XGBoost, CatBoost) with stratified 5-fold cross-validation, optimizing for F1-score given the class imbalance (~2:1 in favor of the high-valuation class).

**Explainability** — SHAP values via LinearExplainer, with a bar plot for global importance, a beeswarm plot for directionality, and three individual force plots.

---

## Results

Logistic Regression achieved the best cross-validated F1-score among the four models. This is consistent with the relatively small sample size and the near-linear separability that tends to emerge when a threshold is applied to a single ordinal target item.

| Metric | Value |
|---|---|
| Accuracy | ~77% |
| F1-score (class 1) | ~0.85 |
| F1-score (class 0) | ~0.56 |
| ROC-AUC | 0.792 |

The six features selected by the model span political system preferences (attitudes toward military rule and technocracy), views on what constitutes democracy (religious law, welfare), and moral attitudes (tax evasion, euthanasia).

The strongest predictor by absolute coefficient is Army_Rule (Q237) — attitude toward military government. Its dominant role is consistent with the EDA and with political science literature on authoritarian versus democratic value orientations.

---

## A Note on Interpretability

Individual coefficients in the final Logistic Regression model are not straightforwardly interpretable in isolation. Several features show signs in the multivariate model that differ from their univariate correlation with the target — a known consequence of statistical suppression, where correlated predictors redistribute variance in ways that can invert individual coefficients while leaving the model's overall predictions intact.

SHAP values address this by computing each feature's marginal contribution to each prediction, holding the others constant. The beeswarm and force plots in the notebook reflect this multivariate structure directly, and the interpretation section discusses which directions are intuitive, which are suppression artifacts, and which are genuinely fragile signals specific to this sample.

---

## Limitations

- Cross-sectional observational data. No causal inference is possible.
- Single-country sample (Argentina). Findings may not generalize across cultural contexts.
- Social desirability bias is plausible in items about democratic values.
- The binarization threshold (score > 8) is a deliberate modeling choice, not a universal definition of "high democratic valuation." A different threshold produces a different model.
- SelectKBest feature selection is data-driven and may retain variables correlated with the target for sample-specific reasons that do not replicate.

---

## Structure

```
notebooks/
    01_wvs_democracy_attitudes.ipynb   — full analysis, modeling, and explainability
data/
    WVS_FINAL_LIMPIO_7_v5.0.csv        — preprocessed Argentine sample
```

---

## Reference

Haerpfer, C., Inglehart, R., Moreno, A., Welzel, C., Kizilova, K., Diez-Medrano, J., Lagos, M., Norris, P., Ponarin, E. & Puranen, B. (eds.). 2022. *World Values Survey: Round Seven — Country-Pooled Datafile Version 5.0*. Madrid & Vienna: JD Systems Institute & WVSA Secretariat. doi:10.14281/18241.20
