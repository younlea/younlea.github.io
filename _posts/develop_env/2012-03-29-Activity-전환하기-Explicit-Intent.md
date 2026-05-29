---
title: "Activity 전환하기 (Explicit Intent)"
excerpt_separator: "<!--more-->"
date: 2012-03-29 09:55:16 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

A Activity -> B Activity -> A Activity 전환.

------------------------------------------------------------------------------------

main.xml

------------------------------------------------------------------------------------

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

<Button

android:id="@+id/buttonA"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="go B"

/>

</LinearLayout>

------------------------------------------------------------------------------------

ActivitySwitch.java

------------------------------------------------------------------------------------

package com.sulac.ActivitySwitch;

import android.app.Activity;

import android.content.Intent;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

public class ActivitySwitch extends Activity {

Button btn;

/-\* Called when the activity is first created. \*-

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

btn = (Button)findViewById(R.id.buttonA);

btn.setOnClickListener(new View.OnClickListener() {

public void onClick(View v) {

Intent intent =new Intent(ActivitySwitch.this, NewActivity.class);

startActivity(intent);

}

});

}

}

------------------------------------------------------------------------------------

testswitch.xml

------------------------------------------------------------------------------------

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

<Button

android:id="@+id/buttonB"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="go A"

/>

</LinearLayout>

------------------------------------------------------------------------------------

NewActivity.java

------------------------------------------------------------------------------------

package com.sulac.ActivitySwitch;

import android.app.Activity;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

public class NewActivity extends Activity {

Button btn;

/-\* Called when the activity is first created. \*-

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.testswitch);

btn = (Button)findViewById(R.id.buttonB);

btn.setOnClickListener(new View.OnClickListener() {

public void onClick(View v) {

//Intent intent =new Intent(NewActivity.this, ActivitySwitch.class);

//startActivity(intent);

finish();

}

});

}

}

------------------------------------------------------------------------------------

ActivitySwitch Manifest

------------------------------------------------------------------------------------

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

<activity android:name=".NewActivity"

android:theme="@android:style/Theme.Dialog"/>

</application>

<uses-sdk android:minSdkVersion="7" />

</manifest>

------------------------------------------------------------------------------------