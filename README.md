# Data for "WAFT: A Fourier-Analytic Framework for Steady-State Performance of Walker Constellations"

Numerical data behind the **non-RGT (Starlink-like) example** of the paper: the steady-state distribution of the aggregate downlink interference at a terrestrial receiver, and the expectation / variance of that interference over a latitude sweep.

The **RGT (repeating ground-track) example** is validated graphically rather than numerically —see [RGT example](#rgt-example-periodicity-verified-in-figures) below.

## Files

| File | Rows | Content |
|---|---|---|
| `steady-state-distribution.csv` | 15 | 3 latitudes × 5 cases (`Sim`, `WAFT`, `CPP`, `BPP`, `NHPP`): 0.1 % / 99.9 % percentile limits, their relative error vs. simulation, and the spurious probability mass |
| `moments-vs-latitude.csv` | 15 | Latitudes 0°–70° in 5° steps: expectation `E` and variance `Var` for the same five cases |

### `steady-state-distribution.csv`

```
Latitude_deg, Method, Q0.1, Q0.1_error_dB, Q99.9, Q99.9_error_dB, Spurious_mass_pct
```

- `Q0.1`, `Q99.9` — 0.1 % and 99.9 % percentiles of the steady-state complementary CDF, in
  **linear** power (see [Conventions](#conventions)).
- `*_error_dB` — relative error of the model with respect to the simulation, in dB, defined as
  `10*log10(X_model / X_sim)`. Negative means the model underestimates the limit.
- `Spurious_mass_pct` — percentage of the probability mass that the model assigns **outside** the
  support `[I_min^sim, I_max^sim]` actually observed in the time-stepped simulation. For `WAFT`
  this residue is a truncation artifact (Gibbs oscillation of the truncated spectrum) and shrinks
  with a higher cutoff.

### `moments-vs-latitude.csv`

```
Latitude_deg, E_WAFT, E_Sim, E_CPP, E_BPP, E_NHPP, Var_WAFT, Var_Sim, Var_CPP, Var_BPP, Var_NHPP
```

One row per latitude, one column pair per case, so the table can be plotted directly against
latitude.

## Conventions

- **All interference values are linear.** They are normalized so that the received power of a satellite at zenith inside the high-gain cap equals 1 (`w = 1` in the paper). Percentile values of order 1 (e.g. `1.1907`) are linear powers, not decibels.
- `Var_*` columns are variances of a linear power, hence linear power squared.
- Only the `*_error_dB` columns and the dB discrepancy table in the paper are logarithmic.
- **Empty cells are not applicable.** The simulation is the reference: its relative error is 0 dB by definition, and the mass it places outside its own support is 0 by construction. `Sim` rows therefore carry only the two percentile values.

## Setup used to generate the data

| Parameter | Non-RGT example |
|---|---|
| Orbital altitude `h` | 550 km |
| Planes / satellites per plane | 72 / 22 (1584 satellites) |
| Phasing factor `F` | 17 |
| Walker-δ type | non-repeating ground track |
| Inclination `i` | 53° |
| Transmit cap half-angle `ψ_t` | 45° |
| Cap gains `G_h` / `G_l` | 24 dBi / 0 dBi |
| Receiver gain `G_r` | 0 dBi |
| Minimum elevation `θ_min` | 10° |
| Power normalization | zenith high-gain received power = 1 |
| Propagation | free space, no fading |
| CCDF latitudes | 0°, 35°, 60° |
| Moment latitudes | 0°–70°, step 5° |
| Simulation duration / step | 30 days / 30 s |
| Spectral cutoff | `sqrt(a^2 + b^2) <= 4800` (dual lattice) |
| Earth radius / `μ` / `ω_E` | 6378.137 km / 398600.4418 km³/s² / 7.2921159e-5 rad/s |

The five cases are: `Sim` (finite-duration time-stepped simulation), `WAFT` (this paper),
`CPP` (Cox point process baseline), `BPP` (binomial point process baseline), and `NHPP`
(nonhomogeneous Poisson point process baseline).

## RGT example: periodicity verified in figures

The repeating ground-track example (Walker-δ, 15:4 resonance, `h = 11090.38` km, 6 planes × 12 satellites, `F = 3`, `ψ_t = 11°`, ground-track repeat period 4 sidereal days, simulated for 4 sidereal days at 1 s) is **not tabulated here**. Its correctness is established
visually, because the quantities being verified are periods — a property read off the alignment of curves, not off a table of numbers:

- **Fig. 6** — instantaneous interference over one profile period: (a) the space–time map over one sidereal day, (b) the longitude profile at `t0 = 0.6 T_per`, (c) the time profile at `λ0 = 61°`. The repeated structure along the time and longitude axes demonstrates `T_per = 3.989 h` and `Δλ_inst = 120°`.
- **Fig. 7** — steady-state expectation and variance versus receiver longitude, with signed discrepancies, and the CCDF at `λ_rx = 60°`. Both moments repeat every `Δλ_steady = 8°`, matching the value predicted analytically, and the WAFT curves coincide with the simulation.

The three periods above are predicted in closed form by the paper (temporal period, instantaneous longitude period, steady-state longitude period); the figures show that the simulated interference reproduces them. Hence no raw CSV is provided for the RGT case.

## Relation to the manuscript

- `steady-state-distribution.csv` backs the spurious-mass table, the percentile-limit discrepancy
  table (dB), and the CCDF figure (Fig. 4).
- `moments-vs-latitude.csv` backs the moment figure (Fig. 5).
- Figure and table numbers refer to the submitted version of the paper.
