# Thermal Early Warning for Drought Stress and Hidden Heat Sources

Semester project for the Sustainable Computing course (SUSCOMP), University of Ljubljana, Faculty of Computer and Information Science, 2026/27.

**Topic:** P9, Eco-Drone for Monitoring Forest Health and Fires (Environmental Monitoring and Biological Aspects, SDG 15)

## Team

| Name | GitHub |
|---|---|
| Raghav Verma | [@Raghavv404](https://github.com/Raghavv404) |
| Doohyun Kim | [@hyun7586](https://github.com/hyun7586) |

## About the project

We test whether a thermal camera can give early warning of two things: drought stress in plants, and hidden heat sources that could start a fire. For the first, we keep 6–10 potted plants indoors, stop watering half of them, and photograph all of them daily with a thermal and a normal camera for three weeks. For the second, we place safe warm objects out of sight and check from several distances whether the camera finds them. We then compare a simple temperature rule with a small ML model and measure the energy each one uses per image. Finally, we estimate the footprint of the whole system: the embodied emissions of the camera, drone and compute, and the solar and battery budget needed to run it.

## Plan in short

| When | What |
|---|---|
| Mid Oct | Register the topic, get the camera, buy plants, submit the proposal |
| Late Oct – mid Nov | Drought experiment with daily photos; hidden heat test |
| Late Nov | Simple rule vs small ML model; energy per image |
| Early Dec | Footprint: embodied emissions, solar and battery budget |
| Mid Dec – Jan | Report, slides, final presentation |

The full week-by-week plan is in [PLAN.md](PLAN.md).

## Repository structure

| Folder | Contents |
|---|---|
| `proposal/` | Project proposal |
| `updates/` | Slides for the three progress updates |
| `data/` | Raw and processed measurements |
| `src/` | Scripts |
| `notebooks/` | Analysis notebooks |
| `results/` | Tables and figures |
| `report/` | Final report |
| `slides/` | Final presentation |
| `docs/` | Capture protocol and work log |

TODO: remove folders you don't use, and add them as you create them.

## Status

This plan is a first version and not final. Still open:

- The deliverable dates are placeholders until we confirm them on Učilnica.
- We do not yet know the camera model or the loan terms.
- A real drone flight is a stretch goal. We decide by 30 Oct, depending on whether the drone can carry the camera, who is allowed to fly it, and where flying is permitted. Otherwise the flight is simulated with the camera on a pole.
- The split of work between us is a suggestion and may change.
