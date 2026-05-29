---
title: "Multiwii를 arduino mega board에 포팅할때 I2C 포트 찾기?"
excerpt_separator: "<!--more-->"
date: 2016-06-01 00:29:19 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

Multiwii를 Arduino mega를 사용하여 만들고 싶다는 분이 있으셔서 간단히 설명 합니다.

일단 Arduino의 경우 모든 포트의 셋팅은  setup 에서 하고 있습니다.

그럼 우리도 핀에 대한 정보는 여기서 확인이 가능하겠죠?

그럼 Multiwii.cpp에서 setup() 을 찾아가 보겠습니다.

위에 66번 째 라인에 initSensor() 가 보이네요.

자 그럼 initSensors를 따라가 보겠습니다.

sensors.cpp

그럼 딱... i2c\_init() 함수가 위의 5번째 라인에서 보이는군요..

그럼 또 따라가 보겠습니다. (우리가 원하는건 i2C 핀 번호이니까요 ^^ 어딘가 있을겁니다. 자자 고고)

sensors.cpp

```
void i2c_init(void) {
  #if defined(INTERNAL_I2C_PULLUPS)
    I2C_PULLUPS_ENABLE
  #else
    I2C_PULLUPS_DISABLE
  #endif
  TWSR = 0;                                    // no prescaler => prescaler = 1
  TWBR = ((F_CPU / I2C_SPEED) - 16) / 2;       // change the I2C clock rate
  TWCR = 1<<TWEN;                              // enable twi module, no interrupt
}
```

자 뭔지는 모르겠지만 I2C\_PULLUPS\_ENABLE 나  I2C\_PULLUPS\_DISABLE 이 보이네요..

이건 어디서 선언했을까요???

또 찾아 봅시다.

찾아보니 defs.h 에서 세군대에서 선언이 되어 있습니다.

그중 우리가 찾고 싶은건 MEGA 이니까요..599line에 가면 아래 부분이 나오네요

여기서.. 25번 줄에 20번과 21번이 SDA 와 SCL 이 나온다고 하네요. ^^;

일단 찾고자 한것은 찾았는데. .한가지 궁굼해 지는것.. 어디서  **MEGA** 를 선언할까요??

모 또 찾아 보면 되겠죠? ^^;

찾아 봅시다..

def.h 에 아래 부분이 있습니다.

```
/**************************************************************************************/
/***************             Proc specific definitions             ********************/
/**************************************************************************************/
// Proc auto detection
#if defined(__AVR_ATmega168__) || defined(__AVR_ATmega328P__)
  #define PROMINI
#endif
#if defined(__AVR_ATmega32U4__) || defined(TEENSY20)
  #define PROMICRO
#endif
#if defined(__AVR_ATmega1280__) || defined(__AVR_ATmega1281__) || defined(__AVR_ATmega2560__) || defined(__AVR_ATmega2561__)
  #define MEGA
#endif
```

오...#if defined(\_\_AVR\_ATmega1280\_\_) || defined(\_\_AVR\_ATmega1281\_\_) || defined(\_\_AVR\_ATmega2560\_\_) ||  defined(\_\_AVR\_ATmega2561\_\_)

이렇게 네가지중 하나라도 선언이 되어 있으면  MEGA 를 선언하는군요..

마지막으로.. 이 네가지 선언은 어디서 하는걸까요??? 필자가 알기로는 arduino sdk 에서 빌드를 할때 칩을 선택하죠?

그렇게 하시면 pre define 되는걸로 알고 있습니다. (못 믿으시겠다구요.. 그럼 저기에 error 코드 넣고 빌드해보세요 ^^)

일단.. 메일로 궁굼하다고 보내주신 분.. ^^ 실명은 밝히지 못하겠고.. 메일로 답변 달면 이쁘게 답변 못들아서 이렇게 답변 사항 올려봅니다.

\