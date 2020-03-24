__Question 3.1a:__ What is an ImageCollection and what does it contain?

* An ImageCollection is a stack or time series of images. Therefore it contains the Images themselves and important metadata of the sensor (e.g. acquisition mode etc.) and images (acquisition time etc.)

__Question 3.1b:__ How many Images are in Entire collection of Sentinel-1 images?

* 11 images

__Question 3.1c:__ How many Images of this collection were acquired in March 2020?

* 5 Images (2x 01-03-2020; 1x 08-03-2020; 2x 13-03-2020)
___
__Task__
Now try to select the VH band of “s1_image” yourself and visualize it. (Hint: Do it similar to the VV-polarization and fill out spaces of ??? in the code below)

var s1VH_image = s1_image.select('VH');

Map.addLayer(s1VH_image, {min: -30, max: -5, palette: ['black', 'white']}, 'Sentinel-1 VH image', true);
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
* 


Question 3.1h: What differences do you see in the ratio band compared to the single VV and VH bands and which classes are visually good to separate?

Question 3.1i: Which colours represent the five main land cover classes in the RGB?

Question 3.1j: Why does water (W) appear black in the RGB? What happens to the radar wave when interacting with a flat (non-rough) water surface? It is helpful to make a sketch including the side-looking radar.

Question 3.1k: Why are plantations much greener in the RGB than regular forest?

__Question__
Analysing and describing class specific backscatter characteristics
Analysing and describing the SAR backscatter is fundamental to build the link between the observed SAR backscatter values and the objects on the ground (trees, river ...). This will assist to answer the following type of questions: How did the differently polarised radar (VV vs. VH) waves interact with the objects on the ground to result in the observed VV and VH backscatter values? Which scatter mechanisms (surface, volume and/or double bounce scattering) did cause for example high VH values over forest? First, the backscatter characteristics of the five main classes are to be analysed and described: forest (F), non-forest (NF), plantation (P), built-up (B) and water (W). This is to be done individually for VV, VH and “VVVH backscatter ratio”.
* Maybe we could insert the answers from the other course as the question is the same. Also this question block might be too much!
___



Question 3.2a: What areas show the most change? Is the change equally distributed for all land cover classes?

Question 3.2b: What do the different colours of the map indicate (e.g.blue, red and yellow patches?)?

Question 3.2c: Why are some areas along the shoreline in a different colour?

Task
Create the first quantile median composites of 2018, 2019 and 2020 for Sentinel-1 VH polarization. Follow the code from above and change if necessary.

Question 3.2d: How does the RGB change for the VH-polarization instead of VV? Why are these areas different?
___

Question 3.3a: Visualize time series for the main five classes (forest, non-forest, plantation, water and built-up) and explain possible differences in VV- and VH-polarizations!

Question 3.3b: Which scatter mechanism(s) cause the high VH backscatter over forest (F) when compared to non-forest (NF) areas? Briefly explain the mechanism(s).

Question 3.3c: Is the signal stable over forest? If not, why does it change?

Question 3.3d: How do you explain the difference of backscatter level for natural forest and plantation?

Question 3.3e: Select some areas that show changes in the multitemporal RGB and plot their time series! Describe what you see and can you explain the sudden backscatter increase after changes?

Extra question: The analysis was done using C-band SAR images acquired by Sentinel-1 (5.3 cm wavelength). How would the separability of forest and non-forest change for a L-band SAR image (e.g. ALOS Palsar 2) considering its longer wavelength (23.6 cm wavelength). Do you expect the separability to be better or worse? Explain!





