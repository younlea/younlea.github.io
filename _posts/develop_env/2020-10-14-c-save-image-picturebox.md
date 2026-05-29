---
title: "c# save image picturebox"
excerpt_separator: "<!--more-->"
date: 2020-10-14 10:13:10 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

save picturebox image to image file.

```
private void button6_Click(object sender, EventArgs e)
{
    string saveFolder = @"D:\temp";
    if (!System.IO.Directory.Exists(saveFolder))
        System.IO.Directory.CreateDirectory(saveFolder);
 
    pictureBox1.Image.Save(saveFolder + "\\test.png", System.Drawing.Imaging.ImageFormat.Png);
    pictureBox2.Image.Save(saveFolder + "\\test.jpg", System.Drawing.Imaging.ImageFormat.Jpeg);
}
```

\