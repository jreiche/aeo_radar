# Multitemporal RGB generation

This part of the practical will give you a short overview on how to generate median values for each pixel over a set period of time for one Sentinel-1 polarization and then plot these as an RGB.

The following code is creating an aoi, searches the Sentinel-1 catalogue within GEE based on dates and instrument metadata and prints the ImageCollection.

```java
// Define a rectangular area of interest, by listing coordinates
var aoi = ee.Geometry.Polygon(
        [[[102.4894322423133, 0.8283349585410177],
          [102.4894322423133, 0.16918388171760845],
          [103.2639683751258, 0.16918388171760845],
          [103.2639683751258, 0.8283349585410177]]], null, false);

// Zoom to this area of of interest
Map.centerObject(aoi);

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
Check the entire ImageCollection via the output in the console (Fig. 8). 

![fig](/figure_08.png)
<sub>Figure 8. ImageCollection of the Sentinel-1 catalogue for the desired aoi from 2018 – 2020/04. </sub>

We are left with 192 single images and need to reduce this information in order to create a meaningful multitemporal RGB. 
First calculate medians for each pixel and each band in the Sentinel-1 ImageCollection for the first quantiles of the years 2018, 2019 and 2020.

```java
// Split the Sentinel-1 collection into three quarterly median composites, to allow creating a multitemporal RGB composite
var s1_median_2018Q1 = s1_collection.filterDate('2018-01-01','2018-04-01').median();
var s1_median_2019Q1 = s1_collection.filterDate('2019-01-01','2019-04-01').median();
var s1_median_2020Q1 = s1_collection.filterDate('2020-01-01','2020-04-01').median();
```
Feel free to print the results and look at the output of the different variables in the console (e.g. via: _print(s1_median_2018Q1,” s1_median_2018Q1”)_). 
As you see the median was calculated for each band separately. In order to visualize an RGB we need to merge the single bands of the three calculated median composite. 
Here we select the VV polarization and rename the band to better localize it in our final multiband image. 
Afterwards we merge all single VV bands of the median composites in one multiband Image.

```java
// Select from each median composite the VV polarisation and rename this corresponding to the time period
var s1VV_median_2018Q1 = s1_median_2018Q1.select('VV').rename('VV_2018Q1')
var s1VV_median_2019Q1 = s1_median_2019Q1.select('VV').rename('VV_2019Q1')
var s1VV_median_2020Q1 = s1_median_2020Q1.select('VV').rename('VV_2020Q1')

// Merge the three images back together, to create one multitemporal image with three bands
var s1VV_multitemporal = s1VV_median_2018Q1.addBands(s1VV_median_2019Q1.addBands(s1VV_median_2020Q1));
```

Print the result in the console and see that we now have three bands in one image of the first quantile median composites of 2018, 2019 and 2020 (Fig. 9).

```java
// Print the multitemporal image to the console
print('Multitemporal Sentinel-1 VV image: ', s1VV_multitemporal)
```
![fig](/figure_09.png)
<sub>Figure 9. Multiband image of the first quantile median composites of 2018, 2019 and 2020. </sub>

All that is left to do is to clip the result to the desired aoi and visualize the RGB based on various parameters.

```java
// Clip the newly created image to the area of interest
var s1VV_multitemporal = s1VV_multitemporal.clip(aoi)

// Add median composite to the map as an RGB image, using the three bands VV, VH and VV/VH
var visualisation_params = {
        "opacity": 1,
        "bands": ["VV_2018Q1","VV_2019Q1","VV_2020Q1"],
        "min": -25,
        "max": 0,
        "gamma": 1
};

Map.addLayer(s1VV_multitemporal, visualisation_params, 'Sentinel-1 multitemporal composite RGB', false)
```

The multitemporal RGB shows various changes throughout the aoi (Fig 10.). 
In order to check the visual parameters (what band is the red channel, etc) you can also adjust and check them in the map layout of GEE (Top right of Fig 10.).

![fig](/figure_10.png)
<sub>Figure 10. RGB of the first quantile median composites of 2018, 2019 and 2020 for Sentinel-1 VV polarization. </sub>

### Question
__Question 4a:__ What areas show the most change and why is that? What main class is showing the most change?

__Question 4b:__ What do the different colours of the map show us (e.g. What do blue, red and yellow patches indicate?)? 

__Question 4c:__ Why are some areas of the shore in a different colour?

### Task
Create the first quantile median composites of 2018, 2019 and 2020 for Sentinel-1 VH polarization. Follow the code above and change if necessary parts to select the VH polarization.

__Question 4d:__ How does the RGB change if you calculate it for the VH-polarization instead of the VV?  Why are these areas different?
