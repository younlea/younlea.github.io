---
title: "arduino 가변 저항을 이용한 ADC 예제"
excerpt_separator: "<!--more-->"
date: 2013-05-07 01:21:46 +0900
categories:
  - embedded
tags:
  - AI
  - 머신러닝

toc : true
toc_sticky : true
---

과제 :아두이노에서 가변저항을 이용하여 들어오는 analog 값을 digital로 연산하는 ADC를 좀더 쉽게 이해하기 위해 제작\
 아두이노에서 가변저항으로 들어오는 값을 가지고 PC에서 만든 processing 어플과 연동 함.

회로 구성\
![image](/assets/images/posts/5742190/c0028014_5187d886b57c3.jpg)source code\
arduino code\
> void setup()\
> {\
>   Serial.begin(9600);\
> }\
> \
> int x = 50;\
> int x\_backup = 50;\
> void loop(){\
> \
>    //adc0  read\
>    x = analogRead(A0);\
>       \
>    if(( abs(x - x\_backup)>10))\
>    {\
>    //write adc0 \
>      Serial.print(x);\
>      delay(15); \
>      x\_backup = x;\
>    }\
> }

processing code (빨간 부분을 추가함)\
base code : Demos -> Performance -> DynamicParticlesImmediate\
serial read and write : <http://www.processing.org/reference/libraries/serial/>\
> PImage sprite;  \
> import processing.serial.\*;\
> Serial myPort;  // The serial port\
> \
> int npartTotal = 10000;\
> int npartPerFrame = 25;\
> float speed = 1.0;\
> float gravity = 0.05;\
> float partSize = 20;\
> \
> int partLifetime;\
> PVector positions[];\
> PVector velocities[];\
> int lifetimes[];  \
> \
> int fcount, lastm;\
> float frate;\
> int fint = 3;\
> \
> void setup() {\
>   size(1024, 1024, P3D);\
>   frameRate(120);\
>   \
>   sprite = loadImage("sprite.png");\
> \
>   partLifetime = npartTotal / npartPerFrame;\
>   initPositions();\
>   initVelocities();\
>   initLifetimes(); \
> \
>   // Writing to the depth buffer is disabled to avoid rendering\
>   // artifacts due to the fact that the particles are semi-transparent\
>   // but not z-sorted.\
>   hint(DISABLE\_DEPTH\_MASK);\
>   \
>   // Testing some hints\
>   //hint(DISABLE\_TRANSFORM\_CACHE);\
>   //hint(ENABLE\_ACCURATE\_2D);\
>   \
>   //test for serial input by sulac\
>   // List all the available serial ports\
>   println(Serial.list());\
>   // Open the port you are using at the rate you want:\
>   myPort = new Serial(this, Serial.list()[1], 9600);\
>  // end test for serial input by sulac \
> } \
> \
> int x = 0, x\_posi = 0 ;\
> int y = 0, y\_posi = 0;\
> String sx;\
> String sy;\
> void draw () {\
>   background(0);\
> \
> // test for serial input by sualc\
>    byte[] inBuffer = new byte[1024];\
>    \
>     if(myPort.available() > 0) {\
>       inBuffer = myPort.readBytes();\
>       myPort.readBytes(inBuffer);\
>       if (inBuffer != null) {\
>         String myString = new String(inBuffer);\
>         print("inputdata : ");\
>         println(myString);\
> \
>         x=Integer.parseInt(myString);\
>         println(x);\
>       }\
>     }\
>     \
> // end test for serial input by sulac\
> \
>   for (int n = 0; n < npartTotal; n++) {\
>     lifetimes[n]++;\
>     if (lifetimes[n] == partLifetime) {\
>       lifetimes[n] = 0;\
>     }      \
> \
>     if (0 <= lifetimes[n]) {      \
>       float opacity = 1.0 - float(lifetimes[n]) / partLifetime;\
>             \
>       if (lifetimes[n] == 0) {\
>         // Re-spawn dead particle\
>      //  positions[n].x = mouseX;\
>        positions[n].x = x;\
>         positions[n].y = mouseY;\
>         \
>         float angle = random(0, TWO\_PI);\
>         float s = random(0.5 \* speed, 0.5 \* speed);\
>         velocities[n].x = s \* cos(angle);\
>         velocities[n].y = s \* sin(angle);\
>       } else {\
>         positions[n].x += velocities[n].x;\
>         positions[n].y += velocities[n].y;\
>         \
>         velocities[n].y += gravity;\
>       }\
>       drawParticle(positions[n], opacity);\
>     }\
>   }\
>   \
>   fcount += 1;\
>   int m = millis();\
>   if (m - lastm > 1000 \* fint) {\
>     frate = float(fcount) / fint;\
>     fcount = 0;\
>     lastm = m;\
>     println("fps: " + frate); \
>   } \
> }\
> \
> void drawParticle(PVector center, float opacity) {\
>   beginShape(QUAD);\
>   noStroke();\
>   tint(255, opacity \* 255);\
>   texture(sprite);\
>   normal(0, 0, 1);\
>   vertex(center.x - partSize/2, center.y - partSize/2, 0, 0);\
>   vertex(center.x + partSize/2, center.y - partSize/2, sprite.width, 0);\
>   vertex(center.x + partSize/2, center.y + partSize/2, sprite.width, sprite.height);\
>   vertex(center.x - partSize/2, center.y + partSize/2, 0, sprite.height);                \
>   endShape();  \
> }\
> \
> void initPositions() {\
>   positions = new PVector[npartTotal];\
>   for (int n = 0; n < positions.length; n++) {\
>     positions[n] = new PVector();\
>   }  \
> }\
> \
> void initVelocities() {\
>   velocities = new PVector[npartTotal];\
>   for (int n = 0; n < velocities.length; n++) {\
>     velocities[n] = new PVector();\
>   }\
> }\
> \
> void initLifetimes() {\
>   // Initializing particles with negative lifetimes so they are added\
>   // progressively into the screen during the first frames of the sketch   \
>   lifetimes = new int[npartTotal];\
>   int t = -1;\
>   for (int n = 0; n < lifetimes.length; n++) {    \
>     if (n % npartPerFrame == 0) {\
>       t++;\
>     }\
>     lifetimes[n] = -t;\
>   }\
> }

동영상\

\