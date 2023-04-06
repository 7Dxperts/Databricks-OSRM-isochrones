# reverse the line geometry coordinate sequence
98
df2['geometry'] = df['geometry'].apply(lambda x: LineString(x.coords[::-1]))
99
df = pd.concat([df, df2]).reset_index(drop = True)
100
tiger = il.GpdIsolines('Prospect Park, Brooklyn, NYC, USA',
101
                            edges = df,
102
                            network_type = 'walk',
103
                            metric = 'time',
104
                            values=[3, 6, 8, 16, 20],
105
                            edge_idcol = 'TLID', 
106
                            fromcol = 'TNIDF',
107
                            tocol = 'TNIDT',
108
                            sample= 400,
109
                            knn = 50
110
                                 )
111
​
112
tiger.plot_isolines(figsize = (10, 10))
113
```
114
![](docs/figs/tiger.png)
115
​
116
(examples for using different data sources:
117
 https://github.com/mlichter2/isolines_examples/blob/master/examples/06_using_different_input_sources.ipynb,
118
 
119
 https://github.com/mlichter2/isolines_examples/blob/master/examples/07_example_using_US_Census_TIGER_dataset.ipynb)
120
​
121
​
122
* Add isolines to an existing instance
123
​
124
```python
125
from shapely.geometry import Point
126
isochrones = il.OsmIsolines(Point(179370.985,664422.488),
127
                                 network_type = 'walk',
128
                                 values=[500, 1500, 2500, 3500],
129
                                 crs = 2039,
130
                                 knn = 25)
131
isochrones.plot_isolines(figsize = (10, 10))
132
```
133
![](docs/figs/habima1.png)
134
```python
135
isochrones.add_isolines([1000, 2000, 3000, 4000])
136
isochrones.plot_isolines()
137
```
138
![](docs/figs/habima2.png)
139
​
140
(examples: https://github.com/mlichter2/isolines_examples/blob/master/examples/02_adding_isolines_and_isochrones.ipynb)
141
​
142
* Extract nodes and edges GeoDataFrames, NetworkX graphs
143
```buildoutcfg
144
nodes = isochrones.get_nodes()
145
edges = isochrones.get_edges()
146
G = isochrones.get_graph()
147
```
148
​
149
1. Moreira, Adriano & Santos, Maribel. (2007). Concave hull: A k-nearest neighbours approach for the computation of the region occupied by a set of points.. 61-68. 
