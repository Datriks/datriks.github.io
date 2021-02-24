---
layout: post
title: "Machine Learning - Logistic Regression"
subtitle: "Forecasting with Machine Learning"
background: '/img/posts/logistic-regression/man-and-tablet.jpg'
---

# Creating a logistic regression to predict absenteeism


```python
## This is a more complex python Machine Learning Algorithm for 
#Logistic Regression
```
## Import the relevant libraries

```python
import pandas as pd
import numpy as np
```

## Load the data


```python
data_preprocessed = pd.read_csv('Absenteeism_preprocessed.csv')
```


```python
data_preprocessed.head()
```

## Create the targets


```python
data_preprocessed['Absenteeism Time in Hours'].median()
```


```python
targets = np.where(data_preprocessed['Absenteeism Time in Hours'] > 
                   data_preprocessed['Absenteeism Time in Hours'].median(), 1, 0)
```


```python
targets
```


```python
data_preprocessed['Excessive Absenteeism'] = targets
```


```python
data_preprocessed.head()
```

## A comment on the targets


```python
targets.sum() / targets.shape[0]
```


```python
data_with_targets = data_preprocessed.drop(['Absenteeism Time in Hours','Day of the Week',
                                            'Daily Work Load Average','Distance to Work'],axis=1)
```


```python
data_with_targets is data_preprocessed
```


```python
data_with_targets.head()
```

## Select the inputs for the regression


```python
data_with_targets.shape
```


```python
data_with_targets.iloc[:,:14]
```


```python
data_with_targets.iloc[:,:-1]
```


```python
unscaled_inputs = data_with_targets.iloc[:,:-1]
```

## Standardize the data


```python
from sklearn.preprocessing import StandardScaler

absenteeism_scaler = StandardScaler()
```


```python
from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.preprocessing import StandardScaler

class CustomScaler(BaseEstimator,TransformerMixin): 
    
    def __init__(self,columns,copy=True,with_mean=True,with_std=True):
        self.scaler = StandardScaler(copy,with_mean,with_std)
        self.columns = columns
        self.mean_ = None
        self.var_ = None

    def fit(self, X, y=None):
        self.scaler.fit(X[self.columns], y)
        self.mean_ = np.mean(X[self.columns])
        self.var_ = np.var(X[self.columns])
        return self

    def transform(self, X, y=None, copy=None):
        init_col_order = X.columns
        X_scaled = pd.DataFrame(self.scaler.transform(X[self.columns]), columns=self.columns)
        X_not_scaled = X.loc[:,~X.columns.isin(self.columns)]
        return pd.concat([X_not_scaled, X_scaled], axis=1)[init_col_order]
```


```python
unscaled_inputs.columns.values
```


```python
#columns_to_scale = ['Month Value','Day of the Week', 'Transportation Expense', 'Distance to Work',
       #'Age', 'Daily Work Load Average', 'Body Mass Index', 'Children', 'Pet']

columns_to_omit = ['Reason_1', 'Reason_2', 'Reason_3', 'Reason_4','Education']
```


```python
columns_to_scale = [x for x in unscaled_inputs.columns.values if x not in columns_to_omit]
```


```python
absenteeism_scaler = CustomScaler(columns_to_scale)
```


```python
absenteeism_scaler.fit(unscaled_inputs)
```


```python
scaled_inputs = absenteeism_scaler.transform(unscaled_inputs)
```


```python
scaled_inputs
```


```python
scaled_inputs.shape
```

## Split the data into train & test and shuffle

### Import the relevant module


```python
from sklearn.model_selection import train_test_split
```

### Split


```python
train_test_split(scaled_inputs, targets)
```


```python
x_train, x_test, y_train, y_test = train_test_split(scaled_inputs, targets, #train_size = 0.8, 
                                                                            test_size = 0.2, random_state = 20)
```


```python
print (x_train.shape, y_train.shape)
```


```python
print (x_test.shape, y_test.shape)
```

## Logistic regression with sklearn


```python
from sklearn.linear_model import LogisticRegression
from sklearn import metrics
```

### Training the model


```python
reg = LogisticRegression()
```


```python
reg.fit(x_train,y_train)
```


```python
reg.score(x_train,y_train)
```

### Manually check the accuracy


```python
model_outputs = reg.predict(x_train)
model_outputs
```


```python
y_train
```


```python
model_outputs == y_train
```


```python
np.sum((model_outputs==y_train))
```


```python
model_outputs.shape[0]
```


```python
np.sum((model_outputs==y_train)) / model_outputs.shape[0]
```

### Finding the intercept and coefficients


```python
reg.intercept_
```


```python
reg.coef_
```


```python
unscaled_inputs.columns.values
```


```python
feature_name = unscaled_inputs.columns.values
```


```python
summary_table = pd.DataFrame (columns=['Feature name'], data = feature_name)

summary_table['Coefficient'] = np.transpose(reg.coef_)

summary_table
```


```python
summary_table.index = summary_table.index + 1
summary_table.loc[0] = ['Intercept', reg.intercept_[0]]
summary_table = summary_table.sort_index()
summary_table
```

## Interpreting the coefficients


```python
summary_table['Odds_ratio'] = np.exp(summary_table.Coefficient)
```


```python
summary_table
```


```python
summary_table.sort_values('Odds_ratio', ascending=False)
```

## Testing the model


```python
reg.score(x_test,y_test)
```


```python
predicted_proba = reg.predict_proba(x_test)
predicted_proba
```


```python
predicted_proba.shape
```


```python
predicted_proba[:,1]
```

## Save the model


```python
import pickle
```


```python
with open('model', 'wb') as file:
    pickle.dump(reg, file)
```


```python
with open('scaler','wb') as file:
    pickle.dump(absenteeism_scaler, file)
```
