---
title: "[Test]Project"
excerpt_separator: "<!--more-->"
date: 2012-03-27 10:43:52 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

Calc

------------------------------------------

0       << edit text

------------------------------------------

button7   | button 8  | button 9 |  button +       << button (selector를 사용하면 버튼이 이쁘게 바뀐다.)

button 4  | button 5  | button 6 |  button -

button 1  | button 2  | button 3 |  button \*

button 0  | button =                | button /

-----------------------------------------

image view  |  Text view.

-----------------------------------------

화면   HVGA, WVGA800을 만족하는 프로젝트를 개발.

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:background="#ffffffff">

<EditText

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:text="0"

android:gravity="right"/>

<RelativeLayout

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content">

<LinearLayout

android:id="@+id/bottom\_linearlayout"

android:orientation="horizontal"

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:layout\_alignParentBottom="true">

<ImageView

android:layout\_width="150dp"

android:layout\_height="200dp"

android:scaleType="fitStart"

android:layout\_marginLeft="5dp"

android:src="@drawable/star1"

android:gravity="left"/>

<TextView

android:layout\_width="wrap\_content"

android:layout\_height="wrap\_content"

android:text="made by sulac"

android:layout\_gravity="bottom|right"

android:gravity="right"/>

</LinearLayout>

<TableLayout

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:stretchColumns='\*'

android:layout\_above="@id/bottom\_linearlayout">

<TableRow android:layout\_weight="1">

<Button android:layout\_height="fill\_parent" android:text="7"/>

<Button android:layout\_height="fill\_parent" android:text="8"/>

<Button android:layout\_height="fill\_parent" android:text="9"/>

<Button android:layout\_height="fill\_parent" android:text="+"/>

</TableRow>

<TableRow android:layout\_weight="1">

<Button android:layout\_height="fill\_parent" android:text="4"/>

<Button android:layout\_height="fill\_parent" android:text="5"/>

<Button android:layout\_height="fill\_parent" android:text="6"/>

<Button android:layout\_height="fill\_parent" android:text="-"/>

</TableRow>

<TableRow android:layout\_weight="1">

<Button android:layout\_height="fill\_parent" android:text="1"/>

<Button android:layout\_height="fill\_parent" android:text="2"/>

<Button android:layout\_height="fill\_parent" android:text="3"/>

<Button android:layout\_height="fill\_parent" android:text="\*"/>

</TableRow>

<TableRow android:layout\_weight="1">

<Button android:layout\_height="fill\_parent" android:text="0"/>

<Button android:layout\_height="fill\_parent"

android:layout\_span="2"

android:text="="/>

<Button android:layout\_height="fill\_parent" android:text="/"/>

</TableRow>

</TableLayout>

</RelativeLayout>

</LinearLayout>

\