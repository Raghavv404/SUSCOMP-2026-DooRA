<!--
DRAFT proposal, built from plans/plan3_Doohyun.md. Not final.
Follows 01_SUSCOMP_Project_Proposal_Template.pdf: title, authors, date, opening paragraph, then
1 Related Work, 2 Methodology, 3 Milestones, 4 Evaluation Plan, 5 Resources, References. Limit: 2 pages.
Before submitting: verify every "(verify)" item, replace TBD values, convert to PDF.
-->

# Can We Trust Software Energy Numbers? Auditing and Calibrating Energy Estimators on a Laptop

Doohyun Kim, Raghav Verma — University of Ljubljana, Faculty of Computer and Information Science

October 2026

Developers and students often compare algorithms by their energy use and recommend the "greener" one. Usually they do not use a real power meter. They use a software estimator that reads hardware sensors or multiplies CPU time by a power value. If the estimator's error is bigger than the real difference between two algorithms, the ranking is wrong, and so is the advice. Green claims built on wrong numbers become greenwashing. This project asks: **are common software energy estimators accurate enough to tell which algorithm is greener, and if not, can a one-time calibration against a wall meter fix them?** We will measure several estimators against a wall power meter on the same laptop and the same workloads, check whether they rank algorithms correctly, and build and test a small calibration tool. The topic is based on S1 (SW code optimization for energy efficiency), with the checking method of #3 (examining tools for automated LCA). It supports SDG 12 (Responsible Consumption and Production) and SDG 13 (Climate Action).

## 1 Related Work

Software energy estimators are now widely used. CodeCarbon, released by Mila, BCG GAMMA and partners in 2020, estimates the CO2 of running code from energy use and the carbon intensity of the local grid [1]. Green Algorithms is an online calculator that estimates the footprint of a computation from its run time, hardware and location [2]. The course's own measuring-software lab uses a profiler based on Intel RAPL on Linux and `powermetrics` on macOS [3].

These tools are not always right. A 2026 study of Kepler, a CNCF energy tool for Kubernetes, reports that its container-level power estimates have an error (RMSE) of 11.9 W against RAPL, about a 15× overestimate [4]. An earlier study, accepted at ICT4S 2025, also concludes that Kepler's energy metrics are "not at a satisfactory level" and proposes an alternative tool [5]. Jay et al. compared software-based power meters for CPU and GPU [6].

**Gap:** we have found no study that checks these tools against a wall meter on an Apple Silicon laptop and asks the practical question: *does the tool still pick the right "greenest" algorithm?* (to be confirmed in our literature review; [6] must be read in full to see how close it comes).

## 2 Methodology

All work runs on one Mac (Apple Silicon). pyRAPL and Intel Power Gadget do not run on it, so we test the estimators that do.

| Part | What we do |
|---|---|
| A. Workload suite | The problems from S1: sorting, image processing, shortest path. 2–3 algorithm variants each (e.g. insertion sort vs quicksort vs built-in sort; naive Python blur vs NumPy; Dijkstra with and without a heap). |
| B. Ground truth | A wall power meter, minus the measured idle power. Fixed protocol: battery full, same screen brightness and Wi-Fi state, runs long enough for the meter, 5 runs plus a warm-up. |
| C. Estimators under test | (1) macOS `powermetrics` (on-chip sensors), (2) CodeCarbon, (3) a CPU-time × power-value model, similar in principle to calculators such as [2]. Metrics: error in %, and ranking agreement with the meter (Kendall's tau). |
| D. Calibration tool | `calibrate.py`: one calibration run against the wall meter fits an idle offset and a scale for each estimator. We test it on workloads not used for calibration: error and ranking before vs after. |
| E. Footprint (desk work) | Energy of the whole experiment vs the laptop's embodied carbon per hour of use (manufacturer environmental report). A measurement checklist for S1-type projects. |

## 3 Milestones

Dates are placeholders until we confirm them on Učilnica.

- **TBD** — Submission of project proposal
- **26.10.2026** — Measurement protocol written; idle power and meter resolution measured; workload suite ready
- **04.11.2026** — Update 1 (first full run with the meter only)
- **16.11.2026** — All estimators measured; error and ranking analysis done
- **25.11.2026** — Update 2 (`calibrate.py` built)
- **30.11.2026** — Before/after calibration test finished
- **07.12.2026** — Footprint analysis and checklist finished
- **18.12.2026** — Update 3 (final figures, Methodology and Results drafted)
- **04.01.2027** — Final report, slides and poster ready
- **11.01.2027** — Final submission and presentation

## 4 Evaluation Plan

Success means a measured answer for each tool, even if the answer is "this tool cannot be trusted."

- **Accuracy:** error of each estimator against the wall meter, in %, with mean and standard deviation over 5 runs.
- **Ranking:** Kendall's tau between each estimator's ranking and the meter's ranking, and a "same winner?" table per problem.
- **Calibration:** error and ranking agreement before vs after `calibrate.py`, on workloads not used to fit it. The tool succeeds if the error clearly drops on these unseen workloads.
- **Footprint:** the experiment's own energy compared with the laptop's embodied carbon per hour, to show which one matters more.
- **Negative results:** if two algorithms differ less than the measurement noise, we report that the tools cannot separate them, and we test bigger inputs.
- **SDGs:** honest energy numbers support SDG 12 (responsible production, no greenwashing) and SDG 13 (climate action through real, not imagined, savings).
- **Rebound effect:** easier measurement may lead developers to optimize code and then simply run it more, so total energy goes up. A "measured as green" label may also be used for greenwashing. We discuss both.

## 5 Resources

- **Equipment:** one Mac laptop (Apple Silicon), one wall power meter (to buy, low cost).
- **Software:** Python, macOS `powermetrics` (needs admin rights), CodeCarbon, the course profiler (week 5), Jupyter notebooks.
- **Data:** our own measurements; the laptop's manufacturer environmental report (verify availability).
- **Roles (to confirm):** Person A — TBD. Person B — TBD. Both — proposal, footprint section, report.
- **Optional extensions** (only if on schedule): repeat with pyRAPL on an Intel/AMD Linux lab machine; share `calibrate.py` with other teams; test a second laptop; compare with the same algorithm in C or Rust; open dataset.

## References

[1] Mila (2020). *Top AI Experts Create CodeCarbon, a Tool to Track and Reduce Computing's CO2 Emissions.* https://mila.quebec/en/news/top-ai-experts-create-codecarbon-a-tool-to-track-and-reduce-computings-co2-emissions

[2] L. Lannelongue, J. Grealey, M. Inouye (2021). Green Algorithms: Quantifying the Carbon Footprint of Computation. *Advanced Science*, 8(12), 2100707. https://doi.org/10.1002/advs.202100707

[3] SUSCOMP 2026 Labs, *Measuring Software Energy Consumption* (week 5). https://github.com/is-professor/SUSCOMP-2026-LABS/tree/main/week5-measuring-software

[4] Z. Bellal, L. Lahlou, N. Kara, T. Murphy, T. P. Nguyen, A. Ahmed, M. Perez-Jimenez (2026). Investigating the Potential of Kepler Toward Power Observability for Sustainable Cloud Computing. *IEEE Transactions on Green Communications and Networking*, 10, 2118–2135. https://doi.org/10.1109/TGCN.2026.3660816

[5] B. Pijnacker, B. Setz, V. Andrikopoulos (2025). *Container-level Energy Observability in Kubernetes Clusters.* arXiv:2504.10702 (accepted at ICT4S 2025). https://arxiv.org/abs/2504.10702

[6] A.-L. Jay, V. Ostapenco, L. Lefèvre, D. Trystram, A.-C. Orgerie (2023). An experimental comparison of software-based power meters: focus on CPU and GPU. *2023 IEEE/ACM 23rd International Symposium on Cluster, Cloud and Internet Computing (CCGrid)*, pp. 106–118. https://doi.org/10.1109/CCGrid57682.2023.00020
