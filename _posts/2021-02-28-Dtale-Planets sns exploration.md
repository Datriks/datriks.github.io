---
layout: post
title: "Python Dtale Data Evaluation"
subtitle: "Fast Data Reporting in Python"
background: '/img/posts/dtale-planets/planets-dtale-python-datriks.jpg'
---


```python
import seaborn as sns
import dtale
```


```python
print(sns.get_dataset_names())
```

    ['anagrams', 'anscombe', 'attention', 'brain_networks', 'car_crashes', 'diamonds', 'dots', 'exercise', 'flights', 'fmri', 'gammas', 'geyser', 'iris', 'mpg', 'penguins', 'planets', 'tips', 'titanic']
    


```python
df = sns.load_dataset('planets')
df.head()
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
      <th>method</th>
      <th>number</th>
      <th>orbital_period</th>
      <th>mass</th>
      <th>distance</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>269.300</td>
      <td>7.10</td>
      <td>77.40</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>874.774</td>
      <td>2.21</td>
      <td>56.95</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>763.000</td>
      <td>2.60</td>
      <td>19.84</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>326.030</td>
      <td>19.40</td>
      <td>110.62</td>
      <td>2007</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>516.220</td>
      <td>10.50</td>
      <td>119.47</td>
      <td>2009</td>
    </tr>
  </tbody>
</table>
</div>




```python
report = dtale.show(df)
report
```

    http://DESKTOP-5T93O8P:40000/dtale/main/1
    It looks like this data may have already been loaded to D-Tale based on shape and column names. Here is URL of the data that seems to match it:
    
    None
    
    If you still want to load this data please use the following command:
    
    dtale.show(df, ignore_duplicate=True)
    


```python
s = df['distance']
q1 = s.quantile(0.25)
q3 = s.quantile(0.75)
iqr = q3 - q1
iqr_lower = q1 - 1.5 * iqr
iqr_upper = q3 + 1.5 * iqr
outliers = dict(s[(s < iqr_lower) | (s > iqr_upper)])
```


```python
outliers
```




    {39: 500.0,
     40: 500.0,
     93: 680.0,
     100: 460.0,
     102: 560.0,
     103: 1150.0,
     104: 1060.0,
     105: 1340.0,
     106: 840.0,
     107: 920.0,
     108: 870.0,
     109: 770.0,
     110: 1230.0,
     111: 600.0,
     158: 480.0,
     185: 535.0,
     187: 411.0,
     189: 642.0,
     190: 501.0,
     192: 447.0,
     193: 542.0,
     197: 453.0,
     654: 550.0,
     658: 1330.0,
     659: 650.0,
     660: 650.0,
     661: 650.0,
     664: 613.0,
     665: 613.0,
     666: 613.0,
     667: 613.0,
     668: 613.0,
     669: 613.0,
     670: 600.0,
     672: 980.0,
     675: 800.0,
     679: 2119.0,
     680: 2119.0,
     688: 800.0,
     689: 800.0,
     690: 1200.0,
     691: 1200.0,
     701: 1400.0,
     702: 1400.0,
     703: 1400.0,
     704: 1400.0,
     705: 1400.0,
     706: 2100.0,
     707: 2100.0,
     718: 1499.0,
     719: 1645.0,
     720: 470.0,
     721: 470.0,
     725: 600.0,
     726: 1200.0,
     727: 2700.0,
     728: 770.0,
     732: 1950.0,
     733: 2250.0,
     735: 855.0,
     736: 855.0,
     737: 1500.0,
     738: 1500.0,
     776: 1000.0,
     780: 1107.0,
     781: 1107.0,
     787: 1180.0,
     788: 1180.0,
     789: 800.0,
     790: 1330.0,
     791: 1140.0,
     793: 570.0,
     818: 780.0,
     819: 780.0,
     820: 780.0,
     821: 780.0,
     822: 780.0,
     823: 780.0,
     824: 780.0,
     825: 1030.0,
     898: 1056.0,
     901: 2000.0,
     905: 3600.0,
     909: 2300.0,
     910: 2800.0,
     911: 7720.0,
     912: 7560.0,
     924: 2570.0,
     925: 4080.0,
     926: 4080.0,
     927: 1760.0,
     928: 4970.0,
     933: 600.0,
     934: 2500.0,
     938: 1200.0,
     945: 1200.0,
     951: 8500.0,
     952: 8500.0,
     955: 492.0,
     959: 408.0,
     974: 400.0,
     990: 450.0,
     1008: 455.0,
     1011: 400.0,
     1012: 480.0,
     1023: 550.0,
     1026: 470.0,
     1028: 3200.0}




```python

import numpy as np
import pandas as pd

if isinstance(df, (pd.DatetimeIndex, pd.MultiIndex)):
	df = df.to_frame(index=False)

# remove any pre-existing indices for ease of use in the D-Tale code, but this is not required
df = df.reset_index().drop('index', axis=1, errors='ignore')
df.columns = [str(c) for c in df.columns]  # update columns to strings in case they are numbers

chart = np.histogram(df[~pd.isnull(df['distance'])][['distance']], bins=20)
# main statistics
stats = df['distance'].describe().to_frame().T
```


```python
chart
```




    (array([704,  49,  21,  12,   7,   3,   3,   1,   1,   2,   0,   1,   0,
              0,   0,   0,   0,   1,   1,   2], dtype=int64),
     array([1.3500000e+00, 4.2628250e+02, 8.5121500e+02, 1.2761475e+03,
            1.7010800e+03, 2.1260125e+03, 2.5509450e+03, 2.9758775e+03,
            3.4008100e+03, 3.8257425e+03, 4.2506750e+03, 4.6756075e+03,
            5.1005400e+03, 5.5254725e+03, 5.9504050e+03, 6.3753375e+03,
            6.8002700e+03, 7.2252025e+03, 7.6501350e+03, 8.0750675e+03,
            8.5000000e+03]))




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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>distance</th>
      <td>808.0</td>
      <td>264.069282</td>
      <td>733.116493</td>
      <td>1.35</td>
      <td>32.56</td>
      <td>55.25</td>
      <td>178.5</td>
      <td>8500.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
if isinstance(df, (pd.DatetimeIndex, pd.MultiIndex)):
	df = df.to_frame(index=False)

# remove any pre-existing indices for ease of use in the D-Tale code, but this is not required
df = df.reset_index().drop('index', axis=1, errors='ignore')
df.columns = [str(c) for c in df.columns]  # update columns to strings in case they are numbers

chart = df.groupby('number')[['distance']].agg(['count', 'mean'])
```


```python
chart
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">distance</th>
    </tr>
    <tr>
      <th></th>
      <th>count</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>number</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>513</td>
      <td>291.743606</td>
    </tr>
    <tr>
      <th>2</th>
      <td>158</td>
      <td>257.691646</td>
    </tr>
    <tr>
      <th>3</th>
      <td>66</td>
      <td>125.906364</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>15.932000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>20</td>
      <td>243.382500</td>
    </tr>
    <tr>
      <th>6</th>
      <td>24</td>
      <td>168.005000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>780.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.corr()
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
      <th>number</th>
      <th>orbital_period</th>
      <th>mass</th>
      <th>distance</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>number</th>
      <td>1.000000</td>
      <td>-0.012570</td>
      <td>-0.241429</td>
      <td>-0.033638</td>
      <td>0.147849</td>
    </tr>
    <tr>
      <th>orbital_period</th>
      <td>-0.012570</td>
      <td>1.000000</td>
      <td>0.173725</td>
      <td>-0.034365</td>
      <td>-0.032333</td>
    </tr>
    <tr>
      <th>mass</th>
      <td>-0.241429</td>
      <td>0.173725</td>
      <td>1.000000</td>
      <td>0.274082</td>
      <td>-0.123787</td>
    </tr>
    <tr>
      <th>distance</th>
      <td>-0.033638</td>
      <td>-0.034365</td>
      <td>0.274082</td>
      <td>1.000000</td>
      <td>0.178922</td>
    </tr>
    <tr>
      <th>year</th>
      <td>0.147849</td>
      <td>-0.032333</td>
      <td>-0.123787</td>
      <td>0.178922</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



    Exception on /dtale/charts/_dash-update-component [POST]
    Traceback (most recent call last):
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 2447, in wsgi_app
        response = self.full_dispatch_request()
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1952, in full_dispatch_request
        rv = self.handle_user_exception(e)
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1821, in handle_user_exception
        reraise(exc_type, exc_value, tb)
      File "C:\Python\Python38\lib\site-packages\flask\_compat.py", line 39, in reraise
        raise value
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1950, in full_dispatch_request
        rv = self.dispatch_request()
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1936, in dispatch_request
        return self.view_functions[rule.endpoint](**req.view_args)
      File "C:\Python\Python38\lib\site-packages\dash\dash.py", line 1076, in dispatch
        response.set_data(func(*args, outputs_list=outputs_list))
      File "C:\Python\Python38\lib\site-packages\dash\dash.py", line 1007, in add_context
        output_value = func(*args, **kwargs)  # %% callback invoked %%
      File "C:\Python\Python38\lib\site-packages\dtale\dash_application\views.py", line 851, in group_values
        group_vals = build_group_val_options(group_vals, group_cols)
      File "C:\Python\Python38\lib\site-packages\dtale\dash_application\layout\layout.py", line 711, in build_group_val_options
        group_vals = find_group_vals(df, group_cols)
      File "C:\Python\Python38\lib\site-packages\dtale\charts\utils.py", line 651, in find_group_vals
        group_vals, _ = retrieve_chart_data(df, group_cols)
      File "C:\Python\Python38\lib\site-packages\dtale\charts\utils.py", line 246, in retrieve_chart_data
        all_data = pd.concat(all_data, axis=1)
      File "C:\Python\Python38\lib\site-packages\pandas\core\reshape\concat.py", line 271, in concat
        op = _Concatenator(
      File "C:\Python\Python38\lib\site-packages\pandas\core\reshape\concat.py", line 329, in __init__
        raise ValueError("No objects to concatenate")
    ValueError: No objects to concatenate
    

    2020-12-06 09:47:58,880 - ERROR    - Exception on /dtale/charts/_dash-update-component [POST]
    Traceback (most recent call last):
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 2447, in wsgi_app
        response = self.full_dispatch_request()
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1952, in full_dispatch_request
        rv = self.handle_user_exception(e)
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1821, in handle_user_exception
        reraise(exc_type, exc_value, tb)
      File "C:\Python\Python38\lib\site-packages\flask\_compat.py", line 39, in reraise
        raise value
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1950, in full_dispatch_request
        rv = self.dispatch_request()
      File "C:\Python\Python38\lib\site-packages\flask\app.py", line 1936, in dispatch_request
        return self.view_functions[rule.endpoint](**req.view_args)
      File "C:\Python\Python38\lib\site-packages\dash\dash.py", line 1076, in dispatch
        response.set_data(func(*args, outputs_list=outputs_list))
      File "C:\Python\Python38\lib\site-packages\dash\dash.py", line 1007, in add_context
        output_value = func(*args, **kwargs)  # %% callback invoked %%
      File "C:\Python\Python38\lib\site-packages\dtale\dash_application\views.py", line 851, in group_values
        group_vals = build_group_val_options(group_vals, group_cols)
      File "C:\Python\Python38\lib\site-packages\dtale\dash_application\layout\layout.py", line 711, in build_group_val_options
        group_vals = find_group_vals(df, group_cols)
      File "C:\Python\Python38\lib\site-packages\dtale\charts\utils.py", line 651, in find_group_vals
        group_vals, _ = retrieve_chart_data(df, group_cols)
      File "C:\Python\Python38\lib\site-packages\dtale\charts\utils.py", line 246, in retrieve_chart_data
        all_data = pd.concat(all_data, axis=1)
      File "C:\Python\Python38\lib\site-packages\pandas\core\reshape\concat.py", line 271, in concat
        op = _Concatenator(
      File "C:\Python\Python38\lib\site-packages\pandas\core\reshape\concat.py", line 329, in __init__
        raise ValueError("No objects to concatenate")
    ValueError: No objects to concatenate
    


```python

```
