---
title: "머신러닝 함수들 "
excerpt_separator: "<!--more-->"
categories:
  - dataScience
tags:
  - ml
  - ML function

toc : true
toc_sticky : true
---

## 데이터 정렬 및 변환
> pandas - crosstab() - 두 변수의 원소 조합 빈도 확인(normalize 인자 설정시 비율을 손쉽게 계산   
> pandas - sort_valus() - 특정 변수를 기준으로 정렬   
> pandas - melt() - wide form -> long form   
> pandas - pivot() - long form -> wide form   

## train and test sample 생성
```python
from sklearn.model_selection import train_test_split
df_train, df_test = train_test_split(df, test_size=0.8, random_state=123)
```

## 상관 관계 -선형성 검사
```python
#pandas - corr(), method - peason, kendal, spearman

from scipy.stats import pearsonr
from scipy.stats import spearmanr
from scipy.stats import kendalltau
```


## 비계층 군집 분석 - k-mean (random_state, seed 고정 필수)
[Scaler참고](https://for-my-wealthy-life.tistory.com/18)   
```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import MinMaxScaler
from sklearn.preprocessing import StandardScaler

scaler = StandardScler().fit(df)
scaler.transform(df)

```
k-mean은 연속형 데이터에서 사용한다.    

``` python
#군집분석 이전에 정규화
nor_minmax = MinMaxScaler().fit(bmi_data)
bmi_data_nor = nor_minmax.transform(bmi_data)
bmi_data_nor = pd.DataFrame(bmi_data_nor, columns= bmi_data.columns)
bmi_data_nor.head(2)

#model.labels_
model = KMeans(n_clusters=4, random_state= 123).fit(bmi_data_nor)   #모델을 만들때는 정규화 실시한 내용을 바탕으로 만들고.. 
bmi_data["cluster"] = model.labels_
bmi_data.head(3) 
bmi_data["cluster"].value_counts()
bmi_data.groupby("cluster")["Age"].mean()

#model.cluster_centers_
model= KMeans(n_clusters=3, random_state= 123).fit(bmi_data)
df_centers = pd.DataFrame(model.cluster_centers_, columns=bmi_data.columns)
df_centers

```

## 단순 회귀 분석 (선형) [선형방정식이 모델이고, 독립변수 넣으면 추정값이 나온다.]
```python
from statsmodels.formula.api import ols
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error
from sklearn.metrics import mean_squared_error

#ols 
import pandas as pd
from statsmodels.formula.api import ols
df = pd.read_csv(xxxx)

model = ols(formula = "independent ~ dependent_val", data = df_train).fit()
pred = model.predict(df_test)
pred[:4]

mean_squared_error(y_pred = pred, y_true = df_test["independent"])**0.5  #RMSE

#LinearRegression
from sklearn.linear_model import LinearRegression
model = LinearRegression().fit(X = df[["PL"]], y=df["PW"])
model.coef_         # 기울기
mddel.intercept_    # 절편
model.predict(df[["PL"]])

#결정계수 (R^2, Coefficient of determination)
model.score(X, y)
```
### 결정계수 수식으로 구하기    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/70f8808a-2aa4-41be-a6ed-53f88c253179)     
![image](https://github.com/younlea/younlea.github.io/assets/1435846/2ea58a44-05f9-4870-93e7-f2cd27904330)     
```python
y_pred = model.predict(X_test)
r2 = 1 - ((y_test - y_pred)**2).sum() / ((y_test - y_test.mean())**2).sum()
print(r2)
```

### 조정된 결정계수 구하기
독립변수의 개수가 증가하면 일방적으로 증가하는 결정계수와 달리 조정된 결정계수는 독립변수가 증가할때 분자를 감소시켜주는 연산을 통해 일방적인 증가를 방지합니다.    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/ca292971-9b9f-4e51-a116-11dbc0304443)


## 다중 회귀 분석 - 다중 공선성, 선형 [선형방정식이 모델이고, 독립변수 넣으면 추정값이 나온다.]
```python
from patsy import dmatrices
from statsmodels.stats.outliers_influence import variance_inflation_factor as vif

formula = "casual ~ " + ".join(df_sub.columns[:-1])
y, X = dmatrices(formula, data = df_sub, return_type = "dataframe")
df_vif = pd.DataFrame()
df_vif["colname"] = X.columns
df_vif["VIF"] = [vif(X.values, i) for i in range(X.shape[1])]
df_vif

from statsmodels.formula.api import ols  #요거써서 여러값이 들어왔을때 수치 예측한다. 
```

## 로지스틱 회귀분석 - 승산비(OR, odds Ratio)
```python
from statsmodels.api import Logit
from sklearn.linear_model import LogisticRegression

from sklearn.metrics import accuracy_score
from sklearn.metrics import precision_score
from sklearn.metrics import recall_score
from sklearn.metrics import f1_score

#모델 만들기
model = Logit(endog = df_train["outcome"], exog = df_train.loc[:,["col1","col2","col3"]]).fit()

#모델 이용해서 예측하기
pred = model.predict(exog = df_test.loc[:,["col1","col2","col3"]])

#성능 검토
accuracy_score(y_pred = pred, y_true = df_test["output"])
```

## 나이브 베이즈
```python
from sklearn.naive_bayes import GaussianNB
#모델 만들기
nb_model = GaussianNB().fit(X = df_train.loc[:, ["col1","col2","col3"]], y = df_train["output"])

#모델 이용해서 예측하기 (확률이라 predict_proba를 사용함)
pred = nb_model.predict_proba(df_test.loc[:,["col1","col2","col3"]])
pred[:4,]

#성능 검토
from sklearn.metrics import accuracy_score
accuracy_score(y_pred = (pred[:,1] > 0.5) + 0, y_true = df_test["output"])

```

## KNN
```python
from sklearn.neighbors import KNeighborsClassifier #KNN 분류 모델
from sklearn.neighbors import KNeighborsRegressor  #KNN 회귀 모델

#model만들고
model = KNeighborsClassifier().fit(X = df_train.loc[:, ["col1","col2","col3"]], y = df_train['output'])

#model을 이용 예측하고
pred = model.predict(df_test.loc[:,["col1","col2","col3"]])

#model 성능을 검토한다. 
from sklearn.metrics import accuracy_score
accuracy_score(y_pred = pred, y_true = df_test["output"])

```

## 의사결정트리
```python
from sklearn.tree import DecisionTreeClassifier #분류 나무
from sklearn.tree import DecisionTreeRegressor #회귀 나무


```

[sklearn 설명](https://www.datamanim.com/dataset/98_sklearn/sklearn.html#)
  
```
sklearn 함수가 기억 안나면 아래처럼 원하는 depth에서 하나씩 확인하고 찾아 들어갈수 있다.
import sklearn
sklearn.__all__
```
```

sklearn
│
├── 01 preprocessing (전처리)
│   │
│   ├── 스케일러
│   │   ├── MinMaxScaler
│   │   ├── RobustScaler
│   │   └── StandardScaler
│   │
│   └── 인코더
│       ├── LabelEncoder
│       └── OneHotEncoder
│  
├── 02 model_selection (모델링 전처리)
│   │
│   ├── 데이터셋 분리
│   │   ├── KFold
│   │   ├── StratifiedKFold
│   │   └── train_test_split
│   │
│   └── 하이퍼파라미터 튜닝
│       └── GridSearchCV
│
├── 03 모델학습
│   │
│   ├── ensemble
│   │   ├── AdaBoostClassifier
│   │   ├── GradientBoostingClassifier
│   │   ├── RandomForestClassifier
│   │   └── RandomForestRegressor
│   │
│   ├── linear_model
│   │   ├── LogisticRegression
│   │   └── RidgeClassifier
│   │
│   ├── neighbors
│   │   └── KNeighborsClassifier
│   │
│   ├── svm
│   │   ├── SVC
│   │   └── SVR
│   │
│   └── tree
│       ├── DecisionTreeClassifier
│       ├── DecisionTreeRegressor
│       ├── ExtraTreeClassifier
│       └── ExtraTreeRegressor
│
├── 04 모델평가
│   │
│   ├── metrics
│   │   ├── accuracy_score
│   │   ├── classification_report
│   │   ├── confusion_matrix
│   │   ├── f1_score
│   │   ├── log_loss
│   │   ├── mean_absolute_error
│   │   ├── mean_squared_error
│   │   └── roc_auc_score
│   │
│   └── model (정의된 모델에서 추출)
│       ├── predict
│       └── predict_proba
│
└── 05 최종앙상블
    │
    └── ensemble
        ├── StackingClassifier
        ├── StackingRegressor
        ├── VotingClassifier
        └── VotingRegressor
```

[scipy설명](https://www.datamanim.com/dataset/97_scipy/scipy.html)    


```
scipy
│
├── 01 integrate 수치적분, 미분방정식
│  
├── 02 linalg (선형대수, 매트릭스 분해)
│ 
├── 03 optimize (방정식 해 구하는 알고리즘, 함수 최적화)
│ 
├── 04 signal (신호 관련)
│
├── 05 sparse (희소 행렬, 희소 선형 시스템)
│
└── 06 stats (통계 분석) 
```

[statsmodels설명](https://www.datamanim.com/dataset/96_statsmodels/statsmodels.html)   
```
statsmodels 함수가 기억 안나면 아래처럼 원하는 depth에서 하나씩 확인하고 찾아 들어갈수 있다.
import statsmodels
help(statsmodels)
```  
```
statsmodels
│
├── 01 사후분석
│   │
│   └──stats
│       └── multicomp
│           ├── MultiComparison
│           │   └── allpairtest
│           └── pairwise_tukeyhsd
│
├── 02 시계열분석
│   │
│   ├── graphics.tsaplots
│   │   ├── plot_acf
│   │   └── plot_pacf
│   └── tsa
│       ├── arima_model
│       │   └── ARIMA
│       └── statesplace.sarimax
│           └── SARIMAX
│
├── 03 ANOVA (scipy모듈과 함께써야 모두 커버가능, 이분산 anova의 경우 pingouin모듈의 welch_anova를 사용)
│   │
│   ├── 다원분산분석 or 이원분산분석
│   └── 일원분산분석
│       └── stats.anova
│           └── anova_lm
│
└── 04 회귀분석
    │
    └── formula.api
        └── ols  
```
