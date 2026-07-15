# Graph-Based Lateral Movement Detection

This repository contains an independent comparison of rule-based and graph-based methods for detecting post-compromise lateral movement in synthetic enterprise authentication logs.

## Project overview

The analysis uses a public Kaggle dataset containing:

- 180,000 authentication events
- 498 active users
- 150 hosts
- 16,391 labeled attack chains
- MITRE ATT&CK techniques T1078 and T1021
- 30 days of simulated enterprise activity

The notebook downloads the dataset directly from Kaggle and evaluates both detectors on a chronologically held-out test period.

## Detection methods

### Rule-based baseline

The baseline flags:

1. A user's first observed login to a destination host
2. A login from a previously unseen source subnet

### Graph-based method

The graph-based model combines:

- Edge novelty
- Path rarity toward the domain controller
- Host degree deviation

Logistic regression learns the feature weights from the fit period.

## Evaluation design

The dataset is divided chronologically:

- Baseline period: 42,000 events
- Fit period: 48,000 events
- Held-out test period: 90,000 events

For the primary comparison, both methods generate exactly **32,013 alerts**, creating an equal analyst-review budget.

## Held-out test results

| Method | Precision | Recall | False-positive rate |
|---|---:|---:|---:|
| Rule-Based | 0.687 | 0.520 | 0.210 |
| Graph-Based | 0.823 | 0.623 | 0.119 |

### Confusion matrices

| Method | TP | FP | FN | TN |
|---|---:|---:|---:|---:|
| Rule-Based | 21,996 | 10,017 | 20,304 | 37,683 |
| Graph-Based | 26,336 | 5,677 | 15,964 | 42,023 |

Under the same alert budget, the graph-based method detects more malicious events and produces fewer false positives.

## Repository contents

```text
.
├── README.md
├── requirements.txt
├── notebooks/
│   └── lateral_movement_detection_analysis.ipynb
├── poster/
│   └── lateral_movement_poster.pdf
├── docs/
│   └── DATASET.md
├── data/
│   └── README.md
└── results/
    ├── performance_metrics.csv
    └── confusion_matrices.csv
```

## Run the notebook

Install the dependencies:

```bash
pip install -r requirements.txt
```

Then launch Jupyter:

```bash
jupyter notebook notebooks/lateral_movement_detection_analysis.ipynb
```

The notebook downloads the public dataset with:

```python
dataset_path = Path(
    kagglehub.dataset_download(
        "danielpeng1995/synthetic-enterprise-auth-logs"
    )
)
```

## Dataset

The dataset is hosted on Kaggle:

https://www.kaggle.com/datasets/danielpeng1995/synthetic-enterprise-auth-logs

The CSV is not duplicated in this repository. See [`docs/DATASET.md`](docs/DATASET.md) for the complete dataset description and limitations.

## Limitations

This is a synthetic, attack-enriched dataset with self-authored ground truth. It is appropriate for controlled experimentation, education, and prototyping, but it does not establish real-world production performance.

The rule-based baseline is intentionally simple and should not be interpreted as a modern commercial UEBA system.

## Author

**Daniel Peng**  
New York University
