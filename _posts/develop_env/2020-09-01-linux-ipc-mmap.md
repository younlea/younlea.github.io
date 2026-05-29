---
title: "linux ipc (mmap)"
excerpt_separator: "<!--more-->"
date: 2020-09-01 11:19:39 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

APP간  IPC통신을 하기 위해서 mmap을 사용하는 방법을 확인해 보았습니다.

file을 하나 잡고 memory mapping해서 쓰는건데 흠.. 전 shared memory에 한표를 던지고 싶네요 ^^;

아래 예제 코드는 maker가 mm file을 만들고 메모리 메핑한 후에 파일에 쓴거고..

user는 해당 mm file을 열어서 메모리 맵핑해서 읽는겁니다.

사용법은 간단하나 만약 app 이 파일을 만들어야 하는공간이 root 이면 약간 문제가 생길듯 하네요 ^^;

maker.c

1. #include <stdio.h>
2. #include <stdlib.h>
3. #include <string.h>
4. #include <unistd.h>
5. #include <fcntl.h>
6. #include <sys/mman.h>
7. #define FLAG PROT\_WRITE | PROT\_READ
9. int main()
10. {
11. int fd, \*pmmap, i;
12. fd = open("mm", O\_RDWR|O\_CREAT, 0666);
13. if(fd < 0)
14. {
15. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("open");
16. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(1);
17. }
19. ftruncate(fd, 4096);
21. pmmap = (int \*)mmap(0, 4096, FLAG, MAP\_SHARED, fd, 0);
22. if(pmmap <0 )
23. {
24. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("mmap");
25. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(1);
26. }
28. for(i = 0; i < 100; i++)
29. {
30. pmmap[i] = i;
31. }
33. pmmap[i+1] = -1;
35. [getchar](http://www.opengroup.org/onlinepubs/009695399/functions/getchar.html)();
36. munmap(pmmap, 4096);
37. close(fd);
38. return 0;
40. }

user.c

1. #include <stdio.h>
2. #include <sys/stat.h>
3. #include <sys/mman.h>
4. #include <unistd.h>
5. #include <stdlib.h>
6. #include <fcntl.h>
7. #define FLAG PROT\_WRITE | PROT\_READ
9. int main(int argc, char \*\*argv)
10. {
11. int fd,  i=0, \*maped;
13. if ((fd = open("mm", O\_RDWR, 0666)) < 0)
14. {
15. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("File Open Error");
16. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(1);
17. }
19. --    if ((maped = (int \*) mmap(0, 4096, FLAG, MAP\_SHARED, fd, 0)) == -1)
20. {
21. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("mmap error");
22. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(1);
23. }
25. while(1)
26. {
27. if (maped[i] == -1) break;
28. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("> %d\n",maped[i]);
29. i++;
30. }
31. close(fd);
32. }

ref : <https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch08_IPC>