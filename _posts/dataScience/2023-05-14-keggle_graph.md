---
title: "kaggle에서 data visualization 학습 (using seabon)"
excerpt_separator: "<!--more-->"
categories:
  - dataScience
tags:
  - kaggle
  - seabon

toc : true
toc_sticky : true
---

<img width="1412" alt="image" src="https://github.com/younlea/younlea.github.io/assets/1435846/57f76661-028b-47c2-a70d-35ca04b46c29">

# 1. Hello. Seaborn
```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

# Set up code checking
import os
if not os.path.exists("../input/fifa.csv"):
    os.symlink("../input/data-for-datavis/fifa.csv", "../input/fifa.csv")  
from learntools.core import binder
binder.bind(globals())
from learntools.data_viz_to_coder.ex1 import *
print("Setup Complete")

# Path of the file to read
fifa_filepath = "../input/fifa.csv"

# Read the file into a variable fifa_data
fifa_data = pd.read_csv(fifa_filepath, index_col="Date", parse_dates=True)

# Set the width and height of the figure
plt.figure(figsize=(16,6))

# Line chart showing how FIFA rankings evolved over time
sns.lineplot(data=fifa_data)

```
<img width="703" alt="image" src="https://github.com/younlea/younlea.github.io/assets/1435846/ee034a66-4cd6-4fd8-a80e-e6904f64eb28">

# 2. Line Charts

```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

# Path of the file to read
museum_filepath = "../input/museum_visitors.csv"

# Fill in the line below to read the file into a variable museum_data
museum_data = pd.read_csv(museum_filepath, index_col="Date", parse_dates=True)

# Line chart showing the number of visitors to each museum over time
sns.lineplot(museum_data)
```
<img width="587" alt="image" src="https://github.com/younlea/younlea.github.io/assets/1435846/29629cc5-3b6d-48b7-95e9-5bd60965dde0">


```python
# Line plot showing the number of visitors to Avila Adobe over time
# ____ # Your code here
plt.figure(figsize=(12,6))
sns.lineplot(museum_data["Avila Adobe"])

```
<img width="831" alt="image" src="https://github.com/younlea/younlea.github.io/assets/1435846/f8792b4f-0429-4ce9-ba65-8749b3bf46a0">



# 3. Bar Charts and Heatmaps
bar type 차트와 heatmaps type 차트를 그릴줄 안다. 

```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
# Path of the file to read
ign_filepath = "../input/ign_scores.csv"

# Fill in the line below to read the file into a variable ign_data
ign_data = pd.read_csv(ign_filepath, index_col="Platform")

# Bar chart showing average score for racing games by platform
plt.figure(figsize=(10,6))     
sns.barplot(y= ign_data.index, x = ign_data["Racing"]) 

```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/d43a3797-35c8-497d-b5ae-248af98906dd)

```python
# Heatmap showing average game score by platform and genre
sns.heatmap(data= ign_data, annot= True) # Your code here
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/33969e7d-940a-4821-b893-90cbf497adbb)

# 4. Scatter Plots
example    
```python
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'])    
sns.regplot(x=insurance_data['bmi'], y=insurance_data['charges'])
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'], hue=insurance_data['smoker'])    
sns.lmplot(x="bmi", y="charges", hue="smoker", data=insurance_data)      
sns.swarmplot(x=insurance_data['smoker'], y=insurance_data['charges'])      
```

```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
# Path of the file to read
candy_filepath = "../input/candy.csv"

# Fill in the line below to read the file into a variable candy_data
candy_data = pd.read_csv(candy_filepath,index_col="id")

candy_data.head(5)
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/fe68369a-4abd-407b-9e5b-a78f4e9d9bc8)

```python
# Scatter plot showing the relationship between 'sugarpercent' and 'winpercent'
sns.scatterplot(x=candy_data['sugarpercent'], y=candy_data['winpercent']) 
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/6bb027c7-8b42-4e21-ae95-5071b5f202b4)

```python
# Scatter plot w/ regression line showing the relationship between 'sugarpercent' and 'winpercent'
sns.regplot(x=candy_data['sugarpercent'], y=candy_data['winpercent']) # Your code here
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/04bfc53a-3931-44c0-aaca-5cc75cdbdb83)

```python
# Scatter plot showing the relationship between 'pricepercent', 'winpercent', and 'chocolate'
sns.scatterplot(x=candy_data['sugarpercent'], y=candy_data['winpercent'], hue=candy_data['chocolate']) 
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/39a2142f-7470-41c7-a309-346bd0c12c92)

```python
# Color-coded scatter plot w/ regression lines
sns.lmplot(x="sugarpercent", y="winpercent", hue="chocolate", data=candy_data)    
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/b042ba12-e217-42f0-afd2-85ca54e46d9e)

```python
# Scatter plot showing the relationship between 'chocolate' and 'winpercent'
sns.swarmplot(x=candy_data['chocolate'], y=candy_data['winpercent'])    
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/155bf006-0c75-478f-8670-099eb15d6223)

# 5. Distributions

# 6. Choosing Plot Types and Custom Styles

# 7. Final Project

# 8. Creating Your Own Notebooks

