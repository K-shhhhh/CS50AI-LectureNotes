# 🧠 CS50 AI - Lecture 1: Knowledge (Part 2)

This part focuses on how we **represent knowledge** in code and how an AI system can **reason logically** to uncover hidden truths. It introduces **knowledge engineering**, **inference rules**, and **first-order logic**.

---

## 🧱 Knowledge Engineering: Clue Mystery Example

We simulate a **murder mystery** game (like Clue), where:

- One person committed the crime.
- One weapon was used.
- The crime happened in one location.

Let's rename our items to make it easier:
- **People**: George, Max, Lily
- **Tools**: Knife, Wrench, Hammer
- **Locations**: Central Park, Brooklyn, Queens

### ✅ Basic Game Rules (Represented in Logic)

| Category     | Logic Representation                        |
|--------------|---------------------------------------------|
| One person   | `George ∨ Max ∨ Lily`                       |
| One tool     | `Knife ∨ Wrench ∨ Hammer`                   |
| One location | `CentralPark ∨ Brooklyn ∨ Queens`           |

### 🕵️‍♂️ Example Clues

Let’s say you saw these cards:

- George ❌
- Brooklyn ❌
- Wrench ❌

Add to Knowledge Base (KB):

```python
Not(George)
Not(Brooklyn)
Not(Wrench)
```

Later someone guesses:

> Lily did it in Queens with the Hammer

That guess was wrong. So we know:

```
Not(Lily) ∨ Not(Queens) ∨ Not(Hammer)
```

Now someone shows you the card **Max** ⇒ Add: `Not(Max)`

Now only one person remains: ✅ Lily

And if we later learn that **CentralPark** ❌  
We can deduce:

✅ Lily with the **Knife** in **Queens**

---

## 🧠 More Logic Puzzles

### Puzzle: Assigning People to Cities

Imagine 4 people (Alex, Mia, John, Sara) are assigned to 4 cities (NewYork, Chicago, Boston, Miami).

Each **person gets one city**, and each **city has one person**.

### 🧩 Propositional Logic Structure

- **Person-City relationships** like `AlexNewYork`, `MiaBoston`
- For each person:
  ```
  AlexNewYork ∨ AlexChicago ∨ AlexBoston ∨ AlexMiami
  ```
- For exclusivity:
  ```
  AlexNewYork → ¬AlexChicago ∧ ¬AlexBoston ∧ ¬AlexMiami
  ```

This gets very long. That’s where **First Order Logic** helps.

---

## 🎯 Logic Puzzle: Mastermind Game

You guess 4 color positions:
```
Guess 1 → Player says "2 correct positions"
Guess 2 → You swap two colors → Player says "0 correct"
Guess 3 → You revert and fix → Player says "4 correct"
```

To model this in logic:

- Use symbols like: `Red0`, `Blue1`, `Green2`...
- Each symbol means: Color X is at Position Y
- Build KB based on rules like:
  - Only one color per slot
  - Each color used only once
  - Feedback: how many are correct

---

## 🧠 Inference Rules: Smarter Deduction

Model Checking is slow (it checks all possibilities).  
Inference rules are faster: they generate **new facts** from old ones.

---

### 📏 Rule 1: Modus Ponens

| Statement 1 | If it rains, John stays in |
|-------------|-----------------------------|
| Statement 2 | It rains                   |
| ✅ Result    | John stays in              |

If `P → Q` and `P`, then we can infer `Q`.

---

### ✂️ Rule 2: And-Elimination

If `John likes coffee ∧ tea`, then:

- ✅ `John likes coffee`
- ✅ `John likes tea`

---

### ❌ Rule 3: Double Negation Elimination

If `¬(¬P)` ⇒ ✅ P is true.

Example:  
“It’s not true that she didn’t come” ⇒ She came.

---

### ➡ Rule 4: Implication Elimination

`P → Q` is equivalent to: `¬P ∨ Q`

| P | Q | P → Q | ¬P ∨ Q |
|---|---|--------|--------|
| F | F | ✅     | ✅     |
| F | T | ✅     | ✅     |
| T | F | ❌     | ❌     |
| T | T | ✅     | ✅     |

---

### ↔ Rule 5: Biconditional Elimination

`P ↔ Q` ≡ `(P → Q) ∧ (Q → P)`

E.g., “If and only if it's sunny, we go out”

---

### 🔄 Rule 6: De Morgan’s Law

| Statement                     | Equivalent                      |
|------------------------------|----------------------------------|
| ¬(A ∧ B)                     | ¬A ∨ ¬B                         |
| ¬(A ∨ B)                     | ¬A ∧ ¬B                         |

---

### 🧩 Rule 7: Distributive Law

- `(A ∧ B) ∨ C` → `(A ∨ C) ∧ (B ∨ C)`
- `(A ∨ B) ∧ C` → `(A ∧ C) ∨ (B ∧ C)`

---

## 🔍 Inference as Search Problem

| Component      | Meaning                             |
|----------------|--------------------------------------|
| Initial State  | Starting Knowledge Base              |
| Actions        | Inference rules applied              |
| Transition     | New KB after applying an inference   |
| Goal Test      | Check if desired conclusion is met   |
| Path Cost      | Steps needed to reach the conclusion |

---

## 🔧 Resolution Rule

If:
- `John is at the park OR Max is at the mall`
- `John is not at the park`

Then ✅ **Max is at the mall**

---

### 👥 Complementary Literals

These are pairs like `P` and `¬P`.  
When used together, they allow simplification.

---

### 🧱 Clauses

- Clause: OR-connected literals → `(P ∨ ¬Q ∨ R)`
- CNF (Conjunctive Normal Form): AND of clauses  
  → `(A ∨ B) ∧ (¬C ∨ D) ∧ (E ∨ F ∨ ¬G)`

---

## 🔄 Steps: Convert to CNF

1. Eliminate biconditionals  
   `(P ↔ Q)` → `(P → Q) ∧ (Q → P)`
2. Eliminate implications  
   `(P → Q)` → `¬P ∨ Q`
3. Apply De Morgan’s Law  
   `¬(A ∧ B)` → `¬A ∨ ¬B`
4. Distribute ORs over ANDs

### 🧪 Example

Convert `(P ∨ Q) → R` to CNF:

```
(P ∨ Q) → R
→ ¬(P ∨ Q) ∨ R        # Step 2
→ (¬P ∧ ¬Q) ∨ R       # Step 3
→ (¬P ∨ R) ∧ (¬Q ∨ R) # Step 4
```

---

### ❌ Empty Clause

Resolving `(P)` and `(¬P)` gives `()`, the **empty clause**, meaning a contradiction.

---

## 🔁 Resolution Algorithm (Proof by Contradiction)

To check: `Does KB ⊨ α`?

Steps:

1. Add `¬α` to KB
2. Convert all to CNF
3. Use resolution to infer new clauses
4. If you reach `()`, contradiction → ✅ α is entailed

---

## 🧮 Example

Does `(A ∨ B) ∧ (¬B ∨ C) ∧ (¬C)` entail `A`?

1. Add `¬A` to KB
2. KB: `(A ∨ B) ∧ (¬B ∨ C) ∧ (¬C) ∧ (¬A)`
3. Inference:

- From `(¬C)` and `(¬B ∨ C)` ⇒ `¬B`
- From `(A ∨ B)` and `¬B` ⇒ `A`
- Now we have `A` and `¬A` ⇒ contradiction ✅

---

## 🧠 First-Order Logic

A more compact logic system:

| Symbol Type          | Example                     |
|----------------------|-----------------------------|
| Constant Symbol      | `John`, `NYC`, `Pizza`       |
| Predicate Symbol     | `Loves(John, Pizza)`         |

---

### 🔁 Quantifiers

| Type                  | Symbol | Meaning                          |
|-----------------------|--------|----------------------------------|
| Universal Quantifier  | `∀`    | For all                          |
| Existential Quantifier| `∃`    | There exists at least one        |

Example:  
- `∀x. Student(x) → Studies(x)`  
  → Every student studies

- `∃x. Likes(x, IceCream)`  
  → Someone likes ice cream

---

### 📌 Summary

| Concept             | Use                                                |
|---------------------|-----------------------------------------------------|
| Propositional Logic | Representing basic facts using True/False           |
| First-Order Logic   | Representing relationships & abstract reasoning     |
| CNF Conversion      | To prepare knowledge for resolution-based inference |
| Inference Rules     | To derive new knowledge without checking all models |
| Resolution          | Powerful way to reach conclusions                   |

---

✅ **End of Lecture 1 – Part 2 Notes**

