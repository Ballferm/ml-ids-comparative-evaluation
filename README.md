# Do ML Intrusion Detection Rankings Survive Unseen Data?

**MSc Cyber Security dissertation, De Montfort University (2026): grade 78%, overall MSc Distinction**
*Comparative Performance Evaluation of Machine Learning Algorithms for Network Intrusion Detection in Security Operations Centres*

Most comparative studies of ML-based intrusion detection rank algorithms on a single benchmark dataset, using aggregate accuracy. This project tests whether those rankings still hold once you add unseen attacks, independently sourced datasets, statistical validation and per-attack-category analysis.

> 🎤 These findings will be presented as **"When the Alarm Cries Wolf"** at **BSides London, 12 December 2026** (Rookie Track).

---

## Key findings

- **High cross-validation scores did not predict real-world performance.** On NSL-KDD, the best model's Matthews Correlation Coefficient (MCC) fell from **0.9978** under 10-fold cross-validation to **0.5763** on unseen test data. The model failed completely on specific, well-populated attack categories that were absent from training.
- **Rankings were consistent across some datasets but not all.** Rankings agreed between NSL-KDD and UNSW-NB15 (Spearman ρ = 0.90, p = 0.037) but broke down once CICIDS2017 was added. The cause was the two closest-matched models, Random Forest and Decision Tree, swapping first and second place.
- **Imbalance handling depended on the attack category.** SMOTE, random undersampling and class weighting rescued recall on some minority classes (e.g. SSH-Patator, the DoS family) and did nothing for others (e.g. Web Attack – Brute Force, FTP-Patator), within the same model.
- **Accuracy-only rankings misled where models were closely matched.** Composite (multi-metric) and accuracy-only rankings agreed when one model dominated, and diverged on UNSW-NB15 where models were close on accuracy but differed on precision and recall.

**Takeaway:** single-dataset, single-metric evaluation is not a sound basis for choosing a detector for a Security Operations Centre. Category-level and cross-dataset results should be reported as standard.

---

## Method

| | |
|---|---|
| **Models** | Logistic Regression, K-Nearest Neighbours, Decision Tree, Random Forest, Support Vector Machine |
| **Datasets** | NSL-KDD, UNSW-NB15, CICIDS2017 |
| **Phase 1** | Stratified 10-fold cross-validation with Wilcoxon signed-rank testing, plus held-out unseen-data testing |
| **Phase 2** | Cross-dataset ranking consistency (Spearman's rank correlation) |
| **Phase 3** | Class-imbalance handling (SMOTE, random undersampling, class weighting) at aggregate and per-attack-category level |
| **Phase 4** | Composite multi-metric ranking vs accuracy-only ranking |
| **Metrics** | MCC, accuracy, precision, recall, F1, false positive rate |

---

## Notebooks (run in order)

| # | Notebook | What it does |
|---|---|---|
| 01 | `01_NSLKDD_exploration.ipynb` | NSL-KDD exploration, preprocessing and cleaning |
| 02 | `02_UNSWNB15_exploration.ipynb` | UNSW-NB15 exploration, preprocessing and cleaning |
| 03 | `03_CICIDS2017_exploration.ipynb` | CICIDS2017 exploration, preprocessing and cleaning |
| 04 | `04_Phase1_NSLKDD_ModelTraining.ipynb` | Phase 1: training and 10-fold CV on NSL-KDD |
| 05 | `05_Phase1_UNSWNB15_ModelTraining.ipynb` | Phase 1: training and 10-fold CV on UNSW-NB15 |
| 06 | `06_Phase1_CICIDS2017_ModelTraining.ipynb` | Phase 1: training and 10-fold CV on CICIDS2017 |
| 07 | `07_Phase2_SpearmanCorrelation.ipynb` | Phase 2: cross-dataset ranking consistency |
| 08 | `08_UnseenDataTesting.ipynb` | Held-out unseen-data testing |
| 09 | `09_Visualisations.ipynb` | Charts used in the results chapter |
| 10 | `10_Phase3_ImbalanceHandling_CICIDS2017.ipynb` | Phase 3: imbalance-handling strategies |
| 11 | `11_Phase4_CompositeRanking.ipynb` | Phase 4: composite vs accuracy-only ranking |
| 12 | `12_Phase3b_AttackCategoryAnalysis.ipynb` | Phase 3b: per-attack-category analysis |

---

## Reproducing the results

**Requirements:** Python 3, scikit-learn, pandas, NumPy, SciPy, imbalanced-learn, matplotlib, Jupyter.

**Datasets** (not included in this repo because of their size and licensing; download them from the official sources):
- NSL-KDD: https://www.unb.ca/cic/datasets/nsl.html
- UNSW-NB15: https://research.unsw.edu.au/projects/unsw-nb15-dataset
- CICIDS2017: https://www.unb.ca/cic/datasets/ids-2017.html

Update the file paths at the top of each preprocessing notebook to point at your local copies. Experiments were run on a laptop (Ryzen 7, 16 GB RAM); chunked loading or sampling was used for CICIDS2017 where memory required it.

---

## Author

**Oluwaferanmi (Feranmi) Olayinka**: MSc Cyber Security (Distinction), De Montfort University
[LinkedIn](https://linkedin.com/in/olayinka-oluwaferanmi-a29618174) · [GitHub](https://github.com/Ballferm)

Supervised by Dr Olabayo Ishola, De Montfort University.
