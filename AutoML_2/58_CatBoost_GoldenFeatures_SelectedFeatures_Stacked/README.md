# Summary of 58_CatBoost_GoldenFeatures_SelectedFeatures_Stacked

[<< Go back](../README.md)


## CatBoost
- **n_jobs**: -1
- **learning_rate**: 0.05
- **depth**: 7
- **rsm**: 1.0
- **loss_function**: MAPE
- **eval_metric**: MAE
- **explain_level**: 0

## Validation
 - **validation_type**: kfold
 - **k_folds**: 10
 - **shuffle**: True

## Optimized metric
mae

## Training time

211.0 seconds

### Metric details:
| Metric   |           Score |
|:---------|----------------:|
| MAE      |    45.4719      |
| MSE      | 25759.4         |
| RMSE     |   160.497       |
| R2       |     0.117097    |
| MAPE     |     3.81365e+12 |



## Learning curves
![Learning curves](learning_curves.png)
## True vs Predicted

![True vs Predicted](true_vs_predicted.png)


## Predicted vs Residuals

![Predicted vs Residuals](predicted_vs_residuals.png)



[<< Go back](../README.md)
