# Spatial Species Distribution Modelling: GLM vs Random Forest

> Predicting European badger (*Meles meles*) habitat suitability while evaluating how spatial autocorrelation affects model performance.

## Overview

Species Distribution Models are widely used to investigate relationships between species occurrence and environmental conditions. However, ecological relationships are often scale-dependent, and spatial clustering can cause conventional model validation to overestimate predictive performance.

This project compares a **Generalised Linear Model (GLM)** and a **Random Forest model** for predicting the spatial distribution of the European badger (*Meles meles*).

The analysis focuses on three questions:

1. At what spatial scales are woodland and urban land cover most strongly associated with badger occurrence?
2. How do badgers respond to woodland cover, urban land cover, and elevation?
3. How does model performance change when spatial dependence is considered during validation?

## Key Findings

* Broadleaf woodland showed its strongest relationship with badger occurrence at approximately **900 metres**.
* Urban land cover showed its strongest relationship at approximately **1,500 metres**.
* Predicted occurrence probability increased with broadleaf woodland cover.
* Elevation had a negative relationship with predicted occurrence probability.
* Random Forest achieved the strongest performance under conventional non-spatial cross-validation.
* Both models performed worse under spatial cross-validation.
* Random Forest showed a larger reduction in performance than GLM after controlling for spatial dependence.
* Ripley’s K analysis identified substantial spatial clustering in the occurrence records.
* GLM provided more stable spatial generalisation and clearer ecological interpretation, while Random Forest captured more complex and localised patterns.

## Why This Project Matters

A model can appear highly accurate when geographically adjacent observations are divided randomly between the training and test sets. Because neighbouring locations often have similar environmental conditions, the model may partly memorise local spatial patterns rather than learn relationships that generalise to new areas.

This project demonstrates why **spatial cross-validation** is important when evaluating models built from geographically clustered data.

The same principle applies beyond ecology to:

* urban analytics;
* retail location modelling;
* mobility analysis;
* property price prediction;
* environmental risk modelling;
* geospatial machine learning.

## Data Preparation

Before modelling, the species occurrence records were:

* filtered to remove missing coordinates;
* filtered to remove records with high coordinate uncertainty;
* clipped to the study area;
* transformed into a projected coordinate reference system;
* combined with **2,000 randomly generated background points** used as pseudo-absence observations.

Three environmental predictors were included:

| Predictor          | Description                                                   | Processing                              |
| ------------------ | ------------------------------------------------------------- | --------------------------------------- |
| Broadleaf woodland | Proportion of broadleaf woodland surrounding each observation | Tested across multiple buffer distances |
| Urban land cover   | Combined urban and suburban land-cover classes                | Tested across multiple buffer distances |
| Elevation          | Elevation extracted from a Digital Elevation Model            | Used at its original spatial resolution |

## Multiscale Feature Engineering

Environmental effects may operate at different spatial scales. To identify an appropriate characteristic scale, buffers of different radii were created around each sample point.

A series of binomial models was fitted for each buffer distance, and model log-likelihood was used to select the scale with the strongest relationship to species occurrence.

The selected characteristic scales were:

| Environmental variable |        Selected scale |
| ---------------------- | --------------------: |
| Broadleaf woodland     |   Approximately 900 m |
| Urban land cover       | Approximately 1,500 m |

This multiscale approach avoids selecting buffer distances arbitrarily and allows the predictors to better represent the spatial scale at which ecological processes operate.

## Modelling Approach

Two binary classification models were developed within a consistent modelling framework.

### Generalised Linear Model

The GLM was implemented as a logistic regression model with a logit link function.

Advantages:

* interpretable coefficients;
* clear response direction;
* relatively smooth prediction surfaces;
* suitable for ecological inference.

### Random Forest

Random Forest combines predictions from multiple decision trees trained using random subsets of observations and predictors.

Advantages:

* captures nonlinear relationships;
* models complex interactions;
* makes fewer assumptions about response shape;
* produces flexible local predictions.

## Model Evaluation

Model discrimination was evaluated using the **Area Under the Receiver Operating Characteristic Curve**, or AUC.

Two validation strategies were compared.

### Non-Spatial Cross-Validation

Observations were randomly divided between training and test sets.

This approach estimates predictive performance when nearby observations may appear in both datasets.

### Spatial Cross-Validation

Observations were divided into geographically separated training and test groups.

This provides a more conservative estimate of how well the model may generalise to new locations.

### Model Performance

Replace the placeholders below with the values exported from the modelling results.

| Model         | Non-spatial CV AUC | Spatial CV AUC |
| ------------- | -----------------: | -------------: |
| GLM           |     `[insert AUC]` | `[insert AUC]` |
| Random Forest |     `[insert AUC]` | `[insert AUC]` |

Random Forest achieved the highest AUC under non-spatial validation. However, its performance decreased more substantially under spatial cross-validation.

This suggests that part of its apparent advantage may have resulted from spatial dependence between nearby observations rather than stronger generalisation to geographically independent areas.

## Spatial Point-Pattern Diagnostics

Ripley’s K function was used to determine whether the species records followed complete spatial randomness.

The observed K function exceeded the expected value under complete spatial randomness across a broad range of distances, indicating strong spatial clustering.

This finding supports the use of spatial cross-validation because randomly divided training and test samples cannot be assumed to be spatially independent.

## Results

### Environmental Response Curves

The response curves showed that:

* predicted badger occurrence increased with broadleaf woodland cover;
* predicted occurrence decreased as elevation increased;
* urban land cover had a weaker positive relationship with occurrence;
* uncertainty increased at the upper end of the urban land-cover gradient.

![Partial response curves](figures/response-curves.png)

### Habitat Suitability Maps

Both models produced similar broad-scale suitability patterns but differed in spatial texture.

The GLM generated a smoother suitability surface, while Random Forest produced more heterogeneous and patchy predictions.

Higher suitability was primarily concentrated in lower-elevation areas and near landscape corridors associated with woodland and human-modified environments.

![Predicted habitat suitability maps](figures/suitability-maps.png)

### Validation and Spatial Clustering

The comparison between non-spatial and spatial cross-validation demonstrates how conventional validation can overestimate model performance when observations are spatially clustered.

![AUC comparison and Ripley's K diagnostic](figures/model-evaluation.png)

## Repository Structure

```text
badger-species-distribution-modelling/
│
├── README.md
├── scripts/
│   ├── 01_data_preparation.R
│   ├── 02_scale_selection.R
│   ├── 03_model_training.R
│   ├── 04_model_validation.R
│   └── 05_visualisation.R
│
├── data/
│   └── README.md
│
├── figures/
│   ├── response-curves.png
│   ├── suitability-maps.png
│   └── model-evaluation.png
│
├── results/
│   └── model-performance.csv
│
├── report/
│   └── project-report.pdf
│
└── sessionInfo.txt
```

## Tools and Methods

* R
* GIS and raster data processing
* Generalised Linear Models
* Random Forest
* Multiscale buffer analysis
* Pseudo-absence generation
* Non-spatial cross-validation
* Spatial cross-validation
* ROC-AUC evaluation
* Ripley’s K point-pattern analysis
* Spatial prediction and visualisation

## Running the Project

1. Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/badger-species-distribution-modelling.git
cd badger-species-distribution-modelling
```

2. Open the project in RStudio.

3. Place the required datasets in the `data/` directory according to the instructions in `data/README.md`.

4. Run the scripts in numerical order:

```text
01_data_preparation.R
02_scale_selection.R
03_model_training.R
04_model_validation.R
05_visualisation.R
```

5. Model outputs will be saved in `results/`, while maps and charts will be saved in `figures/`.

## Data Availability

The repository does not include raw datasets that are restricted by licensing, file size, or species-location sensitivity.

The `data/README.md` file should document:

* dataset names;
* original providers;
* download links;
* coordinate reference systems;
* required file formats;
* preprocessing instructions.

## Limitations

This analysis has several limitations:

* background points were used instead of confirmed species absences;
* only three environmental predictors were included;
* spatial clustering remained present in the occurrence records;
* additional habitat, climate, soil, and landscape-connectivity variables could improve ecological realism;
* the selected spatial scales may be specific to the study area and available data.

## Skills Demonstrated

This project demonstrates the ability to:

* clean and integrate spatial datasets;
* construct predictors from vector and raster data;
* perform multiscale spatial feature engineering;
* compare statistical and machine-learning models;
* identify potential spatial data leakage;
* design geographically appropriate validation strategies;
* interpret model response curves;
* communicate spatial modelling results through maps and visualisations.

## Conclusion

Random Forest produced the strongest apparent predictive performance under conventional validation and captured more complex spatial variation. However, its performance declined substantially when evaluated using spatially separated test data.

GLM produced smoother and more interpretable predictions and remained comparatively stable under spatial cross-validation.

The results show that model selection should not be based only on conventional accuracy metrics. For spatial datasets, the structure of the validation process can be just as important as the choice of algorithm.
