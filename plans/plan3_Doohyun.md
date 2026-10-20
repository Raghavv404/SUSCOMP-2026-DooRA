# Project plan 3: P2 twisted, find the worst heat leaks with a thermal camera, fix them cheaply, and measure the saving

## Problem and goal

**Problem.** Buildings are one of the largest energy users in Europe (the European Commission puts it at about 40% of EU energy use; verify the exact figure and source). Heat loss through windows, frames and walls is a big part of this. Companies like MyHEAT (Canada) already map heat loss from the air for 47+ cities and give each home a heat-loss score. But the evidence that these scores lead to real savings comes from the company and its utility partners, not from independent tests. Nobody closes the loop: **diagnose with a thermal camera → apply a cheap fix → measure the real reduction in heat loss.**

**Goal.** Build a software pipeline that turns thermal images of rooms into a ranked list of the worst heat leaks, with an estimated heat loss in watts for each. Then apply cheap fixes to the top-ranked elements and measure the change against untreated elements.

**Research question.** Can a low-cost thermal-camera pipeline find which building elements leak the most heat, and do cheap fixes on those elements cause a measurable drop in heat loss?

## Topic mapping

- **Base topic: #2 Drone-Based Thermal Energy Auditor (SDG 11, 13).** The original idea: thermal-camera drones fly over buildings, software finds where heat escapes, results are shown on smart glasses.
- **What we keep:** the thermal camera, and the software that finds heat-loss points.
- **What we change:** no drone (EU rules need registration and a pilot licence) and no smart glasses. We scan rooms from the inside at ground level. We add the missing step: apply fixes and measure whether the diagnosis was right.
- **Course ties:** energy basics (week 2), embodied emissions (week 3 and lab), measuring software energy (week 7 lab, for the image pipeline), rebound effect (weeks 7–8), LCA and greenwashing (week 13: checking the "heat-loss score" claims).

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
| A | Capture protocol and baseline: pick about 10 elements (windows, window frames, door gaps, outer walls) in our own rooms and, with permission, at FRI. Capture thermal images in fixed conditions: indoor–outdoor difference of at least about 10 °C (EN 13187 guidance, verify), before sunrise, heating steady, same distance and angle. 3 baseline sessions. | Core |
| B | Pipeline: read per-pixel temperatures from the radiometric images, segment each element, and estimate its U-value and heat loss (W) from surface, indoor and outdoor temperatures with the standard infrared-thermography method (e.g. Fokaides & Kalogirou 2011, verify). Output: a ranked list of leaks. Check camera surface temperatures against a contact thermometer. | Core |
| C | Intervention: pair similar elements, then assign one of each pair to a cheap removable fix (draught strip, window insulation film, radiator reflector foil) and leave the other as a control. | Core |
| D | Measurement: 3 sessions after the fixes, same protocol. Compare the change in the treated elements against the change in the controls (difference-in-differences), with heat loss normalized by the indoor–outdoor temperature difference. | Core |
| E | Footprint and analysis: scale the measured reduction to a heating season with ARSO climate data, convert to CO2 with the emission factor of the room's heat source, and compare with the embodied carbon of the camera, fix materials and compute (payback time). Energy per image of the pipeline with `powermetrics`. Desk estimate of drone energy per building from public specs. Rebound effect, SDG discussion. | Core (desk work) |

Measurement note: the camera must export per-pixel temperatures (radiometric data), not only a coloured picture. Check this before borrowing. Raghav's P9 plan borrows a thermal camera too, so we can share it.

## Extensions (outside the core scope)

Ranked by value for effort. None of them may delay Parts A–E.

| # | Extension | What we do | When | Condition |
|---|---|---|---|---|
| 1 | Known-U check | Measure one element with a known U-value from the manufacturer's spec (e.g. labelled double glazing) to check the pipeline's estimate. | Week of 16 Nov | Such an element is available |
| 2 | Façade scan | Scan the outside of one FRI façade from the street at dawn; compare outside leak spots with the inside ranking. | Week of 30 Nov | Cold dawn with no rain |
| 3 | Cheap camera comparison | Repeat 3 elements with a phone-attached thermal camera; compare accuracy against the borrowed camera vs. price. | After Update 2 (25 Nov) | A phone camera can be borrowed |
| 4 | Housing feedback | Send the ranked leak list and measured results to student housing management; record their response. | Week of 7 Dec | Results are clear enough to share |
| 5 | Open dataset | Release the thermal images, temperatures and protocol under an open licence. | Week of 4 Jan | Professor agrees the data may be published |

## Week by week

| Week of | Work | Course tie-in | Output in repo |
|---|---|---|---|
| 19 Oct | Register topic. Confirm the camera model exports radiometric data, and agree on sharing it with Raghav. Pick the rooms and elements; ask FRI for permission. Buy a contact thermometer, two temperature loggers and fix materials. Write and submit the proposal. | Proposal template; embodied emissions lab: start a table for camera, loggers, fix materials | `proposal/proposal.pdf`, `docs/log.md` started |
| 26 Oct | Test shots: fix distance, angle, emissivity setting and capture time. Write the capture protocol. Script to read per-pixel temperatures. | — | `docs/protocol.md`, `src/read_thermal.py`, first images in `data/raw/thermal/` |
| 2 Nov | Segmentation and U-value/heat-loss estimation. Check camera vs contact thermometer. **Update 1.** | Update 1 slides: Overview & Early Activities; Progress & Next Steps | `src/heat_loss.py`, `notebooks/01_pipeline.ipynb`, `updates/update1.pdf` |
| 9 Nov | Baseline sessions 1–2 on all elements (cold mornings). | Measuring software lab: try `profiler.py` on the pipeline | `data/raw/thermal/baseline/` |
| 16 Nov | Baseline session 3. Build the ranked leak list. Pair elements, assign fixes, apply them at the end of the week. | Rebound archetypes lab: list our project's rebound risks | `results/leak_ranking.md`, `docs/fixes.md` |
| 23 Nov | After session 1. **Update 2.** | Update 2 slides: Progress Summary; Adjustments & Next Steps | `data/raw/thermal/after/`, `updates/update2.pdf` |
| 30 Nov | After sessions 2–3. Energy per image of the pipeline. | Networking/IoT lecture | `results/pipeline_energy.md` |
| 7 Dec | Part E: difference-in-differences, heating-season kWh, CO2, payback time, drone vs ground desk estimate. | Datacentre lecture; measuring cloud emissions lab | `notebooks/02_did.ipynb`, `results/savings.md`, `results/footprint.md` |
| 14 Dec | Final figures. Report sections Methodology and Results drafted. **Update 3.** | Update 3 slides: Outcomes; Summary & Final Work | `updates/update3.pdf`, `report/` draft |
| 21 Dec | Finish the full report draft. Review it as the professor would. | LCA and greenwashing lecture: check our claims | `report/report.pdf` draft |
| 28 Dec | Holiday buffer. Fix review comments only. | — | — |
| 4 Jan | Final report, slides in the FRI template, two timed rehearsals. Check every figure regenerates from the repo. | Project finalization | `report/report.pdf`, `slides/final.pptx` |
| 11 Jan | **Submit and present.** | — | Tagged final commit |

## Repo layout

```
proposal/   updates/   report/   slides/
data/raw/thermal/   data/raw/loggers/   data/processed/
src/        notebooks/        results/
docs/protocol.md   docs/fixes.md   docs/log.md   README.md
```

- `docs/log.md`: dated entries of what each person did; this feeds Author Contributions.
- `docs/protocol.md`: camera model and settings, emissivity, distance, angle, capture time, minimum temperature difference, element list.
- `docs/fixes.md`: which element got which fix, when, and its paired control.
- Commit after every capture session, with a message saying what changed.
- Large images: check repo size after week one; if it grows fast, keep full-resolution files elsewhere and commit reduced copies plus the temperature CSVs.

## Who does what (to decide; record in the log)

- **Person A:** TODO
- **Person B:** TODO
- **Both:** proposal, Part E, report writing.

## Risks and fallbacks

| Risk | Fallback |
|---|---|
| Camera gives only a coloured picture, no temperatures | Use the contact thermometer and reference points to calibrate relative brightness; state the limit |
| Weather too warm (indoor–outdoor difference under about 10 °C) | Capture before sunrise only; move sessions later into December |
| Sun, wind or heating changes confound readings | Capture before sunrise, log wind and outdoor temperature from ARSO, keep heating settings fixed during sessions |
| No permission for fixes at FRI or in student housing | Use only our own rooms and removable fixes |
| Too few elements for a fair comparison | Add elements from Raghav's room |
| Fixes show no measurable change | Report it as a negative result with the measurement uncertainty; Parts B and E still stand |
| Behind schedule at Update 2 | Drop the U-value estimate; keep the temperature-difference-normalized surface temperature as the metric |
| Extensions fall behind | Drop them in reverse order, 5 first |

## Checkpoints

- Each week's "Output in repo" item is in the GitHub repo by Sunday.
- By 16 Nov: protocol written, 3 baseline sessions for all elements, fixes applied.
- By 7 Dec: 3 after-fix sessions for all elements, and the difference-in-differences table is filled.
- By 4 Jan: running the notebooks top to bottom regenerates every figure in the report, and the report is at most 10 pages from Introduction to Conclusion.
