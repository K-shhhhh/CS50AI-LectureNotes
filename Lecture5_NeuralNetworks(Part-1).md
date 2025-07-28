# 🤖 CS50 AI with Python – Lecture 5: Neural Networks (Part 1)

---

## 🧠 Inspiration from Biology

Neural Networks in AI are inspired by the human brain:
- Neurons in the brain receive and send signals.
- When input exceeds a threshold, the neuron activates.

---

## 💻 Artificial Neural Networks (ANN)

ANNs simulate this idea:
- Each **unit** (analogous to a neuron) receives **inputs**.
- Each input is multiplied by a **weight**.
- A **bias** is added.
- The result is passed through an **activation function**.
- The network **learns weights** by training on data.

### 🔧 Hypothesis Function:

- h(x₁, x₂) = w₀ + w₁·x₁ + w₂·x₂


---

## ⚡ Activation Functions

Used to decide the output based on weighted input sum.

### 📉 Step Function:
- Outputs 0 before threshold, 1 after.

### 📈 Logistic Function (Sigmoid):
- Outputs a value between 0 and 1 (confidence score).

### 🧱 ReLU (Rectified Linear Unit):
- If value > 0 → output value  
- If value < 0 → output 0

---

## 🔗 Simple Neural Network Structure

A simple network:
- **Inputs**: x₁, x₂
- **Weights**: w₁, w₂
- **Bias**: w₀
- **Output**: g(w₀ + w₁x₁ + w₂x₂)

---

## 🔢 Logical Operations as Neural Networks

### 🟠 OR Function:

| x₁ | x₂ | f(x₁, x₂) |
|----|----|-----------|
| 0  | 0  |     0     |
| 0  | 1  |     1     |
| 1  | 0  |     1     |
| 1  | 1  |     1     |

#### Neural Network Representation:

- Output = g(-1 + x₁ + x₂), threshold = 0

- If sum ≥ 0 → Output 1
- Else → Output 0

### 🟢 AND Function:
- Similar, but bias = -2

---

## 🌦️ Real-world Examples

- Inputs: Humidity, Pressure
- Output: Probability of Rain

OR

- Inputs: Ad Spend, Month
- Output: Predicted Sales

---

## 🧮 Gradient Descent

Used to **train weights** to minimize loss.

### 🪜 Basic Algorithm:
1. Start with random weights
2. Repeat:
   - Compute **gradient** of loss wrt weights
   - Update weights in direction that **reduces loss**

---

## 🔄 Variants of Gradient Descent

- **Batch GD**: Uses all data → accurate but slow
- **Stochastic GD**: Uses 1 sample → fast but noisy
- **Mini-Batch GD**: Uses small subsets → best of both

---

## 🌤️ Example: Weather Prediction

- Inputs: x₁ … xₙ
- Outputs: Sunny, Rainy, Cloudy
- Each output = its own small neural network with independent weights

---

## ➗ Linear vs Non-linear Models

- Simple perceptron can only learn **linear** boundaries
- Many real-world problems need **non-linear** decision boundaries

---

## 🧠 Multilayer Neural Networks (MLP)

Adds **hidden layers** between input and output

### 🔧 Structure:
- Input Layer
- One or more **Hidden Layers**
- Output Layer

### 🔄 Flow:
- Inputs → Hidden Layer(s) → Outputs
- Each layer processes & passes forward weighted values

---

## 🔁 Backpropagation

Main training algorithm for deep networks.

### 🧠 Steps:
1. Calculate **error** at output layer
2. Propagate error **backward**:
   - Layer by layer, compute gradient for weights
3. Update weights
4. Repeat

➡ Enables Deep Learning: More than 1 hidden layer

---

## 🧠 Overfitting & Dropout

Overfitting = Model fits training data too well → Poor generalization

### 🧪 Dropout Technique:
- Randomly deactivate some units during training
- Prevents reliance on specific neurons
- Encourages network robustness

➡ After training, use full network again for prediction

---

📌 Summary

| Concept                  | Description |
|--------------------------|-------------|
| Neural Network           | Input → Weights → Activation → Output |
| Activation Functions     | Step, Sigmoid, ReLU |
| Logical Functions        | OR, AND via weighted inputs |
| Gradient Descent         | Learn weights to minimize loss |
| Multilayer Perceptron    | Enables non-linear decision boundaries |
| Backpropagation          | Core training algorithm |
| Dropout                  | Reduces overfitting |

📚 Up Next: Deeper architectures, convolutional nets, and applications
