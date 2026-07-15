# About Dataset

## Synthetic Enterprise Authentication Logs for Lateral Movement Detection

### Overview

This dataset contains **180,000 synthetically generated enterprise authentication events** designed for research and experimentation involving post-compromise lateral movement detection.

The data represents authentication activity among users, workstations, servers, and a simulated domain controller named `DC-01`. It includes both normal enterprise activity and labeled malicious authentication sequences aligned with MITRE ATT&CK techniques.

The dataset can be used to evaluate rule-based, machine learning, graph-based, and anomaly-detection approaches. It does not contain or guarantee any predetermined model results. Performance must be calculated independently from the included event-level ground-truth labels.

All data is synthetic. No real users, hosts, organizations, credentials, or breach records are represented.

## Why this dataset exists

Traditional security controls often focus on preventing initial access through firewalls, multifactor authentication, account lockouts, and IP reputation systems. Once an attacker obtains valid credentials, however, later activity may resemble legitimate employee or administrator behavior.

Lateral movement occurs when an attacker uses compromised credentials to access additional internal systems and move toward sensitive targets. Detecting this behavior is difficult because individual authentication events may appear normal when examined in isolation.

This dataset supports research into behavioral and relational detection methods that consider questions such as:

- Has this user accessed this host before?
- Is the source subnet unusual for this user?
- Does the authentication create a rare user-to-host relationship?
- Is the destination unusually close to a sensitive system?
- Has the destination host experienced an abnormal increase in distinct users?
- Does a sequence of events resemble a multi-stage lateral movement chain?

## Dataset composition

The dataset contains:

- **180,000 authentication events**
- **30 days of simulated activity**, from May 1 through May 30, 2026
- **498 active users**
  - 431 standard users
  - 67 administrators
- **150 hosts**
  - 70 workstations
  - 79 servers
  - 1 domain controller, `DC-01`
- **115,140 benign events**
- **64,860 malicious events**
- **16,391 labeled attack chains**
- Authentication using Kerberos and NTLM
- Interactive, Network, Service, and RemoteInteractive logon types
- Successful and failed authentication attempts

Malicious events represent approximately **36.0% of the complete dataset**. This is an intentionally attack-enriched research benchmark and should not be interpreted as a realistic estimate of malicious activity in a production enterprise environment.

## Experimental structure

The events are arranged chronologically to support a time-ordered evaluation:

- **Baseline period:** 42,000 events from days 1 through 7
  - Contains benign activity used to establish normal behavior
- **Model-fit period:** 48,000 events from days 8 through 15
  - Includes 22,560 malicious events
- **Held-out test period:** 90,000 events from days 16 through 30
  - Includes 42,300 malicious events

This structure allows researchers to learn normal user and host behavior from earlier events, fit a model on a later period, and evaluate it on future authentication activity.

## How malicious activity is represented

Each malicious sequence has an `attack_chain_id` ranging from `AC-00001` through `AC-16391`.

Every chain contains:

- One event labeled `T1078`, representing the use of valid or compromised credentials
- Up to three events labeled `T1021`, representing movement through remote services

Most chains contain four events, although some contain fewer because of the way events are distributed across the simulated timeline.

The dataset includes malicious behaviors involving:

- New user-to-host relationships
- Previously unseen source subnets
- Reuse of familiar hosts in abnormal patterns
- Multi-host authentication sequences
- Increased activity around particular destination hosts
- Movement toward high-value internal systems
- Attempts to access the domain controller

## Columns

`event_id` · `timestamp` · `user_id` · `user_role` · `src_host` · `src_subnet` · `dst_host` · `auth_protocol` · `logon_type` · `auth_result` · `is_domain_controller_target` · `is_malicious` · `attack_chain_id` · `mitre_technique`

## Potential uses

- Comparing rule-based and graph-based lateral movement detectors
- Building supervised classification models
- Evaluating anomaly-detection methods
- Creating user-host interaction graphs
- Engineering edge-novelty, path-rarity, and degree-deviation features
- Studying time-ordered cybersecurity model evaluation
- Producing confusion matrices and calculating precision, recall, and false-positive rate
- Measuring event-level and attack-chain-level detection
- Testing graph neural networks, random-walk features, or temporal graph models
- Teaching MITRE ATT&CK-aligned detection concepts
- Demonstrating authentication-log analysis in Python, pandas, NetworkX, or scikit-learn

## Limitations

This is a synthetic and self-authored dataset. The generation process creates both the attack behavior and the labels used for evaluation. Models may therefore perform better on this dataset than they would on real enterprise authentication logs.

The dataset is intentionally attack-enriched. Malicious events make up approximately 36% of the complete file and approximately 47% of the fit and test periods. Real security datasets typically have a much lower malicious-event rate.

The benign activity model is a simplification and does not capture every characteristic of a real organization. Examples of omitted or simplified behavior include:

- Shared service accounts
- Employee turnover
- Seasonal activity
- Temporary access changes
- Maintenance windows
- Business travel
- Hybrid work patterns
- Application-specific authentication
- Complex privilege hierarchies
- Incomplete or noisy security labels

The dataset has not been validated against real incidents or production enterprise logs. It should be used for prototyping, education, controlled benchmarking, and feasibility research rather than as evidence that a detection method is production-ready.

## License and attribution

Released under **CC BY 4.0**. The dataset may be used, modified, and redistributed with appropriate attribution.

Users who publish a notebook, model, article, or research project using the dataset are encouraged to reference or link back to the original Kaggle dataset page.
