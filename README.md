# Hospital Infection Control – Network Features and Logistic Regression

## Project overview

This project investigates whether information from a directed ward-transfer network improves prediction of a later simulated infection-cluster outcome.

The analysis compares two logistic regression models:

1. **Ward-attribute model** using:
   - `occupancy_ratio`
   - `device_use_share`

2. **Ward + network model** using the same two ward attributes plus:
   - `incoming_volume`
   - `from_active_volume`

Only wards without an active cluster at the observation point are included in the prediction task.

## Data

The project uses two synthetic teaching files:

- `hospital_wards.csv`
- `hospital_transfers.csv`

The ward file contains ward-level attributes, initial cluster status, and the later simulated target.  
The transfer file contains directed ward-to-ward links and transfer counts recorded before the prediction period.

The data are synthetic and are not real patient or hospital data.

## Workflow

The notebook:

1. Inspects the ward and transfer data.
2. Restricts the analysis to eligible wards.
3. Builds a directed weighted network.
4. Calculates:
   - total incoming transfer volume;
   - incoming transfer volume from wards already affected at the observation point.
5. Creates one stratified train/test split.
6. Fits scaling using the training data only.
7. Fits two logistic regression models on the same training observations.
8. Evaluates both models on the same test observations using:
   - Accuracy
   - F1 score
   - ROC AUC
   - Confusion matrix

## Main results

There were **102 eligible wards**, of which **39** had a later simulated cluster.

### Ward-attribute model
- Accuracy: **0.581**
- F1 score: **0.435**
- ROC AUC: **0.693**

### Ward + network model
- Accuracy: **0.839**
- F1 score: **0.737**
- ROC AUC: **0.904**

In this synthetic test split, the model that included network features performed better across all three summary metrics.

The baseline model produced **6 false positives** and **7 false negatives**.  
The network model produced **0 false positives** and **5 false negatives**.

## Interpretation

The results suggest that, in this synthetic dataset, earlier directed transfer information contains predictive information beyond occupancy ratio and device-use share.

However, this does **not** show that transfers caused infection clusters. A transfer link is not evidence of an infection route, and improved prediction does not make the model suitable for real clinical decision-making.

The dataset is small, synthetic, and idealised, so the results should be treated as a teaching demonstration rather than evidence about real hospitals.

## Reflection

The most useful part of the project was comparing two models on exactly the same train/test split. This made the effect of the added network features easier to interpret.

A key limitation is that the result depends on one small synthetic dataset and one held-out split. With more time, I would repeat the comparison across multiple splits or use cross-validation to see whether the improvement remains stable.

## Files

- `Option1_Hospital_Infection_Control_COMPLETED.ipynb` – completed notebook with code, outputs, plots, and written interpretations.
- `README.md` – project summary.
