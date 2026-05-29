---
title: "linux ipc (shared memory)"
excerpt_separator: "<!--more-->"
date: 2020-09-01 05:36:17 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

linux app 간 통신이 필요해서 통신 방법을 찾다보니. .일단.. shared memory방식이 있어서 정리 합니다.

의외로 구현도 쉽고 확인도 쉽네요 ^^;

sample code : 5678 키로 만들고 해당 키로 만든 shared memory에 쓰고 읽어가는 코드 입니다.

1 host.c +

1. #include <sys/ipc.h>
2. #include <sys/shm.h>
3. #include <string.h>
4. #include <unistd.h>
5. #include <stdlib.h>
6. #include <stdio.h>
7. int main(int argc, char \*\*argv)
8. {
9. int shmid;
10. void \*shared\_memory = (void \*)0;
11. int skey = 5678;
12. int \*write\_shm;
13. // 공유메모리 공간을 만든다.
14. shmid = shmget((key\_t)skey,
15. sizeof(int), 0666|IPC\_CREAT);
17. if(shmid == -1)
18. {
19. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("shmget failed : ");
20. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
21. }
23. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("Key %x\n", skey);
25. // 공유메모리를 맵핑한다.
26. shared\_memory = shmat(shmid, (void \*)0, 0);
27. if(!shared\_memory)
28. {
29. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("shmat failed : ");
30. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
31. }
33. write\_shm = (int \*)shared\_memory;
34. \*write\_shm = 100;
35. }

1 client.c

1. #include <sys/ipc.h>
2. #include <sys/shm.h>
3. #include <string.h>
4. #include <unistd.h>
5. #include <stdlib.h>
6. #include <stdio.h>
8. int main(int argc, char \*\*argv)
9. {
10. int shmid;
11. int skey = 5678;
12. int \*shared\_memory;
14. shmid = shmget((key\_t)skey, sizeof(int), 0666);
16. if(shmid == -1)
17. {
18. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("shmget failed\n");
19. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
20. }
21. shared\_memory = shmat(shmid, (void \*)0, 0);
23. if(!shared\_memory)
24. {
25. [perror](http://www.opengroup.org/onlinepubs/009695399/functions/perror.html)("shmat failed : ");
26. [exit](http://www.opengroup.org/onlinepubs/009695399/functions/exit.html)(0);
27. }
28. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("shm id : %d\n", shmid);
30. while(1)
31. {
32. int num = \*shared\_memory;
33. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("key[%d] : read shm data : %d\n", skey, num);
34. sleep(1);
35. }
36. }

shared memory 할당 확인

$ipcs -m

1. pi@raspberrypi:~ $ ipcs -m
2. ------ Shared Memory Segments --------
3. key        shmid      owner      perms      bytes      nattch     status
4. 0x00000000 163840     pi         600        393216     2          dest
5. 0x00000000 1033994241 pi         600        393216     2          dest
6. 0x00000000 327682     pi         600        393216     2          dest
7. **0x0000162e 1892384771 pi         666        4          0**
8. 0x00000000 517242885  pi         600        393216     2          dest
9. 0x00000000 1908146182 pi         600        524288     2          dest
10. 0x00000000 280428551  root       777        1228800    2          dest
11. 0x00000000 1854996488 root       600        3686400    1          dest

ref : <https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch08_IPC>