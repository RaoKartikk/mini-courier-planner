# Mini-Courier Planner — Project Report

## 1. Problem statement

The prototype accepts parcel records and a weighted road graph. It orders parcels by priority and deadline, selects a high-value feasible set of parcels under a vehicle weight limit, computes shortest paths between delivery points, and estimates a delivery tour using a nearest-neighbor TSP approximation. Runtime measurements are collected for larger synthetic inputs.

## 2. System design

1. Read parcel and road-edge data from `datasets/`.
2. Sort parcels by ascending priority number, then deadline and parcel ID.
3. Use 0/1 Knapsack Dynamic Programming to maximize total selected parcel value without exceeding capacity.
4. Build an undirected weighted graph from the route dataset.
5. Run Dijkstra's algorithm to calculate shortest road distances and paths.
6. Repeatedly visit the nearest unvisited selected destination; return to the depot at the end.
7. Validate key correctness properties and benchmark the algorithmic components.

## 3. Algorithms

| Requirement | Implementation | Complexity |
|---|---|---|
| Parcel prioritization | Python comparison sort | `O(n log n)` time |
| Optimal packing under the given capacity | 0/1 Knapsack DP table | `O(nW)` time and space |
| Shortest path between graph vertices | Dijkstra with a binary heap | `O((V + E) log V)` time |
| Overall delivery order | Nearest-neighbor TSP approximation using Dijkstra distances | `O((K + 1)(V + E) log V)` in this implementation |
| Runtime evaluation | `time.perf_counter()` and repeated measurements | Depends on the component being tested |

Definitions: `n` = parcel count; `W` = integer capacity; `V` = graph vertices; `E` = road edges; `K` = distinct selected destinations. Nearest-neighbor is a heuristic and does not guarantee the optimal TSP tour.

## 4. Sample inputs

- `datasets/parcels.csv` contains eight sample parcel records with destination, weight, value, priority, and deadline.
- `datasets/routes.csv` contains positive weighted edges for an undirected road graph.
- The demonstration capacity is 12 weight units, and the starting/ending location is `Depot`.

## 5. Validation and performance

The submitted notebook has been executed from top to bottom. All built-in correctness checks passed. Those checks confirm that selected parcel weight does not exceed capacity, the selected value matches the DP optimum, the shortest distance from `Depot` to `A` is 3 through `B`, and the approximate tour visits every selected delivery destination before returning to the depot.

### Sample run result

- Selected parcels: `P006`, `P003`, and `P002`.
- Total selected weight: 12 of 12 weight units.
- Total selected value: 125.
- Approximate delivery stop sequence: `Depot → B → C → F → Depot`.
- Total route distance: 25 distance units.
- Correctness checks: passed.

### Runtime evidence

The notebook measures sorting on 50–5,000 parcel records, Knapsack DP on 10–100 parcels with increasing capacity, Dijkstra on graphs of 20–200 vertices, and the route approximation on 9–49 delivery destinations. It uses repeated batched measurements and reports median per-call time. `images/runtime_comparison.png` contains the four charts; `images/runtime_measurements.csv` contains the underlying measurements. The `images/screenshots/` folder contains VS Code captures showing the loaded sample data, parcel prioritization and packing, graph/Dijkstra output, the final delivery route, correctness checks, and the runtime chart. The runtime measurements table is also saved in the notebook and `images/runtime_measurements.csv`.

**Results note:** runtime varies with hardware, Python/library versions, and system load. The included values are one execution's evidence, not universal performance guarantees. For submission, rerun the notebook on the machine used for the demonstration if updated local measurements are desired.

## 6. Limitations

- The data is a small synthetic demonstration dataset, not live courier data.
- Knapsack maximizes parcel value; the separately displayed priority order does not force priority parcels into the selected set.
- The routing heuristic is approximate and may produce a longer tour than the true optimal TSP solution.
- Distances represent static positive edge weights. Traffic, delivery time windows, vehicle fleets, and live road conditions are outside the assignment prototype's defined scope.

## 7. Conclusion

The project integrates sorting, dynamic programming, graph shortest paths, route approximation, correctness checks, and runtime analysis in one executable Python notebook. The notebook, datasets, generated runtime evidence, setup instructions, and this report form the submission package.
