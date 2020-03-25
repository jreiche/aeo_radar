# 3.1	Sentinel-1 visualisation and RGB generation
## Visualisation
### For the entire practical: copy and paste all parts of the code below into the console of GEE. It is advisable to insert the code snippets step by step and follow the given structure of the practical in order to successfully produce the desired outcome.

#### 1. Select your Area of Interest (aoi) and zoom to it.

```java
// Define a rectangular area of interest, by listing coordinates
var aoi = ee.Geometry.Polygon(
        [[[102.4894322423133, 0.8283349585410177],
          [102.4894322423133, 0.16918388171760845],
          [103.2639683751258, 0.16918388171760845],
          [103.2639683751258, 0.8283349585410177]]], null, false);

// Zoom to this area of of interest
Map.centerObject(aoi,10);
```
#### 2. Access Sentinel-1 images
In order to access the Sentinel-1 data in GEE you need to search the GEE catalogue of provided data and filter based on the desired aoi and date. Therefore you need to define a start and end date and several sensor metadata properties (eg: Instrument mode, orbit pass, etc.). Hereby various [sensor specifc parameters](https://developers.google.com/earth-engine/sentinel1) are possible to select.

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
  
 // Print the image collection to the console
print('Entire collection of Sentinel-1 images: ', s1_collection);
```

To get a better understanding of GEE and its data structure look at the [ImageCollection](https://developers.google.com/earth-engine/ic_creating) “_Entire collection of Sentinel-1 images_”. This _ImageCollection_ should be printed in the console on the top right after running the script (Fig. 1).

![fig](/figures/figure_04.png)
<sub>Figure 1. Sentinel-1 ImageCollection in the console of GEE. </sub>

> ___
> ### Question
> __Question 3.1a:__ What is an ImageCollection and what does it contain? 
> 
> __Question 3.1b:__ How many Images are in _Entire collection of Sentinel-1 images_?
> 
> __Question 3.1c:__ How many Images of this collection were acquired in March 2020?
> ___

3. Visualise Sentinel-1 backscatter images
The visualisation of satellite data is only possible for objects of the class [Image](https://developers.google.com/earth-engine/image_overview) in GEE. Therefore we select the first Image of the _ImageCollection_. 

```java
// Obtain the first image from the Sentinel-1 collection (acquired at February 1st, 2020)
var s1_image = s1_collection.first();

// Print this image to the console
print('First Sentinel-1 image from collection: ', s1_image);
```
Take a look at the single Image. The metadata of the image is printed as "_First Sentinel-1 image from collection_" in the console.

To visualize a VV-polarized Sentinel-1 image as a grayscale, we first subset to the VV-band of this Sentinel-1 image. After that we visualize the band based on various [parameters](https://developers.google.com/earth-engine/image_visualization).

```java
var s1VV_image = s1_image.select('VV');

Map.addLayer(s1VV_image, {min: -25, max: 0, palette: ['black', 'white']}, 'Sentinel-1 VV image', true);
```

Examine the printed Sentinel-1 VV image (Fig. 2). To get a better idea of the study area, you may also deselect the visualized layer (purple box in Fig. 2) and change the basemap to optical satellite imagery – “_Satellite_” (orange box in Fig. 2). 

The five main classes present in the study area are: __forest (F)__, __non-forest (NF)__, __plantation (P)__, __built-up (B)__ and __water (W)__. Here non-forest is predominantly open soil (logging roads or logged forest/plantation). 

![fig](/figures/figure_05.png)
<sub>Figure 2. Sentinel-1 VV image for February 1st, 2020. </sub>

> ___
> ### Task
> Now try to select the VH band of “s1_image” yourself and visualize it.
> (Hint: Do it similar to the VV-polarization and fill out spaces of __???__ in the code below)
>
> ```java
> var s1VH_image = ??? ;
> 
> Map.addLayer(???, {min: -30, max: -5, palette: ['black', 'white']}, 'Sentinel-1 VH image', true);
> ```
> __Question 3.1d:__ What differences do you see between the maps of the two polarizations?
> 
> __Question 3.1e:__ Which polarization is the most suitable to visually separate forest (F) and non-forest (NF)? 
> ___

Check the relationship of the five main land cover classes and the different Sentinel-1 polarizations backscatter values by selecting the “_Inspector_” on the top right in GEE (Fig. 3). Then select a pixel with the mouse courser by simply clicking on the map (Fig. 3).

![fig](/figures/figure_06.png)
<sub>Figure 3. Inspector function to access values of plotted maps in GEE. </sub>

Extract backscatter values for 10 pixels representing each of the five land cover classes via the GEE “_Inspector_” to describe class specific backscatter characteristics. Afterwards calculate the minimal value, the maximal value, the median and the standard deviation based on the 10 pixel for each class and fill it in a table (Example table 1.).

![fig](/figures/table_01.PNG)
<sub> Table 1. Structure of the backscatter values for VV- and VH-polarization for the five main land cover classes </sub>

> ___
> ### Question
> 
> __Question 3.1f:__ What are significant differences in the calculated median and standard deviation values for each class (F, NF, P, W and B) for VV-polarization? Which class shows the lowest and highest values?
> 
> __Question 3.1g:__ What land cover classes are easy to separate based on their backscatter value ranges for either VV- and VH-polarization?
> ___

## Calculating “VVVH backscatter ratio” and RGB composite creation

To create a false colour RGB using a dual-polarised SAR image (two image layers only), the backscatter ratio is most commonly used as third image layer. In case of Sentinel-1, the “VVVH backscatter ratio” is calculated.
In dB scale, the “VVVH backscatter ratio” is not calculated as a classical ratio (for example NDVI), but simply as VV backscatter - VH backscatter. (In linear scale, the “VVVH backscatter ratio” it is calculated as ratio: VV/VH).

To calculate the “VVVH backscatter ratio” and visualize it (Fig. 4) you may use the following code. 
__Note__ that in order to successfully run this code you need to have defined the variable called _s1VH_image_ via the task from visualizing the VH band from before!

```java
// Calculate the  VVVH backscatter ratio by subtracting the single bands of VV and VH
var s1VVVH_image = (s1VV_image.subtract(s1VH_image)).rename('VVVH');

Map.addLayer(s1VVVH_image, {min: 0, max: 10, palette: ['black', 'white']}, 'Sentinel-1 VVVH image', true);
```

![fig](/figures/figure_07.png)
<sub> Figure 4. VVVH backscatter ratio as grayscale for one Sentinel-1 image </sub>

For plotting a radar RGB in GEE we first need to add the newly calculated backscatter ratio to the initial Sentinel-1 image containing the VV- and VH-bands. Out of convenience we also clip the image to the desired aoi.

```java
// Add the backscatter ratio to the initial Sentinel-1 image
var s1_image = s1_image.addBands(s1VVVH_image);

// Clip the Sentinel-1 image
var s1_image_clip = s1_image.clip(aoi)

// Add the Sentinel-1 image to the map as an RGB image, using the three bands VV, VH and VV/VH
var visualisation_params = {
        "opacity": 1,
        "bands": ["VV","VH","VVVH"],
        "min": [-25,-30,0],
        "max": [-5,-10,10],
        "gamma": [1,1,1]
};

Map.addLayer(s1_image_clip, visualisation_params, 'Sentinel-1 RGB', true)
```
> ___
> ### Question
> __Question 3.1h:__ What differences do you see in the ratio band compared to the single VV and VH bands and which classes are visually good to separate?
>
> __Question 3.1i:__ Which colours represent the five main land cover classes in the RGB?
>
>__Question 3.1j:__ Why does water (W) appear black in the RGB? What happens to the radar wave when interacting with a flat (non-rough) water surface? It is helpful to make a sketch including the side-looking radar.
>
>__Question 3.1k:__ Why are plantations much greener in the RGB than regular forest?
> ___

## Analysing and describing class specific backscatter characteristics
Analysing and describing the SAR backscatter is fundamental to build the link between the observed SAR backscatter values and the objects on the ground (trees, river ...). This will assist to answer the following type of questions: How did the differently polarised radar (VV vs. VH) waves interact with the objects on the ground to result in the observed VV and VH backscatter values? Which scatter mechanisms (surface, volume and/or double bounce scattering) did cause for example high VH values over forest? 
First, the backscatter characteristics of the five main classes are to be analysed and described: forest (F), non-forest (NF), plantation (P), built-up (B) and water (W). This is to be done individually for VV, VH and “VVVH backscatter ratio”. 
