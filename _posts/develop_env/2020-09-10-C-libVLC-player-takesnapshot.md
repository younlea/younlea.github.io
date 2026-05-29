---
title: "C# libVLC player takesnapshot"
excerpt_separator: "<!--more-->"
date: 2020-09-10 11:13:18 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

vlc play 도중 특정 시점에서 이미지 캡쳐 하는 방법

ref : <https://code.videolan.org/videolan/LibVLCSharp/-/issues/203>

```
 string _rtspEndpoint = "rtsp://admin:password@192.168.1.108:554/channel=1&stream=0.sdp"; Core.Initialize(); LibVLC libVLC = new LibVLC();             libVLC.SetLogFile("c:\\temp\\vlc.log"); MediaPlayer player = new MediaPlayer(libVLC); player.Play(new Media(libVLC, _rtspEndpoint, FromType.FromLocation)); player.TakeSnapshot(0, "c:\\temp\\pic.jpeg", 0, 0);
```

[ref](http://https://github.com/ZeBobo5/Vlc.DotNet/blob/develop/src/Samples/Samples.Core.Thumbnailer/Program.cs)

mediaPlayer.SetMedia(

new Uri("http://download.blender.org/peach/bigbuckbunny\_movies/big\_buck\_bunny\_480p\_h264.mov"));

TaskCompletionSource<bool> tcs = new TaskCompletionSource<bool>();

var lastSnapshot = 0L;

mediaPlayer.TimeChanged += (sender, e) =>

{

// Maps the time to a 5-seconds interval to take a snapshot every 5 seconds

var snapshotInterval = e.NewTime / 5000;

// Take a snapshot every 5 seconds

if (snapshotInterval > lastSnapshot)

{

lastSnapshot = snapshotInterval;

ThreadPool.QueueUserWorkItem(\_ =>

{

mediaPlayer.TakeSnapshot(0, Path.Combine(destinationFolder, $"{snapshotInterval}.png"), 1024, 0);

});

}

};