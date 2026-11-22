# Resilient Regional Fuel Depot Network Design

This repository contains mixed-integer linear programming (MILP) models for locating fuel depots across a regional road network with a maximum vehicle travel range ($D_{\max}$). The models are implemented in Python using `gurobipy` and `networkx`.

## Problem Overview

Given a weighted road network $G = (V, E, w)$ and a maximum single-tank range $D_{\max}$, the goal is to open the minimum number of fuel depots so that:
1. Every location in the network is covered within $D_{\max}$ of an opened depot.
2. All opened depots form a single connected backbone using feasible single-leg trips.

The notebook evaluates three progressively stricter operational scenarios:
* **Baseline (`Baseline`):** Full coverage and backbone connectivity on the intact road network.
* **Scenario A (`Scenario_A`):** Intact double coverage, plus surviving coverage and backbone connectivity after the outage of any single opened fuel depot.
* **Scenario B (`Scenario_B`):** Intact double coverage, plus surviving coverage and backbone connectivity after the disruption of any single node in the road network (depot or non-depot).

## Modeling Approach

* **Reachability Graph ($H$):** Computed via all-pairs Dijkstra shortest paths on $G$, connecting any pair of nodes with $d_{ij} \le D_{\max}$.
* **Post-Outage Reachability ($H_{-k}$):** For each disrupted node $k \in V$, node $k$ is removed from the original road network $G$ and shortest paths are recomputed before building $H_{-k}$. This captures detours that exceed $D_{\max}$ when a node goes offline.
* **Coverage:** Formulated with closed-neighborhood constraints ($N[i]$ and $N_{-k}[i]$).
* **Connectivity:** Enforced via single-commodity flow-balance equalities using a primary anchor depot and a backup anchor depot for outage scenarios.

## Benchmark Instances

The models are tested on three synthetic road networks:
* **Small Instance:** 10 nodes, 18 roads ($D_{\max} = 20.0\text{ km}$)
* **Medium Instance:** 20 nodes, 36 roads ($D_{\max} = 20.0\text{ km}$)
* **Large Instance:** 30 nodes, 57 roads ($D_{\max} = 44.31\text{ km}$)

| Instance | Baseline Depots | Scenario A Depots | Scenario B Depots |
| :--- | :---: | :---: | :---: |
| Small (10 nodes) | 2 | 5 | 6 |
| Medium (20 nodes) | 4 | 7 | 7 |
| Large (30 nodes) | 3 | 6 | 6 |

*(Update the Medium Instance cells once you run all three models on `data_20`.)*

## Requirements

* Python 3.9+
* Gurobi Optimizer (`gurobipy`) with an active license
* `networkx`, `matplotlib`, `numpy`, `pandas`

## Usage

1. Clone the repository:
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/Resilient_Fuel_Depot_Network.git
   cd Resilient_Fuel_Depot_Network
