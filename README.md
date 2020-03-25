# Advanced Earth Observation - Radar Exercise
### "Introduction to radar remote sensing for forest monitoring in GEE"

Johannes Reiche, Bart Slagter & Johannes Balling
23-03-2020

### Requirements
* Software
  * Google Earth Engine (GEE)
* Dataset provided via GEE
  * Sentinel-1 SAR

### Learning goals
* Introduction to GEE
* Visualisation and RGB generation of Sentinel-1 images in GEE
* Basic time series analysis of Sentinel-1 images in GEE
* Understanding SAR backscatter characteristics over forest land cover

### Content

1. Google Earth Engine Introduction

2. Sentinel-1 SAR data

3. Study area (Riau, Indonsia)
 
4. Google Earth Engine Exercises
   
   4.1. [Sentinel-1 visualisation and RGB generation](https://github.com/jreiche/aeo_radar/blob/master/3_1_Sentinel_1_Visualisation_and_RGB_generation.md)
   
   4.2. [Sentinel-1 multitemporal RGB generation](https://github.com/jreiche/aeo_radar/blob/master/3_2_Sentinel_1_multitemporal_RGB_generation.md)
   
   4.3. [Sentinel-1 time series analysis](https://github.com/jreiche/aeo_radar/blob/master/3_3_Sentinel_1_time_series_analysis.md)


# 1. Introduction to Google Earth Engine
Google Earth Engine (GEE) provides vast amounts of satellite imagery and geospatial datasets ready to use for researcher, scientists and developers. Satellite images include ESA’s Sentinel missions providing both dense optical [(Sentinel-2)](https://developers.google.com/earth-engine/datasets/catalog/sentinel-2) and radar [(Sentinel-1)](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S1_GRD) data streams. These data streams can be directly processed on Googles servers making it a fast and convenient tool to analyse remote sensing data.
For a brief introduction to capabilities of GEE you may watch the videos of this [playlist.](https://www.youtube.com/playlist?list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs). Nessecary videos of this playlist are:
* [General introduction of GEE](https://www.youtube.com/watch?v=W2V_awzKDOg&list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs&index=2&t=0s)
* [Difference between a server and a client.](https://www.youtube.com/watch?v=Tas0c4e_E0M&list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs&index=3&t=0s) It is important to notice that GEE is as server based platform and certain operations that you are used to carry out in R or other programming software are either not working or not advisable.
* [General functions on filtering and displaying data in GEE.](https://www.youtube.com/watch?v=4w6Mt6HTC2I&list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs&index=4&t=0s)

Note that the rest of the videos are not mandatory for this practical, but give a more in depth view on possibilities in GEE.

### Creating and accessing your GEE account
To start working in GEE you first need to sign up, if you have not done so beforehand. Please visit the following [website](https://earthengine.google.com/) and click on the sign-up button on the top of the page (Fig. 3). After filling in all required information, the confirmation for your GEE account might take up to three days according to the website (usually its done in a matter of hours).

![fig](/figures/figure_03.png)
<sub>Figure 3. GEE website - for using GEE, please sign-up on the top right (red box). </sub>

After that you are ready to login and analyse satellite data via the [GEE code editor](https://code.earthengine.google.com/) (Please watch the videos mentioned above to understand the structure of the GEE code editor). 


# 2. Sentinel-1 SAR data
The Sentinel-1 mission comprises a constellation of two polar-orbiting satellites, operating day and night performing C-band synthetic aperture radar imaging, enabling them to acquire imagery regardless of the weather. This C-band radar satellite provides - depending on the acquisition mode - co- and cross-polarization, a spatial resolution of 10 m and a temporal resolution of up to [6 days in the tropics](https://sentinel.esa.int/web/sentinel/missions/sentinel-1/observation-scenario). In this practical we will use [Ground Range Detected](https://sentinel.esa.int/web/sentinel/user-guides/sentinel-1-sar/resolutions/level-1-ground-range-detected) (GRD) products. GRD products consists of focused SAR data that has been detected, multi-looked and projected to ground range using an Earth ellipsoid model. Phase information is lost. For a more indepth dscription please visit [ESA's website](https://sentinel.esa.int/web/sentinel/missions/sentinel-1).

Link to GEE pre-processing: https://developers.google.com/earth-engine/sentinel1
List the pre-processing steps in GEE

# 3. Study area (Riau, Indonesia)
The study area is located in the province of Riau (Indonesia). Riau is located on the east coast of central Sumatra and experiences tropical equatorial climate. This climate is causing a persistent cloud cover throughout the entire year. Primary and secondary dryland and swamp and mangrove forest dominate the natural forest. Riau has the highest forest-cover-loss rates in Indonesia [(Margono et al 2014)](https://www.nature.com/articles/nclimate2277) mainly driven by expansion and conversion to acacia, coconut, rubber plantations and oil palm (Fig.1) [(Uryu et al 2008)](http://assets.panda.org/downloads/riau_co2_report__wwf_id_27feb08_en_lr_.pdf). Oil palm, representing the largest plantation area by species and cover about 3.08 Mha of all land area (Fig.1) [(GlobalForestWatch)](https://www.globalforestwatch.org/dashboards/country/IDN/24?category=land-cover.&dashboardPrompts=eyJvcGVuIjp0cnVlLCJzdGVwSW5kZXgiOjAsInN0ZXBzS2V5Ijoidmlld05hdGlvbmFsRGFzaGJvYXJkcyJ9&map=eyJkYXRhc2V0cyI6W3sib3BhY2l0eSI6MC43LCJ2aXNpYmlsaXR5Ijp0cnVlLCJkYXRhc2V0IjoiNzE0MzM5YzEtYzc3NS00MzAzLWFhZDQtMTZkOTc1YjJmMDIzIiwibGF5ZXJzIjpbIjA3OWZhZTA4LTU2OTYtNDkyNi05NDE3LTc5NGJkM2E3ZThkYyJdfSx7ImRhdGFzZXQiOiJmZGM4ZGMxYi0yNzI4LTRhNzktYjIzZi1iMDk0ODUwNTJiOGQiLCJsYXllcnMiOlsiNmY2Nzk4ZTYtMzllYy00MTYzLTk3OWUtMTgyYTc0Y2E2NWVlIiwiYzVkMWUwMTAtMzgzYS00NzEzLTlhYWEtNDRmNzI4YzA1NzFjIl0sImJvdW5kYXJ5Ijp0cnVlLCJvcGFjaXR5IjoxLCJ2aXNpYmlsaXR5Ijp0cnVlfSx7ImRhdGFzZXQiOiI4OTdlY2M3Ni0yMzA4LTRjNTEtYWViMy00OTVkZTBiZGNhNzkiLCJsYXllcnMiOlsiYzMwNzVjNWEtNTU2Ny00YjA5LWJjMGQtOTZlZDE2NzNmOGI2Il0sIm9wYWNpdHkiOjEsInZpc2liaWxpdHkiOnRydWUsInRpbWVsaW5lUGFyYW1zIjp7InN0YXJ0RGF0ZSI6IjIwMDEtMDEtMDEiLCJlbmREYXRlIjoiMjAxOC0xMi0zMSIsInRyaW1FbmREYXRlIjoiMjAxOC0xMi0zMSJ9LCJwYXJhbXMiOnsidGhyZXNoIjozMCwidmlzaWJpbGl0eSI6dHJ1ZX19XSwiY2VudGVyIjp7ImxhdCI6MC43MTI5NjU1OTc2NTMzNzQzLCJsbmciOjEwMS45MjE4NTAwMDAwMDg3Nn0sImJlYXJpbmciOjAsInBpdGNoIjowLCJ6b29tIjo2LjU2NTcyMjY2NDk3NzY5OSwiY2FuQm91bmQiOmZhbHNlLCJiYm94IjpbXX0%3D).

![fig](/figures/figure_01.png)
<sub>Figure 1. Forest conversion (left) and oil palm plantation (right) in Riau (Indonesia) </sub>

The study area extends approximately 80 x 60 km (0°24’ N; 102°40’ E) and is part of the Pelalawan regency with several plantations and peatlands. The later are diminishing in their quantity due to drainage for land conversion and make them more sensible to fires. Although forbidden by law, fire use for forest removal is still wide-spread in Riau [(Gaveau et al 2014)](https://www.nature.com/articles/srep06112). These fire activities are man-made and can be traced back to fire-related land management practices on forest plantations and can spread to natural forest [(Reiche et al 2018)](https://www.mdpi.com/2072-4292/10/5/777). These fire impacts and logging activities are visible in Sentinel-1 images (Fig. 2).

![fig](/figures/figure_02.png)
<sub>Figure 2. Sentinel-1 RGB (Red: VV-VH, Green: VH, Blue: VV) (left) of an area in Riau and the corresponding photos of the land cover (right – top: water body; middle: secondary peatland forest; plantation logging and secondary peatland forest) </sub>

# 4. Google Earth Engine Exercises
Please click on the different _.md_ files given in this github project and follow the instructions and tasks within them. It is important to carry out the different parts in the intended order of:

4.1. [Sentinel-1 visualisation and RGB generation](https://github.com/jreiche/aeo_radar/blob/master/3_1_Sentinel_1_Visualisation_and_RGB_generation.md)

4.2. [Sentinel-1 multitemporal RGB generation](https://github.com/jreiche/aeo_radar/blob/master/3_2_Sentinel_1_multitemporal_RGB_generation.md)

4.3. [Sentinel-1 time series analysis](https://github.com/jreiche/aeo_radar/blob/master/3_3_Sentinel_1_time_series_analysis.md)
