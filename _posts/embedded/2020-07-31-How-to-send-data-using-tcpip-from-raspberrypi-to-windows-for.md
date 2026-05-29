---
title: "How to send data using tcp/ip from raspberrypi to windows form"
excerpt_separator: "<!--more-->"
date: 2020-07-31 05:35:59 +0900
categories:
  - embedded
tags:
  - travel
  - 여행

toc : true
toc_sticky : true
---

어디에서 어디를 호출할지. ^^; 그림 좀 그려보고 짜보자구

server - [link](https://www.geeksforgeeks.org/socket-programming-cc/) [link2](https://www.geeksforgeeks.org/tcp-server-client-implementation-in-c/) ([enrol\_method](https://dev-ahn.tistory.com/110)) ([nonblocking\_mode](https://velog.io/@jyongk/TCP-Socket-Blocking-Non-Blocking))

-----------------------------------------------------------------------

(noblocking server side code)\

1. #include <unistd.h>
2. #include <stdio.h>
3. #include <sys/socket.h>
4. #include <stdlib.h>
5. #include <netinet/in.h>
6. #include <string.h>
7. #include <fcntl.h>
8. #define PORT 8080
10. int main(int argc, char const \*argv[])
11. {
12. int server\_fd, new\_socket, valread;
13. struct sockaddr\_in address;
14. int opt = 1;
15. int addrlen = sizeof(address);
16. char buffer[1024] = {0};
17. char \*hello = "Hello from server";
19. //Creating socket file descriptor
20. if((server\_fd = socket(AF\_INET, SOCK\_STREAM, 0)) == 0 )
21. {
22. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("socket failed");
23. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(EXIT\_FAILURE);
24. }
26. if(setsockopt(server\_fd, SOL\_SOCKET, SO\_REUSEADDR | SO\_REUSEPORT, & opt, sizeof(opt)))
27. {
28. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("setsockopt");
29. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(EXIT\_FAILURE);
30. }
32. address.sin\_family = AF\_INET;
33. address.sin\_addr.s\_addr = INADDR\_ANY;
34. address.sin\_port = htons(PORT);
36. if(bind(server\_fd, (struct sockaddr \*)&address, sizeof(address))<0)
37. {
38. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("bind failed");
39. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(EXIT\_FAILURE);
40. }
41. if(listen(server\_fd, 3)< 0)
42. {
43. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("listen");
44. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(EXIT\_FAILURE);
45. }
46. if((new\_socket = accept(server\_fd, (struct sockaddr \*)&address, (socklen\_t\*)&addrlen))<0)
47. {
48. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("server accept failed....\n");
49. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
50. }
52. int flag = fcntl(new\_socket, F\_GETFL, 0);
53. fcntl(new\_socket, F\_SETFL, flag | O\_NONBLOCK);
55. int read\_size = 0;
56. while(1)
57. {
59. read\_size = read(new\_socket, buffer, 1024);
60. if(read\_size > 0 )
61. {
62. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("%s\n",buffer);
63. write(new\_socket, hello, [strlen](http://www.opengroup.org/onlinepubs/009695399/functions/strlen.html)(hello));
64. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("Hello message sent\n");
65. }
66. }
67. return 0;
68. }

-----------------------------------------------------------------------

client winform c# - [link](https://foxlearn.com/articles/chat-tcp-ip-client-server-in-csharp-98.html)

-----------------------------------------------------------------------

1. using System;
2. using System.Collections.Generic;
3. using System.ComponentModel;
4. using System.Data;
5. using System.Drawing;
6. using System.Linq;
7. using System.Text;
8. using System.Threading.Tasks;
9. using System.Windows.Forms;
10. using System.Net;
11. using System.Net.Sockets;
12. using SimpleTCP;
14. namespace tcp\_ip\_client\_test
15. {
16. public partial class Form1 : Form
17. {
18. public Form1()
19. {
20. InitializeComponent();
21. }
23. SimpleTcpClient client;
25. private void button1\_Click(object sender, EventArgs e)  //connect
26. {
27. connectBtn.Enabled = false;
28. //Connect to server
29. client.Connect(ttxtHost.Text, Convert.ToInt32(txtPort.Text));
31. }
32. private void Form1\_Load(object sender, EventArgs e)
33. {
34. client = [new](http://www.google.com/search?q=new+msdn.microsoft.com) SimpleTcpClient();
35. client.StringEncoder = Encoding.UTF8;
36. client.DataReceived += Client\_DataReceived;
37. }
38. private void Client\_DataReceived(object sender, SimpleTCP.Message e)
39. {
40. //Update message to txtStatus
41. txtStatus.Invoke((MethodInvoker)delegate ()
42. {
43. txtStatus.Text += e.MessageString;
44. });
45. }
47. private void button2\_Click(object sender, EventArgs e)  //send
48. {
49. client.WriteLineAndGetReply(txtMessage.Text, TimeSpan.FromSeconds(3));
50. }
52. }
53. }

-----------------------------------------------------------------------

[socket\_flow](https://www.geeksforgeeks.org/socket-programming-cc/) [socket\_flow\_2](https://www.cs.dartmouth.edu/~campbell/cs50/socketprogramming.html)

[link1](https://www.hackster.io/arioobarzan/connect-raspberry-pi-and-pc-with-tcp-ip-using-csharp-a299ed) [link1-1](http://www.science.smith.edu/dftwiki/index.php/Tutorial:_Client/Server_on_the_Raspberry_Pi) [link1-2](https://www.monocilindro.com/2017/03/22/how-to-run-a-c-tcpip-server-on-raspberry-pi/)

[link2](https://ultrakid.tistory.com/14)

[fixed ip setting guide](https://wikidocs.net/64942)