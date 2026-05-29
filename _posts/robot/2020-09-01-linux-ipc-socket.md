---
title: "linux ipc (socket)"
excerpt_separator: "<!--more-->"
date: 2020-09-01 14:49:02 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

linux application ipc를 socket를 활용해서 제작..

서버는 소켓을 열고 받은걸 바로 다시 보내는 형태로 제작

클라이언트는 연결된 소켓을 사용해서 보내고 받는걸 처리 (read에서 계속 대기 하는 형태입니다.)

server.c

1. #include <sys/types.h>
2. #include <sys/stat.h>
3. #include <sys/socket.h>
4. #include <sys/un.h>
5. #include <unistd.h>
6. #include <stdio.h>
7. #include <stdlib.h>
8. #include <string.h>
10. #define MAXLINE 1024
11. int main(int argc, char \*\*argv)
12. {
13. int server\_sockfd, client\_sockfd;
14. int state, client\_len;
15. pid\_t pid;
17. struct sockaddr\_un clientaddr, serveraddr;
19. char buf[MAXLINE];
21. if (argc != 2)
22. {
23. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("Usage : %s [socket file name]\n", argv[0]);
24. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("예    : %s /tmp/mysocket\n", argv[0]);
25. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
27. }
29. if (access(argv[1], F\_OK) == 0)
30. {
31. unlink(argv[1]);
33. }
35. client\_len = sizeof(clientaddr);
36. if ((server\_sockfd = socket(AF\_UNIX, SOCK\_STREAM, 0)) < 0)
37. {
38. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("socket error : ");
39. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
41. }
42. bzero(&serveraddr, sizeof(serveraddr));
43. serveraddr.sun\_family = AF\_UNIX;
44. [strcpy](http://www.opengroup.org/onlinepubs/009695399/functions/strcpy.html)(serveraddr.sun\_path, argv[1]);
46. state = bind(server\_sockfd , (struct sockaddr \*)&serveraddr,
47. sizeof(serveraddr));
48. if (state == -1)
49. {
50. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("bind error : ");
51. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
52. }
54. state = listen(server\_sockfd, 5);
55. if (state == -1)
56. {
57. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("listen error : ");
58. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
59. }
61. while(1)
62. {
63. client\_sockfd = accept(server\_sockfd, (struct sockaddr \*)&clientaddr, &client\_len);
64. pid = fork();
65. if (pid == 0)
66. {
67. if (client\_sockfd == -1)
68. {
69. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("Accept error : ");
70. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
71. }
72. while(1)
73. {
74. [memset](http://www.opengroup.org/onlinepubs/009695399/functions/memset.html)(buf, 0x00, MAXLINE);
75. if (read(client\_sockfd, buf, MAXLINE) <= 0)
76. {
77. close(client\_sockfd);
78. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
79. }
80. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("receivd > %s ",buf);
81. write(client\_sockfd, buf, [strlen](http://www.opengroup.org/onlinepubs/009695399/functions/strlen.html)(buf));
82. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("send > %s ", buf);
83. }
84. }
85. }
86. close(client\_sockfd);
87. }

client.c

1. #include <sys/types.h>
2. #include <sys/stat.h>
3. #include <sys/socket.h>
4. #include <unistd.h>
5. #include <sys/un.h>
6. #include <stdio.h>
7. #include <stdlib.h>
8. #include <string.h>
10. #define MAXLINE 1024
12. int main(int argc, char \*\*argv)
13. {
15. int client\_len;
16. int client\_sockfd;                                                                                                                                                                            char buf\_in[MAXLINE];
17. char buf\_get[MAXLINE];
19. struct sockaddr\_un clientaddr;
21. if (argc != 2)
22. {
23. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("Usage : %s [file\_name]\n", argv[0]);
24. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("예    : %s /tmp/mysocket\n", argv[0]);
25. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
27. }
29. client\_sockfd = socket(AF\_UNIX, SOCK\_STREAM, 0);
30. if (client\_sockfd == -1)
31. {
32. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("error : ");
33. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
35. }
36. bzero(&clientaddr, sizeof(clientaddr));
37. clientaddr.sun\_family = AF\_UNIX;
38. [strcpy](http://www.opengroup.org/onlinepubs/009695399/functions/strcpy.html)(clientaddr.sun\_path, argv[1]);
39. client\_len = sizeof(clientaddr);
41. if (connect(client\_sockfd, (struct sockaddr \*)&clientaddr, client\_len) < 0)
42. {
43. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("Connect error: ");
44. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
46. }
47. while(1)
48. {
49. [memset](http://www.opengroup.org/onlinepubs/009695399/functions/memset.html)(buf\_in, 0x00, MAXLINE);
50. [memset](http://www.opengroup.org/onlinepubs/009695399/functions/memset.html)(buf\_get, 0x00, MAXLINE);
51. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("> ");
52. [fgets](http://www.opengroup.org/onlinepubs/009695399/functions/fgets.html)(buf\_in, MAXLINE, stdin);
53. write(client\_sockfd, buf\_in, [strlen](http://www.opengroup.org/onlinepubs/009695399/functions/strlen.html)(buf\_in));
54. read(client\_sockfd, buf\_get, MAXLINE);
55. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("-> %s", buf\_get);
56. }
58. close(client\_sockfd);
59. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
61. }

ref : <https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch08_IPC>