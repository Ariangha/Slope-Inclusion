# Slope-Inclusion

1 Abstract
Recent studies have begun leveraging Graph Neural Networks (GNNs) to predict traffic accidents
by uncovering spatiotemporal patterns in road networks (Nippani et al., 2017; Gao X. et al.,
2024), facilitating improved urban planning and road safety. In this work, we aimed to replicate
the findings of Nippani et al. (2017) and evaluate the impact of incorporating slope data into
the model. Our experiments demonstrated an improvement of 60 basis points in AUROC after
integrating slope information, highlighting its significance in enhancing predictive performance.
2 Introduction
In this study, we focused on the state of Montana, as it was one of the states analyzed in the
original paper. Montana was chosen not only for consistency with prior work but also due to
its smaller road network size, which reduced computational demands and minimized API
calls required to retrieve elevation data for slope calculations. Our workflow was structured into
two main phases: the dataset creation where we extended the original dataset by introducing
a new tensor that encodes slope information for edges in the road networ and the training and
testing where we trained and tested the model using the same Graph Convolutional Network
(GCN) architecture recommended by the original paper, consisting of two layers.
3 Dataset and Benchmark
The base dataset used in this study was sourced from the Harvard Dataverse and covers
traffic data for Montana, USA, from 2016 to 2020. Node-level features included latitude,
longitude, node indegree/outdegree, betweenness centrality, and meteorological variables such
as average, maximum, and minimum temperatures, precipitation, wind speed, and sea-level
pressure. Edge-level features comprised binary indicators for one-way roads, multi-class road
types, road lengths, and Annual Average Daily Traffic (AADT). Slope data was introduced as a
key enhancement in this study, derived from the Open-Elevation API. The slope for each edge
was calculated as the elevation difference between the target and source nodes divided by the
edge length as in the formula below:
slope =
Etarget − Esource
L
(1)
This feature captures road inclines and declines, which are particularly relevant in mountainous
terrains or areas with steep gradients. However, sharp elevation changes introduced noise into
the slope data, which was mitigated by applying thresholds. Unrealistic slopes were filtered
by capping extreme values above ±0.3 to ±0.05, and slopes exceeding ±0.2 on roads shorter
than 50 meters were set to 0. To further improve slope accuracy, higher-precision APIs, such as
Google Maps, and alternative encodings (e.g., categorical representations of slope: flat, mild, or
steep) are proposed for future studies. The processed slope data was ultimately incorporated
into the edge features tensor to enable the GNN to leverage this information during training
and prediction.
4 Model and Approach
We implemented a Graph Convolutional Network (GCN) with two graph convolutional layers
to predict traffic accidents. The model utilized a hidden dimensionality of 256, a learning rate
of 0.001, and was optimized using the Adam optimizer over 100 epochs. These hyperparameters
and the GCN architecture were adopted in this project based on the recommendations from the
original paper for comparison purposes. The model performed solidly across all states tested,
1
and the chosen hyperparameters demonstrated the best performance in terms of predictive
accuracy. Training and evaluation were conducted using a train/validation/test split, covering
the years 2016–2017 for training (40,040 records), 2018 for validation (20,677 records), and
2019–2020 for testing (39,222 records).
5 Experimental Results
We anticipated that integrating slope data into the Graph Neural Networks (GNNs) for road
safety modeling would lead to notable improvements in performance metrics. This expectation is
supported by the findings of Ku¸skapan, E., et al. (2024), who observed that slopes exceeding 6%
combined with snow significantly impacted traffic accidents, emphasizing the critical role of slope
data in understanding accident-prone road conditions. Specifically, the AUROC improved from
82.62% to 83.22%, marking an increase of 60 basis points. This enhancement demonstrates the
model’s improved ability to rank accident-prone road segments more effectively. Additionally,
Precision increased from 5.26% to 8.13%, indicating a reduction in false positives and enhancing
the reliability of the model’s predictions. However, a slight decline in Recall was observed,
dropping from 52.01% to 47.14%, which suggests that while the model became more precise,
it slightly compromised its ability to identify true positives. This trade-off highlights the need
for further optimization through feature engineering and hyperparameter tuning based on this
new information. To summarize the performance metrics of both models, Table 1 provides a
detailed comparison of the AUROC, Precision, and Recall for the Train, Validation, and
Test datasets.
