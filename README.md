# Techno-Economic and Environmental Performance Assessment of a Grid-Connected Solar PV–Battery System

**Independent Python engineering case study | 2026**  
**Author:** Chibuzor Victor Obih  
**Purpose:** Engineering portfolio project relevant to decentralised and sustainable energy systems.

> **Educational model.** All hourly demand and irradiance inputs, tariff and equipment costs, and emissions factors are illustrative. This is an independent portfolio exercise, **not** a validated electrical installation, an actual Nigerian market study, or a prediction of annual site performance.

## 1. Overview

This project models a hypothetical small building supplied by a grid-connected solar PV array and battery energy storage system. An hourly Python simulation allocates electricity from solar generation to building demand, sends PV surplus into the battery, discharges the battery during shortfalls, and imports remaining demand from the grid. Unusable PV generation is curtailed and grid export is disabled.

Three scenarios are compared:
1. Grid electricity only.
2. Grid electricity + 5 kWp solar PV.
3. Grid electricity + 5 kWp solar PV + 10 kWh battery.

The model additionally examines how PV and battery sizing affect electricity imports, solar curtailment, operating costs, and grid-related operational CO₂ emissions.

## 2. Core assumptions

| Input | Illustrative value |
|---|---:|
| Time step | 1 hour |
| Representative simulation day | 24 hours, repeated for annual estimates |
| Daily building demand | 36.70 kWh |
| PV nameplate capacity (reference) | 5 kWp |
| PV performance multiplier | 0.80 |
| Battery nameplate energy capacity | 10 kWh |
| Battery SOC range | 20–90% |
| Charge/discharge efficiencies | 95% / 95% |
| Battery charging | PV surplus only |
| Grid availability / export | Available / no export |
| Electricity tariff | ₦200/kWh (assumed) |
| Grid emissions intensity | 0.45 kg CO₂/kWh (assumed) |
| PV installed capital cost | ₦3,000,000 (assumed) |
| Battery additional capital cost | ₦2,500,000 (assumed) |
| PV annual maintenance | ₦30,000 (assumed) |
| Additional battery annual maintenance | ₦25,000 (assumed) |

See [`documentation/ASSUMPTIONS_AND_LIMITATIONS.md`](documentation/ASSUMPTIONS_AND_LIMITATIONS.md) for complete assumptions and limits.

## 3. Reproduce the calculations

**Prerequisites:** Python 3, VS Code with Python + Jupyter extensions, or another Jupyter environment.

1. Open this repository's root folder in VS Code.
2. Install Python dependencies in your selected environment:
   ```bash
   python -m pip install -r requirements.txt
   ```
3. Open `notebooks/PV_Battery_Energy_Model.ipynb`.
4. Select your Python kernel, choose **Restart Kernel and Run All**, and allow all cells to execute in order.
5. Confirm that CSV datasets are saved into `data/` and PNG charts into `figures/`.
6. Open a VS Code terminal at the repository root and run:
   ```bash
   python validate_project.py
   ```

This repository includes the executed Jupyter notebook, generated CSV datasets, figures and supporting documentation. Re-run the notebook from a fresh kernel and run `python validate_project.py` before publishing updated results.

## 4. Expected reference findings

The following are *independently recalculated expectations* for the explicitly assumed profiles and parameters—not evidence that the uploaded repository has been executed or validated against field data.

| Indicator | Grid only | PV only | PV + battery |
|---|---:|---:|---:|
| Grid import (kWh/day) | 36.70 | 15.82 | 9.5386 |
| Grid-import reduction | 0% | 56.89% | 74.01% |
| Annual operating cost (₦) | 2,679,100 | 1,184,860 | 751,317.80 |
| Annual net savings (₦) | 0 | 1,494,240 | 1,927,782.20 |
| Simple payback (years) | N/A | 2.01 | 2.85 |
| Annual grid-related CO₂ (tonnes) | 6.028 | 2.598 | 1.567 |

**Methodological correction:** A first-day simulation starting at 50% SOC ended at 20% SOC and imported about 6.69 kWh from the grid. That result consumes net previously stored electricity. The final comparison instead uses a repeating-day cycle starting and ending at the same SOC (20%), producing **9.5386 kWh/day** of grid imports for the PV–battery system.

### Sensitivity observations

- With a fixed 10 kWh battery, larger PV capacities reduce grid imports but eventually cause appreciable solar curtailment.
- With PV fixed at 5 kWp, increasing storage from 0 to 10 kWh reduces grid imports; further increases to 15 or 20 kWh produce no additional grid-import reduction for the assumed day.
- Under the assumed costs, PV-only has a shorter simple payback; PV + battery produces greater operating savings.
- These are comparisons among sampled configurations, **not** a formally solved system optimisation.

## 5. Representative figures

After running the notebook, relevant figures include:

- [PV generation versus building demand](figures/pv_vs_building_demand.png)
- [Battery state of charge, initial illustrative day](figures/battery_soc_profile.png)
- [Consistent daily scenario comparison](figures/steady_day_grid_comparison.png)
- [Annual operating costs](figures/annual_operating_cost_comparison.png)
- [Annual CO₂ emissions](figures/annual_co2_emissions_comparison.png)
- [PV capacity sensitivity](figures/sensitivity_pv_capacity_energy.png)
- [Battery capacity sensitivity](figures/sensitivity_battery_capacity_energy.png)

> The *initial-day* SOC figure is a transient case; the consistent repeating-day results, not the initial-day grid-import figure, should be used for recurring comparisons.

## 6. Project structure

```text
solar-pv-battery-energy-modelling/
├── README.md
├── requirements.txt
├── .gitignore
├── validate_project.py
├── notebooks/
│   └── PV_Battery_Energy_Model.ipynb
├── data/
│   └── [generated CSV files]
├── figures/
│   └── [generated PNG charts]
└── documentation/
    ├── METHODOLOGY.md
    ├── ASSUMPTIONS_AND_LIMITATIONS.md
    ├── SOURCE_LOG.md
    └── FINAL_REPORT.md
```

## 7. Interpretation and limitations

The simulated 74.01% reduction applies only to **grid electricity imports** under a representative hypothetical day, not total energy demand or lifetime emissions. Annual estimates multiply the repeated-day results by 365 and assume unchanging profiles, costs and emissions intensity. The emissions calculation covers grid-related operations only and excludes equipment embodied emissions. Simple payback ignores financing, degradation, discounting, and replacements. No irradiance, demand, financial, or emissions inputs are claimed to be observed or verified.

Further improvements could include actual location-specific hourly irradiance and load data, power-limited battery dispatch, inverter characteristics, degradation, lifecycle costs, and hourly grid emissions intensity.

## 8. Technical background and supporting sources

See [`documentation/SOURCE_LOG.md`](documentation/SOURCE_LOG.md). Key sources: NREL PVWatts (PV modelling concepts), System Advisor Model technical descriptions (PV and battery modelling), and GHG Protocol Scope 2 Guidance (purchased electricity emissions). **These sources provide background, not the invented numeric inputs.**

## 9. Provenance and public-sharing precautions

All project scenarios are hypothetical. The work does not use any employer measurements, proprietary drawings, equipment tags, operating data, or confidential site information. Before sharing, review notebook outputs, saved figures and Git commit author settings for identifying information. The `.git` directory is local Git bookkeeping and should not be included in a downloadable project ZIP.
