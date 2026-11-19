# Modelling and evaluation

These five notebooks contain the modelling and evaluation code for my MSc Business Analytics dissertation, *Does Polymarket Add Predictive Value Beyond Recent Returns?*

The target is the next-common-financial-period winner within each competitive event: Bitcoin vs Gold, or Bitcoin vs Gold vs the S&P 500. The baseline uses three lagged financial returns. The enhanced specifications retain those returns and add lagged Polymarket probabilities and trading-activity features.

## Execution order

| Notebook | Purpose | Additional input | Output workbook |
|---|---|---|---|
| `01_Conditional_Logit.ipynb` | Pooled conditional logit, chronological validation, holdout and statistical comparisons | None | `Conditional_Logit_Final_Evaluation_All_Results.xlsx` |
| `02_Traditional_ML.ipynb` | Pooled logistic regression, random forest and XGBoost | None | `Traditional_ML_Final_Evaluation_All_Results.xlsx` |
| `03_Market_Specific_Robustness.ipynb` | Refit and evaluate all four models within each contract | None | `Market_Specific_Robustness_All_Models_Results.xlsx` |
| `04_Polymarket_Feature_Ablation.ipynb` | Compare individual Polymarket information channels using the frozen baseline ML settings | Market-specific results workbook (`Best_ML_Params` sheet) | `Polymarket_Feature_Ablation_RQ2_All_Models_Results.xlsx` |
| `05_Time_to_Resolution.ipynb` | Analyse forecasting gains across contract maturity and compare simple benchmarks | Market-specific results workbook (`Best_ML_Params` and `Final_Event_Predictions` sheets) | `Time_to_Resolution_CORRECTED_with_Benchmarks_Results.xlsx` |

All notebooks require the corrected dataset:

`Final_Competitive_Markets_MODEL_READY_with_CORRECTED_PM_Trading_Features.xlsx`

The `Model_Ready_Master` sheet contains 1,012 alternative-level observations across 428 competitive events and 273 dates.

## Running in Google Colab

1. Open a notebook in Colab and start a fresh runtime.
2. Upload the corrected dataset from the repository's Data folder to `/content/`.
3. For notebooks 04 and 05, also upload the archived market-specific results workbook from the Results folder (or the output of notebook 03).
4. Run the cells in order. The notebooks include package installation cells and export results to Excel.
5. Keep only one matching copy of each input workbook in `/content/`, since the notebooks locate files using filename patterns.

Notebooks 01–03 can be run independently; run 03 before 04 and 05 if regenerating the input results workbook.

## Dependencies

Python 3, NumPy, pandas, SciPy, scikit-learn, statsmodels, XGBoost, openpyxl and matplotlib. The original notebooks use Colab upload/download helpers.

## Research records and reproducibility

The modelling code is preserved from the supplied dissertation notebooks. Saved cell outputs have been cleared because some were from earlier runs; the archived Excel result workbooks and dissertation are the reference for reported results.

A subsequent check with the archived dataset reproduced the pooled Conditional Logit holdout metrics and the Logistic Regression and Random Forest holdout metrics using the archived selected hyperparameters. XGBoost holdout results differed in the checking environment (XGBoost 3.1.3); the exact original package environment has not been recovered. The complete tuning and robustness pipeline has not been rerun as part of this check. For exact numerical replication, package versions and execution settings should be recorded and compared with the original runtime.
