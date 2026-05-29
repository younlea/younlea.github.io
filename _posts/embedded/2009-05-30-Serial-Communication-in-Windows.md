---
title: "Serial Communication in Windows"
excerpt_separator: "<!--more-->"
date: 2009-05-30 14:40:19 +0900
categories:
  - embedded
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

**퍼옴.

Serial Communication in Windows**

- [Download source files - 55 Kb](http://www.codeproject.com/system/serial_com/serial_com_src.zip)

## Introduction

This article is meant to give you a jump start on doing serial communication in Windows (NT family). The article will provide a class called CSerialCommHelper which you can use directly to do serial communication in your application. The class that is provided here with this article does uses overlapped IO. You donot need to know much about serial communication or overlapped IO for this article. However, you need to know some about the synchronization objects like Events and some Windows APIs like WaitForSingleObject and WaitForMultipleObject etc. Also some basic understanding of windows threads is required - like thread creation and termination.

## Introduction

In order for your computer to be able to do serial communication, computer has to have a serial port. Most of the computers have at least one serial port also known as COM port ( communication port ) and are generally called COM1 COM2 etc. Then there are the device drivers for the serial ports. If you think it over, all you that you need to do in serial communication is either send data or receive data. In other words, you are doing input/output (IO) to the serial port. The same IO is done with disk based files. Hence there is no surprise that the APIs for reading and writing to a file apply to serial ports as well. When you send data to the serial port its in terms of bytes but when it leaves the serial port it is in the form of bits. Similarly, when the data arrives at the serial port, its in bit format and when you get data you get it in bytes.

Without any further discussion lets get started.

## Opening the COM port

The first and the foremost step in doing a serial communication is to open the desired port. Lets say you have your device hooked to COM1 you can open the COM port using following API:

```
HANDLE m_hCommPort = ::CreateFile(szPortName,            GENERIC_READ|GENERIC_WRITE,//access ( read and write)            0,    //(share) 0:cannot share the COM port                                    0,    //security  (None)                            OPEN_EXISTING,// creation : open_existing            FILE_FLAG_OVERLAPPED,// we want overlapped operation            0// no templates file for COM port...            );
```

The third fifth and seventh parameters have to be what they are in the above example by law. We want to open the file ( the COM port ) in an overlapped fashion - that's why the sixth param is `FILE_FLAG_OVERLAPPED`. We will get into the details of overlapped IO a bit later. As you must have guessed from the name , `CreateFile` API can be used to create a file (disk based) and also it can be used to open an existing file.

To Windows a serial port or a disk based file both are IO devices . So, in order to open an existing file ( serial port ) all we need to know the name of the device ( COM1) and pass the creation flags as `OPEN_EXISTING`.

If a COM port is opened successfully, the API returns handle to the com port just like a handle to a file. However, if the system could not open the COM port, it would return `INVALID_HANDLE_VALUE` . And you can get the reason by calling `GetLastError`. One of the common errors while opening a COM port is that the COM port is already opened by some other application and in that case you would get `ERROR_ACCESS_DENIED` (5). Similarly if you by mistake opened a COM port that doesn't exist , you would get `ERROR_FILE_NOT_FOUND`  as the last error.

Note: Remember not to do make any function calls (like `ASSERT`) before calling `GetLastError` or you would get 0. Once you have opened the com port all you need to do now is to start using it.

## Reading and Writing

Now, once you have a com port open, you may want to send  some data to the connected device. For example, lets say you want to send "Hello" to the device(e.g., another PC). When you want to send the data across the serial port, you need to write to the serial port just like you would write to a file. You would use following API:

```
iRet = WriteFile (m_hCommPort,data,dwSize,&dwBytesWritten ,&ov);    
```

where data contains "Hello" . Lets say in response to your "Hello" , the device sends you "Hi" . So you need to read the data. Again ,you would  use following API:

```
abRet = ::ReadFile(m_hCommPort,szTmp ,sizeof(szTmp ),                   &dwBytesRead,&ovRead) ;
```

For now do not try to understand everything. We will get to all this later. All this sounds very simple. Right?

Now lets start digging into issues.

## Issues with serial communication

Just now I said, in response to your "Hello", the device may send you "Hi" back and you would like to read that. But the problem here is that you don't know when the device is going to respond? Or will it ever respond? When should you start to read from the port. One option is that as soon as you made call to WriteFile, you make call to ReadFile . If no data is there you need to make read again later on. This leads to what is called polling. You keep polling the port for data. This model does not really  seem to be a good one. It would be nice if somehow you were notified by the system when data has arrived and only then would you make call to ReadFile. This is event driven approach and fits well into Windows programming. And good news is that such a model is possible .

Another issue with the serial communication is that since it always occurs between two devices, the two devices need to agree on how they talk to each other. Each side needs to follow certain protocols to conduct business. Since its the serial port that actually carries out the communication, we need to configure the serial port. There is an API available for exact same purpose. Following is the API:

```
SetCommState ( HANDLE hFile, LPDCB lpDCB)
```

The first parameter is the handle to COM port and the second paramter is what is called device control block (DCB) . The DCB is a struct defined in winbase.h and has 28 data members. For example, we need to specify baud rate at which the COM port operates, you need to set the BaudRate member of the struct . Baud rate is usual 9600 (bps) . But the two devices have to use the same baud rate to conduct business. Similarly if you want to use parity you need to set Parity member of the struct. Again the two devices have to use same parity. Some of the data members are reserved and have to be 0. I have found it easier to get the current DCB struct and then set those members which we are interested in changing. Following code gets the current dcb and sets some of the fields:

```
DCB dcb = {0};dcb.DCBlength = sizeof(DCB);if (!::GetCommState (m_hCommPort,&dcb)){    TRACE ("CSerialCommHelper : Failed to Get Comm State Reason: %d",           GetLastError());    return E_FAIL;}dcb.BaudRate    = dwBaudRate;dcb.ByteSize    = byByteSize;dcb.Parity        = byParity;if ( byStopBits == 1 )    dcb.StopBits    = ONESTOPBIT;else if (byStopBits == 2 )     dcb.StopBits    = TWOSTOPBITS;else     dcb.StopBits    = ONE5STOPBITS;if (!::SetCommState (m_hCommPort,&dcb)){    ASSERT(0);    TRACE ( "CSerialCommHelper : Failed to Set Comm State Reason: %d",            GetLastError());    return E_FAIL;}TRACE ( "CSerialCommHelper : Current Settings, (Baud Rate %d; Parity %d; "        "Byte Size %d; Stop Bits %d", dcb.BaudRate,
```

Most of the time you won't need to change the other fields of this structure. But if you need to change the structure you need to be very careful about the fields as changing the fields will affect the behavior of the serial communication and hence you should be very sure what you want to change.

## Event Driven Approach

Coming back to our earlier problem with the reading of data. If we do not want to  keep polling the COM port for any data then we need to have some kind of event mechanism available. Fortunately there is a way that you can ask the system to notify you when certain events happen. The API to use is

```
SetCommMask( HANDLE hHandle,DWORD dwEvtMask)
```

The first parameter is the handle to the open COM port. The second parameter is used to specify a list of events which we are interested in. \
The events that need to be specified in the mask depends upon the application needs. For simplicity, lets say we are interested in getting  notified whenever a character arrives at the serial port, we would need to specify `EV_RXCHAR` as the event mask. Similarly if we are interested to know when all the data has been sent, we need to specify `EV_TXEMPTY` flag also. So out call would look like this:

```
SetCommMask( m_hCommPort,EV_TXTEMPTY|EV_RXCHAR);
```

  The interesting thing here is that although we told system about the events of our interest, we did not however told system what to do when these events occur. Like how would system let us know that a particular event occurred. An obvious thing seems to be a callback mechanism. But there is not such mechanism available. Here is when things get a little tricky. In order for system to let us know about the communication event occurrence, we need to call `WaitCommEvent` This function waits for the events specified in `SetCommMask`. But if your think a little more, it sounds like we are turning a notification mechanism back to polling mechanism. Actually its even worse that than . `WaitCommEvent` blocks till an event occurs. So what's the use of `WaitCommEvent` ? Well , the answer lies in overlapped IO.\
If you look at the `WaitCommEvent` signature it looks like this:

```
BOOL WaitCommEvent(HANDLE hCommPort, LPDWORD dwEvtMask,LPOVERLAPPED lpOverlapped);
```

The third parameter is the key here.

Think of overlapped IO as asynchronous IO. Whenever a function makes a call and specifies the overlapped IO structure, it means that try to do the current operation but if you are not able to complete it immediately let me know when you are done with this IO. The way system lets you know about the completion is by setting an kernel event object that is part of the `lpOverlapped` structure. So, all you do is spawn a thread and make the thread wait for that event object using one of the `WaitForSingleObject` APIs.\
Lets look at the overlapped structure:

```
typedef struct _OVERLAPPED {    DWORD Internal;    DWORD InternalHigh;    DWORD Offset;    DWORD OffsetHigh;    HANDLE hEvent;} OVERLAPPED, *LPOVERLAPPED;
```

The last parameter is the event handle that you need to create . This event is generally a manual reset event. When you make a call like `WaitCommEvent` passing overlapped structure as the last parameter, and the system could not complete call meaning it did not see any characters at the port, it would return immediately but would return FALSE. If you now make a call to `GetLastError` you would get `ERROR_IO_PENDING` which means that the call has been accepted but no characters have yet arrived at the COM port. Also it means whenever the characters will arrive, the system will set the `hEvent` of the overlapped structure that you passed in. So if your thread would wait for single object on `hEvent` and you pass `INFINITE`, then whenever your Wait fn. returns `WAIT_OBJECT_0` it means some character has arrived  or all the data in the output buffer has been sent.

In our current case since we are interested in more than one events we would need to check what event did we get by making call to `GetCommMask` and checking the `DWORD` against each event. Following  pseudo code will explain it:

 You can read the data from the com port and reset the event and make the call to `WaitCommEvent` again and so on.

```
unsigned __stdcall CSerialCommHelper::ThreadFn(void*pvParam){    OVERLAPPED ov;    memset(&ov,0,sizeof(ov));    ov.hEvent = CreateEvent( 0,true,0,0);    HANDLE arHandles[2];    arHandles[0] = apThis->m_hThreadTerm;    DWORD dwWait;    SetEvent(apThis->m_hThreadStarted);    while (  abContinue )    {                BOOL abRet = ::WaitCommEvent(apThis->m_hCommPort,&dwEventMask, &ov) ;        if ( !abRet )        {                        ASSERT( GetLastError () == ERROR_IO_PENDING);        }                arHandles[1] = ov.hEvent ;                dwWait = WaitForMultipleObjects (2,arHandles,FALSE,INFINITE);        switch ( dwWait )        {        case WAIT_OBJECT_0:            {                _endthreadex(1);            }            break;        case WAIT_OBJECT_0 + 1:            {                DWORD dwMask;                if (GetCommMask(apThis->m_hCommPort,&dwMask) )                {                   if ( dwMask & EV_TXEMPTY )                   TRACE("Data sent");                   ResetEvent ( ov.hEvent );                   continue;                }                else                  {                   //read data here and reset ov.hEvent                }            }        }//switch    }//whilereturn 0;}
```

If you understood the above code , you will understand the whole of this article and the source code provided. The above piece of code is simple using the overlapped IO method to do its job.

Once we have received the indication that the data has arrived we need to read the data. Important thing to note here is that the when data arrives at the serial port, it is copied over to system buffer.  The data is removed from the system buffer only when you have read the data using API such as `ReadFile`.  Like any buffer, system buffer has a limited size. So if you do not read the data from the buffers quick enough the system buffers can be become full if more data is arriving. What happens to further data depends upon the configuration that you have set in the device configuration block (in call to `SetCommState` ). Usually the applications do some kind of handshaking at the application level but you can also make configurations such that the com port does not accept any further data upon buffer-full events. But all that is beyond the scope of this discussion. If possible its always better to have applications themselves implementing some kind of handshaking  - like do not send next block of data until you get okay for the first block. Generally this kind of handshaking is implemented using some sort of ACK / NAK  and ENQ protocol.

In order for us to read data we need to use `ReadFile` API. `ReadFile` API has to specify how much data to read. Lets say we are monitoring character arrivals and 10 characters arrive at the port. As soon as first character arrives at the port the system will set the overlapped structure's event object and out `WaitSingleObject` will return. Next we would need to read the data. So how much data should we read? Should we read 1 byte or  10 bytes? That is a good question.   The way it works is as follows (Note: this is not documented anywhere but this is what I have found by research on Win2K,NT4.0)  :

When one (or more) characters arrive at the port, the event object associated with the overlapped structure set once. Now lets say that you made a call to read and you read 1 character. After reading 1 character , you would finally Reset the overlapped structure's event object. Now you would go back to the `WaitCommEvent` but it would return false since no "new" character has arrived. So you will not be able to read any more characters.  Now when another character arrives, system will set the overlapped event and you would read one more character but this time it will be the character that had arrived earlier and you never read. This clearly is a  problem.

So what is the solution? The easiest solution is that as soon as you got the event object indicating the arrival of a character, you should read all the characters that are present in the port. (If you are familiar with win API `MsgWaitForMultipleObjects` you can draw a analogy here.)

So again the question remains how many characters to read. The answer is read all the characters in a loop using `ReadFile`. Here is the pseudo code

```
threadFn...    WaitCommEvent(m_hCommPort,&dwEventMask, &ov) ;    if ( WaitForSingleObject(ov.hEvent,INFINITE) == WAIT_OBJECT_0)    {            char szBuf[100];        memset(szBuf,0,sizeof(szBuf));        do        {            ReadFile( hPort,szBuf,sizeof(szBuf),&dwBytesRead,&ov);        }while (dwBytesRead > 0 );        }
```

`ReadFile` API has following signature:\

```
BOOL ReadFile( HANDLE hFile, // handle to file         LPVOID lpBuffer, // data buffer         DWORD nNumberOfBytesToRead, // number of bytes to read         LPDWORD lpNumberOfBytesRead, // number of bytes read         LPOVERLAPPED lpOverlapped // overlapped buffer );
```

The first parameter is as usual the com port, the last parameter is the overlapped structure. Again we need to create a manual reset event and pass the overlapped structure to the ReadFile function. Again if you issue a read for say 10 bytes and there is no data available , `ReadFile` will return FALSE and `GetLastError` will return `ERROR_IO_PENDING` and the system will set the overlapped event when the overlapped operation(read ) completes.

As you can see `ReadFile` returns `dwBytesRead` which as is clear returns the number of bytes read. If there are no bytes remaining, the dwBytesRead will return 0. Lets say there are 11 bytes that have arrived and you read 10  characters in the first go in while loop. In the first go 10 characters will be returned in `dwBytesRead`. In the second go with while loop, the `dwBytesRead` will return 1. Now in the third go the `dwBytesRead` will return 0 and you will break out of the while loop. This allows you to read all the data. In this approach ,if you noticed, we never really took advantage of the overlapped structure that we passed to the `ReadFile` function but we still need to pass it because we opened the COM port in Overlapped manner.

And finally when you want to send data to other device, you need to call `WriteFile`. `WriteFile` is not even worth discussing.

There is one more thing that needs to be taken into account before we move on and that is communication timeouts. Its important to set the timeout to proper values for things to work. The API to do so is:

```
SetCommTimeouts ( HANDLE hCommPort, LPCOMMTIMEOUTS lpCommTimeOuts)
```

`COMTIMEOUTS` is a structure with following members:

```
typedef struct _COMMTIMEOUTS {    DWORD ReadIntervalTimeout;   DWORD ReadTotalTimeoutMultiplier;   DWORD ReadTotalTimeoutConstant;   DWORD WriteTotalTimeoutMultiplier;   DWORD WriteTotalTimeoutConstant; } COMMTIMEOUTS,*LPCOMMTIMEOUTS;
```

For a description of all these fields consult MSDN documentation. But one thing I want to point out is this:

> "...A value of `MAXDWORD`, combined with zero values for both the `ReadTotalTimeoutConstant` and `ReadTotalTimeoutMultiplier` members, specifies that the read operation is to return immediately with the characters that have already been received, even if no characters have been received..."

This is exactly what we want . We do NOT want the `ReadFile` to get stuck if there is no data available as we will know with `WaitCommEvent` API. and also

> "...A value of zero for both the WriteTotalTimeoutMultiplier and WriteTotalTimeoutConstant members indicates that total time-outs are not used for write operations..."

is what we want. In short we need to do this:

```
COMMTIMEOUTS timeouts;timeouts.ReadIntervalTimeout        = MAXDWORD; timeouts.ReadTotalTimeoutMultiplier    = 0;timeouts.ReadTotalTimeoutConstant    = 0;timeouts.WriteTotalTimeoutMultiplier    = 0;timeouts.WriteTotalTimeoutConstant    = 0;if (!SetCommTimeouts(m_hCommPort, &timeouts)){    ASSERT(0);    TRACE ( "CSerialCommHelper :  Error setting time-outs. %d",GetLastError());    return E_FAIL;}
```

Now we have discussed almost everything that needs to be discussed for the sake of this article.

## Putting it all together

All this I have put together in a form of two classes:

- The main class is `CSerialCommHelper` - the main class that does performs all the communication .- The helper class called `CSerialBuffer` that is an internal buffer used by the `CSerialCommHelper`.

Here is the main API of the `CSerialCommHelper`:

```
inline bool IsInputAvailable()inline bool IsConnection() {return m_abIsConnected ;}inline void SetDataReadEvent() { SetEvent ( m_hDataRx ); }HRESULT Read_N (std::string& data,long alCount,long alTimeOut);HRESULT Read_Upto (std::string& data,char chTerminator ,                   long* alCount,long alTimeOut);HRESULT ReadAvailable(std::string& data);HRESULT Write (const char* data,DWORD dwSize);HRESULT Init(std::string szPortName, DWORD dwBaudRate,BYTE byParity,             BYTE byStopBits,BYTE byByteSize);HRESULT Start();HRESULT Stop();HRESULT UnInit();
```

and the interface for `CSerialBuffer` is :

```
inline void LockBuffer(); inline void UnLockBuffer();void AddData( char ch ) ;void AddData( std::string& szData ) ;void AddData( std::string& szData,int iLen ) ;void AddData( char *strData,int iLen ) ;std::string GetData() ;void Flush();long Read_N( std::string &szData,long alCount,HANDLE& hEventToReset);bool Read_Upto( std::string &szData,char chTerm,long &alBytesRead,                HANDLE& hEventToReset);bool Read_Available( std::string &szData,HANDLE & hEventToReset);inline long GetSize() ;inline bool IsEmpty() ;
```

 Here is the logic and working behind the classes:

First of let me show you how to use the class. In your application create an object of `CSerialCommHelper` like this:

```
CSerialCommHelper m_theCommPort;
```

Call `m_theCommPort.Init()` passing in the necessary information. If you want you can use default values.\
Next call `m_theCommPort.Start()`

If you want to get notification about when the some data is available you can get the kernel event object to wait on by calling  `m_theCommPort.GetWaitForEvent()`.

What `CSerialCommHelper` does is that on call to Init(), it opens the specified COM port and also starts a thread. The thread starts "listening" for any incoming data and once the data has been received it reads  all the data into a local buffer which is of type `CSerialBuffer` . Once its done reading all the data it sets the event in case you want to get the notification. Now you have three options

- read all the data by calling `ReadAvailable` which reads all the data .- read up to some character by calling `Read_Upto` and passing character up to which you want to read.- read N character calling `Read_N` passing the numbers to be read.

There is one more thing that needs to be paid attention. If you want to read 10 characters  but there are only 5 characters in the local buffer, the read\_N makes a blocking call and waits for the timeout passed as the last parameter .  Same is true for `Read_Upto`.

One more thing. If there are 10 characters in the local buffer but you made a call to `Read_N` for 5 characters you will be returned first 5 characters. If you made a next call `Read_N` for 5 characters again, it would returned next 5 characters.

That's all there is to it.

If you think I have left something please feel free to email me at [ashishdhar@hotmail.com](mailto:ashishdhar@hotmail.com)

## Ashish Dhar

|  |  |
| --- | --- |
|  | Click [here](http://www.codeproject.com/script/profile/whos_who.asp?vt=arts&id=27) to view Ashish Dhar's online profile. **[출처]** [[펌] Serial Communication in Windows](http://blog.naver.com/ascbbs/40008043049)|**작성자** [캐럴리](http://blog.naver.com/ascbbs) |