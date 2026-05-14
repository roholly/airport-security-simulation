# Airport Security Simulation

**Domain:** Operations Research · Discrete-Event Simulation  
**Tools:** Python, SimPy  

---

## The Problem

How many ID checkers and security screening lanes does a busy airport need to keep average passenger wait times under 15 minutes? The answer depends on arrival rates, service times, queue dynamics, and the interaction between two sequential bottlenecks — and it is not obvious without modeling it.

---

## The Model

A two-stage discrete-event simulation built with Python's SimPy library, representing a 9-hour operational day shift with a one-hour warm-up period excluded from steady-state results.

**Stage 1: ID and Boarding Pass Check**  
Passengers arrive following a Poisson process (λ=5 per minute). Each checker has exponential service time with mean 0.75 minutes. Multiple checkers modeled as a shared resource pool.

**Stage 2: Personal Security Screening**  
Passengers dynamically join the shortest available queue. Service time is uniformly distributed between 0.5 and 1.0 minutes per lane.

**Experiment design:** All combinations of 1 to 6 servers at each stage were tested (36 configurations total), with 50 independent replications per configuration. Results include warm-up and steady-state averages, utilization levels, and whether each configuration meets the 15-minute target.

---

## Validation

Before drawing conclusions, simulated service times were validated against their theoretical distributions using Q-Q plots, histograms, and Kolmogorov-Smirnov tests.

Stage 1 (Exponential): observed mean 0.7489 vs. expected 0.75, KS p-value 0.0664. Pass.  
Stage 2 (Uniform): observed mean 0.7498 vs. expected 0.75, KS p-value 0.1408. Pass.

---

## Results

| Stage 1 Agents | Stage 2 Lanes | Steady-State Wait (min) | Meets Target |
|---|---|---|---|
| 1-3 | any | >60 min | No |
| 4 | 4 | 3.94 | Yes |
| 4 | 5 | 3.14 | Yes |
| 5+ | 4+ | <2 min | Yes (diminishing returns) |

The minimum configuration meeting the 15-minute target is 4 ID checkers and 4 screening lanes, with an average steady-state wait of 3.94 minutes (2.36 at Stage 1, 1.58 at Stage 2).

Stage 1 is the bottleneck. With fewer than 4 Stage 1 servers, utilization exceeds capacity and queues grow without bound regardless of how many screening lanes are added. Once Stage 1 reaches 4 servers, total wait times drop sharply. Beyond 4 servers at either stage, improvements are marginal.

An exploratory run at λ=50 (a busier airport) suggests approximately 36 agents per stage would be needed to meet the same target, though only 5 replications were run per configuration due to computing constraints.

---

## What This Demonstrates

- Discrete-event simulation design with warm-up period and steady-state analysis
- Systematic configuration testing across 36 server combinations
- Statistical distribution validation before drawing conclusions
- Identifying system bottlenecks and the point of diminishing returns
- Translating simulation results into a concrete operational recommendation

---

*Python/SimPy code available upon request. As Georgia Tech OMSA coursework, it is not posted publicly out of respect for academic integrity for future students.*
