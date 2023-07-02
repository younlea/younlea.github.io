---
title: "chatgpt구조를 plantuml을 사용해서 그려보자 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - plantuml
  - chatgpt

toc : true
toc_sticky : true
---

# UML 그려주는 VScode extension - PlantUML
[plantuml](https://plantuml.com/ko/)

## VScode Extension  
검색에서 plantuml을 검색하면 설치가 됩니다. 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/f4d7233f-1e68-444c-84b1-def29007d835)

## plantuml 사용법   
일단 확장자를 xxx.plantuml 로 저장하면 됩니다. 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/5068fb84-abdd-42e3-b88d-64fba0ff9785)

오른쪽 위에 돋보기 모양을 누르면 아래처럼 간단히 그려 집니다. 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/ad72e08f-868e-49d1-b3aa-1f473cafb8e8)

## 문법   
대부분 해당 싸이트에 나와 있는데...   
chatgpt를 이용해서 만들어 달라고 하면 쉽게 만들어 줍니다. ^^;    

![image](https://github.com/younlea/younlea.github.io/assets/1435846/965dcc63-8320-4dcd-97dc-ab93801c0ae4)

```
@startuml

package "User Interface" {
  [User Input] --> [ChatGPT]
  [ChatGPT] --> [Response] 
}

package "ChatGPT Backend" {
  [Response Generator] --> [Transformer Model]
  [Context Encoder] --> [Context Model]
  [Knowledge Retriever] --> [Knowledge Base]
}

package "Knowledge Base" {
  [Text Documents]
}

[Knowledge Base] --> [Knowledge Retriever]
[ChatGPT] --> [Response Generator]
[ChatGPT] --> [Context Encoder]
[Transformer Model] --> [Context Encoder]
[Context Model] --> [Response Generator]

@enduml

```
왼쪽은 VScode에서 생성, 오른쪽은 홈페이지에서 작성.. 왼지 홈페이지 작성이 제대로 나온듯 ^^;
![image](https://github.com/younlea/younlea.github.io/assets/1435846/54c155e3-ab51-4346-bc8a-682b2d337e24)

```
이 다이어그램에는 사용자 인터페이스, ChatGPT 백엔드 및 기술 자료의 세 가지 주요 구성 요소가 있습니다.
사용자 인터페이스는 사용자 입력을 캡처하고 생성된 응답을 표시하는 역할을 합니다. "사용자 입력" 및 "응답" 구성 요소로 구성됩니다.
ChatGPT 백엔드는 시스템의 핵심입니다. 여기에는 입력을 기반으로 응답을 생성하는 "응답 생성기"와 대화 컨텍스트를 인코딩하는 "컨텍스트 인코더"가 포함됩니다. 두 구성 요소 모두 ChatGPT에서 사용하는 기본 기계 학습 모델인 "변환기 모델"과 상호 작용합니다.
지식 베이스는 지식 검색에 사용할 수 있는 관련 텍스트 문서를 저장합니다. "지식 검색기" 구성 요소는 대화 컨텍스트를 기반으로 지식 기반에서 정보를 검색합니다.
전반적으로 이 다이어그램은 ChatGPT의 시스템 아키텍처에 대한 높은 수준의 개요를 제공하여 다양한 구성 요소 간의 상호 작용을 보여줍니다.
```
