---
title: "🎼 AI로 음악을 듣고 악보로! - 피아노부터 협주까지 자동 전사 솔루션"
excerpt_separator: "<!--more-->"
categories:
  - etc
tags:
  - [piano, midi, machine-learning, music-ai]

toc : true
toc_sticky : true
---

# 🎼 AI로 음악을 듣고 악보로! - 피아노부터 협주까지 자동 전사 솔루션

인터넷에서 재생되는 음악을 AI가 자동으로 듣고, 이를 악보로 변환해주는 솔루션을 만들고자 합니다.  
초기에는 **피아노 음악**을 중심으로, 이후에는 **여러 악기 협주** 형태로 확장할 예정입니다.

---

## 🎹 1. 피아노 전사 (Solo Piano Transcription)

### ✅ bytedance/piano_transcription
MAESTRO V2 기반 고정밀 피아노 전사 모델
- **GitHub**: [https://github.com/bytedance/piano_transcription](https://github.com/bytedance/piano_transcription)

```
pip install piano_transcription_inference
```

### ✅ azuwis/pianotrans
GUI 기반 피아노 전사 도구
- **GitHub**: [https://github.com/azuwis/pianotrans](https://github.com/azuwis/pianotrans)

### ✅ Hugging Face Space
웹상에서 오디오 파일을 업로드하고 MIDI로 변환
- **Demo**: [https://huggingface.co/spaces/asigalov61/ByteDance-Solo-Piano-Audio-to-MIDI-Transcription](https://huggingface.co/spaces/asigalov61/ByteDance-Solo-Piano-Audio-to-MIDI-Transcription)

---

## 🎼 2. 협주곡/멀티 악기 전사 (Multi-instrument Transcription)

### ✅ Magenta MT3
T5 기반 멀티 악기 전사 모델
- **GitHub**: [https://github.com/magenta/mt3](https://github.com/magenta/mt3)
- **논문**: [https://arxiv.org/abs/2111.03017](https://arxiv.org/abs/2111.03017)

---

## 🧠 3. 최신 연구 기반 전사 모델

### ✅ Transkun
Transformer + semi-CRF 기반 고정밀 피아노 전사
- **GitHub**: [https://github.com/Yujia-Yan/Transkun](https://github.com/Yujia-Yan/Transkun)

### ✅ Omnizart
다양한 악기(피아노, 보컬, 드럼 등)를 지원하는 전사 도구
- **GitHub**: [https://github.com/Music-and-Culture-Technology-Lab/omnizart](https://github.com/Music-and-Culture-Technology-Lab/omnizart)
- **Docs**: [https://music-and-culture-technology-lab.github.io/omnizart-doc](https://music-and-culture-technology-lab.github.io/omnizart-doc)

---

## 🛠️ MIDI → 악보 변환 도구

- **MuseScore**: [https://musescore.org](https://musescore.org)
- **TuxGuitar**: [http://www.tuxguitar.com.ar](http://www.tuxguitar.com.ar)
- **Flat.io**: [https://flat.io](https://flat.io)

---

## ✅ 전체 워크플로우

```
[사용자 오디오 입력]
        ↓
[AI 전사 모델] → ByteDance / MT3 / Omnizart
        ↓
[MIDI 출력]
        ↓
[MuseScore로 악보 변환]
        ↓
[PDF / MusicXML / 웹 뷰어 제공]
```

---

## 📌 모델 비교 요약

| 모델명 | 대상 악기 | 특징 | GitHub 링크 |
|--------|-----------|------|-------------|
| piano_transcription | 피아노 단일 | 고정밀 전사 | [링크](https://github.com/bytedance/piano_transcription) |
| pianotrans (GUI) | 피아노 단일 | GUI 실행 | [링크](https://github.com/azuwis/pianotrans) |
| Magenta MT3 | 멀티 악기 | 협주곡, 다중 트랙 전사 | [링크](https://github.com/magenta/mt3) |
| Transkun | 피아노 | 최신 연구 기반 | [링크](https://github.com/Yujia-Yan/Transkun) |
| Omnizart | 피아노/보컬/드럼 등 | 다양한 악기 지원 | [링크](https://github.com/Music-and-Culture-Technology-Lab/omnizart) |

---

## 🎯 향후 개발 계획

- 피아노 오디오 → 악보 자동 생성 MVP 구현
- 악보 품질 평가 및 PDF 변환 파이프라인 정리
- 멀티 악기 협주곡 자동 전사 확장
- 웹 기반 업로드/다운로드 시스템 구축

---

출처

