---
title: "kaggle에서 data visualization 학습 (using seabon)"
excerpt_separator: "<!--more-->"
categories:
  - dataScience
tags:
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

# 3. Bar Charts and Heatmaps

# 4. Scatter Plots

# 5. Distributions

# 6. Choosing Plot Types and Custom Styles

# 7. Final Project

# 8. Creating Your Own Notebooks

