# Lithium-ion Battery Project (all Li-ion work in one place)

This folder holds the three lithium-ion projects of the MTP, plus a combined step-by-step report with a
comparison and the final results.

## Start here
**[`Lithium_ion_Full_Report/Lithium_ion_Full_Report.pdf`](Lithium_ion_Full_Report/Lithium_ion_Full_Report.pdf)** is the
complete step-by-step report (data → cleaning → features → models → testing → results → comparison → final
result). The same report is also available as a runnable notebook, `Lithium_ion_Full_Report.ipynb`, and every
result is collected in one spreadsheet, `Lithium_ion_All_Results.csv`.

## What is in this folder

| Folder | Project | Data | Approach | Paper replicated |
|---|---|---|---|---|
| [`Sodium_ion_new/`](Sodium_ion_new) | **1. dQ/dV features + 8 ML models** | Tongji, 130 cells | Machine learning | Qian et al. 2026, *ACS Appl. Energy Mater.* |
| [`Lithum_ion/`](Lithum_ion) | **2. BiTCN-MHA-BiGRU hybrid network** | NASA, 4 cells | Deep learning | Liu et al. 2025, *J. Energy Eng.* |
| [`Lithium_ion_Tongji/`](Lithium_ion_Tongji) | **3. The same network on 130 cells** | Tongji, 130 cells | Deep learning | Liu et al. 2025, transferred |
| [`Lithium_ion_Full_Report/`](Lithium_ion_Full_Report) | Combined report, notebook, all-results CSV | all of the above | | |

Two naming notes:
- **`Sodium_ion_new`** is named for sodium-ion, but it is mainly the Li-ion ML project. It also contains the first
  Na-ion experiment and the Tongji raw data (5.2 GB), which `Lithium_ion_Tongji` reads through a link. A duplicate
  of it sits in `../Sodium_ion_Project/Sodium_ion_new`, which holds the Na-ion raw data (linked back here).
- **`Lithum_ion`** (spelling as originally created) is the Phase 1 project and the only one under git.

## Data

| Data set | Cells | Cycles | Chemistry | Conditions | Reach 80% / 70% SOH |
|---|---|---|---|---|---|
| NASA | 4 (B0005, B0006, B0007, B0018) | 636 | 18650, 2.0 Ah | 24 °C, one protocol | 4 / 3 |
| Tongji | 130 | 59,007 after cleaning (59,683 raw) | NCA & NCM (3.5 Ah), NCM+NCA (2.5 Ah) | 25/35/45 °C, 11 charge/discharge rates | 100 / 9 |

## Comparison of the three projects

| | 1. ML (Tongji) | 2. DL (NASA) | 3. DL (Tongji) |
|---|---|---|---|
| Input | one partial charging curve | 16 past cycles | 16 past cycles |
| Best SOH model | Gradient Boosting / LightGBM / XGBoost, R² 0.994–0.995 | BiGRU, R² 0.975 | BiGRU, R² 0.999 |
| Simple baseline (SOH) | — | persistence, R² 0.981 (beats every network) | linear regression, R² 1.000 (beats every network) |
| Best life (RUL) prediction | **Gradient Boosting, 9.2 cycles** (median) | hybrid, 28 cycles = 68% of remaining life (genuine forecast) | hybrid, 66 cycles = 50% (genuine forecast, 80% EOL) |
| Paper's claim holds? | SOH yes, RUL no | method yes, ranking no | same as NASA |

## Final result

- **Best data set:** Tongji. With 130 cells, every model's error is 20–30× lower than on NASA's 4 cells.
- **Best SOH from one partial charge:** the tree models (Gradient Boosting median R² 0.995, LightGBM RMSE 0.62%).
- **Best life (RUL) prediction:** Gradient Boosting / XGBoost (ML), median error **9–10 cycles** at 80% end of life.
- **Deep learning:** the BiTCN-MHA-BiGRU hybrid was never the best model. A plain BiGRU beat it, simple baselines
  beat every network, and its genuine life forecasts were 38–77% off.
- **Overall winner:** machine learning (Project 1, `Sodium_ion_new`) on the Tongji data.

## Rebuild the report

```bash
python3 /Users/vikram/MTP/MTP_Final_Report_2026-09-24/build_battery_reports.py li
```

It re-reads every project's result files, re-runs the notebook, and writes the PDF and the CSV again.

# MTP_Li
