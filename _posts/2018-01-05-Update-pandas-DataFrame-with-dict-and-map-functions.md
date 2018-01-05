---
layout: post
title: Update data in pandas DataFrame with dict and map functions
date: 2018-01-05
category: DataScience
tags: [Python, Note]
---

---
This is a simple example to change or update the data values by ultilizing `dict()` and `map()` functinos in Python.

---

### 1. Read Iris dataset


```python
# import pandas library
import pandas as pd

# read data
iris = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data',
                   names=['sepal_length', 'sepal_width', 'petal_length', 'petal_width', 'species'])

# have a quick look
iris.head()
```




<div class="scroll">
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



### 2. Check species


```python
# check distinct species of iris dataset
iris.species.unique().tolist()
```




    ['Iris-setosa', 'Iris-versicolor', 'Iris-virginica']



### 3. Update data values


```python
# create a `dict` mapper
dict_mapper = {'Iris-setosa': 'A', 
               'Iris-versicolor': 'B', 
               'Iris-virginica': 'C'}

# update species column
iris.species = iris.species.map(dict_mapper)

# have a look
iris.head()
```




<div class="scroll">
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
      <td>A</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>A</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>A</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>A</td>
    </tr>
  </tbody>
</table>
</div>



**Iris-setosa**, **Iris-versicolor** and **Iris-virginica** in **species** column have been changed to **A**, **B** and **C**, respectively.
