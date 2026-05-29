---
title: "android 근접센서(ProximitySensor)"
excerpt_separator: "<!--more-->"
date: 2012-04-04 22:33:58 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

검색 : getDefaultSensor sensor.type\_proximity example

<http://developer.android.com/reference/android/hardware/Sensor.html> 에서 찾아서 구현이 가능할듯...

<http://developer.android.com/guide/topics/sensors/sensors_position.html>  << 예제가 나온다.

## Using the Proximity Sensor

The proximity sensor lets you determine how far away an object is from a device. The following code shows you how to get an instance of the default proximity sensor:

```
private SensorManager mSensorManager;\
private Sensor mSensor;\
...\
mSensorManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);\
mSensor = mSensorManager.getDefaultSensor(Sensor.TYPE_PROXIMITY);
```

The proximity sensor is usually used to determine how far away a person's head is from the face of a handset device (for example, when a user is making or receiving a phone call). Most proximity sensors return the absolute distance, in cm, but some return only near and far values. The following code shows you how to use the proximity sensor:

```
public class SensorActivity extends Activity implements SensorEventListener {\
  private SensorManager mSensorManager;\
  private Sensor mProximity;

  @Override\
  public final void onCreate(Bundle savedInstanceState) {\
    super.onCreate(savedInstanceState);\
    setContentView(R.layout.main);

    // Get an instance of the sensor service, and use that to get an instance of\
    // a particular sensor.\
    mSensorManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);\
    mProximity = mSensorManager.getDefaultSensor(Sensor.TYPE_PROXIMITY);\
  }

  @Override\
  public final void onAccuracyChanged(Sensor sensor, int accuracy) {\
    // Do something here if sensor accuracy changes.\
  }

  @Override\
  public final void onSensorChanged(SensorEvent event) {\
    float distance = event.values[0];\
    // Do something with this sensor data.\
  }

  @Override\
  protected void onResume() {\
    // Register a listener for the sensor.\
    super.onResume();\
    mSensorManager.registerListener(this, mProximity, SensorManager.SENSOR_DELAY_NORMAL);\
  }

  @Override\
  protected void onPause() {\
    // Be sure to unregister the sensor when the activity pauses.\
    super.onPause();\
    mSensorManager.unregisterListener(this);\
  }\
}
```

**Note:** Some proximity sensors return binary values that represent "near" or "far." In this case, the sensor usually reports its maximum range value in the far state and a lesser value in the near state. Typically, the far value is a value > 5 cm, but this can vary from sensor to sensor. You can determine a sensor's maximum range by using the `getMaximumRange()` method.

아웅.. 모.. 그대로 써도 되지만.. 약간 수정해서.. 아래와 같이 하면 toast로 화면에 거리를 뿌려주게 된다. ^^;

---------------------------- ProximitySensor --------------------------------------------\

package com.sulac.ProximitySensor;

import android.app.Activity;

import android.content.Context;

import android.hardware.Sensor;

import android.hardware.SensorEvent;

import android.hardware.SensorEventListener;

import android.hardware.SensorManager;

import android.os.Bundle;

import android.widget.Toast;

public class ProximitySensorActivity extends Activity implements SensorEventListener{

private SensorManager mSensorManager;

private Sensor mProximity;

/-\* Called when the activity is first created. \*-

@Override

public void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

// Get an instance of the sensor service, and use that to get an instance of

// a particular sensor.

mSensorManager = (SensorManager) getSystemService(Context.SENSOR\_SERVICE);

mProximity = mSensorManager.getDefaultSensor(Sensor.TYPE\_PROXIMITY);

}

public final void onAccuracyChanged(Sensor sensor, int accuracy) {

// Do something here if sensor accuracy changes.

}

public final void onSensorChanged(SensorEvent event) {

float distance = event.values[0];

// Do something with this sensor data.

String s = ""+distance;

Toast.makeText(this, s, Toast.LENGTH\_SHORT).show();

}

@Override

protected void onResume() {

// Register a listener for the sensor.

super.onResume();

mSensorManager.registerListener(this, mProximity, SensorManager.SENSOR\_DELAY\_NORMAL);

}

@Override

protected void onPause() {

// Be sure to unregister the sensor when the activity pauses.

super.onPause();

mSensorManager.unregisterListener(this);

}

}