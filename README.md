# Deep Learning-Based GNSS Clock Bias Prediction

A research project focused on predicting satellite clock bias errors in GNSS systems using deep learning models such as GRU and LSTM. The project evaluates how different neural architectures handle raw clock bias signals and proposes a trend–residual decomposition method to improve prediction accuracy.

---

# Overview

Global Navigation Satellite Systems (GNSS) rely on extremely accurate satellite clocks. Even very small timing errors can introduce large positioning errors.

This project explores machine learning approaches to forecast satellite clock bias using real GNSS data from NASA's CDDIS archive.

### Goals

- Predict satellite clock bias at picosecond-level precision
- Compare GRU vs LSTM architectures
- Improve forecasting using trend–residual decomposition
- Build a reproducible deep learning pipeline

---

# Key Results

| Model | Approach | R² Score | Notes |
|------|------|------|------|
| GRU | Raw clock bias | ~0.98 | Strong performance on drifting signal |
| LSTM | Raw clock bias | 0.05–0.49 | Struggles with trend |
| LSTM | Trend + Residual | **~0.99** | Best overall performance |

Trend decomposition significantly improves LSTM performance by simplifying the learning problem.

---

# Dataset

Source: NASA CDDIS GNSS archive

Data used:

- Satellite: AS-C12  
- Date Range: 14 Jan – 20 Jan 2021  
- Sampling interval: 30 seconds  

### Features

| Feature | Description |
|---|---|
| bias | Satellite clock bias |
| drift | Rate of change of bias |
| t_seconds | Time since start of dataset |
| drift_final | Interpolated drift values |

---

# Project Pipeline

1. Data extraction from `.clk` GNSS files  
2. Cleaning and time alignment  
3. Drift interpolation  
4. Feature construction  
5. Normalization  
6. Sliding window sequence generation  
7. Model training (GRU / LSTM)  
8. Evaluation on unseen test day  

---

# Method 1 — GRU on Raw Bias

GRUs handle long sequential dependencies efficiently and are suitable for slowly drifting signals.

### Architecture

```
GRU (128 units, return sequences)
GRU (64 units)
Dropout (0.2)
Dense (32, ReLU)
Dense (1 output)
```

### Training Setup

- Loss: Mean Squared Error
- Optimizer: Adam
- Batch size: 64
- Window size: 60

### Performance

- MAE: ~2.9 × 10⁻⁸ seconds
- RMSE: ~3.5 × 10⁻⁸ seconds
- R²: ~0.98

---

# Method 2 — LSTM with Trend–Residual Decomposition

This approach separates the clock bias signal into:

```
Bias = Trend + Residual
```

### Steps

1. Extract linear trend using polynomial fitting  
2. Compute residual signal  
3. Train LSTM on residual + drift  
4. Reconstruct final bias prediction  

### Why This Works

- Removes long-term drift
- Makes the signal more stationary
- Allows LSTM to focus on short-term patterns

### Performance

- MAE: ~8.8 × 10⁻⁹ seconds
- RMSE: ~9.2 × 10⁻⁹ seconds
- R²: **0.9987**

---

# Technologies Used

- Python
- TensorFlow / Keras
- NumPy
- Pandas
- Scikit-learn
- Matplotlib

---

# Applications

Accurate satellite clock prediction can improve:

- Precise Point Positioning (PPP)
- Real-Time Kinematic (RTK)
- Satellite navigation systems
- GNSS timing infrastructure

---



# Authors

Yash Pandey  
Parv Gupta  
Palak Baisla  


---


---
