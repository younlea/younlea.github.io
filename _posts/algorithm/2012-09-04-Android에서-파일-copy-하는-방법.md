---
title: "Android에서 파일 copy 하는 방법.."
excerpt_separator: "<!--more-->"
date: 2012-09-04 00:11:59 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

public static void copyFile(String src, String dest){

long fsize = 0;

try{

FileInputStream fin = new FileInputStream(src);

FileOutputStream fout = new FileOutputStream(dest);

FileChannel inc = fin.getChannel();

FileChannel outc = fout.getChannel();

//복사할 file size

fsize = inc.size();

inc.transferTo(0, fsize, outc);

inc.close();

outc.close();

fin.close();

fout.close();

/-

//입출력을 위한 버퍼를 할당한다.

ByteBuffer buf = ByteBuffer.allocateDirect(1024);

while(true){

if(inc.read(buf)==-1)

break;

buf.flip();

outc.write(buf);

buf.clear();

}

\*-

}

catch(Exception e){

e.printStackTrace();

}

}