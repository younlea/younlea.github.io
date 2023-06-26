---
title: "ChatGPT를 이용해서 웹 크롤링 해보기 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# ChatGPT를 이용해서 웹크롤링 해보기  

## google에서 검색한 후에 이미지들을 다운받는 방법.  
--> google 크롤링이 막혔는지.. API키랑 권한이 있어야 한다고 한다 ㅜㅜ  

## naver에서 검색해서 다운로드 시도
1. naver  검색한 페이지 링크를 chatgpt 에 넣고   
   ![image](https://github.com/younlea/younlea.github.io/assets/1435846/995181e0-2be2-4cf9-9afe-7e9aa019a0d1)   
  

2. 해당 코드(클롤링후 저장해 달라는)를 chatgpt를 이용해서 python으로 생성한 후에     
   ![image](https://github.com/younlea/younlea.github.io/assets/1435846/1091e1f5-6ac3-47a0-a1a2-2c14f0da85c6)
   
``` 
import os
import requests
from bs4 import BeautifulSoup
from urllib.parse import urlparse, parse_qs

# Create a directory to store the images
image_dir = "images"
if not os.path.exists(image_dir):
    os.makedirs(image_dir)

# Define the URL of the Naver search page
url = "https://search.naver.com/search.naver?where=image&sm=tab_jum&query=%EA%B7%B8%EB%A6%BC"

# Send a GET request to the Naver search page
response = requests.get(url)

# Parse the HTML content
soup = BeautifulSoup(response.content, "html.parser")

# Find all image tags on the page
image_tags = soup.find_all("img")

# Loop through each image tag and download the image
for i, tag in enumerate(image_tags):
    # Get the image URL
    image_url = tag["src"]

    # Extract the image file name
    parsed_url = urlparse(image_url)
    query_params = parse_qs(parsed_url.query)
    file_name = query_params["fname"][0]

    # Set the file path to save the image
    file_path = os.path.join(image_dir, file_name)

    # Download the image
    response = requests.get(image_url)
    with open(file_path, "wb") as file:
        file.write(response.content)

    print(f"Downloaded image {i+1}/{len(image_tags)}")

print("All images downloaded successfully!")
```

3. colab에 접속해서 jupyter notebook에서 동작을 시켜서
![image](https://github.com/younlea/younlea.github.io/assets/1435846/7be44947-fc2c-4b71-a50a-3e03c8605b7b)   
에러가 난다. ㅜㅜ   
모.. 다시 해결해 달라고 하고..   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/c159c1de-3643-494b-82d7-fe4532abc672)   

```
import os
import requests
from bs4 import BeautifulSoup
from urllib.parse import urlparse, parse_qs

# Create a directory to store the images
image_dir = "images"
if not os.path.exists(image_dir):
    os.makedirs(image_dir)

# Define the URL of the Naver search page
url = "https://search.naver.com/search.naver?where=image&sm=tab_jum&query=%EA%B7%B8%EB%A6%BC"

# Send a GET request to the Naver search page
response = requests.get(url)

# Parse the HTML content
soup = BeautifulSoup(response.content, "html.parser")

# Find all image tags on the page
image_tags = soup.find_all("img")

# Loop through each image tag and download the image
for i, tag in enumerate(image_tags):
    # Get the image URL
    image_url = tag["src"]

    # Set the file path to save the image
    file_name = f"image_{i+1}.jpg"
    file_path = os.path.join(image_dir, file_name)

    # Download the image
    response = requests.get(image_url)
    with open(file_path, "wb") as file:
        file.write(response.content)

    print(f"Downloaded image {i+1}/{len(image_tags)}")

print("All images downloaded successfully!")

```
     
4. 이미지가 저장된것을 확인할 수 있다.    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/091b7a4f-68b2-4362-97f7-e613259eeb93)  
![image](https://github.com/younlea/younlea.github.io/assets/1435846/e036e33e-1db9-43d4-9d85-87f604d4ab15)  

## 참고 영상 (google 크롤링은 ㅜㅜ)
<iframe width="560" height="315" src="https://www.youtube.com/embed/ckDHMbJ4MHo" frameborder="0" allowfullscreen></iframe>

