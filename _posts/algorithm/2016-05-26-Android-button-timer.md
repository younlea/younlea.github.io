---
title: "Android button timer"
excerpt_separator: "<!--more-->"
date: 2016-05-26 16:13:22 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

버튼 하나 만들고 해당 버튼을 눌렀을때 이미지 두개를 switch 하고 싶을때 사용하면 좋다 ^^;

[Link](http://stackoverflow.com/questions/27008979/how-to-delay-put-on-button-click-android)

```
clcikbutton.setOnClickListener(new OnClickListener() {    @Override    public void onClick(View arg0) {       SimpleDateFormat simpleDateFormat = new SimpleDateFormat("MM/dd/yyyy hh:mm:ss aa");       simpleDateFormat.setTimeZone(TimeZone.getTimeZone("UTC"));       textView1.setText(DateFormat.getDateTimeInstance().format(new java.util.Date("11/7/2014 5:19:11 AM UTC")));       clcikbutton.setEnabled(false);       new Handler().postDelayed(new Runnable() {            @Override            public void run() {                clcikbutton.setEnabled(true);                    }        },5000);    }});
```

```

```