# 🤖 CS50 AI with Python – Lecture 3: Optimization (Part 2)

This part focuses on **Constraint Satisfaction Problems (CSPs)** — a powerful framework where variables must satisfy constraints to reach valid solutions.

---

## 📦 What is a Constraint Satisfaction Problem?

A CSP consists of:
- A set of **variables**: x₁, x₂, ..., xₙ
- A **domain** for each variable: D₁, D₂, ..., Dₙ
- A set of **constraints**: C

### 🎲 Examples:
- **Sudoku**:
  - Variables = Empty cells
  - Domain = {1–9}
  - Constraints = No repeats in row, column, box

- **Exam Scheduling**:
  - Variables = Courses (A, B, ..., G)
  - Domain = {Monday, Tuesday, Wednesday}
  - Constraints = Same student can’t have >1 exam per day

### 🧩 Graph Representation:
- Nodes = Variables (Courses)
- Edges = Constraints (Conflict between courses)

---

## 📚 Types of Constraints

- **Hard Constraint**: Must be satisfied for a valid solution
- **Soft Constraint**: Preferred, but not required
- **Unary Constraint**: Involves 1 variable (e.g., A ≠ Monday)
- **Binary Constraint**: Involves 2 variables (e.g., A ≠ B)

---

## ✅ Node Consistency

A variable is **node-consistent** if:
- All values in its domain satisfy **unary constraints**

### 🧠 Example:
- A: {Monday, Tuesday, Wednesday}, constraint: A ≠ Monday → Remove Monday → Node-consistent

---

## 🔁 Arc Consistency

A variable X is **arc-consistent** with Y if:
- Every value of X has a valid corresponding value in Y's domain satisfying the binary constraint (e.g., X ≠ Y)

### 🧠 Example:
- A: {Tuesday, Wednesday}, B: {Wednesday}, constraint A ≠ B
- A = Wednesday → B has no valid value → Not arc-consistent
- Remove Wednesday from A → A = {Tuesday} → Now arc-consistent

---

## 🧮 Pseudocode: Revise

\```python
function Revise(csp, X, Y):
    revised = false
    for x in X.domain:
        if no y in Y.domain satisfies constraint(X, Y):
            remove x from X.domain
            revised = true
    return revised
\```

---

## 🧮 Pseudocode: AC-3

\```python
function AC-3(csp):
    queue = all arcs in csp
    while queue is not empty:
        (X, Y) = Dequeue(queue)
        if Revise(csp, X, Y):
            if X.domain is empty:
                return false
            for Z in X.neighbors - {Y}:
                Enqueue(queue, (Z, X))
    return true
\```

➡ AC-3 enforces global arc consistency but doesn’t guarantee a full solution.

---

## 🔍 CSP as a Search Problem

- **Initial State**: No variables assigned  
- **Actions**: Assign variable = value  
- **Transition Model**: Returns updated assignment  
- **Goal Test**: All variables assigned with all constraints satisfied  
- **Path Cost**: Constant

---

## 🔁 Backtracking Search

Efficient recursive CSP search:

- Assign values one by one  
- If conflict → backtrack to previous variable

### 🔧 Pseudocode:

\```python
function Backtrack(assignment, csp):
    if assignment complete:
        return assignment
    var = Select-Unassigned-Var(assignment, csp)
    for value in Domain-Values(var, assignment, csp):
        if value consistent with assignment:
            add {var = value} to assignment
            result = Backtrack(assignment, csp)
            if result ≠ failure:
                return result
            remove {var = value} from assignment
    return failure
\```

### 🧠 Example: Backtracking Steps

- A = Monday ✅  
- B = Monday ❌ (conflict)  
- B = Tuesday ✅  
- Continue assigning...  
➡ If all values fail → backtrack to previous variable.

---

## 🔄 Inference: Maintaining Arc Consistency (MAC)

Improve efficiency by enforcing arc consistency after every assignment.

### 🔧 Updated Backtrack with Inference:

\```python
function Backtrack(assignment, csp):
    if assignment complete:
        return assignment
    var = Select-Unassigned-Var(assignment, csp)
    for value in Domain-Values(var, assignment, csp):
        if value consistent with assignment:
            add {var = value} to assignment
            inferences = Inference(assignment, csp)
            if inferences ≠ failure:
                add inferences to assignment
                result = Backtrack(assignment, csp)
                if result ≠ failure:
                    return result
            remove {var = value} and inferences from assignment
    return failure
\```

---

## 🧠 Heuristics for Better Performance

### 🎯 Minimum Remaining Values (MRV)
- Pick variable with fewest legal values remaining  
- Fail fast on constrained variables

### 📶 Degree Heuristic
- Pick variable involved in most constraints  
- Reduces options for many other variables

### 🧩 Least Constraining Value (LCV)
- Choose value that rules out the fewest options for others

### 🧠 Example: LCV
- C = Tuesday → constrains B, E, F  
- C = Wednesday → constrains B, E  
✅ Choose Wednesday

---

## 📌 Summary

- CSPs use variables, domains, and constraints to define problems  
- Node/Arc consistency help reduce possibilities early  
- Use Backtracking + Inference (MAC) for efficient solving  
- Apply MRV, Degree, and LCV heuristics to minimize search  

Covered across Lecture 3:
- Local Search  
- Hill Climbing  
- Simulated Annealing  
- Linear Programming  
- Constraint Satisfaction
