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
    - Associated R package [spatialcluster GitHub](https://mpadge.github.io/spatialcluster/)
