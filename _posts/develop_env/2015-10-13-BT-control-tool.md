---
title: "BT control tool"
excerpt_separator: "<!--more-->"
date: 2015-10-13 16:04:33 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

bluetooth chat sample code을 사용

api level - properties - android api level 수정

필요없는 부분

null pointer라는 에러가 뜨는데 아래 부분 comment처리하면 됨..

```
   private final void setStatus(int resId) {
        //final ActionBar actionBar = getActionBar();
        //actionBar.setSubtitle(resId);
    }
 
    private final void setStatus(CharSequence subTitle) {
        //final ActionBar actionBar = getActionBar();
        //actionBar.setSubtitle(subTitle);
    }
```

--- connect button 만들기 ( connect and hand shacking)

--- [조이스틱 button 만들기](http://www.akexorcist.com/2012/10/android-code-joystick-controller.html) (android virtual gamepad button example)

[조이스틱 button 만들기](http://www.allmost.org/2012/12/android-bluetooth-dual-joystick.html)

code 받기 : $svn checkout ***http***://yus-repo.googlecode.com/svn/trunk/ yus-repo-read-only

~~`btjoystick.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

Left joystick으로 전후좌후 command 보내도록 수정

```
 private void UpdateMethod() {
        
        // if either of the joysticks is not on the center, or timeout occurred
        if(!mCenterL || !mCenterR || (mTimeoutCounter>=mMaxTimeoutCount && mMaxTimeoutCount>-1) ) {
            // limit to {0..10}
            byte radiusL = (byte) ( Math.min( mRadiusL, 10.0 ) );
            byte radiusR = (byte) ( Math.min( mRadiusR, 10.0 ) );
            // scale to {0..35}
            byte angleL = (byte) ( mAngleL * 18.0 / Math.PI + 36.0 + 0.5 );
            byte angleR = (byte) ( mAngleR * 18.0 / Math.PI + 36.0 + 0.5 );
            if( angleL >= 36 )    angleL = (byte)(angleL-36);
            if( angleR >= 36 )    angleR = (byte)(angleR-36);
            
            if (D) {
                Log.d(TAG, String.format("%d, %d, %d, %d", radiusL, angleL, radiusR, angleR ) );
            }
            
            if(angleL <= 5 || angleL > 31 )
                sendMessage( "F" );
            else if(angleL <= 14 && angleL > 5)
                sendMessage( "L" );
            else if(angleL <= 23 && angleL > 14)
                sendMessage( "B" );
            else if(angleL <= 31 && angleL > 23)
                sendMessage( "R" );
```

arduino에서 확인 하는 코드

```
 
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);   //uart
  Serial1.begin(9600);  // bt
//  Serial1.println("AT+NAMEtest");
}
 
// the loop routine runs over and over again forever:
void loop() {
  if(Serial1.available())
   Serial.println((char)Serial1.read());
  
}
 
```

cmd 잘 오는건 확인했다.. ^^;

device 골라서 연결하는 부분

```
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode){
        case REQUEST_CONNECT_DEVICE:
            // When DeviceListActivity returns with a device to connect
            if (resultCode == Activity.RESULT_OK) {
                // Get the device MAC address
                String address = data.getExtras().getString(DeviceListActivity.EXTRA_DEVICE_ADDRESS);
                // Get the BLuetoothDevice object
                BluetoothDevice device = mBluetoothAdapter.getRemoteDevice(address);
                // Attempt to connect to the device
                mRfcommClient.connect(device);
            }
            break;
        case REQUEST_ENABLE_BT:
            // When the request to enable Bluetooth returns
            if (resultCode != Activity.RESULT_OK) {
                // User did not enable Bluetooth or an error occurred
                Toast.makeText(this, R.string.bt_not_enabled_leaving, Toast.LENGTH_SHORT).show();
                finish();
            }
            break;
        }
    }
```

message send 하는 부분sendMessage( "F" );message receive 하는 부분