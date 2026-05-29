---
title: "checkbox Test and Radio Button"
excerpt_separator: "<!--more-->"
date: 2012-03-27 10:38:32 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent">

<CheckBox

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="진동모드"/>

<RadioGroup

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content">

<RadioButton

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="빨간색"/>

<RadioButton

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="녹색"/>

<RadioButton

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="파란색"/>

</RadioGroup>

</LinearLayout>