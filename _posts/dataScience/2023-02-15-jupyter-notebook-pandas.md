---
title: "jupyter notebook - pandas"
excerpt_separator: "<!--more-->"
date: 2023-02-15 14:18:18 +0900
categories:
  - dataScience
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

ex : [Pandas groupby() and count() with Examples - Spark By {Examples} (sparkbyexamples.com)](https://sparkbyexamples.com/pandas/pandas-groupby-count-examples/)

#pandas function

import pandas as pd

import numpy as np

df = pd.read\_csv("./data\_science\_lv2\_sample/iris.csv")

df.cloumns

df.index

df.info()

df.describe()

df['Sepal.Length'].value\_counts()

df\_over\_length = df.loc[(df['Sepal.Length'] > 5) & (df['Sepal.Length'] < 6)]

df\_over\_length

df\_over\_length.reset\_index()

df\_test = df[df['Sepal.Length']>5][['Species', 'new data']]

df\_test

df\_test1 = df.loc[df['Species'] == 'setosa']

df\_test1

정렬, Aggregation, GroupBy 적용

sort\_values()

aggregation

count()

min()

max()

sum()

groupby()

datetime

bike['datetype']=pd.to\_datetime(bike['datetime'])

bike.dtypes

bike.head()

bike['year'] = bike['datetype'].dt.year

bike['month'] = bike['datetype'].dt.month

bike.groupby(['year','month']).sum()

chatgpt 답변

데이터 사이언스를 공부하고 있다면 pandas는 꼭 알아야 하는 파이썬 라이브러리 중 하나입니다. pandas는 데이터 분석에 유용한 다양한 기능들을 제공하며, 테이블 형태의 데이터를 다루는 데 특화되어 있습니다. 이번에는 pandas를 사용하는 방법에 대해서 간단히 알려드리겠습니다.

1. pandas 라이브러리 import 하기

```
pythonCopy code

import pandas as pd
```

2. 데이터 불러오기

```
pythonCopy code

data = pd.read_csv('data.csv')  # csv 파일 불러오기data = pd.read_excel('data.xlsx')  # 엑셀 파일 불러오기data = pd.read_sql('SELECT * FROM table', conn)  # SQL 쿼리 실행 결과 불러오기
```

3. 데이터 탐색

```
pythonCopy code

data.head()  # 상위 5개의 행 출력data.tail()  # 하위 5개의 행 출력data.info()  # 데이터 정보 출력data.describe()  # 데이터 기술통계 출력
```

4. 데이터 선택

```
pythonCopy code

data['column_name']  # 열 선택data[['column_name_1', 'column_name_2']]  # 여러 열 선택data.loc[1:5, 'column_name']  # 행과 열 동시 선택data.iloc[1:5, 2:4]  # 행과 열 위치로 선택
```

5. 데이터 처리

```
pythonCopy code

data.drop('column_name', axis=1, inplace=True)  # 열 삭제data.fillna(0, inplace=True)  # 결측치 0으로 채우기data.replace('old_value', 'new_value', inplace=True)  # 값 치환data.rename(columns={'old_name': 'new_name'}, inplace=True)  # 열 이름 변경
```

6. 데이터 분석

```
pythonCopy code

data['column_name'].mean()  # 평균 계산data['column_name'].std()  # 표준편차 계산data['column_name'].unique()  # 고유값 출력data['column_name'].value_counts()  # 값 빈도 출력data.groupby('group_column').mean()  # 그룹별 평균 계산
```

위의 내용은 pandas를 사용하는 기본적인 방법들 중 일부입니다. pandas는 이 외에도 다양한 기능들을 제공하기 때문에, 추가적으로 공부하면서 활용해보시면 좋을 것입니다.