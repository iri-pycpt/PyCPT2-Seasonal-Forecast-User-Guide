(cpt-section-label)=
# The Climate Predictability Tool (CPT)

```{image} img/CPT.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```

IRI created the [Climate Predictability Tool (CPT)](https://iri.columbia.edu/our-expertise/climate/tools/cpt/)---at the heart of PyCPT---to facilitate the development of best-practices in statistical/empirical seasonal climate forecasting for NMHSs using multiple linear regression. These were typically based on slowly-evolving SST patterns associated with ENSO as predictors. CPT uses the principle of cross-validation to build and validate multiple linear regression models, and to select the best model.

Because seasonal climate predictability in the tropics involves spatially-correlated patterns of precipitation anomalies associated with patterns of ENSO SST variations, CPT provides two types of __pattern regression:__ Canonical Correlational Analysis (CCA) and Principal Components Regression (PCR).

- PCR considers a univariate predictand (at a local gridpoint), together with a truncated set of the Principal Components (PCs) of the predictor field; it enables a data compression of a large field of predictor grid points that is optimal in variance, with statistically independent predictor time series. 
- CCA compresses both a high-dimensional predictor and a high-dimensional predictand field into a few components of each (the CCA modes), such that the timeseries of these components are maximally correlated between predictor and predictand fields. The CCA modes consist of linear combinations of the leading EOFs of the predictor and predictand fields.

## 1. Regression model validation (and selection)

```{image} img/validationSelection.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```

The CPT model validation and selection workflow is as follows:
1. Test the performance of the regression model using independent data
2. This is done using cross-validation in CPT
3. Various skill scores are used to assess the validity of the model 
4. Note that these are all deterministic skill scores because the regression model is deterministic
5. Validation enables the best model to be selected.

```{warning} "Trying too hard" can lead to overestimating the skill just by chance; this is called selection bias.
```

## 2. Cross-Validation

Cross-validation consists of training a new CCA/PCR model for each hindcast year, omitting that year from the training data set. This is called "leave 1 out" cross-validation and enables the resulting model to be properly tested against the observation year that was withheld from the training. 

```{image} img/leave1outXV.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```

Leaving out more years reduces the negative bias in skill that arises with leave 1 out cross-validation, although leaving too many years out causes large sampling errors. Good compromises are to leave 3 or 5 years out. 

```{image} img/leave5outXV.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```

##
## 3. Probabilistic forecasts 

Once a suitable PCR or CCA regression model has been established, CPT uses it to create probabilistic forecasts. However, the regression model is deterministic. Probabilistic forecasts are created by assuming a parametric form for the forecast PDF (for example a normal bell-curve). The regression model provides the mean of the forecast PDF; its variance (spread) is determined from the errors of the regression model’s cross-validated hindcasts.  Setting the forecast spread to equal the model’s error-variance ensures that the resulting probabilistic forecasts are well calibrated - i.e., that their confidence reflects their historical accuracy.

(add equation for the forecast pdf)

```{image} img/hindcastErrors.png
:alt: fishy
:class: bg-primary
:width: 400px
:align: left
```

```{admonition} Slide presentation
[See slides on the CPT approach](https://www.dropbox.com/s/yym7pb89842dsfx/CPT_concepts_Lome.pdf?dl=0)
from the AICCRA West Africa Regional Improved NextGen Seasonal Forecasting (PyCPT 2) [Training,](https://wiki.iri.columbia.edu/index.php?n=NextGen.AICCRA-Lome-Oct2022) October 10-19, 2022, Lomé, Togo.]
```




<!-- 
## Some CPT usage recommendations

(I think the text below is pitched at the rigt level but needs improving)

### Overview
__Analysis options:__ There are five analysis options in CPT: canonical correlational analysis (CCA), principal component regression (PCR), multiple linear regression (MLR), GCM validation, probabilistic forecast verification (PFV). 

Beginner mode options: In beginner mode, these options are reduced to forecast (CPT automatically chooses CCA for this unless otherwise indicated), GCM validation and forecast verification. 
__Purpose of different analysis options:__ The purpose of GCM validation is to compare GCM output data (X data) to observations (Y data) during a historical time period – outputs include estimates of GCM bias. It should be noted that one can make forecasts using GCM output as well, but the GCM validation functionality is designed to evaluate GCM outputs on a grid to grid or grid to station basis. The purpose of PFV is to compare a probabilistic forecast input (X data) with observations (Y data). The purpose of MLR, PCR and CCA is to make forecasts on the basis of an overlapping training window between the predictor and predictand. 

__Selecting the best prediction tool:__ If both your predictor and predictand data are “index data” (a time series from an index or station rather than gridded data) and you wish to make a forecast, you should select MLR (this method will work for a single index predictor or a predictor file with multiple indices/stations but will not work with gridded data). If either your predictor or your predictand is an index file and the other is gridded, or if they are both gridded, you may choose PCR. If both are gridded arrays, generally, CCA is preferable to PCR. 

__Selection Guidelines:__ The predictand file should be over your region of interest and can be quite small (eg. Rwanda). The predictor domain should generally be larger (eg. SSTs over a major tropical ocean basin, GCM rainfall or wind over a larger region eg. E. Africa) 
Select predictor domains and variables that reflect well-known physical climate mechanisms (eg. ENSO, IOD).

Begin your analysis with about 8 to 10 modes of consideration for X and Y predictors and CCA  modes.
Use a relatively long training period and make a forecast for the current year. One can also make forecasts for several recent years, and separately compare with observations. But in the process of running a forecast, one can also examine how the hindcasts of recent years compared to the observations. 

### How to judge whether a CPT forecast is any good
__Goodness index:__ A goodness index above zero preferred, above 0.2 is good for most applications and a goodness index between 0.1 and 0.2 is OK for some applications (keep in mind that if your training window is very short, you need a higher goodness index to justify the forecast). If the goodness index is negative, this means the forecast is worse than simply forecasting climatology repeatedly, so it should probably be discarded and you should probably try a different predictor or different predictor parameters).

__Test for overfitting:__ If the CPT analysis selects 8 or more modes as optimal…in Y as well as X or even just in one…if so, some skepticism may be warranted.

__Pearson and/or Spearman and/or other skill map correlations:__                                   (from the Tools->Validation dropdown); The skill maps should be positive or neutral almost everywhere (although a few points of negative correlation on a high resolution grid are not necessarily a problem if the vast majority of the region has a strong positive correlation).

__Mode maps for the CCA option:__ The leading X predictor mode should show some sort of physical relationship (sometimes it is strong trend, sometimes it is a dipole if SST and sometimes it is less clear), Y mode should be a deep relatively homogenous color (either blue or red – especially if it is a small spatial domain). 
When interpreting the mode maps, keep sign conventions in mind…(if one model shows mostly blue (negative) loadings for the predictand and a negative Indian Ocean Dipole phase and another model shows red (positive) loadings for the predictand and a positive Indian Ocean Dipole phase, the two models are essentially telling the same qualitative story). 

-Climatology (Tools -> climatological maps dropdown) – verify that the climatology is what it is supposed to be. 
__Scree plots__ There are two potential issues; if the leading mode explains very little variance and if the last modes in the optimization explain exceptionally little variance. By construct, the percentage of variance explained will decrease with each mode and the sum of the percentage of variance explained of all the modes must equal 100%. However, if the last few “optimal” modes explain very little variance, the forecast model is likely overfit.  If there is very little variance explained by the leading mode (especially if the domain is smaller), this may be indicative of a weaker forecast (although not necessarily). 

__Verify hindcasts against past seasons’ observational data__

-Analyze the error of past forecasts with observational data: if the user has a network of station data readily available after the season has passed, it is beneficial to compare the actual observed rainfall at multiple stations to the forecast rainfall and to calculate how close the forecast was to the observations. For locations with large errors, it may also be advisable to look at the forecast rainfall for nearby locations…a forecast model may get the general anomaly correct, but may have a slight spatial bias in the placement of certain meteorological features. 
 -->
