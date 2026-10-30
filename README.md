# Deep Learning-Based Emulation of CMAQ Surface NO₂ Concentrations

## 📌 Project Overview

This project explores deep learning as a computationally efficient alternative for reproducing the spatial and temporal patterns of Chemical Transport Model (CTM) outputs.

A ResNet-based Convolutional Neural Network (CNN) is developed to predict daily mean surface nitrogen dioxide (NO₂) concentrations simulated by the Community Multiscale Air Quality (CMAQ) model.

The model utilizes meteorological conditions, atmospheric emissions, and land-surface characteristics to learn the complex relationships governing NO₂ distribution across space and time.

## 📊 Dataset Description

The study uses data from the **US Environmental Protection Agency (EPA) EQUATES dataset**.

| Parameter | Description |
|:---|:---|
| **Data Source** | US EPA EQUATES |
| **Study Area** | Texas, USA |
| **Spatial Resolution** | 12 km |
| **Study Period** | January 1–15, 2016 |
| **Spatial Domain** | 80 × 100 grid cells |
| **Prediction Target** | Daily mean surface NO₂ concentration |
| **Reference Model** | CMAQ |

## 🔍 Input Features

The model incorporates **Meteorological, emission and land surface based features** representing meteorological conditions, atmospheric emissions, and land-surface characteristics.

## ⚙️ Methodology

### 1. Data Preprocessing

Multidimensional NetCDF datasets were processed and converted into **Zarr format** for efficient storage and data access.

The preprocessing workflow included:

- Aggregation of hourly meteorological and emission variables into daily features.
- Derivation of eastward and northward wind components from wind speed and direction.
- One-hot encoding of categorical land-use variables.
- Log transformation of highly skewed variables.
- Z-score normalization of continuous input features.
- Preparation of spatially aligned input tensors and corresponding CMAQ-derived NO₂ concentration targets.

### 2. Model Architecture

A **ResNet-based Convolutional Neural Network (CNN)** was implemented to capture spatial dependencies and nonlinear relationships between environmental variables and NO₂ concentrations.

The architecture consists of:

- **Input and Feature Extraction Stem:** Extracts initial spatial features from the 27-channel input tensor.
- **Residual Blocks:** Six residual blocks designed to learn hierarchical spatial representations.
- **Dilated Convolutions:** Dilation rates ranging from 1 to 4 to capture spatial dependencies at multiple scales.
- **Normalization:** Group normalization for stable model training.
- **Activation Function:** Gaussian Error Linear Unit (GELU).
- **Output Layer:** A final 1 × 1 convolutional layer generates spatial predictions of daily mean surface NO₂ concentrations.

### 3. Training and Validation

The dataset was divided into temporally independent training and testing periods.

| Dataset | Duration | Description |
|:---|:---|:---|
| Training | 10 days | Model training |
| Testing | 5 days | Independent temporal evaluation |

To evaluate spatial generalization and minimize spatial leakage, **five-fold spatial cross-validation** was performed using spatially separated grid-cell strips.

A **five-column spatial buffer** was introduced between training and validation regions to reduce information leakage caused by spatial autocorrelation.

### 4. Model Evaluation

Model performance was evaluated using the following statistical metrics:

| Metric | Description |
|:---|:---|
| **MAE** | Mean Absolute Error |
| **RMSE** | Root Mean Square Error |
| **Mean Error (ME)** | Measures systematic prediction bias |
| **Pearson Correlation (R)** | Measures linear association between predictions and reference values |
| **Index of Agreement (IOA)** | Evaluates agreement between predicted and reference concentrations |

## 🎯 Project Objective

The primary objective is to investigate whether a deep learning-based emulator can efficiently reproduce the spatial distribution and temporal variability of CMAQ-simulated surface NO₂ concentrations while maintaining predictive accuracy.

By learning from meteorological, emission, and land-surface information, the proposed framework aims to provide a computationally efficient approach for approximating chemical transport model outputs.
