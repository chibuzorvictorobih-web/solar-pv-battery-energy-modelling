# Final Technical Report

## Techno-Economic and Environmental Performance Assessment of a Grid-Connected Solar PV–Battery System

**Independent computational engineering case study**  
**Author:** Chibuzor Victor Obih  
**Year:** 2026

## Abstract

A simplified Python-based hourly energy model was developed to investigate electricity supply to a hypothetical small building connected to solar photovoltaic (PV) generation, battery storage and the utility grid. Three configurations were compared: grid-only, grid with 5 kWp solar PV, and grid with 5 kWp solar PV and 10 kWh battery storage. The model applied solar-first dispatch, explicit battery state-of-charge constraints and assumed charging/discharging efficiencies. To avoid presenting initial stored energy as recurring savings, a repeating-day condition ensured equal starting and ending battery energy. Using synthetic electricity-demand and irradiance profiles, the PV-only and PV–battery systems reduced modelled grid imports by approximately 56.89% and 74.01%, respectively, relative to grid-only operation. Simple economic and grid-related operational CO₂ calculations were performed with hypothetical input values, and PV/battery sizing sensitivities were assessed. Results demonstrate modelling trade-offs rather than validated real-world system performance.

## 1. Problem and objectives

Solar PV generation does not necessarily align with building electricity demand. Solar electricity can be curtailed during surplus hours, while grid electricity may still be needed during demand shortfalls. The study models whether adding PV and battery storage can reduce grid imports and how equipment sizing influences energy, estimated cost and operational grid-related emissions.

## 2. System and methodology

The model uses 24 synthetic hourly building-average-power and irradiance values. At each time step, PV serves the building first; remaining PV charges the battery, while remaining building demand is served by battery discharge and then the grid. Unused PV electricity is curtailed and grid exports are disallowed. Solar generation is estimated with a simple capacity–irradiance equation using an assumed 0.80 performance multiplier. The nominal battery is 10 kWh, constrained to 20–90% SOC with assumed 95% charging and discharging efficiencies.

Initial simulations used a battery starting at 50% SOC, but these results were not used to estimate recurring savings because the battery finished at 20% SOC. Repeating identical 24-hour profiles until the start/end storage converged established a consistent comparison. Internal PV, load, storage and overall energy-balance equations were checked.

See [METHODOLOGY.md](METHODOLOGY.md) for equations and dispatch rules.

## 3. Results

| Measure | Grid only | Grid + PV | Grid + PV + battery |
|---|---:|---:|---:|
| Demand (kWh/day) | 36.70 | 36.70 | 36.70 |
| PV generation (kWh/day) | 0 | 27.84 | 27.84 |
| Grid import (kWh/day) | 36.70 | 15.82 | 9.5386 |
| Grid-import reduction (%) | 0 | 56.89 | 74.01 |
| Annual operating cost (₦) | 2,679,100 | 1,184,860 | 751,317.80 |
| Annual net operating savings (₦) | 0 | 1,494,240 | 1,927,782.20 |
| Simple payback (years) | N/A | 2.01 | 2.85 |
| Annual grid-related CO₂ (tonnes) | 6.028 | 2.598 | 1.567 |

These figures describe hypothetical performance only, extrapolated from 365 identical days with a flat tariff of ₦200/kWh and flat emissions factor of 0.45 kg CO₂/kWh. Capital costs and maintenance assumptions are declared separately. Grid-related emissions exclude PV/battery manufacturing and lifecycle impacts.

The PV-only system had the shorter simple payback under the chosen cost assumptions. The PV–battery system had the lowest operating expenditure and lowest grid-related operational emissions. The incremental battery investment yielded about ₦433,542 annual additional net operating savings and an estimated incremental simple payback of 5.77 years (subject to major limitations of simple payback).

## 4. Sensitivity observations

At fixed 10 kWh storage, increasing PV capacity initially reduced grid imports, but progressively more PV electricity was curtailed at higher capacities. At fixed 5 kWp PV, raising battery capacity from zero to 10 kWh reduced grid electricity imports, while increases to 15 and 20 kWh did not yield further reductions for the selected daily profiles. These examples demonstrate diminishing returns and the need to align sizing with temporal generation and consumption patterns. They **do not establish a globally optimal design**.

## 5. Verification and limitations

The Python calculations include hourly load and PV balances, battery SOC limits and overall daily energy conservation, together with reference-scenario checks. These tests establish internal numerical consistency, not agreement with measured real equipment.

All hourly profiles and financial/emissions inputs are illustrative; there is no detailed inverter/temperature/battery ageing treatment or measured location-specific data. The annual extrapolation ignores day-to-day and seasonal variability. The model excludes battery power limits, grid outages, demand charges, replacements, discounting and full lifecycle environmental impacts.

## 6. Conclusion

An hourly, reproducible modelling framework was developed for an illustrative grid-connected PV–battery system. Under selected assumptions, PV reduces grid electricity use, and storage allows surplus generation to serve later demand. Sensitivity analysis indicates that larger components may produce diminishing energy benefits and weaker investment payback. The study demonstrates transparent assumptions, programming, simulation, energy accounting and cautious technical interpretation relevant to decentralised energy-system analysis. Further work should validate inputs with real hourly resource/demand data and incorporate detailed battery and lifecycle economics.

## References

See [SOURCE_LOG.md](SOURCE_LOG.md) for external references and the distinction between published technical context and hypothetical inputs.
