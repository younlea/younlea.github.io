---
title: "Claude Pro 한도 다 썼을 때 — OmniRoute로 무료 모델 이어 쓰기"
excerpt_separator: "<!--more-->"
date: 2026-09-20
categories:
  - AI
tags:
  - [omniroute, claude-code, ai-gateway, docker, llm]
toc : true
toc_sticky : true
---

## 먼저, Pro / Max 구독자라면 알아야 할 것

결론부터 적는다.

**OmniRoute는 Claude Pro·Max 구독과 합쳐지지 않는다.** 구독 한도를 늘려주는
물건이 아니고, 구독을 더 싸게 쓰게 해주는 물건도 아니다. Claude Code를
OmniRoute에 연결하는 순간, **그 터미널은 당신의 Pro 구독을 전혀 쓰지 않는다.**

이유는 인증 경로 때문이다. Anthropic 지원 문서에 명시돼 있는데, 시스템에
`ANTHROPIC_API_KEY` 환경변수가 설정돼 있으면 Claude Code는 구독(Pro, Max,
Team, Enterprise) 대신 그 키로 인증하고, 구독에 포함된 사용량이 아니라 API
사용료가 청구된다. OmniRoute의 런처도 똑같이 `ANTHROPIC_BASE_URL`과
`ANTHROPIC_AUTH_TOKEN`을 주입해서 `claude`를 띄운다. 즉 Claude Code는
Anthropic이 아니라 로컬 게이트웨이를 바라보게 되고, 구독은 그 세션에서 그냥
논다.

반대로 구독 자격증명을 OmniRoute에 물리는 것도 의미가 없다. 구독으로
인증하는 서드파티 앱은 **여전히 구독의 사용량 한도에서 차감된다.** 프록시를
낀다고 없던 용량이 생기지 않는다. 얻는 건 없고 약관 리스크만 남는다.

### 그럼 이 글은 뭘 위한 글인가

Pro는 claude.ai, 데스크톱 앱, Claude Code가 **하나의 사용량 풀을 공유**한다.
오전에 채팅을 길게 하면 오후 코딩 세션에서 쓸 게 줄어든다. 5시간 세션 한도와
주간 한도가 따로 있고, 주간 한도는 계정마다 지정된 시각에 리셋된다.

그래서 이런 순간이 온다. **한도를 다 썼는데 지금 당장 마저 해야 하는 작업이
남았다.** 선택지는 대충 이렇다.

1. 리셋까지 기다린다
2. Usage credits를 켠다 — Pro·Max 5x·Max 20x는 포함 한도를 넘긴 뒤에도 종량제로
   계속 쓸 수 있고, 지출 상한도 걸 수 있다. 가장 단순한 답이다
3. Max로 업그레이드한다 — 꾸준히 한도를 친다면 이게 정답이다
4. **다른 모델로 갈아타서 작업을 이어간다**

이 글은 4번에 대한 가이드다. OmniRoute라는 로컬 게이트웨이를 하나 띄워두고,
Pro가 막히면 Kimi·GLM·DeepSeek 같은 모델의 무료 티어로 넘어가서 하던 일을
마저 하는 방법이다.

정리하면 **OmniRoute는 Pro의 대체재가 아니라 비상 차선이다.** 한도가 남아
있을 때는 그냥 Pro를 쓰는 게 품질도 좋고 이미 돈도 낸 거다.

---

## OmniRoute가 뭔가

여러 LLM 제공자를 **하나의 로컬 엔드포인트 뒤에 숨겨주는 게이트웨이**다.

Claude Code든 Codex든 Cursor든 `http://localhost:20128` 하나만 바라보게
해두면, 그 뒤에서 OmniRoute가 적당한 제공자로 요청을 보낸다. 쿼터가
떨어지거나 제공자가 죽으면 다음 대상으로 자동으로 넘어간다.

MIT 라이선스, 이 글 기준 v3.8.51. 제공자 352개가 등록돼 있고 그중 152개가
무료 티어 메타데이터를 달고 있다.

> **저장소 주의.** GitHub에 OmniRoute라는 이름의 저장소가 여러 개 있는데
> 대부분 포크다. 원본은 `diegosouzapw/OmniRoute`, Docker 이미지는
> `diegosouzapw/omniroute`다. `omniroute/gateway` 같은 이미지는 존재하지
> 않는다 — 나는 이걸로 30분을 날렸다. API 키를 다루는 물건이니 출처를 꼭
> 확인하자.

---

## 설치

### 방법 1: npm

```bash
npm install -g omniroute
```

설치하면 서버가 `localhost:20128`에서 뜬다. CLI와 웹 대시보드가 같은
프로세스에서 한 포트로 서비스된다.

업데이트는 `omniroute update`를 쓰자. 내부적으로 `--include=optional`이 항상
붙어서 돌기 때문에, npm 설정에 `omit=optional`이 있어도 네이티브 SQLite
드라이버나 OS 키링 바인딩이 조용히 빠지는 사고를 막아준다.

```bash
omniroute update --dry-run   # 뭘 실행할지 미리보기
omniroute update --check     # 구버전이면 exit 1
```

### 방법 2: Docker

```bash
docker run -d --name omniroute --restart unless-stopped \
  -p 127.0.0.1:20128:20128 \
  -v omniroute-data:/app/data \
  diegosouzapw/omniroute:latest
```

`-p` 앞의 `127.0.0.1`을 빼지 말자. 빼면 LAN 전체에 열린다.

**Claude Code를 물릴 거라면 메모리를 키워야 한다.** 이미지 기본값이
`OMNIROUTE_MEMORY_MB=1024`인데 이건 대시보드와 가벼운 채팅 수준이다. 긴
컨텍스트 두 개가 겹치면 V8 힙이 터진다.

```bash
docker run -d --name omniroute --restart unless-stopped --stop-timeout 40 \
  -e OMNIROUTE_MEMORY_MB=8192 --memory=10g \
  -p 127.0.0.1:20128:20128 -v omniroute-data:/app/data \
  diegosouzapw/omniroute:latest
```

컨테이너 메모리 한도(`--memory`)를 힙(`OMNIROUTE_MEMORY_MB`)보다 넉넉히 크게
잡는 게 포인트다. 네이티브 버퍼가 V8 바깥에 있어서 그렇다.

### 동작 확인

설치 직후, **키를 하나도 등록하지 않은 상태에서** 바로 이게 된다.

```bash
curl http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"auto","messages":[{"role":"user","content":"ping"}]}'
```

키 없이 쓸 수 있는 제공자가 `auto` 콤보에 미리 물려 있어서 가입도 설정도 없이
응답이 온다. 브라우저로 `http://localhost:20128`을 열면 대시보드가 나온다.

---

## 무료로 쓸 수 있는 모델 붙이기

여기가 본론이다. 다만 오해하기 쉬운 지점부터 정리하자.

**"무료"에는 두 단계가 있다.**

- **0단계 — 키 없이 바로**: 위 curl이 그거다. 키리스 제공자가 기본으로 물려
  있어서 설치하자마자 동작한다. 대신 선택지가 좁다
- **1단계 — 각 제공자 무료 티어**: Kimi, GLM, DeepSeek 같은 모델을 제대로
  쓰려면 **해당 제공자에 가입해서 무료 티어 키를 발급받아** OmniRoute에
  등록해야 한다. OmniRoute가 키를 대신 만들어주지는 않는다

OmniRoute가 하는 일은 "흩어진 무료 티어들을 한 군데 모아서 자동으로 돌려
쓰게 해주는 것"이다. 무료 티어를 손으로 쌓으려면 SDK도 여러 개, 레이트
리밋도 제각각이고, 지금 얼마 남았는지도 모른다. 그걸 카탈로그로 관리해준다.

### 제공자 등록

```bash
# 키를 환경변수로 넘기는 게 안전하다 (쉘 히스토리에 안 남음)
export GLM_API_KEY=발급받은키
omniroute providers add glm --credential-env GLM_API_KEY --name main

# OAuth를 쓰는 제공자는 이쪽
omniroute providers auth openai

# 기본 모델 지정
omniroute providers edit <connection-id> --default-model glm/glm-5.2

# 제거
omniroute providers remove <connection-id> --yes
```

스크립트에서는 `--credential-env`나 `--credential-stdin`을 쓰자.
`--credential`로 값을 직접 넘기면 히스토리에 남는다.

### 쓸 수 있는 모델 확인

제공자 목록과 모델 ID는 릴리스마다 바뀌니 직접 조회하는 게 정확하다.

```bash
curl -s http://localhost:20128/v1/models | jq -r '.data[].id'
```

무료 티어 잔량은 대시보드의 `/dashboard/free-tiers`에서 실시간으로 볼 수 있다.

### 모델 하나씩 고르지 말고 `auto`를 쓰자

어떤 모델이 지금 여유가 있는지 일일이 신경 쓰는 건 피곤하다. `auto`를 주면
연결된 제공자들로 가상 콤보를 만들어서 알아서 고른다. 목적별 변종이 있다.

| 모델 ID | 최적화 대상 |
|---|---|
| `auto` | 균형 잡힌 기본값. 마지막으로 잘 됐던 제공자에 붙어 있음 |
| `auto/coding` | 코드 생성 품질 우선 |
| `auto/fast` | 지연시간 최소 |
| `auto/cheap` | 토큰당 단가 최소 |
| `auto/offline` | **쿼터 여유가 가장 많은 쪽** |
| `auto/smart` | 품질 우선 + 10% 탐색 |

한도에 쫓기는 상황이면 `auto/offline`이 제일 쓸모 있다.

직접 조립하고 싶으면 `priority`, `round-robin`, `cost-optimized`,
`cache-optimized`, `fusion`(여러 모델에 뿌리고 심판 모델이 종합),
`pipeline`(앞 단계 출력이 다음 단계 입력) 등 19가지 전략을 콤보 단계별로
섞을 수 있다.

### 하나 죽어도 안 멈춘다

복구 로직이 세 층으로 나뉘어 있다. 실패 하나가 전체를 무너뜨리지 않도록
범위를 좁혀둔 설계다.

- **서킷 브레이커** (제공자 단위): 408/5xx가 임계치를 넘으면 그 제공자 전체를
  잠시 차단하고 콤보가 다음 제공자로 우회
- **커넥션 쿨다운** (키/계정 단위): 문제 있는 키 하나만 쉬게 하고 형제 키들은
  계속 서비스. 429는 `Retry-After`를 존중한다
- **모델 락아웃** (모델 단위): 특정 모델의 429나 404는 그 모델만 잠근다.
  커넥션 전체를 죽이지 않는다

---

## Claude Code에 붙이기

세 가지 방법이 있다. 상황에 따라 고르면 된다.

### 방법 A: 설정 파일 없이 그때만 (추천)

Pro를 평소에 쓰다가 막혔을 때만 쓸 거라면 이게 제일 낫다. 설정 파일을 하나도
안 건드리고 환경변수만 주입해서 `claude`를 띄운다.

```bash
omniroute run claude --model glm/glm-5.2
```

모델을 `auto`로 맡기려면:

```bash
omniroute run claude --model auto/coding
```

`--`(대시 두 개) 뒤의 인자는 `claude` 바이너리에 그대로 전달된다.

```bash
omniroute run claude -- --print-system-prompt "review this diff"
```

실행 전에 뭐가 주입되는지 보려면 `--dry-run`을 붙이자. 설정을 안 건드리니까,
터미널만 닫으면 원래대로 Pro를 쓰게 된다. **되돌리는 작업이 없다는 게 이
방법의 핵심 장점이다.**

### 방법 B: 프로필 만들어두고 골라 쓰기

```bash
omniroute setup-claude
```

실행 중인 OmniRoute에서 모델 카탈로그를 읽어다가, 매칭되는 모델마다
`~/.claude/profiles/<name>/settings.json`을 하나씩 만든다. 그 다음:

```bash
omniroute launch --profile glm52
```

모델이 너무 많이 생기는 게 싫으면 필터를 걸자.

```bash
omniroute setup-claude --only glm,kimi
omniroute setup-claude --dry-run    # 뭐가 쓰일지 미리보기
```

### 방법 C: 인터랙티브로 고르기

```bash
omniroute configure claude
```

제공자와 모델을 대화형으로 고르고 Claude Code 설정을 써준다. 뭐가 있는지
모를 때 편하다.

### 함정: Claude Code는 `/v1`을 붙이지 않는다

수동 설정할 일이 있다면 이건 꼭 기억하자. OmniRoute는 OpenAI 호환
인터페이스를 `/v1`에, **Anthropic 호환 인터페이스를 루트에** 열어둔다.
Claude Code는 자기가 `/v1/messages`를 알아서 붙이므로 `ANTHROPIC_BASE_URL`에는
`/v1` 없이 루트를 줘야 한다.

| 도구 | base URL |
|---|---|
| **Claude Code** (`ANTHROPIC_BASE_URL`) | 루트 — `/v1` **없이** |
| Codex CLI | `/v1` 포함 |
| Cline, Goose, Aider | 루트 |
| Continue, Crush, Kilo, Cursor, Qwen | `/v1` 포함 |

여기서 틀리면 404가 나는데 원인 찾기가 은근 귀찮다.

### Pro로 돌아가기

방법 A를 썼다면 할 일이 없다. 그냥 평소처럼 `claude`를 치면 된다.

방법 B·C로 설정을 썼다면, 아래가 비어 있어야 구독을 정상적으로 쓴다.

```bash
echo $ANTHROPIC_API_KEY
echo $ANTHROPIC_BASE_URL
```

뭔가 출력된다면 `~/.bashrc`나 `~/.zshrc`를 확인하자. Claude Code 안에서는
`/status`로 현재 어떤 인증을 쓰는지 볼 수 있다.

> **이건 진짜 확인해보자.** 예전에 API 키를 환경변수에 박아둔 적이 있다면,
> Pro를 결제하고도 구독이 아니라 API 요금이 나가고 있을 수 있다.

---

## 덤: 토큰 압축과 라우팅 추적

RTK와 Caveman이라는 두 압축 엔진을 겹쳐서 쓴다. 문서상 적격 워크로드 기준
15~95% 절감이라고 한다. 도구 호출 출력처럼 장황한 텍스트가 많이 오갈 때
효과가 크다. 범위가 넓은 만큼 본인 작업에서 실제로 얼마나 줄어드는지는
대시보드에서 확인하는 게 낫다.

그리고 모든 응답에 `X-OmniRoute-Decision` 헤더가 붙어서 어떤 전략이 어떤
제공자를 골랐고 지연이 얼마였는지 알려준다. "왜 이 모델이 답했지?" 싶을 때
이게 없으면 답답하다.

---

## 주의사항

**무료 티어 숫자를 그대로 믿지 말자.** 프로젝트가 월 ~14.7억 토큰이라는 수치를
내걸고 있는데, 본인들도 2주마다 재감사하며 숫자가 **양방향으로 움직인다**고
명시해뒀다. 제공자가 무료 티어를 닫으면 내려간다.

**약관은 본인이 확인해야 한다.** 더 중요한 건 이거다. OmniRoute 자체 카탈로그에
약관상 위험하다고 표시된 제공자가 **13곳** 있다. 업무용 코드에 쓸 거면 각
제공자 약관을 직접 읽어보자. "무료니까 일단 다 켠다"는 좋은 태도가 아니다.

**고가용성이 안 된다.** 기본 구성은 Node 프로세스 하나 + SQLite writer 하나다.
같은 SQLite 파일에 여러 레플리카를 붙이면 **DB가 깨진다.** 업그레이드 때는
연결된 세션이 전부 끊긴다고 봐야 한다. 롤링 업데이트는 없다. 개인 로컬
라우터로는 괜찮지만 팀 공용 게이트웨이로 올릴 거면 이 제약부터 정리하자.

**Redis는 켜두는 게 좋다.** 분산 레이트 리미터와 공유 캐시를 Redis가 받친다.
끄면 인메모리 폴백으로 떨어지면서 리미터 품질이 나빠진다. 다만 사이드카
Redis가 `requirepass` 없이 돌기 때문에, 호스트 바인딩을 `0.0.0.0`으로 바꿀
거면 반드시 `--requirepass`를 추가하자. 안 그러면 LAN에 인증 없는 Redis를
여는 셈이다.

---

## 정리

- OmniRoute는 **Pro·Max 구독의 대체재가 아니다.** 붙이는 순간 구독은 안 쓰인다
- 구독을 게이트웨이에 물려도 **한도는 안 늘어난다.** 서드파티 앱도 구독 한도에서
  차감된다
- 쓸모 있는 건 **한도를 다 쓴 뒤의 차선책**으로서다. Kimi·GLM·DeepSeek 무료
  티어로 넘어가 하던 작업을 마저 끝내는 용도
- 한도 부족이 상시적이라면 usage credits나 Max 업그레이드가 더 단순하고 정직한 답이다
- 평소엔 `claude`, 막히면 `omniroute run claude --model auto/offline` — 이 조합을
  추천한다. 설정을 안 건드려서 되돌릴 게 없다

### 참고

- 저장소: <https://github.com/diegosouzapw/OmniRoute>
- 한국어 README: `docs/i18n/ko/README.md`
- Claude Code 상세 가이드: `docs/guides/CLAUDE-CODE-CONFIGURATION.md`
- [Claude Code를 Pro·Max 플랜에서 쓰기 (Anthropic 지원 문서)](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan)
- [Usage credits 관리](https://support.claude.com/en/articles/12429409-manage-usage-credits-for-paid-claude-plans)

> 이 글의 버전과 숫자는 2026년 9월 기준이다. 릴리스 주기가 짧으니 명령어가
> 안 먹으면 저장소의 최신 문서를 먼저 확인하자.
