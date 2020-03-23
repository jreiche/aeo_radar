#	Visualisation and RGB generation
## Visualisation
### !!!! Important !!!!
For the entire practical you need to copy and paste all parts of the code below into the console of GEE. It is advisable to insert the code snippets step by step and follow the given structure of the practical in order to successfully run the code.
### !!!! Important !!!!

To start analysing Satellite Imagery in GEE you first have to sign up and login into GEE code editor. Once you have done that we can start by selecting an Area of Interest (aoi) and zooming to it.

```java
// Define a rectangular area of interest, by listing coordinates
var aoi = ee.Geometry.Polygon(
        [[[102.4894322423133, 0.8283349585410177],
          [102.4894322423133, 0.16918388171760845],
          [103.2639683751258, 0.16918388171760845],
          [103.2639683751258, 0.8283349585410177]]], null, false);

// Zoom to this area of of interest
Map.centerObject(aoi);
```
In order to access the Sentinel-1 data in GEE you need to search the GEE catalogue of provided data and filter based on the desired aoi and date. Therefore you need to define a start and end date and several sensor metadata properties (eg: Instrument mode, orbit pass, etc.).

```java
// Define a start and end date for the analysis
var start_monitoring = ee.Date('2020-02-01');
var end_monitoring = ee.Date('2020-03-15');

// Get a collection of Sentinel-1 images in the area of interest and apply some filters
var s1_collection = ee.ImageCollection('COPERNICUS/S1_GRD')
  .filter(ee.Filter.eq('instrumentMode', 'IW'))
  .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))
  .filterDate(ee.Date(start_monitoring), ee.Date(end_monitoring))
  .filterBounds(aoi);
```

To get a better understanding of GEE and the data structure look at the ImageCollection “Entire collection of Sentinel-1 images”. This ImageCollection should be printed in the console on the top right after running the script (Fig. 4).

