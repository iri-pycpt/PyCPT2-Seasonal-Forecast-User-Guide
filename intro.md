# Introduction

<!-- 
```{image} img/intro.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```
 -->

Welcome to __PyCPT Version 2 for Seasonal Climate Forecasts!__ The goal of this tutorial is to learn how to
 configure and run PyCPT, Version 2, in order to make, calibrate and verify
multi-model seasonal forecasts of precipitation based on the [NOAA North American Multi-Model Ensemble (NMME) ](https://www.cpc.ncep.noaa.gov/products/NMME/)
 and European [Copernicus Climate Change Service (C3S) ](https://climate.copernicus.eu) databases. PyCPT helps to implement the [NextGen approach](nextgen-section-label) using the [Climate Predictability Tool (CPT)](cpt-section-label).

 
The PyCPT set-up and results are illustrated for a West African precipitation forecast example for Jun-Sep 2022, initialized in May 2022. The example uses a two-model multimodel ensemble of the NCEP CFSv2 and ECMWF SEAS5 models.

```{admonition} PyCPT 2 is:
- a rewrite of PyCPT based on [Xarray](https://docs.xarray.dev/en/stable/) and a [new set of Python libraries](https://github.com/iri-pycpt) that provide a Python interface to the CPT fortran-based package, and facilitate access to GCM forecasts/hindcasts and observational gridded datasets via [IRI Data Library](http://iridl.ldeo.columbia.edu)
- easily customizable  to individual forecaster needs using Jupyter Notebooks, by importing the four new Python modules, available via the ```iri-nextgen``` conda channel
- designed to be run natively using Python on linux, Windows and Mac (feature still in progress for Windows)
- designed to facilitiate the use of observational gridded datasets, such as [ENACTS,](https://iri.columbia.edu/resources/enacts/) from the user’s laptop in NetCDF or CPTv10.tsv format. (feature still in progress)
```

 __Who is this guide for?__
 
Basic familiarity with Python, seasonal climate forecasting, and IRI's [Climate Predictability Tool (CPT)](http://iri.columbia.edu/tools/) is assumed.

 __Acknowledgments__

The Guide to PyCPT2 for Seasonal Climate Forecasts and its associated teaching and learning materials represents a collaborative effort, made possible by the IRI’s dedicated team of climate, sectoral, and technical experts in collaboration with the Accelerating Impacts of CGIAR Climate Research for Africa (AICCRA) project.

[Accelerating Impacts of CGIAR Climate Research for Africa (AICCRA)](https://aiccra.cgiar.org) is a project that helps deliver a climate-smart African future driven by science and innovation in agriculture. It is led by the Alliance of Bioversity International and CIAT and supported by a grant from the International Development Association (IDA) of the World Bank.                             

```{image} img/AICCRA_Logo.png
:alt: fishy
:class: bg-primary
:width: 800px
:align: left
```

%## Useful resources

---