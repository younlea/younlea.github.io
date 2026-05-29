---
title: "MFC dialog file 읽기"
excerpt_separator: "<!--more-->"
date: 2009-04-01 21:51:01 +0900
categories:
  - develop_env
tags:
  - AI
  - 머신러닝

toc : true
toc_sticky : true
---

파일열기 공통 다이얼로그 박스

1.     파일 열기

//필터설정

Char szFilter[]=”Image File(\*.bmp;\*.jpg;\*.tga)|\*.bmp;\*.jpg;\*.tga|ALL File(\*.\*)|\*.\*}”;

xml:namespace prefix = o ns = "urn:schemas-microsoft-com:office:office" / 

CfileDialog dlg(TRUE, NULL, NULL, OFN\_HIDEREADONLY | OFN\_OVERWRITEPROMPT,

szFilter, NULL);

 

if(dlg.DoModal() == IDOK)

{

Cstring PathName = dlg.GetPathName(); // 전체 경로를 포함한 파일 이름

Cstring FileName = dlg.GetFileName(); // 파일 이름

}

 

2.     파일 저장

Char szFilter[] = “Text File(\*.txt)|\*.txt|”;

CfileDialog dlg(FALSE, “확장자”, “디폴트파일명”, OFN\_HIDEREADONLY | OFN\_OVERWRITEPROMPT, szFilter, NULL);

 

If(dlg.Domodal() == IDOK)

{

Cstring PathName = dlg.GetPathName(); // 전체 경로를 포함한 파일 이름

Cstring FileName = dlg.GetFileName(); // 파일 이름

}

 

3.     멀티 셀렉트 파일 열기

Char szFilter[]=”Image File(\*.bmp;\*.jpg;\*.tga)|\*.bmp;\*.jpg;\*.tga|ALL File(\*.\*)|\*.\*}”;

 

CfileDialog dlg(TRUE, NULL, NULL, OFN\_HIDEREADONLY | OFN\_OVERWRITEPROMPT | OFN\_ALLOWMULTISELECT, szFilter, NULL);

 

//처음 선택되는 폴더 지정 방법

dlg.m\_ofn.IpstrInitialDir = “C:\\WINNT”; // WINNT 폴더를 보여준다.

 

//셀렉트 되는 파일 목록 수를 늘릴려면 다음과 같이 해준다.

//배열의 크기가 크면 클수록 많은 수를 선택 할 수 있다.

Char strFile[4096]={0,}; //NULL 로 초기화 해줘야 정상 동작한다.

dlg.m\_ofn.IpstrFile = strFile;

dlg.m\_ofn.nMaxFile = sizeof(strFile);

 

if(dlg.DoModal() == IDOK)

{

POSITION pos=dlg.GetStartPosition();

 

Cstring str;

While(pos)

{

Str += dlg.GetNextpathName(pos)+”\n”; // GetNextPathName 함수로 파일 이름을 알아낸다.

}

MessageBox(str); //MessageBox에 파일 리스트를 출력해서 확인한다.

}

xml:namespace prefix = v ns = "urn:schemas-microsoft-com:vml" /

Referance

<http://msdn.microsoft.com/ko-kr/default.aspx>

<http://www.devpia.com/> 

<http://cafe.naver.com/bccdr.cafe>