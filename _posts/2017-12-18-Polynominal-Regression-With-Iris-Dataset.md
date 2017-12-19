---
layout: post
title: Polynominal Regression with Iris Dataset
date: 2017-12-18
category: DataScience
tags: [Python, Bokeh, DataVisualization, LinearRegression]
---




---

<div class="row">
  <div class="col-lg-1">
  </div>
  <div class="col-lg-auto">
    {% include 20171218_polyfit.html %}
  </div>
  <div class="col-lg-1">
  </div>
</div>

---

## Description
This is a short example using [`polyfit`](https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.polyfit.html) in `Numpy` to illustrate the idea of [polynominal regression](https://en.wikipedia.org/wiki/Polynomial_regression).


---

## Dataset
The dataset we used here is the [Iris dataset](https://en.wikipedia.org/wiki/Iris_flower_data_set) provided by [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/iris).

---

## Read the data

The `read_csv` function in `pandas` can be used directly to read the Iris dataset from the [web](https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data). Let's read the data.


```python
# import packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# read data
iris = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data',
                   names=['sepal_length', 'sepal_width', 'petal_length', 'petal_width', 'species'])

# have a quick look
iris.head()
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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>



The original data has strings `Iris-` in the `species` column. Let's remove these strings.


```python
# remove the `Iris-` strings in `species` column
iris.species = iris.species.apply(lambda row: row[5:])
iris.head()
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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
# check correlation
iris.corr()
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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>sepal_length</th>
      <td>1.000000</td>
      <td>-0.109369</td>
      <td>0.871754</td>
      <td>0.817954</td>
    </tr>
    <tr>
      <th>sepal_width</th>
      <td>-0.109369</td>
      <td>1.000000</td>
      <td>-0.420516</td>
      <td>-0.356544</td>
    </tr>
    <tr>
      <th>petal_length</th>
      <td>0.871754</td>
      <td>-0.420516</td>
      <td>1.000000</td>
      <td>0.962757</td>
    </tr>
    <tr>
      <th>petal_width</th>
      <td>0.817954</td>
      <td>-0.356544</td>
      <td>0.962757</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



**petal_length** is highly positive correlated with **petal_width** (**0.962757**), so let's try to use **petal_length** to predict **petal_width**.

---

## Polynominal fit

Here, we would like to predict the **petal_width** based on the value of **petal_length**. Their relationship can be modeled mathematically as

$$y=w_1x+w_0$$

where

<li>$y$ is the **petal_width** to be predicted.</li>
- $x$ is the **petal_length** we used to predict $y$.
- $w_1$ and $w_0$ are the coefficients we want to find out by fitting.


This is the polynominal regression with degree 1.

---

In general, for degree n, we will use the equation

$$y=w_nx^n+...+w_2x^2+w_1x+w_0$$

to fit the data.

---

As a simple example, let's use `polyfit` with degree 3

$$y=w_3x^3+w_2x^2+w_1x+w_0$$


```python
# get x and y
x, y = iris['petal_length'], iris['petal_width']

# fit a polynomial equation with degree 3 to (x, y)
poly_eq = np.polyfit(x, y, deg=3)
poly_eq
```




    array([-0.02633791,  0.31129374, -0.66987598,  0.63408549])



The above output means

- $w_3$ = -0.02633791
- $w_2$ = 0.31129374
- $w_1$ = -0.66987598
- $w_0$ = 0.63408549

$$y=-0.02633791x^3+0.31129374x^2-0.66987598x+0.63408549$$

---

Let's plot the results.


```python
# specify the x value and get the y value
x_vec = np.linspace(0, 7, 50)
y_vec = np.poly1d(poly_eq)(x_vec)

# plot
plt.figure(figsize=(8,5))
plt.plot(x, y, 'bo', x_vec, y_vec, 'k-')
plt.title('Polynominal Regression with degree 3')
plt.xlabel('Petal length (cm)')
plt.ylabel('Petal width (cm)')
plt.margins(.02)
plt.show()
```


<div class="scroll">
  <img src="/figure/2017Dec18_polyfit_deg3.png">
</div>


## Polynominal fit with different degrees
Finally, we use `Bokeh` to draw an interactive graph for Polynominal fit with various degress.


```python
# specify the x values
x_list = np.linspace(0.5, 7.5, 50)
y_list = []

# get the y values
for deg in range(1, 11):
    
    poly_eq = np.poly1d(np.polyfit(x, y, deg=deg))
    
    y_list.append(poly_eq(x_list))

# create a `DataFrame`     
df = pd.DataFrame(y_list).T

# add column names
df.columns = ['degree_' + str(i) for i in range(1,11)]

# add x values for plotting
df['x'] = x_list

# add one default column y for plotting
df['y'] = df['degree_3']

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
      <th>degree_1</th>
      <th>degree_2</th>
      <th>degree_3</th>
      <th>degree_4</th>
      <th>degree_5</th>
      <th>degree_6</th>
      <th>degree_7</th>
      <th>degree_8</th>
      <th>degree_9</th>
      <th>degree_10</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.158304</td>
      <td>-0.174304</td>
      <td>0.373679</td>
      <td>-0.107127</td>
      <td>0.047196</td>
      <td>2.107955</td>
      <td>-0.007857</td>
      <td>-1.476037</td>
      <td>-0.114018</td>
      <td>0.732161</td>
      <td>0.500000</td>
      <td>0.373679</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.098816</td>
      <td>-0.112558</td>
      <td>0.325101</td>
      <td>-0.039109</td>
      <td>0.066459</td>
      <td>1.330754</td>
      <td>0.127552</td>
      <td>-0.623480</td>
      <td>0.000813</td>
      <td>0.361275</td>
      <td>0.642857</td>
      <td>0.325101</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.039328</td>
      <td>-0.050921</td>
      <td>0.287155</td>
      <td>0.021939</td>
      <td>0.090177</td>
      <td>0.800122</td>
      <td>0.185966</td>
      <td>-0.149065</td>
      <td>0.090450</td>
      <td>0.216253</td>
      <td>0.785714</td>
      <td>0.287155</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.020161</td>
      <td>0.010606</td>
      <td>0.259382</td>
      <td>0.077162</td>
      <td>0.117697</td>
      <td>0.462385</td>
      <td>0.202875</td>
      <td>0.087223</td>
      <td>0.150551</td>
      <td>0.178794</td>
      <td>0.928571</td>
      <td>0.259382</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.079649</td>
      <td>0.072024</td>
      <td>0.241319</td>
      <td>0.127637</td>
      <td>0.148470</td>
      <td>0.272008</td>
      <td>0.203398</td>
      <td>0.186174</td>
      <td>0.186229</td>
      <td>0.184248</td>
      <td>1.071429</td>
      <td>0.241319</td>
    </tr>
  </tbody>
</table>
</div>



Draw the interactive plot.


```python
# import package
from bokeh.plotting import figure, show, output_file
from bokeh.layouts import widgetbox, column
from bokeh.models import ColumnDataSource, CustomJS, Slider, HoverTool, CategoricalColorMapper

# 1) Initialize the figure
# customize hover tool
hover = HoverTool(mode='mouse', 
                  tooltips=[('species', '@species'),
                            ('petal length (cm)', '@petal_length'),
                            ('petal width (cm)', '@petal_width'),
                            ('sepal length (cm)', '@sepal_length'),
                            ('sepal width (cm)', '@sepal_width')])

# create a figure
plot = figure(plot_width=350, plot_height=350, title='Polynominal Fit',
              tools=[hover, 'pan', 'wheel_zoom', 'reset'], toolbar_location='above')


# 2) Draw the original iris dataset
# build color mapper
mapper = CategoricalColorMapper(factors=['setosa', 'virginica', 'versicolor'],
                                palette=['orange', 'navy', 'gray'])

# create `ColumnDataSource`
s1 = ColumnDataSource(iris)

# scatterplot the original iris dataset 
scatter = plot.circle('petal_length', 'petal_width', source=s1, legend='species',
                      color={'field': 'species', 'transform': mapper}, size=9, 
                      fill_alpha=.3, hover_alpha=1,)

# add hover effect to the scatterplot
hover.renderers.append(scatter)


# 3) Draw the fitted polynomial line
# create `ColumnDataSource`
s2 = ColumnDataSource(df)

# add the polynomial line
plot.line('x', 'y', source=s2, color='firebrick', line_width=5, line_alpha=.5)

# define a function for our interactive slider
def callback(source=s2, window=None):
    """ an event function to update the fitted polynomial line"""
    
    # get the current slider value
    slider_value = cb_obj.value
    
    # update the y value
    source.data['y'] = source.data['degree_' + str(slider_value)]
    source.change.emit()

# create a slider
slider = Slider(title='Degree of the Polynominal Equation',
                start=1, end=10, step=1, value=3,
                callback=CustomJS.from_py_func(callback))


# 4) Output the figure
# set figure properties
plot.legend.location = 'top_left'
plot.xaxis.axis_label = 'Petal length (cm)'
plot.yaxis.axis_label = 'Petal width (cm)'

# create a widgetbox layout
layout = column(widgetbox(slider), plot)

# show the plot
output_file('20171218_polyfit.html')
show(layout)
```

The final interactive plot is shown in the beginning.
