# PFS and OS in Metastatic Breast Cancer Phase II Clinical Trials

## Overview

This repository contains the dataset and analysis code for:

> **"PFS and OS in Metastatic Breast Cancer Phase II Trials: Effects of Regimen Complexity, Line of Therapy, and Inferred Drug Interactions"**

We pooled **1,088 treatment arms** from **892 phase II clinical trials** encompassing **53,672 patients** with advanced or metastatic breast cancer (MBC) to examine how regimen complexity (mono-, doublet, and triplet therapy), line of therapy, molecular subtype, and baseline ECOG performance status influence progression-free survival (PFS) and overall survival (OS). Drug–drug synergy and antagonism were inferred from trial-level tumor response data using a null model of non-interacting agents.

## Repository Structure

```
├── Dataset_mBC.xlsx              # Full pooled dataset of 1,088 treatment arms from phase II mBC trials
├── Dataset_interaction.xlsx      # Subset dataset for drug–drug interaction (synergy/antagonism) analysis
├── mBC_trend.Rmd                 # R Markdown file containing all analysis code and figure generation
└── README.md
```

### File Descriptions

| File | Description |
|------|-------------|
| `Dataset_mBC.xlsx` | Primary dataset containing arm-level efficacy endpoints (ORR, mPFS, mOS) and explanatory variables (molecular subtype, treatment size, therapy type, treatment line, ECOG PS, drug names) extracted from 892 eligible studies. |
| `Dataset_interaction.xlsx` | Derived dataset for drug–drug interaction analysis, containing expected ORR (from monotherapy benchmarks), observed ORR (from combination arms), and interaction classification for each drug pair. |
| `mBC_trend.Rmd` | Complete R Markdown analysis pipeline organized into the sections described below. |

### Analysis Pipeline (`mBC_trend.Rmd`)

| Code Chunk | Description | Output |
|------------|-------------|--------|
| `Dataset preprocessing` | Reads `Dataset_mBC.xlsx`, constructs combination labels from drug columns (T1–T4), recodes 4-Agent as 3-Agent, performs multiple imputation (MICE, m=20, maxit=30) for missing values, and prepares the final analytic dataset with outlier filters. | Analytic dataset |
| `Dataset Summary` | Generates summary statistics table using `gtsummary`, reporting demographics, molecular subtypes, treatment characteristics, and efficacy endpoints. | Table 1 (`.docx`) |
| `Subtype Treatment Effect` | Box plots of median PFS and OS by treatment regimen stratified by molecular subtype, filtering for regimens with ≥5 contributing studies. | Figure 1 |
| `ggstatsplot Size vs Therapy Type` | Violin plots with Kruskal–Wallis tests and epsilon-squared (ε²) effect sizes comparing PFS and OS distributions across 1-Agent, 2-Agent, and 3-Agent categories. | Figure 2 |
| `Treatment Size vs Line of Therapy` | Six-panel grid comparing first-line vs. pretreated populations within each agent category for both PFS and OS, with rank biserial (r_rb) effect sizes and 95% CIs. | Figure 3 |
| `wECOG Correlation Analysis` | Spearman correlations between weighted ECOG PS and each endpoint (ORR, mPFS, mOS) with scatter plots and effect size interpretation. | Statistical output |
| `Drug-drug interaction analysis` | Reads interaction dataset and generates synergy/antagonism scatter plot (observed vs. expected ORR) with labeled combination points and diagonal reference line. | Figure 4 |

## Methods

All analyses used non-parametric statistics (Kruskal–Wallis, Mann–Whitney U, Dunn's post hoc with Bonferroni–Holm correction) with effect size quantification (epsilon-squared, rank biserial correlation, Spearman's ρ with Fieller CI). Drug–drug interactions were inferred using the Kang et al. (2013) null model framework applied to ORR.

## Software

Analyses were performed in **R (≥ 4.2.2)** with the following packages:

- `tidyverse` — data wrangling and transformation
- `readxl` — reading Excel datasets
- `mice` — multiple imputation by chained equations
- `VIM` — missing data visualization
- `gtsummary` / `gt` — summary table generation
- `ggplot2` — visualization
- `ggstatsplot` — statistical graphics with embedded hypothesis tests
- `effectsize` — effect size estimation and interpretation
- `report` — automated statistical reporting
- `cowplot` — multi-panel figure composition
- `tidytext` — text reordering within facets
- `ggrepel` — non-overlapping text labels
- `colorspace` — qualitative color palettes

