# Project plan: P9, thermal early warning for thirsty plants and hidden heat

## Dates (placeholders: replace with the real ones from Učilnica)

Last year's template dates shifted to this year's calendar:

| Deliverable | Assumed date | 
|---|---|
| Proposal | Fri 16 Oct 2026 |
| Update 1 | Wed 4 Nov 2026 |
| Update 2 | Wed 25 Nov 2026 |
| Update 3 | Fri 18 Dec 2026 |
| Final report and presentation | Mon 11 Jan 2027 |

## Scope

| Part | What | Status |
|---|---|---|
| A | Drought experiment: 6–10 potted plants, half unwatered, daily thermal and normal photos for 3 weeks | Core |
| B | Hidden heat test indoors with safe warm objects, several distances | Core |
| C | Simple temperature rule vs small ML model; energy per image with the week 5 profiler | Core |
| D | Footprint: embodied emissions of camera, drone and compute; solar and battery budget; SDID rubric | Core (desk work) |
| E | Drone flight: thermal hotspot search outdoors over a cold field | Optional, if the gate below passes |

A drone is not necessary for this project, but we can use one. **Gate for Part E (decide by 30 Oct):** the drone carries a thermal camera or the lent camera can be mounted; a qualified person (faculty staff or a certified student) flies it; the faculty confirms where flying is allowed. EU drone rules require registration and a pilot certificate for most drones, so ask the professor who handles that. If any item fails, the flight stays simulated (camera on a pole) and energy per hectare is calculated from the drone's specs.

Winter note: outdoor plants are dormant, so drought stress stays indoors. Cold ground makes warm objects easy to see, so the outdoor hotspot flight suits November and December.

## Extensions (outside the core scope)

Ranked by value for effort. None of them may delay Parts A–D.

| # | Extension | What we do | When | Condition |
|---|---|---|---|---|
| 1 | User conversation | One 20-minute call or email exchange with the Slovenia Forest Service or a local fire brigade. Gives a requirement to measure our results against. | Email sent week of 19 Oct | None |
| 2 | Young trees | Add 4 potted spruce saplings to Part A, 2 dry and 2 watered, same photo schedule. Trees dry slowly, so little change in 3 weeks is itself a result. | Bought week of 12 Oct | Saplings available at a nursery |
| 3 | Satellite comparison | Look up free satellite fire and vegetation data for the 2022 Kras fire. Table of satellite vs drone vs fixed camera: warning time, detail, cost, footprint. | Week of 7 Dec, with Part D | None |
| 4 | Open dataset | Release the photos, labels and capture protocol under an open licence. | Week of 4 Jan | Professor agrees the data may be published |
| 5 | Field unit | Run detection on a Raspberry Pi with the thermal camera, from a battery and small panel; log how long it lasts. | After Update 2 (25 Nov) | On schedule at Update 2, and the camera can connect to a Pi |

## Week by week

| Week of | Work | Course tie-in | Output in repo |
|---|---|---|---|
| 14-15 Oct | Register P9. Get camera model and loan terms. Buy plants (and 4 spruce saplings if a nursery has them) and set up the photo spot. Write and submit the proposal. Ask about the drone and who may fly it. | Proposal template | `proposal/proposal.pdf`, `docs/log.md` started |
| 19 Oct | Test shots: fix distance, angle, time of day, reference card. Write the capture protocol. Let plants settle with normal watering. Email the Slovenia Forest Service or a local fire brigade (Extension 1). | Embodied emissions lab: start a table for camera, drone, laptop | `docs/protocol.md`, first photos in `data/raw/` |
| 26 Oct | **Start Part A**: stop watering the dry group, daily photos. Script to read temperatures from the images. Decide the Part E gate by 30 Oct. | — | `src/extract.py`, `data/raw/dayNN/` |
| 2 Nov | Part A day 7–13. First plot of dry vs watered temperature. **Update 1.** | Update 1 slides: Overview & Early Activities; Progress & Next Steps | `updates/update1.pdf` |
| 9 Nov | Part A day 14–20. Record the day each dry plant visibly wilts. Run Part B (hidden heat, 3 distances, 5 trials each). | Measuring software lab: try `profiler.py` on the extract script | `data/raw/heat/`, wilt dates in `data/labels.csv` |
| 16 Nov | End Part A. Clean dataset. Build the simple rule and compute days of early warning. | — | `data/processed/`, `notebooks/01_rule.ipynb` |
| 23 Nov | Train the small ML model on own photos; compare with the rule. **Update 2.** | Update 2 slides: Progress Summary; Adjustments & Next Steps | `notebooks/02_model.ipynb`, `updates/update2.pdf` |
| 30 Nov | Energy per image for rule and model: 5 runs plus warm-up, mean and standard deviation. Repeat on a Raspberry Pi or old phone if available. Part E flight if the gate passed. | Week 5 method; cloud emissions lab if training ran on Colab | `results/energy.md`, flight data in `data/raw/flight/` |
| 7 Dec | Part D: embodied emissions, solar and battery budget, drone energy per hectare. Apply the SDID rubric to the project. Satellite comparison table (Extension 3). Buffer for a missed flight day. | Week 3 and week 10 labs | `results/footprint.md`, `results/sdid.md`, `results/satellite.md` |
| 14 Dec | Final figures. Report sections Methodology and Results drafted. **Update 3.** | Update 3 slides: Outcomes; Summary & Final Work | `updates/update3.pdf`, `report/` draft |
| 21 Dec | Finish the full report draft. Review it as the professor would. | Report template | `report/report.pdf` draft |
| 28 Dec | Holiday buffer. Fix review comments only. | — | — |
| 4 Jan | Final report, slides in the FRI template, two timed rehearsals. Check every figure regenerates from the repo. | Project finalization | `report/report.pdf`, `slides/final.pptx` |
| 11 Jan | **Submit and present.** | — | Tagged final commit |

## Repo layout

```
proposal/   updates/   report/   slides/
data/raw/   data/processed/   data/labels.csv
src/        notebooks/        results/
docs/protocol.md   docs/log.md   README.md
```

- `docs/log.md`: dated entries of what each person did; this feeds Author Contributions.
- Commit after every photo day and every work session, with a message saying what changed.
- Large images: check repo size after week one; if it grows fast, keep full-resolution files elsewhere and commit reduced copies plus the temperature CSVs.

## Who does what (suggested; adjust and record in the log)

- **Raghav:** plant care and daily capture, hidden heat test, presentations.
- **Teammate:** extraction script, analysis notebooks, energy measurements.
- **Both:** proposal, footprint section, report writing, the drone day.

## Risks and fallbacks

| Risk | Fallback |
|---|---|
| Camera not available every day | Photograph every second day; extend Part A by a week |
| Camera gives only a coloured picture, no temperatures | Use the reference card and compare relative brightness; state the limit |
| Plants show no clear difference | Report it as a negative result with the measurements; Parts B–D still stand |
| No drone or pilot available, or weather blocks the flight | Pole-mounted simulated flight plus calculation |
| Behind schedule at Update 2 | Drop the ML model; keep the rule and the energy measurement |
| Extensions fall behind | Drop them in reverse order, 5 first |

## Checkpoints

- Each week's "Output in repo" item is in the GitHub repo by Sunday.
- By 16 Nov: at least 15 photo days in `data/raw/` and wilt dates recorded.
- By 4 Jan: running the notebooks top to bottom regenerates every figure in the report, and the report is at most 10 pages from Introduction to Conclusion.
