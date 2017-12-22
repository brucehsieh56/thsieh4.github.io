---
layout: post
title: Map of Starbucks Locations Worldwide
date: 2017-12-19
category: DataScience
tags: [Python, Bokeh, DataVisualization]
---


---

<div class="row">
  <div class="col-lg-1">
  </div>
  <div class="col-lg-auto">
    {% include 20171219_StarbucksLocationsWorldwide.html %}
  </div>
  <div class="col-lg-1">
  </div>
</div>

---


## Download the data
The dataset comes from [Kaggle](https://www.kaggle.com/starbucks/store-locations).

---

## Read data


```python
# import packages
import numpy as np
import pandas as pd
from bokeh.plotting import figure, output_file, show
from bokeh.models import ColumnDataSource, HoverTool
from bokeh.tile_providers import CARTODBPOSITRON_RETINA

# read data
starbucks = pd.read_csv('directory.csv')

# have a look
starbucks.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 25600 entries, 0 to 25599
    Data columns (total 13 columns):
    Brand             25600 non-null object
    Store Number      25600 non-null object
    Store Name        25600 non-null object
    Ownership Type    25600 non-null object
    Street Address    25598 non-null object
    City              25585 non-null object
    State/Province    25600 non-null object
    Country           25600 non-null object
    Postcode          24078 non-null object
    Phone Number      18739 non-null object
    Timezone          25600 non-null object
    Longitude         25599 non-null float64
    Latitude          25599 non-null float64
    dtypes: float64(2), object(11)
    memory usage: 2.5+ MB



```python
# have a look
starbucks.head()
```




<div class="scroll">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Brand</th>
      <th>Store Number</th>
      <th>Store Name</th>
      <th>Ownership Type</th>
      <th>Street Address</th>
      <th>City</th>
      <th>State/Province</th>
      <th>Country</th>
      <th>Postcode</th>
      <th>Phone Number</th>
      <th>Timezone</th>
      <th>Longitude</th>
      <th>Latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Starbucks</td>
      <td>47370-257954</td>
      <td>Meritxell, 96</td>
      <td>Licensed</td>
      <td>Av. Meritxell, 96</td>
      <td>Andorra la Vella</td>
      <td>7</td>
      <td>AD</td>
      <td>AD500</td>
      <td>376818720</td>
      <td>GMT+1:00 Europe/Andorra</td>
      <td>1.53</td>
      <td>42.51</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Starbucks</td>
      <td>22331-212325</td>
      <td>Ajman Drive Thru</td>
      <td>Licensed</td>
      <td>1 Street 69, Al Jarf</td>
      <td>Ajman</td>
      <td>AJ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>55.47</td>
      <td>25.42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Starbucks</td>
      <td>47089-256771</td>
      <td>Dana Mall</td>
      <td>Licensed</td>
      <td>Sheikh Khalifa Bin Zayed St.</td>
      <td>Ajman</td>
      <td>AJ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>55.47</td>
      <td>25.39</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Starbucks</td>
      <td>22126-218024</td>
      <td>Twofour 54</td>
      <td>Licensed</td>
      <td>Al Salam Street</td>
      <td>Abu Dhabi</td>
      <td>AZ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>54.38</td>
      <td>24.48</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Starbucks</td>
      <td>17127-178586</td>
      <td>Al Ain Tower</td>
      <td>Licensed</td>
      <td>Khaldiya Area, Abu Dhabi Island</td>
      <td>Abu Dhabi</td>
      <td>AZ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>54.54</td>
      <td>24.51</td>
    </tr>
  </tbody>
</table>
</div>



---

## Convert coordinates 


```python
# define functions for coordinate projection
import math

def lgn2x(a):
    return a * (math.pi/180) * 6378137

def lat2y(a):
    return math.log(math.tan(a * (math.pi/180)/2 + math.pi/4)) * 6378137
```


```python
# project `Longitude` and `Latitude` to `x` and `y` plane, respectively.
starbucks['x'] = starbucks.Longitude.apply(lambda row: lgn2x(row))
starbucks['y'] = starbucks.Latitude.apply(lambda row: lat2y(row))

# have a look
starbucks.head()
```




<div class="scroll">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Brand</th>
      <th>Store Number</th>
      <th>Store Name</th>
      <th>Ownership Type</th>
      <th>Street Address</th>
      <th>City</th>
      <th>State/Province</th>
      <th>Country</th>
      <th>Postcode</th>
      <th>Phone Number</th>
      <th>Timezone</th>
      <th>Longitude</th>
      <th>Latitude</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Starbucks</td>
      <td>47370-257954</td>
      <td>Meritxell, 96</td>
      <td>Licensed</td>
      <td>Av. Meritxell, 96</td>
      <td>Andorra la Vella</td>
      <td>7</td>
      <td>AD</td>
      <td>AD500</td>
      <td>376818720</td>
      <td>GMT+1:00 Europe/Andorra</td>
      <td>1.53</td>
      <td>42.51</td>
      <td>1.703188e+05</td>
      <td>5.237684e+06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Starbucks</td>
      <td>22331-212325</td>
      <td>Ajman Drive Thru</td>
      <td>Licensed</td>
      <td>1 Street 69, Al Jarf</td>
      <td>Ajman</td>
      <td>AJ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>55.47</td>
      <td>25.42</td>
      <td>6.174892e+06</td>
      <td>2.927421e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Starbucks</td>
      <td>47089-256771</td>
      <td>Dana Mall</td>
      <td>Licensed</td>
      <td>Sheikh Khalifa Bin Zayed St.</td>
      <td>Ajman</td>
      <td>AJ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>55.47</td>
      <td>25.39</td>
      <td>6.174892e+06</td>
      <td>2.923724e+06</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Starbucks</td>
      <td>22126-218024</td>
      <td>Twofour 54</td>
      <td>Licensed</td>
      <td>Al Salam Street</td>
      <td>Abu Dhabi</td>
      <td>AZ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>54.38</td>
      <td>24.48</td>
      <td>6.053554e+06</td>
      <td>2.812008e+06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Starbucks</td>
      <td>17127-178586</td>
      <td>Al Ain Tower</td>
      <td>Licensed</td>
      <td>Khaldiya Area, Abu Dhabi Island</td>
      <td>Abu Dhabi</td>
      <td>AZ</td>
      <td>AE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GMT+04:00 Asia/Dubai</td>
      <td>54.54</td>
      <td>24.51</td>
      <td>6.071365e+06</td>
      <td>2.815678e+06</td>
    </tr>
  </tbody>
</table>
</div>




```python
# keep interested columns
interested_columns = ['Store Name', 'x', 'y']
df = starbucks[interested_columns]

# rename column
df.columns = ['StoreName', 'x', 'y']

# drop missing value
df = df.dropna(axis=0)

# have a look
df.head()
```




<div class="scroll">
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: right;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>StoreName</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Meritxell, 96</td>
      <td>1.703188e+05</td>
      <td>5.237684e+06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ajman Drive Thru</td>
      <td>6.174892e+06</td>
      <td>2.927421e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dana Mall</td>
      <td>6.174892e+06</td>
      <td>2.923724e+06</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Twofour 54</td>
      <td>6.053554e+06</td>
      <td>2.812008e+06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Al Ain Tower</td>
      <td>6.071365e+06</td>
      <td>2.815678e+06</td>
    </tr>
  </tbody>
</table>
</div>

---

## Reduce the size of data 
Convert the data type of `x` and `y` from `float64` to `float32` for data reduction.


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 25599 entries, 0 to 25599
    Data columns (total 3 columns):
    StoreName    25599 non-null object
    x            25599 non-null float64
    y            25599 non-null float64
    dtypes: float64(2), object(1)
    memory usage: 800.0+ KB



```python
# convert `float64` to `float32`
df.x = starbucks.x.astype('float32')
df.y = starbucks.y.astype('float32')
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 25599 entries, 0 to 25599
    Data columns (total 3 columns):
    StoreName    25599 non-null object
    x            25599 non-null float32
    y            25599 non-null float32
    dtypes: float32(2), object(1)
    memory usage: 600.0+ KB


---

## Draw an interactive map


```python
# create a `ColumnDataSource`
source = ColumnDataSource(df)

# customize hover tool
hover = HoverTool(tooltips= [('Store Name', "@StoreName")],
                  mode='mouse')

# create figure
p = figure(plot_width=720, plot_height=400,
           x_axis_location=None, y_axis_location=None,
           tools=[hover, 'pan', 'wheel_zoom', 'crosshair'], toolbar_location='above',
           title='Starbucks Locations Worldwide')
           
# overlap map
p.add_tile(CARTODBPOSITRON_RETINA)

# add schools
scatter = p.circle('x', 'y', source=source, fill_color='navy', alpha=.1)

# add hover effect to school
hover.renderers.append(scatter)

# show the plot
output_file('2017Dec19_StarbucksLocationsWorldwide.html')
show(p)
```
