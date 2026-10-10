# Cervical cancer screening: exploratory analysis

An exploratory notebook that examines risk-factor patterns and recorded screening outcomes in the dataset under `data/`. It uses K-Means to describe groups of records and association rules to summarize characteristics that co-occur with positive biopsy results.

## Run the analysis

From the project root, install the dependencies and launch Jupyter:

```powershell
pip install -r requirements.txt
jupyter notebook notebooks/cervical_cancer_analysis.ipynb
```

Run the notebook cells from top to bottom. The notebook also supports being run with `notebooks/` as the working directory. It reads the CSV from `data/` and saves refreshed figures in `outputs/`:

- `outputs/pca_clusters.png` - exploratory groups projected onto two PCA axes
- `outputs/cluster_diagnostic_rates.png` - observed biopsy and Schiller positivity by group
- `outputs/biopsy_rate_by_cluster.png` - biopsy positivity with group sizes labeled

## Current result snapshot

With the supplied CSV and the current notebook settings, the notebook analyzes 858 records and selects three exploratory groups (silhouette score 0.582, compared with 0.575 for two groups). One group contains only 19 records (2.2% of the dataset); its observed biopsy-positive share is 31.6%, compared with 4.9% in the 762-record group and 15.6% in the 77-record group. Treat the small group's percentage as especially uncertain and descriptive, not as an estimate of an individual's risk.

## Models and methods used

- **K-Means clustering:** unsupervised grouping on all 30 standardized risk-factor input features. The notebook compares 2–5 groups by silhouette score and uses the best-scoring count (3 for the supplied data). Biopsy and other screening outcomes are not clustering inputs.
- **Principal component analysis (PCA):** projects the standardized inputs onto two dimensions for the scatterplot only; PCA is not used to assign groups or predict a diagnosis.
- **Apriori association-rule mining:** identifies co-occurring binary characteristics and summarizes direct associations with positive biopsy using support, confidence, and lift. These are descriptive associations, not causal effects or predictions.
- **Preprocessing:** median imputation for missing input features and standard scaling for K-Means and PCA. Screening outcomes are left unimputed.

**No supervised classifier or validated clinical prediction model is trained.** These exploratory methods must not be used to diagnose patients or estimate an individual's risk.

## How to read the results

- **Cluster count:** the notebook selects the best silhouette score among the tested counts (2-5). This is a way to choose an exploratory partition, not evidence that the groups are distinct clinical types.
- **PCA chart:** each point is a record; the clustering itself uses all standardized input features, not just the two plotted axes.
- **Group tables and bars:** show descriptive averages and the share with positive results among records with known results. Cluster IDs are arbitrary labels, not risk rankings.
- **Association rules:** support measures how common the combined pattern is; confidence is the biopsy-positive share among records matching the listed characteristics; lift compares that share with the overall positive share. High lift can still come from very few records.

## Important limitations

This is an exploratory analysis of a historical public dataset, not a validated clinical model. Associations do not establish causation; group rates are not individual predictions. Missing risk-factor inputs are median-imputed, while missing screening outcomes remain unknown and are excluded from outcome summaries. Results should not be used to diagnose, assess an individual's cancer risk, or guide screening or treatment decisions.

## Project layout

```text
data/       Input CSV
notebooks/  Analysis notebook
outputs/    Generated figures
```