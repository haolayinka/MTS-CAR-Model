"---
title: "README: MTS-CAR and Baseline Models"
output: github_document
---

# Overview

This repository contains the R Markdown implementation of the following models:

1. **MTS-CAR**: a multivariate time-series model with conditional autoregressive (CAR) spatial effects.
2. **MTS Baseline**: a baseline multivariate time-series model without the full CAR structure.
3. **SS Baseline**: a single-series AR(1) baseline fitted separately to each series.

The main model in this repository is **MTS-CAR**. It is designed for panel time-series data observed across multiple areas or units, where temporal dependence and spatial dependence are both important.

The repository is intended for users who want to:

- fit the proposed MTS-CAR model,
- compare it with simpler baselines,
- generate leave-one-out forecasts,
- and evaluate predictive performance using the same transformed dataset and forecast metrics.

# Repository contents

The repository currently contains three main R Markdown files:

- `MTS-CAR.Rmd`
- `MTS Baseline.Rmd`
- `SS Baseline.Rmd`

These files are organized as follows:

- **MTS-CAR.Rmd**: full model with spatial random effects and CAR prior structure.
- **MTS Baseline.Rmd**: multivariate baseline model.
- **SS Baseline.Rmd**: single-series baseline model fitted independently to each area.

# Model summary

## MTS-CAR

The MTS-CAR model is the main contribution of this repository. It models each area-specific series using a dynamic regression with spatially structured random effects. The general form is

$$
y_{it} = \theta_0 + \nu_{0i} + (\theta_1 + \nu_{1i}) y_{i,t-1} + \varepsilon_{it},
\qquad
\varepsilon_{it} \sim \mathcal{N}(0,\sigma^2),
$$

where the random effects are assigned a CAR-based prior structure.

This model is appropriate when:

- the data are observed over time,
- there are multiple related spatial units or areas,
- and neighboring areas are expected to share information.

## MTS Baseline

The MTS Baseline provides a multivariate benchmark without the full spatial CAR structure. It is included for model comparison.

## SS Baseline

The SS Baseline is a univariate AR(1) model fitted separately to each series:

$$
y_t = \beta_0 + \beta_1 y_{t-1} + \varepsilon_t,
\qquad
\varepsilon_t \sim \mathcal{N}(0,\sigma^2).
$$

This serves as the simplest benchmark.

# Data requirements

The code assumes that the same dataset is used across all models.

The following objects are expected in the working environment or created during preprocessing:

- `MSA_data`: transformed analysis series used for estimation
- `mdata`: intermediate transformed series used for back-transformation
- `msa_data`: square-root transformed series
- `msa_data_level`: original series in level form
- `Dat2.csv`: adjacency matrix used for the spatial structure in the MTS-CAR model

## Important note on preprocessing

The forecasting code in this repository assumes that the data have already undergone the same preprocessing pipeline used in the paper:

1. transformation,
2. differencing,
3. construction of lagged series,
4. and alignment of the transformed and level datasets.

In particular, the leave-one-out forecast code assumes that:

- the final row of the transformed series is held out,
- the model is fit on rows `1, ..., T-1`,
- the omitted final transformed value is forecast,
- and the forecast is back-transformed to the original level.

