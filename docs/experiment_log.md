## Log

| Experiment ID | Notebook | Description | Config/random_state| Result summary | Status |
|---|---|---|---|---|---|
| EXP-01 | 02_rfm_baseline.ipynb | K-means baseline reproduction, k=3 vs k=5, on FS1 (log1p + RobustScaler RFM) | random_state=42, n_init=10 | k=5 reproduces paper's high/low-value cluster extremes closely (top 8% of customers → 52.2% of sales vs paper's top 5.05% → 25.5%; bottom ~47% of customers → 7.6% of sales vs paper's 4%). Middle segments less sharply separated than paper's, consistent with FS1 silhouette favoring lower k. FS0 (unscaled) silhouette scores (0.81–0.97) confirmed as scale-domination artifact, not used for k-selection. | Complete |