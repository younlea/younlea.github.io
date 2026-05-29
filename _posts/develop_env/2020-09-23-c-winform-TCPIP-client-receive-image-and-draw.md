---
title: "c# winform - TCP/IP client receive image and draw"
excerpt_separator: "<!--more-->"
date: 2020-09-23 08:48:58 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

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
using SimpleTCP;
using System.IO;
 
namespace tcpip_client
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
 
        SimpleTcpClient client;
 
        private void Form1_Load(object sender, EventArgs e)
        {
            client = new SimpleTcpClient();
            client.StringEncoder = Encoding.UTF8;
            client.DataReceived += Client_DataReceived;
        }
        private byte[] StringToByte(string str)
        {
            byte[] StrByte = Encoding.UTF8.GetBytes(str);
            return StrByte;
        }
        public Image ByteArrayToImage(byte[] bytes)
        {
            //ImageConverter imgcvt = new ImageConverter();
            //Image img = (Image)imgcvt.ConvertFrom(bytes);
            //return img;
            MemoryStream ms = new MemoryStream(bytes);
            Image recImg = Image.FromStream(ms);
            return recImg;
        }
        private void Client_DataReceived(object sender, SimpleTCP.Message e)
        {
             pictureBox1.Image = ByteArrayToImage(e.Data);
        }
 
        private void connect_Click(object sender, EventArgs e)
        {
            connect.Enabled = false;
 
            client.Connect("10.253.101.119", 8080);
        }
 
        private void disconnect_Click(object sender, EventArgs e)
        {
            client.Disconnect();
            connect.Enabled = true;
        }
 
        private void getImage_Click(object sender, EventArgs e)
        {
            client.WriteLine("IMG");
        }
 
        private void GetData_Click(object sender, EventArgs e)
        {
            client.WriteLine("DAT");
        }
 
 
    }
}
```