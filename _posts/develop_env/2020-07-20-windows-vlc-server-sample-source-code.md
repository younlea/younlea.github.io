---
title: "windows vlc server sample source code"
excerpt_separator: "<!--more-->"
date: 2020-07-20 17:09:13 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

https://github.com/ZeBobo5/Vlc.DotNet

-> https://github.com/ZeBobo5/Vlc.DotNet/blob/develop/src/Samples/Samples.Core.Streaming/Program.cs

```
1 using System;

2 using System.IO;

3 using System.Reflection;

4

5 namespace Samples.Core.Streaming

6 {

7     class Program

8     {

9         static void Main()

10         {

11             var currentDirectory = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location);

12             // Default installation path of VideoLAN.LibVLC.Windows

13             var libDirectory =

14                 new DirectoryInfo(Path.Combine(currentDirectory, "libvlc", IntPtr.Size == 4 ? "win-x86" : "win-x64"));

15

16             using (var mediaPlayer = new Vlc.DotNet.Core.VlcMediaPlayer(libDirectory))

17             {

18

19                 var mediaOptions = new[]

20                 {

21                     ":sout=#rtp{sdp=rtsp://127.0.0.1:554/}",

22                     ":sout-keep"

23                 };

24

25                 mediaPlayer.SetMedia(new Uri("http://hls1.addictradio.net/addictrock_aac_hls/playlist.m3u8"),

26                     mediaOptions);

27

28                 mediaPlayer.Play();

29

30                 Console.WriteLine("Streaming on rtsp://127.0.0.1:554/");

31                 Console.WriteLine("Press any key to exit");

32                 Console.ReadKey();

33             }

34         }

35     }

36 }

```