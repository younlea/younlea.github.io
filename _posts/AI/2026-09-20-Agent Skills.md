---
title: "Claude Code 에이전트 스킬 (Agent Skills) 완전 정리"
excerpt_separator: "<!--more-->"
date: 2026-09-20
categories:
  - AI
tags:
  - [claude-code, agent-skills, mcp, ai-tooling]

toc : true
toc_sticky : true
---

| | Agent Skills | MCP |
|---|---|---|
| 정체 | `SKILL.md` 마크다운 파일 | 별도 프로세스로 뜨는 도구 서버 |
| 하는 일 | Claude에게 **어떻게 일할지** 알려줌 | Claude에게 **새 도구**를 쥐여줌 |
| 설치 위치 | `~/.claude/skills/` 또는 `.claude/skills/` | `.mcp.json` 또는 `claude mcp add` |
| 예시 | 코드 리뷰 기준, 배포 절차, 문서 작성 규칙 | DB 조회, GitHub API, 브라우저 제어 |

둘 다 Claude Code를 확장하지만 층이 다르다. 스킬은 **지식과 절차**고, MCP는
**도구와 연결**이다.

---

## 스킬이 뭔가

Agent Skills는 전문 지식을 발견 가능한 기능으로 패키징한 것이다. 각 스킬은
Claude가 관련 있을 때 읽는 지침이 담긴 `SKILL.md` 파일과, 선택적인 스크립트나
템플릿 같은 지원 파일로 구성된다.

```
skill-name/
└── SKILL.md     # 필수: YAML frontmatter + 마크다운 본문
```

### 핵심은 "모델이 알아서 부른다"는 것

이게 슬래시 명령과 갈리는 지점이다. 스킬은 **모델 호출(model-invoked)**된다.
사용자의 요청과 스킬의 `description`을 보고 Claude가 언제 쓸지 자율적으로
판단한다. 물론 `/스킬이름`으로 직접 부를 수도 있다.

그래서 **`description`이 스킬의 생사를 가른다.** 여기가 애매하면 스킬이
조용히 한 번도 안 불리고, 사용자는 "설치가 안 됐나?" 하고 헤맨다. 실제로
스킬 작성에서 가장 흔한 실패 원인이 이거다.

---

## 만들어 보기

### 가장 단순한 형태

`~/.claude/skills/summarize-changes/SKILL.md`를 만든다.

```markdown
---
name: summarize-changes
description: 커밋하지 않은 변경 사항을 요약하고 위험한 부분을 짚어준다.
  사용자가 뭐가 바뀌었는지 묻거나, 커밋 메시지를 원하거나,
  diff 리뷰를 요청할 때 사용한다.
---

## 현재 변경 사항

!`git diff HEAD`

## 지침

위 변경 사항을 두세 개의 불릿으로 요약한 뒤, 누락된 에러 처리,
하드코딩된 값, 업데이트가 필요한 테스트 같은 위험 요소를 나열한다.
diff가 비어 있으면 커밋되지 않은 변경이 없다고 말한다.
```

끝이다. 폴더 이름이 곧 명령어가 되고, `description`을 보고 Claude가 자동
로드 여부를 판단한다.

### `!` 백틱 — 동적 컨텍스트 주입

위 예시의 이 줄이 재미있는 부분이다.

```
!`git diff HEAD`
```

Claude Code가 이 명령을 **먼저 실행하고 그 출력으로 줄을 치환한 뒤에**
Claude에게 스킬 내용을 보여준다. 덕분에 "지금 상태"를 스킬 안에 박아 넣을 수
있다. 현재 브랜치, 설치된 패키지 버전, 테스트 결과 같은 걸 넣어두면 훨씬
정확하게 동작한다.

### frontmatter 필드

| 필드 | 설명 |
|---|---|
| `name` | 스킬 이름 (kebab-case) |
| `description` | **언제 쓰는지.** 가장 중요한 필드 |
| `allowed-tools` | 이 스킬이 활성화됐을 때 쓸 수 있는 도구 제한 |
| `disable-model-invocation` | 자동 호출을 끄고 `/이름`으로만 부르게 함 |
| `argument-hint` | 인수 힌트 |
| `license`, `compatibility`, `metadata` | 배포용 메타데이터 |

`allowed-tools`는 읽기 전용 스킬이나 범위를 좁히고 싶은 보안 민감 워크플로우에
쓴다. 지정하지 않으면 Claude는 평소처럼 표준 권한 모델에 따라 도구 사용
권한을 요청한다.

> **주의:** `disable-model-invocation`은 Claude Code CLI 전용이다. 같은 스킬을
> Claude 데스크톱 앱에 업로드하면 frontmatter 검증 오류가 난다. 데스크톱은
> `name`, `description`, `license`, `allowed-tools`, `compatibility`,
> `metadata`만 받는다.

### description 잘 쓰는 법

Anthropic의 스킬 개발 가이드가 권하는 방식은 **3인칭**이다.

```yaml
# 좋음
description: This skill should be used when the user asks to review a
  pull request, mentions "PR 리뷰", or requests feedback on a diff.

# 덜 좋음
description: PR을 리뷰합니다.
```

"무엇을 하는가"와 "언제 써야 하는가"를 **둘 다** 넣고, 실제 사용자가 칠 법한
트리거 문구를 구체적으로 적는 게 핵심이다.

---

## 설치하는 세 가지 경로

### A. 그냥 복사

개인용이면 `~/.claude/skills/`, 프로젝트 전용이면 `.claude/skills/`.

```bash
# 개인 — 모든 프로젝트에서
cp -r my-skill ~/.claude/skills/

# 프로젝트 — git에 커밋해서 팀과 공유
cp -r my-skill .claude/skills/
```

프로젝트 스킬을 커밋하면 팀원이 pull하는 순간 같은 워크플로우를 쓰게 된다.
이게 스킬의 실질적인 가치 중 절반이다.

### B. 플러그인 마켓플레이스

Claude Code 안에서 슬래시 명령으로 설치한다.

```
/plugin marketplace add <owner>/<repo>
/plugin install <플러그인>@<마켓플레이스>
/plugin list
```

플러그인 스킬은 플러그인의 일부로 배포되므로, 플러그인을 설치하면 스킬이
따라온다. 별도 ZIP을 받을 필요가 없다.

변경 사항을 재시작 없이 반영하려면:

```
/reload-plugins
```

### C. skills CLI (크로스 에이전트)

Claude Code, Cursor, Codex, Gemini CLI 등 여러 에이전트에 한 번에 깔고 싶을 때.

```bash
npx skills add <owner>/<repo> --list                    # 목록 확인
npx skills add <owner>/<repo> --skill <이름>             # 하나만
npx skills add <owner>/<repo> --all                     # 전부
npx skills add <owner>/<repo> --all -a claude-code      # 에이전트 지정
npx skills add <owner>/<repo> --all -a '*'              # 전부에 설치
```

기본값은 현재 프로젝트 디렉터리다. 범용 표준 위치인 `~/.agents/skills/`를
쓰는 에이전트도 있다.

### 그 외

**Claude.ai** — 스킬 폴더를 zip으로 압축해서 설정 → Capabilities → Skills에서
업로드한다.

**Claude API** — Messages API의 `container.skills`로 전달한다. Code Execution
Tool 베타가 필요하다.

---

## 설치 후 확인과 관리

```
/skills           # 사용 가능한 스킬 목록
/reload-skills    # 디스크를 다시 스캔 (재시작 불필요)
```

`/skills`는 생각보다 기능이 많다. 이름으로 필터링되고, `t`를 누르면 **토큰 수
기준 정렬**이다. 스킬을 많이 깔면 컨텍스트를 갉아먹기 때문에 이게 중요하다.
`Space`로 각 스킬의 가시성을 순환시키고 `Enter`로 저장하면, Claude에게는
보이되 `/` 메뉴에서는 숨기는 식의 조정이 가능하다.

유료 요금제라면 `/usage`에서 스킬·서브에이전트·플러그인·MCP 서버별 사용량
분석을 볼 수 있다. 어떤 스킬이 실제로 일하고 있는지 확인하는 데 유용하다.

### 스킬 체이닝

v2.1.199부터 여러 스킬을 이어 붙일 수 있다.

```
/skill-a /skill-b 이 diff 검토해줘
```

앞에 명명된 스킬을 전부 로드하고 뒤에 붙은 텍스트를 각각에 인수로 넘긴다.
최대 6개까지 연결된다.

---

## 이미 들어 있는 스킬들

따로 설치할 필요 없이 Claude Code에 번들된 스킬이 꽤 된다. 직접 만들기 전에
이것부터 써보는 게 낫다.

| 명령 | 하는 일 |
|---|---|
| `/code-review` | 현재 diff를 정확성 버그 + 정리 관점으로 검토. `--fix`로 적용 |
| `/simplify` | 버그 탐색 없이 정리만. 4개 에이전트가 병렬로 재사용·단순화·효율성 검토 |
| `/security-review` | 주입, 인증, 데이터 노출 등 보안 취약점 분석 |
| `/run` | 테스트가 아니라 **실제로 앱을 띄워서** 변경이 동작하는지 확인 |
| `/verify` | 빌드하고 실행해서 결과를 관찰 |
| `/batch` | 대규모 변경을 5~30개 단위로 쪼개 worktree별 백그라운드 에이전트에 분배 |
| `/dataviz` | 차트·대시보드 디자인 지침. 색맹 안전성과 대비까지 검증 |
| `/debug` | 디버그 로깅을 켜고 세션 로그를 읽어 문제 진단 |
| `/loop` | 세션이 열려 있는 동안 프롬프트를 반복 실행 |
| `/fewer-permission-prompts` | 트랜스크립트를 스캔해 안전한 명령의 허용 목록을 자동 생성 |

`/run`과 `/verify`가 특히 저평가돼 있다. "테스트는 통과하는데 실제로는 안
되는" 상황을 잡아준다. 프로젝트에 맞게 가르치려면 `/run-skill-generator`를
실행하면 프로젝트 전용 스킬을 써준다.

---

## 쓸 만한 스킬 팩

직접 만들기 전에 남이 만든 걸 보는 게 빠르다. 실제로 존재하는 것들만 적는다.

**`anthropics/skills`** — 공식 저장소. 스킬 작성법의 레퍼런스로 읽을 가치가 있다.

**`alirezarezvani/claude-skills`** — 도메인별로 번들이 나뉜 대형 컬렉션.
엔지니어링 코어 24개, 고급 25개, 제품 12개 등으로 구성돼 있다.

```
/plugin marketplace add alirezarezvani/claude-skills
/plugin install engineering-skills@claude-code-skills
```

**`sanjay3290/ai-skills`** — 24개 크로스플랫폼 스킬. DB(Postgres/MySQL/MSSQL),
메시징, 리서치, TTS, DevOps, Google Workspace.

```bash
npx skills add sanjay3290/ai-skills --list
```

**`glebis/claude-skills`** — 약 100개. 미팅 파이프라인, 리서치, 이미지 생성,
TDD, 퍼블리싱 등 개발 외 영역이 많다.

---

## 주의할 점

**스킬은 코드를 실행할 수 있다.** 지원 파일에 Python이나 셸 스크립트를 번들할
수 있고, 스킬이 활성화되면 Claude가 그걸 실행한다. 이게 스킬의 장점이기도
하다. "조심해서 하라"고 모델에게 부탁하는 대신 결정적인 스크립트에 맡길 수
있으니까. 하지만 **남이 만든 스킬을 설치하기 전에 `SKILL.md`와 번들 스크립트를
직접 읽어보자.** 마켓플레이스에 올라온 스킬이 전부 검증된 건 아니다.

**많이 깔수록 컨텍스트가 줄어든다.** 스킬의 frontmatter는 항상 로드돼 있어야
Claude가 언제 쓸지 판단할 수 있다. 100개를 깔면 100개의 설명이 상시 점유한다.
`/skills`에서 `t`로 토큰 정렬해서 주기적으로 솎아내자.

**description이 나쁘면 조용히 실패한다.** 에러가 안 난다. 그냥 안 불릴 뿐이다.
디버깅 방법이 하나 있는데, Claude에게 이렇게 물어보는 거다.

> "언제 `my-skill` 스킬을 쓸 거야?"

Claude가 `description`을 그대로 읊어준다. 뭐가 빠졌는지 바로 보인다.

**커스텀 명령은 스킬로 통합됐다.** 예전 `.claude/commands/` 방식 문서를 보고
있다면 최신 문서를 확인하자.

---

## 정리

- 스킬은 `SKILL.md` 파일이다. MCP 서버가 아니고, `mcpServers`에 등록하지 않는다
- `~/.claude/skills/`(개인) 또는 `.claude/skills/`(프로젝트, git 커밋 대상)에 둔다
- `description`이 전부다. 무엇을 + 언제를, 3인칭으로, 트리거 문구를 구체적으로
- 설치는 폴더 복사 / `/plugin install` / `npx skills add` 세 가지
- `/skills`로 확인, `/reload-skills`로 재스캔
- 만들기 전에 번들 스킬(`/code-review`, `/run`, `/verify`, `/batch`)부터 써보자

### 참고

- [Claude Code Skills 공식 문서](https://code.claude.com/docs/en/skills)
- [Claude Code 명령어 레퍼런스](https://code.claude.com/docs/ko/commands)
- 스킬 작성 가이드: `anthropics/claude-code` 저장소의
  `plugins/plugin-dev/skills/skill-development/SKILL.md`

> 이 글은 2026년 9월 기준이다. Claude Code는 릴리스 주기가 짧아서 일부 기능에
> 최소 버전 요구사항이 있다(`/reload-skills`는 v2.1.152+, 스킬 체이닝은
> v2.1.199+). 명령이 안 먹으면 `claude --version`부터 확인하자.
