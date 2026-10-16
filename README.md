# ChronoQuality-CRAC

ChronoQuality-CRAC is a reproducible, CPU-only research workflow for studying hospital ranking stability under temporal and measurement-comparability constraints. The repository contains the complete V7 pipeline plus an independent reviewer-requested validation branch in one Kaggle-ready notebook.

## Notebook

[`ChronoQuality_CRAC_Kaggle_CPU.ipynb`](ChronoQuality_CRAC_Kaggle_CPU.ipynb) includes:

- input provenance, schema, row-count, and reporting-window checks;
- a fixed six-concept HCAHPS panel across the 2016–2020 releases;
- comparability-constrained pairwise ranking;
- baseline, ablation, missing-measure, semantic-composition, and temporal sensitivity analyses;
- exact finite-policy top-*k* membership certificates and policy-flip witnesses;
- longitudinal stability and score-change decomposition;
- subgroup, peer-relative, regression, and hospital-cluster bootstrap analyses;
- independent reviewer validation that reconstructs results directly from the five source CSV files;
- figures, tables, quality-control manifests, and ZIP archives for review.

The compact notebook preserves the original V7 execution order. Cells were consolidated for navigation; the research stages were not intentionally removed.

## Required dataset

Attach the Kaggle dataset **[U.S. Hospital Customer Satisfaction 2016–2020](https://www.kaggle.com/datasets/abrambeyer/us-hospital-customer-satisfaction-20162020)** by ABeyer.

The notebook expects these files:

```text
cms_hospital_patient_satisfaction_2016.csv
cms_hospital_patient_satisfaction_2017.csv
cms_hospital_patient_satisfaction_2018.csv
cms_hospital_patient_satisfaction_2019.csv
cms_hospital_patient_satisfaction_2020.csv
```

Together, the pinned files are expected to contain **1,653,683 rows**. The notebook stops if a file is missing or the row count differs, which helps detect an incorrect or changed dataset version. `Facility ID` is read as a string so leading zeroes are preserved.

## Run on Kaggle with CPU

1. Sign in to [Kaggle](https://www.kaggle.com/).
2. Open [`ChronoQuality_CRAC_Kaggle_CPU.ipynb`](ChronoQuality_CRAC_Kaggle_CPU.ipynb) on GitHub.
3. Select **Raw**, save the notebook, and upload it with Kaggle's **Create > New Notebook > File > Import Notebook** option. You may also clone/download this repository and upload the `.ipynb` file directly.
4. In the Kaggle notebook, select **Add Input** (or **Add Data**).
5. Search for `U.S. Hospital Customer Satisfaction 2016-2020`, choose the dataset owned by `abrambeyer`, and add it to the notebook.
6. Open **Notebook options** and set **Accelerator** to **None**. A GPU or TPU is not used.
7. Internet access is not required after the notebook and dataset are attached. Keep the default Kaggle Python environment.
8. Select **Run All** and execute every cell from top to bottom. Do not run later stages in isolation because they use objects and files created by earlier stages.
9. Confirm that the final cells complete without an assertion or exception and print the output archive paths.
10. Open Kaggle's **Output** or `/kaggle/working`, then download the generated ZIP files.

### Dataset path handling

The first pipeline branch checks its known Kaggle path and then searches `/kaggle/input` for the 2016 file. The independent reviewer branch searches for each of the five filenames. Therefore, Kaggle may mount the dataset under a slightly different directory without requiring code changes.

If auto-detection fails, confirm that all five files are attached. For the reviewer branch, you can also set `DATA_HINT` to the mounted dataset directory shown in Kaggle's data panel.

## CPU and dependency requirements

The workflow is designed for a Kaggle CPU session. It uses packages normally included in Kaggle's Python image:

- Python 3
- NumPy
- pandas
- SciPy
- Matplotlib
- statsmodels

No model weights, API keys, external results archive, GPU, or TPU are required. Runtime depends on Kaggle's assigned CPU and current service limits. The notebook includes pairwise comparisons, simulations, repeated resampling, bootstrap calculations, and figure generation, so keep the session open until all cells finish.

## Outputs

Intermediate V7 artifacts are written under:

```text
/kaggle/working/chronoquality_runs/RUN_004/
```

The main downloadable archives are:

```text
/kaggle/working/ChronoQuality_RUN_007_Results.zip
/kaggle/working/ChronoQuality_CRAC_Standalone_Reviewer_Validation_Results.zip
/kaggle/working/ChronoQuality_V7_With_All_Reviewer_Validation.zip
```

The combined archive retains the original V7 archive and the independently generated reviewer-validation archive as separate components. JSON inventories and integrity checks are produced so the exported contents can be audited.

## Successful-run checklist

Before using results, verify that:

- all five source files were detected;
- the input audit reports 1,653,683 total rows;
- no assertion, convergence, missing-file, or corrupt-archive error occurred;
- the adjusted subgroup model reports successful convergence;
- both component ZIP files pass their internal archive checks;
- `ChronoQuality_V7_With_All_Reviewer_Validation.zip` exists in `/kaggle/working`;
- reported results are taken from the completed run rather than copied from an earlier session.

## Scope and interpretation

This is a retrospective, exploratory analysis of historical CMS-derived hospital survey releases. The code does **not** establish causal effects, provide independent clinical outcome validation, perform expert semantic validation of historical measures, or prove that an enumerated scoring-policy family represents every defensible policy. Ranking certificates apply only to the registered finite policy set. Reviewer resampling intervals measure design sensitivity and are not patient-level clinical confidence intervals.

Review the generated quality-control tables, model diagnostics, evidence gates, and limitations before citing results or revising a manuscript.

## Reproducibility notes

- Random seeds and analysis configurations are recorded in the notebook and output JSON files.
- Input SHA-256 manifests are generated for the historical CSV files.
- Stable sorting and explicit tie handling are used where ranking reproducibility matters.
- V7 and reviewer-validation computations use separate output directories.
- The reviewer branch reloads the five CSV files and reconstructs key quantities without reusing V7 intermediate arrays.

## License and data ownership

No dataset files are redistributed in this repository. Use the linked Kaggle dataset subject to its terms and consult the original CMS sources and documentation for authoritative definitions and provenance.
