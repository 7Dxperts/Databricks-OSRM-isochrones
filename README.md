# <img src="https://files.training.databricks.com/images/databricks-logo.png" width=40px> OSRM-Isochrones [Databricks only] 

The **[OSRM](http://project-osrm.org/)** is one of the best routing engines contributions to the OpenStreetMap community.

Over the years whenever we needed to create isochrones for various geospatial use case, we had to rely on other projects that would differ in the output what is reachable compared to OSRM. Then Databricks released **[Mosaic](https://www.databricks.com/blog/2022/05/02/high-scale-geospatial-processing-with-mosaic.html)** and 
**[how to set-up/deploy OSRM backend sever](https://www.databricks.com/solutions/accelerators/scalable-route-generation)**. 
This immediately led us to start thinking how we could add the ability to create isochrones directly using OSRM engine so the community can take more advantage of a scalable data analytics infrastructure like Databricks.

**OSRM-Isochrones** for Databricks is a PySpark library for creating geometric polygons that represent the areas that can be reached within a specified amount of time from a given starting point, using a particular mode of transportation and a set of travel conditions. It is built on top of several libraries and more importantly integrated to take advantage of Databricks Mosaic functions such as **[st_makepolygon](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-makepolygon), [st_geomfromwkt](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-geomfromwkt),[st_aswkt](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-aswkt), [st_intersects](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-intersects), [st_astext](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-astext), [st_transform](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-transform), [st_buffer](https://databrickslabs.github.io/mosaic/api/spatial-functions.html#st-buffer), [st_point](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-point), [st_unaryunion](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-unaryunion)**

Current supported modes of transport are Walking, Car, Cycle by the following below parameters

## Required Parameters 
|Variable Names   |	Description           |	MOSCOW     |	Possible Values            |
|--------------   |-----------------------|------------|-------------------------------|
|latitude         |	latitude of the source / destination based on the direction |	Yes|	53.215|
|longitude        |	longitude of the source / destination based on the direction |	Yes|	-0.2151|
|mode             |	Mode of transport	    |Yes	       |driving-car, foot-walking,  cycling-regular|
|direction        |	Type of journey either arrival (to) or departure (from) the location|	Yes	|Arrival/departure (Default Value Departure)|
|duration         |	Isoline range value. For an isochrone time in minutes|	Yes|	Travel duration in minutes|
|osrm_url        |	hostname of the osrm server either deployed within databricks |	Yes|	http://xx.xxx.x.xxx:5000/  (http://ipaddress:portnumber)|


Note: We are planning to release a non-Databricks version of the library that may benefit the wider geospatial community. IsoDistance is also on the roadmap. Please visit the roadmap to see future releases.  

## Main Features
   * Generate isochrones from a point of location with valid coordinates for a certain duration by mode of transport and direction [arrival/departure]
 
  #### Other Features
  * Setup guide how to install OSRM Table API on Databricks .
    # Instructions for enabling the OSRM Table service: -
     ### Prerequisites
      - OSRM Back end server is deployed on the running cluster
      - Init script is attached to the running cluster (follow the create init script from the below link on how to setup init script https://notebooks.databricks.com/notebooks/RCG/Routing/index.html?_ga=2.172845923.1164585516.1677476401-1538949233.1672914660#Routing_2.html)
      - Required PBF files for each of the profiles are placed in the DBFS and has been defined in the init script. Download the required street network data from **(http://7dxperts.com/data)** 
      
  
  * Extracted data for nodes and edges are available in databaricks as a delta tables which can be analysed for further usecases.
  
  

## Examples

![](docs/pics/Isochrones_London.png)
```
(for basic examples see also link pynb file)

*  explain

```python
```
![](docs/pics/Isochrones_Sanfransisco.png)
* The output isolines/isochroones are based on a **

```python
xxxx
```

