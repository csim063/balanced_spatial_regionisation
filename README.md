# Spatial Regionalization Using Varying Approaches
## Case study using Statistics NZ SA2 data

[![R](https://img.shields.io/badge/R-4.3%2B-blue)](https://www.r-project.org/)
[![Quarto](https://img.shields.io/badge/Quarto-1.6%2B-orange)](https://www.r-project.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

### Background
Economic and transport models in New Zealand are often built around Statistics New Zealand's Statistical Area 2 ([SA2](https://datafinder.stats.govt.nz/layer/104271-statistical-area-2-2020-generalised/)) regions, which themselves were designed around resident populations, not necessarily economic activity. This mismatch can lead to unbalanced business analyses and models, which would be better served by regions designed around total businesses and employees. This repository explores several approaches, all implemented in R, to reagregate the SA2 regions into new contiguous regions that better balance business and employee counts.

### Approaches
1. **Greedy Spatial Algorithm**:
    - A simple custom greedy algorithm that iteratively merges adjacent SA2 regions based on minimizing a cost function related to business and employee counts.
    - *Cost Function*: minimize $J = \omega_E |E_r - E_t| + \omega_B |B_r - B_t| + \lambda$ where $E_r$ and $B_r$ are the employee and business counts of the region, $E_t$ and $B_t$ are the target counts, $\omega_E$, $\omega_B$ are weights, and $\lambda$ is a compactness penalty which limits shape bias.
2. **REDCAP**:
    - **Re**gionalization with **D**ynamically **C**onstrained **A**gglomerative **P**artitioning.
    - Region growing + local search algorithm using above cost function.
    - Adds refinement steps so early greedy choices can be revisited.
    - Associated R package [spatialcluster](https://mpadge.github.io/spatialcluster/).
3. **SKATER**:
    - **S**patial **K**luster **A**nalysis by **T**ree **E**dge **R**emoval.
    - Graph-based hierarchical clustering method.
    - Constructs a contiguity graph, weights edges by dissimilarity. Then builds a minimum spanning tree and prunes edges to form clusters.
    - Associated R package [spdep](https://r-spatial.github.io/spdep/reference/skater.html).
4. **Sequential Monte Carlo (SMC) regionalization**:
    - Sample many contiguous *k*-region plans using SMC with constraints:
        - Parity on employee and business counts
        - Penalize non-compact shapes
        - Contiguity
    - Yields a distribution of region plans
        - Use Pareto optimal plans to explore trade-offs between objectives
    - Associated R package [redist](https://www.rdocumentation.org/packages/redist/versions/2.0.1/topics/redist.smc). 
5. **Simulated Annealing (SA)**
    - Optimization algorithm inspired by annealing in metallurgy.
    - Obtain a single optimal plan by stochastically exploring the solution space moving towards lower energy (cooler) states.
    - *Energy*: $\epsilon = \alpha \cdot Parity(E) + \beta \cdot Imbalance(B) + \gamma \cdot Compactness + \delta \cdot BoundaryLength + ...$
    - Associated R package [redist](https://www.rdocumentation.org/packages/redist/versions/2.0.1/topics/redist.mcmc.anneal).

### Data
- [New Zealand business demography statistics at February 2020 (on statistical area 2 2020)](https://datafinder.stats.govt.nz/layer/105388-new-zealand-business-demography-statistics-at-february-2020-on-statistical-area-2-2020/): This data contains the number of businesses and total employees for each SA2 region in New Zealand. More formally, "Business demography statistics provide an annual snapshot (as at February) of the structure and characteristics of New Zealand businesses. Statistics produced include counts of enterprises and geographic units by industry, geography such as region or statistical area 2 (SA2), institutional sector, business type, degree of overseas ownership, enterprise births, enterprise deaths, survival rate of enterprises and employment levels.
The series covers economically significant private-sector and public-sector enterprises that are engaged in the production of goods and services in New Zealand. These enterprises are maintained on the Statistics NZ Business Register (BR), which generally includes all employing units and those enterprises with GST turnover greater than $30,000 per year."
For further information see [the official documentation](https://www.stats.govt.nz/information-releases/new-zealand-business-demography-statistics-at-february-2020/). 

### Getting started
TODO

### Repository structure

```
.
├── notebooks/
│   ├── 01_data_preparation.qmd          # Data preparation and cleaning
│   ├── 02_greedy_algorithm.qmd          # Implementation of the greedy spatial algorithm
│   ├── 03_redcap.qmd                    # Implementation of REDCAP algorithm
│   ├── 04_skater.qmd                    # Implementation of SKATER algorithm
│   ├── 05_smc_regionalization.qmd       # Implementation of SMC regionalization
│   ├── 06_simulated_annealing.qmd       # Implementation of Simulated Annealing
│   └── 07_comparative_analysis.qmd      # Comparative analysis of all approaches
├── scripts/
│   ├── 00_get_data.R                  # Script to download and prepare data
│   ├── 01_make_adjacency.R            # Script to create adjacency matrix
├── data/
├── outputs/
├── renv/
├── _quarto.yml
├── LICENSE
└── README.md
```

### Licence
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.