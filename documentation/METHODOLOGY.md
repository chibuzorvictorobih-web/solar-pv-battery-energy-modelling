# Technical Methodology

## 1. Model purpose
A 24-step (one-hour interval) energy-balance model examines a hypothetical building supplied by solar PV, a battery and the utility grid. No detailed AC power flow or electrical equipment design is performed.

## 2. Input data
The 24-element building average-power profile (kW) and solar irradiance profile (W/m²) are **synthetic** and documented within `notebooks/PV_Battery_Energy_Model.ipynb` and generated CSVs.

For each one-hour interval, `Demand_kWh = Demand_kW × 1 hour`.

## 3. Solar PV generation

\[P_{PV,t}=P_{rated}(G_t/1000)PR\]

`P_rated = 5 kWp`, `PR = 0.80` (assumed overall multiplier), with nonnegative mean irradiance `G_t` in W/m². For a one-hour interval, PV energy (kWh) is the average PV power (kW) × 1 hour. This is a simplified irradiance-linear approximation and is **not** a PVWatts simulation or site-specific performance prediction.

## 4. Dispatch and storage
At each hour:

1. `PV_to_Load = min(PV_energy, Demand_energy)`.
2. `Surplus_PV = PV_energy − PV_to_Load`; `Load_shortfall = Demand_energy − PV_to_Load`.
3. Charge battery from PV surplus up to available storage space, incorporating charging efficiency: `PV_to_Battery = min(Surplus_PV, (E_max−E_start)/η_charge)` and increase stored energy by `η_charge × PV_to_Battery`.
4. Discharge battery to meet shortfall, subject to minimum state of charge: `Battery_to_Load = min(Load_shortfall, (E_after_charge−E_min) × η_discharge)` and decrease stored energy by `Battery_to_Load / η_discharge`.
5. `Grid_Import = Load_shortfall − Battery_to_Load`; `PV_Curtailed = Surplus_PV − PV_to_Battery`.

The 10 kWh nominal battery operates from 20% to 90% SOC (2–9 kWh stored), with 95% charge and discharge efficiencies. The dispatch assumes no separate charging/discharging power limits, no export, no grid charging, and an always-available grid. Storage losses are explicitly tracked.

## 5. Repeating-day condition
The initial exploratory run uses 50% SOC and ends at 20%; that first-day grid-import result does **not** represent sustainable daily operation. Repeat the same 24-hour profiles, passing each day's ending battery energy to the next day, until the beginning and ending stored energy match within `1e-9 kWh` tolerance. Report the converged daily cycle as the representative one-day comparison. This is an artificial periodic boundary condition, not proof of stability under actual variable weather.

## 6. Energy conservation tests
For each hour:

- `Demand = PV_to_Load + Battery_to_Load + Grid_Import`.
- `PV_Generation = PV_to_Load + PV_to_Battery + PV_Curtailed`.

For the whole day:

`PV_Generation + Grid_Import + Battery_Energy_Start = Demand + PV_Curtailed + Charge_Loss + Discharge_Loss + Battery_Energy_End`.

Battery SOC must remain between its limits. Use numerical tolerance to account for floating-point rounding. These are internal accounting checks, not empirical model validation.

## 7. Economic evaluation
- `Annual_Grid_Cost = Daily_Grid_Import × 365 × 200 NGN/kWh`.
- `Annual_Operating_Cost = Annual_Grid_Cost + Annual_Maintenance`.
- `Annual_Net_Savings = Grid_Only_Annual_Operating_Cost − Scenario_Annual_Operating_Cost`.
- `Simple_Payback = Upfront_Investment / Annual_Net_Savings`, only when savings are positive.

All investment, maintenance and tariff figures are illustrative. Sensitivity analysis scales capital and maintenance costs linearly with PV and battery capacity, an intentionally simplistic pricing rule. Annual results extrapolate one repeatedly identical day; they are not year-round solar-resource forecasts.

## 8. Environmental evaluation
- `Daily_Grid_CO2 (kg) = Grid_Import (kWh) × 0.45 kg CO₂/kWh`.
- `Annual_Grid_CO2 (tonnes) = Daily_Grid_CO2 × 365 / 1000`.
- `Avoided_Grid_CO2 = Grid_Only_CO2 − Scenario_CO2`.

The emissions factor is an illustrative constant, not a verified Nigerian grid value. These estimates cover grid-related operational electricity emissions, **not lifecycle emissions or actual marginal emissions avoided**.

## 9. Sensitivity analysis
- **PV variation:** 0, 2.5, 5, 7.5, 10 kWp at fixed 10 kWh battery.
- **Battery variation:** 0, 5, 10, 15, 20 kWh at fixed 5 kWp solar PV.

Re-run dispatch and periodic balancing for each design and calculate grid import, curtailment, costs, emissions and simple payback. Comparisons do not constitute a global optimisation.

## 10. Reproducibility
Execute notebook top-to-bottom from a fresh kernel, save CSVs and figures, then run `python validate_project.py` from the project root. The reference results are linked to the documented exact profiles; changing inputs requires updating expected verification values and results tables.
