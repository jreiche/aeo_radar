# E.2 Sentinel-1 multitemporal RGB generation
#### Instruction: copy and paste the code below into the GEE console. It is advisable to insert the code snippets step by step and follow the given structure of the practical in order to successfully produce the desired outcome.

This exercise introduces the calculation of median values for each pixel over a set period of time for one Sentinel-1 polarization. This is carried out for several timestamp allowing for the generation of a Sentinel-1 multitemporal RGB. 


### 1. Access Sentinel-1 image collection

```java
// Define a rectangular area of interest, by listing coordinates
var aoi = ee.Geometry.Polygon(
        [[[102.4894322423133, 0.8283349585410177],
          [102.4894322423133, 0.16918388171760845],
          [103.2639683751258, 0.16918388171760845],
          [103.2639683751258, 0.8283349585410177]]], null, false);

// Zoom to this area of of interest
Map.centerObject(aoi,10);

// Define a start and end date for the analysis
var start_monitoring = ee.Date('2018-01-01');
var end_monitoring = ee.Date('2020-04-01');

// Get a collection of Sentinel-1 images in the area of interest and apply some filters
var s1_collection = ee.ImageCollection('COPERNICUS/S1_GRD')
  .filter(ee.Filter.eq('instrumentMode', 'IW'))
  .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))
  .filterDate(ee.Date(start_monitoring), ee.Date(end_monitoring))
  .filterBounds(aoi);

// Print the image collection to the console
print('Entire collection of Sentinel-1 images: ', s1_collection);
```
Check the entire ImageCollection via the output in the console (Fig. 1). 

![fig](/figures/figure_08.png)
<sub>Figure 1. ImageCollection of the Sentinel-1 catalogue for the desired aoi from 2018 – 2020/04. </sub>


### 2. Median calculation for an image collection
This collection contains 192 single images and needs to reduced in order to create a meaningful multitemporal RGB. 
First calculate medians for each pixel and each band in the Sentinel-1 ImageCollection for the first quantiles of the years 2018, 2019 and 2020.

```java
// Split the Sentinel-1 collection into three quarterly median composites, to allow creating a multitemporal RGB composite
var s1_median_2018Q1 = s1_collection.filterDate('2018-01-01','2018-04-01').median();
var s1_median_2019Q1 = s1_collection.filterDate('2019-01-01','2019-04-01').median();
var s1_median_2020Q1 = s1_collection.filterDate('2020-01-01','2020-04-01').median();
```
Print the results and look at the output in the console (e.g. via: _print('s1_median_2018Q1:',s1_median_2018Q1)_). 


### 3. Multitemporal Senintel-1 image
Select the VV polarization and rename the band to better localize it later. Afterwards merge all single VV backscatter images of the median composites in one multiband Image.

```java
// Select from each median composite the VV polarisation and rename this corresponding to the time period
var s1VV_median_2018Q1 = s1_median_2018Q1.select('VV').rename('VV_2018Q1')
var s1VV_median_2019Q1 = s1_median_2019Q1.select('VV').rename('VV_2019Q1')
var s1VV_median_2020Q1 = s1_median_2020Q1.select('VV').rename('VV_2020Q1')

// Merge the three images back together, to create one multitemporal image with three bands
var s1VV_multitemporal = s1VV_median_2018Q1.addBands(s1VV_median_2019Q1.addBands(s1VV_median_2020Q1));

// Print the multitemporal image to the console
print('Multitemporal Sentinel-1 VV image: ', s1VV_multitemporal)
```
Look at the printed result of the multiband image containing the first quantile median composites of 2018, 2019 and 2020 (Fig. 2).

![fig](/figures/figure_09.png)
<sub>Figure 2. Multiband image of the first quantile median composites of 2018, 2019 and 2020. </sub>


### 4. Visualization of a multitemporal RGB

Clip the multiband image to the desired aoi and visualize the RGB.

```java
// Clip the newly created image to the area of interest
var s1VV_multitemporal = s1VV_multitemporal.clip(aoi)

// Add median composite to the map as an RGB image, using the three bands "VV_2018Q1","VV_2019Q1","VV_2020Q1"
var visualisation_params = {
        "opacity": 1,
        "bands": ["VV_2018Q1","VV_2019Q1","VV_2020Q1"],
        "min": -25,
        "max": 0,
        "gamma": 1
};

Map.addLayer(s1VV_multitemporal, visualisation_params, 'Sentinel-1 multitemporal composite RGB', true)
```

The multitemporal RGB shows various changes throughout the aoi (Fig 3.). 
To check the visual parameters (what band is the red channel, etc) inspect and adjust them directly in the map layout of GEE (Top right of Fig 3.).

![fig](/figures/figure_10.png)
<sub>Figure 3. RGB of the first quantile median composites of 2018, 2019 and 2020 for Sentinel-1 VV polarization. </sub>


> ___
>#### Question 3.2a: 
> *What areas show the most change? Is the change equally distributed for all land cover classes?*
>
>#### Question 3.2b: 
> *What do the different colours of the map indicate (e.g.blue, red and yellow patches?)?* 
>
>#### Question 3.2c:
> *Why are some areas along the shoreline in a different colour?*
> ___
> ___
> #### Task
> Create the first quantile median composites of 2018, 2019 and 2020 for Sentinel-1 VH polarization. Follow the code from above and change if necessary.
>
> #### Question 3.2d: 
> *How does the RGB change for the VH-polarization instead of VV? Why are these areas different?*
> ___
