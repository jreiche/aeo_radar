## E.1	Sentinel-1 visualisation and RGB generation

#### Instruction: copy and paste the code below into the GEE console. It is advisable to insert the code snippets step by step and follow the given structure of the practical in order to successfully produce the desired outcome.

#### 1. Select your Area of Interest

```java
// Define a rectangular area of interest, by listing coordinates
var aoi = ee.Geometry.Polygon(
        [[[102.4894322423133, 0.8283349585410177],
          [102.4894322423133, 0.16918388171760845],
          [103.2639683751258, 0.16918388171760845],
          [103.2639683751258, 0.8283349585410177]]], null, false);

// Zoom to area of of interest
Map.centerObject(aoi,10);
```
#### 2. Access Sentinel-1 image collection
For accessing the Sentinel-1 data in GEE search the GEE catalogue of provided data and filter based on the desired aoi and date. Define start and end date and various sensor metadata properties (eg: Instrument mode, orbit pass, etc.). Check the different [sensor specifc parameters](https://developers.google.com/earth-engine/sentinel1).

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

Look at the [ImageCollection](https://developers.google.com/earth-engine/ic_creating) “_Entire collection of Sentinel-1 images_” for a better understanding of GEE's data structure. “_Entire collection of Sentinel-1 images_” is printed in the console on the top right after running the script (Fig. 1).

![fig](/figures/figure_04.png)
<sub>Figure 1. Sentinel-1 ImageCollection in the console of GEE. </sub>

___
> ####  Question E1-1: 
> *How many Sentinel-1 images does the selected image collection contain?*
> ####  Question E1-2: 
> *How many images of this collection were acquired in March 2020?*
___

#### 3. Visualise Sentinel-1 backscatter images
The visualisation of satellite data is only possible for objects of the class [Image](https://developers.google.com/earth-engine/image_overview). The first image of the _ImageCollection_ is selected. 

```java
// Obtain the first image from the Sentinel-1 collection (acquired at February 1st, 2020)
var s1_image = s1_collection.first();

// Print this image to the console
print('First Sentinel-1 image from collection: ', s1_image);
```
Take a look at the single image. The metadata of the image is printed as "_First Sentinel-1 image from collection_" in the console.

To visualize the VV-polarised backscatter image, first subset to the VV-backscatter band of this Sentinel-1 image and then set a number of visualisation [parameters](https://developers.google.com/earth-engine/image_visualization).

```java
var s1VV_image = s1_image.select('VV');

Map.addLayer(s1VV_image, {min: -25, max: 0, palette: ['black', 'white']}, 'Sentinel-1 VV image', true);
```

Examine the plotted Sentinel-1 VV image (Fig. 2). Potentially deselect the visualized layer (purple box in Fig. 2) and change the basemap to optical satellite imagery – “_Satellite_” (orange box in Fig. 2) to get a better idea of the study area. 

The five main classes present in the study area are: __forest (F)__, __non-forest (NF)__, __plantation (P)__, __built-up (B)__ and __water (W)__. Here non-forest is predominantly open soil (logging roads or logged forest/plantation). 

![fig](/figures/figure_05.png)
<sub>Figure 2. Sentinel-1 VV image for February 1st, 2020. </sub>

> ___
> #### Task
> Select the VH backscatter image of “*s1_image*” and visualize it.
>
> ```java
> var s1VH_image = ??? ;
> 
> Map.addLayer(???, {min: -30, max: -5, palette: ['black', 'white']}, 'Sentinel-1 VH image', true);
> ```
>
> #### Question E.1-3: 
> *What differences do you see between the images of the two polarizations?*
> 
> #### Question E.1-4: 
> *Which polarization is the most suitable to visually separate forest (F) and non-forest (NF)?* 
> ___

Check the relationship of the five main land cover classes and the different Sentinel-1 polarizations backscatter values by selecting the “_Inspector_” on the top right in GEE (Fig. 3). Then select a pixel with the mouse cursor by clicking on the map (Fig. 3).

![fig](/figures/figure_06.png)
<sub>Figure 3. Inspector function to access values of plotted maps in GEE. </sub>

Extract backscatter values for a number of pixels (eg. 5 - 10) representing each of the five land cover classes via the GEE “_Inspector_”. Afterwards calculate the mean value for each class based on the selected pixel values and fill it in a table (Example table 1.).

<sub> Table 1. Structure of the backscatter values for VV- and VH-polarization for the five main land cover classes </sub>

__Land cover class__        | __Forest__          |       |     __Non-forest__        |       | __Plantation__      |    | __Built-up__|    |__Water__|    |
------------------------|-----------------|-------|-----------------------|-------|-----------------|----|---------|----|-----|----|
Polarization            | *VV*            |*VH*   |       *VV*            | *VH*  |*VV*             |*VH*|*VV*     |*VH*|*VV* |*VH*|
Pixel 1                 |                 |       |                       |       |                 |    |         |    |     |    |
...                     |                 |       |                       |       |                 |    |         |    |     |    |
Pixel 5                 |                 |       |                       |       |                 |    |         |    |     |    |
*Mean*                |                 |       |                       |       |                 |    |         |    |     |    |

> ___
> #### Question E.1-5: 
> *What are calculated mean values for each class (F, NF, P, W and B) in VV- and VH-polarization?*
> ___

#### 4. Calculating “VV-VH backscatter ratio” and RGB composite creation

To create a false color RGB using a dual-polarized radar image (two image layers only), the backscatter ratio is most commonly used as third image layer. In case of Sentinel-1, the “VV/VH backscatter ratio” is calculated.
In dB scale, the “VV-VH backscatter ratio” is not calculated as a classical ratio (for example NDVI), but simply as *VV backscatter - VH backscatter*. (In linear scale, the “VVVH backscatter ratio” it is calculated as ratio: VV/VH).

To calculate the “VVVH backscatter ratio” and visualize it (Fig. 4) you may use the following code. 
__Note__: In order to successfully run this code a variable called _s1VH_image_ (from visualizing the VH backscatter image) needs to be already defined!

```java
// Calculate the  VVVH backscatter ratio by subtracting the single bands of VV and VH
var s1VVVH_image = (s1VV_image.subtract(s1VH_image)).rename('VVVH');

Map.addLayer(s1VVVH_image, {min: 0, max: 10, palette: ['black', 'white']}, 'Sentinel-1 VVVH image', true);
```

![fig](/figures/figure_07.png)
<sub> Figure 4. VVVH backscatter ratio as grayscale for one Sentinel-1 image </sub>

#### 5. Plot the Sentinel-1 radar RGB

For plotting a radar RGB in GEE first add the newly calculated backscatter ratio to the initial Sentinel-1 image containing the VV- and VH-backscatter images. Afterwards clip the image to the desired aoi.

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
> #### Question E.1-6: 
> *What differences do you see in the VV-VH backscatter ratio image compared to the single VV and VH backscatter images?*
>
> #### Question E.1-7: 
> *Which colours represent the five main land cover classes in the RGB?*
>
> #### Question E.1-8: 
> *Why does water (W) appear black in the RGB? What happens to the radar wave when interacting with a flat (non-rough) water surface? It is helpful to make a sketch including the side-looking radar.*
>
> #### Question E.1-9: 
> *Why are plantations much greener in the RGB than regular forest?*
> ___
