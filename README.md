# Flood-Risk-Weighted Emergency Shelter Optimization Using Exact p-Median Solvers and Reinforcement Learning

**Authors:** Christopher Atta Amponsah, Towfiqul Islam Khan, Chy Mansura 
Mehrun Mumu, Joni Downs  
**Institution:** School of Geosciences, University of South Florida  
**Corresponding author:** attaamponsahc@usf.edu  
**Journal:** Computers, Environment and Urban Systems (under review)

---

## Overview

This repository contains the data, code, and figures supporting the paper
"Flood-Risk-Weighted Emergency Shelter Optimization Using Exact p-Median
Solvers and Reinforcement Learning." The study develops a flood-risk-weighted
p-median optimization framework for emergency shelter placement in Pinellas
County, Florida, and benchmarks four computational methods across three
shelter allocation scenarios (p = 5, 8, and 10 open shelters).

---

## Repository Structure
lood-shelter-optimization-pinellas/
├── data/
│   ├── raw/                  # Original input data
│   ├── processed/            # Shared derived outputs
│   ├── cplex_aggregated/     # CPLEX Community Edition (38-cluster) outputs
│   ├── cplex_full/           # CPLEX Full licence (733-BG) outputs
│   ├── pulp_cbc/             # PuLP/CBC exact benchmark outputs
│   └── q_learning/           # Q-Learning outputs
├── notebooks/
│   ├── phase1_data_preparation/
│   ├── phase2a_optimization/
│   └── phase2b_results_visualisation/
└── figures/
├── analytical/
└── maps/


---

## Data Description

### Raw Data (`data/raw/`)

| File | Description | Source |
|---|---|---|
| `FEMA_Flood_Pinellas.csv` | FEMA NFHL flood zone classifications | FEMA MSC |
| `PC_BlockGroups_Population.csv` | 2020 ACS population, 733 block groups | U.S. Census Bureau |
| `Pinellas_PCEM_Shelters_view.csv` | 25 Non-Evacuation Zone candidate shelters | Pinellas County GIS |
| `Pinellas_PCEM_EvacRoute_view.csv` | 3,233 evacuation route segments | Pinellas County GIS |

### Processed Data (`data/processed/`)

| File | Description |
|---|---|
| `distance_matrix_network.csv` | 733 × 25 road-network OD travel-time matrix (minutes) |
| `results_table.csv` | Four-method results summary for all scenarios |

### CPLEX Aggregated (`data/cplex_aggregated/`)

Results from IBM CPLEX 22.1.2 Community Edition applied to the
38-cluster spatially aggregated problem (975 variables, 989 constraints —
within the Community Edition limit of 1,000).

| File | Description |
|---|---|
| `lp_files/pmedian_p5.lp` | LP problem file for p = 5 (38 clusters) |
| `lp_files/pmedian_p8.lp` | LP problem file for p = 8 (38 clusters) |
| `lp_files/pmedian_p10.lp` | LP problem file for p = 10 (38 clusters) |
| `cplex_results.json` | Objective values, gaps, solve times, shelter selections |
| `cplex_assignments_p5.csv` | Block group assignments for p = 5 |
| `cplex_assignments_p8.csv` | Block group assignments for p = 8 |
| `cplex_assignments_p10.csv` | Block group assignments for p = 10 |

### CPLEX Full (`data/cplex_full/`)

Results from IBM CPLEX 22.1.2 with a full commercial licence applied
to the complete 733-BG problem (18,325 variables, 19,059 constraints).
The LP files allow any researcher with a CPLEX licence to independently
verify these results.

| File | Description |
|---|---|
| `lp_files/pmedian_full_p5.lp` | LP problem file for p = 5 (733 BGs) |
| `lp_files/pmedian_full_p8.lp` | LP problem file for p = 8 (733 BGs) |
| `lp_files/pmedian_full_p10.lp` | LP problem file for p = 10 (733 BGs) |
| `cplex_full_results.json` | Objective values, gaps, solve times, shelter selections |
| `cplex_full_assignments_p5.csv` | Block group assignments for p = 5 |
| `cplex_full_assignments_p8.csv` | Block group assignments for p = 8 |
| `cplex_full_assignments_p10.csv` | Block group assignments for p = 10 |

Expected objective values for independent verification:
- p = 5: **4,184,071.38** person·min
- p = 8: **3,774,238.41** person·min
- p = 10: **3,641,579.59** person·min

### PuLP/CBC (`data/pulp_cbc/`)

Results from PuLP/CBC open-source solver applied to the complete
733-BG problem. Serves as the exact benchmark for all gap calculations.

| File | Description |
|---|---|
| `pulp_results.json` | Objective values, solve times, shelter selections |
| `pulp_assignments_p5.csv` | Block group assignments for p = 5 |
| `pulp_assignments_p8.csv` | Block group assignments for p = 8 |
| `pulp_assignments_p10.csv` | Block group assignments for p = 10 |

### Q-Learning (`data/q_learning/`)

Results from the Q-Learning reinforcement learning agent trained on
the complete 733-BG problem for 3,000 episodes with 300 swap steps
per episode.

| File | Description |
|---|---|
| `rl_results.json` | Objective values, optimality gaps, training times |
| `rl_assignments_p5.csv` | Block group assignments for p = 5 |
| `rl_assignments_p8.csv` | Block group assignments for p = 8 |
| `rl_assignments_p10.csv` | Block group assignments for p = 10 |

---

## Notebooks

### Phase 1 — Data Preparation (`notebooks/phase1_data_preparation/`)

| Notebook | Description |
|---|---|
| `phase1a_population_join.ipynb` | Joins TIGER block group boundaries with ACS 2020 population |
| `phase1a_flood_demand.ipynb` | Spatial join of FEMA flood zones; computes wᵢ = Pᵢ · φᵢ |
| `phase1b_od_matrix.ipynb` | Builds 733 × 25 OD travel-time matrix in ArcGIS Pro |
| `phase1b_diagnostic_unroutable.ipynb` | Identifies unroutable barrier island block group pairs |

> **Note:** `phase1b_od_matrix.ipynb` requires ArcGIS Pro with the
> Network Analyst extension. All other notebooks use the pre-computed
> `distance_matrix_network.csv` and do not require ArcGIS Pro.

### Phase 2A — Optimization (`notebooks/phase2a_optimization/`)

| Notebook | Description |
|---|---|
| `phase2a_pulp_pmedian_full733.ipynb` | Full 733-BG p-median via PuLP/CBC |
| `phase2a_cplex_lp_aggregated.ipynb` | Generates 38-cluster LP files for CPLEX Community Edition |
| `phase2a_cplex_lp_full733.ipynb` | Generates full 733-BG LP files for CPLEX full licence |
| `phase2a_cplex_solution_parser_full733.ipynb` | Parses CPLEX `.sol` output into assignment CSVs |

### Phase 2B — Results and Visualisation (`notebooks/phase2b_results_visualisation/`)

| Notebook | Description |
|---|---|
| `phase2b_qlearning_figures_results.ipynb` | Q-Learning solver and all analytical figures (Google Colab) |
| `phase2b_map_production.ipynb` | All publication maps using geopandas and contextily (Google Colab) |

---

## How to Reproduce the Results

### Step 1 — Install dependencies
```bash
pip install -r requirements.txt
```

### Step 2 — Run Phase 1 (data preparation)
Run the Phase 1 notebooks in order. Skip
`phase1b_od_matrix.ipynb` if you do not have ArcGIS Pro —
use the pre-computed `distance_matrix_network.csv` instead.

### Step 3 — Run Phase 2A (optimization)
Run `phase2a_pulp_pmedian_full733.ipynb` to reproduce the
PuLP/CBC exact benchmark. Run the CPLEX notebooks only if
you have access to IBM CPLEX.

### Step 4 — Run Phase 2B (results and figures)
Open `phase2b_qlearning_figures_results.ipynb` in Google Colab.
Update the Google Drive paths in Cell 1 to match your own Drive.
Upload the four method output files to your Drive before running.

### Step 5 — Verify CPLEX Full independently
Load the LP files from `data/cplex_full/lp_files/` into IBM CPLEX
Interactive Optimizer and run:
read pmedian_full_p5.lp
opt

Compare the reported objective value against the expected values
listed above.

---

## Software Requirements

| Software | Version | Purpose |
|---|---|---|
| Python | 3.10+ | All analysis notebooks |
| PuLP | ≥ 2.7.0 | Exact p-median optimization |
| NumPy | ≥ 1.24.0 | Numerical computation |
| pandas | ≥ 2.0.0 | Data processing |
| geopandas | ≥ 0.13.0 | Spatial data and mapping |
| matplotlib | ≥ 3.7.0 | Figure production |
| contextily | ≥ 1.3.0 | Basemap tiles for maps |
| scikit-learn | ≥ 1.3.0 | K-means clustering |
| ArcGIS Pro | 3.x | Network Analyst OD matrix (Phase 1B only) |
| IBM CPLEX | 22.1.2 | LP file solving (full licence for 733-BG) |
| Google Colab | — | Recommended for Phase 2B notebooks |

---

## Results Summary

| p | Method | Objective (person·min) | Gap vs PuLP/CBC | Time |
|---|---|---|---|---|
| 5 | CPLEX Aggregated | 4,140,078 | −1.05% | <1s |
| 5 | CPLEX Full | 4,184,071.38 | 0.00% | 1.09s |
| 5 | PuLP/CBC | 4,184,071.38 | — (ref) | 7.4s |
| 5 | Q-Learning | 4,184,071 | 0.00% | 124.0s |
| 8 | CPLEX Aggregated | 3,742,284 | −0.85% | <1s |
| 8 | CPLEX Full | 3,774,238.41 | 0.00% | 1.10s |
| 8 | PuLP/CBC | 3,774,238 | — (ref) | 28.2s |
| 8 | Q-Learning | 3,782,699 | +0.22% | 118.3s |
| 10 | CPLEX Aggregated | 3,647,364 | +0.16% | <1s |
| 10 | CPLEX Full | 3,641,579.59 | 0.00% | 1.20s |
| 10 | PuLP/CBC | 3,641,580 | — (ref) | 14.2s |
| 10 | Q-Learning | 3,652,909 | +0.31% | 127.5s |

---

## Citation

If you use this code or data in your research, please cite:
Atta Amponsah, C., Khan, T. I., Mumu, C. M. M., & Downs, J. (2026).
Flood-risk-weighted emergency shelter optimization using exact p-median
solvers and reinforcement learning. Computers, Environment and Urban
Systems. [Under review]

---

## Data Sources

- FEMA National Flood Hazard Layer: https://msc.fema.gov
- U.S. Census Bureau ACS 2020: https://data.census.gov
- Pinellas County GIS Portal: https://pinellas-egis.opendata.arcgis.com

---

## License

This project is licensed under the MIT License. See `LICENSE` for details.

---

## Contact

Christopher Atta Amponsah  
School of Geosciences, University of South Florida  
Tampa, FL 33620, USA  
attaamponsahc@usf.edu
