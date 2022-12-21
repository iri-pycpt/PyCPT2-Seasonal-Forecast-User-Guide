Transforming the Predictand
====

- If the data are not normally distributed, CPT has options to transform the predictand data to a normal distribution. 
- One transformation option is based on the empirical distribution, and so will work for positively and negatively skewed data,  may not be effective if there are numerous ties in the data (for example, many cases of zero precipitation). 
- The other option is to transform the data by fitting a gamma distribution, which may be suitable for positively skewed data, but again is unlikely to work well if there are numerous ties. 
- For both transformation options, the predictand data are transformed to a quantile, which is then transformed to a normal deviate. The inverse process is applied to convert predictions back to the original distribution.