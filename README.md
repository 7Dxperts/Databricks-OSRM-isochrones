# OSRM-Isochrones [Databricks only] 

**The OSRM is one of the best routing engines contributions to the OpenStreetMap community.

Over the years whenever we needed to create isochrones for various geospatial use case, we had to rely on other projects that would differ in the output what is reachable compared to OSRM. Then Databricks released Mosaic and how to set-up/deploy OSRM backend sever [hyperlink]. This immediately led us to start thinking how we could add the ability to create isochrones directly using OSRM engine so the community can take more advantage of a scalable data analytics infrastructure like Databricks.

**Isolines** is a Python library written in spark which runs seamlessly in a databricks platform for creating street networks isochrones (equal-time) polygons with just one line of code. It is built on top of Shapely, geopandas and mosaic.

**Isolines** allows you to create isolines:
from OSM data **use your own data**: your own network and source locations

## Main Features
   * Generation of Isochroones for a given point location
   * Source location should be provided with the valid coordinates
 
  #### Other Features
  * Generation of isochrones using osrm server which is deployed local  / databricks.
  * Extract the data and store in the table.
  * Extraction of the augmented output graph
  

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

