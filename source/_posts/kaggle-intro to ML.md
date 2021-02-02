---
title: kaggle
date: 2021-02-02 16:50:29
tags: [Kaggle, ML]
categories: ML
---



# kaggle-intro to ML

## pandas - data

+ read data
  ```python
  import pandas as pd
  data_file_path = '../../../data.csv'
  data = pd.read_csv(data_file_path)
  ```

  <!--more-->
  
+ DataFrame --> sheet in Excel

+ see data
  ```python
  data.shape()  # (row, column)
  data.describe()    # count / mean / std / max / min ...
  data.head()    # top 5 rows
  data.columns   # columns names
  ```

+ select data
  ```python
  data.columns   # columns names
  data = data.dropna(axis=0) # drop not available (missing) data
  
  y = data.price # select prediction target
  
  data_features = ['rooms', 'size', 'direction']  
  X = data[data_features]
  X.describe()  # check
  X.head()
  ```

  + select prediction target
    + select column --> dot notation
    + single column is stored in `Series`

  + select features
    + a list of column names --> string with quotes

## Sklearn (scikit-learn) - Model

+ modeling the types of data typically stored in DataFrames

+ steps

  + define (model + parameters)
  + fit
  + predict
  + evaluate

+ 
  ```python
  from sklearn.tree import DecisionTreeRegressor
  from sklearn.model_selection import train_test_split
  from sklearn.metrics import mean_absolute_error
  
  train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0) # split test to train and validation
  house_model = DecisionTreeRegressor () # define
  house_model.fit(train_X, train_y) # fit
  val_prediciton = house_model.predict(val_X) # predict
  error = mean_absolute_error(val_y, val_prediciton) #evaluate
  print(error)
  ```

  + `random_state` ensures you get the same results in each run, can use any number

+ overfitting vs. underfitting by `max_leaf_nodes`

  ```python
  import pandas as pd
  from sklearn.metrics import mean_absolute_error
  from sklearn.tree import DecisionTreeRegressor
  from sklearn.model_selection import train_test_split
  
  
  # Load data
  melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
  melbourne_data = pd.read_csv(melbourne_file_path) 
  # Filter rows with missing values
  filtered_melbourne_data = melbourne_data.dropna(axis=0)
  # Choose target and features
  y = filtered_melbourne_data.Price
  melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea']
  X = filtered_melbourne_data[melbourne_features]
  
  
  def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
      model = DecisionTreeRegressor(max_leaf_nodes=max_leaf_nodes, random_state=0)
      model.fit(train_X, train_y)
      preds_val = model.predict(val_X)
      mae = mean_absolute_error(val_y, preds_val)
      return(mae)
  
  # split data into training and validation data, for both features and target
  train_X, val_X, train_y, val_y = train_test_split(X, y,random_state = 0)
  
  # compare MAE with differing values of max_leaf_nodes
  for max_leaf_nodes in [5, 50, 500, 5000]:
      scores = {leaf_size: get_mae(leaf_size, train_X, val_X, train_y, val_y) for leaf_size in candidate_max_leaf_nodes}
      
  best_tree_size = min(scores, key=scores.get)
  
  final_model = DecisionTreeRegressor(max_leaf_nodes = best_tree_size, random_state = 0)
  final_model.fit(X, y)
  
  test_data_path = '../input/test.csv'
  test_data = read_csv(test_data_path)
  test_X = test_data[features]
  test_preds = rf_model.predict(test_X)
  
  # save predictions in format used for competition scoring
  output = pd.DataFrame({'Id': test_data.Id,
                         'SalePrice': test_preds})
  output.to_csv('submission.csv', index=False)
  ```

  + 
    ```python
    get(self, key, default=None, /)
        Return the value for key if key is in the dictionary, else default
    ```

+ random forest --> many random trees and take average

  ```python
  from sklearn.ensemble import RandomForestRegressor
  from sklearn.metrics import mean_absolute_error
  
  forest_model = RandomForestRegressor(random_state=1)
  forest_model.fit(train_X, train_y)
  melb_preds = forest_model.predict(val_X)
  print(mean_absolute_error(val_y, melb_preds))
  ```

  