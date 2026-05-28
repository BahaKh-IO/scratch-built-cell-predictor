# Fine Needle Aspirate (FNA) Cell Geometry Predictor

A pure, from-scratch Python implementation of a Linear Regression model designed to analyze breast mass tumor cell morphology. Without relying on any machine learning frameworks like `scikit-learn`, this project uncovers structural patterns in oncology data using basic statistical mathematics.

## 📌 Project Overview

During a Breast Fine Needle Aspirate (FNA) biopsy, quantitative histopathology tools measure various geometric features of cell nuclei. While capturing the population average (`_mean`) of a biopsy is standard, identifying the absolute most abnormal, extreme outliers (`_worst`) is crucial for clinical risk assessment.

This model explores **nuclear pleomorphism** (the variation in cell size and shape characteristic of cancer cells) by predicting the upper tail bound size (`area_worst`) based exclusively on the overall population average (`area_mean`).

## 📌 Performance Metrics Summary
Following full execution over the model's independent evaluation partition, the system outputs the following validation metrics:
Mean Squared Error (MSE): 10461.3519
Root Mean Squared Error (RMSE): 102.2808 (Average tracking error of  102.28 physical units)
Coefficient of Determination (R^2 Score): 0.9680 (96.80% of testing variance successfully explained)
Optimization Setup: Batch Gradient Descent (alpha = 0.02, iterations = 3000)
Validation Strategy: Chronological Holdout Split (N_train = 450, N_test = 119)



---

## 🛠️ Features

* **Zero ML Dependencies:** Built completely from scratch using basic arithmetic arrays. No `scikit-learn`, `PyTorch`, or `TensorFlow`.
* **Built-in Verification:** Manual calculations for Mean Squared Error (MSE) and $R^2$ Score to measure model confidence.

---

## 💻 Installation & Requirements

The project requires only standard data handling libraries included in typical Python data science setups.

```bash
pip install pandas numpy

```
## 🛠️ Algorithmic Breakdown (From Scratch Implementation)

Unlike template implementations, the notebook constructs the underlying gradient calculation layers sequentially:
1. Cost Objective Engine (cost)

Calculates the Mean Squared Error over an explicit pointer verification step to assess current global fit penalties:

Cost Formula: J(w, b) = (1 / 2m) * Sum( ((w * x + b) - y)^2 )

2. Analytical Gradient Step (gradient)

Calculates simultaneous partial derivative slopes for parameters across loop iterations:

Slope Gradient Formula: dj_dw = (1 / m) * Sum( (predicted_y - actual_y) * x )

Intercept Gradient Formula: dj_db = (1 / m) * Sum( predicted_y - actual_y )

3. Iterative Tuning Horizon (gradient_descent)

Updates variables tracking optimization histories across an asynchronous loop paradigm:

Parameter Step Updates: w = w - (alpha * dj_dw)

b = b - (alpha * dj_db)

### Dataset Structure

The project utilizes the classic *Breast Cancer Wisconsin (Diagnostic) Dataset* (`data.csv`). The script expects at least the following columns:

* `area_mean`: The arithmetic mean of cell nuclear area for each patient sample.
* `area_worst`: The mean of the three largest nuclear areas recorded for each patient sample.

---

## 📋 Direct Console Output Display
When you execute this script, the logging pipeline prints the complete structural breakdown below to the console terminal screen:
Plaintext

Iteration 0, Cost = 543476.9230769231
Iteration 100, Cost = 23610.339611684347
Iteration 200, Cost = 15309.58550181518
Iteration 300, Cost = 14995.738152570414
Iteration 400, Cost = 14983.621453228678
...
Iteration 2900, Cost = 14983.143615175953

==================================================
   LINEAR REGRESSION METRICS (FROM GRADIENT DESCENT)   
==================================================
Optimized Feature Weight (w) : 544.867912
Optimized Engine Bias (b)    : 885.856166
--------------------------------------------------
TRAINING SET PERFORMANCE (N = 450):
  -> Mean Squared Error (MSE) : 29966.2872
  -> R2 Score (Accuracy)      : 0.9038 (90.38%)
--------------------------------------------------
TESTING SET PERFORMANCE (N = 119):
  -> Mean Squared Error (MSE) : 10461.3519
  -> Root MSE (RMSE)          : 102.2808
  -> R2 Score (Accuracy)      : 0.9680 (96.80%)
==================================================

## 📈 Parameter Trace & Variable Unscaling Transformation

Because predictions run natively inside a standard-scaled frame (X_scaled), the math calculation acts as:

Predicted_y_worst = w_scaled * ((area_mean - mean_of_X) / std_of_X) + b_scaled

The script includes an elegant algebraic conversion to unpack this directly back into unscaled, physical real-world values for an operating pathologist:

    w_unscaled = w_scaled / std_of_X * b_unscaled = b_scaled - (w_scaled * mean_of_X) / std_of_X Which yields the final real-world physical prediction rule:

    Predicted_area_worst = 1.5519 * (area_mean) - 135.7379

An R2 score of 96.80% validates that knowing the average cell size acts as a nearly perfect structural proxy for calculating worst-case diagnostic outliers without requiring expensive additional imaging tests.
