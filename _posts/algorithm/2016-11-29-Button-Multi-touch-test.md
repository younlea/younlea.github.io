---
title: "Button Multi touch test"
excerpt_separator: "<!--more-->"
date: 2016-11-29 23:31:25 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

이상하게 이전에 만든 APP에서 multitouch가 지원이 안되서....

최근 버젼으로 한번 버튼 두개 만들어서 multi touch 되는지 확인..

결과는 잘된다...  test version 은 android 6.0 이다.

안되는 버젼은 4.1.2 인데 비슷한 경우가 있는듯 하다.

<http://stackoverflow.com/questions/20050555/android-multitouch-on-2-3-x>

그리고 그냥 테스트하려는 코드를 나중에 또 쓸까봐 올려 놓는다. ^^;

Android XML

```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="${relativePackage}.${activityClass}" >
    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:layout_gravity="right|bottom" >
 
               <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/hello_world" />
            
            <Button
                android:id="@+id/buttonA"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="buttonA"/>
        
            <Button
                android:id="@+id/buttonB"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="buttonB"/>
    </LinearLayout>
</RelativeLayout>
 
```

android main activity java

```
package com.sulac.multitouch_test;
 
import android.app.Activity;
import android.os.Bundle;
import android.util.Log;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
 
public class MainActivity extends Activity {
    private Button mButtonA;
    private Button mButtonB;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        mButtonA = (Button) findViewById(R.id.buttonA);
        mButtonA.setOnClickListener(new OnClickListener() {
            
            public void onClick(View v) {
                // TODO Auto-generated method stub
                Log.d("TEST", "----- press butten A");
            }
        });
        
        mButtonB = (Button) findViewById(R.id.buttonB);
        mButtonB.setOnClickListener(new OnClickListener() {
            
            public void onClick(View v) {
                // TODO Auto-generated method stub
                Log.d("TEST", "----- press butten B");
            }
        });
        
    }
}
 
```

문제점 해결.. ㅜㅜmanifest에서... minSdkVersion... 8로 되어 있었는데.. 이게 프로이요 버젼인것 같구... 이때는 multitouch가 안됐던것 같다.결국 아래 숫자만 8 -> 16으로 바꾸니 잘 된다 ㅜㅜ

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="xx.xxx.xxx"
      android:versionCode="2"
      android:versionName="2.0">
    <uses-sdk android:minSdkVersion="16"/>   <<<<<<-------- this number.. 
    
```