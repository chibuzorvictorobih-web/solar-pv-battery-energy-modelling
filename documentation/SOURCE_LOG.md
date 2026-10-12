# Technical Reference and Source Log

**Compiled:** 2026. Sources provide **technical context only**. The project's numerical inputs are **not drawn from these sources** unless explicitly stated otherwise.

| No. | Source | URL | Technical relevance | How used | Data borrowed? |
|---|---|---|---|---|---|
| 1 | National Renewable Energy Laboratory (NREL), *PVWatts Version 5 Manual*, A. Dobos (2014) | https://docs.nrel.gov/docs/fy14osti/62641.pdf | Overview of PV performance modelling and system assumptions | Context for nameplate rating, irradiance, PV modelling limitations | No |
| 2 | National Laboratory of the Rockies / System Advisor Model, *Photovoltaic Models* | https://sam.nlr.gov/photovoltaic | Comparison of simplified and more detailed PV models | Describing what this project does **not** model (temperature, inverters etc.) | No |
| 3 | NREL / System Advisor Model, *Battery Storage* | https://sam.nlr.gov/index.php?Itemid=360&id=94&option=com_content&view=article | Battery dispatch, storage performance, degradation and more advanced modelling | Context and limitations for our deliberately simplified storage dispatch | No |
| 4 | GHG Protocol, *Scope 2 Guidance* | https://ghgprotocol.org/scope-2-guidance | Purchased electricity emissions accounting | Background for estimating operational grid-related emissions | No |

## Important provenance statement

- The 24 hourly demand values and 24 irradiance values were created solely for this hypothetical study. They are **not** from PVWatts, SAM or an actual installation.
- `PR = 0.80`, `η_charge = η_discharge = 0.95`, `grid factor = 0.45 kg CO₂/kWh`, prices, maintenance costs, tariff and all SOC limits are **assumptions**, not values taken from any listed reference.
- The PV model is the explicit linear equation documented in the notebook; it does **not** reproduce the full PVWatts algorithm.
- Our emissions estimates are proportional to electricity imported from the grid and are **not** certified emissions inventories or full lifecycle calculations.
- For an improved study, use verified weather datasets, real tariff documentation and actual grid emissions information, recording their specific sources and dates.

When reproducing or revising this project, update links, access dates and provenance as necessary. Do not imply external sources endorse this case study.
