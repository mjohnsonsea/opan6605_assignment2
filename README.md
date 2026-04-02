# Georgetown MSBA — OPAN 6605: Modeling Analytics in Operations
## Assignment 3: Monte Carlo Simulation — Airline Revenue Management

---

### Problem Statement

Alpha Airlines must choose a seating configuration for a new DC-717 fleet operating the **Boston → Atlanta → Chicago → Boston** daily circuit. The decision is how many rows to allocate to first class versus tourist class, given stochastic passenger demand.

**Key constraints:**
- The aircraft holds 40 tourist-equivalent rows
- Each first-class row (4 seats) displaces 2 tourist rows (6 seats each)
- Excess demand in either class is lost — no cross-class substitution
- Fixed operating cost: $100,000/day

**Revenue per seat by route:**

| Route   | First Class | Tourist |
|---------|-------------|---------|
| BOS-ATL | $400        | $175    |
| ATL-CHI | $400        | $150    |
| CHI-BOS | $450        | $200    |

**Demand distributions:**

Total seat demand follows a triangular distribution per leg:

| Route   | Minimum | Most Likely | Maximum |
|---------|---------|-------------|---------|
| BOS-ATL | 160     | 180         | 220     |
| ATL-CHI | 140     | 200         | 240     |
| CHI-BOS | 150     | 200         | 225     |

First-class fraction of demand is discrete and independent across legs:

| Fraction    | 5%  | 12% | 15% |
|-------------|-----|-----|-----|
| Probability | 0.2 | 0.5 | 0.3 |

---

### Methodology

The model was built in Microsoft Excel using **@Risk** for Monte Carlo simulation. Each configuration was evaluated over 10,000 simulation trials to estimate the expected daily profit and break-even frequency. The simulation tested all feasible first-class row allocations (0–20 rows) to identify the profit-maximizing configuration.

For each trial:
1. Total demand for each leg is drawn from its triangular distribution
2. First-class fraction is drawn from the discrete distribution (independently per leg)
3. First-class and tourist demand are separated; each is capped at available seat capacity
4. Revenue is computed and the $100,000 fixed cost is subtracted

---

### Results

#### Problem 1 — Seating capacity (4 first-class rows)

| Class       | Rows | Seats |
|-------------|------|-------|
| First Class | 4    | 16    |
| Tourist     | 32   | 192   |
| **Total**   | —    | **208** |

#### Problem 2 — Single-day revenue (4 FC rows, fixed demand scenario)

Given: BOS-ATL demand = 200 (12% FC), ATL-CHI = 175 (5% FC), CHI-BOS = 200 (15% FC)

| Route   | FC Seated | Tourist Seated | Revenue      |
|---------|-----------|----------------|--------------|
| BOS-ATL | 16        | 176            | $37,200.00   |
| ATL-CHI | 8.75      | 166.25         | $28,437.50   |
| CHI-BOS | 16        | 170            | $41,200.00   |
| **Total** | —       | —              | **$106,837.50** |

**Daily profit = $106,837.50 − $100,000 = $6,837.50**

#### Problem 3 — Expected daily profit (3 first-class rows, 10,000 trials)

With 3 FC rows: 12 first-class seats, 204 tourist seats.

**Expected daily profit ≈ $2,838**

#### Problem 4 — Break-even frequency (3 first-class rows)

**≈ 72.64% of days the airline at least breaks even**

#### Problems 5 & 6 — Optimal configuration

The simulation was run for all feasible configurations (0–20 first-class rows):

| FC Rows | FC Seats | Tourist Seats | E[Daily Profit] |
|---------|----------|---------------|-----------------|
| 0       | 0        | 240           | −$11,517        |
| 3       | 12       | 204           | +$2,838         |
| 4       | 16       | 192           | +$6,523         |
| 5       | 20       | 180           | +$9,595         |
| **6**   | **24**   | **168**       | **+$10,400**    |
| 7       | 28       | 156           | +$7,849         |
| 8       | 32       | 144           | +$2,734         |
| 9       | 36       | 132           | −$3,334         |

**Optimal configuration: 6 first-class rows**
**Optimal expected daily profit: ≈ $10,400**

Profit declines sharply beyond 6 FC rows because tourist capacity falls below typical demand, and because the high volume of tourist passengers contributes substantially to total revenue even at lower per-seat fares.

---

### Files

| File | Description |
|------|-------------|
| `Simulation Homework.pdf` | Original assignment prompt (Georgetown McDonough) |
| `simulation_homework.xlsx` | Excel model with @Risk simulation; contains four worksheets: `PROBLEM 1` (deterministic seat calculation), `PROBLEM 2` (deterministic single-day revenue), `SIMULATION` (Monte Carlo model sweeping 0–20 FC rows), and `RiskSerializationData8` (@Risk output data) |

---

### Tools & Skills

- Microsoft Excel with **@Risk** (Palisade) — Monte Carlo simulation, triangular and discrete distributions
- Triangular distribution sampling for stochastic demand
- Sensitivity analysis across discrete decision space (number of first-class rows)
