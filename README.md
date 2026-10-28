# Solar Tower CSP Plant – Design, Yearly Simulation & LCOE Optimisation

End-to-end model of a **molten-salt solar tower power plant** with two-tank thermal energy storage, from sun position to annual electricity yield and levelised cost of electricity, written in Python.

*Academic group project (4 students) – Politecnico di Milano, M.Sc. Energy Engineering, A.Y. 2025–2026.*

---

## Plant

| | |
|---|---|
| Site | Jean, Nevada (35.76° N, 115.36° W) |
| Solar field | 3,615 heliostats, 521,915 m² reflective area (layout from SolarPilot) |
| Tower / receiver | 195 m tower; external cylindrical receiver 15 m × 17.65 m, 1,056 tubes |
| Heat transfer fluid | Solar salts, 255 → 565 °C |
| Storage | Two-tank direct molten-salt TES |
| Power block | Superheated Rankine cycle, 8 feedwater heaters, air-cooled condenser |

## Model chain

1. **Solar geometry** – hourly sun altitude and azimuth for a full year (equation of time, declination, hour angle), aligned with the SolarPilot reference frame.
2. **Optical efficiency** – polynomial fits of field efficiency vs elevation and azimuth from SolarPilot runs.
3. **Receiver thermal model** – tube-by-tube, discretised along the height: Gnielinski internal convection, wall conduction, external natural convection and radiation. Nested loops on salt mass flow and wall temperature until the outlet reaches 565 °C. Three flow-path configurations compared.
4. **Piping losses** – 100-segment radial resistance network along the riser and horizontal piping, at design and part load.
5. **Power block** – Rankine cycle with regenerative feedwater line, evaporation pressure optimised (CoolProp); efficiency as a function of ambient temperature.
6. **Thermal energy storage** – two-tank lumped model with mass/energy balances, wall/roof/floor heat losses and four dispatch modes (excess generation, storage-assisted, shutdown and charging, defocusing).
7. **Auxiliaries** – salt pump power as a function of flow rate, including the geodetic head of the tower.
8. **Techno-economic optimisation** – 8,760-hour simulation swept over **solar multiple** and **hours of storage** to minimise LCOE.

---

## Results

| | |
|---|---|
| Optimal solar multiple | **2.6** |
| Optimal storage | **13 h** |
| **LCOE** | **122.3 $/MWh** |
| Net power at design | 38.8 MW |
| Annual net electricity | 241.7 GWh |
| Equivalent full-load hours | 5,682 h |
| Power block efficiency (design) | 36.7 % |
| Solar-to-electric efficiency (design) | 20.3 % |

![LCOE optimisation](figures/lcoe_optimization.png)
*LCOE as a function of solar multiple and hours of storage.*

![Annual energy flows](figures/annual_sankey.png)
*Annual energy balance from solar resource to net electricity.*

![Receiver](figures/receiver_efficiency.png)
*Receiver thermal efficiency and salt mass flow vs incident power for three flow-path configurations; version 3 was selected.*

![Spring operation](figures/spring_operation.png)
*Three spring days: storage state of charge, solar and thermal powers, net electric output. The plant runs around the clock, with defocusing once the storage is full.*

---

## Repository structure

```
├── csp_model.ipynb     # full model, one section per subsystem
├── report.pdf          # project report with methodology and discussion
└── figures/
```

Requires Python 3 with `numpy`, `scipy`, `matplotlib`, `CoolProp`. Some cells read input files exported from SolarPilot (`dati_matrice.txt`, receiver flux map) and power-block results (`risultatiPB_tamb.txt`).

## Notes

Academic project code, kept as delivered; comments are partly in Italian. The full methodology is in `report.pdf`.
