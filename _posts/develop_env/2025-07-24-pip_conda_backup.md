---
title: "pip and conda backup and restore"
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - write here

toc : true
toc_sticky : true
---

# pip list export
pip freeze > requirements.txt

# pip list load
pip install -r requirements.txt

# conda list export
conda list --export > packagelist.txt

# conda list load 
conda install --file packagelist.txt
