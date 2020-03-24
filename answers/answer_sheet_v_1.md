__Question 3.1a:__ What is an ImageCollection and what does it contain?

* An ImageCollection is a stack or time series of images. Therefore it contains the Images themselves and important metadata of the sensor (e.g. acquisition mode etc.) and images (acquisition time etc.)

__Question 3.1b:__ How many Images are in Entire collection of Sentinel-1 images?

* 11 images

__Question 3.1c:__ How many Images of this collection were acquired in March 2020?

* 5 Images (2x 01-03-2020; 1x 08-03-2020; 2x 13-03-2020)
___
__Task__
Now try to select the VH band of “s1_image” yourself and visualize it. (Hint: Do it similar to the VV-polarization and fill out spaces of ??? in the code below)
```java
var s1VH_image = s1_image.select('VH');

Map.addLayer(s1VH_image, {min: -30, max: -5, palette: ['black', 'white']}, 'Sentinel-1 VH image', true);
```
___

__Question 3.1d:__ What differences do you see between the maps of the two polarizations?
* Certain Plantations better visible in VV due to lower backscatter values
* Water is smoother in VH
* non-forest is more visible in VH
* some strong scatter in built-up areas vanish or have less impact in VH
* forest is similar in both polarizations, but VH seems smoother

__Question 3.1e:__ Which polarization is the most suitable to visually separate forest (F) and non-forest (NF)?
* Both polarizations seems similar for forest, but the difference for non-forest and forest is much more visible in VH

__Question 3.1f:__ What are significant differences in the calculated median and standard deviation values for each class (F, NF, P, W and B) in VV-polarization? Which class shows the lowest and highest values?
* see values in figure
* highest = built-up (median 0.4)
* lowest = water (median -21.1)

![fig](/answers/answer_sheet_table_01.PNG)
<sub> Table of values for the five different land cover classes for both polarizations </sub>


__Question 3.1g:__ What land cover classes are easy to separate based on their backscatter value ranges for either VV- and VH-polarization?
* Water due to very low values (insert value range) easy to separate for both polarizations
* Built-up due to high values (insert value range) easy to separate for both polarizations
* Plantation is easier to separate for VV-polarization due to higher values for Forest compared to Plantations (For VH quite similar)
* Non-forest is easier to separate for VH-polarization due to lower values 
* Forest see Plantations!

__Question 3.1h:__ What differences do you see in the ratio band compared to the single VV and VH bands and which classes are visually good to separate?
* Every land cover that shows differences between VV- and VH-Polarization are very distinct and therefore also easy to detect

  * Plantations
  * non-forest
  
* Water shows high values and beforehand low values for both polarizations
* Forest seems similar
* Built-up still shows very high values

__Question 3.1i:__ Which colours represent the five main land cover classes in the RGB?
* Blue = water
* Green = plantations
* beige = forest
* white = built-up
* violet or pink = non-forest

__Question 3.1j:__ Why does water (W) appear black in the RGB? What happens to the radar wave when interacting with a flat (non-rough) water surface? It is helpful to make a sketch including the side-looking radar.
* Scatter away from satellite due to mirror effect on the smooth surface of the water (due to side-looking into the directions of the satellite) --> no recieved energy at satellite over water 


__Question 3.1k:__ Why are plantations much greener in the RGB than regular forest?
* Higher values for VH polarization --> less volume scattering, but distinct vertical profiles of tree stems and therefore low backscatter in VV

__Question__
Analysing and describing class specific backscatter characteristics
Analysing and describing the SAR backscatter is fundamental to build the link between the observed SAR backscatter values and the objects on the ground (trees, river ...). This will assist to answer the following type of questions: How did the differently polarised radar (VV vs. VH) waves interact with the objects on the ground to result in the observed VV and VH backscatter values? Which scatter mechanisms (surface, volume and/or double bounce scattering) did cause for example high VH values over forest? First, the backscatter characteristics of the five main classes are to be analysed and described: forest (F), non-forest (NF), plantation (P), built-up (B) and water (W). This is to be done individually for VV, VH and “VVVH backscatter ratio”.
* Maybe we could insert the answers from the other course as the question is the same. Also this question block might be too much!
___



__Question 3.2a:__ What areas show the most change? Is the change equally distributed for all land cover classes?
* Change is mainly visible for plantations and along the shoreline
* Also some parts of forest show changes

__Question 3.2b:__ What do the different colours of the map indicate (e.g.blue, red and yellow patches?)?
* blue = changes happened mainly for the first quantile 2020 or between 2019 and 2020
* red = 2018
* yellow = between 2018 and 2019

__Question 3.2c:__ Why are some areas along the shoreline in a different colour?
* Most likley linked to different water levels in the river. It seems like there has been a major inundation for the first quantile of 2020

__Task__
Create the first quantile median composites of 2018, 2019 and 2020 for Sentinel-1 VH polarization. Follow the code from above and change if necessary.

```java
// Select from each median composite the VV polarisation and rename this corresponding to the time period
var s1VH_median_2018Q1 = s1_median_2018Q1.select('VH').rename('VH_2018Q1')
var s1VH_median_2019Q1 = s1_median_2019Q1.select('VH').rename('VH_2019Q1')
var s1VH_median_2020Q1 = s1_median_2020Q1.select('VH').rename('VH_2020Q1')

// Merge the three images back together, to create one multitemporal image with three bands
var s1VH_multitemporal = s1VH_median_2018Q1.addBands(s1VH_median_2019Q1.addBands(s1VH_median_2020Q1))

// Clip the newly created image to the area of interest
var s1VH_multitemporal = s1VH_multitemporal.clip(aoi)

// Add median composite to the map as an RGB image, using the three bands VV, VH and VV/VH
var visualisation_params = {
        "opacity": 1,
        "bands": ["VH_2018Q1","VH_2019Q1","VH_2020Q1"],
        "min": -25,
        "max": 0,
        "gamma": 1
};

Map.addLayer(s1VH_multitemporal, visualisation_params, 'Sentinel-1 multitemporal composite RGB', true)
```



__Question 3.2d:__ How does the RGB change for the VH-polarization instead of VV? Why are these areas different?
* Some areas within plantations show more distinct colors (cyan colored)
* This is also visible for patches within forests
* especially for areas that show a decrease of volume scattering, which VH is sensible to --> loss of forest cover etc
___

__Question 3.3a:__ Visualize time series for the main five classes (forest, non-forest, plantation, water and built-up) and explain possible differences in VV- and VH-polarizations!

![fig](/answers/answer_sheet_forest_ts.PNG)
<sub> Forest ts </sub>

![fig](/answers/answer_sheet_plantation_ts.PNG)
<sub> Plantation ts </sub>

![fig](/answers/answer_sheet_non_forest_ts.PNG)
<sub> Non - Forest ts </sub>

![fig](/answers/answer_sheet_built_up_ts.PNG)
<sub> Built up ts </sub>

![fig](/answers/answer_sheet_water_ts.PNG)
<sub> Water ts </sub>


__Question 3.3b:__ Which scatter mechanism(s) cause the high VH backscatter over forest (F) when compared to non-forest (NF) areas? Briefly explain the mechanism(s).
* Maybe just use the answer from the answer sheet of the initial AEO as this question is directly out of there

__Question 3.3c:__ Is the signal stable over forest? If not, why does it change?
* Maybe just use the answer from the answer sheet of the initial AEO as this question is directly out of there

__Question 3.3d:__ How do you explain the difference of backscatter level for natural forest and plantation?
* Different structure of oil palm trees and natural forest
* plantation has less volume scatters (less leaves - Increased VH-Polarization) and distinct vertical structures (VV - polarization sensible)

__Question 3.3e:__ Select some areas that show changes in the multitemporal RGB and plot their time series! Describe what you see and can you explain the sudden backscatter increase after changes?

![fig](/answers/answer_sheet_plantation_disturbance_ts.PNG)
<sub> Plantation disturbance ts </sub>

* Drop in both polarizations
* Sometimes a bigger drop in VH than in VV

  * Due to loss of leaves (volume scatters, but remaining tree stamps --> vertical structures)
* Most likley linked to an increased soil moisture --> higher backscatter values
* Sometimes also left tree logs can cause interessting behaivour of backscatter values

__Extra question:__ The analysis was done using C-band SAR images acquired by Sentinel-1 (5.3 cm wavelength). How would the separability of forest and non-forest change for a L-band SAR image (e.g. ALOS Palsar 2) considering its longer wavelength (23.6 cm wavelength). Do you expect the separability to be better or worse? Explain!
* Maybe just use the answer from the answer sheet of the initial AEO as this question is directly out of there __But__ invert it!






