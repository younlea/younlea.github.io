---
title: "c# winform panel opacity setting"
excerpt_separator: "<!--more-->"
date: 2020-10-19 16:51:35 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

I want to make opacity about panel2.

- panel2

- panel1 - pictureBox1

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
 
namespace alarm_UI
{
    public partial class Form1 : Form
    {
        int flag = 0;
        public Form1()
        {
            InitializeComponent();
        }
 
        private void Form1_Load(object sender, EventArgs e)
        {
            panel2.Parent = pictureBox1;
            //panel2.BackColor = Color.FromArgb(110, 0, 0, 0);
            panel2.BackColor = Color.FromArgb(110, Color.Red);
        }
    }
}
 
```

panel1에 picturebox1을 그리고..

panel2에 투명도를 넣을때 pictureBox1을 panel2의 parent로 설정해주면 된다. ^^;

\