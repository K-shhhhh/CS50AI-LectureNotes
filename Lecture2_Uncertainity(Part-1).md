# 🤖 CS50 AI with Python – Lecture 2: **Uncertainty & Probability**

This lecture explores how AI handles uncertainty using **probability theory**, enabling intelligent decisions even with incomplete information.

---

## 🧠 Why Uncertainty?

Real-world decisions often lack complete knowledge.  
For example:  
> "What’s the weather tomorrow?"  
We don’t *know* for sure — but we can **predict** using **probabilities**.

---

## 🎲 Basic Concepts of Probability

### 🌍 Possible Worlds (`ω`)
Each outcome is a *possible world*.  
E.g., rolling a die gives:
```
ω ∈ {1, 2, 3, 4, 5, 6}
```
> Probability of world `ω`: `P(ω)`

### 📐 Axioms of Probability

| Rule | Explanation |
|------|-------------|
| `0 ≤ P(ω) ≤ 1` | All probabilities between 0 and 1 |
| `P(impossible)` = 0 | e.g., rolling a 7 on a 6-sided die |
| `P(certain)` = 1 | e.g., getting < 10 on a 6-sided die |
| Sum of all `P(ω)` = 1 | Total probability is 1 |

---

## 🎲 Dice Example: Summing Probabilities

Rolling **two dice** = 36 total outcomes  
- P(sum = 12) → only `(6,6)` → `1/36`  
- P(sum = 7)  → six outcomes → `6/36 = 1/6`

---

## 🔁 Types of Probability

### ✅ Unconditional Probability
- Based on no prior evidence  
> e.g., P(rain) = 0.1

### 🔄 Conditional Probability

> P(a | b) = “Probability of a **given** b is true”

### 📘 Formula:

```
P(a | b) = P(a ∧ b) / P(b)
```

#### 🎯 Example:
> What is P(sum = 12 | first die is 6)?

- Outcomes where die1 = 6: `{(6,1), (6,2), … (6,6)}`
- Only one of them (6,6) has sum = 12
- So, `P(12 | die1 = 6) = 1/6`

---

## 🎲 Random Variables & Distributions

### 🎲 What is a Random Variable?
A variable with multiple possible outcomes  
> e.g., `Roll ∈ {1, 2, 3, 4, 5, 6}`

### 📊 Probability Distribution
| Outcome | Probability |
|---------|-------------|
| On Time | 0.6         |
| Delayed | 0.3         |
| Canceled| 0.1         |

➡ Can be written as vector:  
```
P(Flight) = <0.6, 0.3, 0.1>
```

---

## 🔗 Independence

Two events are **independent** if one doesn’t affect the other:
```
P(A ∧ B) = P(A) * P(B)
```
E.g. Dice rolls are independent; cloudiness and rain are not.

---

## 📏 Bayes’ Rule

### 📘 Formula:
```
P(B | A) = [P(A | B) * P(B)] / P(A)
```

### 🎯 Example: Rain and Clouds
| Event | Value |
|-------|-------|
| P(clouds | rain) | 0.8 |
| P(clouds)        | 0.4 |
| P(rain)          | 0.1 |

> **P(rain | clouds) = (0.8 × 0.1) / 0.4 = 0.2 (20%)**

📌 So even with partial evidence (clouds), we can better predict rain.

---

## 📘 Joint Probability

Probability of two or more events happening **together**.

### 🌦 Clouds and Rain Table

| Cloud | Rain | P     |
|-------|------|-------|
| ✅    | ✅   | 0.08  |
| ✅    | ❌   | 0.32  |
| ❌    | ✅   | 0.02  |
| ❌    | ❌   | 0.58  |

> e.g., `P(cloud ∧ rain) = 0.08`

---

## 🔁 Conditional from Joint

```
P(cloud | rain) = P(cloud ∧ rain) / P(rain)
               = 0.08 / 0.10 = 0.8
```

✅ You can **normalize** values to create distributions:
```
P(cloud | rain) = α <0.08, 0.02>
               = <0.8, 0.2>
```

---

## 📏 Probability Rules

### ❗ Negation:
```
P(¬a) = 1 - P(a)
```

### 🔄 Inclusion-Exclusion:
```
P(a ∨ b) = P(a) + P(b) - P(a ∧ b)
```

📌 Avoids double-counting!

#### 🍨 Example:
```
P(ice cream) = 0.8  
P(cookies) = 0.7  
P(both) = 0.5  

P(either) = 0.8 + 0.7 - 0.5 = 1.0 ✅
```

---

## 🧮 Marginalization

```
P(a) = P(a ∧ b) + P(a ∧ ¬b)
```

➡ Just add both possibilities of b

---

## ⚙ Conditioning (Full Form)

```
P(a) = P(a | b)P(b) + P(a | ¬b)P(¬b)
```

🧠 We're saying:  
> "A can happen **with** B or **without** B"

---

📌 This concludes the **first half** of Lecture 2 – covering all fundamentals of probability in AI.

🧠 Coming up next: **Bayesian Networks & Inference Models**

---
