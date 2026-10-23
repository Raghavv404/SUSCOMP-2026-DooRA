# Proposal outline for idea #9 (you write the text)

This is a skeleton, not a draft. Each section lists what the professor expects, the points worth making, and questions you must answer in your own words. Target length: 2 pages plus references, like the course template.

## Title

Make it say what you measure. Ingredients to combine: thermal camera, early detection, drought stress, smouldering, low-power. Avoid "drone" in the title, since you won't fly one.

## Opening paragraph (motivation)

Points to make, in this order:
1. The problem: drought and wildfire are growing, and both are detected late.
2. A local hook: the 2022 Kras (Karst) wildfire, the largest in Slovenia's history. Look up area burned and cost.
3. The gap: drones and thermal cameras exist, but a monitoring system has its own energy and hardware footprint. Nobody on a student budget knows how small and low-power it can be.
4. Your aim in one sentence.
5. SDG 15 (and 13).

Question to answer: why should a computing course care? (Because the monitoring system's footprint must be smaller than the damage it prevents.)

## 1 Related Work

Find and read 5–8 sources. Search terms and known starting points (verify every one yourself):
- Plant stress by temperature: "Crop Water Stress Index" (Idso 1981; Jackson 1981), "infrared thermography plant water stress" (H. G. Jones).
- Fire detection from thermal or aerial images: "FLAME dataset aerial fire", "early wildfire detection UAV thermal".
- Low-power ML on small devices: "TinyML energy per inference", "MobileNet edge energy".
- Scale of the problem: EFFIS (European Forest Fire Information System) annual reports; Slovenian forest service data.

For each source, write one sentence on what they found and one on what they did not do that you will.

## 2 Methodology

State the four research questions from PLAN.md, then describe each part. The professor will look for specifics, so include numbers.

| Task | What to specify |
|---|---|
| Plant experiment | Species, number of plants, watered vs dry groups, photo schedule, duration, reference object, how you record visible wilting |
| Hidden heat test | Heat sources, temperatures, distances, how many trials |
| Detection | The simple rule; the small model and what it is trained on; how you split training and test photos |
| Energy measurement | Device, tool (week 5 profiler), 5 runs plus warm-up, what is and is not measured |
| Energy budget | Solar and drone calculations, and which data sources feed them |

Also include a short "real / simulated / out of scope" list. Stating limits up front reads as strength.

## 3 Milestones

Five dated lines, matching the template: proposal, update 1, update 2, update 3, final report. Take the dates from Učilnica. Put the plant experiment first; it cannot be rushed later.

## 4 Evaluation Plan

Name the numbers you will report:
- Days of early warning before visible wilting.
- Temperature difference between dry and watered plants, with spread.
- Detection rate and false alarms for the heat test, by distance.
- Accuracy of rule vs model.
- Energy per image (J) and per day (Wh), and CO2e using 0.235 kg CO2e/kWh.
- Solar panel and battery size needed.

Say what counts as success, for example "detects stress at least N days before wilting". Pick N honestly after reading the literature. Also say what you will report if it fails; a negative result with good measurement is still a valid project.

## 5 Resources

Thermal camera (faculty, model name), plants and pots, laptop, optional Raspberry Pi or old phone, public datasets, software libraries, GitHub repo.

## References

Only sources you have opened and read. No invented or half-remembered citations.

## What makes it impactful

- One headline number: "X days earlier than the human eye".
- Your own dataset, published in the repo.
- An honest footprint: the system's energy cost set against its benefit.
- A clear handover: what the faculty would need to fly it for real.
