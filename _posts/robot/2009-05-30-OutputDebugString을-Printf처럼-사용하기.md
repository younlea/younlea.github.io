---
title: "OutputDebugString을 Printf처럼 사용하기~~"
excerpt_separator: "<!--more-->"
date: 2009-05-30 11:41:25 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

아.. 아래껄루 확인할라면.. Dbgview.exe를 사용하면 되용.. 인터넷에 많네용 ^^;

Log\_sulac("sdfsdfasf %d",parm1); 이 가능하게 하려면 아래와 같이 수정하면 됨 (이건 VC6.0에서 사용하는 방법임)

void Log\_Sulac(const char\* format,...)\
{\
 char buffer[4096];\
 va\_list vaList;\
 va\_start(vaList, format);\
 \_vsnprintf(buffer, 4096, format, vaList);\
 va\_end(vaList);

 OutputDebugString(buffer);\
}

아 그리고 만약 앞에 문자열을 넣고 싶을 경우 아래와 같이 바꾸면 된다. ^^;\
void Log\_Sulac(const char\* format,...)\
{\
 char buffer[4096];\
 va\_list vaList;\
 va\_start(vaList, format);\
 \_vsnprintf(buffer, 4096, format, vaList);\
 va\_end(vaList);

// OutputDebugString(buffer);\
 CString str;\
 str.Format("[MEDIC]%s",buffer);\
 OutputDebugString(str);\
}

헉.. 아래 방법도 가능하다고 하네 ㅜㅜ [저기.. ""앞뒤에 괄호를... 사용하면 되나? 나중에 해봐야징 ^^;\

LOG\_SULAC(("Port %d, Iconnect %d, iDetect %d"), PARM1,PARM2,PARM3);xml:namespace prefix = o ns = "urn:schemas-microsoft-com:office:office" /

 

inline void LOG\_SULAC(const char\* format, ...)

{

CString str;

str.Format("[Medic] %s", format);

OutputDebugString(str);

}