# Retail Customer Intelligence: From Transactions to Customer Segments

A Python data analytics project that transforms retail transaction data into customer insights, product associations, and actionable business recommendations.

## Project Overview

This project uses the UCI Online Retail dataset to explore sales performance, understand customer purchasing behaviour, segment customers, and identify products that are frequently purchased together.

The project combines exploratory data analysis, RFM (Recency, Frequency, Monetary) segmentation, association rule mining, a rule-based product recommendation system, and K-Means clustering.

## Business Questions

- How do sales change over time?
- Which products and countries contribute the most observed sales value?
- Which customers are recent, frequent, and high-spending?
- Which customers may benefit from re-engagement campaigns?
- Which products are frequently purchased together?
- Can customer purchasing patterns be grouped using unsupervised machine learning?
- How do rule-based RFM segments compare with K-Means clusters?

## Dataset

**Source:** UCI Machine Learning Repository — Online Retail  
**Dataset page:** https://archive.ics.uci.edu/dataset/352/online+retail

The original dataset contains 541,909 transaction records and eight columns, including invoice number, product code, product description, quantity, invoice date, unit price, customer ID, and country.

The dataset covers transactions from December 2010 to December 2011. It is historical data and should not be interpreted as a representation of current retail activity.

## Technologies

- Python
- pandas and NumPy
- Matplotlib and Seaborn
- scikit-learn
- mlxtend
- Jupyter Notebook
- Git and GitHub

## Project Workflow

### 1. Data Understanding and Cleaning

- Examined the dataset structure, data types, missing values, and duplicates.
- Investigated cancellations, negative quantities, and non-positive prices.
- Standardised column names and converted relevant columns to appropriate data types.
- Removed exact duplicate rows.
- Saved the cleaned transaction dataset for reuse.

### 2. Exploratory Data Analysis

- Analysed monthly sales trends.
- Identified products with high observed sales value and quantity.
- Compared sales across countries.
- Examined transaction categories and unusual transactions.
- Saved summary tables and charts for further analysis.

### 3. RFM Customer Segmentation

Calculated customer-level metrics:

- **Recency:** Days since the customer's most recent qualifying purchase.
- **Frequency:** Number of distinct invoices.
- **Monetary:** Sum of qualifying transaction line values (`quantity × unit price`).

Customers were scored using quintiles and assigned to seven rule-based segments.

### 4. RFM Validation and Business Insights

- Checked for duplicate customer IDs and missing RFM metrics.
- Verified score ranges and segment totals.
- Produced segment summaries and suggested business actions.

### 5. Market Basket Analysis

Applied the Apriori algorithm to invoice-level baskets to find frequently purchased product combinations. Association rules were evaluated using:

- **Support:** Proportion of invoice baskets containing the item combination.
- **Confidence:** Proportion of baskets containing the antecedent that also contain the consequent.
- **Lift:** Strength of the association compared with what would be expected if the products occurred independently.

The initial analysis was limited to the 100 most common products that passed the configured support threshold, with itemset length limited to three.

### 6. Product Recommendation System

Built a rule-based function that recommends other products from one-product-to-one-product association rules. Rules are filtered by confidence and lift, then ranked by confidence, lift, and support.

This is a rule-based recommender, not a personalised recommendation model. Its recommendations have not been validated through a controlled experiment.

### 7. K-Means Customer Clustering

- Selected Recency, Frequency, and Monetary as clustering features.
- Applied `log1p` transformations and standardisation.
- Evaluated cluster counts from K = 2 to K = 7 using silhouette scores.
- Selected K = 2 because it had the highest silhouette score among the tested options.
- Created customer cluster profiles and visualisations.

### 8. RFM and K-Means Comparison

Compared the RFM segments with K-Means cluster assignments for all 4,338 identified customers.

Key observed results:

- 941 customers were classified as **High-Value Loyal** by RFM; all belonged to Cluster 0.
- 1,072 customers were classified as **Potentially Inactive** by RFM; 1,069 belonged to Cluster 1.
- Cluster 0 had higher average purchase frequency and monetary value, and lower average recency, than Cluster 1.

These methods use the same underlying RFM features, so their agreement is useful for comparison but is not independent confirmation.

## Key Findings

### Customer Segments

The RFM analysis identified 4,338 customers.

| RFM segment | Customers | Customer share |
|---|---:|---:|
| High-Value Loyal | 941 | 21.69% |
| Potentially Inactive | 1,072 | 24.71% |
| Frequent Customers | 565 | 13.02% |
| Occasional Customers | 555 | 12.79% |
| Recent and Engaged | 484 | 11.16% |
| Previously Active Customers | 411 | 9.47% |
| Recent Customers | 310 | 7.15% |

The segment percentages total approximately 100%; small differences can occur due to rounding.

### K-Means Clusters

| Cluster | Customers | Customer share | Average recency (days) | Average frequency | Average monetary value |
|---|---:|---:|---:|---:|---:|
| Cluster 0 | 1,668 | 38.45% | 26.45 | 8.44 | 4,535.74 |
| Cluster 1 | 2,670 | 61.55% | 134.67 | 1.67 | 494.98 |

Monetary values are observed transaction values in the dataset's currency, not profit or customer lifetime value.

### Product Associations

The analysis found strong historical associations among products such as Regency teacups and lunch-box products. These patterns can inform cross-selling ideas and product recommendation experiments, but association does not imply causation.

## Business Recommendations

- Consider retention campaigns for High-Value Loyal customers.
- Test re-engagement campaigns for customers with low relative RFM scores.
- Explore cross-selling using high-confidence product associations.
- Investigate frequent customers whose last purchase was relatively long ago.
- Evaluate campaign outcomes before concluding that recommendations increase sales.

## Repository Structure

```text
retail-customer-intelligence-python/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
├── src/
├── reports/
│   └── figures/
├── tests/
├── docs/
├── README.md
├── requirements.txt
└── .gitignore
```

The `data/raw/` folder should contain the original dataset locally. Avoid committing large data files or sensitive data to a public repository unless the licence and repository policy permit it.

## Setup and Execution

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd retail-customer-intelligence-python
```

### 2. Create a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```powershell
python -m pip install -r requirements.txt
```

If `mlxtend` is missing, install it and update the requirements file:

```powershell
python -m pip install mlxtend
python -m pip freeze > requirements.txt
```

### 4. Add the dataset

Download the Online Retail dataset from the UCI page and place `Online Retail.xlsx` in `data/raw/`.

### 5. Open the notebook

```powershell
jupyter notebook
```

Open the analysis notebook under `notebooks/` and execute the cells in order. If using previously saved processed CSV files, ensure the relative paths match the notebook location.

## Limitations

- The dataset represents a historical period, not current customer behaviour.
- RFM segment names depend on the scoring and classification rules used.
- Quintile scores are relative to this dataset; they are not fixed business thresholds.
- K-Means results depend on the selected features, transformations, scaling, and number of clusters.
- A high silhouette score does not automatically mean the clusters are optimal for business use.
- Association rules reveal co-occurrence patterns, not causal relationships.
- Product recommendations have not been tested in a live system or controlled experiment.
- Monetary value is based on observed transaction line values and does not represent profit or customer lifetime value.

## Future Improvements

- Add automated data-quality tests with `pytest`.
- Move reusable analysis functions into the `src/` package.
- Improve market basket efficiency by filtering products before constructing the basket matrix.
- Evaluate recommendations using historical holdout data or controlled experiments.
- Compare additional clustering configurations and assess business interpretability.
- Add sales forecasting with a clearly defined time-based validation strategy.
- Build an interactive Power BI dashboard.
- Add a project presentation with visualisations and business findings.
