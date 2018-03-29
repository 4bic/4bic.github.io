---
title: Plot 9 charts with a line (or 2) in Python
image: /img/pythonheader.jpeg
comments: true
tags: [python, data, plots, data-visualization, pandas, numpy]
---

Simple and practical applications of plotting and data representation in Python.

<!--more-->

I use [Jupiter notebook][1] alot for the convenience and interactivity it allows.

To get us started, load up necessary python statistical library tools.

```python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt

    #enable jupyter to display matplotlib graphs
    %matplotlib notebook

    #import a standard matplotlib color cycle
    plt.style.use('seaborn-colorblind')
```


## Generate plot data

As part of the preparations, using [numpy][2], generate sample time-series data for the plots.

```python

    np.random.seed(123) #makes the random numbers
```
Then create a dataframe using [pandas][5] with three columns(A, B & C).

<!-- Using [cumsum][3] - it returns the cumulative sums up the specified elements within an axis. -->
```python
    df = pd.DataFrame({'A': np.random.randn(365).cumsum(0),
                       'B': np.random.randn(365).cumsum(0)+20,
                       'C': np.random.randn(365).cumsum(0)-20},
                     index= pd.date_range('1/1/2017', periods=365))
    len(df) #365
    df.head() #check top columns and rows of dataframe
```
and here we go . . .


## Simple line plot


```python

df.plot(); #use semi-colon to supress unwanted details on the plot

```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_7_0.png?raw=true)

## Scatter plot


```python
    df.plot('A', 'B', kind = 'scatter');
```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_9_0.png?raw=true)

## Scatter color map

With Values of `B` as the color range

```python
    df.plot.scatter('A', 'C', c='B',
                    s = df['B'], colormap='viridis');
```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_10_0.png?raw=true)


## Scatter color map with Adjusted Aspect Ratio


```python
    ax = df.plot.scatter('A', 'C', c='B',
                        s = df['B'], colormap='viridis');

    ax.set_aspect('equal')
```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_12_0.png?raw=true)


## Box Plots

```python
    df.plot.box();
```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_13_0.png?raw=true)


## Histogram

```python
df.plot.hist(alpha=0.7);
```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_14_0.png?raw=true)


## KDE CHART
```python
df.plot.kde();
'''kernel density estimation (KDE) is a
way to estimate probability of random variables'''
```
![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_15_1.png?raw=true)


Pandas plotting tools
======

For the demonstrations, I'll use [iris.csv][4], a dataset with details on three flowers species,

These details include;

        - SepalLength
        - SepalWidth
        - PetalLength
        - PetalWidth


Using [pandas][5], we can read the file contents and assign into a dataframe


```python
    iris = pd.read_csv('iris.csv')

    iris.head() #check top columns and rows of dataframe

```



```1. Scatter Matrix```

```python
    pd.tools.plotting.scatter_matrix(iris);
```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_18_0.png?raw=true)


```2. Parallel Coordinates```


```python
plt.figure()
pd.tools.plotting.parallel_coordinates(iris, 'Name');
```


![png](https://raw.githubusercontent.com/4bic/4bic_website/master/images/output_19_0.png?raw=true)



```python

```
[1]: http://jupyter.org
[2]: http://www.numpy.org/
[3]: #
[4]: https://github.com/vincentarelbundock/Rdatasets/blob/master/csv/datasets/iris.csv
[5]: http://pandas.pydata.org/
