---
title: "Claude Code에서 PDF 분석 시 토큰 아끼는 방법 (feat. MarkItDown)"
excerpt_separator: "<!--more-->"
date: 2026-09-20
categories:
  - AI
tags:
  - [Claude, ClaudeCode, MarkItDown, MCP, PDF, Markdown]

toc : true
toc_sticky : true
---


Claude나 ChatGPT 같은 AI 모델에게 PDF 문서를 분석시킬 때, 파일을 그대로 업로드하면 토큰(Token) 소모가 매우 심합니다. 이때 마이크로소프트가 공개한 **MarkItDown**을 활용하여 PDF를 마크다운(Markdown)으로 변환한 뒤 입력하면 토큰을 획기적으로 절약할 수 있습니다. 

이번 글에서는 MarkItDown의 핵심 기능과 이를 **Claude Code**에 플러그인(MCP)으로 연동하여 사용하는 방법을 정리해 보겠습니다.

## 1. MarkItDown이란?

**MarkItDown**은 마이크로소프트에서 오픈소스로 공개한 파이썬 유틸리티로, PDF, Word, Excel 등 다양한 형식의 문서를 구조화된 마크다운 형식으로 깔끔하게 변환해 줍니다.

* **압도적인 토큰 절약:** PDF의 복잡한 레이아웃이나 불필요한 메타데이터 없이 순수 텍스트와 구조만 전달하여 토큰을 크게 절약합니다.
* **AI 친화적 구조 유지:** 제목(Heading), 표(Table), 리스트(List) 등 계층 구조를 훼손하지 않아 AI가 문서 구조를 완벽하게 이해합니다.
* **로컬 구동 및 보안:** 서버 업로드 없이 로컬 환경에서 변환되어 민감한 데이터 유출 걱정이 없습니다.

> **📺 참고 영상: MarkItDown 소개 및 활용법**  
> [How to Convert Any File to Markdown with Microsoft MarkItDown](https://www.youtube.com/watch?v=m-ufthgqh7o)  
> (다양한 문서를 마크다운으로 변환하는 과정과 장점을 시각적으로 잘 보여주는 튜토리얼 영상입니다.)

---

## 2. Claude Code에 MarkItDown 플러그인(MCP) 설정하기

Claude는 외부 도구를 연결하는 표준 규격인 **MCP(Model Context Protocol)**를 지원합니다. 마이크로소프트에서 제공하는 전용 패키지(`markitdown-mcp`)를 설치하면 Claude Code가 직접 문서를 마크다운으로 변환하여 읽을 수 있습니다.

### 🛠️ 설정 방법 (CLI)

1. **MCP 패키지 설치:**
   터미널을 열고 아래 명령어를 입력하여 패키지를 설치합니다.
   ```bash
   pip install markitdown-mcp

```

2. **Claude Code에 MCP 연동하기:**
현재 프로젝트에만 적용하려면 아래 명령어를 입력합니다.
```bash
claude mcp add markitdown -- markitdown-mcp

```


만약 모든 프로젝트(글로벌)에서 사용하고 싶다면 `-s user` 플래그를 추가합니다.
```bash
claude mcp add -s user markitdown -- markitdown-mcp

```



### 🚀 사용 방법

설정이 완료되면 Claude Code 내부에 `convert_to_markdown`이라는 도구가 추가됩니다.
이제 프롬프트에 다음과 같이 요청해 보세요.

> *"이 디렉토리에 있는 report.pdf 파일을 분석해 줘."*
> *"data/summary.docx 파일을 읽고 요약해 줘."*

그러면 Claude가 알아서 백그라운드에서 `markitdown` 도구를 호출해 문서를 마크다운으로 변환한 뒤, 텍스트 컨텍스트로 반영하여 토큰을 아끼면서 깊이 있는 분석을 진행합니다.

---

## 3. (보너스) Claude Desktop 앱에서 사용하기

CLI(Claude Code)뿐만 아니라 일반 사용자를 위한 **Claude Desktop 앱**에서도 MCP를 통해 동일하게 설정할 수 있습니다.

1. 터미널(혹은 명령 프롬프트)에서 `pip install markitdown-mcp`를 설치합니다.
2. Claude Desktop의 설정 파일인 `claude_desktop_config.json`을 엽니다.
3. `mcpServers` 항목에 아래 내용을 추가하고 앱을 재시작합니다.

```json
{
  "mcpServers": {
    "markitdown": {
      "command": "markitdown-mcp",
      "args": []
    }
  }
}

```

이제 데스크톱 앱 내에서도 파일 경로를 알려주거나 문서를 언급하면, Claude가 자동으로 마크다운 변환 기능을 활용하여 똑똑하고 효율적으로 분석을 시작합니다!

```
http://googleusercontent.com/youtube_content/1

```
