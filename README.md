# isolines

**Street Networks Isolines and Isochrones**

Create Isolines and Isochrones for street networks.

**isolines** is a Python library for creating street networks isolines (equal-distance) 
and isochrones (equal-time) polygons with just one line of code. It is built on top of Shapely,
geopandas, NetworkX and osmnx.

**isolines** allows you to create isolines:
from OSM data **without any data input** or * **use your own data**: your own network and source locations

## Main Features
   * Generation of isolines/isochroones for areas, segments, and point locations
   * Augmentation of the network for precise isoline/isochroone delineation
   * Based on a **concave hull** algorithm by default (Moreira, Adriano & Santos, Maribel. (2007) [1]) (for walking networks, otherwise convex hull is the default  and concave hull is optional)
   * Source location can be either an address string to be geocoded using OSM Nominatim or a shapely geometry
   * Accepts various graph inputs:  the ```GpdIsolimes``` class excepts edges GeoDataFrames and the
    ```NXIsoliner``` class accepts NetworkX graphs as input and the ```OsmIsolines``` class
    allows you tod downloads a street network from OpenStreetMap (OSM)
    
     #### Other Features
  * Easy built-in visualization
  * Dynamically tweaking isolines/isochroones delineation by re-setting the default
  knn parameter that controls the concave hull heuristic 
  * Dynamically adding isolines/isochroones to an existing instance
  * Extraction of the augmented output graph
  * Generation of nodes and edges GeoDataFrames

## Examples
A repository with a wide variety of examples can be found here: https://github.com/mlichter2/isolines_examples
* Create isolines/isochroones for complex geometries (polygons and linestrings) as well as simple point geometries
If you only want to output a GeoDataFrame of isolines/isochrones 
use the ```isolines``` function:

