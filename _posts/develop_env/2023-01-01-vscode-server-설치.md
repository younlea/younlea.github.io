---
title: "vscode server 설치"
excerpt_separator: "<!--more-->"
date: 2023-01-01 17:58:03 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

vscode install

<https://code.visualstudio.com/download>

.deb file install

$sudo dpkg -i code\_1.74.2-1671533413\_amd64.deb

vscode server install

<https://github.com/coder/code-server>

install 방법

$curl -fsSL https://code-server.dev/install.sh | sh

실행

sudo systemctl enable --now code-server@$USER

서버 접속을 위해.. 본인은 4000번을 사용한다.

vim .config/code-server/config.yaml

bind-addr: 0.0.0.0:4000

auth: password

password: 비밀번호

cert: false

이후 해당 서버 ip:4000으로 접속하면 뜹니다.

외부 방에서 연결하고 싶으신 분들은 AP 에서 port forwarding을 셋팅해서 하시면 됩니다.

- 단어장에 추가
  - 다음에 대한 단어 목록이 없습니다에스페란토어 → 한국어...
  - 새로운 단어 목록 생성...
- 복사