# Predicting Negative Attitudes Toward Democracy  
### A Public Opinion Machine Learning Case Study (WVS Wave 7)

## Problem
How can individual-level perceptions and beliefs help explain negative attitudes toward democracy as a system of government?

This project analyzes survey data to identify which factors are most strongly associated with lower democratic support, with a focus on variables that remain relevant beyond short-term economic or political shocks.

## Data
The analysis uses data from the **World Values Survey – Wave 7 (v5.0)**, a large-scale international survey covering political, social, and economic attitudes.

- Observations: 1,003 respondents  
- Features: ~295 survey variables (ordinal and discrete)  
- Target variable: overall valuation of democracy as a system of government (Q250)

Missing values are encoded as negative values, following the original survey design.

## Approach
The workflow includes:
- Exploratory and descriptive analysis
- Handling of missing values and negative codes
- Feature selection based on correlation and substantive plausibility
- Train/test split
- Model comparison using multiple classifiers

The focus is not only on predictive performance, but also on interpretability and consistency with political theory, in order to avoid spurious correlations.

## Models
Several models were tested and compared, including:
- Logistic Regression
- Random Forest
- Gradient Boosting–based classifiers

Hyperparameter tuning was performed using cross-validation.

## Results
The analysis identifies a small set of attitudinal variables strongly associated with democratic support, including perceptions of elections, civil rights, corruption, and alternative forms of authority.

These results highlight how democratic legitimacy is closely linked to institutional trust and normative beliefs, rather than purely economic conditions.

## Limitations
- The analysis is cross-sectional and does not allow for causal inference.
- Results depend on survey self-reported data
- Findings should be interpreted as associations, not deterministic predictions.

## Structure
- `notebooks/01_wvs_democracy_attitudes.ipynb`: full analysis and modeling workflow


## References:
Haerpfer, C., Inglehart, R., Moreno, A., Welzel, C., Kizilova, K., Diez-Medrano J., M. Lagos, P. Norris, E. Ponarin & B. Puranen (eds.). 2022. World Values Survey: Round Seven - Country-Pooled Datafile Version 5.0. Madrid, Spain & Vienna, Austria: JD Systems Institute & WVSA Secretariat. doi:10.14281/18241.20
