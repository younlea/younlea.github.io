---
title: "[S][데싸]python array to dataframe "
excerpt_separator: "<!--more-->"
categories:
  - dataScience
tags:
  - pandas

toc : true
toc_sticky : true
---

# minmax 정규화 이후 array type를 dataframe type으로 바꾸는 방법
## training data set을 만들고 학습데이터를 쓰기 위해서는 dataframe이 있어야 해서임. 

```python

#train, test set만들기
from sklearn.model_selection import train_test_split
df_train, df_test = train_test_split(df, train_size= 0.8, random_state=123)

#정규화
from sklearn.preprocessing import MinMaxScaler
myminmax = MinMaxScaler().fit(df_train)
df_train1 = myminmax.transform(df_train)
df_test1 = myminmax.transform(df_test)

df_train = pd.DataFrame(df_train1, columns=df.columns)
df_test = pd.DataFrame(df_test1, columns=df.columns)

# 거리 3,5,7,9,11 중에 RMSE가 좋은것(가장 낮은값 구하기)
from sklearn.neighbors import KNeighborsRegressor
from sklearn.metrics import mean_squared_error

nei = [3,5,7,9,11]
for n in nei :
    model = KNeighborsRegressor(n_neighbors= n).fit(X = df_train[independent_col], y = df_train[dependent_col])
    pred = model.predict(df_test[independent_col])
    ans = mean_squared_error(y_true = df_test[dependent_col], y_pred = pred)**0.5
    display(n, ans.round(2))

```

# one hot encoding
## 명목 변수를 수치 변수로 바꾸기 위해 One hot encoding을 하는데 아래 함수 사용
```python
import pandas as pd
df = pd.read_csv("xxxxx.csv")
df = pd.get_dummies(df)
```
해당 식을 쓰면 이름으로 되어 있던 값들이 column값으로 바뀌고 1과 0으로 셋팅이 된다. 
