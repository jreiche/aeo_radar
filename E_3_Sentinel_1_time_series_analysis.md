## E.3 Sentinel-1 time series analysis in GEE
#### Instruction: copy and paste the code below into the GEE console. It is advisable to insert the code snippets step by step and follow the given structure of the practical in order to successfully produce the desired outcome.

This exercise shows how to create Sentinel-1 time series plots.

### 1. Multitemporal RGB for indicating areas of change

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

// Split the Sentinel-1 collection into three quarterly median composites, to allow creating a multitemporal RGB composite
var s1_median_2018Q1 = s1_collection.filterDate('2018-01-01','2018-04-01').median();
var s1_median_2019Q1 = s1_collection.filterDate('2019-01-01','2019-04-01').median();
var s1_median_2020Q1 = s1_collection.filterDate('2020-01-01','2020-04-01').median();

// Select from each median composite the VV polarisation and rename this corresponding to the time period
var s1VV_median_2018Q1 = s1_median_2018Q1.select('VV').rename('VV_2018Q1')
var s1VV_median_2019Q1 = s1_median_2019Q1.select('VV').rename('VV_2019Q1')
var s1VV_median_2020Q1 = s1_median_2020Q1.select('VV').rename('VV_2020Q1')

// Merge the three images back together, to create one multitemporal image with three bands
var s1VV_multitemporal = s1VV_median_2018Q1.addBands(s1VV_median_2019Q1.addBands(s1VV_median_2020Q1))

// Print the multitemporal image to the console
print('Multitemporal Sentinel-1 VV image: ', s1VV_multitemporal)

// Clip the newly created image to the area of interest
var s1VV_multitemporal = s1VV_multitemporal.clip(aoi)

// Add median composite to the map as an RGB image, using the three bands for 2018, 2019 and 2020 respectively
var visualisation_params = {
        "opacity": 1,
        "bands": ["VV_2018Q1","VV_2019Q1","VV_2020Q1"],
        "min": -25,
        "max": 0,
        "gamma": 1
};

Map.addLayer(s1VV_multitemporal, visualisation_params, 'Sentinel-1 multitemporal composite RGB')
```

### 2. Time series plots for Sentinel-1 VV and VH

Please insert the code and follow the steps visualized in figure 1. 

```java
// Code below is used to view a Sentinel-1 time series on a clicked point
var s1_timeseries = function(){
  Map.centerObject(geometry,14);
  
  var s1_collection = ee.ImageCollection('COPERNICUS/S1_GRD')
    .filter(ee.Filter.eq('instrumentMode', 'IW'))
    .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))
    .filterDate(ee.Date(start_monitoring), ee.Date(end_monitoring))
    .filterBounds(aoi).select(['VV','VH']);
  
  var ts_chart = ui.Chart.image.series({imageCollection: s1_collection,
    region: geometry,
    reducer: ee.Reducer.first(), scale: 10
  }).setOptions({title: 'S1 time series', vAxis: {minValue: -30, maxValue: 0},
    series: {0: {pointSize: 3, lineWidth: 1}, 1: {pointSize: 3, lineWidth: 1}}
  })
  print(ts_chart)
}

var button = ui.Button({label: 'View time series', onClick: s1_timeseries})
print(button)
```
![fig](/figures/figure_11.png)
<sub>Figure 1. RGB of the first quantile median composites of 2018, 2019 and 2020 for Sentinel-1 VV polarization and highlighted steps necessary to create time series plots in GEE. </sub>

1.	select a geometry point option by clicking on the label 
2.	create a point by clicking on the desired pixel in the map 
3.	an indication of the geometry should be visible above the source code editor field 
4.	run the entire script 
5.	press “_View time series_” button in the console

To create new time series for a specific pixel __FIRST__ delete the current geometry (Fig. 2) and then follow the instructions mentioned above.

![fig](/figures/figure_12.png)
<sub>Figure 2. How to delete a created geometry in GEE. </sub>

>___
>#### Question E.3-1: 
> *Visualize time series for the main five classes (forest, non-forest, plantation, water and built-up) and explain possible differences in VV- and VH-polarizations!*
>
>#### Question E.3-2: 
> *Which scatter mechanism(s) cause the high VH backscatter over forest (F) when compared to non-forest (NF) areas? Briefly explain the mechanism(s).*
>
>#### Question E.3-3: 
> *Is the signal stable over forest? If not, why does it change?*
>
>#### Question E.3-4: 
> *How do you explain the difference of backscatter level for natural forest and plantation?*
>
>#### Question E.3-5:
> *Select 3 areas indicating changes in the multitemporal RGB and plot their time series! Describe what you see and can you explain the potential sudden backscatter increase or decrease after changes?*
>
>#### Extra question: 
> *The analysis was done using C-band radar images acquired by Sentinel-1 (5.3 cm wavelength). How would the separability of forest and non-forest change for an L-band radar image (e.g. ALOS Palsar 2) be, considering its longer wavelength (23.6 cm wavelength)? Do you expect the separability to be better or worse? Explain!*
>___
