# Assumptions, Input Provenance, and Limitations

## Input provenance

**Invented for this engineering exercise:** 24-hour load and irradiance profiles, building description, equipment operating schedule implied by demand, PV performance factor 0.80, battery size, SOC limits, efficiencies, charging policy, assumed grid reliability and export policy, assumed electricity tariff and all equipment prices, maintenance rates, and the 0.45 kg CO₂/kWh emissions factor.

**General technical ideas informed by external sources:** PV nameplate rating and modelling losses; battery dispatch and SOC constraints; purchased-electricity emissions accounting. External sources are listed in `SOURCE_LOG.md`. They do **not** supply, verify, or validate the invented numerical parameters.

## Reference assumptions

| Quantity | Reference assumption | Meaning / boundary |
|---|---|---|
| Daily building demand | 36.70 kWh | Sum of synthetic hourly demands |
| Solar irradiance | 24 synthetic hourly averages | Not weather station data |
| PV capacity | 5 kWp | DC nameplate reference capacity |
| PV performance multiplier | 0.80 | Simple overall derating approximation |
| Simulation resolution | 1 hour | No sub-hourly effects |
| Battery nominal capacity | 10 kWh | Stored energy capacity, not inverter power rating |
| State-of-charge limits | 20% to 90% | 2 kWh to 9 kWh reference storage range |
| Initial state | 50%, then periodic convergence | Initial transient not used for recurring savings |
| Charging efficiency | 95% | Assumed |
| Discharging efficiency | 95% | Assumed |
| Charging source | PV surplus | No charging from utility grid |
| Export policy | No export | Surplus beyond battery capacity is curtailed |
| Grid supply | Always available | No outage simulations |
| Electricity tariff | ₦200/kWh | Assumed flat volumetric rate |
| Grid emissions factor | 0.45 kg CO₂/kWh | Assumed flat operational factor |
| PV capital / annual O&M | ₦3m / ₦30k | Hypothetical cost |
| Additional battery capital / annual O&M | ₦2.5m / ₦25k | Hypothetical incremental cost |
| Cost scaling in sensitivity | Linear with installed capacities | No economies of scale |
| Annualisation | 365 identical days | Not seasonal annual simulation |

## Technical limitations

- No measured demand, measured irradiance, or site-specific PV calibration.
- No PV module temperature equation, separate inverter model, orientation, shading, or degradation.
- No battery charge/discharge power limit, inverter limit, ramping, ageing, cycle-life, thermal model, or replacement schedule.
- No grid outage, power quality, voltage regulation, frequency stability, load shedding, or protection design.
- No electricity export or feed-in tariff.
- No time-of-use or demand charges.
- No seasonality, weekday changes, stochastic weather, or hourly variability in grid emissions.
- PV capital and battery costs, tariffs and emissions factors are illustrative; the economic analysis is not a bankable feasibility study.
- No net present value, IRR, financing, discounting, tax, inflation, lifecycle cost, or embodied emissions assessment.
- The repeating-day assumption excludes transient energy depletion but does not guarantee a feasible year-round operating strategy.

## Appropriate project claims

**Appropriate:** "Developed a Python-based hourly energy balance and battery dispatch model for a hypothetical grid-connected solar PV–battery system; compared grid imports, illustrative operating costs, grid-related CO₂, and sizing sensitivity."

**Not appropriate:** "Designed and validated an actual microgrid"; "Measured 74% actual carbon reduction"; "Proved a commercially optimal Nigerian solar plant"; "Performed a full lifecycle assessment."

## Privacy
No employer data, equipment tags, private technical drawings, site measurements, photographs, proprietary documents, or numbers inferred from the workplace are used. Review content before publishing.
