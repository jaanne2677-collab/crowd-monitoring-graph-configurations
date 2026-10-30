
# Crowd Monitoring Graph Configurations

This repository provides graph configuration files and derived evacuation candidate maps for the study "Deployment-Oriented Evaluation of AI-Driven Crowd Monitoring Systems: A Two-Stage Decoupled Framework under Structured Spatial Constraints."

The configurations cover three simulated deployment environments: FCU Campus, Taichung Lantern Festival, and Taipei New Year's Eve.

## 1. Configuration Sources

Two configuration sources are included:

- **Human-authored:** Graph configurations used in the main deployment experiments. Node traversal costs and directed edge costs were manually assigned according to assumed evacuation conditions.
- **AI-assisted:** Graph configurations generated once per topology using GPT, the corresponding spatial layout, and predefined constraints. These configurations were used in the separate human-versus-AI comparison experiments.

Both configuration sources use the same evacuation candidate-selection rules.

## 2. File Descriptions

The repository contains 15 JSON files organized into two directories:

| Directory | File type | Description |
|---|---|---|
| `Human-authored/` | `*_neighbors.json` | Node capacities, node traversal costs, adjacency records, and directed edge costs. |
| `Human-authored/` | `*_map_neww.json` | Derived candidate evacuation destinations and their cumulative movement costs. |
| `Human-authored/` | `*_rule.json` | Available cost semantics and candidate-selection rules. |
| `Ai-assisted/` | `*_neighbors_GPT.json` | GPT-generated node and edge cost configurations. |
| `Ai-assisted/` | `*_map_GPT.json` | Derived evacuation candidate maps for the AI-assisted configurations. |

The files correspond to FCU Campus, Taichung Lantern Festival, and Taipei New Year's Eve.

## 3. Cost Calculation and Candidate Selection

Cumulative route cost is calculated using directed edge costs and the traversal costs of intermediate nodes.

For a route containing multiple edges, intermediate-node traversal costs are added when the route passes through those nodes. The traversal cost of the final destination is not added.

The following selection rules apply:

1. A route is eligible only when its cumulative cost is at most 3.
2. Routes exceeding the threshold of 3 are excluded from the evacuation candidate map.
3. Eligible destinations are prioritized by ascending cumulative cost, with lower-cost routes preferred.
4. The swimming-pool zone in the FCU topology is prohibited as an evacuation destination.

**Important:** The field named `hops` in the derived candidate-map JSON files represents cumulative route cost, not the number of traversed edges.

## 4. Representative Cost Example

In the human-authored FCU graph:

- The directed edge from node 8 to node 11 has a cost of 1.
- Node 11 has a traversal cost of 2.
- The directed edge from node 11 to node 13 has a cost of 1.

Therefore:

- Route 8 -> 11: cumulative cost = 1 (eligible).
- Route 8 -> 11 -> 13: cumulative cost = 1 + 2 + 1 = 4 (excluded under the cost threshold).

This example describes the specified route only. A destination may remain eligible through another route whose cumulative cost does not exceed 3.

## 5. Interpretation and Limitations

Node traversal costs and directed edge costs are scenario-specific simulation parameters. They should not be interpreted as measured evacuation travel times or calibrated physical distances.

The human-authored configurations support the main deployment experiments, while the AI-assisted configurations support separate rule-source comparisons.

These files document the graph parameters and derived candidate evacuation maps. They do not include the complete perception models, decision execution pipeline, experimental trial records, or statistical analysis implementation needed to reproduce every result in the paper.
