# Smart Contract Vulnerability Classifier

Binary classifier detecting reentrancy vulnerabilities (SWC-107) in Solidity smart contracts.

## Results
| Metric | Value |
|--------|-------|
| Test F1 | 0.9796 |
| Test AUC | 0.9779 |
| CV F1 (5-fold) | 0.9093 ± 0.0352 |

## Setup
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute smart_contract_vulnerability_classifier_v4.ipynb

## Key Design Decisions
- 28% of safe contracts use call.value() correctly (CEI pattern) — non-trivial separation
- 6% label noise simulates real-world annotation uncertainty
- Random Forest chosen over LR (cannot model feature interactions) and XGBoost (more HPs)
- state_update_after_call is most predictive feature (Gini importance: 0.2997)

## Checkpoint
rf_n200_depth10_seed42.pkl — Random Forest, n=200, max_depth=10, seed=42

## Dataset Design

To avoid trivial detection, two noise sources were injected:

- 28% of safe contracts use `call.value()` correctly following the Checks-Effects-Interactions pattern.
- 6% label noise simulates real-world annotation uncertainty.

This prevents simple token-based classifiers from achieving perfect separation.
## Evaluation

| Metric | Value |
|------|------|
| Test Accuracy | 0.9464 |
| Test F1 | 0.9412 |
| Test AUC | 0.9837 |
| CV F1 | 0.8898 ± 0.0225 |

Evaluation performed using a stratified train/validation/test split (70/15/15) and 5-fold cross-validation.