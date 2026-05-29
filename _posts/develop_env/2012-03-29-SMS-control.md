---
title: "SMS control"
excerpt_separator: "<!--more-->"
date: 2012-03-29 16:06:22 +0900
categories:
  - develop_env
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

Message 보내기.

SmsManager smsManager = SmsManager.getDefault();

smsManager.sendTextMessage("01012345678", null, sb.toString(),null, null);

<uses-permission android:name="android.permission.SEND\_SMS"/>

Message 받기.

public void onReceive(Context context, Intent intent) {

Log.d("MY\_TAG", "BroadcastReceiver onReceive()");

if( intent.getAction().equals("android.provider.Telephony.SMS\_RECEIVED") ) {

StringBuilder sb = new StringBuilder();

Bundle bundle = intent.getExtras();

if (bundle != null) {

// SMS는 PDU라는 데이터 포맷을 사용한다.

Object[] pdusObj = (Object[]) bundle.get("pdus");

// pdu 바이트 배열을 SmsMessage 객체로 변환한다.

SmsMessage[] messages = new SmsMessage[pdusObj.length];

for(int i = 0; i < pdusObj.length; i++) {

messages[i] = SmsMessage.createFromPdu((byte[]) pdusObj[i]);

}

// 모든 수신된 문자 메세지에서 발신자 전화 번호와 수신 메세지를 구한다.

for (SmsMessage currentMessage : messages) {

sb.append("문자메세지가 수신되었습니다.\n");

// 발신자 전화 번호

sb.append("[ 발신자 전화 번호 ]\n");

sb.append(currentMessage.getOriginatingAddress());

// 수신 메세지

sb.append("\n[ 수신 메세지 ]\n");

sb.append(currentMessage.getMessageBody());

}

}

<uses-permission android:name="android.permission.RECEIVE\_SMS"/>