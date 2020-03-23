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
