---
title: "UI Events Handling (Listener)"
excerpt_separator: "<!--more-->"
date: 2012-03-27 16:12:28 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

listener

View.OnclickListener : 클릭에 대한 반응

View.OnFocusChangelistener :  입력 초점의 변경에 대한 반응.

View.OnkeyListener : key에 대한 반응

View.OnLongClickListner :  긴 클릭에 대한 반응.

View.OnTouchListener : 터치에 대한 반응.

**Exanple : OnclickListener**

>>------------AndroidManifest.xml------------<<

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<Button

**android:id="@+id/Button01"**

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="저장"

/>

</LinearLayout>

>>------------WidgetTest.java------------<<

---------------------------------------------------------------------

style 1

---------------------------------------------------------------------

package com.sulac.WidgetTest;

import android.app.Activity;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

public class WidgetTest extends Activity {

Button btn;

int flag = 0;

--\* Called when the activity is first created. --

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

**btn = (Button)findViewById(R.id.Button01);**

btn.setText("android");

btn.setOnClickListener(new View.OnClickListener() {

@Override

public void onClick(View v) {

// TODO Auto-generated method stub

if(flag==0){

btn.setText("android 0");

flag = 1;

}else

{

btn.setText("android 1");

flag = 0;

}

}

});

}

}

---------------------------------------------------------------------

style 2

---------------------------------------------------------------------

package com.sulac.WidgetTest;

import android.app.Activity;

import android.os.Bundle;

import android.view.View;

import android.view.View.OnClickListener;

import android.widget.Button;

public class WidgetTest extends Activity {

Button btn;

int flag = 0;

--\* Called when the activity is first created. --

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

btn = (Button)findViewById(R.id.Button01);

btn.setText("android");

btn.setOnClickListener(sulac);

}

OnClickListener sulac = new View.OnClickListener() {

@Override

public void onClick(View v) {

// TODO Auto-generated method stub

if(flag==0){

btn.setText("android 0");

flag = 1;

}else

{

btn.setText("android 1");

flag = 0;

}

}

};

}

---------------------------------------------------------------------

style 3

---------------------------------------------------------------------

package com.sulac.WidgetTest;

import android.app.Activity;

import android.os.Bundle;

import android.view.View;

import android.view.View.OnClickListener;

import android.widget.Button;

public class WidgetTest extends Activity implements View.OnClickListener{

Button btn;

int flag = 0;

--\* Called when the activity is first created. --

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

btn = (Button)findViewById(R.id.Button01);

btn.setText("android");

btn.setOnClickListener(this);

}

public void onClick(View v) {

// TODO Auto-generated method stub

if(flag==0){

btn.setText("android 0");

flag = 1;

}else

{

btn.setText("android 1");

flag = 0;

}

}

}

---------------------------------------------------------------------