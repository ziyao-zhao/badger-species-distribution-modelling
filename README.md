Spatial Species Distribution Modelling: GLM vs Random Forest

Comparing Generalised Linear Models and Random Forest for predicting European badger (Meles meles) habitat suitability, with particular attention to multiscale environmental effects and spatially robust model validation.

Project Overview

Species Distribution Models (SDMs) are widely used to examine relationships between species occurrence and environmental conditions. However, two methodological issues can strongly affect model results:

environmental variables may influence species at different spatial scales;

spatial clustering can cause conventional cross-validation to overestimate predictive performance.

This project compares a Generalised Linear Model (GLM) and a Random Forest (RF) model for predicting the distribution of European badgers (Meles meles).

The analysis addresses three questions:

At what spatial scales are broadleaf woodland and urban land cover most strongly associated with badger occurrence?

How does predicted badger occurrence respond to woodland cover, urban land cover, and elevation?

How does model performance change when spatial dependence is considered during validation?

Main Findings

Broadleaf woodland showed its strongest relationship with badger occurrence at approximately 900 m.

Urban land cover showed its strongest relationship at approximately 1,500 m.

Predicted occurrence probability increased with broadleaf woodland cover.

Elevation was negatively associated with predicted occurrence probability.

Urban land cover showed a weaker positive relationship with occurrence, with greater uncertainty at higher values.

Random Forest achieved the highest AUC under non-spatial cross-validation.

Both models performed worse under spatial cross-validation.

Random Forest showed a larger decrease in performance than GLM after spatial dependence was controlled.

Ripley’s K analysis indicated strong spatial clustering in the species records.

GLM provided more stable spatial generalisation and clearer ecological interpretation, while Random Forest captured more complex local variation.

Data Preparation

Before modelling, species occurrence records were:

filtered to remove missing coordinates;

filtered to remove records with high coordinate uncertainty;

clipped to the study area;

transformed into a projected coordinate reference system;

combined with 2,000 randomly generated background points used as pseudo-absence observations.

Three environmental predictors were included:

Predictor

Description

Processing

Broadleaf woodland

Proportion of broadleaf woodland surrounding each sample point

Tested across multiple buffer distances

Urban land cover

Combined urban and suburban land-cover classes

Tested across multiple buffer distances

Elevation

Elevation extracted from a Digital Elevation Model

Used at its original raster resolution

Multiscale Feature Engineering

Environmental relationships may operate at different spatial scales. To identify suitable characteristic scales, buffers of different radii were created around each sample point.

Binomial models were fitted at each buffer distance, and model log-likelihood was used to identify the strongest relationship with badger occurrence.

Environmental variable

Selected scale

Broadleaf woodland

Approximately 900 m

Urban land cover

Approximately 1,500 m

This approach avoids choosing buffer distances arbitrarily and allows each predictor to represent the scale at which the ecological relationship is strongest.

Modelling Methods

Generalised Linear Model

The GLM was implemented as a binary logistic regression model with a logit link function.

Its main strengths are:

clear interpretation of predictor effects;

smooth predicted suitability surfaces;

transparent model structure;

suitability for ecological inference.

Random Forest

Random Forest is an ensemble tree-based classifier that combines predictions from multiple decision trees.

Its main strengths are:

ability to capture nonlinear relationships;

ability to model complex interactions;

flexibility with heterogeneous spatial patterns;

strong apparent predictive performance.

Model Evaluation

Model performance was evaluated using the Area Under the Receiver Operating Characteristic Curve (AUC).

Two validation strategies were compared:

Non-Spatial Cross-Validation

Samples were randomly divided between training and test sets.

Because neighbouring observations may be placed in both sets, this method can overestimate model performance when the data are spatially clustered.

Spatial Cross-Validation

Training and test samples were separated geographically.

This provides a more conservative estimate of how well a model may generalise to new locations.

Results

1. Model Performance Comparison

Random Forest achieved the highest AUC under non-spatial validation. However, its performance declined more strongly under spatial cross-validation than the GLM.

Approximate values visible in the evaluation figure are:

Model

Non-spatial AUC

Spatial AUC

GLM

~0.79

~0.75

Random Forest

~0.87

~0.72

These values should be replaced with the exact outputs from the analysis script if available.



The difference between conventional and spatial validation suggests that part of the Random Forest model’s apparent advantage may result from spatial dependence between nearby observations.

2. Broadleaf Woodland Response

Predicted occurrence probability increased as the proportion of broadleaf woodland increased.

The uncertainty interval widened at higher woodland-cover values, indicating fewer observations or greater variability at the upper end of the predictor range.



3. Urban Land-Cover Response

Urban land cover showed a weaker positive relationship with predicted badger occurrence.

The uncertainty interval widened considerably at higher urban-cover values, so the strength of this relationship should be interpreted cautiously.



4. GLM Predicted Suitability

The GLM produced a relatively smooth habitat-suitability surface.

Higher suitability was concentrated mainly in lower-elevation areas and along landscape corridors associated with woodland and human-modified environments.



5. Random Forest Predicted Suitability

The Random Forest prediction was more heterogeneous and spatially patchy than the GLM output.

This reflects the greater flexibility of tree-based models and their ability to capture complex local variation.



6. Spatial Clustering Diagnosis

Ripley’s K function was used to test whether the occurrence records followed complete spatial randomness.

The observed K function was substantially above the theoretical expectation across a broad range of distances, indicating strong spatial clustering.



This result supports the use of spatial cross-validation because nearby observations cannot be assumed to be statistically independent.

Interpretation

The comparison shows that the model with the highest conventional validation score is not necessarily the model that generalises best across space.

Random Forest captured more complex spatial structure and achieved the highest non-spatial AUC. However, its larger decline under spatial cross-validation suggests that it relied more heavily on local spatial dependence.

GLM produced smoother and more interpretable predictions and remained comparatively stable after geographic separation of the training and test data.

For this study:

GLM is more suitable for ecological interpretation and spatially robust inference.

Random Forest remains useful as a complementary predictive model for identifying complex local patterns.

Why This Project Is Relevant Beyond Ecology

The methodological lessons are also applicable to:

urban analytics;

retail site selection;

mobility and transport analysis;

property-price prediction;

environmental risk modelling;

geographically targeted marketing;

other forms of spatial machine learning.

In each of these settings, random train-test splitting can produce overly optimistic results when nearby observations share similar characteristics.

Tools and Skills Demonstrated

R

GIS and raster data processing

Spatial data cleaning

Pseudo-absence generation

Multiscale buffer analysis

Feature engineering

Generalised Linear Models

Random Forest

Non-spatial cross-validation

Spatial cross-validation

ROC-AUC evaluation

Ripley’s K point-pattern analysis

Spatial prediction and visualisation

Model interpretation

Suggested Repository Structure

badger-species-distribution-modelling/
│
├── README.md
├── analysis.R
├── AUCcomparison.png
├── broadleaf-response.png
├── GLM-predicted.png
├── Kcsr.png
├── RF-predited.png
├── urban_resonse.png
│
├── report/
│   └── project-report.pdf
│
└── data/
    └── README.md

Data Availability

Raw data are not included where redistribution is restricted by licensing, file size, or species-location sensitivity.

The project uses:

badger occurrence records;

land-cover data;

a Digital Elevation Model;

generated background points.

The data/README.md file should document the original data providers, file formats, coordinate reference systems, and preprocessing steps.

Limitations

Background points were used instead of confirmed absence records.

Only three environmental predictors were included.

The occurrence records showed substantial spatial clustering.

Additional habitat, climate, soil, or landscape-connectivity variables could improve ecological realism.

The selected spatial scales may be specific to the study area and available datasets.

Conclusion

Random Forest produced the strongest apparent predictive performance under conventional validation, but its performance declined substantially when evaluated using geographically separated test data.

GLM produced smoother, more interpretable predictions and remained more stable under spatial cross-validation.

The project demonstrates that for spatial datasets, model validation design can be just as important as model choice.
