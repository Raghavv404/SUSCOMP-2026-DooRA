<!--
DRAFT proposal, built from plans/plan2_Doohyun.md. Not final.
Follows 01_SUSCOMP_Project_Proposal_Template.pdf: title, authors, date, opening paragraph, then
1 Related Work, 2 Methodology, 3 Milestones, 4 Evaluation Plan, 5 Resources, References. Limit: 2 pages.
Before submitting: verify every "(verify)" item, replace TBD values, convert to PDF.
-->

# Run It When the Grid Is Clean: Carbon-Aware Scheduling of Heavy Background Jobs on a Laptop

Doohyun Kim, Raghav Verma — University of Ljubljana, Faculty of Computer and Information Science

October 2026

The CO2 cost of 1 kWh from the Slovenian grid changes hour by hour, because the mix of nuclear, hydro, coal, gas and solar power changes. Heavy jobs that are not urgent, such as batch LLM inference or a full project rebuild, ignore this. They run the moment we press "start." Large companies already move this kind of flexible work to cleaner hours, but nobody has checked whether it is worth it on a single developer's laptop, where the loads are small and the forecast itself must be cheap to run. This project asks: **at laptop scale, does carbon-aware scheduling of heavy background jobs save real CO2, or do forecast errors, the scheduler's own overhead and Slovenia's grid mix eat the savings?** We will build a scheduler that forecasts tomorrow's carbon intensity and starts each job in the cleanest hour before its deadline, and we will measure the real CO2 difference against running the same jobs immediately. The topic is based on #1 (Hybrid Microgrid System): we keep its "forecast, then redirect flexible load" idea, but the supply is the national grid, and what we move is the start time of computing jobs. It supports SDG 7 (Affordable and Clean Energy) and SDG 13 (Climate Action).

## 1 Related Work

Google's carbon-intelligent computing platform compares a day-ahead forecast of grid carbon intensity with a forecast of its own power demand, and moves non-urgent jobs, such as video processing, to lower-carbon hours [1]. Google reported that this works but did not publish a percentage saving. In the Green Software Foundation's Carbon Hack 2022, the winning tool Lowcarb scheduled federated-learning jobs by carbon intensity and reported 13% lower emissions (organiser-reported) [2]. Zeus, a University of Michigan framework, tunes the GPU power limit and batch size during model training and reports 15.3–75.8% better energy efficiency [3].

**Gap:** these systems run in data centres or GPU clusters. We could not find a measured test of carbon-aware scheduling on one laptop, with a cheap forecast and a real power meter (to be confirmed in our literature review). We also do not know how much it helps in a grid with a large share of nuclear and hydro power, like Slovenia's.

## 2 Methodology

All measurements run on one Mac (Apple Silicon). pyRAPL does not run on it, so we measure whole-system energy with a wall power meter and use `powermetrics` for a CPU/GPU breakdown.

| Part | What we do |
|---|---|
| A. Carbon data | Hourly generation mix for Slovenia from the ENTSO-E Transparency Platform [4], turned into hourly carbon intensity with published lifecycle emission factors (IPCC; verify). We also download ENTSO-E day-ahead wind, solar and load forecasts. |
| B. Forecaster | Predict tomorrow's 24 hourly carbon values. Compare a naive baseline ("same as yesterday") with a simple regression on the ENTSO-E forecasts. |
| C. Workloads | Two deferrable jobs, both scripted and repeatable: (1) batch LLM inference — a fixed set of about 50–100 prompts through a local model (Ollama or MLX); (2) a clean build of a large open-source project, same commit every run. |
| D. Scheduler | Given a job and a deadline (e.g. "done by 9:00"), pick the lowest-carbon start time from the forecast and launch the job then. |
| E. Measurement | Phase 1 (baseline): jobs start immediately at realistic times. Phase 2: the same jobs and deadlines, started by the scheduler. CO2 per run = measured energy × carbon intensity in the hours the job actually ran. We also compute two bounds with hindsight: the best possible start time (oracle) and the worst. |
| F. Analysis (desk work) | Scheduler overhead (forecast runs, idle waiting), production- vs consumption-based intensity (Slovenia imports a lot of power), LLM vs build comparison, a scale-up estimate. |

We dropped laptop charging and short video exports as workloads: they draw only tens of watts for minutes, so the CO2 at stake is too small to measure against meter noise.

## 3 Milestones

Dates are placeholders until we confirm them on Učilnica.

- **TBD** — Submission of project proposal
- **26.10.2026** — Phase 1 baseline starts; carbon-data pipeline built
- **04.11.2026** — Update 1 (hourly carbon intensity for Slovenia; daily range plotted)
- **09.11.2026** — Phase 1 finished (10+ runs per workload); forecasters back-tested
- **16.11.2026** — Scheduler built and dry-run
- **25.11.2026** — Update 2 (Phase 2 running)
- **07.12.2026** — Phase 2 finished; CO2 comparison and analysis done
- **18.12.2026** — Update 3 (final figures, Methodology and Results drafted)
- **04.01.2027** — Final report, slides and poster ready
- **11.01.2027** — Final submission and presentation

## 4 Evaluation Plan

Success means a measured answer, even if the answer is "it saves almost nothing here."

- **CO2 per job:** baseline vs scheduler vs oracle vs worst case, for each workload, with mean and spread over 10+ runs.
- **Forecast:** error of each forecaster, and decision quality — how much more CO2 its chosen start time caused than the oracle's.
- **Overhead:** energy of the forecaster and of idle waiting, subtracted from the saving.
- **Deadlines:** number of jobs that missed their deadline (target: zero).
- **Negative results:** if Slovenia's daily carbon range is small, we report that shifting matters less in a low-variance grid, and we repeat the scheduling in simulation with a high-variance grid (e.g. Germany) to show the contrast.
- **Significance:** absolute savings on one laptop are small. We judge the project by whether the method works and by a scale-up estimate with clear assumptions, not by grams saved.
- **SDGs:** using clean hours better supports SDG 7 and SDG 13.
- **Rebound effect:** if people believe their jobs are "green now," they may run more of them, so total emissions rise. We discuss whether this could cancel the saving.

## 5 Resources

- **Equipment:** one Mac laptop (Apple Silicon), one wall power meter (to buy, low cost).
- **Software:** Python, macOS `powermetrics`, Ollama or MLX, a large open-source repository for the build job, Jupyter notebooks.
- **Data:** ENTSO-E Transparency Platform (free API token needed); published emission factors; optionally electricityMaps for a cross-check (check free access).
- **Roles (to confirm):** Person A — TBD. Person B — TBD. Both — proposal, analysis, report.
- **Optional extensions** (only if on schedule): cross-check our carbon intensity with electricityMaps; add a batch video transcode as a third workload; run on a second laptop; publish the scheduler as an open-source script.

## References

[1] Google (2020). *Our data centers now work harder when the sun shines and wind blows.* https://blog.google/inside-google/infrastructure/data-centers-work-harder-sun-shines-wind-blows/

[2] Green Software Foundation (2022). *That's a wrap — CarbonHack22, a big leap in carbon aware computing.* https://greensoftware.foundation/articles/carbonhack22-a-big-leap-in-carbon-aware-computing/

[3] J. You, J.-W. Chung, M. Chowdhury (2023). Zeus: Understanding and Optimizing GPU Energy Consumption of DNN Training. *USENIX NSDI 2023*. https://www.usenix.org/conference/nsdi23/presentation/you

[4] ENTSO-E. *Transparency Platform.* https://transparency.entsoe.eu/ (verify API access steps)
