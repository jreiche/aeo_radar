# bayts 

Set of tools to apply the probabilistic approach of Reiche et al. (2015, 2018) to combine multiple optical and/or Radar satellite time series and to detect deforestation/forest cover loss in near real-time. The package includes functions to apply the approach to both, single pixel time series and raster time series. Examples and test data are provided below.

### Research version
The package includes the research version of the tools, which have a limited perforamce when applied to raster time series. The reserach version allows (i) to visualise and analyse the entire time series history and (ii) it stepwise applies the probablistic approach consecutively on each observation in the time series to emulate a near real-time scenario. It includes methods used in Reiche et al., 2015 (RSE), Reiche et al., 2018 (RSE) and Reiche et al., 2018 (Remote Sensing)

## Probablistic approach 
The basic version of the probabilistic approach has been published in Reiche et al., (2015). An improved version was published in Reiche et al. (2018, RSE) and was used in Reiche et al., 2018 (Remote Sensing). 

Figure 1 gives an schematic overview the probabilistic approach. We considered a near real-time scenario with past (t-1), current (t) and future observations (t+1), with multiple observations possible at the same observation date. First, once a new observation of either of the input time series was available (t = current) it was converted to the conditional NF probability (s<sup>NF</sup>) using the sensor specific forest (F) and non-forest (NF) probability density functions (pdf) (The sensor specific F and NF pdfs were derived using training data).
The derived conditional NF probability was added to the combined time series of conditional NF probabilities derived from the previous LNDVIn, S1VVn and P2HVn time series observations (t–i). Second, we flagged a potential deforestation event in the case that the conditional NF probability was larger than 0.5. We calculated the probability of deforestation using iterative Bayesian updating. Future observation (t+i) were used to update the probability of deforestation in order to confirm or reject the flagged deforestation event.

![fig](/examples/method_overview.jpg)
<sub>Figure 1. Probabilistic approach used to combine time series of Landsat NDVI (LNDVIn), Sentinel-1 VV (S1VVn) and ALOS-2 PALSAR-2 HV (P2HVn) observations and to detect deforestation in near real-time. (Reiche et al., under review) </sub>

## Core functions

bayts - applies probabalistic approach to single pixel time series (method presented in Reiche et al., 2018, RSE)

baytsSpatial - applies bayts to raster time series

baytsDD - bayts with a priori (i) seasonal-trend model fitting to remove forest seasonality and (ii) data-driven way to derive forest and non-forest distributions (method presented in Reiche et al., 2018, Remote Sensing)

baytsDDSpatial - applies baytsDD to raster time series

## Install

The package can be installed directly from github using devtools
```r
library(devtools)
install_github('jreiche/bayts')
```
