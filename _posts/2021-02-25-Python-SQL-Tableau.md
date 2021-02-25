---
layout: post
title: "Python Sql Tableau"
subtitle: "Python Sql Tableau Integration"
background: '/img/posts/python-sql-tableau/sql-tableau-datriks.jpg'
---

```python
import pandas as pd
```


```python
raw_csv_data = pd.read_csv('D:\\OneDrive - office365hubs.com\\!Python + SQL + Tableau\\Course_Resources\\5. Preprocessing\Original\\Absenteeism_data.csv')
```


```python
raw_csv_data
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
      <th>ID</th>
      <th>Reason for Absence</th>
      <th>Date</th>
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
      <td>11</td>
      <td>26</td>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>36</td>
      <td>0</td>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>23</td>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>7</td>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>23</td>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>17</td>
      <td>10</td>
      <td>23/05/2018</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>696</th>
      <td>28</td>
      <td>6</td>
      <td>23/05/2018</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>697</th>
      <td>18</td>
      <td>10</td>
      <td>24/05/2018</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>698</th>
      <td>25</td>
      <td>23</td>
      <td>24/05/2018</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>699</th>
      <td>15</td>
      <td>28</td>
      <td>31/05/2018</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 12 columns</p>
</div>




```python
df = raw_csv_data.copy()
```


```python
df
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
      <th>ID</th>
      <th>Reason for Absence</th>
      <th>Date</th>
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
      <td>11</td>
      <td>26</td>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>36</td>
      <td>0</td>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>23</td>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>7</td>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>23</td>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>17</td>
      <td>10</td>
      <td>23/05/2018</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>696</th>
      <td>28</td>
      <td>6</td>
      <td>23/05/2018</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>697</th>
      <td>18</td>
      <td>10</td>
      <td>24/05/2018</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>698</th>
      <td>25</td>
      <td>23</td>
      <td>24/05/2018</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>699</th>
      <td>15</td>
      <td>28</td>
      <td>31/05/2018</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 12 columns</p>
</div>




```python
type(df)
```




    pandas.core.frame.DataFrame




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 700 entries, 0 to 699
    Data columns (total 12 columns):
     #   Column                     Non-Null Count  Dtype  
    ---  ------                     --------------  -----  
     0   ID                         700 non-null    int64  
     1   Reason for Absence         700 non-null    int64  
     2   Date                       700 non-null    object 
     3   Transportation Expense     700 non-null    int64  
     4   Distance to Work           700 non-null    int64  
     5   Age                        700 non-null    int64  
     6   Daily Work Load Average    700 non-null    float64
     7   Body Mass Index            700 non-null    int64  
     8   Education                  700 non-null    int64  
     9   Children                   700 non-null    int64  
     10  Pets                       700 non-null    int64  
     11  Absenteeism Time in Hours  700 non-null    int64  
    dtypes: float64(1), int64(10), object(1)
    memory usage: 65.8+ KB
    


```python
display(df)
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
      <th>ID</th>
      <th>Reason for Absence</th>
      <th>Date</th>
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
      <td>11</td>
      <td>26</td>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>36</td>
      <td>0</td>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>23</td>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>7</td>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>23</td>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>17</td>
      <td>10</td>
      <td>23/05/2018</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>696</th>
      <td>28</td>
      <td>6</td>
      <td>23/05/2018</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>697</th>
      <td>18</td>
      <td>10</td>
      <td>24/05/2018</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>698</th>
      <td>25</td>
      <td>23</td>
      <td>24/05/2018</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>699</th>
      <td>15</td>
      <td>28</td>
      <td>31/05/2018</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 12 columns</p>
</div>



```python
df.drop(['ID'],axis = 1)
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
      <th>Reason for Absence</th>
      <th>Date</th>
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
      <td>26</td>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23</td>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>10</td>
      <td>23/05/2018</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>696</th>
      <td>6</td>
      <td>23/05/2018</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>697</th>
      <td>10</td>
      <td>24/05/2018</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>698</th>
      <td>23</td>
      <td>24/05/2018</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>699</th>
      <td>28</td>
      <td>31/05/2018</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 11 columns</p>
</div>




```python
df = df.drop(['ID'],axis=1)
```


```python
df
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
      <th>Reason for Absence</th>
      <th>Date</th>
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
      <td>26</td>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23</td>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>10</td>
      <td>23/05/2018</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>696</th>
      <td>6</td>
      <td>23/05/2018</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>697</th>
      <td>10</td>
      <td>24/05/2018</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>698</th>
      <td>23</td>
      <td>24/05/2018</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>699</th>
      <td>28</td>
      <td>31/05/2018</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 11 columns</p>
</div>




```python
raw_csv_data
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
      <th>ID</th>
      <th>Reason for Absence</th>
      <th>Date</th>
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
      <td>11</td>
      <td>26</td>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>36</td>
      <td>0</td>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>23</td>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>7</td>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>23</td>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>17</td>
      <td>10</td>
      <td>23/05/2018</td>
      <td>179</td>
      <td>22</td>
      <td>40</td>
      <td>237.656</td>
      <td>22</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>696</th>
      <td>28</td>
      <td>6</td>
      <td>23/05/2018</td>
      <td>225</td>
      <td>26</td>
      <td>28</td>
      <td>237.656</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>697</th>
      <td>18</td>
      <td>10</td>
      <td>24/05/2018</td>
      <td>330</td>
      <td>16</td>
      <td>28</td>
      <td>237.656</td>
      <td>25</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>698</th>
      <td>25</td>
      <td>23</td>
      <td>24/05/2018</td>
      <td>235</td>
      <td>16</td>
      <td>32</td>
      <td>237.656</td>
      <td>25</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>699</th>
      <td>15</td>
      <td>28</td>
      <td>31/05/2018</td>
      <td>291</td>
      <td>31</td>
      <td>40</td>
      <td>237.656</td>
      <td>25</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 12 columns</p>
</div>




```python
df['Reason for Absence']
```




    0      26
    1       0
    2      23
    3       7
    4      23
           ..
    695    10
    696     6
    697    10
    698    23
    699    28
    Name: Reason for Absence, Length: 700, dtype: int64




```python
#Sorting a list of numbers
sorted(df['Reason for Absence'].unique())
```




    [0,
     1,
     2,
     3,
     4,
     5,
     6,
     7,
     8,
     9,
     10,
     11,
     12,
     13,
     14,
     15,
     16,
     17,
     18,
     19,
     21,
     22,
     23,
     24,
     25,
     26,
     27,
     28]




```python
#Splitting a column in multiple dummies
#converts categorical variable into dummy variable get_dummies()
reason_columns = pd.get_dummies(df['Reason for Absence'])
reason_columns.head(10)
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>18</th>
      <th>19</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 28 columns</p>
</div>




```python
reason_columns['check'] = reason_columns.sum(axis=1)
```


```python
reason_columns.head(1)
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>19</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>check</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 29 columns</p>
</div>




```python
reason_columns['check'].sum(axis=0)
```




    700




```python
reason_columns['check'].unique()
```




    array([1], dtype=int64)




```python
reason_columns = reason_columns.drop(['check'],axis=1)
```


```python
reason_columns.head(2)
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>18</th>
      <th>19</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 28 columns</p>
</div>




```python
#Dummy variable and their statistical importance
#We are gona grop the first column with the reason 0
reason_columns = pd.get_dummies(df['Reason for Absence'],drop_first=True)
```


```python
reason_columns
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
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>...</th>
      <th>18</th>
      <th>19</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>695</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>696</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>697</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>698</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>699</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 27 columns</p>
</div>




```python
#Grouping-Transforming dummy var in categorical variable
#group the reason of Absence
df.columns.values
```




    array(['Reason for Absence', 'Date', 'Transportation Expense',
           'Distance to Work', 'Age', 'Daily Work Load Average',
           'Body Mass Index', 'Education', 'Children', 'Pets',
           'Absenteeism Time in Hours'], dtype=object)




```python
reason_columns.columns.values
```




    array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17,
           18, 19, 21, 22, 23, 24, 25, 26, 27, 28], dtype=int64)




```python
df = df.drop(['Reason for Absence'],axis=1)
```


```python
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
      <th>Date</th>
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
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Adding reason_columns data set to df data est
#Clasification or grouping
#Split the reason_columns data frame
reason_type_1 = reason_columns.loc[:,1:14].max(axis=1)
reason_type_2 = reason_columns.loc[:,15:17].max(axis=1)
reason_type_3 = reason_columns.loc[:,18:21].max(axis=1)
reason_type_4 = reason_columns.loc[:,22:].max(axis=1)
```


```python
#Concatenating columns in Python
df.columns.values
```




    array(['Date', 'Transportation Expense', 'Distance to Work', 'Age',
           'Daily Work Load Average', 'Body Mass Index', 'Education',
           'Children', 'Pets', 'Absenteeism Time in Hours'], dtype=object)




```python
reason_columns.columns.values
```




    array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17,
           18, 19, 21, 22, 23, 24, 25, 26, 27, 28], dtype=int64)




```python
#joinnig the columns 
df = pd.concat([df,reason_type_1,reason_type_2,
                reason_type_3,reason_type_4],axis=1)
```


```python
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
      <th>Date</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Absenteeism Time in Hours</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.columns.values
```




    array(['Date', 'Transportation Expense', 'Distance to Work', 'Age',
           'Daily Work Load Average', 'Body Mass Index', 'Education',
           'Children', 'Pets', 'Absenteeism Time in Hours', 0, 1, 2, 3],
          dtype=object)




```python
column_names = ['Date', 'Transportation Expense', 'Distance to Work', 'Age',
       'Daily Work Load Average', 'Body Mass Index', 'Education',
       'Children', 'Pets', 'Absenteeism Time in Hours', 'Reason_1', 'Reason_2', 'Reason_3', 'Reason_4']
```


```python
df.columns =column_names
```


```python
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
      <th>Date</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Absenteeism Time in Hours</th>
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Reorder the columns of a data frame
column_names_reordered = ['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4',
                          'Date', 'Transportation Expense', 'Distance to Work', 'Age',
       'Daily Work Load Average', 'Body Mass Index', 'Education',
       'Children', 'Pets', 'Absenteeism Time in Hours']
```


```python
df = df[column_names_reordered]
```


```python
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
      <th>Reason_1</th>
      <th>Reason_2</th>
      <th>Reason_3</th>
      <th>Reason_4</th>
      <th>Date</th>
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
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
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
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
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
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
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
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
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
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



## Create a checkpoint


```python
#Create a checkp[oint]
#For a checkpoint you have to create a copy of the data frame in the state that is now
df_reason_mod = df.copy()
```


```python
df_reason_mod.head()
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
      <th>Date</th>
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
      <td>07/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
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
      <td>14/07/2015</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
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
      <td>15/07/2015</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
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
      <td>16/07/2015</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
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
      <td>23/07/2015</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_reason_mod.columns.values
```




    array(['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4', 'Date',
           'Transportation Expense', 'Distance to Work', 'Age',
           'Daily Work Load Average', 'Body Mass Index', 'Education',
           'Children', 'Pets', 'Absenteeism Time in Hours'], dtype=object)




```python
df_reason_mod['Date']
```




    0      07/07/2015
    1      14/07/2015
    2      15/07/2015
    3      16/07/2015
    4      23/07/2015
              ...    
    695    23/05/2018
    696    23/05/2018
    697    24/05/2018
    698    24/05/2018
    699    31/05/2018
    Name: Date, Length: 700, dtype: object




```python
type(df_reason_mod['Date'][0])
```




    str




```python
#using pandas timestamp
#when doing the conversion from text to date specify the proper 
#format of the date you will be working on
df_reason_mod['Date'] = pd.to_datetime(df_reason_mod['Date'],
                                       format='%d/%m/%Y')
```


```python
type(df_reason_mod['Date'])
```




    pandas.core.series.Series




```python
df_reason_mod.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 700 entries, 0 to 699
    Data columns (total 14 columns):
     #   Column                     Non-Null Count  Dtype         
    ---  ------                     --------------  -----         
     0   Reason_1                   700 non-null    uint8         
     1   Reason_2                   700 non-null    uint8         
     2   Reason_3                   700 non-null    uint8         
     3   Reason_4                   700 non-null    uint8         
     4   Date                       700 non-null    datetime64[ns]
     5   Transportation Expense     700 non-null    int64         
     6   Distance to Work           700 non-null    int64         
     7   Age                        700 non-null    int64         
     8   Daily Work Load Average    700 non-null    float64       
     9   Body Mass Index            700 non-null    int64         
     10  Education                  700 non-null    int64         
     11  Children                   700 non-null    int64         
     12  Pets                       700 non-null    int64         
     13  Absenteeism Time in Hours  700 non-null    int64         
    dtypes: datetime64[ns](1), float64(1), int64(8), uint8(4)
    memory usage: 57.5 KB
    

# Extract the Month value


```python
df_reason_mod['Date'][0]
```




    Timestamp('2015-07-07 00:00:00')




```python
df_reason_mod['Date'][0].month
```




    7




```python
df_reason_mod.shape
```




    (700, 14)




```python
list_months = []
for i in range(700):
    list_months.append(df_reason_mod['Date'][i].month)
```


```python
list_months
```




    [7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     6,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     7,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     8,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     9,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     10,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     11,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     12,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5,
     5]




```python
len(list_months)
```




    700




```python
df_reason_mod['Month Value'] = list_months
```


```python
df_reason_mod.head()
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
      <th>Date</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Absenteeism Time in Hours</th>
      <th>Month Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2015-07-07</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2015-07-14</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2015-07-15</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2015-07-16</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2015-07-23</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_reason_mod.columns.values
```




    array(['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4', 'Date',
           'Transportation Expense', 'Distance to Work', 'Age',
           'Daily Work Load Average', 'Body Mass Index', 'Education',
           'Children', 'Pets', 'Absenteeism Time in Hours', 'Month Value'],
          dtype=object)



## Extracting the day of the week


```python
df_reason_mod['Date'][699].weekday()
```




    3




```python
df_reason_mod['Date'][699]
```




    Timestamp('2018-05-31 00:00:00')




```python
def date_to_weekday(date_value):
    return date_value.weekday()
```


```python
df_reason_mod['Day of the Week'] = df_reason_mod['Date'].apply(date_to_weekday)
```


```python
df_reason_mod.head()
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
      <th>Date</th>
      <th>Transportation Expense</th>
      <th>Distance to Work</th>
      <th>Age</th>
      <th>Daily Work Load Average</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Absenteeism Time in Hours</th>
      <th>Month Value</th>
      <th>Day of the Week</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2015-07-07</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2015-07-14</td>
      <td>118</td>
      <td>13</td>
      <td>50</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2015-07-15</td>
      <td>179</td>
      <td>51</td>
      <td>38</td>
      <td>239.554</td>
      <td>31</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2015-07-16</td>
      <td>279</td>
      <td>5</td>
      <td>39</td>
      <td>239.554</td>
      <td>24</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2015-07-23</td>
      <td>289</td>
      <td>36</td>
      <td>33</td>
      <td>239.554</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>7</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_reason_mod = df_reason_mod.drop(['Date'],axis=1)
```


```python
df_reason_mod.columns.values
```




    array(['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4',
           'Transportation Expense', 'Distance to Work', 'Age',
           'Daily Work Load Average', 'Body Mass Index', 'Education',
           'Children', 'Pets', 'Absenteeism Time in Hours', 'Month Value',
           'Day of the Week'], dtype=object)




```python
col_mod = ['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4', 'Month Value',
       'Day of the Week','Transportation Expense', 'Distance to Work', 'Age',
       'Daily Work Load Average', 'Body Mass Index', 'Education',
       'Children', 'Pets', 'Absenteeism Time in Hours']
```


```python
df_reason_mod = df_reason_mod[col_mod]
```


```python
df_reason_mod.head()
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



## Checkpoint after Date drop


```python
df_reason_date_mod = df_reason_mod.copy()
```


```python
df_reason_date_mod.head()
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
type(df_reason_date_mod['Transportation Expense'][0])
```




    numpy.int64




```python
type(df_reason_date_mod['Distance to Work'][0])
```




    numpy.int64




```python
type(df_reason_date_mod['Age'][0])
```




    numpy.int64




```python
type(df_reason_date_mod['Body Mass Index'][0])
```




    numpy.int64




```python
#transform Education into a dummy variable
df_reason_date_mod['Education'].unique()
```




    array([1, 3, 2, 4], dtype=int64)




```python
df_reason_date_mod['Education'].value_counts()
```




    1    583
    3     73
    2     40
    4      4
    Name: Education, dtype: int64




```python
df_reason_date_mod['Education'] = df_reason_date_mod['Education'].map({1:0,2:1,3:1,4:1})
```


```python
df_reason_date_mod['Education'].unique()
```




    array([0, 1], dtype=int64)




```python
df_reason_date_mod['Education'].value_counts()
```




    0    583
    1    117
    Name: Education, dtype: int64



## Final Checkpoint


```python
df_preprocessed = df_reason_date_mod.copy()
```


```python
df_preprocessed.head(10)
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
    <tr>
      <th>5</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>4</td>
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
      <th>6</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>4</td>
      <td>361</td>
      <td>52</td>
      <td>28</td>
      <td>239.554</td>
      <td>27</td>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>8</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>4</td>
      <td>260</td>
      <td>50</td>
      <td>36</td>
      <td>239.554</td>
      <td>23</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>155</td>
      <td>12</td>
      <td>34</td>
      <td>239.554</td>
      <td>25</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>40</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>0</td>
      <td>235</td>
      <td>11</td>
      <td>37</td>
      <td>239.554</td>
      <td>29</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_preprocessed.to_csv('Absenteeism_preprocessed.csv',index=False)
```


```python
df_preprocessed.to_csv('D:\\OneDrive - office365hubs.com\\!Python + SQL + Tableau\\Absenteeism_preprocessed.csv',index=False)
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


```python

```


```python

```
