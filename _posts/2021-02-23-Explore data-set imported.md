---
layout: post
title: "Explore Data Set"
subtitle: "Import and Explore Data Set"
background: '/img/posts/explore-dataset-important/explore-dataset-important-datriks.jpg'
---

```python
import pandas as pd
import os
print(os.getcwd())
```

    C:\Users\Juvette
    


```python
stats = pd.read_csv('D:\\OneDrive - office365hubs.com\\.Python for data science\\Demographic-Data.csv')
```


```python
stats
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Birth rate</th>
      <th>Internet users</th>
      <th>Income Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>10.244</td>
      <td>78.9</td>
      <td>High income</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>35.253</td>
      <td>5.9</td>
      <td>Low income</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>45.985</td>
      <td>19.1</td>
      <td>Upper middle income</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>12.877</td>
      <td>57.2</td>
      <td>Upper middle income</td>
    </tr>
    <tr>
      <th>4</th>
      <td>United Arab Emirates</td>
      <td>ARE</td>
      <td>11.044</td>
      <td>88.0</td>
      <td>High income</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>190</th>
      <td>Yemen, Rep.</td>
      <td>YEM</td>
      <td>32.947</td>
      <td>20.0</td>
      <td>Lower middle income</td>
    </tr>
    <tr>
      <th>191</th>
      <td>South Africa</td>
      <td>ZAF</td>
      <td>20.850</td>
      <td>46.5</td>
      <td>Upper middle income</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Congo, Dem. Rep.</td>
      <td>COD</td>
      <td>42.394</td>
      <td>2.2</td>
      <td>Low income</td>
    </tr>
    <tr>
      <th>193</th>
      <td>Zambia</td>
      <td>ZMB</td>
      <td>40.471</td>
      <td>15.4</td>
      <td>Lower middle income</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Zimbabwe</td>
      <td>ZWE</td>
      <td>35.715</td>
      <td>18.5</td>
      <td>Low income</td>
    </tr>
  </tbody>
</table>
<p>195 rows × 5 columns</p>
</div>




```python
stats
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Birth rate</th>
      <th>Internet users</th>
      <th>Income Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>10.244</td>
      <td>78.9</td>
      <td>High income</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>35.253</td>
      <td>5.9</td>
      <td>Low income</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>45.985</td>
      <td>19.1</td>
      <td>Upper middle income</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>12.877</td>
      <td>57.2</td>
      <td>Upper middle income</td>
    </tr>
    <tr>
      <th>4</th>
      <td>United Arab Emirates</td>
      <td>ARE</td>
      <td>11.044</td>
      <td>88.0</td>
      <td>High income</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>190</th>
      <td>Yemen, Rep.</td>
      <td>YEM</td>
      <td>32.947</td>
      <td>20.0</td>
      <td>Lower middle income</td>
    </tr>
    <tr>
      <th>191</th>
      <td>South Africa</td>
      <td>ZAF</td>
      <td>20.850</td>
      <td>46.5</td>
      <td>Upper middle income</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Congo, Dem. Rep.</td>
      <td>COD</td>
      <td>42.394</td>
      <td>2.2</td>
      <td>Low income</td>
    </tr>
    <tr>
      <th>193</th>
      <td>Zambia</td>
      <td>ZMB</td>
      <td>40.471</td>
      <td>15.4</td>
      <td>Lower middle income</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Zimbabwe</td>
      <td>ZWE</td>
      <td>35.715</td>
      <td>18.5</td>
      <td>Low income</td>
    </tr>
  </tbody>
</table>
<p>195 rows × 5 columns</p>
</div>




```python
#Number of rows
len(stats)  #195 rows imported
```




    195




```python
# see the columns
```


```python
stats.columns
```




    Index(['Country Name', 'Country Code', 'Birth rate', 'Internet users',
           'Income Group'],
          dtype='object')




```python
len(stats.columns)
```




    5






```python
stats.
```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
