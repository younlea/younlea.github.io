---
title: "Addy Osmani의 Agent Skills — AI 에이전트에게 시니어 개발자의 절차를 주입하기"
excerpt_separator: "<!--more-->"
date: 2026-09-20
categories: [AI, Dev]
tags: [claude-code, agent-skills, addy-osmani, sdlc, ai-tooling]
toc: true
toc_sticky: true
---

## 먼저 정리하고 갈 것

이 프로젝트를 소개하는 글 중에 "구글이 공개한 스킬 세트"라거나 저장소 주소를
`google/skills`, `google/agents-cli`로 적어둔 것들이 있습니다. **둘 다
틀렸습니다.**

실제 저장소는 **`addyosmani/agent-skills`** 하나입니다. MIT 라이선스이고,
구글 회사 프로젝트가 아니라 **Addy Osmani 개인의 오픈소스**입니다.

만든 사람이 구글 크롬 엔지니어링 리드로 유명한 건 맞습니다. 하지만 "구글이
만들었다"와 "구글에서 일하는 사람이 만들었다"는 다른 얘기죠. 회사의 공식 지원도,
보증도 없습니다.

---

## 무엇을 해결하려는 도구인가

AI 코딩 에이전트에게는 공통된 실패 패턴이 있습니다. **스펙을 건너뛰고,
테스트를 건너뛰고, 보안 리뷰를 건너뜁니다.** "정확함"보다 "완료"를 향해
최적화하거든요.

이 팩은 그걸 막으려고 만들어졌습니다. 시니어 엔지니어가 실제로 따르는
워크플로우와 품질 게이트를 구조화된 스킬로 인코딩합니다. 원칙은 세 줄로
요약됩니다.

> 코드보다 스펙이 먼저. 머지보다 테스트가 먼저. 최적화보다 측정이 먼저.

2026년 2월 공개 이후 저장소 스타가 이 글 기준 8만 개 가까이 됩니다.

---

## 구조

### 6단계 라이프사이클

| 단계 | 하는 일 |
|---|---|
| **Define** | 아이디어를 다듬고, 코드 한 줄 쓰기 전에 스펙을 쓴다 |
| **Plan** | 작고 검증 가능한 작업으로 분해한다 |
| **Build** | 점진적 구현, 컨텍스트 엔지니어링, 깔끔한 API 설계 |
| **Verify** | TDD, DevTools 브라우저 테스트, 체계적 디버깅 |
| **Review** | 코드 품질, 보안 강화, 성능 최적화 |
| **Ship** | git 워크플로우, CI/CD, ADR, 출시 전 체크리스트 |

4단계 이름이 Test가 아니라 **Verify**입니다. 테스트를 돌리는 것과 실제로
동작하는지 검증하는 건 다르다는 관점이 이름에 들어가 있습니다.

### 구성 요소

* **스킬 25개** — 처음 공개 때 19개였다가 20개, 지금 25개입니다. 계속 늘어나니
  개수는 참고만 하세요
* **슬래시 커맨드 7개** — 라이프사이클에 매핑
* **전문가 페르소나** — `code-reviewer`, `security-auditor`, `test-engineer`,
  `web-performance-auditor`
* **참조 체크리스트** — 저장소 레벨 `references/` 디렉터리

---

## 진짜 특징: 합리화 방지 테이블

기능 목록만 보면 "또 하나의 프롬프트 모음"처럼 보입니다. 이 팩을 구별 짓는 건
따로 있어요.

**각 스킬마다 에이전트가 단계를 건너뛸 때 흔히 대는 핑계와 그 반박이 표로
정리돼 있습니다.**

"테스트는 나중에 추가할게요." "이건 간단한 변경이라 스펙이 필요 없어요."
"일단 동작하게 만들고 리팩터링하죠." 이런 문장들이요. AI가 지름길로 새는
지점을 미리 파악해서 대응을 문서화해둔 겁니다.

프롬프트에 "TDD로 해줘"라고 쓰는 것과 "TDD를 건너뛰려는 이 여섯 가지 논리에는
이렇게 답하라"고 명시하는 것의 차이입니다. 후자가 훨씬 잘 버팁니다.

각 스킬은 단계별 절차와 **통과 조건(verification gate)**도 명시합니다.
"잘 했으니 넘어가자"가 아니라 조건을 만족해야 다음 단계로 갑니다.

---

## 설치

### 방법 1 — Claude Code 플러그인 (추천)

```
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

**마켓플레이스 이름과 플러그인 이름이 다릅니다.** `agent-skills@addy-agent-skills`가
맞습니다. `agent-skills@agent-skills`로 치면 안 됩니다.

설치하면 슬래시 커맨드 7개가 생기고, 에이전트가 문맥에 따라 관련 스킬을
자동으로 활성화합니다. 저자 본인이 대부분의 사람에게 권하는 경로입니다.

> 마켓플레이스 추가에서 막히면 README에 문서화된 HTTPS 형식을 쓰거나,
> GitHub SSH 키를 먼저 설정하세요.

### 방법 2 — skills CLI (70개 이상 에이전트)

Claude Code, Cursor, Codex, Copilot, Cline 등에 공통으로 깔 수 있습니다.

```bash
npx skills add addyosmani/agent-skills --list     # 먼저 둘러보기
npx skills add addyosmani/agent-skills            # 25개 전부
```

필요한 것만 골라도 됩니다.

```bash
npx skills add addyosmani/agent-skills --skill code-review-and-quality
npx skills add addyosmani/agent-skills --skill test-driven-development
npx skills add addyosmani/agent-skills --skill interview-me
```

> ⚠️ **개별 설치는 `skills/<이름>/`만 복사하고 저장소 레벨의 `references/`
> 디렉터리는 가져오지 않습니다.** 참조 체크리스트까지 필요하면 전체를 설치하세요.

### 방법 3 — 마크다운을 직접 넣기

스킬은 frontmatter가 붙은 평범한 마크다운입니다.

* **Cursor** → `.cursor/rules/`에 넣습니다
* **Gemini CLI** → 자체 설치 경로가 있습니다
* **Codex, Aider, Windsurf, OpenCode** → 시스템 프롬프트를 받는 도구면 읽습니다

**Command Code**를 쓴다면:

```bash
cmd skills add addyosmani/agent-skills              # 프로젝트에 설치
cmd skills add addyosmani/agent-skills --global     # ~/.commandcode/skills/
cmd skills add addyosmani/agent-skills -s spec-driven-development
```

### 방법 4 — 설치하지 않고 읽기

저자가 직접 권하는 사용법 중 하나입니다.

> 아무것도 설치하지 않더라도, 이 스킬들은 **AI와 함께하는 좋은 엔지니어링이
> 어떤 모습인지를 문서화한 명세**입니다.

`code-review-and-quality.md`의 5축 리뷰 프레임워크를 팀의 리뷰 프로세스에
그냥 적용해도 됩니다. 도구보다 그 밑에 깔린 워크플로우가 본체라는 게
저자의 입장이에요.

---

## 슬래시 커맨드

| 명령 | 단계 |
|---|---|
| `/spec` | 코드 한 줄 쓰기 전에 스펙 작성 |
| `/plan` | 작고 검증 가능한 작업으로 분해 |
| `/build` | 점진적 구현 |
| `/test` | TDD, 브라우저 테스트 |
| `/review` | 품질·보안·성능 리뷰 |
| `/code-simplify` | 단순화 |
| `/ship` | git, CI/CD, 출시 전 체크 |

### 먼저 써볼 만한 스킬

**`interview-me`** — 요구사항을 **한 번에 한 질문씩** 캐묻습니다. 애매한
한 줄 요청으로 시작해서 엉뚱한 걸 만드는 사고를 막아줍니다. 개인적으로 이게
가장 즉각적인 효과를 냅니다.

**`code-review-and-quality`** — 5축 프레임워크로 머지 전 리뷰.

**`test-driven-development`** — RED → GREEN → REFACTOR를 강제.

**`doubt-driven-development`** — 구현 전에 적대적 리뷰를 붙입니다.

**`source-driven-development`** — 최신 공식 문서를 확인하게 강제합니다.
모델이 기억으로 API를 지어내는 걸 막는 용도예요.

---

## 도입 방법 — 두 갈래

저장소에 Adoption Guide가 따로 있습니다. 코드베이스 상태에 따라 접근이
갈립니다.

**신규 프로젝트** — 1일차부터 전체 라이프사이클을 돌립니다. `/spec`으로
시작해서 `/ship`까지.

**기존 코드베이스** — 점진적으로, **검증 우선(verification-first)**으로
굴립니다. 기존 코드에 갑자기 전체 절차를 씌우면 마찰만 큽니다. `/review`와
`/test`부터 붙여서 현재 상태를 파악한 뒤에 앞단으로 확장하는 순서입니다.

두 번째 경우가 대부분일 겁니다. 한꺼번에 25개를 다 켜지 마세요.

---

## ⚠️ 주의할 점

### `/build auto`는 승인 한 번에 여러 작업을 돌립니다

설계상 그렇게 만들어진 기능입니다. 계획을 생성하고 여러 작업을 한 번의
승인된 패스로 구현합니다.

편집, 커밋, 푸시, CI 변경, 배포는 **호스트의 평소 승인 절차 뒤에 두세요.**
중간에 방향이 틀어졌다 싶으면 멈추고 `/spec`이나 `/plan`으로 돌아가면 됩니다.

### `/plan`이 세 군데서 충돌합니다

1. Claude Code 내장 `/plan` — 플랜 모드 진입
2. 이 팩의 `/plan` — 작업 분해
3. ECC 수동 설치의 `/plan` — 구현 계획

이 팩을 쓴다면 내장 플랜 모드는 **`Shift+Tab`으로 전환**하는 습관을 들이세요.
`/review`, `/build`도 다른 플러그인과 겹칠 수 있습니다.

### ECC와는 둘 중 하나만

[ECC(Everything Claude Code)](/ai/ecc-everything-claude-code/)와 이 팩은
**같은 문제를 같은 방식으로 풉니다.** 스펙 → 계획 → 구현 → 검증 → 리뷰 →
배포에 가드레일을 씌우는 라이프사이클 커맨드 세트예요. 같이 깔면 명령어가
충돌하고, 같은 역할의 스킬 설명이 두 벌 상주하고, 두 시스템이 서로 다른
절차를 밀어서 에이전트가 갈팡질팡합니다.

| 이럴 때 | 고를 것 |
|---|---|
| 가볍게, 마크다운만, 여러 에이전트에서 | **Addy 팩** (25 스킬) |
| 메모리·지속학습·보안 스캔까지 | **ECC** (68 에이전트 / 292 스킬) |
| 모르겠다 | **Addy 팩부터.** 규모가 1/10이라 되돌리기 쉽습니다 |

### 컨텍스트 예산

25개면 ECC의 292개에 비하면 가볍지만 공짜는 아닙니다. 스킬의 frontmatter는
Claude가 언제 쓸지 판단하려고 항상 로드돼 있어야 하거든요.

```
/skills      # t 키로 토큰 수 정렬
/context     # 컨텍스트 윈도우 사용량
```

전부 깔기보다 `--skill`로 3~5개만 골라 시작하는 걸 권합니다.

---

## 스킬이란 정확히 뭔가

저자가 내린 정의가 깔끔해서 옮깁니다.

> 스킬은 상황이 요구할 때 에이전트의 컨텍스트에 주입되는, frontmatter가 붙은
> 마크다운 파일이다. 시스템 프롬프트 조각과 런북 사이 어디쯤이다.
> **스킬은 참조 문서가 아니다.**

마지막 문장이 중요합니다. API 레퍼런스나 긴 설명서를 스킬에 넣는 건 오용이에요.
스킬은 "이 상황에서 이렇게 하라"는 절차지, 읽을거리가 아닙니다.

스킬 메커니즘 자체가 궁금하다면 별도로 정리해뒀습니다.
👉 [Claude Code 에이전트 스킬 완전 정리](/ai/claude-code-agent-skills/)

---

## 정리

- 저장소는 **`addyosmani/agent-skills`** 하나. `google/*`이 아니고, 구글 공식
  프로젝트도 아닙니다
- Claude Code는 `/plugin install agent-skills@addy-agent-skills`
  (마켓플레이스 이름과 플러그인 이름이 다릅니다)
- 다른 에이전트는 `npx skills add addyosmani/agent-skills`
- 진짜 가치는 스킬 개수가 아니라 **합리화 방지 테이블**에 있습니다
- ECC와는 하나만 고르세요. 둘 다 라이프사이클을 강제합니다
- 기존 코드베이스라면 `/review`, `/test`부터 붙이는 검증 우선 도입을

설치가 부담스러우면 방법 4로 시작해보세요. `code-review-and-quality.md` 하나만
읽고 팀 리뷰에 적용해도 값어치는 합니다.

### 참고

- 저장소: <https://github.com/addyosmani/agent-skills>
- 저자 소개 글: <https://addyosmani.com/blog/agent-skills/>
- 시작 가이드: 저장소의 `docs/getting-started.md`
- Command Code 설정: 저장소의 `docs/commandcode-setup.md`

> 이 글은 2026년 9월 기준입니다. 스킬 개수가 19 → 20 → 25로 계속 늘고 있으니
> 최신 목록은 `npx skills add addyosmani/agent-skills --list`로 확인하세요.
