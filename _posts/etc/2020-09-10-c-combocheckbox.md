---
title: "c# combocheckbox"
excerpt_separator: "<!--more-->"
date: 2020-09-10 08:40:19 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

c# combobox 리스트에 check box와 text와 param input

하고 싶은건

combobox로 드랍다운하고.. list중에 선택을 하고 특정값을  써 넣을수 있게 하는 방식..

ref : <https://www.codeproject.com/Articles/18929/An-OwnerDraw-ComboBox-with-CheckBoxes-in-the-Drop>

ref : <https://www.codeproject.com/Articles/21085/CheckBox-ComboBox-Extending-the-ComboBox-Class-and>

흠 일단.. panel  두개 놓구...입력할때 panel 을 앞으로 나오도록 해서 구현

패널하나에 radio button넣구 textbox 넣어서.. enable 및 data 입력하도록 구현.

셋팅 버튼 누르면 나오도록 하였으므로 panel.SendToBack()함수 쓰면 가능함.

참고 : [http://www.csharpstudy.com/](http://http://www.csharpstudy.com/)

popup은... form을 하나 더 만들고..

Form2 newForm = new Form2();

newForm.Show(); 나 newForm.ShowDialog(); 를 호출해 주면 팝업을 띄워줄수 있음

그냥 아래와 같이 간단히 쓸수도 있음.

System.Windows.Forms.MessageBox.Show("My message here");

MessageBox.Show("My message here 2");

\