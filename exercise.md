PyCPT Exercise
=======================

```{note} **Still Under Construction!**
```

In this exercise, you will first you will run the demo example for West Africa with the xxx model, 
using CCA MOS.

You will then do the following:

1. Clone the [bitbucket repository](https://bitbucket.org/py-iri/iri-pycpt/src/master/) and 
configure `PyCPT_s2sv1.9.ipynb` for your own choice of region/season.
1. Repeat using a different SubX model or the ECMWF model. 
1. Try using PCR instead of CCA
1. Try using a 1-month training season (instead of 3 months)
1. (Try an MME)

## Questions

1. What needs to be modified in order to use a different SubX model?
What aspects of the hindcast configuration need to be considered?

2. How will using a 1-month instead of 3 months impact on the MOS configuration? 
Hint: consider the length of initial training period `lit` and its update interval `liti`