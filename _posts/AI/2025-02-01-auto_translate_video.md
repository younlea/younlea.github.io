---
title: "우분투 CLI에서 동영상 자동 번역 자막 생성 가이드""
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# OpenAI Whisper를 이용한 무료 자막 생성 & 번역

![Whisper Logo](https://openaicom.imgix.net/8d14e8b0-6655-4abb-9b4d-516f6b3d7a8e/whisper.png?auto=compress%2Cformat&fit=min&fm=jpg&q=80&w=1600)

## 1. 필수 패키지 설치
```
sudo apt update && sudo apt install -y ffmpeg python3-pip git
python3 -m venv venv
source venv/bin/activate
pip install faster-whisper git+https://github.com/Sirozha1337/faster-auto-subtitle.git
```

## 2. 기본 사용법

### 영어 -> 한국어 번역
```
auto_subtitle input.mp4 --task translate --tgt-lang ko --model medium
```

### 일본어 -> 한국어 번역 (2단계)
```
# 1단계: 일본어 음성 -> 일본어 텍스트
auto_subtitle japanese_video.mp4 --src-lang ja --output-format srt

# 2단계: 일본어 SRT -> 한국어 번역
pip install googletrans==4.0.0-rc1
python3 - <<EOF
from googletrans import Translator

translator = Translator()
with open('japanese_video.srt') as f:
    jp_text = f.read()

ko_text = translator.translate(jp_text, src='ja', dest='ko').text
with open('korean_translation.srt', 'w') as f:
    f.write(ko_text)
EOF
```

## 3. 일괄 처리 스크립트
`batch_translate.sh` 생성:
```
#!/bin/bash
MODEL="medium"
LANG_MAP=(
    ["en"]="ko"   # 영어->한국어
    ["ja"]="ko"   # 일본어->한국어
)

for video in *.mp4; do
    filename="${video%.*}"
    detected_lang=$(auto_subtitle "$video" --detect-language --quiet)
    tgt_lang=${LANG_MAP[$detected_lang]}
    
    echo "Processing: $video (${detected_lang}->${tgt_lang})"
    auto_subtitle "$video" \
        --task translate \
        --tgt-lang "$tgt_lang" \
        --model "$MODEL" \
        --output-dir translated_subs
done
```

## 4. 고급 기능

### GPU 가속 사용
```
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
auto_subtitle input.mp4 --device cuda
```

### 자막 병합
```
ffmpeg -i input.mp4 -i translated_subs/input_ko.srt \
    -c copy -c:s mov_text \
    -metadata:s:s:0 language=kor \
    output_with_sub.mp4
```

## 5. 주요 옵션 비교

| 옵션 | 값 | 설명 |
|------|----|------|
| --model | tiny/base/small/medium/large | 모델 크기 ↗️ 정확도 ↗️ |
| --task | transcribe/translate | 음성인식/번역 선택 |
| --tgt-lang | ko/ja/en 등 | 출력 언어 지정 |
| --beam-size | 5 | 번역 품질 조정 |

## 6. 트러블슈팅

### 한국어 인식률 향상
```
# 초기 프롬프트 추가
auto_subtitle korean_video.mp4 --initial_prompt "이 동영상은 한국어로 진행되는 기술 강연입니다"
```

### 모델 다운로드 경로
```
# 사용자 지정 모델 경로
auto_subtitle input.mp4 --model-dir ~/custom_models/
```

## 7. 대체 솔루션

### 기존 SRT 파일 번역
```
pip install libretranslate
libretranslate --host 0.0.0.0 --load-only en,ja,ko &
curl -X POST "http://localhost:5000/translate" \
    -H "Content-Type: application/json" \
    -d '{"q":"원본 텍스트","source":"ja","target":"ko"}' 
```

---

**참고자료**:  
- [Whisper 공식 문서](https://openai.com/research/whisper)  
- [faster-auto-subtitle GitHub](https://github.com/Sirozha1337/faster-auto-subtitle)  
- [구글 번역 API 문서](https://py-googletrans.readthedocs.io/en/latest/)  

{% include share.html %}

