Probabilistic Forecasts and Forecast Verification
====

How can these regression-based probabilistic forecasts be verified? While the regression model errors are computed out of sample (they are cross-validated), their variance is computed across all the hindcast years. 

```{image} img/hindcastErrors.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```

We cannot strictly evaluate them on independent data because all the hindcast years have already been used to compute them. For any particular year, the error variance includes a contribution from that year, so the probabilistic skill will be overly optimistic. For this reason, for probabilistic forecast verification, CPT uses a retroactive approach that mimics real-the forecasting.

In CPT, Verification refers to the assessment of the performance of this set of forecasts, using probabilistic metrics like RPSS, GROC.

<!-- 

If cross-validation is used as a model selection procedure we need a separate procedure to to estimate its predictive skill.

```{image} img/retroactive.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```
 -->

In PyCPT, verification is performed on the entire set of hindcast years for expediency.



