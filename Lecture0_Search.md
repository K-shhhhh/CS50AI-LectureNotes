# 🧠 CS50 AI - Lecture 0: Search

This lecture explores the foundational ideas of **search in Artificial Intelligence** — how an agent finds its way from an initial state to a goal state using different strategies

---

## 📌 What is Artificial Intelligence?

Artificial Intelligence (AI) includes a wide range of techniques where computers simulate intelligent behavior.

### Examples:
- 📷 Face recognition (e.g., social media)
- ♟️ Game-playing (e.g., chess)
- 🎤 Voice assistants (e.g., Siri, Alexa)

### Key Areas Covered in the Course:
- **Search** – Finding solutions to problems (like maps, games)
- **Knowledge** – Representing and reasoning from facts
- **Uncertainty** – Using probabilities in uncertain situations
- **Optimization** – Finding the best solution, not just any
- **Learning** – Improving performance over time (e.g., spam filters)
- **Neural Networks** – Brain-inspired models for pattern recognition
- **Language** – Understanding and producing human language

---

## 🔍 Search Fundamentals

### Search Problem Components

| Concept            | Meaning                            | Example                         |
|--------------------|------------------------------------|----------------------------------|
| **Agent**          | Entity making decisions            | A car navigating a map           |
| **State**          | Situation or configuration         | Puzzle layout                    |
| **Initial State**  | Where we start                     | Current location                 |
| **Actions(s)**     | What can be done                   | Slide tiles, move forward        |
| **Transition Model** | What happens after action        | New puzzle layout                |
| **Goal Test**      | Has the goal been reached?         | Destination reached              |
| **Path Cost**      | How costly the path is             | Shortest route                   |

---

## 🧱 Nodes and Frontier

- A **Node** stores the current state, its parent, the action that led there, and the path cost.
- The **Frontier** manages which nodes to explore next.
- The algorithm explores nodes from the frontier, expands them, and adds new nodes until it reaches the goal.

---

## 🔄 Search Algorithms

### 1. Depth-First Search (DFS)
- 📦 Uses: **Stack (LIFO)**
- Explores one path as deep as possible before backtracking.

**Pros:**
- Fast if lucky
- Low memory usage

**Cons:**
- Not optimal
- May go in wrong direction forever

**Real-World Use:** Game trees, puzzles

---

### 2. Breadth-First Search (BFS)
- 📦 Uses: **Queue (FIFO)**
- Explores all nodes at the current depth before moving deeper.

**Pros:**
- Guarantees shortest path
- Complete

**Cons:**
- Memory intensive

**Real-World Use:** Shortest path in maps

---

### 3. Greedy Best-First Search
- Uses a **heuristic function h(n)** to estimate the cost to the goal.
- Always chooses the node closest to the goal based on estimate.

**Pros:**
- Fast with good heuristic

**Cons:**
- Not optimal
- Can go down bad paths

**Real-World Use:** Mazes, navigation AI

---

### 4. A* Search
- Combines:
  - `g(n)` = actual cost so far
  - `h(n)` = estimated cost to goal
  - `f(n) = g(n) + h(n)`

**Pros:**
- Optimal if `h(n)` is admissible and consistent
- Efficient with good heuristic

**Cons:**
- High memory usage

**Real-World Use:** GPS, robotics, pathfinding

---

## 🎮 Adversarial Search

Used when an agent must make decisions against an **opponent**, such as in games like Tic Tac Toe or Chess.

---

### Minimax Algorithm

- Alternates between **maximizer** and **minimizer**.
- Assigns:
  - +1 if maximizer wins
  - -1 if minimizer wins
  - 0 if tie
- Explores all possible game outcomes recursively.
- Chooses action that maximizes or minimizes the expected value.

**Real-World Use:** Board games, decision-making AI

---

### Alpha-Beta Pruning

- Optimizes minimax by pruning branches that won’t affect the final decision.
- Saves time by ignoring obviously bad options.

**Real-World Use:** Chess engines, large game trees

---

### Depth-Limited Minimax

- Only explores a limited depth.
- Uses an **evaluation function** to estimate state utility.
- Commonly used when game trees are too large (e.g., Chess, Go).

**Real-World Use:** Advanced board games with large possibilities

---

## 📊 Summary Table of Search Algorithms

| Algorithm               | Type         | Optimal | Speed               | Memory Usage | Heuristic Used     | Complete | Real-World Use                      |
|-------------------------|--------------|---------|---------------------|----------------|--------------------|----------|-------------------------------------|
| Depth-First Search      | Uninformed   | ❌      | ⚡ Fast (best case)  | 🧠 Low         | ❌                 | ❌       | Game trees, puzzles                 |
| Breadth-First Search    | Uninformed   | ✅      | 🐢 Slower            | 🧠 High        | ❌                 | ✅       | Maps, shortest path problems        |
| Greedy Best-First       | Informed     | ❌      | ⚡ Fast              | 🧠 Medium      | ✅                 | ❌       | Mazes, AI navigation                |
| A* Search               | Informed     | ✅      | ⚡ Fast (good h)     | 🧠 High        | ✅                 | ✅       | GPS, robotics, games                |
| Minimax                 | Adversarial  | ✅ (small games) | 🐌 Slow     | 🧠 High        | ❌                 | ✅       | Tic Tac Toe, board games            |
| Alpha-Beta Pruning      | Adversarial  | ✅      | ⚡ Faster than Minimax | 🧠 Medium   | ❌                 | ✅       | Chess, complex board games          |
| Depth-Limited Minimax   | Adversarial  | ❌ (estimates) | ⚡ Scalable     | 🧠 Medium      | ✅ (evaluation)    | ❌       | Chess, Go, Checkers                 |

---

✅ **End of Lecture 0 Notes**
