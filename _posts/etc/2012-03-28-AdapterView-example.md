---
title: "AdapterView example"
excerpt_separator: "<!--more-->"
date: 2012-03-28 10:40:50 +0900
categories:
  - etc
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

android.widget.AdapterView

ListView

GridView

Spinner

Gallery

어댑터(adapter)

- ArrayAdapter : string 자료

- CursorAdapter : 여러 자료.

------------------------------------------------------------------------

ListView

------------------------------------------------------------------------

main.xml

layout만 만든다.

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<ListView

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:id="@+id/ListView01"/>

</LinearLayout>

src/ListTest.java

package com.sulac.ListTest;

import android.app.Activity;

import android.os.Bundle;

import android.widget.ArrayAdapter;

import android.widget.ListView;

public class ListTest extends Activity {

ListView list;

String[] NUMBER = {"1","2","3","4","5","6","7","8","9","10"};

--\* Called when the activity is first created. --

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

ArrayAdapter adapter = new ArrayAdapter(this,

android.R.layout.simple\_list\_item\_1, NUMBER);

list = (ListView)findViewById(R.id.ListView01);

list.setAdapter(adapter);

}

}

------------------------------------------------------------------------

GridView

------------------------------------------------------------------------

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<GridView

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:id="@+id/GridView01"

android:numColumns="3"

android:horizontalSpacing="1px"

android:verticalSpacing="1px"

android:stretchMode="columnWidth"

android:listSelector="@drawable/icon"

/>

<!-- android:listSelector="@android:color/transparent"-->

</LinearLayout>

package com.sulac.ListTest;

import android.app.Activity;

import android.os.Bundle;

import android.widget.ArrayAdapter;

import android.widget.GridView;

import android.widget.ListView;

public class ListTest extends Activity {

GridView list;

String[] NUMBER = {"1","2","3","4","5","6","7","8","9","10"};

--\* Called when the activity is first created. --

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

ArrayAdapter adapter = new ArrayAdapter(this,

android.R.layout.simple\_list\_item\_1, NUMBER);

list = (GridView)findViewById(R.id.GridView01);

list.setAdapter(adapter);

}

}

------------------------------------------------------------------------

Gallery

------------------------------------------------------------------------

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<Gallery

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:id="@+id/Gallery01"

/>

</LinearLayout>

package com.sulac.ListTest;

import android.app.Activity;

import android.os.Bundle;

import android.widget.ArrayAdapter;

import android.widget.Gallery;

import android.widget.GridView;

public class ListTest extends Activity {

Gallery list;

String[] NUMBER = {"1","2","3","4","5","6","7","8","9","10"};

--\* Called when the activity is first created. --

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

ArrayAdapter adapter = new ArrayAdapter(this,

android.R.layout.simple\_list\_item\_1, NUMBER);

list = (Gallery)findViewById(R.id.Gallery01);

list.setAdapter(adapter);

}

}

------------------------------------------------------------------------

spinner

------------------------------------------------------------------------

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<Spinner

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:id="@+id/Spinner01"

/>

</LinearLayout>

package com.sulac.ListTest;

import android.app.Activity;

import android.os.Bundle;

import android.widget.ArrayAdapter;

import android.widget.Spinner;

public class ListTest extends Activity {

Spinner list;

String[] NUMBER = {"1","2","3","4","5","6","7","8","9","10"};

--\* Called when the activity is first created. --

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

ArrayAdapter adapter = new ArrayAdapter(this,

android.R.layout.simple\_spinner\_item, NUMBER);

adapter.setDropDownViewResource(android.R.layout.simple\_spinner\_dropdown\_item);

list = (Spinner)findViewById(R.id.Spinner01);

list.setAdapter(adapter);

}

}