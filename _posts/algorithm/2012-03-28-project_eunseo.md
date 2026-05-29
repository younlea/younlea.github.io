---
title: "project_eunseo"
excerpt_separator: "<!--more-->"
date: 2012-03-28 16:12:34 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

~~`Project\_eunseoActivity.java`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

~~`project\_eunseo.apk`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

~~`project\_eunseo.vol1.egg`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

~~`project\_eunseo.vol2.egg`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

목표.

- [완료]실행시 화면 터치가 안되게 한다.

- [완료]터치시 뽀로로 이미지를 뿌려준다

- [완료] 터치시 뽀로로 음성을 들려준다.

- [완료]화면 롱프레스 - 뽀로로 음악.

- [완료]볼륨  Up down key를 이용해서 뽀로로 음성  size를 변경한다.

- [완료]back에 대한 동작을 막는다.

- [완료]menu <-> back key 6번 반복시 종료

android.app.admin.DevicePolicyManager 클래스의 lockNow() 메소드를 호출하면 폰이 즉시 잠금상태가 됩니다.

<http://www.androidpub.com/1653214>

- **[현재불가]****위 드래그시 내려오는 상태바(stauts bar)를 막는다.**

<http://www.androidpub.com/4710>

<http://www.androidpub.com/index.php?mid=android_dev_info&category=274552&document_srl=1593227>

**- [현재불가]home key 막기**

- 음원 play시 2D animation으로 뽀로로가 춤추도록 만들어 보자.

<http://developer.android.com/guide/topics/graphics/2d-graphics.html#tween-animation>

- multitouch longpress 시 케릭터 선택 기능.

1.

android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" >

2.

<ImageView

android:id="@+id/position"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:src="@drawable/pororo\_70"

android:scaleType="center"

/>

3.

public class Project\_eunseoActivity extends Activity {

/-\* Called when the activity is first created. \*-

ImageView img;

float deltaX,deltaY;

int x,y;

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

img = (ImageView)findViewById(R.id.position);

Display display = ((WindowManager)getSystemService(Context.WINDOW\_SERVICE)).getDefaultDisplay();

deltaX = display.getWidth() \* 0.5f;

deltaY = display.getHeight() \* 0.5f;

img.setOnTouchListener(new View.OnTouchListener() {

public boolean onTouch(View v, MotionEvent event) {

Vibrator vibrator;

switch(event.getAction()){

case MotionEvent.ACTION\_DOWN:

case MotionEvent.ACTION\_UP:

case MotionEvent.ACTION\_MOVE:

vibrator = (Vibrator) getSystemService(Context.VIBRATOR\_SERVICE);

vibrator.vibrate(50);

x = (int)(event.getX()\*-1 + deltaX);

y = (int)(event.getY()\*-1 + deltaY);

img.scrollTo(x, y);

break;

}

return false;

}

});

}

}

Back key 막기

```
@Override\
    public void onAttachedToWindow() {\
        // disable home key\
        this.getWindow().setType(WindowManager.LayoutParams.TYPE_KEYGUARD);\
        super.onAttachedToWindow();\
    }\
    \
    @Override\
    public void onBackPressed(){\
        // disable back key\
        return;\
    }
```

\