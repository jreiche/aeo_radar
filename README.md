# Advanced Earth Observation - Radar Exercise
### "Introduction to radar remote sensing for forest monitoring in GEE"

Johannes Reiche, Bart Slagter & Johannes Balling
23-03-2020


### Requirements
* Software
  * Google Earth Engine (GEE)
* Dataset provided via GEE
  * Sentinel-1

### Learning goals
* Introduction to GEE
* Visualisation and RGB generation of SAR images
* Understanding basic SAR backscatter characteristics (VV and VH backscatter values and distributions) over different land cover (forest, plantation, non-forest, built-up and water)
* Linking the backscatter characteristics to particular scatter mechanism on the ground (surface, volume and double bounce scattering). Understanding interactions of the radar wave with objects on the ground (tree, water ...).

### Literature
* Very useful overview of scatter mechanism over forest and water areas: Chapter “Interaction of SAR signal with water bodies” in [Thenkabail (2016): Remote Sensing of Water Resources, Disasters, and Urban Studies. CRC Press.](https://books.google.nl/books?id=Sne9CgAAQBAJ&pg=PA148&lpg=PA148&dq=Interaction+of+SAR+signal+with+water+bodies&source=bl&ots=ZKEgNiz87F&sig=RehZTUjv9Y4GjYxthBRS7x6EvtM&hl=nl&sa=X&ved=0ahUKEwi1v87mye3LAhUEIg8KHaK_BkcQ6AEIHDAA#v=onepage&q=Interaction%20of%20SAR%20signal%20with%20water%20bodies&f=false)

* A useful study to learn about SAR backscatter characteristics and basic SAR change detection over tropical forest  areas: [Ribbes et al. 1997](ftp://ftp.eorc.jaxa.jp/cdroms/EORC-036/pi/16floren.pdf)



# Introduction
## Study area - Riau (Indonesia)
The study area is located in the province of Riau (Indonesia). Riau is located on the east coast of central Sumatra and experiences tropical equatorial climate. This climate is causing a persistent cloud cover throughout the entire year. Primary and secondary dryland and swamp and mangrove forest dominate the natural forest. Riau has the highest forest-cover-loss rates in Indonesia (Margono et al 2014) mainly driven by expansion and conversion to acacia, coconut, rubber plantations and oil palm (Fig.1) (Uryu et al 2008). Oil palm, representing the largest plantation area by species and cover about 3.08 Mha (~34) of all land area (Fig.1) (GlobalForestWatch).


