# Enhancing Road Safety Modeling with Graph Neural Networks: The Impact of Slope Inclusion

- Link to the base research: https://arxiv.org/pdf/2311.00164
- Link to the codebase of the research: https://github.com/VirtuosoResearch/ML4RoadSafety

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

## Experimental Results

We anticipated that integrating slope data into the Graph Neural Networks (GNNs) for road safety modeling would lead to notable improvements in performance metrics. This expectation is supported by the findings of Kuşkapan, E., et al. (2024), who observed that slopes exceeding 6% combined with snow significantly impacted traffic accidents, emphasizing the critical role of slope data in understanding accident-prone road conditions.

Specifically:
- **AUROC** improved from **82.62%** to **83.22%**, marking an increase of **60 basis points**. This enhancement demonstrates the model’s improved ability to rank accident-prone road segments more effectively.
- **Precision** increased from **5.26%** to **8.13%**, indicating a reduction in false positives and enhancing the reliability of the model’s predictions.
- However, a slight decline in **Recall** was observed, dropping from **52.01%** to **47.14%**, which suggests that while the model became more precise, it slightly compromised its ability to identify true positives.

This trade-off highlights the need for further optimization through feature engineering and hyperparameter tuning based on this new information.

To summarize the performance metrics of both models, the table below provides a detailed comparison of the **AUROC**, **Precision**, and **Recall** for the Train, Validation, and Test datasets.

| **Metric**       | **Model - Slope**  | **Model - No Slope** | **Improvement (b.p)** |
|-------------------|--------------------|-----------------------|------------------------|
| **Train AUROC**   | 83.31 ± 0.35       | 82.99 ± 0.15          | +32                   |
| **Valid AUROC**   | 83.03 ± 0.12       | 82.73 ± 0.04          | +30                   |
| **Test AUROC**    | 83.22 ± 0.30       | 82.62 ± 0.21          | +60                   |
| **Train Precision** | 11.89 ± 4.32     | 8.29 ± 3.92           | +360                  |
| **Valid Precision** | 8.96 ± 2.93      | 6.46 ± 1.36           | +250                  |
| **Test Precision**  | 8.13 ± 2.77      | 5.26 ± 1.25           | +287                  |
| **Train Recall**  | 44.35 ± 5.56       | 55.46 ± 5.54          | -1111                 |
| **Valid Recall**  | 46.79 ± 6.60       | 55.96 ± 3.90          | -917                  |
| **Test Recall**   | 47.14 ± 8.47       | 52.01 ± 1.34          | -487                  |

**Table 1**: Performance comparison between models trained with and without slope data across AUROC, Precision, and Recall metrics for Train, Validation, and Test datasets. Improvements are expressed in basis points (b.p).

## Conclusions and Future Work

The findings from this study highlight the potential of integrating slope data into GNNs for improving road safety modeling. The enhanced **AUROC** and **Precision** reflect the model’s better ranking of accident-prone areas and reduced false positives, contributing to more accurate predictions. 

However, the trade-off between **Precision** and **Recall** underscores the need for additional refinement of the model. 

### Future Work:
- Testing the model across diverse geographical areas.
- Improving the quality of slope data using the **Google Maps API**.
- Optimizing the model parameters based on the new slope data.
