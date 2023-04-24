# <img src="https://files.training.databricks.com/images/databricks-logo.png" width=40px> OSRM-Isochrones [Databricks only] 

The **[OSRM](http://project-osrm.org/)** is one of the best routing engines contributions to the OpenStreetMap community.

Over the years whenever we needed to create isochrones for various geospatial use case, we had to rely on other projects that would differ in the output what is reachable compared to OSRM. Then Databricks released **[Mosaic](https://www.databricks.com/blog/2022/05/02/high-scale-geospatial-processing-with-mosaic.html)** and 
**[how to set-up/deploy OSRM backend sever](https://www.databricks.com/solutions/accelerators/scalable-route-generation)**. 
This immediately led us to start thinking how we could add the ability to create isochrones directly using OSRM engine so the community can take more advantage of a scalable data analytics infrastructure like Databricks.

**OSRM-Isochrones** for Databricks is a PySpark library for creating geometric polygons that represent the areas that can be reached within a specified amount of time from a given starting point, using a particular mode of transportation and a set of travel conditions. It is built on top of several libraries and more importantly integrated to take advantage of Databricks Mosaic functions such as **[st_makepolygon](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-makepolygon), [st_geomfromwkt](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-geomfromwkt),[st_aswkt](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-aswkt), [st_intersects](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-intersects), [st_astext](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-astext), [st_transform](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-transform), [st_buffer](https://databrickslabs.github.io/mosaic/api/spatial-functions.html#st-buffer), [st_point](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-point), [st_unaryunion](https://databrickslabs.github.io/mosaic/api/geometry-constructors.html#st-unaryunion)**

Current supported modes of transport are Driving using car where as Walking and Cycling are provided as experimental using the following below parameters

## Required Parameters 
|Variable Names   |	Description           |	Mandatory (Yes/No)     |	Possible Values            |
|--------------   |-----------------------|------------|-------------------------------|
|latitude         |	latitude of the source / destination based on the direction |	Yes|	53.215|
|longitude        |	longitude of the source / destination based on the direction |	Yes|	-0.2151|
|mode             |	Mode of transport	    |Yes	       |driving-car, foot-walking,  cycling-regular|
|direction        |	Type of journey either arrival (to) or departure (from) the location|	Yes	|Arrival/departure (Default Value Departure)|
|duration         |	Isoline range value. For an isochrone time in minutes|	Yes|	Travel duration in minutes|
|osrm_url        |	hostname of the osrm server either deployed within databricks |	Yes|	http://xx.xxx.x.xxx:5000/  (http://ipaddress:portnumber)|


Note: We are planning to release a non-Databricks version of the library that may benefit the wider geospatial community but to be confirmed. IsoDistance is also on the roadmap. Please visit the roadmap to see future releases(https://github.com/7Dxperts/OSRM-isochrones/discussions/categories/updates-and-roadmaps).  

## Main Features
   * Generate isochrones from a point of location with valid coordinates for a certain duration using a mode of transport and travel direction [arrival/departure]
   * The resulting nodes and edges data (reachable street network) will automatically be stored in separate Delta tables. These data could be consumed for other data analysis purposed.
   * The traversed data for all reachable locations from the origin (routes) is also stored in Delta table with duration in minutes.
 
  #### Prerequisites 
  Databricks has provided a comprehensive guide how to deploy OSRM back end server (https://www.databricks.com/solutions/accelerators/scalable-route-generation) but it doesnt include how to enable the table API required for this project. Therefore we have provided instruction below how to accomplish this
  
  Setup guide how to install OSRM Table API on Databricks
  
   ### Instructions for enabling the OSRM Table service: 

   * OSRM Back end server is already deployed on the running cluster.
   * A Init script is attached to the running cluster (follow the create init script from the below link on how to setup init script https://notebooks.databricks.com/notebooks/RCG/Routing/index.html?_ga=2.172845923.1164585516.1677476401-1538949233.1672914660#Routing_2.html)
   * Required PBF files for each of the profiles are placed in the DBFS and has been defined in the init script. Download the processed street network data based on required profile from **(https://7dxperts.com/network-data-landing)** 

   ### Follow the below steps to test and enable table API on your OSRM setup:
   Run the below code to extract the Host IP of the deployed OSRM servers, note there will more than one when deployed in multi node cluster.
   ```
      import requests
      import subprocess
      myRDD = sc.parallelize(range(sc.defaultParallelism))
      ip_addresses = set( 
                          sc.runJob(
                                    myRDD, 
                                    lambda _: [subprocess.run(['hostname','-I'], capture_output=True).stdout.decode('utf-8').strip()] # run hostname -I on each executor
                                    )
                         )
       print(ip_addresses)
   ```
   Run the below sample code to test if the OSRM routing API is working as expected. 
   ```
        responses = []

        # for each worker ip address
        for ip in ip_addresses:

          # get a response from the osrm backend server
          resp = requests.get(f'http://{ip}:5000/route/v1/driving/-81.333495,27.004571;-81.906328,28.881845;-81.906328,28.881845').text
          responses += [(ip, resp)]

        # display responses generated by each worker
        display(
          pd.DataFrame(responses, columns=['ip','response'])
          )
   ```
   Once you find routing service is working without any error, now let’s trial table service in OSRM, we use the above IP address that is been used for routing and by giving source and multiple target locations in databricks notebook. 
   First, we need to import the necessary libraries and packages:
   
   ```
    import json
    import requests
   ```
   Then, we define the source and target locations:
   
   ```
    source = '52.503033,13.424086'
    targets = ['52.507033,13.425884', '52.508033,13.426884', '52.509033,13.427884']
   ```
   Next, we create the API call for the OSRM table service with the source and targets:
   
   ```
    url = 'http://{}:5000/table/v1/driving/{}'.format(IP, source)
    params = {
        'destinations': ';'.join(targets)
    }
   ```
   Then, we make the request to the API and get the response:
   ```
    response = requests.get(url, params=params)
   ```
   Finally, we can parse the response and extract the data:
   ```
    data = json.loads(response.text)
    durations = data['durations']
   ```

   The durations will contain the duration (in seconds) of the journey from the source to the target locations.

   Once the table service is working without any errors. Now, pass the HTTPS link as osrm_link to the isochrone function. Example and the syntax of how the link is to be sent as a parameter to osrm_link for function isochrones are as follows: -
   ```
        Syntax: - http://ip_address:port_number/
        Example: - http://10.191.0.96:5000/
   ```

  
  
  

## Examples

*  Sample code for calling the Isochrones library
``` import eqolines eq
    latitude       = 51.531856
    longiude       = -0.106573
    mode           = 'driving-car'
    durations       = 5
    osrm_url       = "http://xx.xxx.xx.xxxx:5000/"
    final_response = eq.generate_isochrone(latitude, longiude , mode , durations, osrm_url)
```

![](isolines/examples/Isochrones_London.png)

![](isolines/examples/Isochrones_Sanfransisco.png)
```

Known Issues:
OSRM (Open Source Routing Machine) is an open-source software package used for route planning and navigation. When using OSRM, one potential issue that can arise is a timeout error. This error occurs when the OSRM server takes too long to respond to a request from a client.

When OSRM times our please restart the OSRM server and rerun the isochrones again. Current we are trying to find the cause and a workaround for resolving timeout when generating isochrones with the OSRM server.
