---
layout: default
title: Get Coordinates in Longitude and Latitude using Google Maps Geocoding API
permalink: /posts/DataCollection/2017Nov17
---

## Get Coordinates in Longitude and Latitude using Google Maps Geocoding API

## Motivation
Although there are many 

However, it is not easy to find the geo location for a data. 

Luckily, **Google Maps Geocoding API** has such a generous service for us to get geo-coordinates in longitude and latitude. In this post, we are trying to get the coordinates (longtitude and latitude) for schools in **Georgia** from Google Maps Geocoding API using Python automatically.

---

But before starting, we need an **API key** to make a query for the coordinates. Google has a [quick guide](https://developers.google.com/maps/documentation/geocoding/get-api-key) to get an **API key**.

Once having your API key, we can get start!!

## Data
The original dataset can be found [here](https://collegecost.ed.gov/catc/default.aspx#).

## Get coordinates 
Let's import the python packages.


```python
import numpy as np
import pandas as pd
import json
from urllib.request import urlopen
```

The `urllib` and `json` packages are used to make a request to Google Maps Geocoding API. Let's read the data.


```python
# file name
file_name = 'CATClists2015.xlsx'

# read excel
xml = pd.ExcelFile(file_name)

# check sheet
xml.sheet_names
```




    ['Introduction',
     'Tuition',
     'NetPrice',
     'TuitionChange',
     'NetPriceChange',
     'Variable Values',
     'Stats_abef',
     'Stats_cd']



There are several sheets available. In this post, we are only interested in **school name**. Actually, `Tuition`, `NetPrice`, `TuitionChange` and `NetPriceChange` have **school name**, so we can just use any one of them. Let's choose `Tuition` sheet.


```python
# get `Tuition` sheet
df = xml.parse('Tuition')

# have a look
df.head()
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
      <th>Sector</th>
      <th>Sector name</th>
      <th>UnitID</th>
      <th>OPEID</th>
      <th>Name of institution</th>
      <th>State</th>
      <th>2015-16 Tuition and fees</th>
      <th>List A: High tuition and fee indicator</th>
      <th>List E: Low tuition and fee indicator</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4-year, public</td>
      <td>231624</td>
      <td>370500</td>
      <td>College of William and Mary</td>
      <td>VA</td>
      <td>19372</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>4-year, public</td>
      <td>215293</td>
      <td>337900</td>
      <td>University of Pittsburgh-Pittsburgh Campus</td>
      <td>PA</td>
      <td>18192</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>4-year, public</td>
      <td>214777</td>
      <td>332900</td>
      <td>Pennsylvania State University-Main Campus</td>
      <td>PA</td>
      <td>17514</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>4-year, public</td>
      <td>126775</td>
      <td>134800</td>
      <td>Colorado School of Mines</td>
      <td>CO</td>
      <td>17353</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>4-year, public</td>
      <td>183044</td>
      <td>258900</td>
      <td>University of New Hampshire-Main Campus</td>
      <td>NH</td>
      <td>16986</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



This data has various interesting columns. 
- **Sector name** tells us the type of a school (e.g. *4-year, public*, *4-year, private not-for-profit*, *Less than 2-year, public* and so on). 
- The column **Name of institution** has the schools that we want to get their coordinates.
- **2015-16 Tuition and fees** list the tuition.

---

Let's focus on the **4-year** schools in **Georgia**.


```python
# filter 
school = df[(df['State']=='GA') & (df['Sector name'].str.contains('4-year'))]

# choose interesting columns
school = school[['Sector name', 'Name of institution', '2015-16 Tuition and fees']]

# rename the columns
school.columns = ['sector_name', 'school_name', 'tuition_fee']

# shape
school.shape
```




    (78, 3)



We have **78** 4-year schools in Georgia.


```python
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
      <th>school_name</th>
      <th>tuition_fee</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
    </tr>
    <tr>
      <th>95</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
    </tr>
    <tr>
      <th>211</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
    </tr>
    <tr>
      <th>222</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
    </tr>
    <tr>
      <th>261</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
    </tr>
  </tbody>
</table>
</div>



Now we have the school name, it's time to get their coordinates!!

## Google search

As [Google Geocoding API](https://developers.google.com/maps/documentation/geocoding/intro) pointed out, to obtain a coordinates for **1600 Amphitheatre Parkway, Mountain View, CA**, we can use the following format:

<p style="text-align: center;">`https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=YOUR_API_KEY`</p>

---

Specifically, we should have the format:

<p style="text-align: center;">`https://maps.googleapis.com/maps/api/geocode/json?address=` + `the place we are interested in` + `API Key`</p>

for our coordinate query. 
- `the place we are interested in` is the **school** in our data. 
- `API Key` is the key for your own query.

---

In detail, `the place we are interested in` should have the format like `1600+Amphitheatre+Parkway,+Mountain+View,+CA`. For example, if we want to get the coordinates of **University of Pittsburgh-Pittsburgh Campus**, we should use

<p style="text-align: center;">`University+of+Pittsburgh-Pittsburgh+Campus` </p>

instead of `University of Pittsburgh-Pittsburgh Campus` (insert `+` between words). The final url for a coordinate query should look like this

`https://maps.googleapis.com/maps/api/geocode/json?address=University+of+Pittsburgh-Pittsburgh+Campus&key=YOUR_API_KEY`

Actually, you can open a browser and paste the above link (with YOUR API KEY!!), and you should see the page like this

![demo](/demo.png)

This is what we are looking for!! But instead of using a browser and copying and pasting an url, we use more fancy way to query: `Python`.

---

Now, let's do some string operation to our **school_name** and get the format mentioned above.


```python
# insert `+`
school['for_google_search'] = school.apply(lambda row: '+'.join(row['school_name'].split()), axis=1)

# replace `–` and `-` with `+`
# note that `–` is longer than `-`, and `–` will cause some encoding problem 
school['for_google_search'] = school.apply(lambda row: row['for_google_search'].replace('–', '+').replace('-', '+'), axis=1)

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
      <th>school_name</th>
      <th>tuition_fee</th>
      <th>for_google_search</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
      <td>Georgia+Institute+of+Technology+Main+Campus</td>
    </tr>
    <tr>
      <th>95</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
      <td>University+of+Georgia</td>
    </tr>
    <tr>
      <th>211</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
      <td>Georgia+College+and+State+University</td>
    </tr>
    <tr>
      <th>222</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
      <td>Georgia+State+University</td>
    </tr>
    <tr>
      <th>261</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
      <td>Augusta+University</td>
    </tr>
  </tbody>
</table>
</div>



The last column has the school format for querying. Let's get the coordinates for the first school in our school `DataFrame`.


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

<p style="text-align: center;">`'geometry': {'location': {'lat': 33.7756178, 'lng': -84.39628499999999},`</p>

is what we are looking for. Now let's define a function to get coordinates for all of schools.


```python
def get_coord(row, YOUR_KEY):
    """ get coordinates """

    # construct url
    school_url = 'https://maps.googleapis.com/maps/api/geocode/json?address=' + row + '&key=' + YOUR_KEY
    
    # request a json response
    with urlopen(school_url) as url:
        
        # save the requested json response as `dict`
        google = json.loads(url.read().decode())

    # get coordinates
    coord = google['results'][0]['geometry']['location']
    
    return pd.Series([coord['lat'], coord['lng']])
```

Apply our function `get_coord`.


```python
# get coordinates
school[['lat', 'lng']] = school['for_google_search'].apply(lambda row: get_coord(row, YOUR_KEY))

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
      <th>school_name</th>
      <th>tuition_fee</th>
      <th>for_google_search</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
      <td>Georgia+Institute+of+Technology+Main+Campus</td>
      <td>33.775618</td>
      <td>-84.396285</td>
    </tr>
    <tr>
      <th>95</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
      <td>University+of+Georgia</td>
      <td>33.948005</td>
      <td>-83.377322</td>
    </tr>
    <tr>
      <th>211</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
      <td>Georgia+College+and+State+University</td>
      <td>33.079888</td>
      <td>-83.231805</td>
    </tr>
    <tr>
      <th>222</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
      <td>Georgia+State+University</td>
      <td>33.753068</td>
      <td>-84.385282</td>
    </tr>
    <tr>
      <th>261</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
      <td>Augusta+University</td>
      <td>33.475556</td>
      <td>-82.022789</td>
    </tr>
  </tbody>
</table>
</div>



With the aid of **Google Maps Geocoding API**, we can get the coordinates we want for a place, cool!!

## Convert coordinate

Already having the coordinates in longitude and latitude, however, sometimes when we want to overlay our data on a map ([for case without a map](https://thsieh4.github.io/posts/DataVisualization/2017Nov01)), we need a coordinate transformation. Why is that? Because

> Most map APIs are a **2D plane** map.

And the geographical system using longitude, latitude and elevation is measured in 3D aspect, so we need a coordinate transformation to project our longitude and latitude onto a 2D plane. Here is a [great explanation](https://www.uwgb.edu/dutchs/FieldMethods/UTMSystem.htm).

---

So let's define the coordinate transformation function.


```python
import math

def lon2x(a):
    return a * (math.pi/180) * 6378137

def lat2y(a):
    return math.log(math.tan(a * (math.pi/180)/2 + math.pi/4)) * 6378137
```

The above coordinate transformation is modified according to the [wiki page](http://wiki.openstreetmap.org/wiki/Mercator) of [OpenStreetMap](https://www.openstreetmap.org/). Finally, we apply the functions above to the `lat` and `lng` columns. 


```python
# project `longitude` to `x`
school['x'] = school.apply(lambda row: lon2x(row['lng']), axis=1)

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
      <th>school_name</th>
      <th>tuition_fee</th>
      <th>for_google_search</th>
      <th>lat</th>
      <th>lng</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>4-year, public</td>
      <td>Georgia Institute of Technology-Main Campus</td>
      <td>12204</td>
      <td>Georgia+Institute+of+Technology+Main+Campus</td>
      <td>33.775618</td>
      <td>-84.396285</td>
      <td>-9.394951e+06</td>
      <td>3.998713e+06</td>
    </tr>
    <tr>
      <th>95</th>
      <td>4-year, public</td>
      <td>University of Georgia</td>
      <td>11622</td>
      <td>University+of+Georgia</td>
      <td>33.948005</td>
      <td>-83.377322</td>
      <td>-9.281521e+06</td>
      <td>4.021823e+06</td>
    </tr>
    <tr>
      <th>211</th>
      <td>4-year, public</td>
      <td>Georgia College and State University</td>
      <td>9170</td>
      <td>Georgia+College+and+State+University</td>
      <td>33.079888</td>
      <td>-83.231805</td>
      <td>-9.265322e+06</td>
      <td>3.905913e+06</td>
    </tr>
    <tr>
      <th>222</th>
      <td>4-year, public</td>
      <td>Georgia State University</td>
      <td>8974</td>
      <td>Georgia+State+University</td>
      <td>33.753068</td>
      <td>-84.385282</td>
      <td>-9.393727e+06</td>
      <td>3.995693e+06</td>
    </tr>
    <tr>
      <th>261</th>
      <td>4-year, public</td>
      <td>Augusta University</td>
      <td>8282</td>
      <td>Augusta+University</td>
      <td>33.475556</td>
      <td>-82.022789</td>
      <td>-9.130735e+06</td>
      <td>3.958597e+06</td>
    </tr>
  </tbody>
</table>
</div>



Once having `x` and `y` coordinates, we can overlay this data on a 2D map.


```python
# school.to_csv('school.csv', index_label=False)
```
