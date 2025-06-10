---
title: "LangChain강의 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# LangChain이란?
<img width="802" alt="image" src="https://github.com/user-attachments/assets/fef829ce-fe97-4551-9871-97a02ad1bce1" />   
<img width="783" alt="image" src="https://github.com/user-attachments/assets/225a7a03-66f5-4b20-b47b-0b83d94e8c78" />   
<img width="828" alt="image" src="https://github.com/user-attachments/assets/86b2b2c3-89f0-4854-8c32-ae7cef15ab6d" />
[https://www.langchain.com/](https://www.langchain.com/)   
<img width="801" alt="image" src="https://github.com/user-attachments/assets/cb7e5ac0-9ae8-4e2f-aeb2-010b29f57f1b" />
[RAG](https://python.langchain.com/docs/tutorials/rag/)    
<img width="801" alt="image" src="https://github.com/user-attachments/assets/1191bb43-48eb-444a-92b3-c1d9ad96ada9" />
<img width="801" alt="image" src="https://github.com/user-attachments/assets/ac1ada30-db51-476b-a123-7caeff6125a0" />

## LangChain Quick start
[colab 예제](https://colab.research.google.com/drive/16b1DHbkYP9xEJt03r_V98_dkbJd0Sm1R)    

## RAG 
<img width="801" alt="image" src="https://github.com/user-attachments/assets/da78b06d-910f-4e31-bdb4-5c30437df6b0" />

## RAG로 chatgpt만들기 (실습)
[colab 예제](https://colab.research.google.com/drive/1z3tTM3V7_vnb3Mrugm9XShazI_nNJTy4)   

### RAG 생성   
<img width="1390" alt="image" src="https://github.com/user-attachments/assets/79252e0e-338c-4677-91c9-f0cb5749b1c9" />   

### RAG 사용   
<img width="1390" alt="image" src="https://github.com/user-attachments/assets/20632e2d-1909-48a0-84da-6ad095e71f13" />   

# RAG 단계별 설명 및 예제
## Document Loader
[colab 예제](https://colab.research.google.com/drive/1sJnmJKbDXyN3-QkkbPp8s1dWZUXSpBsZ)    
[Documnet Loaders](https://python.langchain.com/docs/integrations/document_loaders/)    

<img width="796" alt="image" src="https://github.com/user-attachments/assets/df9cf5b8-08a9-473d-9a81-88e77e7e020d" />   
<img width="796" alt="image" src="https://github.com/user-attachments/assets/6f89ff06-f96f-4238-95e8-3dba996e2b4b" />

### WebBase Loaders   
<img width="796" alt="image" src="https://github.com/user-attachments/assets/62fd9af1-2c6a-45d5-a26c-b9e8d917e1d6" />
[webbase loaders](https://python.langchain.com/docs/integrations/document_loaders/web_base/#loader-features)     

### CSVLoader   
<img width="796" alt="image" src="https://github.com/user-attachments/assets/1ca75138-8a21-4316-9176-31ff244ca6b4" />

### DirctoryLoader
<img width="796" alt="image" src="https://github.com/user-attachments/assets/88be24ae-61b8-43c1-aaa8-e67aec9c3786" />

### HTML Loader
<img width="796" alt="image" src="https://github.com/user-attachments/assets/9f21688d-fc0b-440e-8505-37575c160628" />

### JSON loader
<img width="796" alt="image" src="https://github.com/user-attachments/assets/eaf14d3b-cb82-4418-980a-a02c54c90ebc" />

### MarkdownLoader
<img width="796" alt="image" src="https://github.com/user-attachments/assets/f132f618-0513-40b3-8108-2ec530173c6f" />

### PDFLoader
<img width="796" alt="image" src="https://github.com/user-attachments/assets/f9cc4dc4-c654-4c7a-8162-84750613480e" />

## Document Transformers
Document loader로 불려진 내용을 임베딩 하기 전에 작게 쪼개는 역할   
> from langchain.text_splitter import RecursiveCharacterTextSplitter

[colab 예제](https://colab.research.google.com/drive/1WmZDQA50xX-MKz59XMZO6nO83Vsf0slb)    
[Documnet Transformers](https://python.langchain.com/docs/integrations/document_transformers/)

## Vector Embedding and Vector Store
### 임베딩 개념과 장점
<img width="811" alt="image" src="https://github.com/user-attachments/assets/fd033bc4-ec94-47ef-9e24-9ccd566220b5" />    
<img width="811" alt="image" src="https://github.com/user-attachments/assets/728b44ab-b53c-4ebe-8253-5d933f417260" />    
<img width="811" alt="image" src="https://github.com/user-attachments/assets/2a21f07b-c149-4b70-ae14-756c1fcce5b9" />    
<img width="811" alt="image" src="https://github.com/user-attachments/assets/d891ca75-9a98-4816-8fcf-d16c154fb77c" />    
<img width="811" alt="image" src="https://github.com/user-attachments/assets/e6762a60-c861-4425-8f50-9b85daae9dd9" />    
<img width="811" alt="image" src="https://github.com/user-attachments/assets/d6aa3239-6fb2-4aba-8b70-bc1e047cb721" />    

## 임베딩 테스트 (임베딩 벡터 사칙연산)   
[Korea Word2Vector](https://word2vec.kr/search/)     
<img width="811" alt="image" src="https://github.com/user-attachments/assets/08850f08-729a-42f6-9ef0-6c63aa60d637" />

### 임베딩과 백터 스토어
[Colab 예제](https://colab.research.google.com/drive/1sNFXTg6TrIm8szg5VtwIrfSs6FdklBxu)    
[text embedding](https://python.langchain.com/docs/integrations/text_embedding/)    
[text embedding models](https://python.langchain.com/docs/how_to/embed_text/)     

<img width="996" alt="image" src="https://github.com/user-attachments/assets/9ceaddad-1142-410e-87b8-a3b8dbf160d1" />   
<img width="994" alt="image" src="https://github.com/user-attachments/assets/c43155fe-1250-456b-ac38-d596c05692d9" />    
백터스토어 (예로 cromadb로 테스트 해봄)    

## Retriever 
<img width="823" alt="image" src="https://github.com/user-attachments/assets/0d664560-1c3c-49ae-a05f-3acc0fafeadb" />
[Colab 예제](https://colab.research.google.com/drive/13yxs8cRNxDEjr8YbpWbANRqE4HJII23Y)    
[retriever](https://python.langchain.com/docs/how_to/#retrievers)   

## huggingface embedding
무료 임베딩 모델 테스트     
[colab 예제](https://colab.research.google.com/drive/1NNCIDGHPB_U32iMqKEyRkqqS2LJkc7lt)    
[임베딩 모델 비교](https://huggingface.co/spaces/mteb/leaderboard)    
<img width="826" alt="image" src="https://github.com/user-attachments/assets/00dd0a34-c26a-4266-926e-d92ea0eb2c9a" />

## 이전 내용 기억(메모리)   
[colab 예제](https://colab.research.google.com/drive/15Xz086eK4xMzSpnxsfqdINB7qfaQJnEv)    
[langchain chatbot](https://python.langchain.com/docs/tutorials/chatbot/)
<img width="1001" alt="image" src="https://github.com/user-attachments/assets/ff087891-8c48-4926-9c23-3347e8da5aa3" />
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/ac2c891a-d06f-48f8-81f5-dfd9a2122b22" />
<img width="1002" alt="image" src="https://github.com/user-attachments/assets/e1d07d4e-31dc-4e8e-a0cd-38d911bd9a0c" />
<img width="1002" alt="image" src="https://github.com/user-attachments/assets/353d927b-ef51-4541-8261-37e803d7b874" />

## 예제 (판사GPT만들기)
[Colab 예제](https://colab.research.google.com/drive/1GrGMHo2Le5DRcMRI5yXz1HjpeUSnyXVk)    
[Colab 예제](https://colab.research.google.com/drive/1op-I0PcmOROCWZy7tiIdhbUZ87ZT_RLt)    

## 예제 (특허 GPT 만들기)   
[Colab 예제](https://colab.research.google.com/drive/18xN1z2aiDaWD6vM5epqTh0Nbc130cXTS)    
[Colab 예제](https://colab.research.google.com/drive/1QSRc2gWiNxwOMMyYC4dD08jEkM0D6TM1)    

## Langchain으로 엔티티 추출
[Colab 예제](https://colab.research.google.com/drive/1S8JdDU3moHAtNiADbcsDN_VcO6z7GAjo)    
[langchain entity extration](https://python.langchain.com/docs/tutorials/extraction/)     
<img width="1024" alt="image" src="https://github.com/user-attachments/assets/91df03a8-d71b-4e1e-841d-7643749b2817" />

## 예제 (리뷰감정분석 GPT)
[Colab 예제](https://colab.research.google.com/drive/1cdbGYY9WKxlw7sPrhyxnALey1AQAo9d8)    

## 랭체인에 llama모델 연동하기
[Colab 예제](https://colab.research.google.com/drive/1y8qqpiAux0dIdicMVd1HTql2h3SGEOdx)  
[llama.cpp](https://python.langchain.com/docs/integrations/llms/llamacpp/)    

[llama model](https://huggingface.co/TheBloke/CodeLlama-13B-Instruct-GGUF)    

