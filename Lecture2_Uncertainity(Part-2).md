# 🤖 CS50 AI with Python – Lecture 2: **Bayesian Networks, Inference & Markov Models**

This is the **second part** of Lecture 2, covering **Bayesian networks**, **probabilistic inference**, and **temporal models** such as **Markov** and **Hidden Markov Models**.

---

## 📊 Bayesian Networks

A **Bayesian network** is a directed graph that models dependencies between variables.

### 🔗 Key Properties
- Nodes = random variables
- Edges = dependencies (causal or correlational)
- A node’s probability depends only on its **parents**
- Each node has `P(X | Parents(X))`

---

### 🧭 Example: Will You Reach on Time?

#### 📍 Variables
- **Rain**: {none, light, heavy}  
- **Maintenance**: {yes, no}  
- **Train**: {on time, delayed}  
- **Appointment**: {attend, miss}  

---

### 🌧️ Rain (Root Node)
| Rain | P     |
|------|-------|
| none | 0.7   |
| light| 0.2   |
| heavy| 0.1   |

---

### 🛠️ Maintenance (Depends on Rain)

| Rain  | Maint | P     |
|-------|-------|-------|
| none  | yes   | 0.4   |
| none  | no    | 0.6   |
| light | yes   | 0.2   |
| light | no    | 0.8   |
| heavy | yes   | 0.1   |
| heavy | no    | 0.9   |

---

### 🚆 Train (Depends on Rain & Maintenance)

| Rain  | Maint | Train   | P     |
|-------|-------|---------|-------|
| none  | yes   | on time | 0.8   |
| none  | yes   | delayed | 0.2   |
| none  | no    | on time | 0.9   |
| none  | no    | delayed | 0.1   |
| light | yes   | on time | 0.6   |
| light | yes   | delayed | 0.4   |
| light | no    | on time | 0.7   |
| light | no    | delayed | 0.3   |
| heavy | yes   | on time | 0.4   |
| heavy | yes   | delayed | 0.6   |
| heavy | no    | on time | 0.5   |
| heavy | no    | delayed | 0.5   |

---

### 📅 Appointment (Depends on Train)

| Train   | Appointment | P     |
|---------|-------------|-------|
| on time | attend      | 0.9   |
| on time | miss        | 0.1   |
| delayed | attend      | 0.6   |
| delayed | miss        | 0.4   |

---

### 🔍 Joint Probability Example

To compute  
```
P(light, no, delayed, miss)
```
Use:
```
= P(light) × P(no | light) × P(delayed | light, no) × P(miss | delayed)
```

---

## 🧠 Inference in Bayesian Networks

Unlike **logical inference**, we estimate **probability distributions**.

### 🔢 Key Components
- **Query**: variable we want (e.g., Appointment)
- **Evidence (E)**: observed values (e.g., Rain = light)
- **Hidden Variables (Y)**: unobserved (e.g., Maintenance)

---

### 🔎 Example:
> Compute: `P(Appointment | light, no)`

We marginalize over hidden variables (like Train):
```
P(Appointment | light, no) = α [ P(Appointment, light, no, delayed) + P(Appointment, light, no, on time) ]
```

---

## 🧮 Inference by Enumeration

### 🧠 Equation
```
P(X | e) = α Σ_y P(X, e, y)
```
- X = query
- e = observed evidence
- y = hidden variables
- α = normalization constant

---

## 🐍 Code: Using `pomegranate` for Bayesian Inference

```python
from pomegranate import *

# Rain
rain = Node(DiscreteDistribution({
    "none": 0.7,
    "light": 0.2,
    "heavy": 0.1
}), name="rain")

# Maintenance
maintenance = Node(ConditionalProbabilityTable([
    ["none", "yes", 0.4], ["none", "no", 0.6],
    ["light", "yes", 0.2], ["light", "no", 0.8],
    ["heavy", "yes", 0.1], ["heavy", "no", 0.9]
], [rain.distribution]), name="maintenance")

# Train
train = Node(ConditionalProbabilityTable([
    ["none", "yes", "on time", 0.8], ["none", "yes", "delayed", 0.2],
    ...
], [rain.distribution, maintenance.distribution]), name="train")

# Appointment
appointment = Node(ConditionalProbabilityTable([
    ["on time", "attend", 0.9], ["on time", "miss", 0.1],
    ["delayed", "attend", 0.6], ["delayed", "miss", 0.4]
], [train.distribution]), name="appointment")

# Model creation
model = BayesianNetwork()
model.add_states(rain, maintenance, train, appointment)
model.add_edge(rain, maintenance)
model.add_edge(rain, train)
model.add_edge(maintenance, train)
model.add_edge(train, appointment)
model.bake()
```

---

## 🎯 Querying the Model

```python
probability = model.probability([["none", "no", "on time", "attend"]])
print(probability)

predictions = model.predict_proba({ "train": "delayed" })
for node, prediction in zip(model.states, predictions):
    ...
```

---

## 🔁 Approximate Inference: Sampling

Instead of **enumerating** all probabilities, we **sample**.

### 🎲 Sampling Steps:
1. Sample each variable according to its distribution.
2. Use previously sampled values as conditions for next node.
3. Repeat N times and count outcomes.

---

### 🐍 Sampling Example in Python

```python
from collections import Counter

def generate_sample():
    ...
    return sample

# Rejection sampling
N = 10000
data = []

for _ in range(N):
    sample = generate_sample()
    if sample["train"] == "delayed":
        data.append(sample["appointment"])

print(Counter(data))
```

---

## 🎯 Likelihood Weighting

A more efficient alternative to rejection sampling.

### ⚙️ Steps
- Fix evidence variables.
- Sample others conditionally.
- Weight sample by likelihood of evidence.

---

## ⌛ Markov Models

A **Markov Model** introduces the **dimension of time**.

### 📌 Markov Assumption
> Current state depends only on a **finite number of past states**

### 🔁 Markov Chain

Each state depends **only on the previous state**

```python
start = DiscreteDistribution({ "sun": 0.5, "rain": 0.5 })

transitions = ConditionalProbabilityTable([
    ["sun", "sun", 0.8], ["sun", "rain", 0.2],
    ["rain", "sun", 0.3], ["rain", "rain", 0.7]
], [start])

model = MarkovChain([start, transitions])
print(model.sample(50))
```

---

## 🌧 Hidden Markov Models (HMMs)

In an HMM, **true state** is hidden — we only observe outcomes.

### 🎯 Examples
- Robot’s position (hidden), sensor readings (observed)
- Weather (hidden), umbrellas (observed)
- Spoken words (hidden), sound waveforms (observed)

---

### 📦 HMM in Code

```python
sun = DiscreteDistribution({ "umbrella": 0.2, "no umbrella": 0.8 })
rain = DiscreteDistribution({ "umbrella": 0.9, "no umbrella": 0.1 })

transitions = np.array([[0.8, 0.2], [0.3, 0.7]])
starts = np.array([0.5, 0.5])

model = HiddenMarkovModel.from_matrix(
    transitions, [sun, rain], starts,
    state_names=["sun", "rain"]
)
model.bake()
```

---

### 🔍 Most Likely Explanation

```python
observations = [
    "umbrella", "umbrella", "no umbrella",
    "umbrella", "umbrella", "umbrella",
    "umbrella", "no umbrella", "no umbrella"
]

predictions = model.predict(observations)
for i in predictions:
    print(model.states[i].name)
```

> Output: `rain, rain, sun, rain, rain, rain, rain, sun, sun`

---

📌 This concludes **Part 2** of Lecture 2, focusing on real-world modeling using **Bayesian inference**, **sampling**, and **Markov models**. These tools form the **foundation for decision-making** in uncertain, dynamic environments.

---
