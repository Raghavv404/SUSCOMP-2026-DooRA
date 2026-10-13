claud# CarbonSched: A Forecast-Driven Carbon-Aware Task Scheduler

## 1. Introduction

**Electricity is not always "clean." Its carbon cost changes hour by hour, but software never checks this before running a task.**

- Power grids mix clean energy (solar, wind) and dirty energy (coal, gas). The CO2 cost of using 1 kWh changes during the day.
- Right now, software always runs tasks the moment you click "start." It never asks: "Is this a good time, carbon-wise?"
- We will build CarbonSched, a tool that delays non-urgent tasks (video export, backups, model training) to hours when the power grid is cleanest.
- This project supports **SDG 7** (clean energy — better use of energy we already produce) and **SDG 13** (climate action — direct emission cuts).
- No new hardware is needed. Any developer can install this tool today.

## 2. Related Work

**Big tech companies already proved this idea works — we bring it down to a size one student team can build and test.**

- Google shifts data center workloads to low-carbon hours. Result: 25% less CO2.
- The Green Software Foundation ran a hackathon (CarbonHack22). Winning tools **Lowcarb** (-13% CO2) and **Zeus** (-24% CO2) proved carbon-aware scheduling works for AI training.
- ING built a scheduler called SCALE for their own data centers. Result: 20% less CO2.
- **Gap we fill**: all these tools are built for big companies with big data centers. There is no simple, open tool that one developer can run locally, test with real tasks, and measure with their own hardware. We build that.

## 3. Proposed Approach / Methodology

**We build, test, and measure a working scheduler — not just a design on paper.**

1. Pick 3–5 everyday compute tasks (e.g., video export, file compression, a small ML training run).
2. Connect to a free carbon-intensity API (electricityMaps or WattTime) to get real-time and forecast CO2-per-kWh data for Slovenia.
3. Build the scheduler: user marks a task "not urgent," sets a deadline, and the tool picks the lowest-carbon time slot before that deadline.
4. Add a small forecasting model (e.g., Prophet or simple moving average) to predict carbon intensity a few hours ahead — this lets the scheduler plan instead of just reacting.
5. Measure real energy use of each task with pyRAPL (or Intel Power Gadget).
6. Compare: same task run immediately vs. run through CarbonSched. Calculate CO2 saved.
7. Also measure the forecasting model's own energy cost, to check if the "smart" part is actually worth it.

## 4. Milestones

| Date | Deliverable |
|---|---|
| 20.10.2026 | Proposal submitted |
| 03.11.2026 | API connected, pyRAPL measurement working on 1 task |
| 24.11.2026 | Core scheduler working on 3 tasks, first before/after numbers |
| 05.01.2027 | Forecasting model added, full test on 5 tasks, report draft |
| 08.01.2027 | Final report, code, and poster submitted |
| 11.01.2027 | Presentation |

## 5. Evaluation Plan

**Success means a real, measured drop in CO2 — not just a working demo.**

- Main metric: % CO2 reduction when using CarbonSched vs. running tasks immediately, across all test tasks.
- Target: match or beat published results (10–25% reduction).
- Check the forecasting model's own energy cost against the CO2 it helps save — if the model costs more than it saves, we report that honestly.
- **Rebound effect discussion**: if people feel their tasks are "green now," they might run more non-urgent tasks than before. We will discuss this risk and whether it could cancel out the savings.

## 6. Resources

**Everything needed is free or already available — no purchase required.**

- Software: Python, pyRAPL / Intel Power Gadget, electricityMaps or WattTime API (free academic tier), Prophet or statsmodels for forecasting.
- Hardware: our own laptops/desktops. No special equipment needed.
- Team roles (to confirm with teammate):
  - Person A: API integration, pyRAPL measurement, data analysis
  - Person B: scheduler logic, forecasting model, evaluation automation

## 7. References

- Green Software Foundation, *CarbonHack22 Results* — https://greensoftware.foundation/articles/carbonhack22-a-big-leap-in-carbon-aware-computing/
- Green Software Foundation, *Carbon Aware SDK* — https://greensoftware.foundation/stories/carbon-aware-sdk/
- Google, *Carbon-Intelligent Computing* (data center load shifting)
- ING, *S.C.A.L.E.: Scheduler for Carbon-Aware Load Execution*
- Survey: *A Survey on Task Scheduling in Carbon-Aware Container Orchestration* — https://arxiv.org/pdf/2508.05949
- electricityMaps API docs — https://www.electricitymaps.com/
- WattTime API docs — https://www.watttime.org/
