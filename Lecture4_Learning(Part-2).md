# 🤖 CS50 AI with Python – Lecture 4: Learning (Part 2)

---

## 🎯 Overfitting

Overfitting occurs when a model fits the training data too well but fails to generalize to new, unseen data.

- Loss functions can be misleading — a perfect fit (loss = 0) might still generalize poorly.

### 🧠 Example:
In the overfitted model, a dot near a red label might still be incorrectly predicted as red (No Rain), even if it should be blue (Rain), because the model is too specific to training data.

---

## 🛡️ Regularization

Regularization penalizes model complexity to avoid overfitting.

### 🧮 Cost Function with Regularization:

---

## 🧠 Reinforcement Learning

Reinforcement learning (RL) teaches an agent to make decisions by providing feedback:

- Reward (**+ve**)
- Punishment (**–ve**)

The environment gives:

- State

Agent takes action
Receives new state + reward

---

## 🔁 Markov Decision Processes (MDPs)

RL can be modeled as MDPs with:

- Set of States **S**
- Actions **A**
- Transition Model **P(s' | s, a)**
- Reward Function **R(s, a, s')**

### 🧠 Example:
- Grid world
- Agent = Yellow circle
- Goal = Reach green square while avoiding red
- Each square = state
- Moving in any direction = action
- Red = negative reward, Green = positive reward

---

## 🧮 Q-Learning

Q-learning learns the value **Q(s, a)** — expected utility of taking action **a** in state **s**.

### 🔧 Update Rule:

- Q(s, a) ← Q(s, a) + α * (r + γ * max(Q(s', a')) - Q(s, a))

- Where:

- **α**: learning rate
- **γ**: discount factor
- **r**: reward
- **s'**: new state

### 💡 Explore vs Exploit:
- **Exploit**: Choose best known action
- **Explore**: Try new actions for better learning

### 🧠 ε-greedy algorithm:
- With probability **ε**: random action (explore)
- With probability **1-ε**: best known action (exploit)

### 🔢 Q-Learning Example 1: Gridworld
- States: Grid cells
- Actions: Up, Down, Left, Right
- Rewards:
  - +10 for reaching goal
  - -1 per move
  - -10 for hitting wall or trap
- Agent learns which path gives maximum reward

### 🔢 Q-Learning Example 2: Game of Nim
- Train agent via self-play
- Reward: +1 for win, -1 for loss
- Initially plays randomly
- After thousands of games, learns optimal strategy

---

## 📈 Function Approximation

In complex games like Chess, explicitly storing **Q(s, a)** is infeasible.

### Solution:
- Use function approximators (e.g., Neural Networks)
- Generalize Q-values from similar states/actions

---

## 🧠 Unsupervised Learning

Unsupervised learning finds patterns in unlabeled data.

### 🔗 Clustering
Groups similar data points together.

### 🧠 Use cases:
- Genetics (similar genes)
- Image segmentation (similar pixel regions)

---

## 📊 k-means Clustering

### Algorithm:
- Randomly initialize **k** cluster centers
- Assign each point to nearest cluster center
- Recalculate cluster centers as mean of assigned points
- Repeat until assignments no longer change

---

## ✅ Summary Table of Learning (Part 2)

| Topic                  | Type         | Notes                                   |
| :--------------------- | :----------- | :-------------------------------------- |
| Overfitting            | Supervised   | Model fits training data too closely    |
| Regularization         | Supervised   | Penalize complex models                 |
| Cross Validation       | Supervised   | Evaluate generalization                 |
| scikit-learn           | Supervised   | Popular ML library                      |
| Reinforcement Learning | Reinforcement| Feedback-based learning via reward/punishment |
| Q-Learning             | Reinforcement| Value-based learning, epsilon-greedy policy |
| Function Approximation | Reinforcement| Generalize across similar states/actions|
| Clustering             | Unsupervised | Group data without labels               |
| k-means                | Unsupervised | Partition data into k clusters 