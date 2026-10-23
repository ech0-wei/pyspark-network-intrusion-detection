# Network Intrusion Detection with PySpark

**PySpark · Apache Spark (RDDs / DataFrames) · Spark MLlib · Databricks · Python**

*Graduate-level academic project — not a production security system.*

## Overview

This project analyzes the historical **KDD Cup 99 10%** dataset using Apache Spark for exploratory analysis, feature preparation, and binary network intrusion classification. The saved coursework notebook processed **494,021 network-connection records** and used a **70/30 train/test split**.

## Implementation

- Retrieve a compressed KDD Cup 99 extract into **Databricks DBFS** and load it as a Spark RDD.
- Parse connection records and convert RDDs to Spark DataFrames.
- Explore protocol types, services, flags, traffic volumes, and normal/attack labels.
- Map the original attack labels to a binary **normal (0) / attack (1)** target.
- Use Spark MLlib **StringIndexer** for categorical fields and **VectorAssembler** for model features.
- Train **Logistic Regression, Random Forest, and LinearSVC**, evaluating accuracy, precision, recall, and F1.

## Saved coursework results

The following metrics are from the **original saved notebook outputs**, not from a fresh or independently validated run.

| Classifier | Accuracy | F1 |
| --- | ---: | ---: |
| Logistic Regression | 97.74% | 0.9772 |
| Random Forest | 99.79% | 0.9979 |
| Linear SVC | 97.74% | 0.9773 |

**Caveat:** KDD Cup 99 is a historical, imbalanced benchmark. Preprocessing in the original notebook also fits category encoders before the train/test split, which can introduce data leakage. These scores do **not** establish production intrusion-detection performance.

## Project contents

- [`network_intrusion_detection_pyspark.ipynb`](network_intrusion_detection_pyspark.ipynb): data exploration, feature engineering, Spark MLlib models, and evaluation code
- [`requirements.txt`](requirements.txt): Python dependencies (an appropriate Spark/Databricks runtime is also required)
- [`.gitignore`](.gitignore): excludes datasets, secrets, and temporary files

## Run in Databricks

1. Import the notebook into a Python notebook attached to a Databricks Spark cluster.
2. The code uses Databricks-specific `dbutils`, `display`, and DBFS paths. The historical KDD Cup 99 URL is `http://kdd.ics.uci.edu/databases/kddcup99/kddcup.data_10_percent.gz`; it may no longer be reachable.
3. If the URL is unavailable, obtain the 10% KDD Cup 99 dataset separately and place `kddcup_data.gz` in `dbfs:/kdd/`, skipping the download cell.
4. Run the remaining notebook sections in order. Metrics may vary by Spark version and execution environment.

The dataset is **not redistributed** in this repository. The public notebook is a code-focused edition; original execution outputs were omitted for readability. Reported figures above refer to the preserved coursework results.

## Limitations and improvements

- Fit categorical encoders on the training partition, preferably within a Spark ML Pipeline.
- Use per-class confusion matrices, precision, and recall; accuracy alone can be misleading on imbalanced data.
- Evaluate on a modern intrusion dataset before drawing security conclusions.
- Rework `.toPandas()` calls for datasets too large to fit into driver memory.
- Add automated data validation and environment-independent data loading.

## Provenance

Prepared from an academic PySpark coursework notebook. Code and experimental approach are preserved while GitHub-facing titles, descriptions, and private execution metadata are cleaned. This is a portfolio project, not production deployment.
