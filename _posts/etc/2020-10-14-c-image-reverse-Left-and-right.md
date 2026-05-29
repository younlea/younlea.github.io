---
title: "c# image reverse Left and right"
excerpt_separator: "<!--more-->"
date: 2020-10-14 11:20:16 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

I used c# and opencv.

```
using OpenCvSharp;
 
VideoCapture video;
Mat frame = new Mat();
Mat frame2 = new Mat();
 
video.Read(frame);
Cv2.Flip(frame, frame2, FlipMode.Y);
pictureBoxIpl1.ImageIpl = frame2;
```

\