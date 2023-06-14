---
title: "wrtn에서 토큰 걱정없이 python code 만들기 != 안되네 ㅜㅜ "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - wrtn

toc : true
toc_sticky : true
---

chatgpt의 경우 3월 1일부로 무료사용자는 api콜을 못하게 되어서 다른 방법을 찾다 wrtn을 사용해 보았습니다.   
🔴  <h1 style="color: #ff0000">결론 : wrtn에게 물어보면 된다고 하나 실제로는 제공 안함.. 이놈의 할루시네이션 ㅜㅜ</h1>
# wrtn.ai에 가입하고 질문을 해봅니다.
```
prompt :
python을 이용해서 뤼튼에 질문하는 app을 만들려고해. python 예제 코드를 제공해줘

answer: 
뤼튼에게 질문하는 과정은 Wrtn의 API를 호출하여 이루어집니다. 이를 Python 코드로 구현하기 위해서는 Python에서 HTTP 요청을 처리하는 모듈을 사용해야합니다.

아래는 Python에서 requests 모듈을 사용하여 Wrtn API를 호출하는 예제 코드입니다. 이 코드는 "뤼튼"에게 "안녕하세요"라는 인사말을 전달하는 예제입니다. 아래 코드의 YOUR_API_KEY 부분에 본인의 Wrtn API 키를 입력하여 실행하시면 됩니다.
```
```python
import requests
import json

api_url = "https://api.wrtn.ai/v1/gen_text"

payload = {
    'text': '안녕하세요', # 입력 텍스트
    'model': 'lplp', # 사용할 모델 (lplp, t5xl, megatron-11b)
    'api_key': 'YOUR_API_KEY' # Wrtn API 키
}

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(api_url, data=json.dumps(payload), headers=headers)

if response.status_code == 200:
    result = json.loads(response.content)
    generated_text = result['generated'][0]['text']
    print(generated_text)
else:
    print("Error:", response.status_code, response.text)
```
```
위 코드는 Wrtn API를 사용하기 위한 기본적인 코드 구조입니다. 앞으로 API의 다양한 기능을 활용하려면 API 문서를 참고하여 요청 데이터(payload)와 응답 데이터(result)를 처리하는 방법 등을 숙지하시면 됩니다.
```

# 윽 무료가 아니네 ㅜㅜ
```
prompt : wrtn api 발급은 어디서 받아?
answer : 
Wrtn API 발급은 https://wrtn.ai/ 에서 회원가입 후 API 키(Key)를 발급 받을 수 있습니다.

API 키 발급을 위해서는 "Price" 메뉴에서 비즈니스 플랜을 신청해야 합니다. 현재는 유료로 제공하고 있으며, 월별 이용량에 따라 요금이 부과됩니다. 저희 회사 고객 센터를 통해 더 자세한 내용을 문의하실 수 있습니다.

한번 API 키를 받으신 후 API 문서를 참고하여 API 사용법을 익히시면 됩니다. 문서는 다음 링크에서 확인하실 수 있습니다.

https://wrtn.ai/docs/api/
```
# 무료도 사용 가능하다고 나오네.. 
## 테스트는 가능할듯.. 
```
질문:
뤼튼 api를 호출해서 하려는데 무료 회원은 안되?

답변: 
무료 회원도 Wrtn API를 사용할 수 있습니다. 다만, 무료 회원의 경우에는 API 사용량이 제한되어 있습니다.
무료 회원으로 가입하시면, 매달 1,000자까지의 무료 쿼터가 제공됩니다. 이 쿼터를 초과하여 API를 사용하실 경우, 추가 요금이 부과됩니다.
만약 추가 쿼터가 필요하신 경우, 유료 요금제로 업그레이드하시면 됩니다. 유료 요금제에는 무제한 API 요청을 비롯하여 다양한 기능과 서비스가 제공됩니다.

자세한 내용은 Wrtn 웹사이트에서 가격정책을 확인해보시기 바랍니다.
```
## 어디에서 위에 제공한 곳이 안나옴.. 심지어 문의도
## 결론.. (Q&A 들어가서 확인해 보니 공식 답변은 제공 안한다네 ㅜㅜ)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/d773adba-991b-4d6e-be10-364341cd16b8)

## 된다더니 ㅜㅜ
![image](https://github.com/younlea/younlea.github.io/assets/1435846/93c98073-190d-49e6-a0af-76380d50e343)
