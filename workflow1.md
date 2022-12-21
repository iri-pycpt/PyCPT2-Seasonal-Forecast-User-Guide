# Installing and configuring

PyCPT 2 can be installed on Linux and Mac computers by following [these instructions](https://iri-pycpt.github.io/installation/). 

The full software documentation can be found here: [https://iri-pycpt.github.io](https://iri-pycpt.github.io).

```{note} 
Although PyCPT 2 is designed to run natively on Windows computers, this option is not yet avaiable and Windows users should either install Windows Subsystem for Linux (WSL), or use a virtual machine such as Virtual Box. Please see these instructions for [installtion of PyCPT 2 on WSL](https://www.dropbox.com/s/do9sx0dz5pqteyp/PYCPT_WSL_v2.pptm?dl=0). 
```

The PyCPT 2 workflow consists of the following steps, which we will illustrate using a West Africa precipitation example:

- name a case directory for your results
- choose the desired MOS technique (CCA or PCR)
- select the GCM predictor and observational predictand datasets from IRI Data Library
- set the parameters for the analysis (predictor & predictand spatial domains, forecast initialization month and  target season, and hindcast period for regression model fitting
- for each GCM hindcast set, fit the regression model over the common hindcast period
- plot cross-validated deterministic skill scores for each GCM for CPT model validation
- plot the CCA and/or EOF modes and check for physical consistency 
- if needed, adjust the predictor domain, the maximum number of EOF/CCA modes allowed, and other parameters including any transformation the predictand data, dry mask etc. 
- select the subset of GCMs to include in the multi-model combination (MME)
- make MME hindcasts and examine their deterministic and probabilistic skill
- make the deterministic forecast in the desired format (e.g. forecast anomalies), and the probabilistic forecast in tercile and flexible formats.


## West Africa precipitation example 

In this example, we are going to predict June-September total precipitation over West Africa, from GCM forecast models initialized in the previous May. We will use __CCA__ to MOS-correct the predicted precipitation fields from two GCMs (`NOAA CFSv2` and `ECMWF SEAS5`) over the West African region, using `CHIRPS` data for model selection and validation.  

We will name our case directory `caseDir = "pycpt_WAfricaJJAS_startMay"`

###### 1. Choose the MOS technique and predictor and predictand datasets
```python
MOS = 'CCA' # must be one of 'CCA', 'PCR', or "None"
predictor_names = ['CFSv2.PRCP','SEAS5.PRCP']
predictand_name = 'UCSB.PRCP'
```

The predictor and predictand names each indicate the dataset.variable. Type `dl.observations.keys()` to see all options for predictand, and `dl.hindcasts.keys()` to see all options for predictors. UCSB.PRCP denotes the CHIRPS dataset (UCSB=University of California, Santa Barbara).

```{important} 
1. Some sets of hindcasts have been superceded by new model versions. This [table](https://www.dropbox.com/s/q4xt92632r8cft5/NMME_C3S_models_table.pdf?dl=0) contains an inventory of which GCM forecasts are currently available in real time in IRI Data Library, along with the associated hindcast datasets.

2. Make sure your `first_year` & `final_year` of the hindcast training period are compatible with your selections for your predictors and predictands. 
```

###### 2. Specify the forecast dates and geographical domains

__a) Forecast initialization date and hindcast period__   

The following parameters specify the __initialization date__ of the model forecasts `fdate`, together with the `first_year` & `final_year`  of the __hindcast period.__ 

```python
   # 'fdate':
   #   the initialization date of the model forecasts / hindcasts
   #   this field is defined by a python datetime.datetime object
   #   for example: dt.datetime(2022, 5, 1) # YYYY, MM, DD as integers
   #   The year field is only used for forecasts, otherwise ignored
   #   The day field is only used in subseasonal forecasts, otherwise ignored
   #   The month field is an integer representing a month - ie, May=5
  'fdate': dt.datetime(2022, 5, 1),  
    
   # 'first_year':
   #   the first year of hindcasts you want. **NOT ALL MODELS HAVE ALL YEARS**
   #   double check that your model has hindcast data for all years in [first_year, final_year]
   #   This field is defined by a python integer representing a year, ie: 1993
  'first_year': 1982, 
    
   # 'final_year':
   #   the final year of hindcasts you want. **NOT ALL MODELS HAVE ALL YEARS**
   #   double check that your model has hindcast data for all years in [first_year, final_year]
   #   This field is defined by a python integer representing a year, ie: 2016
  'final_year': 2016, 
```

Here we have chosen 1982-2016 as the training period, for which both the `CFSv2` and `SEAS5` models have hindcasts available (the `SEAS5` hindcasts stop in 2016). The [CHIRPS dataset](http://iridl.ldeo.columbia.edu/SOURCES/.UCSB/.CHIRPS/.v2p0/.daily-improved/.global/.0p25/) (UCSB.PRCP) starts in 1981 and runs to the present.

`fdate` is the initialization date of the model forecast/hindcasts. Here PyCPT will make a forecast for Jun-Sep 2022, initialized on May 1, 2022, calibrated using hindcasts initialized in May of each year 1982-2016.

```{admonition} What are hindcasts?
Hindcasts are forecasts for past years, made using the current version of the GCM forecast model.
```

__b) Forecast lead time and target season__

The following parameters specify the __lead time__ of the forecasts/hindcasts. `lead_low` is the number of months from the first of the initialization month to the center of the __first__ month included in the target period. `lead_high` isa similarly the number of months from the first of the initialization month to the center of the __last__ month included in the target period, given by `target`.

For the Jun-Sep target with an `fdate` of May 1, we need to set `lead_low=1.5` and `lead_low=4.5`. 

```python     
   # 'lead_low': 
   #   the number of months from the first of the initialization month to the center of 
   #   the first month included in the target period. Always an integer + 0.5. 
   #   this field is defined by a python floating point number 
   #   for example  a lead-1 forecast would use lead_low=1.5, if you want init=may, target=Jun-..
  'lead_low': 1.5,
    
   # 'lead_high': 
   #   the number of months from the first of the initialization month to the center of 
   #   the last month included in the target period. Always an integer + 0.5. 
   #   this field is defined by a python floating point number 
   #   for example  a forecast initialized in may, whose target period ended in Aug, 
   #   would use lead_high=3.5
  'lead_high': 4.5, 
    
   # 'target': 
   #   Mmm-Mmm indicating the months included in the target period of the forecast. 
   #   this field is defined by a python string, with two three-letter month name abbreviations 
   #   whose first letters are capitalized, and all other letters are lowercase
   #   and who are separated by a dash character. 
   #   for example, if you wanted a JJA target period, you would use 'Jun-Aug'
  'target': 'Jun-Sep',
```

__c) Geographical domains of the predictor and predictand__

Finally, the following parameters give the geographical bounding boxes of the climate model data (`predictor_extent`) and observation data (`predictand_extent`).  

```python    
   # 'predictor_extent':
   #   The geographic bounding box of the climate model data you want to download
   #   This field is defined by a python dictionary with the keys "north", "south",
   #   "east", and "west", each of which maps to a python integer representing the 
   #   edge of a bounding box. i.e., "north" will be the northernmost boundary,
   #   "south" the southernmost boundary. Example: {"north": 90, "south": 90, "east": 0, "west": 180}
  'predictor_extent': {
    'east': 20,
    'west': -20, 
    'north': 20, 
    'south': 0
  }, 
   # 'predictand_extent':
   #   The geographic bounding box of the observation data you want to download
   #   This field is defined by a python dictionary with the keys "north", "south",
   #   "east", and "west", each of which maps to a python integer representing the 
   #   edge of a bounding box. i.e., "north" will be the northernmost boundary,
   #   "south" the southernmost boundary. Example: {"north": 90, "south": 90, "east": 0, "west": 180}
  'predictand_extent': {
    'east': 10,
    'west': -18, 
    'north': 18, 
    'south': 3
  },
```

```{admonition} How large should the predictor and predictands geographic domains be?
The predictand domain should cover your region of interest and can be quite small (e.g., Togo). The predictor domain should generally be larger to represent GCM seasonal rainfall variations over a larger region (e.g., West Africa). Select predictor domains and variables that reflect well-known physical climate mechanisms (e.g., the West African monsoon).
```

###### 3. Set the CPT analysis parameters

The following parameters control the behavior of CPT. 

```python
 cpt_args = { 
    'transform_predictand': None,  # transformation to apply to the predictand dataset - None, 'Empirical', 'Gamma'
    'tailoring': 'Anomaly',  # tailoring None, Anomaly, StdAnomaly, or SPI (SPI only available on Gamma)
    'cca_modes': (1,3), # minimum and maximum of allowed CCA modes 
    'x_eof_modes': (1,8), # minimum and maximum of allowed X Principal Componenets 
    'y_eof_modes': (1,6), # minimum and maximum of allowed Y Principal Components 
    'validation': 'crossvalidation', # the type of validation to use - crossvalidation, retroactive, or doublecrossvalidation
    'drymask': False, #whether or not to use a drymask of -999
    'scree': True, # whether or not to save % explained variance for eof modes
    'crossvalidation_window': 5,  # number of samples to leave out in each cross-validation step 
    'synchronous_predictors': True, # whether or not we are using 'synchronous predictors'
}
```



```{admonition} What is __force_download?__
The predictor and predictand datasets can be large and lengthy to download from IRI Data Library. Setting `force_download = False` will not repeat thre downloading of datasets if their space and time domains are the same.  
```






     