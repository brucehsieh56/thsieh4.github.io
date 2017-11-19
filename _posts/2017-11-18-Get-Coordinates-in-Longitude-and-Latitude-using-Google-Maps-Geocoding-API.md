---
layout: post
title: Get Coordinates in Longitude and Latitude using Google Maps Geocoding API
date: 2017-11-18
category: programming
tags: [python, github-pages, DataVisualization]
---

## Get Coordinates in Longitude and Latitude using Google Maps Geocoding API

Test tags

{% for tag in page.tags %}
  <small><span><a href="/tags/{{ tag }}/">{{ tag }}</a></span></small>
{% endfor %}

---

### Motivation
Although nowaday there are many free websites or database (e.g. [PostGIS](http://postgis.net/)) providing data related to geolocation, lots of the time I still can't find the data with geographical data I want.

Luckily, **Google Maps Geocoding API** has such a generous service for us to get coordinates in longitude and latitude (and other information). In this post, we will show how to get the coordinates for schools (college and university) in **Georgia** from Google Maps Geocoding API using `Python` automatically. After that, we will draw and overlap these schools on a map using `Bokeh`.

---

But before getting our hands dirty, we need an **API key** from Google to make a query for the coordinates. Here is the [quick guide](https://developers.google.com/maps/documentation/geocoding/get-api-key) to get your **API key**.

Once have your secret API key, we can get start!!

### Download the data
- The first data is [here](https://github.com/thsieh4/thsieh4.github.io/blob/master/data/school_GA.csv), it has information including a list of school and the corresponding tuition fee. This one has been manipulated so that we don't need additional complex data cleaning (The origin of the first data comes from [U.S Department of Education](https://collegecost.ed.gov/catc/default.aspx#)).

- Another dataset we used is the [geometry of Georgia](https://github.com/thsieh4/thsieh4.github.io/blob/master/data/geometry_GA.csv). Basically, it's the boundary of Georgia State, we will use it in the last part of this post.

### Get the school name
To get the coordinates for schools in **Georgia**, the first thing is to have a list of **school names**, which is included in our first dataset.

Import the python packages.


```python
import numpy as np
import pandas as pd
import json
from urllib.request import urlopen
```

Here, `urllib` and `json` packages are used to make an **address (coordinate)** request to Google Maps Geocoding API and read `html` file later. 

Let's read the dataset of school first.


```python
# read data
school = pd.read_csv('school_GA.csv')

# have a look
school.head()
```




<div class="scroll">
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sector_name</th>
      <th>institution_name</th>
      <th>2015_16_tuition_fee</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
    </tr>
  </tbody>
</table>
</div>



This data has three interesting columns.
- **sector_name** tells us the type of a school (e.g. *4-year, public*, *4-year, private not-for-profit*, *Less than 2-year, public* and so on).
- **institution_name** has a list of schools that we want to know their coordinates.
- **2015_16_tuition_fee** shows the tuition fee for each school during 2015 and 2016.

Now we have the **school name**, it's time to get their coordinates!!

### Request the coordinates

As [Google Geocoding API](https://developers.google.com/maps/documentation/geocoding/intro) pointed out, to query the coordinates for **1600 Amphitheatre Parkway, Mountain View, CA**, we should use the following format:

<p style="text-align: center;"><code>https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=YOUR_API_KEY</code></p>

---

Let's break the above link or format into three parts:

<p style="text-align: center;">1.<code>https://maps.googleapis.com/maps/api/geocode/json?address=</code></p>

<p style="text-align: center;">2.<code>1600+Amphitheatre+Parkway,+Mountain+View,+CA</code></p>

<p style="text-align: center;">3.<code>&key=YOUR_API_KEY</code></p>

or more specifically,

<p style="text-align: center;">1.<code>https://maps.googleapis.com/maps/api/geocode/json?address=</code></p>

<p style="text-align: center;">2.<code>the place we are interested in</code></p>

<p style="text-align: center;">3.<code>API Key</code></p>
 
1. `https://maps.googleapis.com/maps/api/geocode/json?address=` is the default url for address (coordaintes) query, there is no need to modify it.
2. `the place we are interested in` is the **school name** in our data, but with different format.
3. `API Key` is your secret API key (that you get from [Google](https://developers.google.com/maps/documentation/geocoding/get-api-key)).

---

In detail, `the place we are interested in` should have the format like `1600+Amphitheatre+Parkway,+Mountain+View,+CA`. For example, if we want to get the coordinates of **University of Pittsburgh-Pittsburgh Campus**, we should use

<p style="text-align: center;"><code>University+of+Pittsburgh+Pittsburgh+Campus</code></p>

by inserting `+` between each word (instead of `University of Pittsburgh-Pittsburgh Campus`). 

---

So, to query the coordinates for **University of Pittsburgh-Pittsburgh Campus**, we should use 

`https://maps.googleapis.com/maps/api/geocode/json?address=University+of+Pittsburgh-Pittsburgh+Campus&key=YOUR_API_KEY`

Actually, you can open a browser, paste the above url (with YOUR OWN API KEY!!) and hit enter. You should see a page look like this

![2017Nov17_demo](/2017Nov17_demo)

This is what (longitude and latitude) we are looking for (red rectangle)!! But instead of using a browser and copy and paste an url, we use a more fancy way to query: `Python`.

---

So, let's use `Python` to do some string operation and get the desired url format.


```python
# insert `+`
school['for_query'] = school.apply(lambda row: '+'.join(row['institution_name'].split()), axis=1)

# replace `–` and `-` with `+`
# note that `–` is longer than `-`. Longer `–` will cause some encoding problem 
school['for_query'] = school.apply(lambda row: row['for_query'].replace('–', '+').replace('-', '+'), axis=1)

# have a look
school.head()
```




<div class="scroll">
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sector_name</th>
      <th>institution_name</th>
      <th>2015_16_tuition_fee</th>
      <th>for_query</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
      <td>Georgia+Institute+of+Technology+Main+Campus</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
      <td>University+of+Georgia</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
      <td>Georgia+College+and+State+University</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
      <td>Georgia+State+University</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
      <td>Augusta+University</td>
    </tr>
  </tbody>
</table>
</div>



The last column has the format for querying. Let's get the coordinates for the first school **Georgia Institute of Technology-Main Campus** in our school `DataFrame`.


```python
# use the first school for query
this_school = school.iloc[0,-1]

# put your key here
YOUR_KEY = 'YOUR_API_KEY'

# construct url
school_url = 'https://maps.googleapis.com/maps/api/geocode/json?address=' + this_school + '&key=' + YOUR_KEY

# request, read and decode
txt_json = urlopen(school_url).read().decode()

# load the request `json` as `dict`
json.loads(txt_json)
```




    {'results': [{'address_components': [{'long_name': 'North Ave NW',
         'short_name': 'North Ave NW',
         'types': ['route']},
        {'long_name': 'Atlanta',
         'short_name': 'Atlanta',
         'types': ['locality', 'political']},
        {'long_name': 'Fulton County',
         'short_name': 'Fulton County',
         'types': ['administrative_area_level_2', 'political']},
        {'long_name': 'Georgia',
         'short_name': 'GA',
         'types': ['administrative_area_level_1', 'political']},
        {'long_name': 'United States',
         'short_name': 'US',
         'types': ['country', 'political']},
        {'long_name': '30332', 'short_name': '30332', 'types': ['postal_code']}],
       'formatted_address': 'North Ave NW, Atlanta, GA 30332, USA',
       'geometry': {'location': {'lat': 33.7756178, 'lng': -84.39628499999999},
        'location_type': 'GEOMETRIC_CENTER',
        'viewport': {'northeast': {'lat': 33.7769667802915,
          'lng': -84.3949360197085},
         'southwest': {'lat': 33.7742688197085, 'lng': -84.3976339802915}}},
       'place_id': 'ChIJ40_D64oE9YgRD8XYOj7QKrU',
       'types': ['establishment', 'point_of_interest', 'university']}],
     'status': 'OK'}



Google Geocoding API have offered lots of information for our query. Here,

<p style="text-align: center;"><code>'geometry': {'location': {'lat': 33.7756178, 'lng': -84.39628499999999},</code></p>

has the location information we want. Now we define a function to get coordinates for all of schools.


```python
def get_coord(row, YOUR_KEY):
    """ get coordinates in lng and lat"""

    # construct url
    school_url = 'https://maps.googleapis.com/maps/api/geocode/json?address=' + row + '&key=' + YOUR_KEY
    
    # request a json response
    with urlopen(school_url) as url:
        
        # save the requested json response as `dict`
        google = json.loads(url.read().decode())

    # get coordinates
    coord = google['results'][0]['geometry']['location']
    
    return pd.Series([coord['lng'], coord['lat']])
```

Apply the function `get_coord` to our `school` `DataFrame`.


```python
# get coordinates
school[['lng', 'lat']] = school['for_query'].apply(lambda row: get_coord(row, YOUR_KEY))

# have a look
school.head()
```




<div class="scroll">
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sector_name</th>
      <th>institution_name</th>
      <th>2015_16_tuition_fee</th>
      <th>for_query</th>
      <th>lng</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
      <td>Georgia+Institute+of+Technology+Main+Campus</td>
      <td>-84.396285</td>
      <td>33.775618</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
      <td>University+of+Georgia</td>
      <td>-83.377322</td>
      <td>33.948005</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
      <td>Georgia+College+and+State+University</td>
      <td>-83.231805</td>
      <td>33.079888</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
      <td>Georgia+State+University</td>
      <td>-84.385282</td>
      <td>33.753068</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
      <td>Augusta+University</td>
      <td>-82.022789</td>
      <td>33.475556</td>
    </tr>
  </tbody>
</table>
</div>



With the aid of **Google Maps Geocoding API**, we can get the coordinates we want to know for a place, so cool!!

### Convert coordinate

Although having already the coordinates in longitude and latitude, sometimes when we want to overlay our data on a map ([for case without a map](https://thsieh4.github.io/posts/DataVisualization/2017Nov01)), we need a coordinate transformation. Why is that? Because

> Most map APIs are a **2D plane map**, but longitude and latitude are measured in 3D.

Since geographical system using longitude, latitude and elevation is measured in **3D** aspect, so we need a coordinate transformation to project our longitude and latitude onto a 2D plane. Here is a [great explanation in detail](https://www.uwgb.edu/dutchs/FieldMethods/UTMSystem.htm).

---

So let's define the coordinate transformation function.


```python
import math

def lgn2x(a):
    return a * (math.pi/180) * 6378137

def lat2y(a):
    return math.log(math.tan(a * (math.pi/180)/2 + math.pi/4)) * 6378137
```

The above coordinate transformation is modified according to the [wiki page](http://wiki.openstreetmap.org/wiki/Mercator) of [OpenStreetMap](https://www.openstreetmap.org/). Finally, we apply the functions above to the `lat` and `lng` columns.


```python
# project `longitude` to `x`
school['x'] = school.apply(lambda row: lgn2x(row['lng']), axis=1)

# project `latitude` to `y`
school['y'] = school.apply(lambda row: lat2y(row['lat']), axis=1)

# take a look
school.head()
```




<div class="scroll">
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sector_name</th>
      <th>institution_name</th>
      <th>2015_16_tuition_fee</th>
      <th>for_query</th>
      <th>lng</th>
      <th>lat</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
      <td>Georgia+Institute+of+Technology+Main+Campus</td>
      <td>-84.396285</td>
      <td>33.775618</td>
      <td>-9.394951e+06</td>
      <td>3.998713e+06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
      <td>University+of+Georgia</td>
      <td>-83.377322</td>
      <td>33.948005</td>
      <td>-9.281521e+06</td>
      <td>4.021823e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
      <td>Georgia+College+and+State+University</td>
      <td>-83.231805</td>
      <td>33.079888</td>
      <td>-9.265322e+06</td>
      <td>3.905913e+06</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
      <td>Georgia+State+University</td>
      <td>-84.385282</td>
      <td>33.753068</td>
      <td>-9.393727e+06</td>
      <td>3.995693e+06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
      <td>Augusta+University</td>
      <td>-82.022789</td>
      <td>33.475556</td>
      <td>-9.130735e+06</td>
      <td>3.958597e+06</td>
    </tr>
  </tbody>
</table>
</div>



Now it's a perfect time to overlay this data on a 2D map.

### Overlay data on a map
Import `Bokeh` packages for interactive plot.


```python
from bokeh.plotting import figure, output_file, show
from bokeh.models import ColumnDataSource, HoverTool, PanTool, ResetTool, WheelZoomTool

# import map
from bokeh.tile_providers import STAMEN_TONER, CARTODBPOSITRON_RETINA
```

Bokeh have several differnt types of [map](https://bokeh.pydata.org/en/latest/docs/reference/tile_providers.html) for overlaying. Here we use `CARTODBPOSITRON_RETINA` map for demonstration.


```python
# dot size for scatterplot
school['dot_size'] = (school['2015_16_tuition_fee'])**.4*200

# create a `ColumnDataSource`
source = ColumnDataSource(school)

# customize hover tool
hover = HoverTool(tooltips= [("Univ.", "@institution_name"), ('Tuition', '$@2015_16_tuition_fee')],
                  mode='mouse')

# customize tools
tools = [hover, WheelZoomTool(), PanTool(), ResetTool()]

# create figure
p = figure(tools=tools, x_range=(-9550000,-8900000), y_range=(3500000,4200000),
           x_axis_location=None, y_axis_location=None)

# overlap map
p.add_tile(CARTODBPOSITRON_RETINA)

# add schools
scatter = p.circle('x', 'y', source=source, fill_color='blue', alpha=.5, radius='dot_size')

# add hover effect to school
hover.renderers.append(scatter)

# show the plot
output_file('map_school_GA_CARTODBPOSITRON_RETINA.html')
show(p)
```

<div class="row">
  <div class="col-lg-1">
  </div>
  <div class="col-lg-auto">
    {% include map_school_GA_CARTODBPOSITRON_RETINA.html %}
  </div>
  <div class="col-lg-1">
  </div>
</div>


We can also add the boundary of Georgia so that the graph will look more clear.


```python
# read data
geo = pd.read_csv('geometry_GA.csv')

# data cleaning
geo['lng'] = geo.apply(lambda row: [float(i.split(',')[0]) for i in row.geometry[51:-55].split(' ')], axis=1)
geo['lat'] = geo.apply(lambda row: [float(i.split(',')[1]) for i in row.geometry[51:-55].split(' ')], axis=1)

# transform coordinate
geo_x = [lgn2x(i) for i in geo['lng'][0]]
geo_y = [lat2y(i) for i in geo['lat'][0]]
```

Draw again, but this time we add the boundary and use `STAMEN_TONER` map.


```python
# customize hover tool
hover = HoverTool(tooltips= [("Univ.", "@institution_name"), ('Tuition', '$@2015_16_tuition_fee')],
                  mode='mouse')

# customize tools
tools = [hover, WheelZoomTool(), PanTool(), ResetTool()]

# create figure
p = figure(tools=tools, x_range=(-9550000,-8900000), y_range=(3500000,4200000),
           x_axis_location=None, y_axis_location=None)

# overlap map
p.add_tile(STAMEN_TONER)

# ADD BOUNDARY
p.line(geo_x, geo_y, line_width=2, color='orange')

# add schools
scatter = p.circle('x', 'y', source=source, fill_color='blue', alpha=.5, radius='dot_size')

# add hover effect to school
hover.renderers.append(scatter)

# show the plot
output_file('map_school_GA_STAMEN_TONER.html')
show(p)
```

<div class="row">
  <div class="col-lg-1">
  </div>
  <div class="col-lg-auto">
    {% include map_school_GA_STAMEN_TONER.html %}
  </div>
  <div class="col-lg-1">
  </div>
</div>
