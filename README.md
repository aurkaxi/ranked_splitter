# Ranked Money Splitter

A calculator for any situation where you have a fixed pot of money and people who deserve different amounts based on rank — and some ranks have more than one person in them.

Prize money. Salami. Bonuses. Scholarship pools. Tournament payouts. Anything where rank matters and equal split is the wrong answer.

---

## The Problem

Equal split ignores rank. Manual calculation ignores headcount. Most tools make you do one or the other by hand.

This calculator solves both at once. You set the total, define your ranks, say how many people are in each, and it figures out exactly how much each person gets — with rank always respected regardless of group size.

Some real situations this handles:

- **Prize money** — 1st place gets more than 2nd, but there are 3 people tied for 3rd. What does each of them get?
- **Salami / Eidi** — your crush is rank 1, cousins are rank 3. Six cousins should not collectively get more than one crush.
- **Team bonuses** — senior tier, mid tier, junior tier. Different headcounts, different amounts per person.
- **Tournament payouts** — top 8 split a pot. Ranks 1 through 8, some brackets have multiple players.

---

## How It Works

### Step 1 — Make your ranks and fill them

You create groups, one per rank. Each group has a name and either a headcount or a list of individual names.

| Rank | Group | People |
|------|-------|--------|
| 1 | 1st place | 1 |
| 2 | 2nd place | 1 |
| 3 | 3rd place | 3 |
| 4 | Participation | 8 |

Rank 1 is the top. Drag to reorder, arrow buttons also work.

---

### Step 2 — Weights are assigned automatically

Each rank gets a weight. With 4 groups:

| Rank | Weight |
|------|--------|
| 1 | 4 |
| 2 | 3 |
| 3 | 2 |
| 4 | 1 |

Top rank gets weight N, next gets N−1, all the way down to 1. No configuration needed — it is derived entirely from how many groups you have.

This is called **triangular number weighting**. The sum of all weights (1+2+3+4 = 10) is always N×(N+1)÷2, the Nth triangular number.

---

### Step 3 — The base unit

Instead of splitting money by group, the calculator splits by **each person weighted by their rank.**

```
pool = (weight₁ × people₁) + (weight₂ × people₂) + ...
unit = total ÷ pool
```

Using the example above with ৳10,000:

```
pool = (4×1) + (3×1) + (2×3) + (1×8)
     = 4 + 3 + 6 + 8
     = 21

unit = 10000 ÷ 21 = 476.19...
```

Think of `unit` as the value of one point. Everyone earns points equal to their rank weight. The unit converts points into money.

---

### Step 4 — Each person's amount

```
per person = weight × unit, rounded to nearest ৳5
```

```
1st place (rank 1):    4 × 476.19 = 1904.76  →  ৳1905 or ৳1900
2nd place (rank 2):    3 × 476.19 = 1428.57  →  ৳1430 or ৳1425
3rd place (rank 3):    2 × 476.19 =  952.38  →   ৳955 or ৳950  (each, ×3 people)
Participation (rank 4): 1 × 476.19 =  476.19  →   ৳480 or ৳475  (each, ×8 people)
```

Every person within the same rank gets the same per-head amount. Rank 1 always beats rank 2 per person, no matter how many are in either group.

---

### Step 5 — Rounding

Raw math gives decimals. Nobody hands out ৳952.38.

Every amount is rounded to a multiple of your chosen step. You pick the step from a fixed set:

**5 / 10 / 50 / 100 / 200 / 500 / 1000**

Smaller steps mean more precise distribution. Larger steps mean cleaner, rounder amounts — useful when payouts are large and you want nice numbers like ৳5000 or ৳10,000.

**Round down** — stays within your budget. A small amount will be left unspent. This is the default.

**Round up** — slightly more generous per person. You may need a small amount of extra cash beyond your stated budget.

The summary below the balance input shows you exactly how much is going out and whether you have leftover or need extra — before you hand anything to anyone.

---

## Why Not Just Divide Equally Per Group

The naive approach gives each group a budget slice, then splits that slice among the people inside. This creates a broken outcome: a rank-1 group with 5 people ends up paying each person less than a rank-4 group with 1 person. Rank collapses under headcount.

The correct approach weights each **person** by rank, not each group. Group size only determines the total flowing out to that group — it never touches what any individual receives. Rank holds.

---

## Quick Example — Prize Money

৳20,000 tournament pot, three prize tiers:

| Tier | Rank | Weight | Players |
|------|------|--------|---------|
| Champion | 1 | 3 | 1 |
| Runner-up | 2 | 2 | 2 |
| Semi-final | 3 | 1 | 4 |

```
pool = (3×1) + (2×2) + (1×4) = 3 + 4 + 4 = 11
unit = 20000 ÷ 11 = 1818.18

Champion:   3 × 1818.18 = 5454.54  →  ৳5450
Runner-up:  2 × 1818.18 = 3636.36  →  ৳3635  each  (×2 = ৳7270 total)
Semi-final: 1 × 1818.18 = 1818.18  →  ৳1815  each  (×4 = ৳7260 total)

Total going out: 5450 + 7270 + 7260 = ৳19,980
Stays in your pocket: ৳20
```

Champion walks away with ৳5450. Each runner-up gets ৳3635. Each semi-finalist gets ৳1815. Clean, fair, no spreadsheet needed.

---

## Built with

Vanilla HTML, CSS, JS. No frameworks. Data stored in your browser's localStorage — nothing leaves your device. Fast, offline-friendly, one file.