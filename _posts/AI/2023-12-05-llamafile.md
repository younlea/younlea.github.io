---
title: "portable llamafile "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - llama

toc : true
toc_sticky : true
---
# [llamafile](https://github.com/Mozilla-Ocho/llamafile)    
이제 llama 설치 등 힘들이지 않고 파일 하나만 실행하고 로컬 호스트에서 써봐요.



## Quickstart

The easiest way to try it for yourself is to download our example 
llamafile for the [LLaVA](https://llava-vl.github.io/) model (license: [LLaMA 2](https://ai.meta.com/resources/models-and-libraries/llama-downloads/), 
[OpenAI](https://openai.com/policies/terms-of-use)). LLaVA is a new LLM that can do more 
than just chat; you can also upload images and ask it questions 
about them. With llamafile, this all happens locally; no data 
ever leaves your computer.

1. Download [llava-v1.5-7b-q4-server.llamafile](https://huggingface.co/jartine/llava-v1.5-7B-GGUF/resolve/main/llava-v1.5-7b-q4-server.llamafile?download=true) (3.97 GB).

2. Open your computer's terminal.

3. If you're using macOS, Linux, or BSD, you'll need to grant permission 
for your computer to execute this new file. (You only need to do this 
once.)

```sh
chmod +x llava-v1.5-7b-q4-server.llamafile
```

4. If you're on Windows, rename the file by adding ".exe" on the end.

5. Run the llamafile. e.g.:

```sh
./llava-v1.5-7b-q4-server.llamafile
```

6. Your browser should open automatically and display a chat interface. 
(If it doesn't, just open your browser and point it at https://localhost:8080.)

7. When you're done chatting, return to your terminal and hit 
```Control-C``` to shut down llamafile.

**Having trouble? See the "Gotchas" section below.**
