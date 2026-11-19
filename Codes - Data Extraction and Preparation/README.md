# Data Extraction and Preparation

Six notebooks build the modelling inputs for the MSc Business Analytics dissertation on Polymarket and next-common-period relative asset performance.

## Run order

| Notebook | Input | Output |
| --- | --- | --- |
| 01 Polymarket data extraction | Polymarket Gamma and CLOB APIs | `Polymarket_Dissertation_All_Markets_FULL_History.xlsx` |
| 02 Yahoo Finance data extraction | Yahoo Finance via yfinance | `Final_Yahoo_Finance_BTC_Gold_SP500_Raw.xlsx` |
| 03 Base dataset preparation | Both raw price workbooks | `Final_Competitive_Markets_BASE_Modeling_Dataset.xlsx` |
| 04 No-leak feature engineering | Base workbook and both raw price workbooks | `Final_Competitive_Markets_MODEL_READY_Dataset.xlsx` |
| 05 Trading activity extraction | Model-ready workbook and Polymarket trade API | `Polymarket_Selected_Markets_Historical_Trades.csv.gz` and an intermediate trading activity workbook |
| 06 Corrected trading features | Model-ready workbook, trade CSV and optional archived corrected workbook | `Replicated_CORRECTED_PM_Trading_Features.xlsx` |

## Running in Colab

- Use the original historical data snapshots for the dissertation replication. The first notebook originally ran on 11 August 2026 at 14:39:18 UTC; its current `now_utc` setting otherwise downloads later observations. To reproduce the historical extraction cutoff, replace it with `pd.Timestamp("2026-08-11 14:39:18", tz="UTC")`. API availability and historical revisions can still differ.
- Notebook 01 writes its output in `/content/polymarket_dissertation_all_markets/`. Notebooks 03 and 04 expect `/content/Final Polymarkets datasets.xlsx` (sheet `All Daily Wide`); copy/rename the extracted workbook or use the archived snapshot under this expected name. Notebook 02 uses 1 June 2025 through 11 August 2026 inclusive.
- Place the required input workbooks in `/content` in each Colab session. Notebook 05 exports the full cleaned trade CSV; its earlier alternative-mapped features are intermediate and **must not be used as the final modelling inputs**.
- Notebook 06 may run in raw mode using the archived transaction CSV, or checkpoint mode using the archived corrected workbook. In checkpoint mode it recalculates lagged and rolling features from saved daily aggregates; it does not independently reconstruct the aggregates from raw transactions. Place the files in the working directory or set `DATA` appropriately.
- The final modelling notebooks expect exactly one workbook whose filename contains both `MODEL_READY` and `CORRECTED_PM_Trading_Features`, with sheet `Model_Ready_Master`. After validating a regenerated workbook, copy it to `Final_Competitive_Markets_MODEL_READY_with_CORRECTED_PM_Trading_Features.xlsx` in a separate modelling session; do not overwrite the archived original or upload both copies into the same session.

## Integrity checks

The original saved base-preparation output contains 434 events. After complete-event filtering, the model-ready sample contains 428 events and 1,012 alternative-level rows (121, 151 and 156 events by contract). The saved original trading extraction reports 14,054 cleaned trades, no failed windows and 9,711 records mapped in the initial specification. Notebook 06 supersedes that initial mapping using both YES and NO trades.

For this portfolio, notebook 06 was recreated from the dissertation methodology and archived data. A fresh checkpoint run matched all 51 daily activity columns (1,585 rows) and all 41 model-ready columns (1,012 rows) against the archived workbook, within the notebook's numerical tolerance. The raw transaction aggregation and live extraction were not rerun in this check. All six notebooks passed notebook-format and Python syntax validation.

The first five notebooks retain the original code; notebook 06 retains the recreated implementation. Saved execution outputs have been removed for a cleaner repository and to avoid exposing wallet addresses in notebook previews. The archived raw data and results should be retained separately, subject to source terms and university requirements.
