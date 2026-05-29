---
title: "NFS(Network File System)구축하기"
excerpt_separator: "<!--more-->"
date: 2012-01-31 15:17:51 +0900
categories:
  - robot
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

인터넷의 바다에 용자들은 참 많다는걸 느끼네요..

**1. NFS(Network File System)이란?**

(1) 개요: NFS는 Sun Microsystems사에서 개발된 것으로 TCP/IP네트워크 상에서 다른 컴퓨터의 파일\
          시스템을 마운트하고 공유하여 상대방의 파일 시스템 일부를 마치 자기 자신의 디렉토리인\
          것처럼 사용할 수 있게 해준다. NFS가 사용되는 주된 이유는 고용량의 하드디스크를 탑재\
          하고 있는 몇 대의 워크스테이션이 네트워크의 다른 컴퓨터들에게 파일 시스템서비스를 \
          해줌으로써 모든 컴퓨터들이 필요 이상의 자원을 가질 필요도 없고 소프트웨어를 여기저기\
          깔아둘 필요도 없어지게 된다. NFS는 사용이 편리한 대신에 보안에 상당히 미약하기 때문\
          에 주의해서 사용해야 한다.\
(2) NFS서버의 설정\
   1) 설명: NFS서버는 언제든지 클라이언트가 마운트할 수 있도록 준비되어 있어야 하며 NFS는 rpc.\
          mountd와 rpc.nfsd 두 데몬을 가지고 있다. 참고로 /etc/rc.d/init.d/nfs 스트립트를 실행\
          하면 이 두 데몬을 실행시킨다.\
   2) 관련데몬: NFS를 사용하기 위해서는 다음과 같은 데몬을 실행시켜야 한다.\
     ㄱ. netfs: 삼바, NFS, NCP등을 마운트하거나 언마운트해주는 데몬으로 NFS 서버데몬을 띄우기\
               전에 미리 실행시켜야 한다.\
     ㄴ. nfs: NFS서비스를 해주는 데몬이다. 참고로 이 데몬을 실행시키려면 먼저 /etc/exports\
             파일을 설정해야 한다.\
     ㄷ. portmap: RPC(Remote Procedure Call)연결에 관여하는 데몬으로 NFS, NIS을 사용할 때 필\
                 요함으로 실행시켜야 한다.\
     ㄹ. nfslock: 파일잠금을 제공하는데 이것은 동시에 여러 사람들이 동일한 파일을 수정하는 \
                 것을 막는다.\
    (참고) 데몬의 확인\
          [root@www init.d]# rpcinfo -p\
             program vers proto   port\
              100000    2   tcp    111  portmapper\
              100000    2   udp    111  portmapper\
              100024    1   udp  32768  status\
              100024    1   tcp  32768  status\
              100011    1   udp   1022  rquotad\
              100011    2   udp   1022  rquotad\
              100005    1   udp  32771  mountd\
              100005    1   tcp  32769  mountd\
              100005    2   udp  32771  mountd\
              100005    2   tcp  32769  mountd\
              100005    3   udp  32771  mountd\
              100005    3   tcp  32769  mountd\
              100003    2   udp   2049  nfs\
              100003    3   udp   2049  nfs\
              100021    1   udp  32772  nlockmgr\
              100021    3   udp  32772  nlockmgr\
              100021    4   udp  32772  nlockmgr\
            => (설명)\
              rpc.mountd: 외부의 요청에 반응하는 NFS 마운트 프로토콜이다. 클라이언트들이 서버\
                         를 이용할 수 있도록 디렉토리를 공유해주는 역할을 한다. NFS는 접속해제\
                         방식으로 웹서버처럼 접속요청이 있을 때만 연결이 이루어진다.\
              rpc.nfsd: 서버측에서 rpc.mountd에 의해 마운트되면 클라이언트는 rpc.nfsd로 서버에\
                       요구를 보내게 된다. 이 요구를 처리하는 주 데몬이다.\
              rpc.lockd: 파일잠금을 제공하는데 이것은 동시에 여러 사람들이 동일한 파일을 수정\
                        하는 것을 막는다.\
              rpc.statd: rpc.lockd와 함께 작동하면 NFS서버가 비정상적으로 종료되었거나 리부팅\
                        했을 경우 복구하는 역할을 한다.\
              rpc.rquotad: 원격 쿼터 서버로서 NFS서버의 파일 시스템을 마운트한 로컬 유저의 쿼\
                          터에 넘겨준다.\
          [root@www /]# ps ax\
            => rpc.mountd와 nfsd 데몬이 실행중인지 확인하면 된다.\
   2) 관련파일: /etc/exports\
     ㄱ. 개요: 파일에 마운트를 허가할 디렉토리와 마운트를 허가할 호스트 목록을 설정한다.\
     ㄴ. /etc/exports에서 사용가능한 옵션들\
        root\_squash: 클라이언트에서 루트를 서버상에 nobody사용자로 매핑한다. \
        no\_root\_squash: 서버와 클라이언트 모두 같은 루트(root)를 사용한다. 즉 클라이언트에서의\
                       root의 요청을 서버의 root로 매핑한다.\
        ro: 파일 시스템을 읽기 전용(read only)로 마운트한다.\
        rw: 파일 시스템을 읽고 쓸수 있도록 마운트한다.(read write)\
        insecure: 인증되지 않은 접근도 가능하도록 한다.'\
        link\_relative: 심볼릭 링크를 상대 심볼릭 링크로 바꿀 때 사용한다.\
        noaccess: 지정된 디렉토리에는 접근을 금지한다. 특정시스템에 대한 공유 디렉토리 일부를\
                 접근 못하게 할 경우에 사용한다.\
        (참고) 가장 많이 나오는 옵션으로 root\_squash와 no\_root\_squash가 있는데 squash라는 뜻이\
              '짓누르다', '억압하다'라는 뜻이 있다. 따라서 root\_squash라는 것은 root를 짓누르\
              다. 즉 root사용자를 무시한다는 뜻이고 no\_root\_squash는 그 반대의 뜻이다.\
     ㄷ. /etc/exports파일의 예\
        /home/ftp/pub \*.sample.com(ro)\
        / master(rw) trusty(rw,no\_root\_squash)\
        /projects proj\*.local.domain(rw)\
        /data 192.168.0.0/255.255.255.0(ro)\
        /work 192.168.0.2(rw)\
        /pub \*(ro, insecure, root\_squash)\
        /pub/private \*.social.com(noaccess)\
         => (설명)\
           1. /home/ftp/pub디렉토리를 sample.com도메인을 사용하는 모든 사용자가 읽는 것을 허용\
             한다.\
           2. /디렉토리를 master, trusty호스트가 읽기/쓰기를 허용한다.\
           3. 도메인이름이 local.domain이고 호스트이름이 proj로 시작하는 호스트에 대해서 /pro\
             jects라는 디렉토리로 읽기/쓰기를 허용한다.\
           4. 네트워크주소가 192.168.0 대역에 속한 모든 호스트에 대해서 data디렉토리를 읽기만\
             허용한다.\
           5. /work 디렉토리를 192.168.0.2 호스트만 read and write권한으로 설정한다.\
           6. /pub디렉토리에 읽기전용으로 마운트할 수 있고, 인증없이 마운트가 가능하며 마운트하\
             는 모든 컴퓨터의 루트를 서버에서 nobody로 접근할 수 있게한다.\
           7. /pub/private디렉토리는 social.com에 해당하는 시스템은 접근할 수 없다.\
   3) NFS 서버설정예\
     ㄱ. /etc/exports에 설정한다.\
       예) /data 192.168.3.220/255.255.255.0(rw)\
     ㄴ. 데몬을 띄운다. (netfs, portmap, nfs)\
       예) /etc/rc.d/init.d/nfs start\
     ㄷ. 해당디렉토리의 퍼미션을 푼다.\
       예) chmod 777 /data\
(3) NFS 클라이언트에서 사용하기\
   1) 설명: 클라이언트에서는 mount명령을 이용하여 NFS서버의 파일시스템을 이용할 수 있다. \
   2) 마운트형식\
     ㄱ. 명령행에서 마운트하기\
        mount -t nfs NFS-서버:Exported디렉토리 마운트포인트\
     ㄴ. /etc/fstab에 정의하여 부팅시마다 사용하기\
        NFS-서버:/디렉토리   마운트포인트   nfs   options 0 0\
   3) 사용예\
     ㄱ. mount -t nfs nfs.linux.co.kr:/usr/local /usr/local\
        => nfs.linux.co.kr의 /usr/local디렉토리를 자신의 /usr/local디렉토리에 nfs파일시스템\
          타입으로 마운트한다.\
     ㄴ. mount -t nfs 203.247.xxx.100:/data /pds\
        => 203.247.xxx.100의 /data라는 디렉토리를 현재 시스템의 /pds라는 디렉토리로 마운트한\
          다.\
     ㄷ. /etc/fstab을 이용한 nfs마운트 설정예\
        nfs.linux.co.kr:/usr/local   /usr/local   nfs   timeo=15,intr   0 0\
         => nfs.linux.co.kr의 /usr/local디렉토리를 자신의 /usr/local디렉토리에 nfs파일시스템\
           으로 마운트한다. 타임아웃시간을 1.5초로 하고, 파일시스템을 인터럽트할 수 있는 옵션\
           을 설정하였다. \
       (참고) /etc/fstab에 사용되는 NFS마운트관련옵션\
             timeo=n : RPC 타임아웃이 발생되고 나서 첫번째 재전송 요구를 보낼때 사용되는 시간\
                      기본값은 7(1/10초)\
             intr : 주 타임아웃이 발생되었을 때 신호를 보내 NFS호출을 인터럽트한다.\
             rsize=n : NFS서버로부터 읽어들이는 바이트 수 지정. 기본값은 1024바이트\
             wsize=n : NFS서버에 쓰기할 때 사용하는 바이트 수지정. 기본값은 1024바이트\
             retrans=n : 주타임아웃을 발생시키는 부타임아웃을 재전송 회수 기본값은 3번의 타임\
                        아웃\
             port=n : NFS서버와 연결할 수 있는 포트번호 지정\
             fg : 첫번째 NFS마운트 시도에서 타임아웃이 발생되면 즉시 중단함. 기본값\
             hard : 주타임아웃이 발생되면 server not responding을 출력하고 무한정 재시도\
             soft : 주타임아웃이 발생되면 프로그램에게 I/O 에러 보고\
   4) 참고: NFS를 사용가능한 지 클라이언트 점검\
    ㄱ. 설명: 클라이언트에서 NFS를 사용하기 위해서는 파일시스템에서 nfs를 지원하는지 점검해야\
             한다.\
    ㄴ. 확인하기\
       **[root@www root]# cat /proc/filesystems**\
       nodev   rootfs\
       nodev   bdev\
       nodev   proc\
       nodev   sockfs\
       nodev   tmpfs\
       nodev   shm\
       nodev   pipefs\
               ext2\
       nodev   ramfs\
               iso9660\
       nodev   devpts\
               ext3\
       nodev   usbdevfs\
       nodev   usbfs\
       nodev   autofs\
       nodev   binfmt\_misc\
       nodev   nfs             // 이 부분이 있어야 한다.\
        => /proc/filesystems는 현재 시스템에서 지원하는 파일시스템 목록을 담고 있는 파일이다.\
    ㄷ. 나타나지 않는 경우\
       modprobe nfs \
        => nfs를 모듈로 올린다.\
    ㄹ. 모듈로 올릴 때 에러가 발생하는 경우\
       커널에서 nfs모듈이 설정되지 않은 경우이다. '/usr/src/리눅스버전'디렉토리로 이동한 뒤에\
      make menuconfig를 실행하여 File sysmtem => Network Filesystem => NFS Client 관련항목을\
      선택하고 재컴파일한다.\
(4) 관련명령어\
   1) exportfs\
     ㄱ. 설명: NFS에서 익스포트된 리스트를 보여준다.\
     ㄴ. 사용법\
        exportfs [option]\
     ㄷ. 옵션\
        -v : 익스포트된 리스트를 자세히 보여준다.\
        -r : 익스포트된 내역을 다시 읽어들인다.\
     ㄹ. 사용예\
        a. [root@www /]# exportfs\
           /data           192.168.0.3/255.255.255.0\
        b. [root@www /]# exportfs -v\
           /data           192.168.0.3/255.255.255.0(rw,async,wdelay,root\_squash)\
        c. [root@www /]# exportfs -ar\
             => 현재 설정된 내역이나 변경된 내역을 다시 읽어들인다.\
   2) showmount\
     ㄱ. 설명: NFS서버의 마운트된 정보를 보여준다.\
     ㄴ. 사용법\
        showmount [option]\
     ㄷ. option\
        -a : host:dir 형태로 출력한다.\
        -e : 익스포트리스트를 보여준다.\
     ㄹ. 사용예\
        a. [root@www /]# showmount\
           Hosts on www:\
           192.168.0,3\
        b. [root@www /]# showmount -a\
           All mount points on www:\
           192.168.0.3:/data\
        c. [root@www /]# showmount -e\
           Export list for www:\
           /data 192.168.0.3/255.255.255.0\

출처 : 대전국제IT교육센터 정성재 강사