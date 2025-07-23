# 🤖 CS50 AI with Python – Lecture 4: Learning (Part 1)

This lecture introduces **Machine Learning (ML)** — the ability of computers to learn patterns from data instead of being explicitly programmed.

---

## 📚 What is Machine Learning?

- Machine learning finds **patterns** in historical data and generalizes them for **future predictions**.
- Instead of programming specific behavior, we give examples and let the system **learn the mapping** from inputs to outputs.

---

## 🎯 Supervised Learning

- In **supervised learning**, the algorithm learns from **labeled data** (inputs paired with outputs).
- The goal is to learn a function `f(x)` that maps inputs to the correct outputs.

### Example: Weather Prediction
- Inputs: Humidity, Pressure
- Output: Rain or No Rain
- Learn a function `h(humidity, pressure)` that approximates `f`

---

## 🧪 Classification

- Classification is a type of supervised learning where output is **discrete**.
- The learned model maps inputs to classes like:
  - ✅ Rain
  - ❌ No Rain

---

## 📍 Nearest Neighbor Classification

- **Nearest neighbor**: Classify a new point based on the label of the **closest** point in the training data.
- Can be affected by **outliers**.

### 🧠 k-Nearest Neighbors (k-NN)
- Instead of one neighbor, look at **k closest** neighbors.
- Use the **majority label** among those k.
- More robust to noise.
- Drawback: Requires **storing and comparing** to all data points → expensive for large datasets.

---

## 🧮 Perceptron

- The perceptron is a **linear classifier**.
- It classifies data by computing a **weighted sum** of the inputs:

### 🔢 Perceptron Equation

- h(x₁, x₂) =
-   1 if w₀ + w₁x₁ + w₂x₂ ≥ 0
-   0 otherwise


- Represent inputs as vector `x = (1, x₁, x₂)`
- Represent weights as vector `w = (w₀, w₁, w₂)`
- Compute `w · x` (dot product)

### 🔧 Perceptron Learning Rule

- wᵢ = wᵢ + α(y - h(x)) * xᵢ


- If prediction is wrong, adjust weights
- α is the **learning rate**

---

## 🧱 Thresholds

- **Hard Threshold**: Predicts 0 or 1, no in-between
- **Soft Threshold (Sigmoid/Logistic Function)**:
  - Outputs probability (between 0 and 1)
  - Reflects **confidence** in the prediction

---

## 🧭 Support Vector Machines (SVM)

- SVM tries to find the **best boundary** (hyperplane) that **maximizes the margin** between classes.

### Benefits:
- Robust against outliers
- Can handle **non-linear** boundaries using **kernel trick**

---

## 📈 Regression

- **Regression** predicts **continuous values** (e.g., house price, temperature).
- Goal: Fit a **line or curve** to the data that minimizes error.

---

## 🎯 Loss Functions

Used to quantify how **wrong** a prediction is.

### 🧮 Classification Loss: 0–1 Loss

- L(actual, predicted) =
-   0 if correct
-   1 if incorrect


### 📉 Regression Loss:

- **L1 Loss**: `|actual - predicted|`
- **L2 Loss**: `(actual - predicted)²`

- **L2 Loss** penalizes outliers more heavily than L1.

---

## 📊 Summary Table

| Technique       | Type          | Notes                                      |
|----------------|---------------|--------------------------------------------|
| k-NN           | Classification | Uses k nearest neighbors                   |
| Perceptron     | Classification | Linear boundary, simple & fast             |
| SVM            | Classification | Maximizes margin, robust, non-linear OK    |
| Regression     | Regression     | Predicts continuous values, uses loss fn.  |

---

📌 End of Lecture 4 (Part 1): You now understand how computers can learn from data using:
- k-NN
- Perceptrons
- SVM
- Regression
- Loss functions

Next up: **Learning Part 2 – Unsupervised Learning, Neural Networks, and Beyond**
