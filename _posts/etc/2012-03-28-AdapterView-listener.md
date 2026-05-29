---
title: "AdapterView listener"
excerpt_separator: "<!--more-->"
date: 2012-03-28 11:22:15 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

AdapterView.OnItemclicklistener

AdapterView.OnitemLongclickListener

Adapterview.OnItemSelectedListener

example

------------------------------main.xml --------------------------------------------

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<ListView

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:id="@+id/ListView01"

/>

</LinearLayout>

-------------------------------java code-------------------------------------------

package com.sulac.ListTest;

import android.app.Activity;

import android.os.Bundle;

import android.view.View;

import android.widget.AdapterView;

import android.widget.ArrayAdapter;

import android.widget.ListView;

import android.widget.Toast;

public class ListTest extends Activity {

ListView list;

String[] NUMBER = {"1","2","3","4","5","6","7","8","9","10"};

/-\* Called when the activity is first created. \*-

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

ArrayAdapter adapter = new ArrayAdapter(this,

android.R.layout.simple\_list\_item\_1, NUMBER);

list = (ListView)findViewById(R.id.ListView01);

list.setAdapter(adapter);

//item click listener

list.setOnItemClickListener(new AdapterView.OnItemClickListener() {

public void onItemClick(AdapterView parent, View v, int position, long id)

{

Toast.makeText(ListTest.this, NUMBER[position], Toast.LENGTH\_SHORT).show();

}

});

//item long click listener

list.setOnItemLongClickListener(new AdapterView.OnItemLongClickListener() {

public boolean onItemLongClick(AdapterView parent, View v, int posotion, long id)

{

Toast.makeText(ListTest.this, "long", Toast.LENGTH\_SHORT).show();

return true;

}

});

// item selected listener

list.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {

public void onItemSelected(AdapterView parent, View v, int position, long id)

{

Toast.makeText(ListTest.this, "Selected", Toast.LENGTH\_SHORT).show();

}

public void onNothingSelected(AdapterView parent)

{

Toast.makeText(ListTest.this, "onNothingSelected", Toast.LENGTH\_SHORT).show();

}

});

}

}