# Experiment Summary

This document summarizes the public-safe modeling evidence for the LG AImers 5th manufacturing quality prediction repo. It is written as a reviewer guide, not as a claim of a final private leaderboard score.

## Main Notebook

The main inspection artifact is [`../notebooks/final_modeling.ipynb`](../notebooks/final_modeling.ipynb). It contains the end-to-end flow used for:

1. Loading the official competition CSV files from a local `data/` directory.
2. Auditing table shape, target distribution, and submission columns.
3. Cleaning missing, constant, duplicated, and low-signal columns.
4. Encoding categorical manufacturing-process fields.
5. Creating process-aware derived features.
6. Running stratified validation.
7. Comparing tree-based models and sampling strategies.
8. Generating `Normal` / `AbNormal` submission labels.

## Data and Target

The task is binary classification for manufacturing quality:

- `Normal`
- `AbNormal`

The public README records the original working scale as roughly 40,000 training rows and more than 460 columns. The competition data itself is not included in the repository, so reviewers should treat notebook outputs as inspectable experiment history unless they have authorized access to the original data.

## Preprocessing

The preprocessing work focuses on reducing avoidable noise before model training:

- Drop all-missing columns.
- Drop constant columns.
- Remove redundant or unhelpful judgment columns.
- Separate submission identifiers from trainable features.
- Encode categorical process columns for tree-model training.

This keeps the modeling task closer to manufacturing signal extraction and reduces the risk that a model simply memorizes repeated administrative fields.

## Feature Engineering

The feature engineering experiments inspect repeated and highly correlated process measurements. Two derived feature styles are used:

- `SUM`: summarizes related repeated measurements as a total process-intensity signal.
- `DELTA`: captures differences between related measurements that may indicate abnormal variation.

Supporting notebooks:

- [`../notebooks/experiments/process_analysis_experiment.ipynb`](../notebooks/experiments/process_analysis_experiment.ipynb)
- [`../notebooks/experiments/feature_importance_experiment.ipynb`](../notebooks/experiments/feature_importance_experiment.ipynb)

## Validation and Metric

The notebook uses stratified K-fold validation to preserve the minority `AbNormal` ratio across folds. The reported local validation fields include:

- F1
- precision
- recall
- accuracy

F1 is the main comparison metric because the class distribution is imbalanced and abnormal-case detection matters more than overall accuracy.

## Imbalance Handling

The experiments compare:

- No additional sampling baseline.
- Undersampled normal-class training sets at several ratios.
- `RandomOverSampler` for the minority class.

The evidence to inspect is the notebook validation tables, especially the precision/recall/F1 tradeoff for each sampling setup. The docs do not promote a single threshold or sampling ratio as production-ready because the hidden test distribution is not available in this public repo.

## Model Candidates

The public notebook surface includes tree-based tabular classifiers:

- RandomForest
- XGBoost
- LightGBM
- CatBoost

CatBoost appears as a primary candidate in the final notebook because it is practical for mixed tabular data and rapid competition iteration. XGBoost and LightGBM are included as comparable gradient-boosted baselines.

## Result Evidence

Public-safe evidence:

- Main notebook validation tables.
- Experiment notebooks for process analysis, feature importance, and latest score exploration.
- Final presentation file under `assets/`.
- Local QA evidence under `.omo/evidence/practitioner-github-portfolio/` in the workspace.

Not claimed:

- No public leaderboard rank.
- No final private score.
- No production quality guarantee.

## Reproducibility Boundary

Full reproduction requires the official LG AImers CSV files. Public reviewers without those files can inspect:

- Feature logic.
- Validation design.
- Model and sampling choices.
- Notebook outputs already stored in the repo.
- The public data policy in [`reproducibility.md`](reproducibility.md).

## Limitations

- The raw data is competition-restricted and excluded.
- Notebook outputs can become stale if dependency versions or random seeds change.
- Local validation may not match hidden leaderboard behavior.
- Certificate and raw Drive materials require separate publication review.
- The repo demonstrates competition modeling practice, not a monitored manufacturing deployment.
