---
layout: post
title: "Logistic Regression In Python"
subtitle: "Developed Logistic Regression"
background: '/img/posts/logistic-regression/man-and-tablet.jpg'
---

# Creating a logistic regression to predict absenteeism

## Import the relevant libraries


```python
# import the relevant libraries
import pandas as pd
import numpy as np
```

## Load the data


```python
# load the preprocessed CSV data
data_preprocessed = pd.read_csv('Absenteeism_preprocessed.csv')
```


```python
# eyeball the data
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
# find the median of 'Absenteeism Time in Hours'
data_preprocessed['Absenteeism Time in Hours'].median()
```




    3.0




```python
# create targets for our logistic regression
# they have to be categories and we must find a way to say if someone is 'being absent too much' or not
# what we've decided to do is to take the median of the dataset as a cut-off line
# in this way the dataset will be balanced (there will be roughly equal number of 0s and 1s for the logistic regression)
# as balancing is a great problem for ML, this will work great for us
# alternatively, if we had more data, we could have found other ways to deal with the issue 
# for instance, we could have assigned some arbitrary value as a cut-off line, instead of the median

# note that what line does is to assign 1 to anyone who has been absent 4 hours or more (more than 3 hours)
# that is the equivalent of taking half a day off

# initial code from the lecture
# targets = np.where(data_preprocessed['Absenteeism Time in Hours'] > 3, 1, 0)

# parameterized code
targets = np.where(data_preprocessed['Absenteeism Time in Hours'] > 
                   data_preprocessed['Absenteeism Time in Hours'].median(), 1, 0)
```


```python
# eyeball the targets
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
# create a Series in the original data frame that will contain the targets for the regression
data_preprocessed['Excessive Absenteeism'] = targets
```


```python
# check what happened
# maybe manually see how the targets were created
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
      <th>Excessive Absenteeism</th>
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
  </tbody>
</table>
</div>



## A comment on the targets


```python
# check if dataset is balanced (what % of targets are 1s)
# targets.sum() will give us the number of 1s that there are
# the shape[0] will give us the length of the targets array
targets.sum() / targets.shape[0]
```




    0.45571428571428574




```python
# create a checkpoint by dropping the unnecessary variables
# also drop the variables we 'eliminated' after exploring the weights
data_with_targets = data_preprocessed.drop(['Absenteeism Time in Hours','Day of the Week',
                                            'Daily Work Load Average','Distance to Work'],axis=1)
```


```python
# check if the line above is a checkpoint :)

# if data_with_targets is data_preprocessed = True, then the two are pointing to the same object
# if it is False, then the two variables are completely different and this is in fact a checkpoint
data_with_targets is data_preprocessed
```




    False




```python
# check what's inside
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
      <th>Transportation Expense</th>
      <th>Age</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Excessive Absenteeism</th>
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
      <td>289</td>
      <td>33</td>
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
      <td>118</td>
      <td>50</td>
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
      <td>179</td>
      <td>38</td>
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
      <td>279</td>
      <td>39</td>
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
      <td>289</td>
      <td>33</td>
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




    (700, 12)




```python
# Selects all rows and all columns until 14 (excluding)
data_with_targets.iloc[:,:14]
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
      <th>Transportation Expense</th>
      <th>Age</th>
      <th>Body Mass Index</th>
      <th>Education</th>
      <th>Children</th>
      <th>Pets</th>
      <th>Excessive Absenteeism</th>
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
      <td>289</td>
      <td>33</td>
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
      <td>118</td>
      <td>50</td>
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
      <td>179</td>
      <td>38</td>
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
      <td>279</td>
      <td>39</td>
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
      <td>289</td>
      <td>33</td>
      <td>30</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>179</td>
      <td>40</td>
      <td>22</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>696</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>225</td>
      <td>28</td>
      <td>24</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>697</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>330</td>
      <td>28</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>698</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>235</td>
      <td>32</td>
      <td>25</td>
      <td>1</td>
      <td>0</td>
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
      <td>291</td>
      <td>40</td>
      <td>25</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 12 columns</p>
</div>




```python
# Selects all rows and all columns but the last one (basically the same operation)
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
      <th>Transportation Expense</th>
      <th>Age</th>
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
      <td>289</td>
      <td>33</td>
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
      <td>118</td>
      <td>50</td>
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
      <td>179</td>
      <td>38</td>
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
      <td>279</td>
      <td>39</td>
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
      <td>289</td>
      <td>33</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>179</td>
      <td>40</td>
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
      <td>225</td>
      <td>28</td>
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
      <td>330</td>
      <td>28</td>
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
      <td>235</td>
      <td>32</td>
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
      <td>291</td>
      <td>40</td>
      <td>25</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 11 columns</p>
</div>




```python
# Create a variable that will contain the inputs (everything without the targets)
unscaled_inputs = data_with_targets.iloc[:,:-1]
```

## Standardize the data


```python
# standardize the inputs

# standardization is one of the most common preprocessing tools
# since data of different magnitude (scale) can be biased towards high values,
# we want all inputs to be of similar magnitude
# this is a peculiarity of machine learning in general - most (but not all) algorithms do badly with unscaled data

# a very useful module we can use is StandardScaler 
# it has much more capabilities than the straightforward 'preprocessing' method
from sklearn.preprocessing import StandardScaler


# we will create a variable that will contain the scaling information for this particular dataset
# here's the full documentation: http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html

# define scaler as an object
absenteeism_scaler = StandardScaler()
```


```python
# import the libraries needed to create the Custom Scaler
# note that all of them are a part of the sklearn package
# moreover, one of them is actually the StandardScaler module, 
# so you can imagine that the Custom Scaler is build on it

from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.preprocessing import StandardScaler

# create the Custom Scaler class

class CustomScaler(BaseEstimator,TransformerMixin): 
    
    # init or what information we need to declare a CustomScaler object
    # and what is calculated/declared as we do
    
    def __init__(self,columns,copy=True,with_mean=True,with_std=True):
        
        # scaler is nothing but a Standard Scaler object
        self.scaler = StandardScaler(copy,with_mean,with_std)
        # with some columns 'twist'
        self.columns = columns
        self.mean_ = None
        self.var_ = None
        
    
    # the fit method, which, again based on StandardScale
    
    def fit(self, X, y=None):
        self.scaler.fit(X[self.columns], y)
        self.mean_ = np.mean(X[self.columns])
        self.var_ = np.var(X[self.columns])
        return self
    
    # the transform method which does the actual scaling

    def transform(self, X, y=None, copy=None):
        
        # record the initial order of the columns
        init_col_order = X.columns
        
        # scale all features that you chose when creating the instance of the class
        X_scaled = pd.DataFrame(self.scaler.transform(X[self.columns]), columns=self.columns)
        
        # declare a variable containing all information that was not scaled
        X_not_scaled = X.loc[:,~X.columns.isin(self.columns)]
        
        # return a data frame which contains all scaled features and all 'not scaled' features
        # use the original order (that you recorded in the beginning)
        return pd.concat([X_not_scaled, X_scaled], axis=1)[init_col_order]
```


```python
# check what are all columns that we've got
unscaled_inputs.columns.values
```




    array(['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4', 'Month Value',
           'Transportation Expense', 'Age', 'Body Mass Index', 'Education',
           'Children', 'Pets'], dtype=object)




```python
# choose the columns to scale
# we later augmented this code and put it in comments
# columns_to_scale = ['Month Value','Day of the Week', 'Transportation Expense', 'Distance to Work',
       #'Age', 'Daily Work Load Average', 'Body Mass Index', 'Children', 'Pet']
    
# select the columns to omit
columns_to_omit = ['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4','Education']
```


```python
# create the columns to scale, based on the columns to omit
# use list comprehension to iterate over the list
columns_to_scale = [x for x in unscaled_inputs.columns.values if x not in columns_to_omit]
```


```python
# declare a scaler object, specifying the columns you want to scale
absenteeism_scaler = CustomScaler(columns_to_scale)
```


```python
# fit the data (calculate mean and standard deviation); they are automatically stored inside the object 
absenteeism_scaler.fit(unscaled_inputs)
```

    c:\python\python38\lib\site-packages\sklearn\base.py:193: FutureWarning: From version 0.24, get_params will raise an AttributeError if a parameter cannot be retrieved as an instance attribute. Previously it would return None.
      warnings.warn('From version 0.24, get_params will raise an '
    




    CustomScaler(columns=['Month Value', 'Transportation Expense', 'Age',
                          'Body Mass Index', 'Children', 'Pets'],
                 copy=None, with_mean=None, with_std=None)




```python
# standardizes the data, using the transform method 
# in the last line, we fitted the data - in other words
# we found the internal parameters of a model that will be used to transform data. 
# transforming applies these parameters to our data
# note that when you get new data, you can just call 'scaler' again and transform it in the same way as now
scaled_inputs = absenteeism_scaler.transform(unscaled_inputs)
```


```python
# the scaled_inputs are now an ndarray, because sklearn works with ndarrays
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
      <th>Transportation Expense</th>
      <th>Age</th>
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
      <td>1.005844</td>
      <td>-0.536062</td>
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
      <td>-1.574681</td>
      <td>2.130803</td>
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
      <td>-0.654143</td>
      <td>0.248310</td>
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
      <td>0.854936</td>
      <td>0.405184</td>
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
      <td>1.005844</td>
      <td>-0.536062</td>
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
    </tr>
    <tr>
      <th>695</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-0.388293</td>
      <td>-0.654143</td>
      <td>0.562059</td>
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
      <td>0.040034</td>
      <td>-1.320435</td>
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
      <td>1.624567</td>
      <td>-1.320435</td>
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
      <td>0.190942</td>
      <td>-0.692937</td>
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
      <td>1.036026</td>
      <td>0.562059</td>
      <td>-0.408580</td>
      <td>0</td>
      <td>-0.019280</td>
      <td>0.268487</td>
    </tr>
  </tbody>
</table>
<p>700 rows × 11 columns</p>
</div>




```python
# check the shape of the inputs
scaled_inputs.shape
```




    (700, 11)



## Split the data into train & test and shuffle

### Import the relevant module


```python
# import train_test_split so we can split our data into train and test
from sklearn.model_selection import train_test_split
```

### Split


```python
# check how this method works
train_test_split(scaled_inputs, targets)
```




    [     Reason_1  Reason_2  Reason_3  Reason_4  Month Value  \
     208         0         0         1         0    -0.388293   
     593         0         0         0         1    -1.244823   
     209         1         0         0         0    -0.388293   
     11          1         0         0         0     0.182726   
     290         0         0         0         1     1.039256   
     ..        ...       ...       ...       ...          ...   
     363         0         0         0         1    -1.530333   
     566         0         0         0         1     1.610276   
     475         0         0         0         1     0.182726   
     167         1         0         0         0    -0.959313   
     351         0         0         0         1     1.610276   
     
          Transportation Expense       Age  Body Mass Index  Education  Children  \
     208                0.040034 -1.320435        -0.643782          0 -0.019280   
     593               -0.654143  0.248310         1.002633          0 -0.919030   
     209               -0.578689 -1.477309        -1.349389          0 -0.919030   
     11                 0.568211 -0.065439        -0.878984          0  2.679969   
     290                0.190942  0.091435         0.532229          1 -0.019280   
     ..                      ...       ...              ...        ...       ...   
     363                1.005844 -0.536062         0.767431          0  0.880469   
     566                0.040034 -1.320435        -0.643782          0 -0.019280   
     475                1.005844 -0.536062         0.767431          0  0.880469   
     167               -1.016322 -0.379188        -0.408580          0  0.880469   
     351               -0.654143  0.248310         1.002633          0 -0.919030   
     
              Pets  
     208  1.126663  
     593 -0.589690  
     209 -0.589690  
     11  -0.589690  
     290  0.268487  
     ..        ...  
     363  0.268487  
     566  1.126663  
     475  0.268487  
     167 -0.589690  
     351 -0.589690  
     
     [525 rows x 11 columns],
          Reason_1  Reason_2  Reason_3  Reason_4  Month Value  \
     653         0         0         0         1    -0.959313   
     592         0         0         0         1    -1.244823   
     396         0         0         0         1    -0.959313   
     498         0         0         0         1     0.753746   
     694         0         0         0         1    -0.388293   
     ..        ...       ...       ...       ...          ...   
     18          1         0         0         0     0.182726   
     327         1         0         0         0     1.324766   
     3           1         0         0         0     0.182726   
     437         0         0         0         1    -0.388293   
     150         0         0         0         1    -1.244823   
     
          Transportation Expense       Age  Body Mass Index  Education  Children  \
     653                2.213108 -0.849811        -0.408580          0  1.780219   
     592                0.040034  0.718933         0.297027          1  0.880469   
     396               -0.654143  0.562059        -1.114186          1  0.880469   
     498               -1.016322 -0.379188        -0.408580          0  0.880469   
     694                1.036026  0.562059        -0.408580          0 -0.019280   
     ..                      ...       ...              ...        ...       ...   
     18                -0.503235 -0.536062        -0.408580          0  0.880469   
     327               -1.574681  0.091435         0.297027          0 -0.919030   
     3                  0.854936  0.405184        -0.643782          0  0.880469   
     437               -0.654143 -1.006686        -1.819793          1 -0.919030   
     150                0.040034 -1.320435        -0.643782          0 -0.019280   
     
              Pets  
     653 -0.589690  
     592  1.126663  
     396 -0.589690  
     498 -0.589690  
     694  0.268487  
     ..        ...  
     18   1.126663  
     327 -0.589690  
     3   -0.589690  
     437 -0.589690  
     150  1.126663  
     
     [175 rows x 11 columns],
     array([1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 0,
            0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0,
            1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1,
            1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0,
            1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 1, 1, 0, 1, 0, 0,
            0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1,
            0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0,
            1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 0, 1, 0,
            1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1,
            0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0,
            0, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1, 0, 1, 1,
            0, 0, 1, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1,
            1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 1,
            0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1,
            0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 1, 1,
            0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 0,
            0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1,
            0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0,
            0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0,
            0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1,
            1, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 0,
            1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0,
            1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0,
            1, 1, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 1, 0]),
     array([1, 0, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0,
            1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 1, 1, 1, 0, 0, 0,
            1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1,
            0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 0, 0,
            1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0,
            0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0,
            0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1,
            1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0])]




```python
# declare 4 variables for the split
x_train, x_test, y_train, y_test = train_test_split(scaled_inputs, targets, #train_size = 0.8, 
                                                                            test_size = 0.2, random_state = 20)
```


```python
# check the shape of the train inputs and targets
print (x_train.shape, y_train.shape)
```

    (560, 11) (560,)
    


```python
# check the shape of the test inputs and targets
print (x_test.shape, y_test.shape)
```

    (140, 11) (140,)
    

## Logistic regression with sklearn


```python
# import the LogReg model from sklearn
from sklearn.linear_model import LogisticRegression

# import the 'metrics' module, which includes important metrics we may want to use
from sklearn import metrics
```

### Training the model


```python
# create a logistic regression object
reg = LogisticRegression()
```


```python
# fit our train inputs
# that is basically the whole training part of the machine learning
reg.fit(x_train,y_train)
```




    LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
                       intercept_scaling=1, l1_ratio=None, max_iter=100,
                       multi_class='auto', n_jobs=None, penalty='l2',
                       random_state=None, solver='lbfgs', tol=0.0001, verbose=0,
                       warm_start=False)




```python
# assess the train accuracy of the model
reg.score(x_train,y_train)
```




    0.7732142857142857



### Manually check the accuracy


```python
# find the model outputs according to our model
model_outputs = reg.predict(x_train)
model_outputs
```




    array([0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 1, 0,
           0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1,
           1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0,
           0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 0,
           0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0,
           0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0,
           0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0,
           1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0,
           0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 1,
           1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1,
           1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 0, 1,
           0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1,
           0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0,
           0, 0, 1, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0,
           1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1,
           0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1,
           0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 0, 0,
           1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 0, 0, 0,
           0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 1, 0,
           0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0,
           1, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 0,
           0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0,
           0, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1,
           0, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 0, 0,
           0, 1, 0, 1, 1, 1, 0, 0, 0, 0])




```python
# compare them with the targets
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
# ACTUALLY compare the two variables
model_outputs == y_train
```




    array([ True,  True,  True,  True,  True,  True,  True,  True,  True,
            True, False,  True, False, False,  True,  True,  True,  True,
           False,  True, False,  True, False, False,  True,  True,  True,
           False,  True,  True,  True,  True,  True,  True,  True,  True,
           False, False, False, False,  True,  True,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True,  True, False,  True,  True,
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
           False, False,  True, False,  True,  True,  True, False,  True,
            True,  True,  True,  True,  True,  True, False,  True,  True,
            True,  True,  True,  True,  True,  True, False,  True,  True,
            True, False, False,  True,  True,  True,  True,  True,  True,
           False,  True,  True,  True,  True,  True,  True, False, False,
           False,  True,  True,  True,  True, False,  True, False,  True,
            True,  True,  True,  True,  True,  True, False,  True, False,
           False,  True,  True,  True,  True,  True, False,  True,  True,
            True,  True, False, False,  True, False,  True,  True,  True,
            True,  True,  True,  True,  True, False,  True,  True, False,
           False,  True,  True,  True,  True,  True, False,  True, False,
            True,  True,  True, False, False,  True,  True,  True, False,
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
            True,  True, False,  True,  True,  True,  True, False,  True,
           False,  True,  True,  True, False, False,  True,  True,  True,
            True, False,  True,  True,  True,  True,  True,  True,  True,
            True,  True, False,  True,  True, False, False,  True,  True,
           False,  True,  True,  True,  True,  True,  True, False, False,
            True,  True, False,  True,  True,  True,  True, False,  True,
            True,  True,  True,  True, False,  True,  True,  True,  True,
            True, False,  True,  True, False, False,  True,  True,  True,
           False,  True,  True,  True,  True,  True, False,  True,  True,
           False, False, False,  True,  True, False,  True,  True,  True,
           False,  True,  True,  True,  True,  True,  True,  True,  True,
            True, False,  True,  True,  True,  True,  True,  True,  True,
            True,  True, False,  True,  True,  True,  True, False,  True,
           False,  True])




```python
# find out in how many instances we predicted correctly
np.sum((model_outputs==y_train))
```




    433




```python
# get the total number of instances
model_outputs.shape[0]
```




    560




```python
# calculate the accuracy of the model
np.sum((model_outputs==y_train)) / model_outputs.shape[0]
```




    0.7732142857142857



### Finding the intercept and coefficients


```python
# get the intercept (bias) of our model
reg.intercept_
```




    array([-1.6474549])




```python
# get the coefficients (weights) of our model
reg.coef_
```




    array([[ 2.80019733,  0.95188356,  3.11555338,  0.83900082,  0.1589299 ,
             0.60528415, -0.16989096,  0.27981088, -0.21053312,  0.34826214,
            -0.27739602]])




```python
# check what were the names of our columns
unscaled_inputs.columns.values
```




    array(['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4', 'Month Value',
           'Transportation Expense', 'Age', 'Body Mass Index', 'Education',
           'Children', 'Pets'], dtype=object)




```python
# save the names of the columns in an ad-hoc variable
feature_name = unscaled_inputs.columns.values
```


```python
# use the coefficients from this table (they will be exported later and will be used in Tableau)
# transpose the model coefficients (model.coef_) and throws them into a df (a vertical organization, so that they can be
# multiplied by certain matrices later) 
summary_table = pd.DataFrame (columns=['Feature name'], data = feature_name)

# add the coefficient values to the summary table
summary_table['Coefficient'] = np.transpose(reg.coef_)

# display the summary table
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
      <td>2.800197</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_2</td>
      <td>0.951884</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_3</td>
      <td>3.115553</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Reason_4</td>
      <td>0.839001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Month Value</td>
      <td>0.158930</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Transportation Expense</td>
      <td>0.605284</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Age</td>
      <td>-0.169891</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Body Mass Index</td>
      <td>0.279811</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Education</td>
      <td>-0.210533</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Children</td>
      <td>0.348262</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Pets</td>
      <td>-0.277396</td>
    </tr>
  </tbody>
</table>
</div>




```python
# do a little Python trick to move the intercept to the top of the summary table
# move all indices by 1
summary_table.index = summary_table.index + 1

# add the intercept at index 0
summary_table.loc[0] = ['Intercept', reg.intercept_[0]]

# sort the df by index
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
      <td>-1.647455</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_1</td>
      <td>2.800197</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_2</td>
      <td>0.951884</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Reason_3</td>
      <td>3.115553</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reason_4</td>
      <td>0.839001</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Month Value</td>
      <td>0.158930</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Transportation Expense</td>
      <td>0.605284</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Age</td>
      <td>-0.169891</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Body Mass Index</td>
      <td>0.279811</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Education</td>
      <td>-0.210533</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Children</td>
      <td>0.348262</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Pets</td>
      <td>-0.277396</td>
    </tr>
  </tbody>
</table>
</div>



## Interpreting the coefficients


```python
# create a new Series called: 'Odds ratio' which will show the.. odds ratio of each feature
summary_table['Odds_ratio'] = np.exp(summary_table.Coefficient)
```


```python
# display the df
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
      <td>-1.647455</td>
      <td>0.192539</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_1</td>
      <td>2.800197</td>
      <td>16.447892</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_2</td>
      <td>0.951884</td>
      <td>2.590585</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Reason_3</td>
      <td>3.115553</td>
      <td>22.545903</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reason_4</td>
      <td>0.839001</td>
      <td>2.314054</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Month Value</td>
      <td>0.158930</td>
      <td>1.172256</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Transportation Expense</td>
      <td>0.605284</td>
      <td>1.831773</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Age</td>
      <td>-0.169891</td>
      <td>0.843757</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Body Mass Index</td>
      <td>0.279811</td>
      <td>1.322880</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Education</td>
      <td>-0.210533</td>
      <td>0.810152</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Children</td>
      <td>0.348262</td>
      <td>1.416604</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Pets</td>
      <td>-0.277396</td>
      <td>0.757754</td>
    </tr>
  </tbody>
</table>
</div>




```python
# sort the table according to odds ratio
# note that by default, the sort_values method sorts values by 'ascending'
summary_table.sort_values('Odds_ratio', ascending=False)
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
      <td>3.115553</td>
      <td>22.545903</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reason_1</td>
      <td>2.800197</td>
      <td>16.447892</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reason_2</td>
      <td>0.951884</td>
      <td>2.590585</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reason_4</td>
      <td>0.839001</td>
      <td>2.314054</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Transportation Expense</td>
      <td>0.605284</td>
      <td>1.831773</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Children</td>
      <td>0.348262</td>
      <td>1.416604</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Body Mass Index</td>
      <td>0.279811</td>
      <td>1.322880</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Month Value</td>
      <td>0.158930</td>
      <td>1.172256</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Age</td>
      <td>-0.169891</td>
      <td>0.843757</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Education</td>
      <td>-0.210533</td>
      <td>0.810152</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Pets</td>
      <td>-0.277396</td>
      <td>0.757754</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Intercept</td>
      <td>-1.647455</td>
      <td>0.192539</td>
    </tr>
  </tbody>
</table>
</div>



## Testing the model


```python
# assess the test accuracy of the model
reg.score(x_test,y_test)
```




    0.75




```python
# find the predicted probabilities of each class
# the first column shows the probability of a particular observation to be 0, while the second one - to be 1
predicted_proba = reg.predict_proba(x_test)

# let's check that out
predicted_proba
```




    array([[0.71340413, 0.28659587],
           [0.58724228, 0.41275772],
           [0.44020821, 0.55979179],
           [0.78159464, 0.21840536],
           [0.08410854, 0.91589146],
           [0.33487603, 0.66512397],
           [0.29984576, 0.70015424],
           [0.13103971, 0.86896029],
           [0.78625404, 0.21374596],
           [0.74903632, 0.25096368],
           [0.49397598, 0.50602402],
           [0.22484913, 0.77515087],
           [0.07129151, 0.92870849],
           [0.73178133, 0.26821867],
           [0.30934135, 0.69065865],
           [0.5471671 , 0.4528329 ],
           [0.55052275, 0.44947725],
           [0.5392707 , 0.4607293 ],
           [0.40201117, 0.59798883],
           [0.05361575, 0.94638425],
           [0.7003009 , 0.2996991 ],
           [0.78159464, 0.21840536],
           [0.42037128, 0.57962872],
           [0.42037128, 0.57962872],
           [0.24783565, 0.75216435],
           [0.74566259, 0.25433741],
           [0.51017274, 0.48982726],
           [0.85690195, 0.14309805],
           [0.20349733, 0.79650267],
           [0.78159464, 0.21840536],
           [0.63043442, 0.36956558],
           [0.32093965, 0.67906035],
           [0.31497433, 0.68502567],
           [0.47131917, 0.52868083],
           [0.78159464, 0.21840536],
           [0.46493449, 0.53506551],
           [0.77852919, 0.22147081],
           [0.26307895, 0.73692105],
           [0.59501956, 0.40498044],
           [0.39494012, 0.60505988],
           [0.78924152, 0.21075848],
           [0.54775534, 0.45224466],
           [0.76248708, 0.23751292],
           [0.60166502, 0.39833498],
           [0.17244553, 0.82755447],
           [0.43202425, 0.56797575],
           [0.30886675, 0.69113325],
           [0.71340413, 0.28659587],
           [0.78064733, 0.21935267],
           [0.7966903 , 0.2033097 ],
           [0.42371744, 0.57628256],
           [0.6705336 , 0.3294664 ],
           [0.33487603, 0.66512397],
           [0.73050501, 0.26949499],
           [0.16678032, 0.83321968],
           [0.56508475, 0.43491525],
           [0.11625388, 0.88374612],
           [0.76872928, 0.23127072],
           [0.66584142, 0.33415858],
           [0.65567061, 0.34432939],
           [0.30090655, 0.69909345],
           [0.34505737, 0.65494263],
           [0.70755059, 0.29244941],
           [0.20799242, 0.79200758],
           [0.79249724, 0.20750276],
           [0.73159442, 0.26840558],
           [0.91291434, 0.08708566],
           [0.77852919, 0.22147081],
           [0.26754583, 0.73245417],
           [0.69469781, 0.30530219],
           [0.77852919, 0.22147081],
           [0.70985592, 0.29014408],
           [0.09561809, 0.90438191],
           [0.53938703, 0.46061297],
           [0.39825313, 0.60174687],
           [0.78159464, 0.21840536],
           [0.22645293, 0.77354707],
           [0.26837292, 0.73162708],
           [0.25831165, 0.74168835],
           [0.32855571, 0.67144429],
           [0.75417184, 0.24582816],
           [0.92387058, 0.07612942],
           [0.76872928, 0.23127072],
           [0.24741155, 0.75258845],
           [0.56558369, 0.43441631],
           [0.87775043, 0.12224957],
           [0.29572951, 0.70427049],
           [0.42037128, 0.57962872],
           [0.75881641, 0.24118359],
           [0.32093965, 0.67906035],
           [0.82488593, 0.17511407],
           [0.84950183, 0.15049817],
           [0.77374985, 0.22625015],
           [0.73159442, 0.26840558],
           [0.74903632, 0.25096368],
           [0.14944055, 0.85055945],
           [0.70403751, 0.29596249],
           [0.23713867, 0.76286133],
           [0.75645005, 0.24354995],
           [0.78203264, 0.21796736],
           [0.37671823, 0.62328177],
           [0.32093965, 0.67906035],
           [0.30409309, 0.69590691],
           [0.25542876, 0.74457124],
           [0.55063861, 0.44936139],
           [0.51677447, 0.48322553],
           [0.72259112, 0.27740888],
           [0.14944055, 0.85055945],
           [0.2220274 , 0.7779726 ],
           [0.85521156, 0.14478844],
           [0.93001302, 0.06998698],
           [0.09640551, 0.90359449],
           [0.33736668, 0.66263332],
           [0.66154998, 0.33845002],
           [0.47623876, 0.52376124],
           [0.43903262, 0.56096738],
           [0.21378105, 0.78621895],
           [0.17783793, 0.82216207],
           [0.45811539, 0.54188461],
           [0.6906917 , 0.3093083 ],
           [0.74041049, 0.25958951],
           [0.8393858 , 0.1606142 ],
           [0.18777875, 0.81222125],
           [0.54775534, 0.45224466],
           [0.78159464, 0.21840536],
           [0.64457904, 0.35542096],
           [0.78924152, 0.21075848],
           [0.89029009, 0.10970991],
           [0.25937462, 0.74062538],
           [0.7003009 , 0.2996991 ],
           [0.38415023, 0.61584977],
           [0.78924152, 0.21075848],
           [0.70403751, 0.29596249],
           [0.64457904, 0.35542096],
           [0.73490357, 0.26509643],
           [0.47623876, 0.52376124],
           [0.5392707 , 0.4607293 ],
           [0.68498978, 0.31501022],
           [0.75645005, 0.24354995],
           [0.53938703, 0.46061297]])




```python
predicted_proba.shape
```




    (140, 2)




```python
# select ONLY the probabilities referring to 1s
predicted_proba[:,1]
```




    array([0.28659587, 0.41275772, 0.55979179, 0.21840536, 0.91589146,
           0.66512397, 0.70015424, 0.86896029, 0.21374596, 0.25096368,
           0.50602402, 0.77515087, 0.92870849, 0.26821867, 0.69065865,
           0.4528329 , 0.44947725, 0.4607293 , 0.59798883, 0.94638425,
           0.2996991 , 0.21840536, 0.57962872, 0.57962872, 0.75216435,
           0.25433741, 0.48982726, 0.14309805, 0.79650267, 0.21840536,
           0.36956558, 0.67906035, 0.68502567, 0.52868083, 0.21840536,
           0.53506551, 0.22147081, 0.73692105, 0.40498044, 0.60505988,
           0.21075848, 0.45224466, 0.23751292, 0.39833498, 0.82755447,
           0.56797575, 0.69113325, 0.28659587, 0.21935267, 0.2033097 ,
           0.57628256, 0.3294664 , 0.66512397, 0.26949499, 0.83321968,
           0.43491525, 0.88374612, 0.23127072, 0.33415858, 0.34432939,
           0.69909345, 0.65494263, 0.29244941, 0.79200758, 0.20750276,
           0.26840558, 0.08708566, 0.22147081, 0.73245417, 0.30530219,
           0.22147081, 0.29014408, 0.90438191, 0.46061297, 0.60174687,
           0.21840536, 0.77354707, 0.73162708, 0.74168835, 0.67144429,
           0.24582816, 0.07612942, 0.23127072, 0.75258845, 0.43441631,
           0.12224957, 0.70427049, 0.57962872, 0.24118359, 0.67906035,
           0.17511407, 0.15049817, 0.22625015, 0.26840558, 0.25096368,
           0.85055945, 0.29596249, 0.76286133, 0.24354995, 0.21796736,
           0.62328177, 0.67906035, 0.69590691, 0.74457124, 0.44936139,
           0.48322553, 0.27740888, 0.85055945, 0.7779726 , 0.14478844,
           0.06998698, 0.90359449, 0.66263332, 0.33845002, 0.52376124,
           0.56096738, 0.78621895, 0.82216207, 0.54188461, 0.3093083 ,
           0.25958951, 0.1606142 , 0.81222125, 0.45224466, 0.21840536,
           0.35542096, 0.21075848, 0.10970991, 0.74062538, 0.2996991 ,
           0.61584977, 0.21075848, 0.29596249, 0.35542096, 0.26509643,
           0.52376124, 0.4607293 , 0.31501022, 0.24354995, 0.46061297])



## Save the model


```python
# import the relevant module
import pickle
```


```python
# pickle the model file
with open('model', 'wb') as file:
    pickle.dump(reg, file)
```


```python
# pickle the scaler file
with open('scaler','wb') as file:
    pickle.dump(absenteeism_scaler, file)
```


```python

```
