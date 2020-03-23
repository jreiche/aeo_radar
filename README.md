# Advanced Earth Observation - Radar Exercise
### "Introduction to radar remote sensing for forest monitoring in GEE"

Johannes Reiche, Bart Slagter & Johannes Balling
23-03-2020


### Requirements
* Software
  * Google Earth Engine (GEE)
* Dataset provided via GEE
  * Sentinel-1

### Learning goals
* Introduction to GEE
* Visualisation and RGB generation of SAR images
* Understanding basic SAR backscatter characteristics (VV and VH backscatter values and distributions) over different land cover (forest, plantation, non-forest, built-up and water)
* Linking the backscatter characteristics to particular scatter mechanism on the ground (surface, volume and double bounce scattering). Understanding interactions of the radar wave with objects on the ground (tree, water ...).

### Content

1. Introduction
   1.1 Study Area
   1.2 Data
2. Google Earth Engine Introduction
3. Google Earth Engine Exercises
   3.1 Sentinel-1 visualisation and RGB generation
   3.2 Sentinel-1 multitemporal RGB generation
   3.3 Sentinel-1 time series analysis



# Introduction
## Study area - Riau (Indonesia)
The study area is located in the province of Riau (Indonesia). Riau is located on the east coast of central Sumatra and experiences tropical equatorial climate. This climate is causing a persistent cloud cover throughout the entire year. Primary and secondary dryland and swamp and mangrove forest dominate the natural forest. Riau has the highest forest-cover-loss rates in Indonesia (Margono et al 2014) mainly driven by expansion and conversion to acacia, coconut, rubber plantations and oil palm (Fig.1) (Uryu et al 2008). Oil palm, representing the largest plantation area by species and cover about 3.08 Mha (~34) of all land area (Fig.1) (GlobalForestWatch).

![fig](/figure_01.png)
<sub>Figure 1. Forest conversion and oil palm plantation in Riau (Indonesia) </sub>

The study area extends approximately 80 x 60 km (0°24’ N; 102°40’ E) and is part of the Pelalawan regency with several plantations and peatlands. The later are diminishing in their quantity due to drainage for land conversion and make them more sensible to fires. Although forbidden by law, fire use for forest removal is still wide-spread in Riau (Gaveau et al 2014). These fire activities are man-made and can be traced back to fire-related land management practices on forest plantations and can spread to natural forest. These impacts are visible in Sar images (Fig. 2).

![fig](/figure_02.png)
<sub>Figure 2. Sentinel-1 RGB (Red: VV-VH, Green: VH, Blue: VV) (left) of an area in Riau and the corresponding photos of the land cover (right – top: water body; middle: secondary peatland forest; plantation logging and secondary peatland forest) </sub>

## Sentinel-1 satellite data
Sentinel-1 is a C-band radar satellite providing the “Interferometric Wide swath” mode with co- and cross-polarization, a spatial resolution of 10 m and a temporal resolution of up to 6 days in the tropics [38].
More to come!

Detailed information regarding the Sentinel-1 sensors are accessible on – Reference.

## Introduction to Google Earth Engine
Google Earth Engine (GEE) provides vast amounts of satellite imagery and geospatial datasets ready to use for researcher, scientists and developers. Satellite images include ESA’s Sentinel missions providing both dense optical (Sentinel-2) and radar (Sentinel-1) data streams. These data streams can be directly processed on Googles servers making it a fast and convenient tool to analyse remote sensing data.
For a brief introduction to capabilities of GEE you may watch the videos of this [playlist.](https://www.youtube.com/playlist?list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs)
* [A short general introduction of what GEE is and what you can do with it.](https://www.youtube.com/watch?v=W2V_awzKDOg&list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs&index=2&t=0s)
* [Difference between a server and a client.](https://www.youtube.com/watch?v=Tas0c4e_E0M&list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs&index=3&t=0s) It is important to notice that GEE is as server based platform and certain operations that you are used to carry out in R or other programming software are either not working or not advisable.
* [General functions on filtering and displaying data in GEE.](https://www.youtube.com/watch?v=4w6Mt6HTC2I&list=PLivRXhCUgrZpCR3iSByLYdd_VwFv-3mfs&index=4&t=0s)
Note that the rest of the videos are not mandatory for this practical, but give a more in depth view on possibilities in GEE.

### Creating and accessing your GEE account
To start working in GEE you first need to sign up, if you have not done so beforehand. Please visit the following [website](https://earthengine.google.com/) to do so and click on the sign-up button on the top of the page (Fig. 3). After filling in all required information, the confirmation for your GEE account might take up to three days according to the website (usually its done in a matter of hours).

![fig](/figure_03.png)
<sub>Figure 3. GEE website - for using GEE, please sign-up on the top right (red box). </sub>

After signing up to GEE you are ready to login and analyse satellite data via the GEE code editor (Please watch the videos mentioned above to understand the structure of the GEE code editor). 

Please click on the different _.md_ files of the pratical and follow the instructions and tasks within them. It is important to carry out the different parts in the intended order of:

1. Visualisation and RGB generation
2. Multitemporal RGB generation
3. Time series analysis
