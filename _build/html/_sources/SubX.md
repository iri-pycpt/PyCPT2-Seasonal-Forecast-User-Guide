# The SubX models

```{image} img/subx.png
:alt: fishy
:class: bg-primary
:width: 600px
:align: left
```

PyCPT currently allows the following SubX models to be used:
- [NCEP CFSv2](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX/.NCEP/): 
initialized every 6 hours
- [EMC GEFSv12](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX/.EMC/.GEFSv12/): 
initialized every Wednesday 
- [ESRL FIM](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX/.ESRL/): 
initialized every Wednesday 
- [ECCC GEM (GEPS6)](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX/.ECCC/.GEPS6/): 
initialized every Thursday in real time

#### Important Notes

Except for ECCC (Environment Canada), the hindcasts were made every Wednesday during the 1999--2016 hindcast period
using a fixed version of the forecast model. 

ECCC uses a different approach in which a new set of hindcasts is made every time that a 
that a forecast is made (i.e. every Thursday in real time). This  **"on-the-fly"** approach 
allows ECCC to update their forecast model whenever they want. ECMWF also uses an
on-the-fly approach.

When ECCC makes a forecast every Thursday in real time, they run a set of 20 hindcasts
initialized on the same calendar date of each year 1998--2017. But note that these hindcast 
starts are generally not on Thursdays of the hindcast!

The CFSv2 forecasts are initialized every 6 hours with 4 members (1 member for the
hindcasts). A **lagged ensemble** is used to augment the ensemble size, by including the
initializations over 1 or more days. This involves a trade off between ensemble size 
and increasing lead time. PyCPT uses a 3-day lagged ensemble, which yields 12 ensemble
members (3 x 4) for the hindcasts and 48 for the forecasts (3 x 16).

#### Resources  

- [SubX model table](http://cola.gmu.edu/subx/data/modelinfo.html)

- [SubX BAMS paper](https://journals.ametsoc.org/view/journals/bams/100/10/bams-d-18-0270.1.xml)

- [SubX Data in IRI Data Library](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX/)


%#### A SubX PyCPT MME worked Example

%This example will use the CFSv2, GEFSv12 and FIM models to build a subseasonal MME.

---