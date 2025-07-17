# 🤖 CS50 AI with Python – Lecture 3: **Optimization (Part 1)**

This lecture focuses on **optimization algorithms**—methods used to choose the best option among many possible ones. These are foundational to solving problems efficiently when searching through large solution spaces.

---

## 🚀 What is Optimization?
Optimization = Choosing the **best** outcome from possible options.
- Used in **minimax**, **pathfinding**, **resource allocation**, etc.

---

## 🔍 Local Search
A search algorithm that:
- Starts at a single node (state)
- Moves to **neighboring** states
- Goal: Find a "good enough" answer with **less computation**

### 🏥 Example:
Placing 2 hospitals to minimize Manhattan distances to 4 houses.
- Cost = Sum of distances from each house to the nearest hospital
- State = One configuration of hospital placements

### 📉 State-Space Landscape Terms
- **Objective Function**: To **maximize** value
- **Cost Function**: To **minimize** cost
- **Current State**: The active configuration
- **Neighbor State**: Slight variation of current state (e.g., move hospital one cell)

---

## 🧗 Hill Climbing
A local search strategy:
- Compare current state to neighbors
- Move to better neighbor (higher value or lower cost)
- Repeat until no neighbor is better

### 🧠 Pseudocode:
```python
function Hill-Climb(problem):
    current = initial state
    repeat:
        neighbor = best-valued neighbor of current
        if neighbor not better than current:
            return current
        current = neighbor
```

---

### ⚠️ Limitation:
- Gets stuck in **local minima** (not global best)
- Example: hospital placement improves from cost 17 → 11 but misses optimal cost of 9

---

## 📉 Local vs Global Minima/Maxima

| Term            | Description                     |
|-----------------|---------------------------------|
| Local Maximum   | Better than nearby states       |
| Global Maximum  | Best of all                     |
| Local Minimum   | Worse than nearby states        |
| Global Minimum  | Worst of all                    |

#### Special Cases:
- **Flat Local Max/Min**: Plateau of same-value neighbors  
- **Shoulder**: Plateau with both better and worse neighbors; algorithm stuck in center

---

## 🔁 Variants of Hill Climbing

| Variant            | Description                                                              |
|--------------------|--------------------------------------------------------------------------|
| Steepest-ascent    | Pick best neighbor (default)                                             |
| Stochastic         | Pick randomly from better neighbors                                      |
| First-choice       | Pick first better neighbor found                                         |
| Random-restart     | Try multiple random starts, pick best result                             |
| Local Beam Search  | Track `k` best states at once                                            |

➡ These aim to **reduce chances** of getting stuck, but don't guarantee global optimality.

---

## ❄️ Simulated Annealing

Inspired by metal cooling:
- Start with high "temperature" → allow random moves
- Gradually lower temp → fewer random moves
- Can escape **local optima**

### 🔢 Pseudocode:
```python
function Simulated-Annealing(problem, max):
    current = initial state
    for t = 1 to max:
        T = Temperature(t)
        neighbor = random neighbor
        ΔE = neighbor value - current value
        if ΔE > 0:
            current = neighbor
        else with probability e^(ΔE/T):
            current = neighbor
    return current
```

- `e ≈ 2.72`, ΔE < 0 means worse
- High `T` → more likely to accept worse neighbor
- Low `T` → less likely

---

## 🧭 Traveling Salesman Problem (TSP)

- Find shortest route visiting all cities and returning
- 10 points → 10! = 3.6 million routes
- Simulated annealing gives **good solutions** efficiently

---

## 📐 Linear Programming (LP)

Optimize **linear cost functions** with constraints:

### ✍️ Components
- **Cost Function**: `c1*x1 + c2*x2 + ...`
- **Constraints**:
  - Inequality: `a1*x1 + a2*x2 + ... ≤ b`
  - Equality: `a1*x1 + a2*x2 + ... = b`
- **Bounds**: `li ≤ xi ≤ ui`

---

### 💡 Example

- 2 machines: X1 ($50/hr), X2 ($80/hr)
- Minimize cost → `50x1 + 80x2`

Constraints:
- Labor: `5x1 + 2x2 ≤ 20`
- Output: `10x1 + 12x2 ≥ 90` → rewritten as `-10x1 -12x2 ≤ -90`

---

### 🐍 Python Code:
```python
import scipy.optimize

result = scipy.optimize.linprog(
    [50, 80],
    A_ub=[[5, 2], [-10, -12]],
    b_ub=[20, -90]
)

if result.success:
    print(f"X1: {round(result.x[0], 2)} hours")
    print(f"X2: {round(result.x[1], 2)} hours")
else:
    print("No solution")
```

➡ Libraries like `scipy.optimize` implement solvers like **Simplex** and **Interior-Point**.

---

📌 This concludes **Part 1** of Lecture 3 – covering:
- Local Search
- Hill Climbing
- Simulated Annealing
- TSP
- Linear Programming

🧠 Next up: **Continuous Optimization & Gradient Descent**