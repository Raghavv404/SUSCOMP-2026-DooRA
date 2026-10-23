# Project plan 1: P1 twisted, scheduling heavy, non-urgent compute to Slovenia's cleanest hours

## Problem and goal

**Problem.** The CO2 cost of 1 kWh from the Slovenian grid changes hour by hour, because the mix of nuclear, hydro, coal, gas and solar changes. Heavy but non-urgent compute jobs — batch LLM inference, a full project rebuild, a batch video transcode — ignore this and just run the moment we hit "start." Big companies already shift this kind of flexible work to clean hours (Google's carbon-intelligent computing compares a day-ahead carbon forecast with a demand forecast; Zeus and the Green Software Foundation's Carbon Hack winners shift or throttle ML training). Nobody has checked whether this is worth it for a single developer's laptop, where the loads are much smaller and the forecast has to be cheap enough to be worth running at all.

Laptop charging and a single short video export were considered and dropped: their power draw is tens of watts for minutes, so the CO2 at stake is a few grams — not enough signal to say anything meaningful against meter noise and grid-intensity variance. Heavy, genuinely deferrable jobs (batch LLM inference, a clean project build) draw sustained load for tens of minutes to hours, which gives a real signal and matches how companies actually use carbon-aware scheduling: on background jobs nobody is waiting on right now.

**Goal.** Build a working scheduler that forecasts tomorrow's carbon intensity for Slovenia and, for a queue of heavy non-urgent jobs, decides when within a deadline to run them. Measure the real CO2 difference against running the same jobs immediately, the way we normally would.

**Research question.** At single-laptop scale, does carbon-aware scheduling of heavy background jobs save real CO2, or do forecast errors, the scheduler's own overhead, and Slovenia's low grid variance eat the savings?

## Topic mapping

- **Base topic: #1 Hybrid Microgrid System (SDG 7, 13).** The original idea: forecast energy production from weather, then redirect surplus energy to charging and computers.
- **What we keep:** the forecast-then-redirect structure, and "redirect to computers."
- **What we change:** no own wind turbine or PV panel. The supply is the Slovenian grid; "surplus" means hours with low carbon intensity instead of hours with excess local generation. What gets redirected is not power into a battery, but the execution time of heavy compute jobs — the scheduler decides *when* to run them, not where power flows.
- **Course ties:** energy basics (week 2), software carbon intensity (week 6), rebound effect (weeks 7–8), measuring cloud emissions lab (week 11).

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
| A | Data pipeline: hourly generation mix for Slovenia from the ENTSO-E Transparency Platform, turned into hourly carbon intensity with published lifecycle emission factors (IPCC AR5). Also pull ENTSO-E day-ahead wind/solar and load forecasts. | Core |
| B | Forecaster: predict tomorrow's 24 hourly carbon values. Compare a naive baseline ("same as yesterday") with a simple regression on the ENTSO-E forecasts. Judge it by error and by decision quality: did it pick a truly clean window? | Core |
| C | Two workload types, both genuinely deferrable: (1) **batch LLM inference** — run a fixed set of ~50–100 prompts through a local model (Ollama or MLX) in one sitting; (2) **clean project build** — `make clean && make` (or equivalent) on a large open-source repo, same repo state every run. Both are scripted to be fully reproducible and timed. | Core |
| D | Scheduler: each evening, given a job and a deadline (e.g. "done by 9am"), compare the carbon forecast against the deadline window and pick the lowest-carbon slot to launch the job. | Core |
| E | Measurement: two phases on the same laptop. Phase 1 baseline — jobs launched immediately at realistic trigger times (e.g. right after we'd naturally queue them, evening/late night) regardless of carbon intensity. Phase 2 — same jobs, same deadlines, launched by the scheduler. A wall power meter on the laptop's charger logs energy per run; CO2 = energy × the grid intensity at the time the job actually ran. Also compute an "oracle" schedule with perfect hindsight as the upper bound, and a "worst case" (always the dirtiest slot in the deadline window) as the lower bound. | Core |
| F | Analysis: scheduler overhead (forecaster run cost, wait-loop idle power), production-based vs consumption-based intensity (Slovenia imports a lot), whether job type (LLM vs build) changes the result, rebound effect, SDG discussion. | Core (desk work) |

Measurement note: the laptop is a Mac (Apple Silicon), so pyRAPL does not run. Whole-system energy comes from the wall power meter on the charger; `sudo powermetrics` gives the CPU/GPU/ANE power breakdown during each job, to show where the energy actually goes.

## Extensions (outside the core scope)

Ranked by value for effort. None of them may delay Parts A–F.

| # | Extension | What we do | When | Condition |
|---|---|---|---|---|
| 1 | Data cross-check | Compare our computed intensity with electricityMaps for Slovenia. | Week of 9 Nov | Free access to electricityMaps data for SI (verify) |
| 2 | Third workload | Add a batch video transcode (folder of clips, ffmpeg) as a third job type. | Week of 23 Nov | On schedule at Update 2 |
| 3 | Second device | Run the same two workloads on Raghav's laptop, compare results across hardware. | After Update 2 (25 Nov) | On schedule at Update 2 |
| 4 | Scale-up estimate | Estimate savings if all FRI students ran their heavy background jobs this way, with clear assumptions. | Week of 7 Dec | None |
| 5 | Open tool | Publish the scheduler as a small open-source script with a README. | Week of 4 Jan | Code is clean enough |

## Week by week

| Week of | Work | Course tie-in | Output in repo |
|---|---|---|---|
| 19 Oct | Register topic. Request ENTSO-E API token. Buy a wall power meter. Pick and pin down the two workloads (model + prompt set; repo + build command), confirm both run reliably and reproducibly. Write and submit the proposal. | Proposal template; embodied emissions lab: start a table for laptop and meter | `proposal/proposal.pdf`, `docs/log.md` started |
| 26 Oct | **Start Phase 1 baseline**: run both workloads at realistic immediate-trigger times, log meter energy per run. Build Part A pipeline and backfill 1 year of history. | — | `src/entsoe_fetch.py`, `data/raw/meter/`, `data/raw/entsoe/` |
| 2 Nov | Hourly carbon intensity series. First plot: daily min/max range for Slovenia. **Update 1.** | Update 1 slides: Overview & Early Activities; Progress & Next Steps | `notebooks/01_intensity.ipynb`, `updates/update1.pdf` |
| 9 Nov | End Phase 1 (enough runs of both workloads, aim for 10+ each). Build Part B forecasters, back-test on history. | Measuring software lab: try `profiler.py` on the forecaster | `notebooks/02_forecast.ipynb` |
| 16 Nov | Build Part D scheduler. Dry run for a few days (log decisions only, no real launches). | Rebound archetypes lab: list our project's rebound risks | `src/scheduler.py` |
| 23 Nov | **Start Phase 2**: scheduler launches both workloads for real. **Update 2.** | Update 2 slides: Progress Summary; Adjustments & Next Steps | `updates/update2.pdf`, `data/raw/meter/phase2/` |
| 30 Nov | Phase 2 continues. Break down each job's energy with `powermetrics` (CPU vs GPU vs ANE). | Networking/IoT lecture | `results/jobs_breakdown.md` |
| 7 Dec | End Phase 2 (enough runs of both workloads). Compute CO2 for baseline, scheduler, oracle and worst case. Part F: overhead, imports, job-type comparison, rebound. | Datacentre lecture; measuring cloud emissions lab | `results/co2_compare.md`, `results/overhead.md` |
| 14 Dec | Final figures. Report sections Methodology and Results drafted. **Update 3.** | Update 3 slides: Outcomes; Summary & Final Work | `updates/update3.pdf`, `report/` draft |
| 21 Dec | Finish the full report draft. Review it as the professor would. | LCA and greenwashing lecture: check our claims | `report/report.pdf` draft |
| 28 Dec | Holiday buffer. Fix review comments only. | — | — |
| 4 Jan | Final report, slides in the FRI template, two timed rehearsals. Check every figure regenerates from the repo. | Project finalization | `report/report.pdf`, `slides/final.pptx` |
| 11 Jan | **Submit and present.** | — | Tagged final commit |

## Repo layout

```
proposal/   updates/   report/   slides/
data/raw/entsoe/   data/raw/meter/   data/processed/
src/        notebooks/        results/
docs/protocol.md   docs/log.md   README.md
```

- `docs/log.md`: dated entries of what each person did; this feeds Author Contributions.
- `docs/protocol.md`: laptop model, meter model, the exact workloads (model name + prompt set; repo + commit + build command), deadline window used for scheduling, how a "run" is defined and timed.
- Commit after every work session, with a message saying what changed. Meter logs and job timing CSVs are small, so they go in the repo directly.

## Who does what (to decide; record in the log)

- **Person A:** TODO
- **Person B:** TODO
- **Both:** proposal, Part F, report writing.

## Risks and fallbacks

| Risk | Fallback |
|---|---|
| ENTSO-E token delayed | Use electricityMaps or public historical CSVs for Part A; switch later |
| LLM model too large for the laptop's memory | Drop to a smaller model (e.g. 7–8B instead of larger); document the constraint |
| Build job finishes too fast to be "heavy" | Pick a bigger repo, or loop the build N times back-to-back |
| Daily carbon range in Slovenia is small | Report it as a finding: shifting matters less in a low-variance grid. Compare with a high-variance grid (e.g. Germany) in simulation |
| Forecaster is no better than "same as yesterday" | Keep the naive baseline in the scheduler; the forecaster comparison is still a result |
| Wall meter has no usable log/API | Read values manually at job start/end, log by hand into a CSV |
| Behind schedule at Update 2 | Drop the build workload; keep LLM inference only |
| Extensions fall behind | Drop them in reverse order, 5 first |

## Checkpoints

- Each week's "Output in repo" item is in the GitHub repo by Sunday.
- By 9 Nov: at least 10 baseline runs of each workload with meter energy logged, and 1 year of hourly intensity history.
- By 7 Dec: at least 10 scheduler-run runs of each workload, and the baseline vs scheduler vs oracle vs worst-case table is filled.
- By 4 Jan: running the notebooks top to bottom regenerates every figure in the report, and the report is at most 10 pages from Introduction to Conclusion.
