# Summary of 29_CatBoost_GoldenFeatures_SelectedFeatures_BoostOnErrors

[<< Go back](../README.md)


## CatBoost
- **n_jobs**: -1
- **learning_rate**: 0.2
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

674.4 seconds

### Metric details:
| Metric   |           Score |
|:---------|----------------:|
| MAE      |    45.4274      |
| MSE      | 25403.4         |
| RMSE     |   159.384       |
| R2       |     0.129302    |
| MAPE     |     6.44661e+09 |



## Learning curves
![Learning curves](learning_curves.png)
## True vs Predicted

![True vs Predicted](true_vs_predicted.png)


## Predicted vs Residuals

![Predicted vs Residuals](predicted_vs_residuals.png)



[<< Go back](../README.md)
