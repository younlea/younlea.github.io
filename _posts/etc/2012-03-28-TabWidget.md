---
title: "TabWidget"
excerpt_separator: "<!--more-->"
date: 2012-03-28 14:56:17 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

<?xml version="1.0" encoding="utf-8"?>

<TabHost xmlns:android="http://schemas.android.com/apk/res/android"

android:id="@android:id/tabhost"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent">

<LinearLayout

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent">

<FrameLayout

android:layout\_weight="1"

android:id="@android:id/tabcontent"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent">

<AnalogClock

android:id="@+id/AnalogClock01"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

/>

<DigitalClock

android:id="@+id/DigitalClock01"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:textSize="50dp"

android:gravity="center"

/>

</FrameLayout>

<TabWidget

android:id="@android:id/tabs"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"/>

</LinearLayout>

</TabHost>

---------------------------------------------------------------------------------

package com.hardrock.hellotest;

import android.app.TabActivity;

import android.os.Bundle;

import android.widget.TabHost;

public class HelloTestActivity extends TabActivity {

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

TabHost mTabHost = getTabHost();

mTabHost.addTab(mTabHost.newTabSpec("tab\_test1")

.setIndicator("아날로그 시계")

.setContent(R.id.AnalogClock01));

mTabHost.addTab(mTabHost.newTabSpec("tab\_test2")

.setIndicator("디지털 시계")

.setContent(R.id.DigitalClock01));

mTabHost.setCurrentTab(0);

}

}

\