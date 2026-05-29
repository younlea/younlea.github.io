---
title: "C# file open dialog"
excerpt_separator: "<!--more-->"
date: 2020-10-22 16:24:37 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

file open and the name return

```
OpenFileDialog openFile = new OpenFileDialog();
openFile.DefaultExt = "jpg";
openFile.Filter = "Images Files(*.jpg; *.jpeg; *.gif; *.bmp; *.png)|*.jpg;*.jpeg;*.gif;*.bmp;*.png";
openFile.ShowDialog();
if (openFile.FileNames.Length > 0)
{
    foreach (string filename in openFile.FileNames)
    {
        this.textBox1.Text = filename;
    }
}
```

\