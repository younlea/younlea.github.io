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

우분투 CLI에서 동영상을 자동 번역하여 자막 파일을 생성하는 방법을 설명할게.    

1. 필요한 패키지 설치    

아래 패키지들이 필요해    
	•	whisper (음성 인식)    
	•	ffmpeg (오디오 추출)    
	•	translate-shell 또는 argos-translate (번역)    

설치 방법:    

```bash
sudo apt update
sudo apt install ffmpeg translate-shell
pip install openai-whisper
```

2. 오디오 추출    

동영상에서 오디오를 추출해야 해.    
```
ffmpeg -i input.mp4 -vn -acodec mp3 output.mp3
```
3. 음성 인식 (자막 생성)    

Whisper를 이용해 오디오를 텍스트로 변환할 수 있어.    
```
whisper output.mp3 --model small --language en --output_format srt
```
위 명령어는 영어 음성을 텍스트로 변환하여 .srt 파일을 생성해.   
일본어 음성일 경우:    
```
whisper output.mp3 --model small --language ja --output_format srt
```
4. 자동 번역 (영어/일본어 → 한국어)    

Whisper가 생성한 .srt 파일을 번역해야 해.    
예를 들어, 영어 .srt 파일을 한국어로 번역하는 경우:    
```
cat output.srt | trans -b -s en -t ko > output_ko.srt
```
일본어 .srt 파일이라면:    
```
cat output.srt | trans -b -s ja -t ko > output_ko.srt
```

5. 여러 동영상 자동 처리 (스크립트화)    

여러 개의 영상을 한 번에 처리하는 스크립트를 만들 수 있어.    
아래 스크립트는 videos/ 폴더에 있는 모든 동영상을 변환한 후 한국어 자막을 생성해.    

```bash
#!/bin/bash

# 사용자에게 폴더 입력 받기
read -p "번역할 동영상이 있는 폴더 경로를 입력하세요: " input_folder

# 폴더가 존재하는지 확인
if [ ! -d "$input_folder" ]; then
    echo "오류: 입력한 폴더가 존재하지 않습니다."
    exit 1
fi

# 결과 저장 폴더 생성
output_folder="$input_folder/subtitles"
mkdir -p "$output_folder"

# 동영상 처리
for file in "$input_folder"/*.mp4; do
    # 파일이 없을 경우 (mp4 파일이 없으면) 스킵
    if [ ! -f "$file" ]; then
        echo "해당 폴더에 MP4 파일이 없습니다."
        exit 1
    fi

    filename=$(basename -- "$file")
    name="${filename%.*}"

    echo "처리 중: $file"

    # 오디오 추출
    ffmpeg -i "$file" -vn -acodec mp3 "$output_folder/$name.mp3"

    # 음성 인식 (Whisper, 영어 및 일본어 감지)
    whisper "$output_folder/$name.mp3" --model small --output_format srt

    # 번역 (영어 및 일본어 -> 한국어 자동 감지)
    cat "$output_folder/$name.srt" | trans -b -s auto -t ko > "$output_folder/${name}_ko.srt"

    echo "완료: $output_folder/${name}_ko.srt"
done

echo "모든 파일이 번역 완료되었습니다!"
```
```
chmod +x translate_subtitles.sh
./translate_subtitles.sh
```

추가 사항     
	•	whisper의 모델을 medium이나 large로 변경하면 더 정확한 자막을 생성할 수 있음.    
	•	GPU 사용을 원하면 whisper 실행 시 --device cuda 옵션 추가.    
	•	번역 품질을 높이고 싶다면 DeepL API 같은 유료 서비스를 사용할 수도 있음.    

이렇게 하면 여러 개의 영상을 자동으로 번역해서 자막을 만들 수 있어! 🚀    



