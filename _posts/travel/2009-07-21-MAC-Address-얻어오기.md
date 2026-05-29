---
title: "MAC Address 얻어오기."
excerpt_separator: "<!--more-->"
date: 2009-07-21 20:02:58 +0900
categories:
  - travel
tags:
  - travel
  - 여행

toc : true
toc_sticky : true
---

GetAdaptersInfo() 

퍼옴..

**[출처]** [MacAddress 맥어드레스 구하기](http://blog.naver.com/trts1015/20032641689)|**작성자** [trts1015](http://blog.naver.com/trts1015)

//==============================================================

//GetAdaptersInfo를 이용하여 맥어드레스를 얻는 방법입니다.\
#include <Iphlpapi.h>\
#pragma comment(lib, "Iphlpapi.lib")

BOOL GetMacAddress(BYTE\* byteMacAddress) {\
 BOOL  bResult = FALSE;\
 IP\_ADAPTER\_INFO   \*p;\
 ULONG     len = 0;

 if (GetAdaptersInfo(NULL, &len) == ERROR\_BUFFER\_OVERFLOW ) {\
  p = new IP\_ADAPTER\_INFO[ len ]; \
  GetAdaptersInfo(p, &len);

  if (p) {\
   memcpy(byteMacAddress, p->Address, 6);\
   bResult = TRUE;\
  }\
  delete[] p;\
 } \
 return bResult; \
}\
//-------------------------

//문자열로 바로 리턴하여주는 함수.\
CString GetMacAddress() {\
 BOOL  bResult = FALSE;\
 IP\_ADAPTER\_INFO   \*p;\
 ULONG     len = 0;\
 PBYTE mac;\
 CString one, str = \_T("");\
 if (GetAdaptersInfo(NULL, &len) == ERROR\_BUFFER\_OVERFLOW ) {\
  p = new IP\_ADAPTER\_INFO[ len ]; \
  GetAdaptersInfo(p, &len);\
  if (p) {\
   mac = (PBYTE) p->Address;\
   len = p->AddressLength;\
   for(ULONG i=0; i < len; i++){\
    one.Format(\_T("%02X"), mac[i]);\
    str += one;\
    if( i < len-1 ) str += \_T(":");\
   }\
  }\
  delete[] p;\
 } \
 return str; \
}

//==============================================================

//-------------------------

//IP를 이용하여 맥어드레스를 얻는 방법입니다.

//자기자신일경우:GetMacAddress(\_T("\*"));

//남일경우:GetMacAddress(\_T("192.168.1.1"));

//-------------------------\
#include <nb30.h>\
#pragma comment(lib, "netapi32")\
//netapi32.lib\
CString GetMacAddress(CString strIP) \
{\
    NCB Ncb;\
    UCHAR uRetCode;\
    LANA\_ENUM lenum;\
    int i;\
    CString strOutput =\_T("");\
    CString string;\
    ADAPTER\_STATUS Adapter;

    memset( &Ncb, 0, sizeof(Ncb) );\
    Ncb.ncb\_command = NCBENUM;\
    Ncb.ncb\_buffer = (UCHAR \*)&lenum;\
    Ncb.ncb\_length = sizeof(lenum);\
    uRetCode = Netbios( &Ncb );

    for(i=0; i < lenum.length ;i++)\
    {\
        memset( &Ncb, 0, sizeof(Ncb) );\
        Ncb.ncb\_command = NCBRESET;\
        Ncb.ncb\_lana\_num = lenum.lana[i];

        uRetCode = Netbios( &Ncb );

        memset( &Ncb, 0, sizeof (Ncb) );\
        Ncb.ncb\_command = NCBASTAT;\
        Ncb.ncb\_lana\_num = lenum.lana[i];

        strcpy( (char\*)Ncb.ncb\_callname, strIP ); \
        Ncb.ncb\_buffer = (unsigned char \*) &Adapter;\
        Ncb.ncb\_length = sizeof(Adapter);

        uRetCode = Netbios( &Ncb );\
        if ( uRetCode == 0 )\
        {\
            string.Format("%02X:%02X:%02X:%02X:%02X:%02X",\
                    Adapter.adapter\_address[0],\
                    Adapter.adapter\_address[1],\
                    Adapter.adapter\_address[2],\
                    Adapter.adapter\_address[3],\
                    Adapter.adapter\_address[4],\
                    Adapter.adapter\_address[5] );\
            strOutput += string ;\
        }\
    }\
    return strOutput;\
}

//==============================================================

//아래소스는 GetAdaptersInfo 함수를 사용하지 못하는 특정한 CE 플렛폼에서 사용하였음.

//-------------------------

//KernelIoControl를 이용하여 맥어드레스를 얻는 방법입니다.

//-------------------------

#include <winioctl.h>\
extern "C" BOOL KernelIoControl ( DWORD , LPVOID , DWORD , LPVOID , DWORD , LPDWORD);\
#define IOCTL\_HAL\_REBOOT  CTL\_CODE(FILE\_DEVICE\_HAL, 15, METHOD\_BUFFERED, FILE\_ANY\_ACCESS)\
#define IOCTL\_HAL\_DRAWER\_ON  CTL\_CODE  ( FILE\_DEVICE\_HAL , 2048 , METHOD\_BUFFERED , FILE\_ANY\_ACCESS )\
#define IOCTL\_HAL\_DRAWER\_OFF CTL\_CODE ( FILE\_DEVICE\_HAL , 2049 , METHOD\_BUFFERED , FILE\_ANY\_ACCESS ) \
#define IOCTL\_HAL\_DRAWER\_OPEN CTL\_CODE ( FILE\_DEVICE\_HAL , 2050 , METHOD\_BUFFERED , FILE\_ANY\_ACCESS ) \
#define IOCTL\_HAL\_MAC\_READ  CTL\_CODE ( FILE\_DEVICE\_HAL , 2051 , METHOD\_BUFFERED , FILE\_ANY\_ACCESS )\
#define IOCTL\_HAL\_MAC\_WRITE  CTL\_CODE ( FILE\_DEVICE\_HAL , 2052 , METHOD\_BUFFERED , FILE\_ANY\_ACCESS )\
#define IOCTL\_HAL\_BEEP\_ON  CTL\_CODE ( FILE\_DEVICE\_HAL , 2053 , METHOD\_BUFFERED , FILE\_ANY\_ACCESS )\
#define IOCTL\_HAL\_BEEP\_OFF  CTL\_CODE ( FILE\_DEVICE\_HAL , 2054 , METHOD\_BUFFERED , FILE\_ANY\_ACCESS )\
BOOL GetMacAddress(BYTE\* byteMacAddress) {\
 BOOL  bResult = FALSE;\
 unsigned char MacAddress[10];\
 DWORD dwS;\
 bResult = KernelIoControl ( IOCTL\_HAL\_MAC\_READ , NULL , NULL, (LPVOID)&MacAddress , 7 , &dwS );\
 memcpy(byteMacAddress, MacAddress, 6);\
 return bResult;\
}