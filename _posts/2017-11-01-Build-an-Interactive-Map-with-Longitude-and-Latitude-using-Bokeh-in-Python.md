---
layout: default
title: Build an Interactive Map with Longitude and Latitude using Bokeh in Python
date: 2017-11-01
category: DataScience
tags: [Python, DataVisualization]
---

## Build an Interactive Map with Longitude and Latitude using Bokeh in Python

---

### Motivation
As a data enthusiast, occasionally the data we analyzed contains information related to specific places. For example, the [population density for each State in the United States](https://en.wikipedia.org/wiki/List_of_U.S._states_and_territories_by_population). In this situation, it would be easier for us to tell the interesting story we have found to our readers using a geographical map. Practically, a static plot of data on a map is good enough to do this job. 

> However, an interactive graph would be even more appealing and informative to our readers!

Although there are many free map API (e.g. [OpenStreetMap](https://www.openstreetmap.org)) and maps with limited data usage (e.g. [Google Maps API](https://developers.google.com/maps/)) available, it is usually not a simple task to combine or overlay our findings on these maps, especially when we want to create an interactive graph! 

In this post, we will use Python with [Bokeh](https://bokeh.pydata.org/en/latest/) to draw an interactive map without using any of geographical map APIs. Instead, what we need here are the coordinates (a collection of longitude and latitude) of the boundary of a place.

### Download the data
- The *original* datasets we used can be found at [Here](https://fusiontables.google.com/DataSource?docid=1Ewx86_38dldGMt-fSKjBELX_VF7kuVz8cbXQ0A#rows:id=1) and [Here](https://quickstats.nass.usda.gov/). 
- The code and data we used can be found in [this repository](https://github.com/thsieh4/thsieh4.github.io/tree/master/code/2017Nov01).
- Detailed code (include data cleaning) written in Jupyter Notebook is [Here](https://github.com/thsieh4/thsieh4.github.io/blob/master/code/2017Nov01/BuildAnInteractiveMapWithLongitudeAndLatitudeUsingBokehInPython.ipynb).

### Draw a map based on longitude and latitude
Already got the data? Cool! Now the first thing is to import the necessary libraries.

```python
# import library
import numpy as np
import pandas as pd

from bokeh.plotting import figure
from bokeh.io import output_file, show
from bokeh.models import ColumnDataSource, HoverTool, LogColorMapper
from bokeh.palettes import RdYlBu5
```

Let's load the data and take a quick look.

``` python
# read geographical data
df = pd.read_csv('geo_bounary_NC.csv')
df.head()
```

<div class="scroll">
  <img src="/figure/2017Nov01_df_head.png" alt="df.head()">
</div>
  
The data we are interested in are the columns **County Name** and **geometry**. The column **geometry** has the boundary information of a **County**, which will be used to darw the map using [Bokeh patches](https://bokeh.pydata.org/en/latest/docs/reference/models/glyphs/patches.html). Let's manipulate the **geometry** column (**longitude** and **latitude**) a little bit and rearrange them in a way that Bokeh `patches` favors.

```python
# make a copy and extract interested columns
geo = df.copy()[['County Name', 'geometry']]

# extract a set of longitude (x) and latitude (y)
geo['x'] = geo.apply(lambda row: [float(i.split(',')[0]) for i in row.geometry[51:-55].split(' ')], axis=1)
geo['y'] = geo.apply(lambda row: [float(i.split(',')[1]) for i in row.geometry[51:-55].split(' ')], axis=1)

# rename and extract interested the column
geo.columns = ['County', 'geometry', 'x', 'y']
geo = geo[['County', 'x', 'y']]

# take a look
geo.head()
```

<div class="scroll">
  <img src="/figure/2017Nov01_head_geo.png" alt="geo.head()">
</div>

After a series of data cleaning, we have the desired data format for drawing an interactive map. Specifically, for county **Alamance** (the first row), we have
- longtitude `[-79.54207, -79.54208, -79.54203, -79.54201, ...` in **x** column
- latitude `[35.88714, 35.8894, 35.8936, 35.89584, 35.8996...` in **y** column

These are the boundary of **Alamance**.

---

Now that we have the information of interest, it is time for us draw an interactive map.

```python
# create ColumnDataSource
source = ColumnDataSource(geo)

# build a hover tool to show the county name and coordinates
hover = HoverTool(tooltips= [('County Name', '@County'),
                             ('(Long, Lat)', '($x, $y)')],
                  mode='mouse')

# create a figure
p = figure(title="Counties in North Carolina", plot_width=650, plot_height=300,
           tools=[hover, 'reset', 'save'], toolbar_location="above",
           x_axis_location=None, y_axis_location=None)

# remove grids
p.grid.grid_line_color = None

# plot the map
p.patches('x', 'y', source=source, line_color='black', line_width=0.8, fill_alpha=0.4)

# output html file and show it
output_file('county_NC.html')
show(p)
```

<div class="row">
  <div class="col-lg-1">
  </div>
  <div class="col-lg-auto">
    {% include county_NC.html %}
  </div>
  <div class="col-lg-1">
  </div>
</div>

Now, we can move our mouse cursor on the interactive map above. It will tell us which county and the coordiantes we are pointed at in real time!!

### Add more imformation
Of course we can do more than that! Let's try to merge another dataset ([the cattle sold in USD](https://github.com/thsieh4/thsieh4.github.io/blob/master/code/2017Nov01/cattle_sold_NC_2012.csv)) and make our map more attractive and informative.

```
# read cattle data 
df = pd.read_csv('cattle_sold_NC_2012.csv')

# extracted interested columns
df = df[['Year', 'County', 'Data Item', 'Domain Category', 'Value']]

# copy original df
cattle = df.copy()

# specify missing values and remove them
cattle.Value = cattle.Value.str.lstrip().replace('(D)', np.nan)
cattle = cattle.dropna(axis=0)

# convert str to float in 'Value' column
cattle.Value = cattle.Value.apply(lambda row: np.int(''.join(row.split(','))))

# convert county name to 'title' from 'uppercase'
cattle.County = cattle.County.str.title()

# deal with counties who have multiple values in $
cattle = cattle.groupby(['County'])[['Value']].sum()

# merge geo data and cattle sell
nc = pd.merge(geo, cattle, left_on='County', right_index=True, how='left')

# take a look
nc.head()
```

<div class="scroll">
  <img src="/figure/2017Nov01_head_nc.png" alt="nc.head()">
</div>

We have one additional column **Value**. This is the cattle sold in each county in the unit of USD. 

---

Time to make an interactive plot of this new data!!

```python
# create color mapper
RdYlBu5.reverse()
mapper = LogColorMapper(palette=RdYlBu5)

# create ColumnDataSource
source = ColumnDataSource(nc)

# build a hover tool to show the county name and cattle sold
hover = HoverTool(tooltips= [("County Name", "@County"),
                             ('Cattle Sold', '$@Value')],
                  mode='mouse')

# create a figure
p = figure(title="Cattle Sold in 2012 in North Carolina",
           tools=[hover, 'reset', 'save'], toolbar_location="above",
           x_axis_location=None, y_axis_location=None, plot_width=650, plot_height=300)

# remove grid
p.grid.grid_line_color = None

# darw the map
p.patches('x', 'y', source=source,
          line_color="black", line_width=0.8,
          fill_alpha=0.4, fill_color={'field': 'Value', 'transform': mapper},)

# output html file and show it
output_file('cattle_sold_NC_2012.html')
show(p)
```

<div class="row">
  <div class="col-lg-1">
  </div>
  <div class="col-lg-auto">
    {% include cattle_sold_NC_2012.html %}
  </div>
  <div class="col-lg-1">
  </div>
</div>

The above interactive map shows the cattle sold in $USD for most counties in North Carolina, USA in 2012. Notice that some counties have missing values colored in gray.
