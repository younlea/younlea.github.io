---
title: "char device driver sample."
excerpt_separator: "<!--more-->"
date: 2020-06-10 17:00:04 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

block device driver source code

```
#include <linux/init.h>

#include <linux/kernel.h>

#include <linux/module.h>

#include <linux/fs.h>

#include <linux/uaccess.h>

#include <linux/slab.h>

#define DRV_NAME "thermal_buffer"

#define  BUFF_SIZE      1024

#define  MAJOR_NUMBER   255

static char *buffer  = NULL;

static int my_open( struct inode *inode, struct file *filp  )

{

printk( "[sulac] opened\n"  );

return 0;

}

static int my_release( struct inode *inode, struct file *filp  )

{

printk( "[sulac] released\n"  );

return 0;

}

static ssize_t my_write( struct file *filp, const char *buf, size_t count, loff_t *f_pos  )

{

int i =0;

int sz_data = count;

printk( "[sulac] write to buffer \n");

printk( "[sulac] write to buffer - before count [%ld]\n", count );

printk("%d \n", copy_from_user(buffer, buf, sz_data ));

printk( "[sulac] write to buffer - after\n" );

return count;

}

static ssize_t my_read( struct file *filp, char *buf, size_t count, loff_t *f_pos  )

{

int sz_data = 0;

printk( "[sulac] read from buffer [%ld]\n", count  );

printk("%s \n ",buffer);

copy_to_user( buf, buffer, 1024 );

return sz_data;

}

static struct file_operations vd_fops = {

.read = my_read,

.write = my_write,

.open = my_open,

.release = my_release

};

int __init my_init( void  )

{

register_chrdev( MAJOR_NUMBER, DRV_NAME, &vd_fops  );

buffer = (char*) kmalloc( BUFF_SIZE, GFP_KERNEL  );

memset( buffer, 0, BUFF_SIZE );

if(buffer == NULL)

printk("Could not alloc \n");

printk( "[sulac] initialized\n" );

return 0;

}

void __exit my_exit( void  )

{

unregister_chrdev( MAJOR_NUMBER, DRV_NAME );

kfree( buffer  );

printk( "[sulac] exited\n" );

}

module_init( my_init  );

module_exit( my_exit  );

MODULE_LICENSE( "GPL"  );
```

application source code.

------------------------------------------------------------------------------

1. #include <sys/types.h>
2. #include <sys/stat.h>
3. #include <fcntl.h>
4. #include <unistd.h>
5. #include <stdio.h>
6. #include <math.h>

10. int main(void)
11. {
12. int fd,ret;
13. ret = 0;
14. unsigned char buf[]="eofeofeof";
15. unsigned char readbuf[10]={0,};
16. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("app start\n");
18. fd = open("/dev/thermal\_buffer",O\_RDWR);
19. if(fd < 0)
20. {
21. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("fd file open fail\n");
22. return -1;
23. }
25. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("write data [%s] write result[%ld] \n", buf, write(fd, buf, 10));
26. //write(fd,&buf,1);
27. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("read result[%ld]  ", read(fd, readbuf, 10));
28. [printf](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)("read data [%s]\n", readbuf);
29. //read(fd,&buf,1);
30. //printf("ioctl %d \n", ioctl(fd,1,2));
31. //ioctl(fd,1,2);
32. //printf("close fd [%d] \n", close(fd));
33. close(fd);
34. return ret;
35. }

------------------------------------------------------------------------------

Makefile

------------------------------------------------------------------------------

1. obj-m :=thermal\_driver.o
2. KDIR :=/lib/modules/$(shell uname -r)/build
3. PWD := $(shell pwd)
4. default:
5. $(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules
6. gcc -o test\_app application.c
7. clean:
8. rm -rf \*.order \*.symvers \*.ko \*.mod.\* \*.o test\_app

------------------------------------------------------------------------------

build

- make

test 절차

1. device driver node 만들기

sudo touch /dev/thermal\_buffer

mknod /dev/thermal\_buffer b 250 0( /dev폴더에 (주 250/부 0 )번호 thermal\_buffer 라는 장치파일을 생성)\

sudo mknod -m 666 /dev/thermal\_buffer c 250 0  (/dev 폴더에  주250, 부 0 번호  thermal\_buffer 라는 케릭터 디바이스 드라이버 생성)

-->>  250 을 할당하기 전에 cat /proc/devices 를 열어서 사용하고 있는지 확인을 미리 해야 합니다.

2. block device drivere 올리기

2. char device drivere 올리기

sudo insmod thermal\_driver.ko

(올라간지 확인 : lsmod | grep thermal\_driver , cat /proc/devices )

3. application 설행

sudo ./test\_app

4. check

dmesg | grep sulac

1. younlea@sulac:~/temp/thermal/RamDiskDriver/test$ dmesg | grep sulac
2. [2852979.919437] [sulac] exited
3. [2856399.540554] [sulac] initialized
4. [2856489.844273] [sulac] exited
5. [2856517.427697] [sulac] initialized
6. [2856789.656291] [sulac] exited
7. [2856974.656498] [sulac] initialized
8. [2857918.200307] [sulac] exited
9. [2857924.684514] [sulac] initialized
10. [2876690.184567] [sulac] exited
11. [2876695.304778] [sulac] initialized
12. [2876709.260492] [sulac] opened
13. [2876709.260499] [sulac] read from buffer
14. [2876709.260505] [sulac] released
15. [2876723.004447] [sulac] opened
16. [2876723.004450] [sulac] write to buffer
17. [2876723.004461] [sulac] read from buffer
18. [2876723.004470] [sulac] released

5.driver내리기

sudo rmmod thermal\_driver

driver 지우기

sudo rm -rf /dev/thermal\_buffer

버퍼에 올리고 쓸꺼라 block device driver를 쓸 필요가 없어서 char device driver 로 선회 ^^;

error case : "No such device or address" 라고 나오고 open이 안된다.

해당 이슈는 device driver 가 insmode가 안되어서 나오는 이슈로 insmod 후에 확인해 보면 됩니다.

driver가 잘 올라갔는지 확인할때 cat /proc/drivers에서도 확인이 가능합니다.

Test app없이도 cat으로 file을 open 하면 dmesg로 쓴것을 확인할수 있습니다.

[참고](http://blog.naver.com/PostView.nhn?blogId=k5248&logNo=120125605262)

dmesg 실시간으로 보기가 하고 싶을듯 하여 방법을 추가합니다.

아래 두가지중 한가지를  사용하시면 됩니다.

$ tail -f /var/log/kern.log

$ watch "dmesg |tail -f"

훔.. 동작하다 보면 커널이 죽어 버리는.. 쿨럭... 테스트 해봐야 할껀 copy\_to\_user, copy\_from\_user 인데...

[참고](https://www.joinc.co.kr/w/Site/Embedded/Documents/WritingDeviceDriversInLinux)

sample code 에 있는 <asm/uaccess.h>는 빌드를 하면 에러가 난다.

<https://lkml.org/lkml/2017/9/30/189> 를 참고해 보면 linux/uaccess.h 로 바꾸면 빌드가 잘 된다고 해서 바꿔서 하니 잘된다.

copy\_to\_user, copy\_from\_user 를 쓸수 있게 된다.

[리눅스 헤더파일을 못찾아서 빌드가 안될때 참고](https://younlea.github.io/develop_env/make시-linux-header-못찾을때/)