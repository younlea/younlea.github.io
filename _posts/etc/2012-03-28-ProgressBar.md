---
title: "ProgressBar"
excerpt_separator: "<!--more-->"
date: 2012-03-28 13:55:06 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

>

<ProgressBar

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

style="?android:attr/progressBarStyleHorizontal"

android:max="100"

android:id="@+id/ProgressBar01"

/>

<ProgressBar

android:layout\_width="wrap\_content"

android:layout\_height="wrap\_content"

style="?android:attr/progressBarStyleLarge"

android:max="100"

/>

<ProgressBar

android:layout\_width="wrap\_content"

android:layout\_height="wrap\_content"

style="?android:attr/progressBarStyleSmall"

android:max="100"

/>

<SeekBar

android:layout\_width="fill\_parent"

android:layout\_height="wrap\_content"

android:max="100"

android:progress="50"

/>

<RatingBar

android:layout\_width="wrap\_content"

android:layout\_height="wrap\_content"

android:numStars="5"

android:stepSize="0.1"

/>

<TextView

android:layout\_width="fill\_parent"

android:layout\_height="fill\_parent"

android:id="@+id/TextView01" />

<!-- android:progress="50" -->

</LinearLayout>

---------------------------------------------------------------------------------------------------

package com.sulac.TestProgress;

import android.app.Activity;

import android.os.Bundle;

import android.widget.ProgressBar;

import android.widget.SeekBar;

import android.widget.TextView;

public class TestProgress extends Activity {

ProgressBar prog;

TextView text;

/-\* Called when the activity is first created. \*-

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

text = (TextView) findViewById(R.id.TextView01);

prog = (ProgressBar) findViewById(R.id.ProgressBar01);

prog.setMax(255);

prog.setProgress(0);

prog.setSecondaryProgress(0);

SeekBar seek = (SeekBar)findViewById(R.id.SeekBar01);

seek.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {

@Override

public void onStopTrackingTouch(SeekBar seekBar) {

// TODO Auto-generated method stub

}

@Override

public void onStartTrackingTouch(SeekBar seekBar) {

// TODO Auto-generated method stub

}

@Override

public void onProgressChanged(SeekBar seekBar, int progress,

boolean fromUser) {

// TODO Auto-generated method stub

prog.setSecondaryProgress(progress);

text.setBackgroundColor(0xFF000000 + (progress<<8) + (progress << 16) + progress);

}

});

}

}