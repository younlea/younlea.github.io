---
title: "AI 음원 압보 생성기 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

아래는 GitHub.io 블로그에 바로 사용할 수 있도록 정리한 Markdown 형식의 AI 악보 생성 솔루션 정리글입니다.

⸻


# 🎼 AI로 음악을 듣고 악보로! - 피아노부터 협주까지 자동 전사 솔루션

인터넷에서 재생되는 음악을 AI가 자동으로 듣고, 이를 악보로 변환해주는 솔루션을 만들고자 합니다.  
초기에는 **피아노 음악을 중심으로**, 이후에는 **여러 악기가 함께 연주되는 협주곡 형태로 확장**할 예정입니다.

이 글에서는 해당 프로젝트에 활용 가능한 오픈소스 모델들을 정리합니다.

---

## 🎹 1. 피아노 전사 (Solo Piano Transcription)

### ✅ [bytedance/piano_transcription](https://github.com/bytedance/piano_transcription)
- MAESTRO V2 데이터셋 기반의 고성능 피아노 전사 모델 (PyTorch).
- **오디오 → MIDI**로 정확하게 변환.
- onset/offset 단위의 정밀한 음 추출이 가능.

```bash
pip install piano_transcription_inference

✅ azuwis/pianotrans
	•	위 모델을 바탕으로 제작된 GUI 툴.
	•	윈도우/리눅스/맥에서 직접 음원을 불러와서 전사 가능.

✅ Hugging Face Space Demo
	•	웹에서 바로 사용 가능.
	•	오디오 업로드 → MIDI 다운로드 + 악보 시각화

⸻

🎼 2. 협주곡/멀티 악기 전사 (Multi-instrument Transcription)

✅ Magenta MT3 (Multi-Task Multitrack Music Transcription)
	•	Google Magenta에서 개발한 T5 기반의 멀티 악기 전사 모델.
	•	드럼, 피아노, 스트링 등 다양한 악기를 동시에 전사 가능.
	•	예: 협주곡, 밴드 연주, 드럼 트랙 등.

참고: 논문 링크

⸻

🧠 3. 최신 연구 기반 전사 모델

✅ Transkun
	•	Transformer + semi-CRF 기반의 고정밀 전사기.
	•	2024 ISMIR 논문 기반.
	•	피아노 곡의 표현력 높은 전사에 적합.

✅ Omnizart
	•	멀티 악기 + 보컬 + 드럼 + 코드 등 다양한 요소 전사 가능.
	•	GUI, CLI 모두 제공.

공식 문서: https://music-and-culture-technology-lab.github.io/omnizart-doc

⸻

🛠️ 4. 악보 출력 (MIDI → 악보)

전사된 MIDI를 시각적 악보(PDF 등)로 변환하려면:
	•	MuseScore
	•	TuxGuitar
	•	Flat.io

위와 같은 무료 악보 편집 프로그램을 활용할 수 있습니다.

⸻

✅ 전체 워크플로우 예시

[사용자 오디오 입력]
        ↓
[AI 전사 모델] → ByteDance / MT3 / Omnizart
        ↓
[MIDI 출력]
        ↓
[MuseScore로 악보 변환]
        ↓
[PDF / MusicXML / 웹 뷰어 제공]


⸻

📌 요약 비교표

모델명	대상 악기	특징	GitHub
piano_transcription	피아노 단일	고정밀 전사	링크
pianotrans (GUI)	피아노 단일	GUI 실행 가능	링크
Magenta MT3	멀티 악기	협주곡 가능	링크
Transkun	피아노	최신 연구 기반 모델	링크
Omnizart	피아노/드럼/보컬	가장 다양한 악기 지원	링크


⸻

🎯 앞으로의 계획
	•	피아노 오디오 → 악보 자동 생성 MVP 구현
	•	악보 품질 평가 및 PDF 변환 파이프라인 정리
	•	다중 악기 및 협주곡 지원 기능 추가
	•	웹 기반 업로드 + 다운로드 서비스 배포

⸻

👉 GitHub에 구현 중인 코드/서비스는 차후 공개 예정입니다.
관심 있으신 분들은 언제든지 피드백 주세요!

---

이 마크다운 파일은 GitHub Pages 기반 블로그(`username.github.io`)나 Jekyll, Hugo 등 마크다운 기반 정적 블로그에 바로 사용 가능합니다. 필요하시면 HTML 스타일 포함 버전도 따로 만들어드릴 수 있어요.
