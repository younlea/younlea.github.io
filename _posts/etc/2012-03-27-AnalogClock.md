---
title: "AnalogClock"
excerpt_separator: "<!--more-->"
date: 2012-03-27 14:13:25 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<AnalogClock

android:layout\_width="wrap\_content"

android:layout\_height="wrap\_content"

android:dial="@drawable/clockgoog\_dial"

android:hand\_hour="@drawable/clockgoog\_hour"

android:hand\_minute="@drawable/clockgoog\_minute"

/>

<DigitalClock

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:textSize="50sp"/>

</LinearLayout>