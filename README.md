# UK Weather Temperature Prediction
A machine learning project predicting 2-meter temperature using high-resolution UK meteorological data from ERA5 (European Centre for Medium-Range Weather Forecasts). Developed for Machine Learning module.

## Goal
Develop accurate temperature prediction models to improve weather forecasting for UK-based meteorological agencies using spatial and temporal patterns in atmospheric data.

## Dataset

**Source:** ERA5 reanalysis dataset (ECMWF)

**Size:** 13,288,920 observations in year 2018

**Features:** Meteorological variables including precipitation, wind components, surface pressure, cloud cover, and temporal/spatial coordinates

**Target:** Temperature at 2 meters above surface (t2m) in Kelvin

## Feature Engineering

### Temporal Encoding
Converted cyclical time features to sin/cos representations to capture seasonal and daily patterns - recognizing that December 31st is close to January 1st in terms of weather patterns.

### Wind Features
Engineered wind speed and direction from u100/v100 components to capture temperature effects from different air masses (Arctic air from North/East, warmer air from South/West).

### Spatial Features
Created spatial clustering with historic local wind speed and precipitation features, though these proved less significant than initially expected.

### Feature Selection
Dropped redundant features (u10, v10) that highly correlated with u100, v100. Ordinally encoded precipitation types by formation temperature requirements.

## Models Evaluated

### K-Nearest Neighbors (KNN) + PCA  
- **Validation RMSE:** 0.5139
- **Test RMSE:** 0.5140
- Applied PCA dimensionality reduction (19 → 16 features, 95% variance retained)
- Leverages spatial-temporal similarity in weather patterns

### Random Forest Regressor
- **Validation RMSE:** 0.5228
- Strong ensemble performance with complex non-linear relationships
- Feature importance revealed seasonal variations (day sin/cos) as most critical
- Engineered historic features showed minimal importance

### XGBoost
- **Training CV RMSE:** 3.4 | **Validation RMSE:** 1.6
- Not selected due to data leakage from interleaved train/validation split
- Artificially low validation performance from temporal adjacency in high-resolution data

### Elastic Net
- **L1 ratio:** 0.1 | **Alpha:** 0.01
- Lower performance due to model simplicity relative to data complexity
- Ridge-weighted regularization suggested many features are significant

### Neural Networks
- Three-layer net performed best but didn't surpass KNN
- Early stopping too aggressive - prevented sufficient pattern learning
- Would benefit from dropout and weight decay techniques

## Key Findings

**Most Important Predictors:**
1. Seasonal timing (day/month sin/cos features)
2. Latitude (sun exposure)
3. Time of day (diurnal temperature cycle)
4. Surface pressure (directly proportional to temperature)
5. Cloud cover (insulation effect)
6. Precipitation type (indicator of atmospheric temperature)

**Data Characteristics:**
- Dense temporal/spatial sampling causes apparent overfitting (training RMSE ≈ 0.0000)
- Validation set design with interleaved samples creates temporal adjacency issues
- Full annual cycle required in training prevents clean train/test temporal separation

## Best Model Performance

**KNN with PCA achieved RMSE of 0.5140 K on test data**

This represents an average prediction error of approximately 0.5°C - highly accurate for meteorological forecasting given the complexity and variability of weather patterns.


## License

MIT License - Academic portfolio project
