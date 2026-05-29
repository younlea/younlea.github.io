---
title: "opencv face detect and text naming"
excerpt_separator: "<!--more-->"
date: 2020-10-05 17:43:03 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

opencv face detect and text naming

```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using OpenCvSharp;
 
namespace faceDetect_v2
{
 
    public partial class Form1 : Form
    {
        VideoCapture video;
        Mat frame = new Mat();
        public Form1()
        {
            InitializeComponent();
        }
 
        private void Form1_Load(object sender, EventArgs e)
        {
            try
            {
                video = new VideoCapture(0);
                video.FrameWidth = 640;
                video.FrameHeight = 480;
            }
            catch
            {
                timer1.Enabled = false;
            }
        }
 
        private void timer1_Tick(object sender, EventArgs e)
        {
            int sleepTime = (int)Math.Round(1000 / video.Fps);
 
            String filenameFaceCascade = "haarcascade_frontalface_alt.xml";
            CascadeClassifier faceCascade = new CascadeClassifier();
 
            if (!faceCascade.Load(filenameFaceCascade))
            {
                Console.WriteLine("error");
                return;
            }
 
            video.Read(frame);
 
            // detect
            Rect[] faces = faceCascade.DetectMultiScale(frame);
            Console.WriteLine(faces.Length);
 
            foreach (var item in faces)
            {
                Cv2.Rectangle(frame, item, Scalar.Red); // add rectangle to the image
                Cv2.PutText(frame, "Test", item.TopLeft, HersheyFonts.HersheyComplex, 2, Scalar.White, 5, LineTypes.AntiAlias);
 
                Console.WriteLine("faces : " + item);
            }
 
            // display
            pictureBoxIpl1.ImageIpl = frame;
            faceCascade.Dispose();
 
            Cv2.WaitKey(sleepTime);
        }
    }
}
 
```

ref : [text](https://shalchoong.tistory.com/26)

\