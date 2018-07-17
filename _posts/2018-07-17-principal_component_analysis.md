
# Principal Component Analysis

PCA is just a transformation of your data and attempts to find out what features explain the most variance in your data

#### Import Libraries


```jupyter
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns


%matplotlib inline
```

#### Load Data

Let's work with the cancer data set since it had so many features.



```jupyter
from sklearn.datasets import load_breast_cancer
```


```jupyter
cancer = load_breast_cancer()
```


```jupyter
cancer.keys()
```




    dict_keys(['data', 'target', 'target_names', 'DESCR', 'feature_names'])




```jupyter
df = pd.DataFrame(cancer['data'],columns=cancer['feature_names'])
```


```jupyter
df.head(3)
```




<div>
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
      <th>mean radius</th>
      <th>mean texture</th>
      <th>mean perimeter</th>
      <th>mean area</th>
      <th>mean smoothness</th>
      <th>mean compactness</th>
      <th>mean concavity</th>
      <th>mean concave points</th>
      <th>mean symmetry</th>
      <th>mean fractal dimension</th>
      <th>...</th>
      <th>worst radius</th>
      <th>worst texture</th>
      <th>worst perimeter</th>
      <th>worst area</th>
      <th>worst smoothness</th>
      <th>worst compactness</th>
      <th>worst concavity</th>
      <th>worst concave points</th>
      <th>worst symmetry</th>
      <th>worst fractal dimension</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>17.99</td>
      <td>10.38</td>
      <td>122.8</td>
      <td>1001.0</td>
      <td>0.11840</td>
      <td>0.27760</td>
      <td>0.3001</td>
      <td>0.14710</td>
      <td>0.2419</td>
      <td>0.07871</td>
      <td>...</td>
      <td>25.38</td>
      <td>17.33</td>
      <td>184.6</td>
      <td>2019.0</td>
      <td>0.1622</td>
      <td>0.6656</td>
      <td>0.7119</td>
      <td>0.2654</td>
      <td>0.4601</td>
      <td>0.11890</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20.57</td>
      <td>17.77</td>
      <td>132.9</td>
      <td>1326.0</td>
      <td>0.08474</td>
      <td>0.07864</td>
      <td>0.0869</td>
      <td>0.07017</td>
      <td>0.1812</td>
      <td>0.05667</td>
      <td>...</td>
      <td>24.99</td>
      <td>23.41</td>
      <td>158.8</td>
      <td>1956.0</td>
      <td>0.1238</td>
      <td>0.1866</td>
      <td>0.2416</td>
      <td>0.1860</td>
      <td>0.2750</td>
      <td>0.08902</td>
    </tr>
    <tr>
      <th>2</th>
      <td>19.69</td>
      <td>21.25</td>
      <td>130.0</td>
      <td>1203.0</td>
      <td>0.10960</td>
      <td>0.15990</td>
      <td>0.1974</td>
      <td>0.12790</td>
      <td>0.2069</td>
      <td>0.05999</td>
      <td>...</td>
      <td>23.57</td>
      <td>25.53</td>
      <td>152.5</td>
      <td>1709.0</td>
      <td>0.1444</td>
      <td>0.4245</td>
      <td>0.4504</td>
      <td>0.2430</td>
      <td>0.3613</td>
      <td>0.08758</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 30 columns</p>
</div>



## PCA Visualization

As it is difficult to visualize high dimensional data, we can use PCA to find the first two principal components, and visualize the data in this new, two-dimensional space, with a single scatter-plot.

Before this though, we'll need to scale our data so that each feature has a single unit variance.



```jupyter
from sklearn.preprocessing import StandardScaler
```


```jupyter
scaler = StandardScaler()
```


```jupyter
scaler.fit(df)
```




    StandardScaler(copy=True, with_mean=True, with_std=True)




```jupyter
scaled_data = scaler.transform(df)
```

#### PCA with Scikit Learn

uses a very similar process to other preprocessing functions that come with SciKit Learn.

    - Instantiate a PCA object,
    - find the principal components using the fit method,
    - Apply the rotation and dimensionality reduction by calling transform().

We can also specify how many components we want to keep when creating the PCA object.



```jupyter
from sklearn.decomposition import PCA
```


```jupyter
pca = PCA(n_components=2)
```


```jupyter
pca.fit(scaled_data)
```




    PCA(copy=True, iterated_power='auto', n_components=2, random_state=None,
      svd_solver='auto', tol=0.0, whiten=False)



**transform this data to its first 2 principal components.**


```jupyter
x_pca = pca.transform(scaled_data)
```


```jupyter
scaled_data.shape
```




    (569, 30)




```jupyter
x_pca.shape
```




    (569, 2)



**We've reduced 30 dimensions to just 2! Let's plot these two dimensions out!**


```jupyter
plt.figure(figsize=(10,6))
plt.scatter(x_pca[:,0],x_pca[:,1],c=cancer['target'],cmap='coolwarm' )
plt.xlabel('First Principal Component')
plt.ylabel('Second Principal Component')
```




    Text(0,0.5,'Second Principal Component')




![png](output_23_1.png)


Clearly by using these two components easily separate these two classes.



## Interpreting the components

Unfortunately, with this great power of dimensionality reduction, comes the cost of being able to easily understand what these components represent.

The components correspond to combinations of the original features, the components themselves are stored as an attribute of the fitted PCA object:


```jupyter
pca.components_
```




    array([[ 0.21890244,  0.10372458,  0.22753729,  0.22099499,  0.14258969,
             0.23928535,  0.25840048,  0.26085376,  0.13816696,  0.06436335,
             0.20597878,  0.01742803,  0.21132592,  0.20286964,  0.01453145,
             0.17039345,  0.15358979,  0.1834174 ,  0.04249842,  0.10256832,
             0.22799663,  0.10446933,  0.23663968,  0.22487053,  0.12795256,
             0.21009588,  0.22876753,  0.25088597,  0.12290456,  0.13178394],
           [-0.23385713, -0.05970609, -0.21518136, -0.23107671,  0.18611302,
             0.15189161,  0.06016536, -0.0347675 ,  0.19034877,  0.36657547,
            -0.10555215,  0.08997968, -0.08945723, -0.15229263,  0.20443045,
             0.2327159 ,  0.19720728,  0.13032156,  0.183848  ,  0.28009203,
            -0.21986638, -0.0454673 , -0.19987843, -0.21935186,  0.17230435,
             0.14359317,  0.09796411, -0.00825724,  0.14188335,  0.27533947]])



Each row above represents a principal component, and each column relates back to the original features.



# Visualize the relationship using a heatmap

The relation exist between PCA and original features


```jupyter
df_comp = pd.DataFrame(pca.components_,columns=cancer['feature_names'])
```


```jupyter
df_comp
```




<div>
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
      <th>mean radius</th>
      <th>mean texture</th>
      <th>mean perimeter</th>
      <th>mean area</th>
      <th>mean smoothness</th>
      <th>mean compactness</th>
      <th>mean concavity</th>
      <th>mean concave points</th>
      <th>mean symmetry</th>
      <th>mean fractal dimension</th>
      <th>...</th>
      <th>worst radius</th>
      <th>worst texture</th>
      <th>worst perimeter</th>
      <th>worst area</th>
      <th>worst smoothness</th>
      <th>worst compactness</th>
      <th>worst concavity</th>
      <th>worst concave points</th>
      <th>worst symmetry</th>
      <th>worst fractal dimension</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.218902</td>
      <td>0.103725</td>
      <td>0.227537</td>
      <td>0.220995</td>
      <td>0.142590</td>
      <td>0.239285</td>
      <td>0.258400</td>
      <td>0.260854</td>
      <td>0.138167</td>
      <td>0.064363</td>
      <td>...</td>
      <td>0.227997</td>
      <td>0.104469</td>
      <td>0.236640</td>
      <td>0.224871</td>
      <td>0.127953</td>
      <td>0.210096</td>
      <td>0.228768</td>
      <td>0.250886</td>
      <td>0.122905</td>
      <td>0.131784</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.233857</td>
      <td>-0.059706</td>
      <td>-0.215181</td>
      <td>-0.231077</td>
      <td>0.186113</td>
      <td>0.151892</td>
      <td>0.060165</td>
      <td>-0.034768</td>
      <td>0.190349</td>
      <td>0.366575</td>
      <td>...</td>
      <td>-0.219866</td>
      <td>-0.045467</td>
      <td>-0.199878</td>
      <td>-0.219352</td>
      <td>0.172304</td>
      <td>0.143593</td>
      <td>0.097964</td>
      <td>-0.008257</td>
      <td>0.141883</td>
      <td>0.275339</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 30 columns</p>
</div>




```jupyter
plt.figure(figsize=(10,6))
sns.heatmap(df_comp,cmap='plasma')
# closer to yellow the higher the co-relation
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a10b30128>




![png](output_29_1.png)


This heatmap and the color bar basically represent the correlation between the various feature and the principal component itself.
