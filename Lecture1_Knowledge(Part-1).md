# 🧠 CS50 AI – Lecture 1: Knowledge (Part 1)

In this lecture, we explore how an AI agent **represents knowledge** and uses **logic to reason** and make conclusions — just like humans!

---

## 🧑‍🎓 What is a Knowledge-Based Agent?

A **Knowledge-Based Agent** stores facts (sentences) and uses **logic** to reason about the world.

🧠 Think: _“Given what I know, what else must be true?”_

---

## 🧙 Harry Potter Example: Logic in Action

Let's say we are given these 3 statements:

1. _If it didn’t rain, Harry visited Hagrid today._
2. _Harry visited Hagrid or Dumbledore today (but not both)._
3. _Harry visited Dumbledore today._

Based on this, we can conclude:

➡️ Harry **did not** visit Hagrid.  
➡️ That means **it rained today** ✅

📌 **This is called reasoning using logic.**

---

## 🔠 What is a Sentence?

A **sentence** is a fact or assertion stored in a language that the AI can use to infer new knowledge.

---

## 📚 Propositional Logic

A system of logic where:

- Every statement is either **True** or **False**
- Uses **symbols** to represent statements
- Combines them using **logical connectives**

---

## 🔣 Propositional Symbols

Used to represent facts:
```
P = It is raining  
Q = Harry visited Hagrid
R = Harry visited Dumbledore
```

---

## 🧩 Logical Connectives

Logical symbols used to build complex sentences.

---

### ❗ NOT (¬)

Flips the truth value.

| P      | ¬P     |
|--------|--------|
| False  | True   |
| True   | False  |

---

### 🟢 AND (∧)

True **only if both** statements are true.

| P      | Q      | P ∧ Q |
|--------|--------|--------|
| False  | False  | False  |
| False  | True   | False  |
| True   | False  | False  |
| True   | True   | ✅ True  |

---

### 🔵 OR (∨)

True if **at least one** statement is true.

| P      | Q      | P ∨ Q |
|--------|--------|--------|
| False  | False  | ❌ False  |
| False  | True   | ✅ True   |
| True   | False  | ✅ True   |
| True   | True   | ✅ True   |

> 🔍 This is **inclusive OR** (can be both true).
> 💡 **Exclusive OR (XOR)** is true only when **exactly one** is true.

---

### 👉 IMPLICATION (→)

_"If P, then Q"_  
Only **False** when P is True but Q is False.

| P      | Q      | P → Q |
|--------|--------|--------|
| False  | False  | ✅ True  |
| False  | True   | ✅ True  |
| True   | False  | ❌ False |
| True   | True   | ✅ True  |

📌 When the **condition is False**, the statement is considered **"trivially true"**.

---

### 🔁 BICONDITIONAL (↔)

_"P if and only if Q"_  
True when both have the **same** truth value.

| P      | Q      | P ↔ Q |
|--------|--------|--------|
| False  | False  | ✅ True  |
| False  | True   | ❌ False |
| True   | False  | ❌ False |
| True   | True   | ✅ True  |

---

## 🧪 Models

A **model** is a truth assignment for each proposition.

📌 If we have:
```
P = It is raining  
Q = It is Tuesday
```

Possible models (2 symbols → 2² = 4 models):

- P = T, Q = T  
- P = T, Q = F  
- P = F, Q = T  
- P = F, Q = F

---

## 🧠 Knowledge Base (KB)

A **Knowledge Base** is the set of facts (sentences) the AI knows.

> Example:  
> KB = { P, Q, P → R }

---

## 🔗 Entailment (⊨)

If **KB ⊨ α**, then whenever KB is true, **α must also be true**.

🧠 Example:
```
KB: "It is Tuesday in January"  
α: "It is January"

✅ KB ⊨ α
```

This is different from implication:
- Implication: A logical connector
- Entailment: A **guarantee** of truth

---

## 🧮 Inference

🧠 **Inference** = Drawing new conclusions from existing knowledge

➡️ Example: In Harry Potter, we **inferred** “It rained today” from 3 other statements.

---

## 🔍 Model Checking Algorithm

To check if:
```
KB ⊨ α
```

We do:

1. **Enumerate all models**
2. **In each model**, check:
   - If KB is **true**
   - Then α must also be **true**
3. If true in all such models → ✅ Entailment holds!

---

## 📊 Example Model Checking

Given:

- P = It is Tuesday  
- Q = It is raining  
- R = Harry goes for a run  
- KB = (P ∧ ¬Q) → R  
- α = R  

### All possible models (P, Q, R):

| P     | Q     | R     | KB = (P ∧ ¬Q) → R |
|--------|--------|--------|------------------------|
| False | False | False | ✅ True                 |
| False | False | True  | ✅ True                 |
| False | True  | False | ✅ True                 |
| False | True  | True  | ✅ True                 |
| True  | False | False | ❌ False                |
| True  | False | True  | ✅ True                 |
| True  | True  | False | ✅ True                 |
| True  | True  | True  | ✅ True                 |

✅ ✅ Since only **one model** satisfies the KB and in that model **R is true**, we conclude:
```
KB ⊨ R ✅
```

---

## 💻 Coding Representation

Using the `logic.py` helper functions from CS50:

```python
from logic import *

# Symbols
rain = Symbol("rain")
hagrid = Symbol("hagrid")
dumbledore = Symbol("dumbledore")

# Knowledge Base
knowledge = And(
    Implication(Not(rain), hagrid),
    Or(hagrid, dumbledore),
    Not(And(hagrid, dumbledore)),
    dumbledore
)
```

---

## 🧠 The `check_all()` Recursive Function

This function explores all models and checks whether `query` is true **in all models where `KB` is true**.

```python
def check_all(knowledge, query, symbols, model):
    if not symbols:
        if knowledge.evaluate(model):
            return query.evaluate(model)
        return True
    else:
        remaining = symbols.copy()
        p = remaining.pop()
        model_true = model.copy()
        model_true[p] = True
        model_false = model.copy()
        model_false[p] = False
        return (check_all(knowledge, query, remaining, model_true) and
                check_all(knowledge, query, remaining, model_false))
```

🧠 This is a recursive function that:
- Tries **every combination** of True/False values for the propositions
- Only considers models where **KB is true**
- Returns True if **query is always true** when KB is true

---

✅ End of Part 1 – Lecture 1 Notes

Coming up next in Part 2:
- 🧩 Logical Equivalences
- 🔁 Resolution
- 📦 Horn Clauses
- 🔍 Forward/Backward Chaining

