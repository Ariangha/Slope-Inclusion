# Enhancing Road Safety Modeling with Graph Neural Networks: The Impact of Slope Inclusion

## Abstract
Recent studies have begun leveraging Graph Neural Networks (GNNs) to predict traffic accidents by uncovering spatiotemporal patterns in road networks (Nippani et al., 2017; Gao X. et al., 2024), facilitating improved urban planning and road safety. In this work, we aimed to replicate the findings of Nippani et al. (2017) and evaluate the impact of incorporating slope data into the model. Our experiments demonstrated an improvement of **60 basis points in AUROC** after integrating slope information, highlighting its significance in enhancing predictive performance.

## Introduction
In this study, we focused on the state of Montana, as it was one of the states analyzed in the original paper. Montana was chosen not only for consistency with prior work but also due to its smaller road network size, which reduced computational demands and minimized API calls required to retrieve elevation data for slope calculations.

Our workflow was structured into two main phases:
1. **Dataset creation**: Extended the original dataset by introducing a new tensor that encodes slope information for edges in the road network.
2. **Training and testing**: Trained and tested the model using the same Graph Convolutional Network (GCN) architecture recommended by the original paper, consisting of two layers.

## Dataset and Benchmark
The base dataset used in this study was sourced from the **Harvard Dataverse** and covers traffic data for Montana, USA, from 2016 to 2020.

### Node-level features:
- Latitude, longitude
- Node indegree/outdegree
- Betweenness centrality
- Meteorological variables:
  - Average, maximum, and minimum temperatures
  - Precipitation
  - Wind speed
  - Sea-level pressure

### Edge-level features:
- Binary indicators for one-way roads
- Multi-class road types
- Road lengths
- Annual Average Daily Traffic (AADT)
- **Slope data**:
  - Derived from the Open-Elevation API
  - Calculated as:
    \[
    \text{slope} = \frac{E_{\text{target}} - E_{\text{source}}}{L}
    \]
    Where \(E\) is the elevation and \(L\) is the edge length.

### Slope Data Processing:
- Mitigated noise in slope data by:
  - Capping extreme values (±0.3 to ±0.05).
  - Setting slopes exceeding ±0.2 to 0 for roads shorter than 50 meters.
- Proposed improvements for future studies:
  - Use higher-precision APIs (e.g., Google Maps).
  - Employ alternative encodings (e.g., categorical representations: flat, mild, or steep).

## Model and Approach
We implemented a **Graph Convolutional Network (GCN)** with the following specifications:
- **Architecture**: Two graph convolutional layers
- **Hidden dimensionality**: 256
- **Learning rate**: 0.001
- **Optimizer**: Adam
- **Epochs**: 100

The hyperparameters and GCN architecture were adopted based on the recommendations from the original paper for consistent comparison.

### Training and Evaluation:
- Train/Validation/Test split:
  - **Training**: 2016–2017 (40,040 records)
  - **Validation**: 2018 (20,677 records)
  - **Testing**: 2019–2020 (39,222 records)
- Model demonstrated solid performance across all tested states with optimized predictive accuracy.
