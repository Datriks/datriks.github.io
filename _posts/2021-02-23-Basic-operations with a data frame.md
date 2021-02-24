---
layout: post
title: "Basic Manipulation of a Data Frame in Python"
subtitle: "Data Frame manipulation in Python"
background: '/img/posts/basic-opeartions-with-a-data-frame/python-data-frame-manipulation-datriks.jpg'
---

```python
import pandas as pd
import os
```


```python
stats = pd.read_csv('D:\\OneDrive - office365hubs.com\\.Python for data science\\Demographic-Data.csv')
```


```python
stats.columns = ['CountryName','CountryCode','BirthRate','InternetUsers','IncomeGroup']
```


```python
stats.head()
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
      <th>CountryName</th>
      <th>CountryCode</th>
      <th>BirthRate</th>
      <th>InternetUsers</th>
      <th>IncomeGroup</th>
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
  </tbody>
</table>
</div>




```python
stats[['CountryCode','BirthRate']].head()
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
      <th>CountryCode</th>
      <th>BirthRate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABW</td>
      <td>10.244</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AFG</td>
      <td>35.253</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AGO</td>
      <td>45.985</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALB</td>
      <td>12.877</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ARE</td>
      <td>11.044</td>
    </tr>
  </tbody>
</table>
</div>




```python
stats[['CountryCode','BirthRate','InternetUsers']].head()
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
      <th>CountryCode</th>
      <th>BirthRate</th>
      <th>InternetUsers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABW</td>
      <td>10.244</td>
      <td>78.9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AFG</td>
      <td>35.253</td>
      <td>5.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AGO</td>
      <td>45.985</td>
      <td>19.1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALB</td>
      <td>12.877</td>
      <td>57.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ARE</td>
      <td>11.044</td>
      <td>88.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
stats[['CountryCode','BirthRate','InternetUsers']][4:8].head()
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
      <th>CountryCode</th>
      <th>BirthRate</th>
      <th>InternetUsers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>ARE</td>
      <td>11.044</td>
      <td>88.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ARG</td>
      <td>17.716</td>
      <td>59.9</td>
    </tr>
    <tr>
      <th>6</th>
      <td>ARM</td>
      <td>13.308</td>
      <td>41.9</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ATG</td>
      <td>16.447</td>
      <td>63.4</td>
    </tr>
  </tbody>
</table>
</div>




```python
#mathematical operations:
result = stats.BirthRate * stats.InternetUsers
```


```python
result.head()
```




    0    808.2516
    1    207.9927
    2    878.3135
    3    736.5644
    4    971.8720
    dtype: float64




```python
# Add a column to your data frame
stats['MyCalc'] = stats.BirthRate * stats.InternetUsers
```


```python
stats.head()
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
      <th>CountryName</th>
      <th>CountryCode</th>
      <th>BirthRate</th>
      <th>InternetUsers</th>
      <th>IncomeGroup</th>
      <th>MyCalc</th>
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
      <td>808.2516</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>35.253</td>
      <td>5.9</td>
      <td>Low income</td>
      <td>207.9927</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>45.985</td>
      <td>19.1</td>
      <td>Upper middle income</td>
      <td>878.3135</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>12.877</td>
      <td>57.2</td>
      <td>Upper middle income</td>
      <td>736.5644</td>
    </tr>
    <tr>
      <th>4</th>
      <td>United Arab Emirates</td>
      <td>ARE</td>
      <td>11.044</td>
      <td>88.0</td>
      <td>High income</td>
      <td>971.8720</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Removing a column
stats.head()
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
      <th>CountryName</th>
      <th>CountryCode</th>
      <th>BirthRate</th>
      <th>InternetUsers</th>
      <th>IncomeGroup</th>
      <th>MyCalc</th>
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
      <td>808.2516</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>35.253</td>
      <td>5.9</td>
      <td>Low income</td>
      <td>207.9927</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>45.985</td>
      <td>19.1</td>
      <td>Upper middle income</td>
      <td>878.3135</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>12.877</td>
      <td>57.2</td>
      <td>Upper middle income</td>
      <td>736.5644</td>
    </tr>
    <tr>
      <th>4</th>
      <td>United Arab Emirates</td>
      <td>ARE</td>
      <td>11.044</td>
      <td>88.0</td>
      <td>High income</td>
      <td>971.8720</td>
    </tr>
  </tbody>
</table>
</div>




```python
stats.drop('MyCalc', 1)
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
      <th>CountryName</th>
      <th>CountryCode</th>
      <th>BirthRate</th>
      <th>InternetUsers</th>
      <th>IncomeGroup</th>
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
stats = stats.drop('MyCalc', 1)
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
      <th>CountryName</th>
      <th>CountryCode</th>
      <th>BirthRate</th>
      <th>InternetUsers</th>
      <th>IncomeGroup</th>
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
