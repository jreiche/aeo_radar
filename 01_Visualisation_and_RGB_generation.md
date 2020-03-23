#	Visualisation and RGB generation
## Visualisation
### !!!! Important !!!!
For the entire practical you need to copy and paste all parts of the code below into the console of GEE. It is advisable to insert the code snippets step by step and follow the given structure of the practical in order to successfully run the code.

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

```java
// Print the image collection to the console
print('Entire collection of Sentinel-1 images: ', s1_collection);
```
![fig](/figure_04.png)
<sub>Figure 4. Sentinel-1 ImageCollection in the console of GEE. </sub>

### Questions
__Question 1a:__ How is the ImageCollection arranged and what does it contain? 

__Question 1b:__ How many Images are in this Collection?

__Question 1c:__ How many Images of this collection were acquired in March 2020?

The visualisation of images is only possible for objects of the class _Image_ in GEE. Therefore we select the first Image of the _ImageCollection_.

```java
// Obtain the first image from the Sentinel-1 collection (acquired at February 1st, 2020)
var s1_image = s1_collection.first();
```
Take a look at the single Image and print it in the console. The metadata of the image is printed in the console.

```java
// Print this image to the console
print('First Sentinel-1 image from collection: ', s1_image)
```

To visualize a VV-polarized Sentinel-1 image as a grayscale, we first subset to the VV-band of this Sentinel-1 image. After that we visualize the band.

```java
var s1VV_image = s1_image.select('VV');

Map.addLayer(s1VV_image, {min: -25, max: 0, palette: ['black', 'white']}, 'Sentinel-1 VV image', false);
```

Examine the printed Sentinel-1 VV image (Fig. 5). To get a better idea of the study area, you may also deselect the visualized layer (purple box in Fig. 5) and change the basemap to an optical satellite imagery – “Satellite” (orange box in Fig. 5). 

The five main classes present in the study area are: __forest (F)__, __non-forest (NF)__, __plantation (P)__, __built-up (B)__ and __water (W)__. Here non-forest is predominantly open soil (logging roads or logged forest/plantation). 

![fig](/figure_05.png)
<sub>Figure 5. Sentinel-1 VV image for February 1st, 2020. </sub>

### Task
Now try to select the VH band of “s1_image” yourself and visualize it similar to the VV-image.
(Hint: Do it similar to the VV-polarization and fill out spaces of __???__ yourself - see below)

```java
var s1VH_image = ??? ;

Map.addLayer(???, {min: -30, max: -5, palette: ['black', 'white']}, 'Sentinel-1 VH image', false);
```

