---
title: "Tinyduino(타이니두이노) - 03-3. bluetoothchat program- 2"
excerpt_separator: "<!--more-->"
date: 2014-11-11 23:45:02 +0900
categories:
  - embedded
tags:
  - etc

toc : true
toc_sticky : true
---

지난번 import error 관련해서 계속~~~

아래와 같이 import error가 발생하고 있다.. 이건 모지..\
![image](/assets/images/posts/5854175/c0028014_54621fd5e0c81.png)\
project.properties에 3이라고 되어있다.. (후에 알게 되었지만 이건 현재 setting되어 있는 level이다 ㅡㅡ)\
![image](/assets/images/posts/5854175/c0028014_54621fdb63ddb.png)\
관련 API set 을 구글에서 찾아보았다.\
![image](/assets/images/posts/5854175/c0028014_54621fe1106c8.png)\
떠그럴 developer site에 버젓이... level5란다 \
![image](/assets/images/posts/5854175/c0028014_54621fe84b19c.png)\
자.. package manager로 다운로드를 받고.. (level 5가 없어서 그 다음인 level7를 받아본다.\
![image](/assets/images/posts/5854175/c0028014_54621fedad87b.png)\
관련 level을 적용하기 위해 아래와 같이 처리 한다.\
![image](/assets/images/posts/5854175/c0028014_54621ff96036b.png)\
![image](/assets/images/posts/5854175/c0028014_54621ffdcf9d2.png)

많은 것들이 없어지고.. \
이제 6개가 남았는데..이건 또 어찌 해야할지 ㅡㅜ\
![image](/assets/images/posts/5854175/c0028014_546220d2f1a3d.png)

예전에 적었던거에 있는데 아래와 같이 clear link Markers를 호출해 주면 깔끔해 진다.\
![image](/assets/images/posts/5854175/c0028014_5462244462eee.png)

자.. 이후 또 아래와 같은 문제가 발생 ㅡㅡ; 아.. 쉽지 않네.. 그래도 에러 문구가 보이잖아.. 그럼 우리에게 구굴신이.. 쿨룩\
'launching New\_configuration' has encountered a problem.\
![image](/assets/images/posts/5854175/c0028014_54622800f1db1.png)\
찾아보면.... configuration -> run/debug setting 에서 지우면 된단다.. \
![image](/assets/images/posts/5854175/c0028014_546228174b4ae.png)

지우고 나니 아래와 같이 Run As에 여러개가 나오는데 이중 첫번째껄 실행하면 드뎌 디바이스랑 연결되서\
다운로드하고 디버깅 log cat도 동작한다. ㅋㅋㅋ.. 이제 타이니두이노 연결할 일이 남았군 ㅡ.ㅡ;\
![image](/assets/images/posts/5854175/c0028014_54622893a7168.png)

어제 무리해서 늦잠자서리 오늘은 여기까지~~~ ^^;