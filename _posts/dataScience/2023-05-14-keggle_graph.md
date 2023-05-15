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
## histograms and density plots.
```python
# Histogram 
sns.histplot(iris_data['Petal Length (cm)'])
# KDE plot (kernel density estimate (KDE))
sns.kdeplot(data=iris_data['Petal Length (cm)'], shade=True)
# 2D KDE plot
sns.jointplot(x=iris_data['Petal Length (cm)'], y=iris_data['Sepal Width (cm)'], kind="kde")

#color-coded (대충 다 넣으면 알아서 처리해 주는듯.)
# Histograms for each species
sns.histplot(data=iris_data, x='Petal Length (cm)', hue='Species')

# KDE plots for each species
sns.kdeplot(data=iris_data, x='Petal Length (cm)', hue='Species', shade=True)
```

```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
# Path of the files to read
cancer_filepath = "../input/cancer.csv"

# Fill in the line below to read the file into a variable cancer_data
cancer_data = pd.read_csv(cancer_filepath, index_col="Id")
# Print the first five rows of the data
cancer_data.head(5)
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/5a427d16-51e4-487f-948e-4a25c94b70c9)

```python
# Histograms for benign and maligant tumors
sns.histplot(data=cancer_data, x='Area (mean)', hue='Diagnosis')
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/76fdd2e6-c989-4f2f-95cd-70f89dd89563)

```python
# KDE plots for benign and malignant tumors
sns.kdeplot(data=cancer_data, x='Radius (worst)', hue='Diagnosis', shade=True)
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/efa40ea9-481c-4711-8ae0-994823831568)

# 6. Choosing Plot Types and Custom Styles
![image](https://github.com/younlea/younlea.github.io/assets/1435846/da4d25bd-a74a-465c-965f-d1422eaf7be7)

> Trends - A trend is defined as a pattern of change.   
> sns.lineplot - Line charts are best to show trends over a period of time, and multiple lines can be used to show trends in more than one group.   
> Relationship - There are many different chart types that you can use to understand relationships between variables in your data.   
> sns.barplot - Bar charts are useful for comparing quantities corresponding to different groups.   
> sns.heatmap - Heatmaps can be used to find color-coded patterns in tables of numbers.   
> sns.scatterplot - Scatter plots show the relationship between two continuous variables; if color-coded, we can also show the relationship with a third categorical variable.    
> sns.regplot - Including a regression line in the scatter plot makes it easier to see any linear relationship between two variables.   
> sns.lmplot - This command is useful for drawing multiple regression lines, if the scatter plot contains multiple, color-coded groups.   
> sns.swarmplot - Categorical scatter plots show the relationship between a continuous variable and a categorical variable.   
> Distribution - We visualize distributions to show the possible values that we can expect to see in a variable, along with how likely they are.   
> sns.histplot - Histograms show the distribution of a single numerical variable.   
> sns.kdeplot - KDE plots (or 2D KDE plots) show an estimated, smooth distribution of a single numerical variable (or two numerical variables).   
> sns.jointplot - This command is useful for simultaneously displaying a 2D KDE plot with the corresponding KDE plots for each individual variable.   

```python
# Path of the file to read
spotify_filepath = "../input/spotify.csv"

# Read the file into a variable spotify_data
spotify_data = pd.read_csv(spotify_filepath, index_col="Date", parse_dates=True)

# Change the style of the figure
sns.set_style("dark")

# Line chart 
plt.figure(figsize=(12,6))
sns.lineplot(data=spotify_data)
```
>> try other type
>> "darkgrid"   
>> "whitegrid"    
>> "dark"   
>> "white"   
>> "ticks"   

![image](https://github.com/younlea/younlea.github.io/assets/1435846/c3242da7-85b0-48b9-80b5-6eac43b17bcb)

# 7. Final Project
> https://www.kaggle.com/datasets  참고    
>>  해당 dataset에서 검색후 다운받아서 확인한다.    

# 8. Creating Your Own Notebooks
> https://www.kaggle.com/code   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/ddf52df2-eb31-4d9c-aed8-8d1e91983edd)
```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
print("Setup Complete")
```

