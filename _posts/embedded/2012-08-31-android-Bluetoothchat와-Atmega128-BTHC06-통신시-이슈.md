---
title: "android Bluetoothchat와 Atmega128 BT(HC-06) 통신시 이슈"
excerpt_separator: "<!--more-->"
date: 2012-08-31 01:38:57 +0900
categories:
  - embedded
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

안드로이드 예제 블루투스 채팅에서  Phone에서 나가는 데이터는 잘 받아지나.

ATmega128에서 오는 데이터는 자꾸 첫번째 바이트 읽고 나머지 바이트를 읽는 이슈가 있어서..

BluetoothChatService.java에서 아래와 같이 수정하였음.

이렇게 되면 문제 되는 부분은 해결이 된다. ㅋㅋㅋ 좋구나~~

public void run() {

Log.i(TAG, "BEGIN mConnectedThread");

byte[] buffer = new byte[1024];

int bytes;

int check\_bytes = 0;

byte[] buffer\_b = new byte[1024];

// Keep listening to the InputStream while connected

while (true) {

try {

// Read from the InputStream

bytes = mmInStream.read(buffer);

if(bytes == 1)

{

//buffer\_b 에 buffer 를 1byte copy

System.arraycopy(buffer, 0, buffer\_b, 0, 1);

check\_bytes = 1;

}

else

{

if(check\_bytes==1)

{

//buffer\_b에 buffer를 붙이고 bytes+1 해서 보냄

System.arraycopy(buffer, 0, buffer\_b, 1, bytes);

mHandler.obtainMessage(Sensor\_mode.MESSAGE\_READ, bytes+1, -1, buffer\_b)

.sendToTarget();

check\_bytes=0;

}else

{

// Send the obtained bytes to the UI Activity

mHandler.obtainMessage(Sensor\_mode.MESSAGE\_READ, bytes, -1, buffer)

.sendToTarget();

}

}

} catch (IOException e) {

Log.e(TAG, "disconnected", e);

connectionLost();

break;

}

}

}