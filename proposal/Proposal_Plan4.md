# Find It, Fix It, Measure It: A Thermal-Camera Heat-Loss Audit with a Real Before/After Test

Doohyun Kim, Raghav Verma — University of Ljubljana, Faculty of Computer and Information Science

October 2026

Buildings are one of the largest energy users in Europe (about 40% of total energy consumption in the EU, according to the recitals of the EU Energy Performance of Buildings Directive [1]). A lot of this energy leaves through windows, frames and walls. Thermal cameras make these leaks visible, and some companies now map heat loss for whole cities. But a heat-loss map only helps if fixing the leaks really saves energy, and this step is rarely measured. This project asks: **can a low-cost thermal-camera pipeline find which building elements leak the most heat, and do cheap fixes on those elements cause a measurable drop in heat loss?** We will build software that turns thermal images of rooms into a ranked list of leaks, apply cheap removable fixes to half of the top elements, and measure the change against untreated elements. The topic is based on #2 (Drone-Based Thermal Energy Auditor). It supports SDG 7 (Affordable and Clean Energy), SDG 11 (Sustainable Cities and Communities) and SDG 13 (Climate Action).

## 1 Related Work

MyHEAT is a company built on research from the University of Calgary. It collects aerial thermal images, and by the end of 2019 it had processed images for 47+ cities in two countries [2]. In Saint John, New Brunswick, the utility Saint John Energy uses MyHEAT maps in its Home Heat Saver programme: each home gets a heat-loss score from 1 to 10, and homeowners with high scores are encouraged to upgrade and are connected with rebate programmes [3].

On the measurement side, infrared thermography can estimate the U-value (heat transfer coefficient) of a building wall or window on site, from surface, indoor and outdoor temperatures [4], and a review covers how thermography is used for building diagnostics [5]. The standard method for qualitative thermography of building envelopes, EN 13187, was replaced in 2023 by EN ISO 6781-1 [6]; we will check it for the conditions we must meet (for example the indoor–outdoor temperature difference).

**Gap:** the outcome figure we found for such programmes, that homeowners are "5x more likely to make improved changes", is quoted by the utility from "similar projects" and is not a result of this programme [3]. We found no independent evaluation. We also could not find a small, controlled study that closes the loop: diagnose with a camera, apply a fix, and measure the real change against an untreated control (to be confirmed in our literature review).

## 2 Methodology

We do not use a drone: EU rules require registration and a pilot licence for most drones (verify). We scan rooms from the inside, at ground level, with a borrowed thermal camera.

| Part | What we do |
|---|---|
| A. Protocol and baseline | About 10 elements (windows, frames, door gaps, outer walls) in our own rooms and, with permission, at FRI. Fixed conditions: indoor–outdoor difference of about 10 °C or more (a working assumption, to be checked against EN ISO 6781-1 [6]), before sunrise, heating steady, same distance and angle. 3 baseline sessions. |
| B. Pipeline | Read per-pixel temperatures, mark each element, estimate its U-value and heat loss in watts with the infrared-thermography method [4], and output a ranked list of leaks. Check camera temperatures against a contact thermometer. |
| C. Intervention | Pair similar elements. One of each pair gets a cheap removable fix (draught strip, window insulation film, radiator reflector foil); the other is the control. |
| D. Measurement | 3 sessions after the fixes, same protocol. We compare the change in fixed elements with the change in controls (difference-in-differences), with heat loss divided by the indoor–outdoor temperature difference. |
| E. Footprint (desk work) | Scale the saving to a heating season with ARSO climate data; convert to CO2 with the emission factor of the room's heating; compare with the embodied carbon of the camera, materials and compute (payback time). Energy per image of the pipeline with `powermetrics`. A desk estimate of drone energy per building. |

## 3 Milestones

Dates are placeholders until we confirm them on Učilnica.

- **TBD** — Submission of project proposal
- **26.10.2026** — Capture protocol written; script reads per-pixel temperatures
- **04.11.2026** — Update 1 (heat-loss estimation working; camera checked against the contact thermometer)
- **16.11.2026** — 3 baseline sessions done; ranked leak list; fixes applied
- **25.11.2026** — Update 2 (first session after the fixes)
- **30.11.2026** — 3 sessions after the fixes done; pipeline energy measured
- **07.12.2026** — Difference-in-differences, heating-season saving, CO2 and payback analysis finished
- **18.12.2026** — Update 3 (final figures, Methodology and Results drafted)
- **04.01.2027** — Final report, slides and poster ready
- **11.01.2027** — Final submission and presentation

## 4 Evaluation Plan

Success means a measured answer, even if the fixes turn out to make no clear difference.

- **Diagnosis quality:** camera temperatures against the contact thermometer (mean error); does the ranked list put the worst elements first?
- **Fix effect:** the difference-in-differences of normalized heat loss between fixed elements and controls, with its uncertainty.
- **Energy and CO2:** the measured saving scaled to one heating season, in kWh and kg CO2, and the payback time against the embodied carbon of the camera, materials and compute.
- **Negative results:** if the fixes show no measurable change, we report this with the measurement uncertainty. The pipeline and footprint parts still stand.
- **SDGs:** lower heating demand supports SDG 7 and SDG 13; cheap, repeatable audits for student housing support SDG 11.
- **Rebound effect:** a warmer room may lead people to raise the thermostat or heat more rooms ("comfort taking"), so the saving shrinks. Cheap audits may also mean more cameras bought and later thrown away. We discuss both.

## 5 Resources

- **Equipment:** a borrowed thermal camera that exports per-pixel temperatures (model and loan terms TBD; can be shared with the team's other thermal work), a contact thermometer, two temperature loggers, removable fix materials (low cost), a Mac laptop.
- **Software:** Python, Jupyter notebooks, macOS `powermetrics`, the course profiler (week 5).
- **Data:** our own thermal images and logs; ARSO weather and climate data; manufacturer data for embodied emissions.
- **Permissions:** rooms we live in; FRI building only with permission; fixes must be removable.
- **Roles (to confirm):** Person A — TBD. Person B — TBD. Both — proposal, footprint section, report.
- **Optional extensions** (only if on schedule): check one element with a known U-value; scan one FRI façade from outside; compare with a cheap phone thermal camera; share results with student housing; open dataset.

## References

[1] European Parliament and Council (2010). *Directive 2010/31/EU of 19 May 2010 on the energy performance of buildings (recast).* https://eur-lex.europa.eu/eli/dir/2010/31/oj (the 40% figure is in the recitals; check the recital number)

[2] The Geovation Group, University of Calgary. *MyHEAT.* https://wpsites.ucalgary.ca/geovation/research/myheat

[3] Electricity Canada. *Home Heat Saver Program (Saint John Energy and MyHEAT).* https://electricity.ca/lead/centre-of-excellence/home-heat-saver-program/

[4] P. A. Fokaides, S. A. Kalogirou (2011). Application of infrared thermography for the determination of the overall heat transfer coefficient (U-value) in building envelopes. *Applied Energy*, 88(12), 4358–4365. https://doi.org/10.1016/j.apenergy.2011.05.014

[5] M. Kylili, P. A. Fokaides, P. Christou, S. A. Kalogirou (2014). Infrared thermography (IRT) applications for building diagnostics: A review. *Applied Energy*, 134, 531–549. https://doi.org/10.1016/j.apenergy.2014.08.005

[6] EN ISO 6781-1:2023. *Thermal performance of buildings — Qualitative detection of thermal irregularities in building envelopes — Infrared method* (replaces EN 13187:1999). Check the full text for the temperature-difference conditions.
