# 3.1	Sentinel-1 visualisation and RGB generation
## Visualisation
### For the entire practical: copy and paste all parts of the code below into the console of GEE. It is advisable to insert the code snippets step by step and follow the given structure of the practical in order to successfully produce the disered outcome.

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
  
 // Print the image collection to the console
print('Entire collection of Sentinel-1 images: ', s1_collection);
```

To get a better understanding of GEE and the data structure look at the ImageCollection “_Entire collection of Sentinel-1 images_”. This ImageCollection should be printed in the console on the top right after running the script (Fig. 1).

![fig](/figures/figure_04.png)
<sub>Figure 1. Sentinel-1 ImageCollection in the console of GEE. </sub>

> ___
> ### Question
> __Question 3.1a:__ How is the ImageCollection arranged and what does it contain? 
> 
> __Question 3.1b:__ How many Images are in this Collection?
> 
> __Question 3.1c:__ How many Images of this collection were acquired in March 2020?
> ___

The visualisation of images is only possible for objects of the class _Image_ in GEE. Therefore we select the first Image of the _ImageCollection_. 

```java
// Obtain the first image from the Sentinel-1 collection (acquired at February 1st, 2020)
var s1_image = s1_collection.first();

// Print this image to the console
print('First Sentinel-1 image from collection: ', s1_image);
```
Take a look at the single Image and print it in the console. The metadata of the image is printed as "_First Sentinel-1 image from collection_" in the console.

To visualize a VV-polarized Sentinel-1 image as a grayscale, we first subset to the VV-band of this Sentinel-1 image. After that we visualize the band.

```java
var s1VV_image = s1_image.select('VV');

Map.addLayer(s1VV_image, {min: -25, max: 0, palette: ['black', 'white']}, 'Sentinel-1 VV image', true);
```

Examine the printed Sentinel-1 VV image (Fig. 2). To get a better idea of the study area, you may also deselect the visualized layer (purple box in Fig. 2) and change the basemap to an optical satellite imagery – “Satellite” (orange box in Fig. 2). 

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
> Map.addLayer(???, {min: -30, max: -5, palette: ['black', 'white']}, 'Sentinel-1 VH image', false);
> ```
> __Question 3.1d:__ What differences do you see between the maps of the two polarizations?
> 
> __Question 3.1e:__ Which polarization is the most suitable to visually separate forest (F) and non-forest (NF)? 
> ___

For a better understanding of the backscatter values for the five main classes you might check their specific values, by selecting the “_Inspector_” on the top right and then select a point with the mouse courser by clicking on the maps (Fig. 3).

![fig](/figures/figure_06.png)
<sub>Figure 3. Inspector function to access values of plotted maps in GEE. </sub>

To describe a class specific backscatter characteristic, extract backscatter values for 10 pixels of homogeneous areas representing each of the five land cover classes via the GEE “_Inspector_”. Afterwards calculate the minimal value, the maximal value, the median and the standard deviation based on the 10 pixel for each class and fill it in a table (Example table 1.).

![fig](/figures/table_01.PNG)
<sub> Table 1. Sentinel-1 image modes at medium spatial resolution. VV - vertical transmit and vertical receive polarisation; VH - vertical transmit and horizontal receive polarisation. </sub>

> ___
> ### Question
> 
> __Question 3.1f:__ What are significant differences in the calculated median and standard deviation values for VV- and VH-polarization  of each class (F, NF, P, W and B)? Which class shows the lowest and highest values?
> 
> __Question 3.1g:__ Assign for each of the five main classes the range of backscatter values that you examine with the help of the GEE Inspector.
> ___

## Calculating “VVVH backscatter ratio” and RGB composite creation

To create a false colour RGB using a dual-polarised SAR image (two image layers only), the backscatter ratio is most commonly used as third image layer. In case of Sentinel-1, the “VVVH backscatter ratio” is calculated.
In dB scale, the “VVVH backscatter ratio” is not calculated as a classical ratio (for example NDVI), but simply as VV backscatter - VH backscatter. (In linear scale, the “VVVH backscatter ratio” it is calculated as ratio: VV/VH).

To calculate the “VVVH backscatter ratio” and visualize it (Fig. 4) you may use the following code. 
__Note__ that in order to successfully run this code you need to have defined the variable called _s1VH_image_ via the task from visualizing the VH band from before!

```java
// Calculate the  VVVH backscatter ratio by subtracting the single bands of VV and VH
var s1VVVH_image = (s1VV_image.subtract(s1VH_image)).rename('VVVH');

Map.addLayer(s1VVVH_image, {min: 0, max: 10, palette: ['black', 'white']}, 'Sentinel-1 VVVH image', false);
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

Map.addLayer(s1_image_clip, visualisation_params, 'Sentinel-1 RGB', false)
```
> ___
> ### Question
> __Question 3.1h:__ What differences do you see in the ratio band compared to the single VV and VH bands and which classes are visually especially good separately?
>
> __Question 3.1i:__ Which colours represent the five main land cover classes in the RGB and why?
>
>__Question 3.1j:__ Why are plantations much greener in the RGB than regular forest?
> ___

## Analysing and describing class specific backscatter characteristics
Analysing and describing the SAR backscatter is fundamental to build the link between the observed SAR backscatter values and the objects on the ground (trees, river ...). This will assist to answer the following type of questions: How did the differently polarised radar (VV vs. VH) waves interact with the objects on the ground to result in the observed VV and VH backscatter values? Which scatter mechanisms (surface, volume and/or double bounce scattering) did cause for example high VH values over forest? 
First, the backscatter characteristics of the five main classes are to be analysed and described: forest (F), non-forest (NF), plantation (P), built-up (B) and water (W). This is to be done individually for VV, VH and “VVVH backscatter ratio”. 
