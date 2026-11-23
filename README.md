# Resilient Regional Fuel Depot Network Design

This repository contains mixed-integer linear programming (MILP) models for locating fuel depots across a regional road network with a maximum vehicle travel range ($D_{\max}$). The models are implemented in Python using `gurobipy` and `networkx`.

## Problem Overview

Given a weighted road network $G = (V, E, w)$ and a maximum single-tank range $D_{\max}$, the goal is to open the minimum number of fuel depots so that:
1. Every location in the network is covered within $D_{\max}$ of an opened depot.
2. All opened depots form a single connected backbone using feasible single-leg trips.

The notebook evaluates three progressively stricter operational scenarios:
- **Baseline (`Baseline`):** Full coverage and backbone connectivity on the intact road network.
- **Scenario A (`Scenario_A`):** Intact double coverage, plus surviving coverage and backbone connectivity after the outage of any single opened fuel depot.
- **Scenario B (`Scenario_B`):** Intact double coverage, plus surviving coverage and backbone connectivity after the disruption of any single node in the road network (depot or non-depot).

## Modeling Approach

- **Reachability Graph ($H$):** Computed via all-pairs Dijkstra shortest paths on $G$, connecting any pair of nodes with $d_{ij} \le D_{\max}$.
- **Post-Outage Reachability ($H_{-k}$):** For each disrupted node $k \in V$, node $k$ is removed from the original road network $G$ and shortest paths are recomputed before building $H_{-k}$. This captures detours that exceed $D_{\max}$ when a node goes offline.
- **Coverage:** Formulated with closed-neighborhood constraints ($N[i]$ and $N_{-k}[i]$).
- **Connectivity:** Enforced via single-commodity flow-balance equalities using a primary anchor depot and a backup anchor depot for outage scenarios.

## Benchmark Instances & Results

The models are tested on three synthetic road networks:
- **Small Instance:** 10 nodes, 18 roads ($D_{\max} = 20.0\text{ km}$)
- **Medium Instance:** 20 nodes, 36 roads ($D_{\max} = 20.0\text{ km}$)
- **Large Instance:** 30 nodes, 57 roads ($D_{\max} = 44.31\text{ km}$)

| Instance | Baseline Depots | Scenario A Depots | Scenario B Depots |
| :--- | :---: | :---: | :---: |
| Small (10 nodes) | 2 | 5 | 6 |
| Medium (20 nodes) | 4 | 7 | 7 |
| Large (30 nodes) | 3 | 6 | 6 |

### Network Topology & Depot Placement Plots

Each plot shows the road network graph, node IDs, and selected depot locations (highlighted in red) under each resilience level:

| Instance | Baseline | Scenario A (Depot Outage) | Scenario B (Any Node Outage) |
| :--- | :---: | :---: | :---: |
| **Small (10 nodes)** | <img src="https://github.com/user-attachments/assets/9064d6bc-140e-4006-b17a-55e72e7cfabb" alt="Small Base Case" width="320" /> | <img src="https://github.com/user-attachments/assets/4f5adb6c-5da9-4499-951d-3aa9d71c7f4e" alt="Small A Case" width="320" /> | <img src="https://github.com/user-attachments/assets/a4e57a91-ea31-417c-a644-ee9475ca3090" alt="Small B Case" width="320" /> |
| **Medium (20 nodes)** | <img src="https://github.com/user-attachments/assets/66baa6c6-ff5f-4b05-a9ed-1a6e0cc99682" alt="Medium Base Case" width="320" /> | <img src="https://github.com/user-attachments/assets/2fcd7e4e-6cf8-4d91-ba4d-cb3242ecbb8e" alt="Medium A Case" width="320" /> | <img src="https://github.com/user-attachments/assets/c4d6ace7-2093-4356-8dea-6cd4c5a9212c" alt="Medium B Case" width="320" /> |
| **Large (30 nodes)** | <img src="https://github.com/user-attachments/assets/1850ed6f-3e08-4e47-9e17-ab217616c0ca" alt="Large Base Case" width="320" /> | <img src="https://github.com/user-attachments/assets/d709a364-d988-4014-a5a0-f66c62269395" alt="Large A Case" width="320" /> | <img src="https://github.com/user-attachments/assets/b49e0fc8-703d-4688-b528-3ceb58d61ee7" alt="Large B Case" width="320" /> |

### What the Plots Show
- **Resilience costs more depots:** Moving from `Baseline` to `Scenario A` and `Scenario B` adds extra depots across all three network sizes so that backup coverage and connected detours remain feasible when a depot or road node fails.
- **Spatial dispersion across the grid:** `Baseline` solutions place minimal, central-leaning depots, whereas `Scenario A` and `Scenario B` spread depots across outer and peripheral clusters to prevent single-point isolation.
- **Scenario A vs. Scenario B behavior:** On the Medium (7 depots) and Large (6 depots) instances, the depot count stays equal between Scenario A and Scenario B with shifts in selected nodes, whereas the Small instance increases from 5 to 6 depots when any road node can fail.

## Future plans
I plan on making a Case study for the Area of Amsterdam with correct travel distances, accurate map and having 50 potential locations. This is plan the "To Do" for the upcoming weeks 

## Requirements

- Python 3.9+
- Gurobi Optimizer (`gurobipy`) with an active license
- `networkx`, `matplotlib`, `numpy`, `pandas`

## Usage

1. Clone the repository:
   ```bash
   git clone [https://github.com/](https://github.com/)<ivaylo-iliev-31>/Resilient_Fuel_Depot_Network.git
   cd Resilient_Fuel_Depot_Network
