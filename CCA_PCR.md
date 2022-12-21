CCA or PCR?
====

CCA and PCR are varieties of multiple linear regression (MLR) that overcome the difficulties of MLR with high dimensional datasets where the number of spatial grids exceeds the number of training samples. The number of training samples for the NMME hindcasts, which begin in 1982 (30+ years) and in C3S which are currently in most cases limited to 1993-2016 (24 years).
[[Need to explain why we want multiple predictors, and which we can just use OLS.]]

CCA diagonalizes the regression matrix by transforming the variables X & Y, such that the elements become cc coefficients
Lin Alg: forecast = cc.<CCA_X_loadings>.<X_forexast>.<CCA_Y_loadings>
Lin Alg: forecast(x,y) = cc * <CCA_X_loadings>.<X_forecast> * Y_CCA_loadings(x,y)
Models with high cc coefficients and interpretable mappings are preferable

CCA Visualization of the regression matrix.

For PCR, the X EOFs provide some physical interpretation of the predictors 
EOF patterns are constrained by orthogonality so physical interpretation is more difficult than for the CCA modes


### Advantages of CCA
- capitalizes on the larger spatial scales inherent in seasonal climate predictability, which result from the oceans’ role in providing the main “sources” of prediction skill on seasonal time scales, by identifying spatially correlated “patterns” of precipitation/tempdrature that can be predicted
- by identifying predictable spatial patterns in the observations, it is robust to local-scale observational error (noise)
allows physical interoperation of the regression between GCM predictions and in-country climate variability 
- produces forecasts that vary smoothly are are consistent from grid point to grid point

Advantages of PCR
- Does not truncate the predictand, enabling for predictand to be potentially predicted.




