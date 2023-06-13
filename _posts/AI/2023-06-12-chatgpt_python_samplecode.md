---
title: "chatgpt python 예제 코드 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - chatgpt

toc : true
toc_sticky : true
---

# chatgpt python 예제 코드

> prompt :
> python으로 chatgpt를 이용해서 질문하는 함수를 만들어줄래?

결과물이 아래와 같이 나온다.  

필요 사항..   
필자의 경우 jupyter notebook을 이용..  
!pip install openai 실행  
openai에서 개인 키를 가져옴 : [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)    

```python
import openai

openai.api_key = 'YOUR_API_KEY'

def ask_gpt(question):
    prompt = f"Question: {question}\nAnswer:"

    response = openai.Completion.create(
        engine='text-davinci-003',
        prompt=prompt,
        max_tokens=100,
        temperature=0.7,
        n=1,
        stop=None,
        echo=True
    )

    answer = response.choices[0].text.strip().split(':')[1].strip()
    return answer

# Example usage
question = input("Ask a question: ")
answer = ask_gpt(question)
print(answer)
```
```
이 예에서는 OpenAI Python 라이브러리를 사용하여 API 요청을 수행합니다. 'YOUR_API_KEY'를 실제 OpenAI API 키로 바꿔야 합니다.
ask_gpt 함수는 ChatGPT에 묻고자 하는 질문을 나타내는 question 매개변수를 사용합니다. 질문 형식을 프롬프트로 지정하고 API에 완료 요청을 보냅니다. 
응답을 처리하여 생성된 답변을 추출한 다음 반환합니다.
원하는 질문으로 ask_gpt 함수를 호출하고 반환된 답변을 인쇄하여 테스트할 수 있습니다.
이 코드를 실행하기 전에 pip install openai를 실행하여 OpenAI Python 라이브러리를 설치해야 합니다.
```


# error 발생
RateLimitError: You exceeded your current quota, please check your plan and billing details.

```
prompt : 
RateLimitError: You exceeded your current quota, please check your plan and billing details.  가 나는데 왜 이래? 난 아직 여유 무료 token이 있는 걸로 아는데
답변 :
I apologize for the confusion. If you're receiving a RateLimitError stating that you exceeded your quota, there could be a few possible reasons for this issue:
Quota limitations: Even if you have free tokens available, there could be a limit on the number of requests you can make within a certain time period. Double-check the specific limits for your plan or the usage limits for free trial users.
Billing issues: Ensure that your billing details are correct and up to date. If there are any issues with your payment method or if you have reached the usage limits associated with your plan, it could result in the RateLimitError.
API key issues: Make sure you're using the correct API key associated with your OpenAI account. If you're using an incorrect or expired API key, it can lead to rate limit errors.
If you believe none of these reasons apply to your situation, I recommend reaching out to the OpenAI support team directly. They will be able to assist you further in identifying the cause of the issue and help resolve it.

번역결과

혼란을 드려 죄송합니다. 할당량을 초과했다는 RateLimitError를 수신하는 경우 이 문제에 대한 몇 가지 이유가 있을 수 있습니다.

할당량 제한: 사용 가능한 무료 토큰이 있더라도 특정 기간 내에 할 수 있는 요청 수에 제한이 있을 수 있습니다. 플랜의 특정 한도 또는 무료 평가판 사용자의 사용 한도를 다시 확인하십시오.
청구 문제: 청구 세부 정보가 정확하고 최신인지 확인하십시오. 결제 방법에 문제가 있거나 요금제와 관련된 사용 한도에 도달한 경우 RateLimitError가 발생할 수 있습니다.
API 키 문제: OpenAI 계정과 연결된 올바른 API 키를 사용하고 있는지 확인하세요. 올바르지 않거나 만료된 API 키를 사용하는 경우 속도 제한 오류가 발생할 수 있습니다.

이러한 이유가 귀하의 상황에 해당하지 않는다고 생각되면 OpenAI 지원 팀에 직접 문의하는 것이 좋습니다. 그들은 문제의 원인을 식별하고 해결하는 데 도움을 줄 수 있습니다.
```
# 무료 토큰관련 문의
```
prompt:
무료 토큰 사용자는  api를 사용하지 못하는거야?
answer:
As of March 1st, 2023, with the launch of the ChatGPT API, free trial users can no longer access the API for free. API access comes with its own separate cost and is not included in the free trial. If you're using a free trial account, you won't be able to make API requests without upgrading to a paid plan.
For more information about the pricing and details regarding API access, I recommend visiting the OpenAI Pricing page (https://openai.com/pricing) or contacting the OpenAI sales team for further assistance.

번역결과

2023년 3월 1일부터 ChatGPT API가 출시되면서 무료 평가판 사용자는 더 이상 무료로 API에 액세스할 수 없습니다. API 액세스에는 별도의 비용이 부과되며 무료 평가판에는 포함되지 않습니다. 무료 평가판 계정을 사용 중인 경우 유료 플랜으로 업그레이드하지 않으면 API 요청을 할 수 없습니다.
API 액세스에 대한 가격 및 세부 정보에 대한 자세한 내용은 OpenAI 가격 책정 페이지(https://openai.com/pricing)를 방문하거나 OpenAI 영업팀에 문의하여 추가 지원을 받는 것이 좋습니다.
```
## 결국 3월부터 정책이 바꼈군. 결재 해야것다 ㅜㅜ

## 아 결재를 따로 안해도 카드만 등록하면 된다. 
아래 링크 들어가서 Payment Methods 창에서 입력하면 됨.
[카드등록](https://platform.openai.com/account/billing/overview)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/56822fe1-e068-4ab0-ba45-8aaeb5270a68)

이후 사용량을 보면 Usage this month가 생긴걸 볼수 있다.     
움 가격이 120불 리미트가 걸려 있는데 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/10aee741-6aa9-41c9-a913-3a33d6c767ba)

아래 그림처럼 사용량 제한을 걸어 둘수 있다.
![image](https://github.com/younlea/younlea.github.io/assets/1435846/895a427b-6938-434c-b3c2-98eb3574bc3d)
기존 120$ 였는데 2$로 수정
![image](https://github.com/younlea/younlea.github.io/assets/1435846/b0bbb656-df9d-40f3-8916-a8fd56f12377)
실제로 바뀐걸 볼수 있다. 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/9f595fd9-6488-43b8-bd87-253f1d9c7216)

질문 하나 날렸는데.. 아래와 같이 사용량이 나오는걸 볼수 있다. ^^
![image](https://github.com/younlea/younlea.github.io/assets/1435846/4f18a4e9-fc95-4d3f-8547-54a6a6e10dbe)

써보고 나중에 월정액으로 바꿔 봐야것다.

참고로 vscode에 추가된 AICodeHelper 같은 경우 ChatGPT API를 쓰기 때문에 결재 하지 않으면 위에 보였던 에러가 나오니 참고 하시길 ^^;
