# Project plan 2: S1 + #3 twisted, can we trust software "energy" numbers? Auditing and fixing energy estimators on a laptop

## Problem and goal

**Problem.** Projects like S1 ("SW code optimization for energy efficiency") compare algorithms by energy, measured with software estimators instead of a real meter. If an estimator's error is bigger than the real difference between two algorithms, the ranking is wrong and the "greener code" advice is wrong too. This has happened in practice: an independent study found that Kepler, a widely used Kubernetes energy tool, overestimated container power by about 15× against RAPL. Green claims built on wrong numbers become greenwashing, a topic of the course (week 13).

**Goal.** Measure how wrong common energy estimators are on a real laptop, check whether they still rank algorithms correctly, and build a working calibration tool that fixes their output.

**Research question.** Are software energy estimators accurate enough to tell which algorithm is greener? If not, can a one-time calibration against a wall meter fix them?

## Topic mapping

- **Base topic: S1 SW code optimization for energy efficiency.** The original idea: pick problems such as sorting, image processing and shortest path; implement different algorithms; measure CPU, RAM and energy with pyRAPL, Intel Power Gadget or similar.
- **Method borrowed from #3 Examining tools for automated LCA.** #3 checks LCA tools against manufacturer numbers to find over- and under-reporting, then proposes methodology fixes. We do the same for software energy estimators, with a wall meter as the baseline.
- **What we change:** S1 trusts the measuring tool. We test the tool itself, then fix it.
- **Course ties:** where energy is spent in a computer (week 2), embodied emissions (week 3 and lab), measuring software energy (week 7 lab), LCA and greenwashing (week 13).

## Dates (placeholders: replace with the real ones from Učilnica)

| Deliverable | Assumed date |
|---|---|
| Proposal | TBD (changed on Učilnica, check) |
| Update 1 | Wed 4 Nov 2026 |
| Update 2 | Wed 25 Nov 2026 |
| Update 3 | Fri 18 Dec 2026 |
| Final report and presentation | Mon 11 Jan 2027 |

## Scope

| Part | What | Status |
|---|---|---|
| A | Workload suite from S1's own examples: sorting, image processing, shortest path. 2–3 algorithm variants each (e.g. insertion sort vs quicksort vs built-in sort; naive Python blur vs NumPy; Dijkstra with and without a heap). | Core |
| B | Ground truth: wall power meter, minus idle baseline. Fixed protocol: battery full, screen brightness fixed, Wi-Fi state fixed, runs long enough for the meter's resolution, 5 runs plus warm-up. | Core |
| C | Estimators under test, all on the Mac: (1) macOS `powermetrics` (on-chip sensors), (2) CodeCarbon, (3) a CPU-time × TDP model, the method many online calculators use. Metrics: absolute error in %, and ranking agreement with the meter (Kendall's tau): would each tool pick the same "greenest" algorithm? | Core |
| D | Build the fix: `calibrate.py`. One calibration run against the wall meter fits a correction per estimator (idle offset plus scale). Then it corrects new measurements without the meter. Test on workloads not used in calibration: error and ranking before vs after. | Core |
| E | Footprint and analysis: energy of the whole experiment vs the laptop's embodied carbon per hour of use (from Apple's product environmental report). Rebound effect, SDG discussion, a measurement checklist for S1-type projects. | Core (desk work) |

Measurement note: the laptop is a Mac (Apple Silicon), so pyRAPL and Intel Power Gadget do not run. We say this in the report; testing them on an Intel/AMD Linux machine is Extension 1.

## Extensions (outside the core scope)

Ranked by value for effort. None of them may delay Parts A–E.

| # | Extension | What we do | When | Condition |
|---|---|---|---|---|
| 1 | Linux tools | Repeat Part C with pyRAPL on an Intel/AMD Linux machine, same meter. | After Update 2 (25 Nov) | Access to an FRI lab machine where we may plug in our meter |
| 2 | Class adoption | Share `calibrate.py` and the checklist in the course Slack; record how many teams try it. | Week of 30 Nov | Professor agrees |
| 3 | Second device | Repeat calibration on Raghav's laptop; check if one device's correction works on another. | Week of 7 Dec | Different laptop model available |
| 4 | Language comparison | Add the same algorithm in C or Rust to see if language choice outweighs algorithm choice. | Week of 7 Dec | On schedule |
| 5 | Open dataset | Release the measurements and protocol under an open licence. | Week of 4 Jan | Professor agrees the data may be published |

## Week by week

| Week of | Work | Course tie-in | Output in repo |
|---|---|---|---|
| 19 Oct | Register topic. Buy the wall power meter. Install CodeCarbon, test `sudo powermetrics`. Write and submit the proposal. | Proposal template; embodied emissions lab: start a table for the laptop | `proposal/proposal.pdf`, `docs/log.md` started |
| 26 Oct | Write the measurement protocol. Measure idle baseline and meter resolution. Write the Part A workload suite. | — | `docs/protocol.md`, `src/workloads/` |
| 2 Nov | First full run: all workloads, meter only. First plot of real energy per algorithm. **Update 1.** | Update 1 slides: Overview & Early Activities; Progress & Next Steps | `data/raw/meter/`, `updates/update1.pdf` |
| 9 Nov | Run all workloads with the 3 estimators at the same time as the meter. | Measuring software lab: also try `profiler.py` as a 4th estimator | `data/raw/estimators/` |
| 16 Nov | Part C analysis: error per tool, Kendall's tau, "same winner?" table. | Rebound archetypes lab | `notebooks/01_error.ipynb`, `results/error.md` |
| 23 Nov | Build `calibrate.py`; split workloads into calibration and test sets. **Update 2.** | Update 2 slides: Progress Summary; Adjustments & Next Steps | `src/calibrate.py`, `updates/update2.pdf` |
| 30 Nov | Part D test: error and ranking before vs after calibration on test workloads. | Networking/IoT lecture | `notebooks/02_calibration.ipynb`, `results/calibration.md` |
| 7 Dec | Part E: experiment energy vs embodied carbon per hour, rebound, SDG. Write the checklist. | Datacentre lecture; measuring cloud emissions lab | `results/footprint.md`, `docs/checklist.md` |
| 14 Dec | Final figures. Report sections Methodology and Results drafted. **Update 3.** | Update 3 slides: Outcomes; Summary & Final Work | `updates/update3.pdf`, `report/` draft |
| 21 Dec | Finish the full report draft. Review it as the professor would. | LCA and greenwashing lecture: check our claims | `report/report.pdf` draft |
| 28 Dec | Holiday buffer. Fix review comments only. | — | — |
| 4 Jan | Final report, slides in the FRI template, two timed rehearsals. Check every figure regenerates from the repo. | Project finalization | `report/report.pdf`, `slides/final.pptx` |
| 11 Jan | **Submit and present.** | — | Tagged final commit |

## Repo layout

```
proposal/   updates/   report/   slides/
data/raw/meter/   data/raw/estimators/   data/processed/
src/workloads/    src/calibrate.py   notebooks/   results/
docs/protocol.md   docs/checklist.md   docs/log.md   README.md
```

- `docs/log.md`: dated entries of what each person did; this feeds Author Contributions.
- `docs/protocol.md`: laptop and meter model, idle baseline, brightness, battery state, run length, number of runs.
- Commit after every measurement session, with a message saying what changed.

## Who does what (to decide; record in the log)

- **Person A:** TODO
- **Person B:** TODO
- **Both:** proposal, Part E, report writing.

## Risks and fallbacks

| Risk | Fallback |
|---|---|
| Meter resolution too coarse for short workloads | Loop each workload to at least 60 s; report energy per iteration |
| Charging power mixes into the meter reading | Run only at full battery; check the meter shows a stable idle value before each run |
| CodeCarbon does not measure on Apple Silicon (verify) | Report which method it falls back to; that is itself a finding |
| `powermetrics` needs admin rights we cannot use | Use it only on our own laptop, document the command |
| Algorithms differ less than the measurement noise | Report it as a result: the tools cannot separate them; increase input sizes |
| Behind schedule at Update 2 | Drop the TDP model; keep `powermetrics` and CodeCarbon |
| Extensions fall behind | Drop them in reverse order, 5 first |

## Checkpoints

- Each week's "Output in repo" item is in the GitHub repo by Sunday.
- By 9 Nov: protocol written, idle baseline measured, all workloads run at least 5 times with the meter.
- By 30 Nov: before/after calibration table filled for the test workloads.
- By 4 Jan: running the notebooks top to bottom regenerates every figure in the report, and the report is at most 10 pages from Introduction to Conclusion.
