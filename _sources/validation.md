# Regression Model Validation (and Selection)

```{image} img/validationSelection.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```


- Test the performance of the regression model using independent data
- This is done using cross-validation in CPT
- Various skill scores are used to assess the validity of the model 
- Note that these are all deterministic skill scores because the regression model is deterministic
- Validation enables the best model to be selected.
- Warning! Trying too hard can lead to overestimating the skill just by chance; this is called selection bias

## Cross-Validation

```{image} img/leave1outXV.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```

Leaving out more years reduces the negative bias in skill that arises with leave-one-out, although too many years out causes large sampling errors. Good compromises are to leave 3 or 5 years out. 

```{image} img/leave5outXV.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```



