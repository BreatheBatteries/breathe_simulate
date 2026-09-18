# Changelog

## [2.7.0] - 15/09/2026

### Added

- Pulse heating for cold cells: `generate_pulse_heating_lut` gives the largest zero-mean current pulse a cell can take at each state of charge and temperature without breaching its voltage cutoffs, its anode plating threshold or its cathode ceiling. Choose the pulse frequency and the limits, and get back a table you can inspect with `to_dataframe()` and `plot()`.
- `Cycler.pulse_heating` flies that table in a simulation. The amplitude follows the cell's own state of charge and temperature as it warms, so the applied current rises as the cell's resistance falls. The run stops at a target temperature or after a set duration.
- `generate_resistance_vs_frequency` shows how total cell resistance falls as the pulse frequency rises, from the DC value down towards the high-frequency floor. This is the reason pulse heating runs fast: the double layer shunts the charge-transfer path, leaving more headroom for a larger amplitude within the same voltage limits. Requires a cell with a fitted double layer.


## [2.5.4] - 24/08/2026

### Fixed

- Design parameters are checked before a simulation is sent. An unsupported key in a `run_sim` design used to fail part-way through with an unhelpful error; it now raises straight away, listing the supported design parameters and suggesting the intended one when it looks like a typo.
- The ageing state can be written the way `get_aged_ocv` spells it — `LAMPE`, `LAMNE`, `LLI`, in any capitalisation — inside a design dict. It is read as `lampe`/`lamne`/`lli` with a warning, rather than failing the simulation.
- `aluminumThickness_um` is accepted and read as `aluminiumThickness_um`, the name the simulation uses. The cell parameter list now shows the correct spelling.
- `separatorPorosity` removed from the cell parameter list. It was listed as a design parameter, but separator porosity comes from the cell's separator component and could never be applied to a design.

## [2.5.3] - 19/08/2026

### Changed

- Example notebook ECM Generation: truncated the thermal constants output.

## [2.5.2] - 18/08/2026

### Changed

- Example notebook ECM Generation updated: denser fit grids and new comparison plots.

## [2.5.1] - 16/08/2026

First published release containing the 2.5.0 changes below. No package changes beyond the release itself.

## [2.5.0] - 14/08/2026

### Added

- ECM generation: `generate_ecm` fits an equivalent circuit model (1, 2 or 3 RC pairs) to any cell design and returns look-up tables over SoC, temperature and C-rate, plus OCV curves and thermal constants. Configure the fit with `ecm_options`, inspect it with `to_dataframe()` and `plot_parameters()`, and round-trip it with `save()`/`load()`.
- Aged-cell ECMs: `run_ageing_sim(..., ecm=True)` fits the ECM at the end-of-campaign aged state, available as `result.ecm`. Compare against a fresh fit to see how the circuit parameters drift with degradation.
- Example notebook ECM Generation walks through a fresh fit and a fresh versus aged comparison.

### Changed

- Queued `run_sim` jobs no longer have a client-side time limit, matching `run_ageing_sim`.

## [2.3.0] - 06/08/2026

### Added

- `capacityVariationPercent` design knob on `run_sim` (-5 to +5): cell-to-cell capacity variation, matching the Simulink block parameter. See the cell parameter list and the run_sim docstring.
- Cell-to-cell capacity variation section in the Manufacturing Variability example notebook.

## [2.2.0] - 29/07/2026

### Added

- Degradation simulation: `run_ageing_sim` runs cycle-by-cycle ageing campaigns (baseline RPT, N ageing cycles, RPT, repeat) on degradation-calibrated cells, with SEI growth, lithium plating and mechanical damage mechanisms.
- `AgeingCycler` builds the ageing protocol: CC-CV cycling in voltage and/or SoC windows, calendar storage, fully custom cycles, and drive cycles from CSV with per-leg ambient temperatures and voltage/SoC guards.
- Power as an input alongside current: CSV power profiles (`type="power"`), power control arrays, and command strings in watts.
- `RptCycler` builds the check-up: user-defined capacity checks, HPPC-style DCIR pulse maps, configurable rests and conditioning, and an optional dedicated DVA cycle.
- `describe()`, `template()`, `preview()` and `plot_preview()` on both builders.
- Campaign controls: RPT cadence, stop criteria (SoH, DCIR, energy throughput, time, max cycles), SoC reference (latest RPT or beginning of life), phased schedules, and thermal controls matching `run_sim`.
- End-of-campaign performance test: any `run_sim` cycler executed on the aged cell, returned in `run_sim`'s format.
- `AgeingSimulationResults`: per-RPT, per-capacity-check, per-pulse and per-cycle tables, a labelled campaign timeseries with step filtering, DVA/ICA curves, fast-charge time over life (`charge_time_soc_window`) with an opt-in per-cycle charge analysis, the LLI split between SEI and plating, the per-cycle minimum anode potential, and plots for each.
- `get_operating_window(base_battery)`: the enforced limits for both simulation families, with an explicit answer when a cell has no degradation model.
- Calibration validity windows enforced on everything that runs. Campaigns outside the fitted range are rejected up front.
- Example notebook series: a first campaign, protocol design, results analysis, and performance testing fresh and aged cells.

##  [2.1.5] - 23/06/2026

### Fixed

- typo in example notebook

##  [2.1.4] - 23/06/2026

### Added

- `isothermal` flag on `run_sim`  runs the dynamic simulation at constant cell temperature (held at `initialTemperature_degC`, no self-heating or ambient exchange). Applies to every run in a batch sweep and is recorded in `input_parameters`.
- `electrodeArea_cm2` (cathode plate area) and `anodeCathodeAreaRatio` (anode/cathode coating-area ratio) exposed as read-only outputs in `get_design_parameters`. If passed back inside a design they are dropped with a guidance warning, they are geometry-derived, not settable knobs.

### Changed

- `heatTransferCoefficient` upper bound removed (was 1000 W/m²/K); any value `>= 0` is now accepted. Lower bound unchanged.

## [2.1.0] - 28/03/2026

### Added

- optional dynamic-analysis controls for anode potential, temperature, and SoC / voltage-based step profiles

## [2.0.1] - 19/03/2026

### Changed

- updated the registration link in the README

## [2.0.0] - 18/03/2026

### Changed

- `breathe_design` is now called `breathe_simulate`

## [1.8.1] - 03/03/2026

### Added

- deprecation warning for `breathe_design` which is now moving to `breathe_simulate`

## [1.8.0] - 25/02/2026

### Added

- Optional `initialVoltageOcvType` argument to `run_sim` to control how initial SOC for simulations is determined when using the `initialVoltage` argument

## [1.7.0] - 11/02/2026

### Added

- optional parameter`heatCapacity_kJkgK` to `run_api` to pass heat capacity to api

### Fixed

- Ensure DCIR values are positive

## [1.6.0] - 05/02/2026

### Changed

- significant improvements in simulation speed and flexibility

## [1.5.0] - 13/01/2026

### Added

- More flexible dynamic simulation with custom experiments and drive cycles

## [1.4.0] - 02/01/2026

### Added

- `run_sim` function now accepts initial voltage or SoC

## [1.3.3] - 10/12/2025

## Changed

- Renaming of variable names in dynamic results
  Available variables:
  Time [s]
  Voltage [V]
  Negative electrode potential [V]
  Positive electrode potential [V]
  Open-circuit voltage [V]
  Negative electrode open-circuit potential [V]
  Positive electrode open-circuit potential [V]
  Cell temperature [°C]
  SoC
  Negative electrode state of charge
  Positive electrode state of charge
  Current [A]
  Heat generation total [W]

## [1.3.2] - 03/12/2025

### Fixed

- Download design with no designs now completes successfully and downloads the Baseline design

## [1.3.1] - 03/12/2025

### Added

- `heatTransferCoefficient` parameter to dynamic simulations for better thermal control

## [1.3.0] - 01/12/2025

### Changed

- Improved performance of simulation functions

## [1.2.3] - 26/11/2025

### Changed

- changed some material names to lower case for consistency

## [1.2.2] - 17/11/2025

### Removed

- the example code from the README. The notebooks provide examples of how to use the API.

## [1.2.1] - 14/11/2025

### Added

- Added additional information to error messages, to help debugging/bug hunting

### Changed

- Update readme with details of sign up form to register for api access

## [1.2.0] - 13/11/2025

### Changed

- reduce minimum python version from `3.11` to `3.9`

### Fixed

- Bug that prevented `DCIR` simulations from running

## [1.1.0] - 02/10/2025

### Added

- Progress bar to show simulation progress
- Examples and utilities for performing Monte Carlo explorations of design variations. See the example notebook `10 Manufacturing Variability.ipynb`

### Removed

- Incorrect cell and link in the example notebook `01 Getting Started.ipynb`

## [1.0.2] - 25/09/2025

### Fixed

- Fixed possible timeout when running a large number of simulations at once

## [1.0.0] - 16/09/2025

- Breathe Simulate is released.
