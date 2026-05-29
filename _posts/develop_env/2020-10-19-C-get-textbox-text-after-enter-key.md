---
title: "C# get textbox text after enter key."
excerpt_separator: "<!--more-->"
date: 2020-10-19 08:10:51 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

1. add key press event.

2. check e.KeyChar == 13 (enter key)

```
private void textBox1_KeyPress(object sender, KeyPressEventArgs e)
{
    if(textBox1.Text != "")
    {
        if(e.KeyChar == 13)  // 13 == enter
        Console.WriteLine("------------------------------>" + textBox1.Text);
    }
}
```

c#에서 textbox에 숫자를 입력했을때 엔터키를 넣으면 숫자를 읽어 오도록 하는 코드 입니다.