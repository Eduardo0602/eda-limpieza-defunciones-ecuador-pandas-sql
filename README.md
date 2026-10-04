<p align="right"><b>English</b> · <a href="README.es.md">Español</a></p>

# Messy-data EDA: what must be fixed before trusting an official registry

Data-quality audit, reproducible cleaning and SQL exploration of the 2021 General Deaths Statistical Registry of Ecuador's INEC (107,648 records).

![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white) ![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas) ![SQLite](https://img.shields.io/badge/SQLite-003B57?logo=sqlite) ![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0) ![License](https://img.shields.io/badge/license-MIT-green) [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Eduardo0602/eda-limpieza-defunciones-ecuador-pandas-sql/blob/master/notebooks/01_auditoria_calidad.ipynb)

## The problem

Real public data are rarely ready to analyze: they contain missing values that do not look missing, wrong types, duplicates and impossible dates. The goal here is not a model but turning dirty data into reliable data and documenting every decision. The workflow has three stages: diagnosis, treatment and analysis.

## Data

| Feature | Detail |
|---|---|
| Source | [INEC Ecuador, General Deaths Statistical Registry 2021](https://anda.inec.gob.ec/anda/index.php/catalog/930) |
| Original dimensions | 107,648 rows × 45 columns |
| Dimensions after cleaning | 107,641 rows × 58 columns |
| Rows removed | 7 (confirmed exact duplicates, mostly neonatal) |
| Columns added | 13 (ICD-10 code breakdown and late-registration flag) |
| Problems found | 369,685 cells with disguised missing values, 3 columns with wrong types, impossible dates, semantic duplicates |

The data are not included in the repository; they are downloaded from the INEC portal.

## Methodological foundation

**Disguised missing values.** Blank spaces, "Sin información" (no information) or "No aplica" (not applicable) are not recognized as missing by pandas. They were replaced by explicit `NaN` values and, column by column, it was documented whether they were imputed, removed or kept as structural missing values.

**Structural missing values versus missing data.** `muj_fertil`, `mor_viol` and `lug_viol` only apply to subpopulations (women of childbearing age, violent deaths). Their missing values correctly mean "not applicable" and were kept.

**Choice of correlation coefficient.** Spearman ($`\rho_s`$) was used instead of Pearson ($`r`$): the numeric variables are discrete (time components) with outliers, and Spearman neither requires normality nor is limited to linear relationships. The ICD-10 codes (`cod_causa103`, `cod_causa80`, `cod_causa67B`) were excluded from the matrix: they are nominal labels, not magnitudes.

**Density estimation.** Age is shown with a histogram and a kernel density curve, $`\hat{f}(x) = \frac{1}{nh}\sum_{i=1}^{n} K\left(\frac{x - x_i}{h}\right)`$. The axis was limited to $`[-5, 125]`$ to avoid showing the artificial density at negative ages that the Gaussian kernel produces near 0.

## Results

- **COVID-19 was the leading single cause of death** with 21,002 deaths (19.51 %), although grouped cardiovascular diseases add up to 25,368 (23.57 %).
- **April 2021** recorded 13,785 deaths, about twice the months of the second half of the year: the third COVID-19 wave.
- **Women die on average 5.8 years later than men** (69.1 versus 63.3 years).
- **The share of violent deaths varies widely across provinces** (among those with at least 2,000 deaths): Esmeraldas (12.04 %) doubles Loja (5.00 %). This is a descriptive comparison, not a hypothesis test.

![Top 10 causes of death](reports/figures/viz_01_top10_causas.png)

![Monthly evolution](reports/figures/viz_02_evolucion_mensual.png)

## Verification

The `limpiar_dataset()` function, applied to the raw dataset, reproduces exactly the result of the step-by-step pipeline: same shape (107,641 × 58) and 58 of 58 identical columns. Each transformation is also checked after it runs in notebook 02.

## How to reproduce

```bash
git clone https://github.com/Eduardo0602/eda-limpieza-defunciones-ecuador-pandas-sql.git
cd eda-limpieza-defunciones-ecuador-pandas-sql
conda create -n ds_portafolio python=3.11 -y
conda activate ds_portafolio
pip install -r requirements.txt
# Download the INEC dataset (https://anda.inec.gob.ec/anda/index.php/catalog/930) into data/raw/
jupyter lab   # open 01, 02 and 03 in order: Kernel → Restart & Run All
```

| Notebook | Contents |
|---|---|
| [`01_auditoria_calidad.ipynb`](notebooks/01_auditoria_calidad.ipynb) | Missing-value map, disguised missing values, duplicates, types and dates; audit table |
| [`02_pipeline_limpieza.ipynb`](notebooks/02_pipeline_limpieza.ipynb) | Reproducible pipeline: missing values, types, ICD-10 codes, dates; before and after comparison |
| [`03_analisis_exploratorio_sql.ipynb`](notebooks/03_analisis_exploratorio_sql.ipynb) | Load into SQLite, 6 analytical queries, 9 visualizations, Spearman correlation and conclusions |

Notebooks and code comments are in Spanish.

## Project structure

```
eda-limpieza-defunciones-ecuador-pandas-sql/
├── data/processed/       # defunciones_2021_limpio.csv (generated by notebook 02)
├── notebooks/            # 01 → 02 → 03
├── src/                  # data.py, features.py, visualization.py
├── reports/figures/      # 11 figures
├── requirements.txt
└── LICENSE
```

## Limitations

- It is a descriptive analysis of one year: differences between groups were not subjected to hypothesis tests.
- Provincial rates are proportions within registered deaths, not population rates (population projections were not used).

## What I learned

1. **Cleaning is the longest and most important stage.** In this project, the audit and cleaning took most of the effort.
2. **Missing values are not always missing.** Detecting "Sin información" or blank spaces requires manual inspection and domain knowledge.
3. **The statistical tool is chosen before it is applied.** Pearson would have produced numbers without a technical error but methodologically wrong; choosing Spearman and excluding nominal codes mattered more than the computation.
4. **SQL and pandas complement each other.** SQL is more expressive for grouped aggregations with filters and subqueries; pandas, for column-wise transformations and immediate visualization.

---

### Portfolio *From Mathematician to Data Scientist*

| Project | Question | Tools |
|---|---|---|
| [Complex survey sampling with Ser Estudiante](https://github.com/Eduardo0602/muestreo-complejo-ser-estudiante) | How wrong is an analysis that ignores the sampling design? | R, survey |
| **Messy-data EDA: deaths 2021** (this repository) | What must be fixed before trusting an official registry? | Python, pandas, SQL |
| [Linear regression from scratch](https://github.com/Eduardo0602/regresion-lineal-numpy-desde-cero) | Can a plane predict how deep Ecuador's earthquakes are? | Python, NumPy |
| [Visual linear algebra](https://github.com/Eduardo0602/algebra-lineal-visual-numpy) | What does a matrix do, geometrically? | Python, NumPy |

Eduardo Araque · Mathematician (Universidad Central del Ecuador) · [GitHub](https://github.com/Eduardo0602) · [LinkedIn](https://www.linkedin.com/in/eduardo-araque-jacome-math)
