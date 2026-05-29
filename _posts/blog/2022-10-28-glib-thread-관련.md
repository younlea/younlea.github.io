---
title: "glib thread 관련."
excerpt_separator: "<!--more-->"
date: 2022-10-28 11:52:03 +0900
categories:
  - blog
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

glib thread에 있는 timer를 쓰려다 삽질을 많이 해서 살짝 정리 좀.

[glib document](https://docs.gtk.org/glib/index.html) : 솔찍히 보기 힘들다 ㅜㅜ

[glib\_2.56.0\_document](https://www.manpagez.com/html/glib/glib-2.56.0/glib-The-Main-Event-Loop.php)

[glib 2.72.3 document](https://libsoup.org/glib/index.html)

[glib referance manual](https://www.freedesktop.org/software/gstreamer-sdk/data/docs/2012.5/glib/glib-The-Main-Event-Loop.html)

기본적인 main loop만드는것 정리된 곳 - [link](https://lethean.github.io/2009/09/21/using-glib-mainloop/) [link](https://d-yong.tistory.com/11)

remove 와 destroy, unref 관련 정리 된돗 - [link](https://sigquit.wordpress.com/2010/01/05/g_source_unref-and-g_source_destroy-are-your-friends/)

[c++ example 확인 가능한 곳](https://cpp.hotexamples.com/) - 함수 하나 넣으면 샘플 코드 볼 수가 있네요.

참고 [link](https://ioctl.tistory.com/253)

--------------------------------------------------------------

g\_timer\_add()의 경우 defautl source를 셋팅하는것으로 보인다. 그렇다 보니 timer 여러개를 이걸로 동시에 셋팅하면 안되는걸로 보임.

해결책으로는 timeout\_source를 추가해서 설정하면 되는것으로 확인.

[ref](https://stackoverflow.com/questions/22980188/run-g-timeout-add-in-thread-context-and-not-in-default-context)

--------------------------------------------------------------------------

```
GSource *source = g_timeout_source_new (interval_in_msecs);
```

```
g_source_set_priority (source, your_priority);
```

```
g_source_set_callback (source, your_callback, your_data, your_data_notify);
```

```
g_source_set_name (source, source_name); // useful for debuggingreturn
```

```
g_source_attach (source, your_main_context);
```

```
--------------------------------------------------------------------------
```

```

```

```
priority 관련 내용 확인 link
```

```
#define G_PRIORITY_HIGH            -100
```

```
#define G_PRIORITY_DEFAULT          0
```

```
#define G_PRIORITY_HIGH_IDLE        100
```

```
#define G_PRIORITY_DEFAULT_IDLE     200
```

```
#define G_PRIORITY_LOW              300
```