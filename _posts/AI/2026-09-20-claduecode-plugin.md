---
title: "Claude Code 바이브 코딩 최적화: 필수 플러그인 5종 완벽 가이드"
excerpt_separator: "<!--more-->"
date: 2026-09-20
categories:
  - AI
tags:
  - [Claude, VibeCoding, Ponytail, OmniRoute, Graphify, AgentSkills, ECC]

toc : true
toc_sticky : true
---

최근 Claude Code를 활용한 '바이브 코딩(Vibe Coding)'이 대세로 떠오르고
있습니다. 하지만 무턱대고 사용하다 보면 사용량 한도를 순식간에 태우거나, AI가
코드의 전체 맥락을 놓쳐 비효율적인 코드를 생성하는 경우가 많습니다.

이 문제를 다른 층위에서 공략하는 **플러그인 5가지**의 특징과 정확한 설치
방법을 정리했습니다. 글 마지막에는 다섯 개를 실제로 어떻게 엮어 쓰는지
단계별 워크플로우도 넣었습니다.

> **⚠️ 먼저 확인하세요.** 모든 설치 명령은 2026년 9월 기준 공식 저장소에서
> 확인한 것입니다. 이 바닥은 주 단위로 바뀌니 안 먹히면 각 저장소의 최신
> README를 먼저 보세요. 그리고 **AI가 알려준 설치 명령은 그대로 치기 전에
> 패키지명을 한 번 확인하는 습관**을 권합니다. 실제로 존재하지 않는 패키지명이
> 그럴듯하게 돌아다닙니다.

---

## 1. 포니테일 (Ponytail) 🐴

AI 에이전트에게 **"게으른 시니어 개발자"**의 마인드를 부여하는 플러그인입니다.
슬로건이 정확히 이겁니다. *"가장 좋은 코드는 애초에 쓰지 않은 코드다."*

### 💡 유용한 점

* **YAGNI 래더 강제:** 코드를 짜기 전 7단계를 순서대로 거칩니다.
  1. 이게 존재해야 하나? → 아니면 건너뛴다 (YAGNI)
  2. 이 코드베이스에 이미 있나? → 다시 쓰지 말고 재사용
  3. 표준 라이브러리로 되나? → 쓴다
  4. 플랫폼 기본 기능인가? → 쓴다
  5. 이미 설치된 의존성에 있나? → 쓴다
  6. 한 줄로 되나? → 한 줄로
  7. 그제서야: 동작하는 최소한

* **읽기는 게으르지 않습니다.** 래더는 문제를 이해한 *뒤에* 돕니다. 변경이
  닿는 코드를 읽고 실제 흐름을 추적한 다음에 단을 고릅니다. 해답에 게으를 뿐
  독해에 게으른 게 아닙니다.

* **게으르되 부주의하지는 않게:** 신뢰 경계 검증, 데이터 손실 처리, 보안,
  접근성은 **절대 잘라내지 않습니다.** 오버엔지니어링만 막습니다.

### 📊 벤치마크는 조금 걸러 들으세요

프로젝트가 공개한 v1.0.0 벤치마크는 토큰 16% 감소, 약 4배 빠름, 293줄 →
47줄입니다. 프론티어 모델(Haiku/Sonnet/Opus)에서는 코드량 80~94% 감소라는
수치도 나옵니다.

다만 이 숫자에 공개적인 반박이 있습니다. Scott Logic의 Colin Eberhardt는
원래 벤치마크가 단발성이었고, 베이스라인 모델이 산문으로 답을 늘리는
경향이 있어서 줄 수를 비교하면 격차가 부풀려진다고 지적했습니다. 더 뼈아픈
건, 그냥 **"Follow YAGNI principles, and prefer one-liner solutions"라는
7단어 프롬프트**가 Ponytail 점수에 거의 근접했다는 테스트 결과입니다.

프로젝트 쪽도 정직한 편입니다. llama3.2(3B) 로컬 모델 벤치마크에서 코드량
감소가 노이즈였다는 결과(한 번은 17% 감소, 다음은 50% 증가)를 묻지 않고
공개했습니다. 지시를 제대로 따르는 모델에 맞춰 튜닝됐다는 설명과 함께요.

정리하면 **프론티어 모델에서 쓸 만하고, 숫자는 마케팅 톤으로 읽으세요.**

### 🛠️ 설치 방법

Node.js 라이프사이클 훅 두 개를 돌리므로 `node`가 `PATH`에 있어야 합니다.
(nvm/Nix 사용자는 **비대화형 셸의 PATH**에 있어야 합니다.) 없으면 스킬은
동작하되 상시 활성화만 조용히 꺼집니다.

1. 터미널에서 `claude` 실행
2. 아래 두 줄을 **반드시 따로따로** 입력합니다. 한 번에 보내면 설치가 안 됩니다.

```
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

3. `/exit`으로 종료 후 `claude` 재실행

### 🚀 명령어

| 명령 | 하는 일 |
|---|---|
| `/ponytail` | 현재 강도 레벨과 상태 뱃지 확인 |
| `/ponytail [lite\|full\|ultra]` | 개입 강도 조절 |
| `/ponytail-review` | 현재 diff를 읽고 **뭘 지울지** 제안 (추천) |
| `/ponytail-audit` | 저장소 전체에서 오버엔지니어링 탐색 |
| `/ponytail-debt` | 기술 부채 점검 |
| `/ponytail-gain` | 효과 측정 |
| `/ponytail-help` | 전체 명령 목록 |

설치하면 매 세션 자동 활성화되고 `[PONYTAIL]` 상태줄 뱃지가 뜹니다.

---

## 2. 옵니라우트 [OmniRoute](https://younlea.github.io/ai/OmniRoute/) 🔀

여러 LLM 제공자를 **하나의 로컬 엔드포인트 뒤에 숨겨주는 게이트웨이**입니다.
쿼터가 떨어지거나 제공자가 죽으면 다음 대상으로 자동 전환합니다.

### 🚨 Pro / Max 구독자는 이것부터 읽으세요

**OmniRoute는 Claude Pro·Max 구독과 합쳐지지 않습니다.** 한도를 늘려주지도,
구독을 싸게 만들어주지도 않습니다.

Claude Code를 OmniRoute에 연결하는 순간 **그 터미널은 구독을 전혀 쓰지
않습니다.** Anthropic 지원 문서에 명시돼 있듯, `ANTHROPIC_API_KEY`나
`ANTHROPIC_BASE_URL`이 설정되면 Claude Code는 구독 대신 그 경로로 인증합니다.
OmniRoute 런처가 하는 일이 정확히 그겁니다.

반대로 구독 자격증명을 OmniRoute에 물리는 것도 무의미합니다. 구독으로
인증하는 서드파티 앱도 여전히 구독 한도에서 차감되니까요.

**그래서 Pro 구독자에게 OmniRoute는 대체재가 아니라 비상 차선입니다.**
한도가 남아 있을 땐 그냥 Claude를 쓰고, 막혔을 때 Kimi·GLM·DeepSeek 무료
티어로 넘어가 작업을 마저 끝내는 용도입니다.

### 💡 유용한 점

* **한도 소진 후에도 작업 지속:** 구독 → API 키 → 저가 → 무료 순서의 4단 폴백
* **키 없이 즉시 동작:** 설치 직후 가입도 API 키도 없이 `auto` 모델로 응답이 옵니다
* **로컬 우선:** 키는 AES-256-GCM으로 암호화해 로컬 저장, 프롬프트가 OmniRoute
  클라우드를 거치지 않습니다

제공자는 352개가 등록돼 있고 그중 152개가 무료 티어 메타데이터를 답니다.
무료 토큰은 월 약 **14.7억 개** 규모로 공시되는데, 프로젝트 본인들이 2주마다
재감사하며 **숫자가 양방향으로 움직인다**고 명시합니다. 그대로 믿지 마세요.

### 🛠️ 설치 방법

**npm (가장 간단)**

```bash
npm install -g omniroute
```

서버가 `localhost:20128`에서 뜹니다. CLI와 웹 대시보드가 같은 프로세스에서
한 포트로 서비스됩니다.

업데이트는 `omniroute update`를 쓰세요. 내부적으로 `--include=optional`이 항상
붙어서 돌기 때문에, npm 설정에 `omit=optional`이 있어도 네이티브 SQLite
드라이버나 OS 키링 바인딩이 조용히 빠지는 사고를 막아줍니다.

**Docker**

```bash
docker run -d --name omniroute --restart unless-stopped \
  -e OMNIROUTE_MEMORY_MB=8192 --memory=10g \
  -p 127.0.0.1:20128:20128 -v omniroute-data:/app/data \
  diegosouzapw/omniroute:latest
```

`-p` 앞의 `127.0.0.1`을 빼지 마세요. 빼면 LAN 전체에 열립니다. 그리고
코딩 에이전트를 물릴 거면 `OMNIROUTE_MEMORY_MB`를 꼭 키우세요. 기본값
1024로는 긴 컨텍스트 두 개가 겹치면 V8 힙이 터집니다.

> ⚠️ 이미지 이름은 `diegosouzapw/omniroute`입니다. `omniroute/gateway` 같은
> 이미지는 **존재하지 않습니다.** 원본 저장소는 `diegosouzapw/OmniRoute`이고
> 같은 이름의 포크가 GitHub에 여럿 있습니다.

### 동작 확인

```bash
curl http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"auto","messages":[{"role":"user","content":"ping"}]}'
```

---

## 3. 그래피파이 [Graphify](https://younlea.github.io/ai/Graphify/) 🕸️

프로젝트 폴더 전체를 **질의 가능한 지식 그래프**로 바꿔주는 도구입니다.
CLI 빌드 도구가 아니라 어시스턴트 안에서 호출하는 스킬이라는 게 핵심입니다.

### 💡 유용한 점

* **파일을 하나씩 읽는 걸 줄여줍니다.** "인증이 어디서 DB를 건드리지?"를 물으면
  에이전트가 grep 돌리고 파일을 열고 또 여는 걸, 그래프 조회로 대체합니다.
  (파일 읽기를 *차단*하는 건 아닙니다. 더 싼 경로를 주고 훅으로 유도합니다.)
* **코드 파싱은 로컬에서, API 호출 없이.** tree-sitter AST로 36개 문법을
  파싱합니다. 결정적이고, 아무것도 밖으로 안 나갑니다. 코드만 있는 프로젝트는
  API 키 없이 완전 오프라인으로 돕니다.
* **찾은 것과 추측한 것을 구분해줍니다.** 모든 관계에 `EXTRACTED`(소스에 명시),
  `INFERRED`(추론 + 신뢰도 점수), `AMBIGUOUS`(검토 필요) 태그가 붙습니다.
* **"왜"를 뽑아줍니다.** `# NOTE:`, `# WHY:`, `# HACK:` 같은 주석과 docstring,
  문서의 설계 근거를 별도 노드로 만들어 해당 코드에 연결합니다.

### 🛠️ 설치 방법

> ⚠️ **공식 패키지는 `graphifyy`입니다. y가 두 개.** 프로젝트가 README에
> "PyPI의 다른 `graphify*` 패키지는 무관하다"고 명시해뒀습니다.
> `graphify-code` 같은 패키지를 설치하라는 안내를 봤다면 잘못된 정보입니다.
> CLI 명령은 `graphify`(y 하나)가 맞습니다.

Python 3.10 이상이 필요합니다.

```bash
# 1단계: 설치 (uv 권장 — PATH 설정이 자동)
uv tool install graphifyy

# 또는
pipx install graphifyy

# pip도 되지만 Mac/Windows에서는 피하는 게 좋습니다
pip install graphifyy

# 2단계: 어시스턴트에 스킬 등록
graphify install
```

Mac/Windows에서 plain `pip`을 피하라는 건 프로젝트 권고입니다. 스킬이 런타임에
파이썬을 찾는 경로와 pip 설치 환경이 다르면 `ModuleNotFoundError`가 납니다.

### 🚀 사용 방법

Claude Code를 열고:

```
/graphify .
```

`graphify-out/` 폴더에 세 개가 나옵니다.

* `graph.html` — 브라우저로 열어서 노드 클릭, 필터, 검색
* `GRAPH_REPORT.md` — 핵심 개념, 의외의 연결, 던져볼 만한 질문
* `graph.json` — 전체 그래프. 파일을 다시 안 읽고 이걸 질의

**그리고 이 한 줄을 꼭 추가하세요.**

```bash
graphify claude install
```

`CLAUDE.md`와 PreToolUse 훅을 설치해서, Claude가 검색하거나 파일을 하나씩
읽기 전에 그래프를 먼저 보도록 유도합니다. **이걸 빼면 그래프만 만들어두고
안 쓰게 됩니다.**

질의는 이렇게 합니다.

```
/graphify query "인증 흐름을 보여줘"
/graphify path "UserService" "DatabasePool"
```

---

## 4. 에이전트 스킬 (Agent Skills) 🛠️

Claude Code에 전문 워크플로우를 추가하는 확장 방식입니다. `SKILL.md` 파일
하나에 "언제 쓰는지"와 "어떻게 하는지"를 적어두면, Claude가 상황에 맞을 때
알아서 불러옵니다.

### 💡 유용한 점

* **반복 프롬프팅 감소:** 매번 설명하던 규칙을 파일로 고정합니다. 코드 리뷰
  기준, 커밋 컨벤션, 배포 절차 같은 것들이요.
* **자동 호출:** Claude가 `description`을 보고 관련 있을 때 스스로 로드합니다.
  직접 `/스킬이름`으로 부를 수도 있고요.
* **팀 공유:** 프로젝트의 `.claude/skills/`에 넣고 git에 커밋하면 팀 전체가
  같은 워크플로우를 씁니다.

### 🛠️ 설치 방법

**방법 A — 직접 만들기 (제일 빠름)**

```bash
mkdir -p ~/.claude/skills/my-skill
```

그 안에 `SKILL.md`를 만들면 끝입니다.

```markdown
---
name: my-skill
description: 무엇을 하는지 + 언제 써야 하는지. 이 필드가 스킬의 생사를 가릅니다.
---

여기에 지침을 씁니다.
```

**방법 B — 플러그인 마켓플레이스**

```
/plugin marketplace add alirezarezvani/claude-skills
/plugin install engineering-skills@claude-code-skills
```

**방법 C — skills CLI** (Claude Code, Cursor, Codex 등 공용)

```bash
npx skills add sanjay3290/ai-skills --list    # 목록 확인
npx skills add sanjay3290/ai-skills --all     # 전부 설치
```

설치 후 `/skills`로 목록을 확인하고, 재시작 없이 반영하려면 `/reload-skills`를
실행합니다. `/skills`에서 `t`를 누르면 **토큰 수 기준 정렬**인데, 많이 깔면
컨텍스트를 갉아먹으니 주기적으로 확인하세요.

### 🎁 이미 들어 있는 것부터 써보세요

따로 설치할 필요 없이 Claude Code에 번들된 스킬이 꽤 됩니다.
`/code-review`, `/simplify`, `/security-review`, `/run`, `/verify`, `/batch`,
`/debug` 등이요. 특히 `/run`과 `/verify`는 "테스트는 통과하는데 실제로는 안
되는" 상황을 잡아줍니다.

> ⚠️ **스킬은 MCP가 아닙니다.** 스킬을 `mcpServers` 항목에 등록하라는 안내를
> 봤다면 잘못된 정보입니다. 둘은 다른 메커니즘이고, Claude Code에
> `clauderc.json`이라는 설정 파일은 존재하지 않습니다.

👉 **자세한 내용**: [Claude Code 에이전트 스킬 완전 정리](/ai/claude-code-agent-skills/)

---

## 5. ECC (Everything Claude Code) 🧠

Claude Code에 `계획 → 테스트 → 구현 → 리뷰 → 검증 → 기억 → 개선` 파이프라인을
통째로 얹는 에이전트 하네스 시스템입니다. 매번 프롬프트로 "TDD로 해줘"를
설명하는 대신, 그 절차를 설치해서 에이전트의 기본 동작으로 만듭니다.

### 💡 유용한 점

* **워크플로우 강제:** 계획을 채팅 기록에 흘려보내는 대신 편집 가능한
  산출물로 만들고, TDD를 RED → GREEN → REFACTOR 게이트로 돌립니다.
* **역할 분리:** 코드를 짠 컨텍스트가 아니라 **새 컨텍스트의 리뷰어**가
  검토합니다. 같은 대화에서 "리뷰해줘"라고 하면 자기가 방금 내린 판단을
  방어하기 쉬운데, 그 구조적 문제를 건드립니다.
* **AgentShield:** 프롬프트, 훅, MCP 설정, 권한, 시크릿, 에이전트 파일까지
  하네스 자체를 공격 표면으로 보고 스캔합니다.

MIT 라이선스, 이 글 기준 v2.2.2 / 에이전트 68개 / 스킬 292개 / 명령 94개입니다.

### ⚠️ 규모를 주의하세요

스킬 292개를 다 깔면 그 설명들이 컨텍스트를 상시 점유합니다. 프로젝트 문서도
이렇게 적어뒀습니다. *"플러그인은 설치된 카탈로그를 모델에게 알린다. 컨텍스트
사용량이 중요하다면 선택적/수동 프로파일을 쓸 것."*

"풀로 깔면 좋다"가 아닙니다. 프로파일을 골라야 합니다.

### 🛠️ 설치 방법

**설치 경로는 반드시 하나만 고르세요.** 같은 하네스에 두 번 설치하면 스킬,
명령어, 훅이 중복됩니다.

**권장 — 가이드 설치 마법사**

```bash
npx ecc-universal@2.2.2 setup
```

기존 설치 상태를 먼저 점검한 뒤 스코프(user/project/local)와 훅 프로파일을
물어보고 진행합니다. 업데이트나 스코프 변경도 같은 명령을 다시 실행하면 됩니다.

컨텍스트가 걱정되면 최소 프로파일로:

```bash
npx ecc-universal@2.2.2 install --profile minimal --target claude
```

**대안 — Claude Code 네이티브 플러그인 명령**

```
/plugin marketplace add https://github.com/affaan-m/ECC
/plugin install ecc@ecc
```

둘 중 하나만. 이 위에 수동 설치를 얹지 마세요.

**rules는 별도입니다.** Claude Code 플러그인은 rules를 배포할 수 없어서
직접 복사해야 합니다.

```bash
git clone https://github.com/affaan-m/ECC.git && cd ECC
mkdir -p ~/.claude/rules/ecc
cp -R rules/common ~/.claude/rules/ecc/
cp -R rules/typescript ~/.claude/rules/ecc/   # 본인 스택으로 교체
```

rules는 **상시 로드**되니 `common` 하나에 실제로 쓰는 언어 팩 하나 정도로
시작하세요.

### 첫 사용

```
/ecc:plan "OAuth 기반 사용자 인증 추가"
```

플러그인 설치는 `/ecc:` 네임스페이스를 씁니다. 수동 설치라면 `/plan`처럼
짧은 형태가 노출될 수 있습니다.

> ⚠️ **공식 채널만 사용하세요.** 저장소 `affaan-m/ECC`, npm `ecc-universal`과
> `ecc-agentshield`, 플러그인 슬러그 `ecc@ecc`, 사이트 `ecc.tools`뿐입니다.
> 서드파티 재업로드와 비공식 미러는 프로젝트가 관리·검토하지 않으며
> 악성코드가 있을 수 있다고 공식 경고돼 있습니다. 옛 마켓플레이스 식별자
> `everything-claude-code@everything-claude-code`는 이제 동작하지 않습니다.

👉 **자세한 내용**: [ECC 설치와 사용 완전 정리](/ai/ecc-everything-claude-code/)

---

# 🚀 실전 활용 가이드: 5가지를 엮어 쓰는 법

## 먼저: 다 깔 필요는 없습니다

"시너지"라는 말로 뭉뚱그리기 전에, 겹치는 지점부터 솔직하게 정리하겠습니다.

| 도구 | 작동 층위 | 겹침 |
|---|---|---|
| **OmniRoute** | 모델 라우팅 (프로세스 밖) | 없음 |
| **Graphify** | 컨텍스트 공급 | 없음 |
| **Ponytail** | 코드 생성 시 간결성 강제 | ECC와 **철학이 충돌** |
| **Agent Skills** | 워크플로우 정의 | ECC가 스킬 292개를 이미 포함 |
| **ECC** | 전체 프로세스 강제 | Ponytail, Agent Skills와 겹침 |

**Ponytail과 ECC는 서로 당깁니다.** Ponytail은 "코드를 최소로"를 밀고, ECC는
"테스트 먼저, 검증 증거 남기기"를 강제합니다. 테스트 코드는 코드량을 늘립니다.
둘 다 쓰면 Ponytail이 프로덕션 코드를, ECC가 프로세스를 맡는 식으로 역할이
갈리긴 하지만, 충돌을 느끼면 한쪽 강도를 낮추세요.

**ECC를 깔면 Agent Skills 섹션은 대체로 흡수됩니다.** 스킬을 직접 만들 생각이
없다면 4번은 건너뛰어도 됩니다. 반대로 내 팀 컨벤션만 몇 개 고정하고 싶은
정도라면 ECC는 과합니다.

### 조합 추천

| 상황 | 추천 조합 |
|---|---|
| 혼자, 작은 프로젝트 | Ponytail + 번들 스킬(`/code-review`, `/verify`) |
| 혼자, 파일 수백 개 | + Graphify |
| Pro 한도를 자주 침 | + OmniRoute (비상용) |
| 팀, 프로세스를 강제하고 싶음 | Graphify + ECC (Ponytail은 `lite`로) |
| 전부 | 컨텍스트 예산부터 확인하세요 |

### 🚨 훅 충돌 주의

Ponytail(라이프사이클 훅 2개), Graphify(PreToolUse 훅), ECC(훅 런타임) —
**셋 다 훅을 설치합니다.** 동시에 켜면 매 도구 호출마다 여러 훅이 발동해서
느려지거나 메시지가 시끄러워질 수 있습니다.

처음에는 ECC를 `--profile minimal`(훅 런타임 제외)로 깔고, 필요를 느낄 때
추가하는 순서를 권합니다.

---

## 1단계: 인프라 세팅 (최초 1회)

### OmniRoute — 띄워만 두세요

```bash
omniroute        # 백그라운드로 서버만 켜둡니다
```

**Pro/Max 구독자라면 여기서 멈추세요.** Claude Code의 엔드포인트를 상시
로컬로 돌리지 마세요. 그 순간 매달 내는 구독이 놀게 됩니다. 한도가 막혔을 때만
4단계에서 전환합니다.

구독이 없고 API 종량제로 쓰고 있다면, 이때는 상시 연결이 합리적입니다.

### Graphify — 지도를 만들고 Claude에게 쥐여주기

```bash
uv tool install graphifyy
graphify install
```

Claude Code를 열고:

```
/graphify .
```

그다음 **이 한 줄을 반드시 실행하세요.**

```bash
graphify claude install
```

이게 빠지면 지도를 만들어두고 Claude가 안 봅니다. 그리고 커밋마다 자동
재빌드를 걸어두면 편합니다. AST만 돌아서 API 비용이 0입니다.

```bash
graphify hook install
```

팀이라면 `graphify-out/`을 git에 커밋하세요. 팀원이 pull하는 순간 같은 지도를
갖게 됩니다.

### Ponytail — 설치하고 강도 정하기

```
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

`/exit` 후 재시작. ECC도 같이 쓸 거면 처음엔 약하게 시작하세요.

```
/ponytail lite
```

### ECC — 프로파일을 골라서

```bash
npx ecc-universal@2.2.2 install --profile minimal --target claude
```

rules는 수동 복사(위 5번 참고). 여기까지 하면 한 번 점검해볼 만합니다.

```
/skills      # t 키로 토큰 정렬 — 뭐가 무거운지 확인
/context     # 컨텍스트 윈도우 사용량
```

---

## 2단계: 기획 — 다짜고짜 코딩시키지 않기

바이브 코딩의 가장 흔한 실패는 "만들어줘" 한 마디로 시작하는 겁니다.
ECC는 이 지점을 막습니다.

```
/ecc:plan "소셜 로그인 기능 추가 (Google, GitHub OAuth)"
```

**프롬프트로 "ECC 워크플로우를 따라줘"라고 쓰지 마세요.** 슬래시 명령으로
부르는 게 정확합니다. 플래너 에이전트가 구현 청사진을 만들어 오고, 승인이나
수정을 요청합니다.

이 단계에서 Graphify 맵이 자동으로 활용됩니다. `graphify claude install`을
해뒀다면 Claude가 파일을 뒤지기 전에 그래프를 조회합니다. 명시적으로 물어볼
수도 있습니다.

```
/graphify query "현재 인증 관련 모듈이 어디에 흩어져 있어?"
```

계획이 마음에 안 들면 그 자리에서 고칩니다. **채팅 기록에 흘려보내는 대신
편집 가능한 산출물로 남는 게** ECC의 핵심입니다.

---

## 3단계: 구현 — TDD 게이트

계획을 승인하면 TDD 워크플로우로 넘어갑니다.

```
tdd-workflow 스킬 활성화
```

`tdd-guide` 에이전트가 테스트를 먼저 쓰게 강제하고, RED 증거를 남긴 뒤에
구현으로 들어갑니다. 이때 **Ponytail이 백그라운드에서 개입**합니다. 별도
명령이 필요 없습니다. 코드를 뱉기 전에 YAGNI 래더를 돌려 "기존 코드로 되나?
한 줄로 되나?"를 먼저 검증합니다.

구현이 길어지면 중간에 한 번 끊고 확인하세요.

```
/ponytail-review     # 방금 짠 diff에서 지울 것 찾기
/context             # 컨텍스트 압박 확인
```

---

## 4단계: 한도가 막혔다면

Pro는 claude.ai, 데스크톱 앱, Claude Code가 **하나의 사용량 풀**을 공유합니다.
오전에 채팅을 길게 하면 오후 코딩에서 쓸 게 줄어듭니다.

막혔을 때 선택지는 이렇습니다.

1. **Usage credits 켜기** — 포함 한도를 넘긴 뒤 종량제로 계속. 지출 상한도
   걸 수 있어서 가장 단순합니다
2. **Max 업그레이드** — 꾸준히 한도를 친다면 이게 정답입니다
3. **OmniRoute로 전환** — 무료 티어로 작업을 마저 끝냅니다

3번은 설정 파일을 안 건드리는 방식이 제일 편합니다.

```bash
omniroute run claude --model auto/offline
```

`auto/offline`은 쿼터 여유가 가장 많은 제공자를 고릅니다. 터미널만 닫으면
원래대로 Pro로 돌아옵니다. **되돌리는 작업이 없다는 게 이 방법의 장점입니다.**

> 품질은 떨어질 수 있습니다. 설계 판단이 필요한 작업은 한도가 리셋된 뒤로
> 미루고, 보일러플레이트나 테스트 작성처럼 기계적인 일을 이때 몰아 하는 게
> 낫습니다.

---

## 5단계: 검증 — 새 컨텍스트로

구현이 끝났다고 끝난 게 아닙니다.

```
/code-review         # 새 컨텍스트 리뷰어가 회귀와 맹점을 본다
/verify              # 테스트가 아니라 실제로 앱을 띄워서 확인
/security-scan       # OWASP Top 10 감사
```

`/verify`가 특히 저평가돼 있습니다. "테스트는 다 통과하는데 실제로는 안 되는"
상황을 잡아줍니다.

마지막으로 오버엔지니어링을 한 번 훑습니다.

```
/ponytail-review
```

---

## 6단계: 세션 정리 — 다음 번을 위해

긴 세션을 그냥 닫으면 이번에 배운 게 사라집니다.

```
/learn-eval          # 이번 세션에서 패턴을 추출해 평가하고 저장
/save-session        # 세션 요약 저장
```

구조가 크게 바뀌었으면 지도도 갱신합니다. (훅을 걸어뒀다면 커밋 시 자동)

```
/graphify . --update
```

다음 세션은 이걸로 이어갑니다.

```
/resume-session
```

---

## 💡 요약: 실제 대화 흐름

세팅이 끝나면 개발자가 치는 건 이 정도입니다.

> **👨‍💻** `/ecc:plan "결제 모듈 추가 — Stripe 연동, 웹훅 처리"`
>
> **🤖** *(Graphify 그래프로 기존 구조 파악)* "현재 주문 플로우가
> `OrderService`에 있고 웹훅 핸들러가 없습니다. 3단계 구현 계획을 세웠습니다.
> 검토해주세요."
>
> **👨‍💻** "2단계는 기존 `WebhookRouter` 재사용해. 나머지 진행."
>
> **🤖** *(tdd-workflow: 실패 테스트 작성 → 구현 → Ponytail이 간결성 검증)*
> "테스트 12개 통과. 커버리지 84%."
>
> **👨‍💻** `/code-review`
>
> **🤖** *(새 컨텍스트 리뷰어)* "웹훅 서명 검증이 빠졌습니다. 재시도 로직의
> 멱등성 처리도 필요합니다."

여기서 중요한 건 마지막 턴입니다. **같은 컨텍스트였으면 "잘 구현했습니다"로
끝났을 지점**에서 실제 문제 두 개가 나옵니다. 도구를 엮는 이유가 이거예요.

---

## ⚠️ 마지막 당부

이 다섯 개는 전부 **서드파티**입니다. 훅과 스크립트를 실행하고, 일부는 API
키를 다룹니다. 설치 전에 다음을 확인하세요.

* 패키지명이 공식 저장소와 정확히 일치하는가 (`graphifyy`, `ecc-universal`,
  `diegosouzapw/omniroute`)
* 포크가 아닌 원본 저장소인가
* 훅이 뭘 하는지 `SKILL.md`나 훅 정의를 읽어봤는가

그리고 각 도구의 최신 명령어는 **공식 GitHub 저장소**를 확인하세요.
이 바닥은 주 단위로 바뀝니다.
