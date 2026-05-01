# Wumpus-World-Agent

# Wumpus World — Dynamic Logic Agent

A web-based Knowledge-Based Agent that navigates a Wumpus World grid using **Propositional Logic** and **Resolution Refutation**. Built with pure Vanilla HTML, CSS, and JavaScript 

---


## Features

- Dynamic grid sizing (user defines Rows × Columns)
- Random placement of Pits, Wumpus, and Gold every episode
- Real-time percept generation (Breeze near Pit, Stench near Wumpus)
- Propositional Logic Knowledge Base (KB) updated on every move
- Resolution Refutation engine to prove cell safety before moving
- Visual grid: Blue = Agent, Green = Safe, Red = Danger, Gray = Unknown
- Metrics dashboard: Inference Steps and current Percepts

---

## How It Works

### 1. Percept Generation
When the agent enters a cell, it receives:
- **Breeze** if any adjacent cell contains a Pit
- **Stench** if any adjacent cell contains the Wumpus
- **Glitter** if the current cell contains Gold

### 2. Knowledge Base (KB)
The agent maintains a KB of CNF clauses. On each visit:
- If **Breeze** at (r,c): add clause `P_r1_c1 ∨ P_r2_c2 ∨ ...` (at least one neighbor has a pit)
- If **no Breeze** at (r,c): add `¬P_nr_nc` for every neighbor (no neighbor has a pit)
- Same logic applies for Stench and Wumpus

### 3. Resolution Refutation
Before moving to an unvisited cell (nr, nc), the agent asks:
> "Is this cell safe? i.e., can I prove ¬P_nr_nc AND ¬W_nr_nc?"

To prove `¬P_nr_nc`:
1. Add assumption `P_nr_nc = TRUE` to a copy of the KB
2. Run the resolution loop: repeatedly pick two clauses and resolve them
3. If the **empty clause** (contradiction) is derived → `P_nr_nc` must be FALSE → no pit
4. Same process repeated for `W_nr_nc`

If both are proven false → cell is **Safe** (marked green).

### 4. Movement Strategy
Priority order:
1. Move to a **proven safe** unvisited cell
2. Move to an **unknown** cell (risky, taken only if no safe option)
3. **Backtrack** to a previously visited cell

---


## Running Locally

No build steps needed. Just open the file:

```bash
# Option 1: open directly
open wumpus_agent.html


---

## Deployment (Vercel)

```bash
npm install -g vercel
vercel --prod
```

Or connect the GitHub repository directly on [vercel.com](https://vercel.com).

---
