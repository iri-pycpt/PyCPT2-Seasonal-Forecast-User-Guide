# The PyCPT Dictionary 

```{image} img/dict.png
:alt: fishy
:class: bg-primary
:width: 1000px
:align: left
```

Each model has entries in the PyCPT dictionary containing Ingrid URLs that point to the hindcast, observed data and forecast data in IRIDL. Below is an example for the GEFSv12 model, week 1 precipitation:

## Hindcasts 

```python
 SOURCES .Models .SubX .EMC .GEFSv12 .hindcast .weekly .pr
  S (0000 6 Jan 1999) (0000 28 Jun 2017) RANGEEDGES
  S (days since 1999-01-01) streamgridunitconvert
  Y -5 30 RANGE
  X -30 30 RANGE
  L (0) (7) RANGEEDGES
  [M]average
  L 7 runningAverage
  SOURCES .Models .SubX .EMC .GEFSv12 .hindcast .dc0018 .pr
   Y -5 30 RANGE
   X -30 30 RANGE
   L (0) (7) RANGEEDGES
   L 7 runningAverage
   S to366daysample
   [YR]average
   S sampleDOY
   sub
  S (May-Jul) VALUES
  L removeGRID
  S (T) renameGRID
```

This code first selects the `X, Y, and S` (start grid) hindcast ranges. The common SubX hindcast period 6 Jan 1999 - 28 Jun 2017 (common to GEFS, ESRL & CFSv2) is selected. Precipitation rate `.pr` is then averaged over all ensemble members `[M]average` and over leads 0 to 7 days, corresponding to Week 1. 

The second `SOURCES` loads the daily hindcast climatology over the period 2000-2018 `.dc0018` onto the top of the stack and makes X,Y and L selections and the Week 1 average.

The commands
 ```python
S to366daysample
[YR]average
S sampleDOY
sub
```
subtract the climatology to construct Week 1 anomalies from the weekly lead-dependent climatology.

Finally, all hindcasts within the target season are selected `S (May-Jul) VALUES`, the `L` grid is removed, and the `S` grid is renamed `T`.

## Observations matching the hindcasts 

CPT requires that the time sequences predictor and predictand **match** each other; they must correspond to the same weeks. This is achieved in ingrid by first loading the hindcast target weeks. The obs data is them sampled to match these using  `T 2 index .T SAMPLE`.

```python
SOURCES .Models .SubX .EMC .GEFSv12 .hindcast .weekly .pr
  S (0000 6 Jan 1999) (0000 28 Jun 2017) RANGEEDGES
  L (0) (7) RANGEEDGES
  L 7 runningAverage
  S (May-Jul) VALUES
  L
   S add
   [L S] /T sampleNDto1D 	# T grid of hindcast Week 1 samples
  SOURCES .UCSB .CHIRPS .v2p0 .daily-improved .global .0p25 .prcp
   X -180. .5 180. GRID		# Interploation of CHIRPS to 0.5-degree grid
   Y -90 .5 90 GRID
   Y 1 20 RANGE
   X -20 10 RANGE
   SOURCES .ECMWF .S2S .climatologies .observed .CHIRPS .prcpSmooth
    X -180. .5 180. GRID
    Y -90 .5 90 GRID
    Y 1 20 RANGE
    X -20 10 RANGE
    T to366daysample
    [YR]average
    T sampleDOY
    sub				# Subtract CHIRPS daily climatology ot from daily anomalies
   T (days since 1960-01-01) streamgridunitconvert
   T 7 runningAverage		# CHIRPS weekly anomalies
   c: 7.0
     /units /days def
     :c
    mul
   T 2 index .T SAMPLE		# Sample CHIRPS according to the hindcasts 
   nip dup T npts
     /I exch NewIntegerGRID
     replaceGRID
   I
    3 -1 roll
    .T replaceGRID		# More Ingrid magic :)
```
## Forecasts 

This one is the shortest. Get the forecast and subtract the hindcast climatology for that start and lead:

```python
SOURCES .Models .SubX .EMC .GEFSv12 .forecast .pr
  S (0000 2 Jun 2021) VALUES
  Y -5 30 RANGE
  X -30 30 RANGE
  L (0) (7) RANGEEDGES
  [M]average
  L 7 runningAverage
  SOURCES .Models .SubX .EMC .GEFSv12 .hindcast .dc0018 .pr
   Y -5 30 RANGE
   X -30 30 RANGE
   L (0) (7) RANGEEDGES
   L 7 runningAverage
   S (T) renameGRID
   pentadAverage pentadmean
   T (S) renameGRID
   [S]regridLinear
   S
    1 setgridtype
    pop
   S 2 index .S SAMPLE
   sub
  ```

```{warning}
This should be updated to use the following (as used in the hindcasts) in place of `pentadAverage`:
```python
S to366daysample
[YR]average
S sampleDOY  
```
