# Reproducibility

This repo supports inspection-first reproducibility. Full execution is available only to users who already have authorized access to the LG AImers 5th competition data.

## Environment

Recommended baseline:

- Python 3.9 or newer
- Jupyter Notebook or JupyterLab

Install dependencies:

```bash
pip install -r requirements.txt
```

## Data Placement

The raw competition CSV files are intentionally excluded from git. To run the notebook locally, place the official files here:

```text
data/
  train.csv
  test.csv
  submission.csv
```

Do not commit the `data/` directory or generated submission files.

## Run Order

1. Install dependencies from `requirements.txt`.
2. Place authorized competition CSVs under `data/`.
3. Open [`../notebooks/final_modeling.ipynb`](../notebooks/final_modeling.ipynb).
4. Run the import and data-audit cells.
5. Run preprocessing and feature engineering cells.
6. Run validation cells and inspect F1, precision, recall, and accuracy.
7. Run submission-generation cells only in a local ignored output path.

## What Can Be Reproduced Publicly

Without private competition data, public reviewers can still inspect:

- Notebook source and stored outputs.
- Data cleaning decisions.
- Feature engineering logic.
- Imbalance handling experiments.
- Stratified validation design.
- Model candidates and dependency list.

## What Cannot Be Reproduced Publicly

Public reproduction is blocked for:

- Training on the original competition rows.
- Recreating final submission CSV files.
- Verifying a private leaderboard score or rank.
- Redistributing raw data, Drive archives, certificates with personal identifiers, or private team materials.

## Verification Notes

Expected public checks:

- `git status --short` to confirm the local worktree state.
- README keyword scan for metric, validation, feature, imbalance, limitations, data, reproduce, role, and evidence coverage.
- Recursive filename and file-size risk scan excluding `.git`.
- Credential-pattern scan excluding `.git`.
- `git diff` capture for documentation review.

## Limitations

- Dependency versions in notebooks may reflect the original competition environment.
- Sampling and validation results can vary with random seeds and library versions.
- Stored notebook outputs are evidence of prior runs, not a substitute for authorized reruns.
- This repo does not include production monitoring, drift detection, or a manufacturing-line deployment workflow.
