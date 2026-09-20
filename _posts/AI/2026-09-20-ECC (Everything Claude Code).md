---
title: "ECC (Everything Claude Code) — 설치부터 실전 사용까지"
excerpt_separator: "<!--more-->"
date: 2026-09-20
categories:
  - AI
tags:
  - [claude-code, ecc, agent-harness, tdd, ai-tooling]

toc : true
toc_sticky : true
---


## ECC가 뭔가

Claude Code에 **코디네이트된 엔지니어링 시스템**을 설치하는 프로젝트다.
한 줄로 표현하면 이 파이프라인이다.

```
plan -> test -> implement -> review -> verify -> remember -> improve
```

핵심 발상은 이거다. 매번 프롬프트에 "계획부터 세우고, 테스트 먼저 쓰고,
다 하면 리뷰해줘"를 다시 쓰는 대신, **그 절차를 한 번 설치해서 에이전트가
일하는 방식 자체로 만든다.**

MIT 라이선스 오픈소스다. Claude Code에서 가장 잘 돌아가고, Codex는 지원되는
동기화 경로가 있으며, Cursor·OpenCode·Gemini·Zed·Copilot·Antigravity·Qwen
등에는 기능이 제한된 어댑터를 제공한다. 이 글은 2026년 9월 기준 v2.2.2다.

| 구성 요소 | 개수 | 하는 일 |
|---|---|---|
| Agents | 68 | 계획, 리뷰, 빌드 복구, 보안, 아키텍처, 도메인 작업 |
| Skills | 292 | TDD, 리서치, 보안, 문서, 프론트엔드, 데이터, ML, 운영 |
| Commands | 94 | 스킬 우선 구조로 옮겨가는 중의 편의 진입점 |
| Hooks / Memory | 런타임 | 강제, 세션 요약, 지속 학습, 컨텍스트 제어 |
| Rules | 선택 | 언어·프로젝트별로 직접 고르는 상시 로드 표준 |
| AgentShield | 포함 | 프롬프트, 훅, MCP 설정, 권한, 시크릿 스캔 |

---

## 먼저: 이름이 세 개다

ECC는 공개 식별자가 세 개고 **서로 바꿔 쓸 수 없다.** 이걸 모르면 설치에서
막힌다.

| 용도 | 식별자 |
|---|---|
| GitHub 저장소 | `affaan-m/ECC` |
| Claude 마켓플레이스/플러그인 | `ecc@ecc` |
| npm 패키지 | `ecc-universal` |

의도된 설계다. Anthropic 마켓플레이스 설치는 표준 플러그인 식별자를 키로 쓰기
때문에, 도구 이름과 슬래시 명령 네임스페이스를 짧게 유지하려고 `ecc@ecc`를
쓴다. npm은 별개로 `ecc-universal`에 머물렀다.

**옛날 글에 나오는 `everything-claude-code@everything-claude-code`는 이제
동작하지 않는다.** 레거시 별칭으로만 취급해야 한다.

그리고 하나 더. `npx ecc-install`은 쓰면 안 된다. `ecc-install`은
`ecc-universal` 안의 바이너리 이름이지 별도로 퍼블리시된 npm 패키지가 아니다.

### ⚠️ 공식 채널

README에 WARNING으로 박혀 있는 내용이다. 서드파티 재업로드와 비공식 미러는
프로젝트가 관리하거나 검토하지 않으며 **악성코드가 있을 수 있다.** 공식
채널은 이것뿐이다.

- GitHub: `github.com/affaan-m/ECC`
- npm: `ecc-universal`, `ecc-agentshield`
- GitHub App: `github.com/apps/ecc-tools`
- 플러그인 슬러그: `ecc@ecc`
- 웹사이트: `ecc.tools`

검색하면 `mrebrahim/everything-claude-code` 같은 미러에서 clone하라는 안내가
나온다. 정확히 저 경고가 가리키는 대상이다.

---

## 설치

### 사전 요구사항

- Node.js 18 이상
- Claude 플러그인 설치는 Git + Claude Code 2.1 이상이 `PATH`에 있어야 함

### 🚨 경로를 하나만 고를 것

이게 ECC 설치에서 제일 중요한 규칙이다. **같은 하네스에 두 번 설치하면 스킬,
명령어, 훅, 설정이 중복된다.**

| 조합 | 가능? |
|---|---|
| Claude Code 플러그인 + Codex 네이티브 플러그인 | ✅ 다른 하네스라 괜찮음 |
| Claude Code 플러그인 + 레거시 Codex 동기화 | ✅ |
| Claude Code 플러그인 + Claude 수동 풀 설치 | ❌ 중복 |
| Codex 동기화 + Codex 마켓플레이스 플러그인 | ❌ 중복 |

여러 하네스에 각각 한 번씩 까는 건 문제없다. **한 하네스에 두 번**이 문제다.

### 방법 1 — 가이드 설치 (권장)

```bash
npx ecc-universal@2.2.2 setup
```

마법사가 공식 마켓플레이스와 기존 Claude 설치 스코프를 전부 점검한 뒤,
`ecc@ecc`를 원하는 스코프로 설치하거나 업데이트하거나 안전하게 옮긴다.
업데이트, 스코프 변경, 훅 프로파일 변경도 같은 명령을 다시 실행하면 된다.

다른 패키지 러너도 지원한다.

| 러너 | 명령 |
|---|---|
| npm / npx | `npx ecc-universal@2.2.2 setup` |
| pnpm | `pnpm dlx ecc-universal@2.2.2 setup` |
| Yarn 2+ | `yarn dlx ecc-universal@2.2.2 setup` |
| Bun | `bunx ecc-universal@2.2.2 setup` |

Yarn Classic 1에는 `yarn dlx`가 없으니 `npx`를 쓰자.

버전 에러가 나면 레지스트리 버전을 먼저 확인한다.

```bash
npm view ecc-universal version
```

> 프로젝트 자체가 덧붙인 주의사항이 정직해서 그대로 옮긴다. **버전 핀은
> 보안 감사도 무결성 검사도 아니다.** 패키지 코드를 실행하기 전에 릴리스
> 소스와 레지스트리 무결성을 검토하라고 명시돼 있다.

여러 하네스를 한 번에 설정하려면 멀티 하네스 마법사를 쓴다.

```bash
npx ecc-universal@2.2.2 install --guided
```

Claude Code, Codex, Kimi Code를 조합해 고를 수 있고, 각 설치 채널과 목적지를
보여준 뒤 첫 쓰기 전에 프리플라이트를 돌리고 최종 확인을 한 번 받는다.
**어느 마법사도 감지된 모든 하네스에 조용히 설치하지 않는다.**

### 방법 2 — 네이티브 플러그인 명령

Claude Code 안에서:

```
/plugin marketplace add https://github.com/affaan-m/ECC
/plugin install ecc@ecc
```

스킬, 에이전트, 명령어, 플러그인 관리 훅이 설치된다. **이 경로를 골랐으면
거기서 멈춰야 한다.** 수동 설치를 위에 얹지 말자.

설치 충돌이나 스코프 충돌 에러가 나면 Claude Code의 내장 파서가 낸 에러라
ECC가 가로챌 수 없다. 방법 1의 가이드 설치를 쓰거나 충돌하는 스코프를
먼저 정리하고 재시도해야 한다.

`settings.json`으로 선언적으로 넣는 것도 가능하다.

```json
{
  "extraKnownMarketplaces": {
    "ecc": {
      "source": { "source": "github", "repo": "affaan-m/ECC" }
    }
  },
  "enabledPlugins": { "ecc@ecc": true }
}
```

### rules는 어느 경로든 수동이다

**Claude Code 플러그인은 rules를 배포할 수 없다.** 그래서 원하는 rule 팩만
직접 복사해야 한다.

```bash
git clone https://github.com/affaan-m/ECC.git
cd ECC
mkdir -p ~/.claude/rules/ecc
cp -R rules/common ~/.claude/rules/ecc/
cp -R rules/typescript ~/.claude/rules/ecc/   # 본인 스택으로 교체
```

`rules/common` 하나에 실제로 쓰는 언어 팩 하나를 더하는 선에서 시작하자.
**rules는 상시 로드되는 컨텍스트**라서 많이 깔수록 매 세션이 무거워진다.

사용 가능한 팩: `common`, `typescript`, `python`, `golang`, `swift`, `php`,
`arkts`.

전역이 아니라 특정 저장소에만 적용하려면 프로젝트 안에 넣는다.

```bash
cd your-project
mkdir -p .claude/rules/ecc
cp -R /path/to/ECC/rules/common .claude/rules/ecc/
```

> 파일이 아니라 **언어 디렉터리 통째로** 복사해야 상대 참조가 유지되고
> 파일명 충돌이 안 난다.

---

## 컨텍스트 관리 — 이 부분을 꼭 읽자

ECC의 가장 큰 트레이드오프다. README의 플랫폼 지원표에 이렇게 적혀 있다.

> 플러그인은 설치된 카탈로그를 모델에게 알린다. 컨텍스트 사용량이 중요하다면
> 선택적/수동 프로파일을 쓸 것.

스킬 292개를 다 깔면 그 설명들이 컨텍스트를 상시 점유한다. "많이 깔수록
좋다"가 아니다. 프로파일을 골라야 한다.

### 저컨텍스트 설치 (훅 런타임 없음)

rules, 에이전트, 명령어, 플랫폼 설정, 핵심 워크플로우만 가져오고 런타임 훅은
빼는 경로다.

```bash
npx ecc-universal@2.2.2 install --profile minimal --target claude
```

소스 체크아웃에서는:

```bash
./install.sh --profile minimal --target claude   # macOS / Linux
.\install.ps1 --profile minimal --target claude  # Windows
```

훅만 나중에 추가할 수도 있다.

```bash
./install.sh --target claude --modules hooks-runtime --enable-hooks
```

> 훅 런타임이 생기는 설치는 **명시적 결정을 요구한다.** `--enable-hooks`나
> `--no-hooks` 없이 실행하면 설치기가 훅이 뭘 할 수 있는지 출력하고
> 아무것도 쓰지 않은 채 멈춘다. 좋은 설계다.

### 필요한 것만 고르기

뭘 깔아야 할지 모르겠으면 패키지에 들어 있는 어드바이저에게 물어본다.

```bash
node scripts/ecc.js consult "security reviews" --target claude
```

매칭되는 컴포넌트, 관련 프로파일, 미리보기/설치 명령을 돌려준다.

개별 스킬만 지정할 수도 있다.

```bash
./install.sh --target claude --skills tdd-workflow,security-review
```

설치 후에는 `/skills`에서 `t`를 눌러 토큰 기준으로 정렬해보자. 뭐가 무거운지
바로 보인다.

---

## 쓰기

### 처음 열 분

전체 카탈로그가 아니라 **지금 필요한 워크플로우 하나**부터 시작하는 게 맞다.

| 지금 하려는 일 | 시작점 |
|---|---|
| 기능 만들기 | `/ecc:plan "기능 설명"` → `tdd-workflow` |
| 버그 수정 | 실패하는 테스트로 재현 → `tdd-workflow` |
| 코드 리뷰 | `/code-review` (새 컨텍스트 리뷰) |
| 빌드 복구 | `/build-fix` |
| 코드베이스 정리 | `/refactor-clean` |
| 컨텍스트 압박 확인 | `/context-budget` |
| 긴 세션 끝낼 때 | `/save-session` 또는 `/learn-eval` |
| 나중에 이어서 | `/resume-session` |
| 에이전트 설정 감사 | `/security-scan` 또는 `agentshield scan --path .` |

### 명령 형태가 두 가지다

플러그인 설치는 네임스페이스를 쓴다.

```
/ecc:plan "Add authentication"
```

수동 설치는 짧은 호환 형태가 노출될 수 있다.

```
/plan "Add authentication"
```

뭐가 깔렸는지는 이걸로 확인한다.

```
/plugin list ecc@ecc
```

### 실제 흐름

**새 기능:**

```
/ecc:plan "OAuth 기반 사용자 인증 추가"
   -> planner 가 구현 청사진을 만든다
tdd-workflow 스킬
   -> tdd-guide 가 테스트 먼저 쓰도록 강제한다
/code-review
   -> code-reviewer 가 새 컨텍스트로 검토한다
```

여기서 나오는 결과물은 코드만이 아니다. **계획, 실패한 테스트, 통과한 테스트,
리뷰 지적사항, 최종 검증**까지 증거의 흔적이 남는다.

**프로덕션 준비:**

```
/security-scan        -> security-reviewer: OWASP Top 10 감사
e2e-testing 스킬      -> e2e-runner: 핵심 사용자 플로우 테스트
/test-coverage        -> 커버리지 80% 이상 확인
```

### 스킬이 주 표면이다

ECC는 명령어에서 스킬 중심으로 옮겨가는 중이다. `commands/`는 마이그레이션
기간의 호환 진입점이고, `/tdd`나 `/eval` 같은 은퇴한 짧은 이름 셤은
`legacy-command-shims/`에 들어가 있어서 명시적으로 옵트인해야 쓸 수 있다.

새로 배운다면 스킬 이름(`tdd-workflow`, `e2e-testing`, `search-first`)을
익히는 게 낫다.

---

## 네 가지 개념 구분하기

ECC가 이것들의 역할을 분리한 게 설계의 핵심이다. 컨텍스트 거동이 다르기
때문이다.

| 개념 | 하는 일 | 컨텍스트 거동 |
|---|---|---|
| **Skills** | TDD, 보안 리뷰 같은 재사용 워크플로우 | 필요할 때 로드 |
| **Agents** | 자기 컨텍스트와 도구 권한을 가진 범위 제한 작업자 | 계획·구현·리뷰를 격리 |
| **Rules** | 프로젝트·언어 표준 | **항상 로드. 그래서 선별 설치** |
| **Hooks** | 하네스 이벤트로 발동하는 스크립트 | 모델 컨텍스트 밖에서 실행 |
| **Instincts** | 실제 세션에서 학습한 패턴 + 신뢰도 점수 | 관련될 때 회수 |

훅이 모델 컨텍스트 밖에서 돈다는 게 중요하다. "조심해서 해줘"라고 모델에게
부탁하는 대신 결정적인 검사를 붙일 수 있다.

---

## 알아둘 함정들

### MCP는 자동으로 안 켜진다

플러그인 설치는 ECC의 번들 MCP 서버 정의를 **의도적으로 자동 활성화하지
않는다.** 엄격한 서드파티 게이트웨이에서 플러그인 MCP 도구 이름이 너무 길어지는
문제를 피하기 위해서다.

필요하면 Claude Code의 `/mcp`를 쓰거나, 저장소 로컬이면
`mcp-configs/mcp-servers.json`에서 원하는 정의를 프로젝트 `.mcp.json`으로
복사한다.

참고로 ECC가 기본 커넥터로 제공하는 건 `chrome-devtools` 하나뿐이다. 2026년
6월 감사에서 기존 여섯 개를 은퇴시켰다.

### 훅을 직접 복사하지 말 것

저장소의 `hooks/hooks.json`을 `~/.claude/settings.json`에 그대로 복사하면 안
된다. 그 파일은 플러그인/저장소용이라 훅 명령 경로가 다시 쓰여야 한다.

```bash
bash ./install.sh --target claude --modules hooks-runtime --enable-hooks
```

**플러그인으로 설치했다면 훅을 `settings.json`에 복사하지 말자.** Claude Code
2.1 이상은 플러그인의 `hooks/hooks.json`을 이미 자동 로드한다. 중복하면
중복 실행과 크로스플랫폼 훅 충돌이 난다.

### `multi-*` 명령은 별도 런타임이 필요하다

`/multi-plan`, `/multi-execute`, `/multi-backend`, `/multi-frontend`,
`/multi-workflow`는 기본 설치에 **포함되지 않는다.** `ccg-workflow` 런타임을
따로 설치해야 하고, ECC는 CCG를 번들하지도, 호환·감사된 릴리스를 보증하지도
않는다고 명시한다. 안 깔고 쓰면 그냥 동작하지 않는다.

### 스킬 디렉터리를 중첩하지 말 것

수동 설치 시 Claude는 `~/.claude/skills/`의 **직계 자식**에서 스킬을 찾는다.
`~/.claude/skills/ecc/` 아래에 넣으면 안 된다.

### Windows 네이티브는 제약이 있다

핵심 Node.js CLI와 설치기는 Windows·macOS·Linux 모두 돌지만 선택 기능은
동등하지 않다.

| 플랫폼 | 상태 |
|---|---|
| Linux | 코어 지원. 선택 기능이 Bash/Python을 요구할 수 있음 |
| macOS | 코어 지원. 독립 GAN 셸 경로가 시스템 Bash 3.2와 비호환 |
| Windows + WSL | 코어 지원. Linux 경로를 따름 |
| Windows 네이티브 | 제한적 지원. 지속 학습 v2 옵저버 데몬과 메모리 볼트 쓰기에 미해결 결함 |

Windows라면 WSL을 쓰는 게 마음 편하다.

---

## 문제가 생겼을 때

### 상태 점검과 복구

```bash
npx ecc-universal@2.2.2 list-installed
npx ecc-universal@2.2.2 doctor
npx ecc-universal@2.2.2 repair
```

로컬 Claude 설정이 날아갔다고 해서 뭘 다시 사야 하는 게 아니다. 위 세 개를
먼저 돌려보면 대부분 ECC 관리 파일이 복구된다.

### 제거

```bash
npx ecc-universal@2.2.2 uninstall --dry-run
npx ecc-universal@2.2.2 uninstall
```

ECC는 자기 설치 상태에 기록된 파일만 제거한다. 하네스 디렉터리의 무관한
파일을 건드리지 않는다. 수동으로 복사한 rules 폴더는 직접 지워야 한다.

### 중복 설치를 이미 했다면

순서대로 정리한다.

1. Claude Code 플러그인 설치 제거
2. 관리 설치 상태가 있는 프로젝트 디렉터리에서 ECC uninstall 실행
3. 수동으로 복사한 rule 폴더 중 불필요한 것 삭제
4. **하나의 경로만 골라** 다시 설치

---

## 정리

- 식별자 세 개가 다르다: 저장소 `affaan-m/ECC`, 플러그인 `ecc@ecc`,
  npm `ecc-universal`
- 권장 설치는 `npx ecc-universal@2.2.2 setup`. 대안은 네이티브 `/plugin` 명령
- **한 하네스에 한 경로만.** 섞으면 중복된다
- rules는 어느 경로든 수동 복사. 상시 로드니까 `common` + 언어 하나로 시작
- 스킬 292개를 다 깔 필요 없다. 컨텍스트가 걱정되면 `--profile minimal`
- 전체 카탈로그가 아니라 `/ecc:plan` 하나부터 시작하자

개인적으로 가장 인상적인 지점은 **코드를 짠 컨텍스트가 아니라 새 컨텍스트가
리뷰한다**는 설계다. 같은 대화에서 "이거 리뷰해줘"라고 하면 자기가 방금
내린 판단을 그대로 방어하는 경우가 많은데, 그 구조적 문제를 건드린다.

반대로 규모가 부담이다. 68 에이전트 × 292 스킬은 작은 프로젝트에 과하다.
프로파일을 좁혀서 쓰거나, 필요한 워크플로우 몇 개만 가져다 쓰는 쪽을 권한다.

### 참고

- 저장소: <https://github.com/affaan-m/ECC>
- 한국어 README: `docs/ko-KR/README.md`
- 입문 가이드: `the-shortform-guide.md` — **이걸 먼저 읽자**
- 심화: `the-longform-guide.md` (컨텍스트 경제학, 메모리, 평가, 병렬 에이전트)
- 보안: `the-security-guide.md` (프롬프트 인젝션, 훅, MCP, AgentShield)
- 명령어 요약: `COMMANDS-QUICK-REF.md`

> 이 글은 2026년 9월, ECC 2.2.2 기준이다. 단일 메인테이너가 주 단위로
> 릴리스하는 프로젝트라 명령어와 개수가 자주 바뀐다. 안 먹으면 저장소 README를
> 먼저 확인하자.
