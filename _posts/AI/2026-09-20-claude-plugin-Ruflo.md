---
title: "[Claude Code] AI 개발팀을 구성하는 멀티 에이전트 프레임워크, Ruflo 설치 및 활용 가이드"
excerpt_separator: "<!--more-->"
date: 2026-09-20
categories:
  - AI
tags:
  - [Claude, ClaudeCode, Ruflo, ClaudeFlow, MultiAgent, MCP]

toc : true
toc_sticky : true
---


AI 코딩 보조 도구를 사용할 때 단일 AI 모델의 맥락(Context) 한계나 일회성 답변에 아쉬움을 느낀 적이 있으신가요? 

**Ruflo(구 Claude Flow)**는 Claude Code 환경에서 여러 AI 에이전트가 역할을 분담하고, 기억을 공유하며 복잡한 개발 작업을 협업하도록 돕는 **오케스트레이션(Swarm Framework)** 도구입니다.

이번 글에서는 Ruflo가 무엇인지, 어떻게 설치하고 세팅하는지, 그리고 상황에 맞춰 켜고 끄는 효율적인 사용법까지 정리해 보겠습니다.

---

## 1. Ruflo란 무엇인가요?

Ruflo는 단순한 코드 완성 도구를 넘어, 개발 프로젝트에 **"AI 전담 팀"**을 꾸려주는 프레임워크입니다.

* **멀티 에이전트 협업:** 아키텍트, 보안 담당, 테스터, 코더 등 전용 역할을 맡은 여러 에이전트가 병렬로 작업을 수행합니다.
* **SPARC 방법론 적용:** 명세(Specification) → 계획(Plan) → 설계(Architecture) → 조사(Research) → 코딩(Coding)의 단계적 프로세스로 안정적인 코드를 만듭니다.
* **공유 기억(AgentDB):** 작업 내역, 과거 에러 해결 기록, 프로젝트 규칙을 내장 DB에 기억하여 맥락 끊김을 방지합니다.
* **MCP(Model Context Protocol) 지원:** 표준 MCP 기반으로 동작하여 기존 개발 환경에 자연스럽게 녹아듭니다.

---

## 2. 사전 준비 (Prerequisites)

Ruflo를 실행하려면 아래 환경이 준비되어 있어야 합니다.

1. **Node.js 20 이상** 설치
2. **Claude Code** 설치 및 계정 로그인 완료 (`claude` 명령어 사용 가능 상태)

---

## 3. Ruflo 설치 및 초기 세팅

설치는 프로젝트 단위로 진행되며, 복잡한 설정 파일 작성 없이 대화형 마법사를 통해 진행됩니다.

### 1) 프로젝트 이동 및 설치 명령어 실행
Ruflo를 적용할 프로젝트 폴더로 이동한 후 터미널에 아래 명령어를 입력합니다.

```bash
cd /path/to/your-project
npx claude-flow@latest init --sparc
