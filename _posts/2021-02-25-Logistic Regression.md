---
layout: post
title: "Logistic Regression Analysis"
subtitle: "Logistic Regression"
background: '/img/posts/logistic regression/logistic-regression-datriks.jpg'
---

```python
#IMPORT ALL THE LIBRARIES BELOW
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
pd.set_option('display.max_columns',200)
pd.set_option('display.max_rows',200)
print('StandardImport Completed')
```

    StandardImport Completed
    


```python
data_preprocessed = pd.read_csv('D:\\OneDrive - office365hubs.com\\!Python + SQL + Tableau\\Absenteeism_preprocessed.csv')
```


```python
data_preprocessed.head()
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Absenteeism Time in Hours</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>2</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



## Create the targets


```python
data_preprocessed['Absenteeism Time in Hours'].median()
```




    3.0




```python
 targets = np.where(data_preprocessed['Absenteeism Time in Hours'] > 
                    data_preprocessed['Absenteeism Time in Hours'].median(),1,0)
```


```python
targets
```




    array([1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0,
           1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1,
           0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1,
           0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1,
           0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0,
           0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0,
           1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1,
           0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1,
           1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1,
           0, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0,
           0, 0, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0,
           0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1,
           1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0,
           1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0,
           1, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1,
           1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1,
           1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1,
           0, 0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1,
           1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1,
           1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 0, 1, 0, 1,
           1, 1, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0,
           1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1,
           0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0,
           0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1,
           0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0,
           1, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1,
           1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0])




```python
data_preprocessed['ExcesiveAbsenteeism'] = targets
```


```python
data_preprocessed
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Absenteeism Time in Hours</th>
      <th>ExcesiveAbsenteeism</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>2</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>695</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
    </tr>
    <tr>
      <th>696</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>697</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
    </tr>
    <tr>
      <th>698</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>699</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 16 columns</p>
</div>




```python
(targets.sum() / targets.shape[0])*100
```




    45.57142857142858




```python
data_with_targets = data_preprocessed.drop(['Absenteeism Time in Hours'],axis = 1)
```


```python
data_with_targets is data_preprocessed
```




    False




```python
data_with_targets.head()
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>ExcesiveAbsenteeism</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>2</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Select the inputs for the regression


```python
data_with_targets.shape
```




    (700, 15)




```python
#Select the imnputs for our regression
data_with_targets.iloc[:,0:14]
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>2</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>695</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>696</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>697</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>698</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>699</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 14 columns</p>
</div>




```python
data_with_targets.iloc[:,:-1]
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>2</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>695</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>696</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>697</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>698</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>699</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 14 columns</p>
</div>




```python
unscaled_inputs = data_with_targets.iloc[:,:-1]
```


```python
unscaled_inputs.head()
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>2</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## Standardise the data


```python
#here we prepare the scaling mechanism
from sklearn.preprocessing import StandardScaler
#absenteeism _scaler will be used to substract the mean and divide
#by the standard deviation variablewise
absenteeism_scaler = StandardScaler()
```


```python
from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.preprocessing import StandardScaler

class CustomScaler(BaseEstimator,TransformerMixin):
    
    def __init__(self,columns,copy=True,with_mean=True,with_std=True):
        self.scaler=StandardScaler(copy,with_mean,with_std)
        self.columns=columns
        self.mean_ = None
        self.var_ = None
        
    def fit(self,X,y=None):
        self.scaler.fit(X[self.columns],y)
        self.mean_=np.mean(X[self.columns])
        self.var_=np.var(X[self.columns])
        return self
    
    def transform(self,X,y=None,copy=None):
        init_col_order = X.columns
        X_scaled = pd.DataFrame(self.scaler.transform(X[self.columns]),columns=self.columns)
        X_not_scaled = X.loc[:,~X.columns.isin(self.columns)]
        return pd.concat([X_not_scaled,X_scaled],axis=1)[init_col_order]
```


```python
unscaled_inputs.columns.values
```




    array(['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4', 'Month Value',
           'Day of the Week', 'Transportation Expense', 'Distance to Work',
           'Age', 'Daily Work Load Average', 'Body Mass Index', 'Education',
           'Children', 'Pets'], dtype=object)




```python
#columns_to_scale = ['Month Value','Day of the Week', 'Transportation Expense', 'Distance to Work',
       #'Age', 'Daily Work Load Average', 'Body Mass Index','Children', 'Pets']
columns_to_omit = ['Reason_1','Reason_2','Reason_3','Reason_4','Education']    
```


```python
columns_to_scale = [x for x in unscaled_inputs.columns.values 
                    if x not in columns_to_omit]
```


```python
absenteeism_scaler = CustomScaler(columns_to_scale)
```


```python
#this will calculate and store the mean and standard deviation for each element
absenteeism_scaler.fit(unscaled_inputs)
```

    C:\Python\Python38\lib\site-packages\sklearn\base.py:193: FutureWarning: From version 0.24, get_params will raise an AttributeError if a parameter cannot be retrieved as an instance attribute. Previously it would return None.
      warnings.warn('From version 0.24, get_params will raise an '
    




    CustomScaler(columns=['Month Value', 'Day of the Week',
                          'Transportation Expense', 'Distance to Work', 'Age',
                          'Daily Work Load Average', 'Body Mass Index', 'Children',
                          'Pets'],
                 copy=None, with_mean=None, with_std=None)




```python
#transform the unscaled inputs:substarct the mean and divide bi standard deviation
scaled_inputs = absenteeism_scaler.transform(unscaled_inputs)
```


```python
#new_dsata_raw = pd.read_csv('new_data.csv')
#new_data_scaled = absenteeism_scale.transform(new_data_raw)
```


```python
scaled_inputs
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.182726</td>
      <td>-0.683704</td>
      <td>1.005844</td>
      <td>0.412816</td>
      <td>-0.536062</td>
      <td>-0.806331</td>
      <td>0.767431</td>
      <td>0</td>
      <td>0.880469</td>
      <td>0.268487</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.182726</td>
      <td>-0.683704</td>
      <td>-1.574681</td>
      <td>-1.141882</td>
      <td>2.130803</td>
      <td>-0.806331</td>
      <td>1.002633</td>
      <td>0</td>
      <td>-0.019280</td>
      <td>-0.589690</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.182726</td>
      <td>-0.007725</td>
      <td>-0.654143</td>
      <td>1.426749</td>
      <td>0.248310</td>
      <td>-0.806331</td>
      <td>1.002633</td>
      <td>0</td>
      <td>-0.919030</td>
      <td>-0.589690</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.182726</td>
      <td>0.668253</td>
      <td>0.854936</td>
      <td>-1.682647</td>
      <td>0.405184</td>
      <td>-0.806331</td>
      <td>-0.643782</td>
      <td>0</td>
      <td>0.880469</td>
      <td>-0.589690</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.182726</td>
      <td>0.668253</td>
      <td>1.005844</td>
      <td>0.412816</td>
      <td>-0.536062</td>
      <td>-0.806331</td>
      <td>0.767431</td>
      <td>0</td>
      <td>0.880469</td>
      <td>0.268487</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>695</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-0.388293</td>
      <td>-0.007725</td>
      <td>-0.654143</td>
      <td>-0.533522</td>
      <td>0.562059</td>
      <td>-0.853789</td>
      <td>-1.114186</td>
      <td>1</td>
      <td>0.880469</td>
      <td>-0.589690</td>
    </tr>
    <tr>
      <th>696</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-0.388293</td>
      <td>-0.007725</td>
      <td>0.040034</td>
      <td>-0.263140</td>
      <td>-1.320435</td>
      <td>-0.853789</td>
      <td>-0.643782</td>
      <td>0</td>
      <td>-0.019280</td>
      <td>1.126663</td>
    </tr>
    <tr>
      <th>697</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-0.388293</td>
      <td>0.668253</td>
      <td>1.624567</td>
      <td>-0.939096</td>
      <td>-1.320435</td>
      <td>-0.853789</td>
      <td>-0.408580</td>
      <td>1</td>
      <td>-0.919030</td>
      <td>-0.589690</td>
    </tr>
    <tr>
      <th>698</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>-0.388293</td>
      <td>0.668253</td>
      <td>0.190942</td>
      <td>-0.939096</td>
      <td>-0.692937</td>
      <td>-0.853789</td>
      <td>-0.408580</td>
      <td>1</td>
      <td>-0.919030</td>
      <td>-0.589690</td>
    </tr>
    <tr>
      <th>699</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>-0.388293</td>
      <td>0.668253</td>
      <td>1.036026</td>
      <td>0.074838</td>
      <td>0.562059</td>
      <td>-0.853789</td>
      <td>-0.408580</td>
      <td>0</td>
      <td>-0.019280</td>
      <td>0.268487</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 14 columns</p>
</div>




```python
scaled_inputs.shape
```




    (700, 14)



## Split the data into train and test and shufle

## Import the relevant module


```python
from sklearn.model_selection import train_test_split
```

## Split


```python
train_test_split(scaled_inputs,targets)
```




    [     Reason_1  Reason_2  Reason_3  Reason_4  Month Value  Day of the Week  \
     121         0         0         0         1    -1.530333        -1.359682   
     327         1         0         0         0     1.324766        -0.007725   
     380         0         0         0         1    -1.244823        -1.359682   
     117         0         0         0         1    -1.530333        -0.007725   
     342         0         0         0         1     1.610276        -1.359682   
     ..        ...       ...       ...       ...          ...              ...   
     192         0         0         0         1    -0.673803         0.668253   
     567         0         0         0         1     1.610276         0.668253   
     644         0         0         0         1    -0.959313         1.344231   
     108         0         0         0         1     1.610276        -0.683704   
     638         0         0         0         1    -0.959313        -0.683704   
     
          Transportation Expense  Distance to Work       Age  \
     121               -1.574681         -1.344669  0.091435   
     327               -1.574681         -1.344669  0.091435   
     380               -0.503235         -0.060353 -0.536062   
     117                0.040034         -0.263140 -1.320435   
     342                0.190942         -1.277074  0.091435   
     ..                      ...               ...       ...   
     192               -1.574681         -1.141882  2.130803   
     567                0.040034         -0.263140 -1.320435   
     644               -0.654143         -0.263140 -1.006686   
     108                0.040034         -0.263140 -1.320435   
     638               -0.654143          1.426749  0.248310   
     
          Daily Work Load Average  Body Mass Index  Education  Children      Pets  
     121                 0.919937         0.297027          0 -0.919030 -0.589690  
     327                 0.305783         0.297027          0 -0.919030 -0.589690  
     380                -0.499679        -0.408580          0  0.880469  1.126663  
     117                 0.919937        -0.643782          0 -0.019280  1.126663  
     342                -0.879469         0.532229          1 -0.019280  0.268487  
     ..                       ...              ...        ...       ...       ...  
     192                 1.366488         1.002633          0 -0.019280 -0.589690  
     567                 0.218718        -0.643782          0 -0.019280  1.126663  
     644                -1.240355        -1.819793          1 -0.919030 -0.589690  
     108                -0.262439        -0.643782          0 -0.019280  1.126663  
     638                -1.240355         1.002633          0 -0.919030 -0.589690  
     
     [525 rows x 14 columns],
          Reason_1  Reason_2  Reason_3  Reason_4  Month Value  Day of the Week  \
     222         1         0         0         0    -0.102784        -0.683704   
     123         0         0         0         1    -1.530333        -0.683704   
     492         0         0         0         1     0.468236        -0.683704   
     288         1         0         0         0     0.753746        -1.359682   
     24          0         0         1         0     0.468236        -1.359682   
     450         0         0         0         1    -0.102784        -1.359682   
     147         0         0         0         1    -1.244823        -0.683704   
     662         0         0         0         1    -0.673803        -0.683704   
     697         1         0         0         0    -0.388293         0.668253   
     56          0         0         1         0     0.753746        -0.007725   
     195         0         0         0         1    -0.673803         1.344231   
     649         0         0         0         1    -0.959313        -0.007725   
     457         0         0         0         1    -0.102784         1.344231   
     414         0         0         0         1    -0.673803         1.344231   
     341         0         0         0         1     1.610276        -1.359682   
     636         0         0         0         1    -0.959313        -1.359682   
     399         0         0         0         1    -0.959313        -0.007725   
     48          0         0         0         1     0.753746         1.344231   
     555         1         0         0         0     1.610276        -0.683704   
     231         1         0         0         0    -0.102784        -0.007725   
     78          0         0         0         1     1.039256         1.344231   
     619         0         0         0         1    -0.959313        -1.359682   
     615         0         0         0         1    -1.244823        -0.007725   
     265         0         0         0         1     0.468236        -1.359682   
     21          1         0         0         0     0.468236        -1.359682   
     268         1         0         0         0     0.468236        -1.359682   
     202         1         0         0         0    -0.673803         1.344231   
     674         0         0         0         1    -0.388293        -1.359682   
     366         0         0         0         1    -1.530333        -1.359682   
     39          0         0         0         1     0.468236         1.344231   
     36          0         0         0         1     0.468236        -0.683704   
     448         0         0         0         1    -0.102784        -1.359682   
     536         1         0         0         0     1.324766        -0.683704   
     541         0         0         0         1     1.324766        -0.007725   
     218         1         0         0         0    -0.388293        -0.007725   
     198         0         0         1         0    -0.673803        -0.683704   
     462         1         0         0         0     0.182726         1.344231   
     244         1         0         0         0     0.182726        -0.683704   
     14          0         0         0         1     0.182726        -0.007725   
     542         1         0         0         0     1.324766         0.668253   
     209         1         0         0         0    -0.388293        -0.007725   
     148         0         0         0         1    -1.244823        -0.683704   
     658         1         0         0         0    -0.673803         0.668253   
     16          0         0         0         1     0.182726         1.344231   
     627         0         0         0         1    -0.959313         1.344231   
     664         1         0         0         0    -0.673803        -0.007725   
     165         1         0         0         0    -0.959313        -1.359682   
     46          0         0         0         1     0.753746        -1.359682   
     479         0         0         0         1     0.182726         0.668253   
     23          0         0         0         1     0.468236         1.344231   
     139         1         0         0         0    -1.244823        -0.007725   
     565         1         0         0         0     1.610276        -0.683704   
     420         1         0         0         0    -0.673803        -0.007725   
     272         0         0         1         0     0.753746        -0.007725   
     643         0         0         0         1    -0.959313         0.668253   
     53          0         0         0         1     0.753746        -0.683704   
     534         1         0         0         0     1.324766        -0.007725   
     394         0         0         0         1    -0.959313        -1.359682   
     594         0         0         1         0    -1.244823        -1.359682   
     508         1         0         0         0     1.039256         0.668253   
     444         0         0         0         1    -0.102784         0.668253   
     281         0         0         0         1     0.753746         1.344231   
     170         0         0         0         1    -0.959313         1.344231   
     367         0         0         0         1    -1.530333        -0.007725   
     85          1         0         0         0     1.324766        -1.359682   
     362         0         0         0         1    -1.530333        -0.683704   
     197         0         0         1         0    -0.673803         1.344231   
     609         0         0         0         1    -1.244823        -1.359682   
     484         0         0         0         1     0.468236        -0.683704   
     37          1         0         0         0     0.468236         0.668253   
     8           0         0         1         0     0.182726        -1.359682   
     174         0         0         0         1    -0.959313        -0.007725   
     61          0         0         0         1     0.753746         1.344231   
     679         1         0         0         0    -0.388293         0.668253   
     519         0         0         0         1     1.039256        -0.683704   
     504         0         0         0         1     0.753746        -1.359682   
     375         0         0         0         1    -1.244823        -1.359682   
     143         0         0         0         1    -1.244823         1.344231   
     668         0         1         0         0    -0.673803        -0.683704   
     485         0         0         0         1     0.468236        -0.683704   
     530         0         0         0         0     1.039256         2.696187   
     478         0         0         0         1     0.182726         0.668253   
     587         1         0         0         0    -1.244823        -0.007725   
     186         0         0         0         1    -0.673803        -0.683704   
     228         1         0         0         0    -0.102784        -0.007725   
     427         1         0         0         0    -0.388293        -1.359682   
     411         0         0         0         1    -0.959313        -0.683704   
     415         0         0         0         1    -0.673803        -1.359682   
     499         0         0         0         1     0.753746         0.668253   
     387         1         0         0         0    -1.244823         1.344231   
     547         0         0         0         1     1.324766        -0.683704   
     336         0         0         0         0     1.324766         1.344231   
     325         0         0         0         0     1.324766        -0.683704   
     156         0         0         1         0    -0.959313        -0.683704   
     402         0         0         0         1    -0.959313         1.344231   
     554         0         0         0         1     1.324766        -1.359682   
     290         0         0         0         1     1.039256        -0.007725   
     473         0         0         0         1     0.182726         1.344231   
     128         0         0         0         1    -1.530333        -0.683704   
     573         0         0         0         1    -1.530333         1.344231   
     131         0         0         0         1    -1.530333        -0.007725   
     177         1         0         0         0    -0.959313        -0.683704   
     210         0         0         0         1    -0.388293        -1.359682   
     498         0         0         0         1     0.753746        -0.683704   
     200         0         0         1         0    -0.673803        -0.007725   
     576         1         0         0         0    -1.530333         0.668253   
     690         0         0         0         0    -0.388293        -0.007725   
     476         0         0         0         1     0.182726        -1.359682   
     49          1         0         0         0     0.753746        -1.359682   
     305         0         0         0         1     1.039256        -0.007725   
     368         0         0         0         1    -1.530333         1.344231   
     102         0         0         0         1     1.610276         0.668253   
     79          0         0         0         1     1.039256         1.344231   
     295         1         0         0         0     1.039256        -0.007725   
     566         0         0         0         1     1.610276        -0.683704   
     60          0         0         0         1     0.753746         1.344231   
     339         1         0         0         0     1.610276        -1.359682   
     110         0         0         0         1     1.610276         1.344231   
     540         0         0         0         1     1.324766        -0.683704   
     261         1         0         0         0     0.468236        -0.007725   
     675         0         0         1         0    -0.388293        -0.007725   
     324         0         0         0         1     1.324766        -1.359682   
     266         0         0         0         1     0.468236        -0.007725   
     610         0         0         0         1    -1.244823        -1.359682   
     586         0         0         0         1    -1.244823        -0.683704   
     673         0         0         0         1    -0.673803         1.344231   
     104         0         0         1         0     1.610276        -0.683704   
     571         1         0         0         0    -1.530333         0.668253   
     533         0         0         0         1     1.324766        -0.007725   
     144         1         0         0         0    -1.244823        -1.359682   
     300         0         0         0         0     1.039256         1.344231   
     40          0         0         0         1     0.753746        -0.683704   
     127         0         0         0         1    -1.530333        -1.359682   
     107         0         0         0         1     1.610276         1.344231   
     477         0         0         0         1     0.182726        -0.683704   
     604         0         0         0         1    -1.244823        -0.007725   
     166         0         0         0         1    -0.959313        -1.359682   
     240         0         0         0         1     0.182726        -0.683704   
     229         1         0         0         0    -0.102784         1.344231   
     412         0         0         0         1    -0.673803        -0.007725   
     383         0         0         0         1    -1.244823        -0.683704   
     124         0         0         0         1    -1.530333        -0.007725   
     71          0         0         0         1     1.039256         0.668253   
     548         0         0         0         0     1.324766        -0.683704   
     230         0         0         0         1    -0.102784        -0.683704   
     4           0         0         0         1     0.182726         0.668253   
     423         0         0         0         1    -0.673803        -1.359682   
     557         0         0         0         1     1.610276        -0.007725   
     259         0         0         0         1     0.468236        -1.359682   
     373         0         0         0         1    -1.244823         1.344231   
     11          1         0         0         0     0.182726        -0.683704   
     31          0         0         1         0     0.468236         0.668253   
     482         0         0         0         1     0.468236        -0.683704   
     19          0         0         0         1     0.468236        -0.007725   
     365         0         0         0         1    -1.530333        -1.359682   
     227         1         0         0         0    -0.102784        -1.359682   
     560         0         0         0         1     1.610276        -0.007725   
     487         0         0         0         1     0.468236        -1.359682   
     316         0         0         0         1     1.039256         2.020209   
     183         0         0         0         1    -0.959313        -1.359682   
     698         0         0         0         1    -0.388293         0.668253   
     445         0         0         0         1    -0.102784         1.344231   
     574         1         0         0         0    -1.530333        -1.359682   
     684         0         0         0         1    -0.388293         1.344231   
     406         0         0         0         0    -0.959313         0.668253   
     475         0         0         0         1     0.182726        -1.359682   
     628         0         0         0         1    -0.959313        -1.359682   
     109         0         0         0         1     1.610276        -0.007725   
     418         0         0         0         1    -0.673803        -0.007725   
     551         0         0         0         0     1.324766         0.668253   
     296         1         0         0         0     1.039256        -0.007725   
     172         1         0         0         0    -0.959313        -1.359682   
     452         0         0         0         1    -0.102784        -1.359682   
     173         1         0         0         0    -0.959313        -0.683704   
     155         0         0         0         1    -0.959313        -0.683704   
     
          Transportation Expense  Distance to Work       Age  \
     222                0.356940         -0.330735  0.718933   
     123               -1.574681         -1.344669  0.091435   
     492               -1.574681         -1.344669  0.091435   
     288               -0.654143          1.426749  0.248310   
     24                 1.005844          0.412816 -0.536062   
     450               -0.654143          1.426749  0.248310   
     147                0.040034         -0.263140 -1.320435   
     662               -1.574681         -1.141882  2.130803   
     697                1.624567         -0.939096 -1.320435   
     56                 0.040034         -0.263140 -1.320435   
     195                1.036026          0.074838  0.562059   
     649               -0.654143          1.426749  0.248310   
     457                0.356940         -0.330735  0.718933   
     414                1.624567         -0.939096 -1.320435   
     341               -0.654143          1.426749  0.248310   
     636               -0.654143          1.426749  0.248310   
     399                2.092381          1.494345 -1.320435   
     48                 0.568211          1.359154 -0.065439   
     555               -0.654143          1.426749  0.248310   
     231               -1.574681         -1.141882  2.130803   
     78                 2.092381          1.494345 -1.320435   
     619                0.387122         -0.330735  1.660180   
     615               -0.654143          1.426749  0.248310   
     265                0.190942         -1.277074  0.091435   
     21                -0.654143          1.426749  0.248310   
     268                2.092381          1.494345 -1.320435   
     202                0.190942         -1.277074  0.091435   
     674                0.190942         -1.277074  0.091435   
     366               -1.574681         -1.141882  2.130803   
     39                 0.568211          1.359154 -0.065439   
     36                 1.005844          0.412816 -0.536062   
     448               -0.654143          1.426749  0.248310   
     536               -1.574681         -1.344669  0.091435   
     541                1.036026          0.074838  0.562059   
     218               -1.574681         -1.141882  2.130803   
     198                1.005844          0.412816 -0.536062   
     462                0.568211          1.359154 -0.065439   
     244                1.624567         -0.939096 -1.320435   
     14                -0.654143          1.426749  0.248310   
     542               -1.574681         -1.344669  0.091435   
     209               -0.578689          0.818389 -1.477309   
     148               -0.654143         -0.263140 -1.006686   
     658               -0.503235         -0.060353 -0.536062   
     16                -0.654143          1.426749  0.248310   
     627               -0.654143         -0.263140 -1.006686   
     664               -1.574681         -1.344669  0.091435   
     165               -1.016322         -1.209478 -0.379188   
     46                -0.654143          1.426749  0.248310   
     479                0.854936         -1.682647  0.405184   
     23                 0.568211          1.359154 -0.065439   
     139               -0.654143          1.426749  0.248310   
     565               -0.654143         -0.263140 -1.006686   
     420               -1.574681         -1.141882  2.130803   
     272                1.005844          0.412816 -0.536062   
     643               -0.654143          1.426749  0.248310   
     53                -1.574681         -1.344669  0.091435   
     534                1.624567         -0.939096 -1.320435   
     394               -0.654143          1.426749  0.248310   
     594                0.160760          1.426749 -0.849811   
     508                0.190942         -0.939096 -0.692937   
     444                0.040034         -0.263140 -1.320435   
     281                1.036026          0.074838  0.562059   
     170                0.568211          1.359154 -0.065439   
     367               -0.654143          1.426749  0.248310   
     85                -1.016322         -1.209478 -0.379188   
     362               -1.016322         -1.209478 -0.379188   
     197                0.568211          1.359154 -0.065439   
     609                0.190942         -0.939096 -0.692937   
     484                1.005844          0.412816 -0.536062   
     37                 1.036026          1.359154 -0.692937   
     8                 -1.016322         -1.209478 -0.379188   
     174                0.040034         -0.263140 -1.320435   
     61                 0.568211          1.359154 -0.065439   
     679               -0.654143         -0.263140 -1.006686   
     519                2.213108         -0.871500 -0.849811   
     504               -0.654143         -0.263140 -1.006686   
     375               -0.654143          1.426749  0.248310   
     143                1.005844          0.412816 -0.536062   
     668                0.190942         -0.939096 -0.692937   
     485                0.190942         -0.668713  1.032682   
     530                0.040034         -0.263140 -1.320435   
     478                1.036026          0.074838  0.562059   
     587                0.387122         -0.330735  1.660180   
     186               -1.016322         -1.209478 -0.379188   
     228               -1.574681         -1.141882  2.130803   
     427                2.213108         -0.871500 -0.849811   
     411                0.356940         -0.330735  0.718933   
     415                2.213108         -0.871500 -0.849811   
     499                1.036026          0.074838  0.562059   
     387                0.356940         -0.330735  0.718933   
     547                0.040034         -0.263140 -1.320435   
     336                2.348925          1.291558 -0.065439   
     325                1.624567         -0.939096 -1.320435   
     156                0.568211          1.359154 -0.065439   
     402               -1.574681         -1.141882  2.130803   
     554                2.092381          1.494345 -1.320435   
     290                0.190942         -1.277074  0.091435   
     473               -1.574681         -1.141882  2.130803   
     128               -1.574681         -1.344669  0.091435   
     573               -0.654143         -0.533522  0.562059   
     131               -1.574681         -1.344669  0.091435   
     177                0.040034         -0.263140 -1.320435   
     210               -1.016322         -1.209478 -0.379188   
     498               -1.016322         -1.209478 -0.379188   
     200                2.348925          1.291558 -0.065439   
     576                1.005844          1.223963  1.973929   
     690                2.348925          1.291558 -0.065439   
     476                0.190942         -0.668713  1.032682   
     49                 1.036026          0.074838  0.562059   
     305                0.190942         -0.668713  1.032682   
     368               -0.654143          1.426749  0.248310   
     102                0.040034         -0.263140 -1.320435   
     79                 0.568211          1.359154 -0.065439   
     295               -0.654143         -0.263140 -1.006686   
     566                0.040034         -0.263140 -1.320435   
     60                -0.654143          1.426749  0.248310   
     339                0.040034         -0.263140 -1.320435   
     110               -1.574681         -1.344669  0.091435   
     540                2.092381          1.494345 -1.320435   
     261                0.688938         -1.277074 -0.536062   
     675                0.040034         -1.006691  0.718933   
     324                0.190942         -0.668713  1.032682   
     266               -1.574681         -1.141882  2.130803   
     610               -0.654143          1.426749  0.248310   
     586                0.040034         -0.263140 -1.320435   
     673               -0.654143         -0.263140 -1.006686   
     104               -1.574681         -1.344669  0.091435   
     571               -0.654143         -0.263140 -1.006686   
     533               -1.574681         -1.344669  0.091435   
     144                2.499833         -1.006691  2.130803   
     300                0.190942         -0.668713  1.032682   
     40                -0.578689          0.818389 -1.477309   
     127               -1.574681         -1.344669  0.091435   
     107                0.568211          1.359154 -0.065439   
     477                0.356940         -0.330735  0.718933   
     604               -0.654143         -0.263140 -1.006686   
     166                0.568211          1.359154 -0.065439   
     240                1.624567         -0.939096 -1.320435   
     229                0.190942         -1.277074  0.091435   
     412               -0.654143          1.426749  0.248310   
     383                1.036026          0.074838  0.562059   
     124               -1.574681         -1.344669  0.091435   
     71                 1.036026          0.074838  0.562059   
     548                1.036026          0.074838  0.562059   
     230               -1.574681         -1.141882  2.130803   
     4                  1.005844          0.412816 -0.536062   
     423               -0.654143          1.426749  0.248310   
     557                0.040034         -0.263140 -1.320435   
     259                1.005844          0.412816 -0.536062   
     373                0.568211          1.359154 -0.065439   
     11                 0.568211          1.359154 -0.065439   
     31                 0.190942         -0.060353  1.817054   
     482                0.356940         -0.330735  0.718933   
     19                 0.387122         -0.330735  1.660180   
     365                0.190942         -0.668713  1.032682   
     227                0.356940         -0.330735  0.718933   
     560                0.040034         -0.263140 -1.320435   
     487               -0.654143          1.426749  0.248310   
     316               -1.574681         -1.141882  2.130803   
     183                0.040034         -0.263140 -1.320435   
     698                0.190942         -0.939096 -0.692937   
     445                0.568211          1.359154 -0.065439   
     574               -0.654143         -0.263140 -1.006686   
     684               -0.654143         -0.263140 -1.006686   
     406                0.356940         -0.330735  0.718933   
     475                1.005844          0.412816 -0.536062   
     628                0.190942         -0.939096 -0.692937   
     109                2.092381          1.494345 -1.320435   
     418               -0.654143          1.426749  0.248310   
     551                0.190942         -0.668713  1.032682   
     296               -1.574681         -1.344669  0.091435   
     172                0.854936         -1.682647  0.405184   
     452               -0.654143          1.426749  0.248310   
     173               -0.654143          1.426749  0.248310   
     155                1.036026          1.359154 -0.692937   
     
          Daily Work Load Average  Body Mass Index  Education  Children      Pets  
     222                 2.644155        -0.878984          0 -0.919030 -0.589690  
     123                 0.919937         0.297027          0 -0.919030 -0.589690  
     492                -0.550213         0.297027          0 -0.919030 -0.589690  
     288                 0.560476         1.002633          0 -0.919030 -0.589690  
     24                 -1.647399         0.767431          0  0.880469  0.268487  
     450                -0.446195         1.002633          0 -0.919030 -0.589690  
     147                 0.769711        -0.643782          0 -0.019280  1.126663  
     662                -0.637953         1.002633          0 -0.019280 -0.589690  
     697                -0.853789        -0.408580          1 -0.919030 -0.589690  
     56                 -0.758273        -0.643782          0 -0.019280  1.126663  
     195                 1.366488        -0.408580          0 -0.019280  0.268487  
     649                -1.240355         1.002633          0 -0.919030 -0.589690  
     457                -0.446195        -0.878984          0 -0.919030 -0.589690  
     414                -0.809957        -0.408580          1 -0.919030 -0.589690  
     341                -0.879469         1.002633          0 -0.919030 -0.589690  
     636                -1.240355         1.002633          0 -0.919030 -0.589690  
     399                -0.685486         0.061825          0 -0.019280  2.843016  
     48                 -0.758273        -0.878984          0  2.679969 -0.589690  
     555                 0.218718         1.002633          0 -0.919030 -0.589690  
     231                 2.644155         1.002633          0 -0.019280 -0.589690  
     78                 -0.458497         0.061825          0 -0.019280  2.843016  
     619                -1.240355         1.237836          0  0.880469  0.268487  
     615                -0.188851         1.002633          0 -0.919030 -0.589690  
     265                -0.154696         0.532229          1 -0.019280  0.268487  
     21                 -1.647399         1.002633          0 -0.919030 -0.589690  
     268                -0.154696         0.061825          0 -0.019280  2.843016  
     202                 1.366488         0.532229          1 -0.019280  0.268487  
     674                -0.853789         0.532229          1 -0.019280  0.268487  
     366                 1.456728         1.002633          0 -0.019280 -0.589690  
     39                 -1.647399        -0.878984          0  2.679969 -0.589690  
     36                 -1.647399         0.767431          0  0.880469  0.268487  
     448                -0.446195         1.002633          0 -0.919030 -0.589690  
     536                -0.082083         0.297027          0 -0.919030 -0.589690  
     541                -0.082083        -0.408580          0 -0.019280  0.268487  
     218                 2.677510         1.002633          0 -0.019280 -0.589690  
     198                 1.366488         0.767431          0  0.880469  0.268487  
     462                -1.037971        -0.878984          0  2.679969 -0.589690  
     244                 0.087771        -0.408580          1 -0.919030 -0.589690  
     14                 -0.806331         1.002633          0 -0.919030 -0.589690  
     542                -0.082083         0.297027          0 -0.919030 -0.589690  
     209                 2.677510        -1.349389          0 -0.919030 -0.589690  
     148                 0.769711        -1.819793          1 -0.919030 -0.589690  
     658                -0.637953        -0.408580          0  0.880469  1.126663  
     16                 -0.806331         1.002633          0 -0.919030 -0.589690  
     627                -1.240355        -1.819793          1 -0.919030 -0.589690  
     664                -0.637953         0.297027          0 -0.919030 -0.589690  
     165                 1.786584        -0.408580          0  0.880469 -0.589690  
     46                 -0.758273         1.002633          0 -0.919030 -0.589690  
     479                -1.037971        -0.643782          0  0.880469 -0.589690  
     23                 -1.647399        -0.878984          0  2.679969 -0.589690  
     139                 0.769711         1.002633          0 -0.919030 -0.589690  
     565                 0.218718        -1.819793          1 -0.919030 -0.589690  
     420                -0.809957         1.002633          0 -0.019280 -0.589690  
     272                 0.560476         0.767431          0  0.880469  0.268487  
     643                -1.240355         1.002633          0 -0.919030 -0.589690  
     53                 -0.758273         0.297027          0 -0.919030 -0.589690  
     534                -0.082083        -0.408580          1 -0.919030 -0.589690  
     394                -0.685486         1.002633          0 -0.919030 -0.589690  
     594                -0.188851        -1.349389          1 -0.019280  6.275721  
     508                 0.326336        -0.408580          1 -0.919030 -0.589690  
     444                -0.446195        -0.643782          0 -0.019280  1.126663  
     281                 0.560476        -0.408580          0 -0.019280  0.268487  
     170                 1.786584        -0.878984          0  2.679969 -0.589690  
     367                 1.456728         1.002633          0 -0.919030 -0.589690  
     85                  0.863727        -0.408580          0  0.880469 -0.589690  
     362                 1.456728        -0.408580          0  0.880469 -0.589690  
     197                 1.366488        -0.878984          0  2.679969 -0.589690  
     609                -0.188851        -0.408580          1 -0.919030 -0.589690  
     484                -0.550213         0.767431          0  0.880469  0.268487  
     37                 -1.647399        -0.878984          0 -0.919030 -0.589690  
     8                  -0.806331        -0.408580          0  0.880469 -0.589690  
     174                 1.786584        -0.643782          0 -0.019280  1.126663  
     61                 -0.758273        -0.878984          0  2.679969 -0.589690  
     679                -0.853789        -1.819793          1 -0.919030 -0.589690  
     519                 0.326336        -0.408580          0  1.780219 -0.589690  
     504                -0.251187        -1.819793          1 -0.919030 -0.589690  
     375                -0.499679         1.002633          0 -0.919030 -0.589690  
     143                 0.769711         0.767431          0  0.880469  0.268487  
     668                -0.637953        -0.408580          1 -0.919030 -0.589690  
     485                -0.550213         2.649049          0 -0.019280 -0.589690  
     530                 0.326336        -0.643782          0 -0.019280  1.126663  
     478                -1.037971        -0.408580          0 -0.019280  0.268487  
     587                -0.188851         1.237836          0  0.880469  0.268487  
     186                 1.366488        -0.408580          0  0.880469 -0.589690  
     228                 2.644155         1.002633          0 -0.019280 -0.589690  
     427                -0.643304        -0.408580          0  1.780219 -0.589690  
     411                -0.685486        -0.878984          0 -0.919030 -0.589690  
     415                -0.809957        -0.408580          0  1.780219 -0.589690  
     499                -0.251187        -0.408580          0 -0.019280  0.268487  
     387                -0.499679        -0.878984          0 -0.919030 -0.589690  
     547                -0.082083        -0.643782          0 -0.019280  1.126663  
     336                 0.305783        -1.349389          0  0.880469  2.843016  
     325                 0.305783        -0.408580          1 -0.919030 -0.589690  
     156                 1.786584        -0.878984          0  2.679969 -0.589690  
     402                -0.685486         1.002633          0 -0.019280 -0.589690  
     554                -0.082083         0.061825          0 -0.019280  2.843016  
     290                -0.169648         0.532229          1 -0.019280  0.268487  
     473                -1.037971         1.002633          0 -0.019280 -0.589690  
     128                 0.919937         0.297027          0 -0.919030 -0.589690  
     573                 1.043433        -1.114186          1  0.880469 -0.589690  
     131                 0.919937         0.297027          0 -0.919030 -0.589690  
     177                 1.786584        -0.643782          0 -0.019280  1.126663  
     210                 2.677510        -0.408580          0  0.880469 -0.589690  
     498                -0.251187        -0.408580          0  0.880469 -0.589690  
     200                 1.366488        -1.349389          0  0.880469  2.843016  
     576                 1.043433         2.178644          0 -0.919030  1.126663  
     690                -0.853789        -1.349389          0  0.880469  2.843016  
     476                -1.037971         2.649049          0 -0.019280 -0.589690  
     49                 -0.758273        -0.408580          0 -0.019280  0.268487  
     305                -0.169648         2.649049          0 -0.019280 -0.589690  
     368                 1.456728         1.002633          0 -0.919030 -0.589690  
     102                -0.262439        -0.643782          0 -0.019280  1.126663  
     79                 -0.458497        -0.878984          0  2.679969 -0.589690  
     295                -0.169648        -1.819793          1 -0.919030 -0.589690  
     566                 0.218718        -0.643782          0 -0.019280  1.126663  
     60                 -0.758273         1.002633          0 -0.919030 -0.589690  
     339                -0.879469        -0.643782          0 -0.019280  1.126663  
     110                -0.262439         0.297027          0 -0.919030 -0.589690  
     540                -0.082083         0.061825          0 -0.019280  2.843016  
     261                -0.154696        -0.408580          1 -0.919030 -0.589690  
     675                -0.853789         0.297027          1  0.880469  1.126663  
     324                 0.305783         2.649049          0 -0.019280 -0.589690  
     266                -0.154696         1.002633          0 -0.019280 -0.589690  
     610                -0.188851         1.002633          0 -0.919030 -0.589690  
     586                -0.188851        -0.643782          0 -0.019280  1.126663  
     673                -0.637953        -1.819793          1 -0.919030 -0.589690  
     104                -0.262439         0.297027          0 -0.919030 -0.589690  
     571                 1.043433        -1.819793          1 -0.919030 -0.589690  
     533                -0.082083         0.297027          0 -0.919030 -0.589690  
     144                 0.769711        -0.643782          0 -0.919030 -0.589690  
     300                -0.169648         2.649049          0 -0.019280 -0.589690  
     40                 -0.758273        -1.349389          0 -0.919030 -0.589690  
     127                 0.919937         0.297027          0 -0.919030 -0.589690  
     107                -0.262439        -0.878984          0  2.679969 -0.589690  
     477                -1.037971        -0.878984          0 -0.919030 -0.589690  
     604                -0.188851        -1.819793          1 -0.919030 -0.589690  
     166                 1.786584        -0.878984          0  2.679969 -0.589690  
     240                 0.087771        -0.408580          1 -0.919030 -0.589690  
     229                 2.644155         0.532229          1 -0.019280  0.268487  
     412                -0.809957         1.002633          0 -0.919030 -0.589690  
     383                -0.499679        -0.408580          0 -0.019280  0.268487  
     124                 0.919937         0.297027          0 -0.919030 -0.589690  
     71                 -0.458497        -0.408580          0 -0.019280  0.268487  
     548                -0.082083        -0.408580          0 -0.019280  0.268487  
     230                 2.644155         1.002633          0 -0.019280 -0.589690  
     4                  -0.806331         0.767431          0  0.880469  0.268487  
     423                -0.809957         1.002633          0 -0.919030 -0.589690  
     557                 0.218718        -0.643782          0 -0.019280  1.126663  
     259                -0.154696         0.767431          0  0.880469  0.268487  
     373                -0.499679        -0.878984          0  2.679969 -0.589690  
     11                 -0.806331        -0.878984          0  2.679969 -0.589690  
     31                 -1.647399         1.473038          0 -0.019280  3.701192  
     482                -0.550213        -0.878984          0 -0.919030 -0.589690  
     19                 -1.647399         1.237836          0  0.880469  0.268487  
     365                 1.456728         2.649049          0 -0.019280 -0.589690  
     227                 2.644155        -0.878984          0 -0.919030 -0.589690  
     560                 0.218718        -0.643782          0 -0.019280  1.126663  
     487                -0.550213         1.002633          0 -0.919030 -0.589690  
     316                -0.169648         1.002633          0 -0.019280 -0.589690  
     183                 1.786584        -0.643782          0 -0.019280  1.126663  
     698                -0.853789        -0.408580          1 -0.919030 -0.589690  
     445                -0.446195        -0.878984          0  2.679969 -0.589690  
     574                 1.043433        -1.819793          1 -0.919030 -0.589690  
     684                -0.853789        -1.819793          1 -0.919030 -0.589690  
     406                -0.685486        -0.878984          0 -0.919030 -0.589690  
     475                -1.037971         0.767431          0  0.880469  0.268487  
     628                -1.240355        -0.408580          1 -0.919030 -0.589690  
     109                -0.262439         0.061825          0 -0.019280  2.843016  
     418                -0.809957         1.002633          0 -0.919030 -0.589690  
     551                -0.082083         2.649049          0 -0.019280 -0.589690  
     296                -0.169648         0.297027          0 -0.919030 -0.589690  
     172                 1.786584        -0.643782          0  0.880469 -0.589690  
     452                -0.446195         1.002633          0 -0.919030 -0.589690  
     173                 1.786584         1.002633          0 -0.919030 -0.589690  
     155                 1.786584        -0.878984          0 -0.919030 -0.589690  ,
     array([0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 1,
            0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 0,
            0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1,
            0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0,
            1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 1, 0, 0, 1,
            0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0,
            1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1,
            1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1,
            0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 1,
            0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0,
            0, 0, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0,
            0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1,
            1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 1,
            0, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1,
            1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0,
            0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1,
            0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0,
            0, 0, 0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1,
            0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 1, 0,
            1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1,
            0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1,
            1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1,
            1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1,
            0, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0]),
     array([1, 0, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 0,
            0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0,
            0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 1,
            1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1,
            0, 1, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0,
            0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 0,
            0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0,
            1, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0])]




```python
#x_train,x_test,y_train,y_test = train_test_split(scaled_inputs,targets, train_size=0.9)
#means that 90% used for trainin and only 10% for testing
x_train,x_test,y_train,y_test = train_test_split(scaled_inputs,targets,train_size = 0.8,random_state=20)
```


```python
print(x_train.shape,y_train.shape)
```

    (560, 14) (560,)
    


```python
print(x_test.shape,y_test.shape)
```

    (140, 14) (140,)
    


```python
#sklearn.mode_selection.train_test_split(inputs ,targets,train_size,
#shuffle=True,random_state)
#split arrays or matrices into random train and test subsets
```

## Logistic Regression with sklearn


```python
from sklearn.linear_model import LogisticRegression
from sklearn import metrics
```

### Training the model


```python
reg = LogisticRegression()
```


```python
#sklearn.linear_model.LogisticRegression.fit(x,y)
#fits the model according to the given training data
reg.fit(x_train,y_train)
```




    LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
                       intercept_scaling=1, l1_ratio=None, max_iter=100,
                       multi_class='auto', n_jobs=None, penalty='l2',
                       random_state=None, solver='lbfgs', tol=0.0001, verbose=0,
                       warm_start=False)




```python
#sklearn.linear_model.LogisticRegression.score(inputs,targets)
#returns the mean accuracy on the given test data and labels
reg.score(x_train,y_train)
#the model is 78.3928 correct
#the model learn to clasify 78% of our data correctly
```




    0.775



### Manually check the accuracy


```python
#sklearn.linear_model.LogisticRegression.predict(inputs)
# predicts class labels (logistic regression outputs)for given input samples
model_outputs = reg.predict(x_train)
```


```python
#predictions
model_outputs
```




    array([0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 1, 0,
           0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1,
           1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0,
           0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 0,
           0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0,
           0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0,
           0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0,
           1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0,
           0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 1,
           1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1,
           1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1,
           0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,
           0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0,
           0, 0, 1, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0,
           1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1,
           0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1,
           0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 0, 0,
           1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 0, 0, 0,
           0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 1, 0,
           0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0,
           0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0,
           1, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 0,
           0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 0,
           0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1,
           0, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 0, 0,
           0, 1, 0, 1, 1, 1, 0, 0, 0, 0])




```python
# targets
y_train
```




    array([0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0,
           1, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1,
           1, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0,
           0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1,
           1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0,
           0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 1, 1,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1,
           0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 1, 0,
           0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1,
           1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0,
           1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0,
           0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0,
           1, 0, 1, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0,
           0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1,
           1, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1,
           0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0,
           1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0,
           0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 1,
           0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0,
           0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0,
           1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0,
           1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 1,
           0, 1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0,
           0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0,
           0, 0, 0, 1, 1, 1, 1, 0, 1, 0])




```python
model_outputs == y_train
```




    array([ True,  True,  True,  True,  True,  True,  True,  True,  True,
            True, False,  True, False, False,  True,  True,  True,  True,
           False,  True, False,  True, False, False,  True,  True,  True,
           False,  True,  True,  True,  True,  True,  True,  True,  True,
            True, False, False, False,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True,  True,
            True,  True,  True,  True, False,  True, False,  True,  True,
            True,  True,  True, False,  True,  True,  True,  True,  True,
           False,  True, False,  True,  True, False, False, False,  True,
            True,  True,  True,  True,  True,  True,  True, False,  True,
            True,  True,  True,  True,  True,  True,  True,  True,  True,
            True,  True,  True,  True, False,  True,  True,  True,  True,
           False,  True,  True,  True,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True,  True,
            True, False,  True,  True,  True,  True,  True,  True, False,
            True, False,  True, False,  True,  True,  True,  True, False,
           False, False,  True,  True, False,  True, False,  True,  True,
            True, False,  True, False,  True, False,  True, False,  True,
            True,  True,  True,  True,  True,  True,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True,  True,
            True, False,  True,  True,  True,  True,  True,  True,  True,
            True,  True,  True,  True,  True,  True, False,  True, False,
           False,  True,  True,  True,  True,  True,  True,  True, False,
            True, False,  True, False,  True,  True,  True,  True, False,
            True, False, False,  True,  True,  True,  True,  True, False,
           False, False,  True, False,  True, False,  True, False,  True,
            True,  True,  True,  True,  True,  True, False,  True,  True,
            True,  True,  True,  True,  True,  True,  True,  True,  True,
            True, False, False,  True,  True, False,  True,  True,  True,
           False,  True,  True,  True,  True,  True,  True, False, False,
           False,  True,  True,  True,  True, False,  True, False,  True,
            True,  True,  True,  True,  True,  True, False,  True, False,
           False,  True,  True,  True,  True,  True, False,  True,  True,
            True,  True, False, False,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True, False,
           False,  True,  True,  True,  True,  True, False,  True, False,
            True,  True,  True,  True, False,  True,  True,  True, False,
            True, False,  True,  True,  True,  True,  True,  True,  True,
            True,  True,  True,  True,  True,  True,  True,  True,  True,
           False,  True,  True, False,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True,  True,  True,  True,  True,
            True,  True,  True, False,  True,  True,  True, False,  True,
            True, False,  True, False,  True,  True,  True, False,  True,
            True,  True,  True,  True,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True,  True,  True,  True,  True,
           False,  True,  True, False,  True, False,  True,  True,  True,
            True,  True,  True, False,  True,  True, False,  True, False,
            True,  True,  True,  True,  True, False, False,  True,  True,
            True,  True,  True,  True,  True,  True,  True,  True,  True,
           False,  True,  True,  True, False, False,  True,  True,  True,
            True, False,  True,  True,  True,  True,  True,  True,  True,
            True,  True, False,  True,  True, False, False,  True,  True,
           False,  True,  True,  True,  True,  True,  True, False, False,
            True,  True, False,  True,  True,  True,  True, False,  True,
            True,  True,  True, False, False,  True,  True,  True,  True,
            True, False,  True,  True, False,  True,  True,  True,  True,
           False,  True,  True,  True,  True,  True, False,  True,  True,
           False, False, False,  True,  True, False,  True,  True,  True,
           False,  True,  True,  True,  True,  True,  True,  True,  True,
            True, False,  True,  True,  True,  True,  True,  True,  True,
            True,  True, False,  True,  True,  True,  True, False,  True,
           False,  True])




```python
np.sum(model_outputs==y_train)
```




    434




```python
#Accuracy = Correct Predictions / nr. observation
```


```python
model_outputs.shape[0]
```




    560




```python
#Accuracy
np.sum((model_outputs==y_train))/model_outputs.shape[0]
```




    0.775



### Finding the intercept and coefficients


```python
reg.intercept_
```




    array([-1.6561092])




```python
reg.coef_
```




    array([[ 2.80096498e+00,  9.34857518e-01,  3.09561645e+00,
             8.56587468e-01,  1.66248119e-01, -8.43703301e-02,
             6.12732578e-01, -7.79685996e-03, -1.65922708e-01,
            -1.47005122e-04,  2.71811477e-01, -2.05738037e-01,
             3.61989880e-01, -2.85510745e-01]])




```python
unscaled_inputs.columns.values
```




    array(['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4', 'Month Value',
           'Day of the Week', 'Transportation Expense', 'Distance to Work',
           'Age', 'Daily Work Load Average', 'Body Mass Index', 'Education',
           'Children', 'Pets'], dtype=object)




```python
feature_name = unscaled_inputs.columns.values
```


```python
#Create a dataframe to contain the intercept,the feature_name,coeficients
summary_table = pd.DataFrame(columns=['Feature name'],
                             data=feature_name)

summary_table['Coefficient']=np.transpose(reg.coef_)

summary_table
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
      <th>Feature name</th>
      <th>Coefficient</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Reason_1</td>
      <td>2.800965</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_2</td>
      <td>0.934858</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_3</td>
      <td>3.095616</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Reason_4</td>
      <td>0.856587</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Month Value</td>
      <td>0.166248</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Day of the Week</td>
      <td>-0.084370</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Transportation Expense</td>
      <td>0.612733</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Distance to Work</td>
      <td>-0.007797</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Age</td>
      <td>-0.165923</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Daily Work Load Average</td>
      <td>-0.000147</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Body Mass Index</td>
      <td>0.271811</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Education</td>
      <td>-0.205738</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Children</td>
      <td>0.361990</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Pets</td>
      <td>-0.285511</td>
    </tr>
  </tbody>
</table>
</div>




```python
#shift the whole data frame down one row
summary_table.index = summary_table.index+1
summary_table.loc[0] = ['Intercept',reg.intercept_[0]]
summary_table = summary_table.sort_index()
summary_table
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
      <th>Feature name</th>
      <th>Coefficient</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Intercept</td>
      <td>-1.656109</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_1</td>
      <td>2.800965</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_2</td>
      <td>0.934858</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Reason_3</td>
      <td>3.095616</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reason_4</td>
      <td>0.856587</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Month Value</td>
      <td>0.166248</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Day of the Week</td>
      <td>-0.084370</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Transportation Expense</td>
      <td>0.612733</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Distance to Work</td>
      <td>-0.007797</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Age</td>
      <td>-0.165923</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Daily Work Load Average</td>
      <td>-0.000147</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Body Mass Index</td>
      <td>0.271811</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Education</td>
      <td>-0.205738</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Children</td>
      <td>0.361990</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Pets</td>
      <td>-0.285511</td>
    </tr>
  </tbody>
</table>
</div>



### Interpreting the coefficients


```python
#log(odds) =intercept +b1x1+b2x2+...+b14x14
#where b are the coeficients
```


```python
summary_table['Odds_ratio']=np.exp(summary_table.Coefficient)
summary_table
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
      <th>Feature name</th>
      <th>Coefficient</th>
      <th>Odds_ratio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Intercept</td>
      <td>-1.656109</td>
      <td>0.190880</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_1</td>
      <td>2.800965</td>
      <td>16.460523</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_2</td>
      <td>0.934858</td>
      <td>2.546851</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Reason_3</td>
      <td>3.095616</td>
      <td>22.100858</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reason_4</td>
      <td>0.856587</td>
      <td>2.355110</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Month Value</td>
      <td>0.166248</td>
      <td>1.180866</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Day of the Week</td>
      <td>-0.084370</td>
      <td>0.919091</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Transportation Expense</td>
      <td>0.612733</td>
      <td>1.845467</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Distance to Work</td>
      <td>-0.007797</td>
      <td>0.992233</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Age</td>
      <td>-0.165923</td>
      <td>0.847112</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Daily Work Load Average</td>
      <td>-0.000147</td>
      <td>0.999853</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Body Mass Index</td>
      <td>0.271811</td>
      <td>1.312340</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Education</td>
      <td>-0.205738</td>
      <td>0.814046</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Children</td>
      <td>0.361990</td>
      <td>1.436184</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Pets</td>
      <td>-0.285511</td>
      <td>0.751630</td>
    </tr>
  </tbody>
</table>
</div>




```python
# DataFrame.sort_values(Series) sort the values in a data frame in respect to a given column(Series)
summary_table.sort_values('Odds_ratio',ascending=False)
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
      <th>Feature name</th>
      <th>Coefficient</th>
      <th>Odds_ratio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Reason_3</td>
      <td>3.095616</td>
      <td>22.100858</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_1</td>
      <td>2.800965</td>
      <td>16.460523</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_2</td>
      <td>0.934858</td>
      <td>2.546851</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reason_4</td>
      <td>0.856587</td>
      <td>2.355110</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Transportation Expense</td>
      <td>0.612733</td>
      <td>1.845467</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Children</td>
      <td>0.361990</td>
      <td>1.436184</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Body Mass Index</td>
      <td>0.271811</td>
      <td>1.312340</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Month Value</td>
      <td>0.166248</td>
      <td>1.180866</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Daily Work Load Average</td>
      <td>-0.000147</td>
      <td>0.999853</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Distance to Work</td>
      <td>-0.007797</td>
      <td>0.992233</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Day of the Week</td>
      <td>-0.084370</td>
      <td>0.919091</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Age</td>
      <td>-0.165923</td>
      <td>0.847112</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Education</td>
      <td>-0.205738</td>
      <td>0.814046</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Pets</td>
      <td>-0.285511</td>
      <td>0.751630</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Intercept</td>
      <td>-1.656109</td>
      <td>0.190880</td>
    </tr>
  </tbody>
</table>
</div>




```python
# if coef is close to 0 means not important: will be multiplied with 0 
# with this reselts that Daily Work load Average is close to 0  
#and becomes non important as does Distance to Work and Day of the Week
#based on the features given these do not make any difference
#Reason_0 is no reason has been chosen as the base for our model- no reason
```


```python
#need to go back to where we standardised the data
#and put the code in comments with #
```

### Testing the Model


```python
#Find the accuracy
reg.score(x_test,y_test)
```




    0.7428571428571429




```python
#based on the data that the test has not seen before in 74,2%
#of the cases the model will predict that a person is going to 
#be excessively absent
#test accuracy is going to be sless than train accuracy due to overfiting
```


```python
#instead of 0 and 1 we can get the probability of an output being 0 and 1
predicted_proba = reg.predict_proba(x_test)
predicted_proba
#first column probability of being 0
#second column probability of being 1
```




    array([[0.73838887, 0.26161113],
           [0.60860095, 0.39139905],
           [0.40910176, 0.59089824],
           [0.80489361, 0.19510639],
           [0.0732329 , 0.9267671 ],
           [0.31965834, 0.68034166],
           [0.31302205, 0.68697795],
           [0.13341719, 0.86658281],
           [0.79712508, 0.20287492],
           [0.75274419, 0.24725581],
           [0.48222467, 0.51777533],
           [0.1964133 , 0.8035867 ],
           [0.07857533, 0.92142467],
           [0.70622367, 0.29377633],
           [0.30708515, 0.69291485],
           [0.57055326, 0.42944674],
           [0.54143955, 0.45856045],
           [0.57205946, 0.42794054],
           [0.38194051, 0.61805949],
           [0.04857923, 0.95142077],
           [0.6977753 , 0.3022247 ],
           [0.79578125, 0.20421875],
           [0.3949288 , 0.6050712 ],
           [0.42248618, 0.57751382],
           [0.26634773, 0.73365227],
           [0.75608758, 0.24391242],
           [0.51088279, 0.48911721],
           [0.86807166, 0.13192834],
           [0.20221381, 0.79778619],
           [0.78635626, 0.21364374],
           [0.62645167, 0.37354833],
           [0.31328112, 0.68671888],
           [0.31159674, 0.68840326],
           [0.45858575, 0.54141425],
           [0.79578125, 0.20421875],
           [0.49182472, 0.50817528],
           [0.78931369, 0.21068631],
           [0.25573014, 0.74426986],
           [0.56312684, 0.43687316],
           [0.40961671, 0.59038329],
           [0.77498126, 0.22501874],
           [0.56525557, 0.43474443],
           [0.78298102, 0.21701898],
           [0.60686095, 0.39313905],
           [0.1856875 , 0.8143125 ],
           [0.42930644, 0.57069356],
           [0.30749736, 0.69250264],
           [0.72725066, 0.27274934],
           [0.79795353, 0.20204647],
           [0.81942132, 0.18057868],
           [0.40762628, 0.59237372],
           [0.65418911, 0.34581089],
           [0.33228577, 0.66771423],
           [0.71457855, 0.28542145],
           [0.15042569, 0.84957431],
           [0.52954972, 0.47045028],
           [0.11080494, 0.88919506],
           [0.74385207, 0.25614793],
           [0.68026142, 0.31973858],
           [0.68231544, 0.31768456],
           [0.27821651, 0.72178349],
           [0.3428341 , 0.6571659 ],
           [0.68801424, 0.31198576],
           [0.21288704, 0.78711296],
           [0.80153376, 0.19846624],
           [0.73465654, 0.26534346],
           [0.91807768, 0.08192232],
           [0.76974456, 0.23025544],
           [0.2729828 , 0.7270172 ],
           [0.66535124, 0.33464876],
           [0.78933713, 0.21066287],
           [0.70551061, 0.29448939],
           [0.09090252, 0.90909748],
           [0.56088305, 0.43911695],
           [0.38017902, 0.61982098],
           [0.78635626, 0.21364374],
           [0.21629641, 0.78370359],
           [0.29187436, 0.70812564],
           [0.23531607, 0.76468393],
           [0.31944675, 0.68055325],
           [0.75422935, 0.24577065],
           [0.92008641, 0.07991359],
           [0.77509813, 0.22490187],
           [0.27020432, 0.72979568],
           [0.5854272 , 0.4145728 ],
           [0.87650641, 0.12349359],
           [0.3034177 , 0.6965823 ],
           [0.43644559, 0.56355441],
           [0.76735169, 0.23264831],
           [0.32567953, 0.67432047],
           [0.83634391, 0.16365609],
           [0.87595892, 0.12404108],
           [0.77828463, 0.22171537],
           [0.71185032, 0.28814968],
           [0.7733574 , 0.2266426 ],
           [0.13219834, 0.86780166],
           [0.7290936 , 0.2709064 ],
           [0.21325641, 0.78674359],
           [0.77402702, 0.22597298],
           [0.76410694, 0.23589306],
           [0.37129308, 0.62870692],
           [0.30114391, 0.69885609],
           [0.29238488, 0.70761512],
           [0.29979282, 0.70020718],
           [0.53024524, 0.46975476],
           [0.48047234, 0.51952766],
           [0.74745112, 0.25254888],
           [0.1530908 , 0.8469092 ],
           [0.22040653, 0.77959347],
           [0.84774718, 0.15225282],
           [0.92679343, 0.07320657],
           [0.09680302, 0.90319698],
           [0.35797301, 0.64202699],
           [0.6462144 , 0.3537856 ],
           [0.44688429, 0.55311571],
           [0.40048686, 0.59951314],
           [0.19617726, 0.80382274],
           [0.19551922, 0.80448078],
           [0.47478212, 0.52521788],
           [0.69975423, 0.30024577],
           [0.76490785, 0.23509215],
           [0.82784477, 0.17215523],
           [0.16951382, 0.83048618],
           [0.52283963, 0.47716037],
           [0.78635626, 0.21364374],
           [0.61061436, 0.38938564],
           [0.80341124, 0.19658876],
           [0.88000548, 0.11999452],
           [0.25392134, 0.74607866],
           [0.68558416, 0.31441584],
           [0.34499614, 0.65500386],
           [0.79423988, 0.20576012],
           [0.68176584, 0.31823416],
           [0.65044433, 0.34955567],
           [0.70612579, 0.29387421],
           [0.48943933, 0.51056067],
           [0.55792328, 0.44207672],
           [0.69226758, 0.30773242],
           [0.74270849, 0.25729151],
           [0.5608647 , 0.4391353 ]])




```python
predicted_proba.shape
```




    (140, 2)




```python
predicted_proba[:,1]
```




    array([0.26161113, 0.39139905, 0.59089824, 0.19510639, 0.9267671 ,
           0.68034166, 0.68697795, 0.86658281, 0.20287492, 0.24725581,
           0.51777533, 0.8035867 , 0.92142467, 0.29377633, 0.69291485,
           0.42944674, 0.45856045, 0.42794054, 0.61805949, 0.95142077,
           0.3022247 , 0.20421875, 0.6050712 , 0.57751382, 0.73365227,
           0.24391242, 0.48911721, 0.13192834, 0.79778619, 0.21364374,
           0.37354833, 0.68671888, 0.68840326, 0.54141425, 0.20421875,
           0.50817528, 0.21068631, 0.74426986, 0.43687316, 0.59038329,
           0.22501874, 0.43474443, 0.21701898, 0.39313905, 0.8143125 ,
           0.57069356, 0.69250264, 0.27274934, 0.20204647, 0.18057868,
           0.59237372, 0.34581089, 0.66771423, 0.28542145, 0.84957431,
           0.47045028, 0.88919506, 0.25614793, 0.31973858, 0.31768456,
           0.72178349, 0.6571659 , 0.31198576, 0.78711296, 0.19846624,
           0.26534346, 0.08192232, 0.23025544, 0.7270172 , 0.33464876,
           0.21066287, 0.29448939, 0.90909748, 0.43911695, 0.61982098,
           0.21364374, 0.78370359, 0.70812564, 0.76468393, 0.68055325,
           0.24577065, 0.07991359, 0.22490187, 0.72979568, 0.4145728 ,
           0.12349359, 0.6965823 , 0.56355441, 0.23264831, 0.67432047,
           0.16365609, 0.12404108, 0.22171537, 0.28814968, 0.2266426 ,
           0.86780166, 0.2709064 , 0.78674359, 0.22597298, 0.23589306,
           0.62870692, 0.69885609, 0.70761512, 0.70020718, 0.46975476,
           0.51952766, 0.25254888, 0.8469092 , 0.77959347, 0.15225282,
           0.07320657, 0.90319698, 0.64202699, 0.3537856 , 0.55311571,
           0.59951314, 0.80382274, 0.80448078, 0.52521788, 0.30024577,
           0.23509215, 0.17215523, 0.83048618, 0.47716037, 0.21364374,
           0.38938564, 0.19658876, 0.11999452, 0.74607866, 0.31441584,
           0.65500386, 0.20576012, 0.31823416, 0.34955567, 0.29387421,
           0.51056067, 0.44207672, 0.30773242, 0.25729151, 0.4391353 ])




```python
#in reality logistic regression models calc these probabilities in background
#if the probability is below 0.5 it places a 0
#if prob is above 0.5 it places a 1
```


```python
#1. Save the model
#2.Create module
#Get new data and pass it through SQL and analise it in Tableau
```

# Save the Model


```python
#save the reg object
import pickle 
```


```python
with open('model','wb') as file:
    pickle.dump(reg, file)
    #model is the file name; wb is write bytes and dump is save the file    
```


```python
# pickle the scaler file
with open('scaler','wb') as file:
    pickle.dump(absenteeism_scaler, file)
```


```python
#Ceate a mechanism to load the model and deploy it or make predictions

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


