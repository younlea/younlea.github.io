---
title: "Activity 끼리 Data 전달하기"
excerpt_separator: "<!--more-->"
date: 2012-03-29 11:21:11 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Activity A -> Activity B (data 입력) -> Activity A (data receive and write to text box)

---------------------------------------------------------------------------------

ActivitySwitch mani

---------------------------------------------------------------------------------

<?xml version="1.0" encoding="utf-8"?>

<manifest xmlns:android="http://schemas.android.com/apk/res/android"

package="com.sulac.ActivitySwitch"

android:versionCode="1"

android:versionName="1.0">

<application android:icon="@drawable/icon" android:label="@string/app\_name">

<activity android:name=".ActivitySwitch"

android:label="@string/app\_name"

android:launchMode="singleTask">

<intent-filter>

<action android:name="android.intent.action.MAIN" />

<category android:name="android.intent.category.LAUNCHER" />

</intent-filter>

</activity>

<activity android:name=".NewActivity"/>

</application>

<uses-sdk android:minSdkVersion="7" />

</manifest>

---------------------------------------------------------------------------------

ActivitySwitch.java

---------------------------------------------------------------------------------

package com.sulac.ActivitySwitch;

import android.app.Activity;

import android.content.Intent;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

import android.widget.TextView;

public class ActivitySwitch extends Activity {

private final int REQUEST\_CODE = 1;

Button btn;

TextView tv;

/-\* Called when the activity is first created. \*-

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

btn = (Button)findViewById(R.id.buttonA);

btn.setOnClickListener(new View.OnClickListener() {

public void onClick(View v) {

Intent intent =new Intent(ActivitySwitch.this, NewActivity.class);

startActivityForResult(intent, REQUEST\_CODE);

}

});

}

protected void onActivityResult(int requestCode, int resultCode, Intent data){

super.onActivityResult(requestCode, resultCode, data);

if(requestCode == REQUEST\_CODE)

{

if(resultCode == RESULT\_OK)

{

String str = data.getStringExtra("data\_name");

tv = (TextView)findViewById(R.id.TextViewA);

tv.setText(str);

}

}

}

}

---------------------------------------------------------------------------------

main.xml

---------------------------------------------------------------------------------

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<TextView

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="-----A-----"

/>

<TextView

android:id="@+id/TextViewA"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

/>

<Button

android:id="@+id/buttonA"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="go B"

/>

</LinearLayout>

---------------------------------------------------------------------------------

NewActivity.java

---------------------------------------------------------------------------------

package com.sulac.ActivitySwitch;

import android.app.Activity;

import android.content.Intent;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

import android.widget.EditText;

public class NewActivity extends Activity {

Button btn;

EditText et;

/-\* Called when the activity is first created. \*-

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.testswitch);

btn = (Button)findViewById(R.id.buttonB);

btn.setOnClickListener(new View.OnClickListener() {

public void onClick(View v) {

et = (EditText)findViewById(R.id.EditText01);

Intent intent = getIntent();

intent.putExtra("data\_name", et.getText().toString());

setResult(RESULT\_OK, intent);

finish();

}

});

}

}

---------------------------------------------------------------------------------

testswitch.xml

---------------------------------------------------------------------------------

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<TextView

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="-----B-----"

/>

<EditText

android:id="@+id/EditText01"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

/>

<Button

android:id="@+id/buttonB"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="go A"

/>

</LinearLayout>

---------------------------------------------------------------------------------

---------------------------------------------------------------------------------

\