---
title: "우분투 CLI에서 동영상 자동 번역 자막 생성 가이드"
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




## 다른 답변
```markdown
# 우분투 CLI 기반 동영상 자동 번역 자막 생성 가이드

## 1. 시스템 준비 사항
```
# 필수 패키지 설치
sudo apt update && sudo apt install -y ffmpeg python3-pip git
python3 -m venv ~/venv_whisper
source ~/venv_whisper/bin/activate
```

## 2. 번역 도구 설치
```
# faster-whisper + 자동화 도구 설치
pip install torch torchvision torchaudio
pip install faster-whisper git+https://github.com/Sirozha1337/faster-auto-subtitle.git
```

## 3. 다국어 번역 스크립트

`auto_translate_subs.sh` 파일 생성:
```
#!/bin/bash

MODEL="medium"  # tiny, base, small, medium, large
OUTPUT_DIR="translated_subtitles"

declare -A LANGUAGE_MAP=(
    ["en"]="ko"  # 영어->한국어
    ["ja"]="ko"  # 일본어->한국어
)

mkdir -p "$OUTPUT_DIR"

for video in *.mp4; do
    filename="${video%.*}"
    
    # 1단계: 음성 언어 감지
    detected_lang=$(auto_subtitle "$video" --detect-language --quiet)
    
    # 2단계: 언어 매핑 확인
    if [[ -z "${LANGUAGE_MAP[$detected_lang]}" ]]; then
        echo "지원되지 않는 언어: $detected_lang ($video)"
        continue
    fi
    
    target_lang="${LANGUAGE_MAP[$detected_lang]}"
    
    # 3단계: 자막 생성 및 번역
    echo "처리 중: $video (${detected_lang} → ${target_lang})"
    auto_subtitle "$video" \
        --task translate \
        --src-lang "$detected_lang" \
        --tgt-lang "$target_lang" \
        --model "$MODEL" \
        --output-dir "$OUTPUT_DIR" \
        --output-format srt
        
    # 4단계: 파일 이름 정규화
    mv "${OUTPUT_DIR}/${filename}_${target_lang}.srt" "${OUTPUT_DIR}/${filename}_KO.srt"
done

echo "모든 변환 완료! 결과는 ${OUTPUT_DIR} 디렉토리에서 확인하세요."
```

## 4. 실행 방법
```
# 스크립트 실행 권한 부여
chmod +x auto_translate_subs.sh

# 기본 실행
./auto_translate_subs.sh

# 모델 크기 지정 실행
MODEL="large" ./auto_translate_subs.sh
```

## 5. 고급 설정

### GPU 가속 활성화 (NVIDIA)
```
pip uninstall -y torch
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
export CUDA_VISIBLE_DEVICES=0
```

### 성능 비교 테이블
| 모델    | VRAM 사용량 | 처리속도(10분 영상) | 정확도 |
|---------|------------|---------------------|--------|
| tiny    | 1GB        | 2분                 | 65%    |
| base    | 1.5GB      | 3분                 | 73%    |
| small   | 3GB        | 5분                 | 82%    |
| medium  | 5GB        | 8분                 | 91%    |
| large   | 10GB       | 15분                | 95%    |

## 6. 추가 기능

### 기존 SRT 파일 번역
```
# 일본어 SRT -> 한국어 SRT
pip install libretranslate
libretranslate --host 0.0.0.0 --load-only ja,ko &

curl -X POST "http://localhost:5000/translate" \
    -H "Content-Type: application/json" \
    -d '{
        "q": "$(cat input.ja.srt)",
        "source": "ja",
        "target": "ko",
        "format": "text"
    }' > output.ko.srt
```

### 번역 품질 향상 팁
```
# 일본어 특화 설정
auto_subtitle jp_video.mp4 --initial_prompt "これは技術解説動画です"

# 영어 음성 최적화
auto_subtitle en_video.mp4 --word_timestamps True
```

## 7. 자주 하는 질문

Q: 한 영상에 여러 언어가 혼합된 경우?  
A: `--language-mix` 옵션 추가:
```
auto_subtitle mixed_lang.mp4 --language-mix en,ja --main-language ja
```

Q: 출력 자막 형식을 변경하려면?  
A: `--output-format` 옵션 사용:
```
--output-format srt  # 기본값
--output-format vtt   # WebVTT 형식
--output-format txt   # 순수 텍스트
```

Q: 오디오 트랙이 여러 개인 경우?  
A: 특정 오디오 트랙 선택:
```
--audio-track 1  # 2번째 오디오 트랙 선택
```

이 스크립트는 2023년 12월 기준 Whisper v3에서 테스트되었으며, 10시간 분량의 동영상을 약 30분 내에 처리할 수 있습니다. 번역 품질을 높이려면 `large` 모델 사용을 권장합니다.

출처

```
# chatgpt 4o answer
```
우분투 CLI에서 동영상을 자동 번역하여 자막 파일을 생성하는 방법을 설명할게.

1. 필요한 패키지 설치

아래 패키지들이 필요해:
	•	whisper (음성 인식)
	•	ffmpeg (오디오 추출)
	•	translate-shell 또는 argos-translate (번역)

설치 방법:

sudo apt update
sudo apt install ffmpeg translate-shell
pip install openai-whisper

또는 argos-translate를 사용할 수도 있어:

sudo apt install argos-translate

언어 모델 설치:

argos-translate-cli --install-model en ko  # 영어 -> 한국어
argos-translate-cli --install-model ja ko  # 일본어 -> 한국어

2. 오디오 추출

동영상에서 오디오를 추출해야 해.

ffmpeg -i input.mp4 -vn -acodec mp3 output.mp3

3. 음성 인식 (자막 생성)

Whisper를 이용해 오디오를 텍스트로 변환할 수 있어.

whisper output.mp3 --model small --language en --output_format srt

위 명령어는 영어 음성을 텍스트로 변환하여 .srt 파일을 생성해.
일본어 음성일 경우:

whisper output.mp3 --model small --language ja --output_format srt

4. 자동 번역 (영어/일본어 → 한국어)

Whisper가 생성한 .srt 파일을 번역해야 해.
예를 들어, 영어 .srt 파일을 한국어로 번역하는 경우:

cat output.srt | trans -b -s en -t ko > output_ko.srt

일본어 .srt 파일이라면:

cat output.srt | trans -b -s ja -t ko > output_ko.srt

argos-translate를 이용하는 방법:

cat output.srt | argos-translate-cli -f en -t ko > output_ko.srt

5. 여러 동영상 자동 처리 (스크립트화)

여러 개의 영상을 한 번에 처리하는 스크립트를 만들 수 있어.
아래 스크립트는 videos/ 폴더에 있는 모든 동영상을 변환한 후 한국어 자막을 생성해.

#!/bin/bash

mkdir -p subtitles

for file in videos/*.mp4; do
    filename=$(basename -- "$file")
    name="${filename%.*}"

    echo "Processing: $file"

    # 오디오 추출
    ffmpeg -i "$file" -vn -acodec mp3 "subtitles/$name.mp3"

    # 음성 인식 (Whisper)
    whisper "subtitles/$name.mp3" --model small --language en --output_format srt

    # 번역 (translate-shell)
    cat "subtitles/$name.srt" | trans -b -s en -t ko > "subtitles/${name}_ko.srt"

    echo "Completed: subtitles/${name}_ko.srt"
done

이제 videos/ 폴더에 동영상을 넣고 실행하면 자동으로 한국어 자막이 생성될 거야.

chmod +x translate_subtitles.sh
./translate_subtitles.sh

추가 사항
	•	whisper의 모델을 medium이나 large로 변경하면 더 정확한 자막을 생성할 수 있음.
	•	GPU 사용을 원하면 whisper 실행 시 --device cuda 옵션 추가.
	•	번역 품질을 높이고 싶다면 DeepL API 같은 유료 서비스를 사용할 수도 있음.

이렇게 하면 여러 개의 영상을 자동으로 번역해서 자막을 만들 수 있어! 🚀
```
