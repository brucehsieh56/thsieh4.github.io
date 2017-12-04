---
layout: post
title: Draw Interactive Images with Python package Bokeh
date: 2017-11-30
category: DataScience
tags: [Python, Bokeh, DataVisualization]
---

---

## Motivation
This is a short example using `Bokeh` with its `hovertool` to draw images.

<div class="row">
  <div class="col-lg-1">
  </div>
  <div class="col-lg-auto">
    {% include octupos.html %}
  </div>
  <div class="col-lg-1">
  </div>
</div>

## Download the data
Both of the dataset comes from [Quick Draw Dataset](https://github.com/googlecreativelab/quickdraw-dataset#the-raw-moderated-dataset).
- The first one is the [preprocessed dataset](https://storage.cloud.google.com/quickdraw_dataset/full/numpy_bitmap/octopus.npy?_ga=2.259297098.-1542652997.1508970834), which contains the preprocessed image data of **octopus**.
- The second data is the [raw data](https://storage.cloud.google.com/quickdraw_dataset/full/raw/octopus.ndjson?_ga=2.24642426.-1542652997.1508970834) of octopus stored in [`ndjson`](http://ndjson.org/) format. We use this data to get the country code.

## Read the data
Import the necessary packages.


```python
import numpy as np
import matplotlib.pyplot as plt
from bokeh.layouts import column
from bokeh.models import CustomJS, ColumnDataSource, HoverTool, CategoricalColorMapper
from bokeh.plotting import figure, output_file, show
from bokeh.palettes import Viridis256
```


```python
# load img
img_data = np.load('octopus.npy')

# check dimension
img_data.shape
```




    (150152, 784)



There are 150152 images. Each image has 784 pixels. To draw an image, we have to reshape the image matrix to (28, 28)  from (1, 784).


```python
# draw the last image
plt.matshow(img_data[-1,:].reshape(28,28))
plt.show()
```

<div class="scroll">
  <img src="/figure/2017Nov30_octupos_demo.png" alt="octupos">
</div>


Next, we get the corresponding country code of each image.


```python
# import package
import jsonlines

# count country
with jsonlines.open('octopus.ndjson') as reader:
    country_data = [data['countrycode'] for data in reader]
```

Here, `jsonlines` is the package for reading `ndjson` file.

## Draw the images

The image size of each side is 28 pixels, and we choose the first 50 images for drawing.


```python
img_size, img_num = 28, 50
```


```python
# 1) Figure for selection

# create data points randomly in y-axis
x = np.arange(img_num)
y = np.random.random(size=img_num) * 100

# create a color mapper
country_list = country_data[:img_num]
color_mapper = CategoricalColorMapper(factors=np.unique(country_list), palette=Viridis256[::10])

# create a `ColumnDataSource`
s1 = ColumnDataSource(data=dict(x=x, y=y, country=country_list))

# create the first figure
p1 = figure(plot_width=350, plot_height=150,
            x_axis_location=None, y_axis_location=None,
            tools='crosshair', toolbar_location='above', 
            title='Move Your Mouse Here')

# draw the country as random data points
cr = p1.square('x', 'y', source=s1, 
               color=dict(field='country', transform=color_mapper),
               alpha=0.5, size=10, hover_color='olive', hover_alpha=1.0)
```


```python
# 2) Figure for images

# create a `ColumnDataSource`
img_set = [np.flipud(img.reshape(img_size, img_size)) for img in img_data[:img_num,:]]
img_dict = {'temp': img_set, 'img': [np.zeros((img_size, img_size))] * img_num}
s2 = ColumnDataSource(data=img_dict)

# create the second figure
p2 = figure(plot_width=350, plot_height=350, 
            x_axis_location=None, y_axis_location=None,
            x_range=(0, img_size), y_range=(0, img_size), 
            tools='', toolbar_location='above', 
            title='Octopus Image Shows Here')

# draw the image
p2.image('img', source=s2, x=0, y=0, dw=img_size, dh=img_size)

# callback function for hover tool
def callback(source=s2, window=None):
    
    # get the coordinates of your mouse
    g = cb_data.index['1d'].indices
    
    # assign the image according to the coordinates
    source.data['img'] = [source.data['temp'][g]]
    
    # update the image
    source.change.emit()

# add hover tool
p1.add_tools(HoverTool(tooltips=[("country", "@country")], 
                       callback=CustomJS.from_py_func(callback),
                       renderers=[cr]))

# draw the plot
output_file('octupos.html')
show(column(p1, p2))
```

We define the `callback` function above for our hover tool. Basically, when we move our mouse on the figure, the callback function will obtain the current position of the mouse, then update the octopus image (according to the position of the mouse cursor).

The output interactive graph is shown in the beginning.
