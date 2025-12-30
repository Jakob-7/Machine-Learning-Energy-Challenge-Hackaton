# Machine Learning Energy Disaggregation  
### 🥉 3rd Place — Machine Learning Energy Challenge Hackathon

This repository contains our final solution for the **Machine Learning Energy Challenge**, a hackathon organized in collaboration with **TaDa** and **Hiop**.

The goal of the challenge was to apply **supervised machine learning** techniques to disaggregate household electricity consumption and reconstruct the **refrigerator power signal** from aggregate smart-meter data.

---

## 🧠 Problem Overview

Electricity disaggregation aims to infer the consumption of individual appliances from a household’s total power usage.

Refrigerators represent a particularly challenging case because they:
- Consume relatively **low power**
- Operate **continuously** with intermittent compressor cycles
- Produce signals that are easily hidden in background noise

The task was to predict the refrigerator’s instantaneous power consumption given **only the aggregate household power signal**.

---

## 📊 Dataset Description

The dataset is derived from **UK-DALE** and resampled using the **Chain2 transmission protocol**, the standard adopted by second-generation Italian smart meters.

### Input Data
- **5 households** with labeled fridge consumption (training)
- **1 unseen household** used for evaluation

### Files
- `train.csv`
  - `datetime`: timestamp (ISO format, timezone-aware)
  - `power`: aggregate household power (W)
  - `fridge`: refrigerator power (W)
  - `home_id`: household identifier

- `test.csv`
  - `datetime`
  - `power`

- `sample_submission.csv`
  - Example of required prediction format

---

## ⏱️ Chain2 Sampling Protocol

The dataset has a **variable sampling frequency**:
- A reading is recorded **every 15 minutes**
- OR whenever power crosses a **300W multiple threshold** (up or down)

This creates irregularly spaced time series, requiring careful preprocessing before model training.

---

## 🔧 Methodology

## 🔧 Methodology

The solution follows a structured pipeline designed to handle **irregularly sampled smart-meter data** and ensure **generalization across households**.

### Data Preprocessing
- Parsed timestamps and ordered observations chronologically
- Addressed the **Chain2 variable sampling frequency** via resampling and alignment
- Prepared clean, regularly spaced time-series inputs for modeling

### Feature Engineering
- Constructed lagged aggregate power features
- Computed rolling statistics (mean, standard deviation)
- Added temporal context features to capture periodic behavior
- Applied smoothing to highlight refrigerator compressor cycles

### Cross-Validation Strategy
- Used **Leave-One-House-Out (LOHO)** cross-validation
- Trained models on multiple households and validated on unseen ones
- Ensured robustness and avoided household-specific overfitting

### Modeling
- Trained **LightGBM** and **XGBoost** regression models
- Leveraged tree-based models for non-linear, noisy signals
- Tuned models under time constraints typical of a hackathon setting

### Ensembling
- Combined LightGBM and XGBoost predictions via blending
- Improved stability and reduced variance across households

### Evaluation & Inference
- Evaluated performance using **Mean Absolute Error (MAE)**
- Visually inspected predicted vs. true refrigerator consumption
- Generated final predictions for the unseen test household


---

## 🏆 Results

Our final model achieved **3rd place overall** in the hackathon ranking.

Qualitative evaluation on unseen household data showed:
- Accurate identification of compressor activation cycles
- Stable predictions during low-variation periods
- Good robustness across different household profiles

---

## 📁 Repository Structure

```text
.
├── final_model.ipynb      # Final training, validation, and inference pipeline
├── data/                 # data description
├── README.md             # Project documentation
