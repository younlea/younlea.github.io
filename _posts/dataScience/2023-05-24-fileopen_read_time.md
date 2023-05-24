---
title: "pandas csv file 읽을때 저장된 시간 알아내기 "
excerpt_separator: "<!--more-->"
categories:
  - dataScience
tags:
  - pandas

toc : true
toc_sticky : true
---

# pandas csv file 저장 시간 확인
pandas에서 csv 파일을 읽어올 때 해당 파일의 생성 날짜를 알고 싶으시다면, 다음과 같이 하시면 됩니다.

```python
import os.path, time

file_path = '파일 경로'
print("파일 생성 시간 : ", time.ctime(os.path.getctime(file_path)))
```
위 코드에서 '파일 경로’에는 읽어올 csv 파일의 경로를 입력하시면 됩니다. 이 코드를 실행하면 해당 파일의 생성 시간이 출력됩니다.
