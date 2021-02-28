---
layout: post
title: "Python Dtale Car Crashes Data evaluation"
subtitle: "Car Crashes Data Reporting in Python with Dtale"
background: '/img/posts/car-crashes/car-crash-dtale-python-datriks.jpg'
---

# Dtale-sns-car-crashes

```python
pip install --upgrade pip
```

    Collecting pip
      Downloading pip-20.3.1-py2.py3-none-any.whl (1.5 MB)
    Installing collected packages: pip
      Attempting uninstall: pip
        Found existing installation: pip 20.2.4
        Uninstalling pip-20.2.4:
          Successfully uninstalled pip-20.2.4
    Successfully installed pip-20.3.1
    Note: you may need to restart the kernel to use updated packages.
    


```python
import seaborn as sns
import dtale
import numpy as np
import pandas as pd 
```


```python
print(sns.get_dataset_names())
```

    ['anagrams', 'anscombe', 'attention', 'brain_networks', 'car_crashes', 'diamonds', 'dots', 'exercise', 'flights', 'fmri', 'gammas', 'geyser', 'iris', 'mpg', 'penguins', 'planets', 'tips', 'titanic']
    


```python
df = sns.load_dataset('car_crashes')
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
      <th>total</th>
      <th>speeding</th>
      <th>alcohol</th>
      <th>not_distracted</th>
      <th>no_previous</th>
      <th>ins_premium</th>
      <th>ins_losses</th>
      <th>abbrev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.8</td>
      <td>7.332</td>
      <td>5.640</td>
      <td>18.048</td>
      <td>15.040</td>
      <td>784.55</td>
      <td>145.08</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>1</th>
      <td>18.1</td>
      <td>7.421</td>
      <td>4.525</td>
      <td>16.290</td>
      <td>17.014</td>
      <td>1053.48</td>
      <td>133.93</td>
      <td>AK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.6</td>
      <td>6.510</td>
      <td>5.208</td>
      <td>15.624</td>
      <td>17.856</td>
      <td>899.47</td>
      <td>110.35</td>
      <td>AZ</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22.4</td>
      <td>4.032</td>
      <td>5.824</td>
      <td>21.056</td>
      <td>21.280</td>
      <td>827.34</td>
      <td>142.39</td>
      <td>AR</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12.0</td>
      <td>4.200</td>
      <td>3.360</td>
      <td>10.920</td>
      <td>10.680</td>
      <td>878.41</td>
      <td>165.63</td>
      <td>CA</td>
    </tr>
  </tbody>
</table>
</div>




```python
report = dtale.show(df)
report
```



<iframe
    width="100%"
    height="475"
    src="http://DESKTOP-5T93O8P:40001/dtale/iframe/1"
    frameborder="0"
    allowfullscreen
></iframe>






    




```python
# DISCLAIMER: 'df' refers to the data you passed in when calling 'dtale.show'

if isinstance(df, (pd.DatetimeIndex, pd.MultiIndex)):
	df = df.to_frame(index=False)

# remove any pre-existing indices for ease of use in the D-Tale code, but this is not required
df = df.reset_index().drop('index', axis=1, errors='ignore')
df.columns = [str(c) for c in df.columns]  # update columns to strings in case they are numbers

chart = np.histogram(df[~pd.isnull(df['total'])][['total']], bins=20)
# main statistics
stats = df['total'].describe().to_frame().T
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
      <th>total</th>
      <td>51.0</td>
      <td>15.790196</td>
      <td>4.122002</td>
      <td>5.9</td>
      <td>12.75</td>
      <td>15.6</td>
      <td>18.5</td>
      <td>23.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# DISCLAIMER: 'df' refers to the data you passed in when calling 'dtale.show'

import pandas as pd

if isinstance(df, (pd.DatetimeIndex, pd.MultiIndex)):
	df = df.to_frame(index=False)

# remove any pre-existing indices for ease of use in the D-Tale code, but this is not required
df = df.reset_index().drop('index', axis=1, errors='ignore')
df.columns = [str(c) for c in df.columns]  # update columns to strings in case they are numbers

chart_data = pd.concat([
	df['speeding'],
	df['alcohol'],
], axis=1)
chart_data = chart_data.sort_values(['speeding'])
chart_data = chart_data.rename(columns={'speeding': 'x'})
chart_data = chart_data.dropna()
chart_data = chart_data.sort_values('x')

import plotly.graph_objs as go

charts = []
charts.append(go.Bar(
	x=list(range(len(chart_data['x']))),
	y=chart_data['alcohol'],
	hovertext=chart_data['x'].values,
	hoverinfo='y_text')
              
figure = go.Figure(data=charts, layout=go.Layout({
    'barmode': 'group',
    'legend': {'orientation': 'h'},
    'title': {'text': 'alcohol by speeding (No Aggregation)'},
    'xaxis': {'nticks': 51, 'tickmode': 'auto', 'title': {'text': 'speeding'}},
    'yaxis': {'title': {'text': 'alcohol (No Aggregation)'}}}))

# If you're having trouble viewing your chart in your notebook try passing your 'chart' into this snippet:

# from plotly.offline import iplot, init_notebook_mode

# init_notebook_mode(connected=True)
# chart.pop('id', None) # for some reason iplot does not like 'id'
# iplot(chart)
```


      File "<ipython-input-18-ee26758b6fee>", line 30
        figure = go.Figure(data=charts, layout=go.Layout({
        ^
    SyntaxError: invalid syntax
    


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
    

    2020-12-06 10:11:25,809 - ERROR    - Exception on /dtale/charts/_dash-update-component [POST]
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
    

    2020-12-06 10:11:32,796 - ERROR    - Exception on /dtale/charts/_dash-update-component [POST]
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
