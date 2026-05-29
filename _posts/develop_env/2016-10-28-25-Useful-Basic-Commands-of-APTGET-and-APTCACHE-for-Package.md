---
title: "25 Useful Basic Commands of APT-GET and APT-CACHE for Package Management"
excerpt_separator: "<!--more-->"
date: 2016-10-28 13:43:45 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

copy by below address.

<http://www.tecmint.com/useful-basic-commands-of-apt-get-and-apt-cache-for-package-management/>

This article explains how quickly you can learn to install, remove, update and search software packages using apt-get and apt-cache commands from the command line. This article provides some useful commands that will help you to handle package management in Debian/Ubuntu based systems.

[![APT-GET and APT-CACHE Commands](http://www.tecmint.com/wp-content/uploads/2013/03/apt-get-commands-300x215.png)](http://www.tecmint.com/wp-content/uploads/2013/03/apt-get-commands.png)

APT-GET and APT-CACHE Commands

##### What is apt-get?

The apt-get utility is a powerful and free package management command line program, that is used to work with Ubuntu’s APT (Advanced Packaging Tool) library to perform installation of new software packages, removing existing software packages, upgrading of existing software packages and even used to upgrading the entire operating system.

##### What is apt-cache?

The apt-cache command line tool is used for searching apt software package cache. In simple words, this tool is used to search software packages, collects information of packages and also used to search for what available packages are ready for installation on Debian or Ubuntu based systems.

APT-CACHE – 5 Useful Basic Commands

### 1. How Do I List All Available Packages?

To list all the available packages, type the following command.

```
$ apt-cache pkgnames
```

```
esseract-ocr-epopipenightdreamsmumudvbtbb-exampleslibsvm-javalibmrpt-hmtslam0.9libboost-timer1.50-devkcm-touchpadg++-4.5-multilib...
```

### 2. How Do I Find Out Package Name and Description of Software?

To find out the package name and with it description before installing, use the ‘search‘ flag. Using “search” with apt-cache will display a list of matched packages with short description. Let’s say you would like to find out description of package ‘vsftpd‘, then command would be.

```
$ apt-cache search vsftpd
```

```
vsftpd - lightweight, efficient FTP server written for securityccze - A robust, modular log coloriserftpd - File Transfer Protocol (FTP) serveryasat - simple stupid audit tool
```

To find and list down all the packages starting with ‘vsftpd‘, you could use the following command.

```
$ apt-cache pkgnames vsftpd
```

```
vsttpd
```

### 3. How Do I Check Package Information?

For example, if you would like to check information of package along with it short description say (version number, check sums, size, installed size, category etc). Use ‘show‘ sub command as shown below.

```
$ apt-cache show netcat
```

```
Package: netcatPriority: optionalSection: universe/netInstalled-Size: 30Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>Original-Maintainer: Ruben Molina <rmolina@udea.edu.co>Architecture: allVersion: 1.10-40Depends: netcat-traditional (>= 1.10-39)Filename: pool/universe/n/netcat/netcat_1.10-40_all.debSize: 3340MD5sum: 37c303f02b260481fa4fc9fb8b2c1004SHA1: 0371a3950d6967480985aa014fbb6fb898bcea3aSHA256: eeecb4c93f03f455d2c3f57b0a1e83b54dbeced0918ae563784e86a37bcc16c9Description-en: TCP/IP swiss army knife -- transitional packageThis is a "dummy" package that depends on lenny's default version ofnetcat, to ease upgrades. It may be safely removed.Description-md5: 1353f8c1d079348417c2180319bdde09Bugs: https://bugs.launchpad.net/ubuntu/+filebugOrigin: Ubuntu
```

### 4. How Do I Check Dependencies for Specific Packages?

Use the ‘showpkg‘ sub command to check the dependencies for particular software packages. whether those dependencies packages are installed or not. For example, use the ‘showpkg‘ command along with package-name.

```
$ apt-cache showpkg vsftpd
```

```
Package: vsftpdVersions: 2.3.5-3ubuntu1 (/var/lib/apt/lists/in.archive.ubuntu.com_ubuntu_dists_quantal_main_binary-i386_Packages)Description Language: File: /var/lib/apt/lists/in.archive.ubuntu.com_ubuntu_dists_quantal_main_binary-i386_PackagesMD5: 81386f72ac91a5ea48f8db0b023f3f9bDescription Language: enFile: /var/lib/apt/lists/in.archive.ubuntu.com_ubuntu_dists_quantal_main_i18n_Translation-enMD5: 81386f72ac91a5ea48f8db0b023f3f9bReverse Depends: ubumirror,vsftpdharden-servers,vsftpdDependencies: 2.3.5-3ubuntu1 - debconf (18 0.5) debconf-2.0 (0 (null)) upstart-job (0 (null)) libc6 (2 2.15) libcap2 (2 2.10) libpam0g (2 0.99.7.1) libssl1.0.0 (2 1.0.0) libwrap0 (2 7.6-4~) adduser (0 (null)) libpam-modules (0 (null)) netbase (0 (null)) logrotate (0 (null)) ftp-server (0 (null)) ftp-server (0 (null)) Provides: 2.3.5-3ubuntu1 - ftp-server Reverse Provides:
```

### 5. How Do I Check statistics of Cache

The ‘stats‘ sub command will display overall statistics about the cache. For example, the following command will display Total package names is the number of packages have found in the cache.

```
$ apt-cache stats
```

```
Total package names: 51868 (1,037 k)Total package structures: 51868 (2,490 k)Normal packages: 39505Pure virtual packages: 602Single virtual packages: 3819Mixed virtual packages: 1052Missing: 6890Total distinct versions: 43015 (2,753 k)Total distinct descriptions: 81048 (1,945 k)Total dependencies: 252299 (7,064 k)Total ver/file relations: 45567 (729 k)Total Desc/File relations: 81048 (1,297 k)Total Provides mappings: 8228 (165 k)Total globbed strings: 286 (3,518 )Total dependency version space: 1,145 kTotal slack space: 62.6 kTotal space accounted for: 13.3 M
```

APT-GET – 20 Useful Basic Commands for Package Management

### 6. How to Update System Packages

The ‘update‘ command is used to resynchronize the package index files from the their sources specified in /etc/apt/sources.list file. The update command fetched the packages from their locations and update the packages to newer version.

```
$ sudo apt-get update
```

```
[sudo] password for tecmint: Ign http://security.ubuntu.com quantal-security InRelease                      Get:1 http://security.ubuntu.com quantal-security Release.gpg [933 B]          Get:2 http://security.ubuntu.com quantal-security Release [49.6 kB]            Ign http://in.archive.ubuntu.com quantal InRelease                             Ign http://in.archive.ubuntu.com quantal-updates InRelease                     Get:3 http://repo.varnish-cache.org precise InRelease [13.7 kB]                Ign http://in.archive.ubuntu.com quantal-backports InRelease                   Hit http://in.archive.ubuntu.com quantal Release.gpg                           Get:4 http://security.ubuntu.com quantal-security/main Sources [34.8 kB]       Get:5 http://in.archive.ubuntu.com quantal-updates Release.gpg [933 B]         ...
```

### 7. How to Upgrade Software Packages

The ‘upgrade‘ command is used to upgrade all the currently installed software packages on the system. Under any circumstances currently installed packages are not removed or packages which are not already installed neither retrieved and installed to satisfy upgrade dependencies.

```
$ sudo apt-get upgrade
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DoneThe following packages have been kept back:linux-headers-generic linux-image-generic wine1.5 wine1.5-i386The following packages will be upgraded:activity-log-manager-common activity-log-manager-control-center adium-theme-ubuntu alacartealsa-base app-install-data-partner appmenu-gtk appmenu-gtk3 apport apport-gtk aptapt-transport-https apt-utils aptdaemon aptdaemon-data at-spi2-core bamfdaemon base-files bind9-host...
```

However, if you want to upgrade, unconcerned of whether software packages will be added or removed to fulfill dependencies, use the ‘dist-upgrade‘ sub command.

```
$ sudo apt-get dist-upgrade
```

### 8. How Do I Install or Upgrade Specific Packages?

The ‘install‘ sub command is tracked by one or more packages wish for installation or upgrading.

```
$ sudo apt-get install netcat
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DoneThe following extra packages will be installed:netcat-traditionalThe following NEW packages will be installed:netcat netcat-traditional0 upgraded, 2 newly installed, 0 to remove and 328 not upgraded.Need to get 67.1 kB of archives.After this operation, 186 kB of additional disk space will be used.Do you want to continue [Y/n]? yGet:1 http://in.archive.ubuntu.com/ubuntu/ quantal/universe netcat-traditional i386 1.10-40 [63.8 kB]Get:2 http://in.archive.ubuntu.com/ubuntu/ quantal/universe netcat all 1.10-40 [3,340 B]Fetched 67.1 kB in 1s (37.5 kB/s)Selecting previously unselected package netcat-traditional.(Reading database ... 216118 files and directories currently installed.)Unpacking netcat-traditional (from .../netcat-traditional_1.10-40_i386.deb) ...Selecting previously unselected package netcat.Unpacking netcat (from .../netcat_1.10-40_all.deb) ...Processing triggers for man-db ...Setting up netcat-traditional (1.10-40) ...Setting up netcat (1.10-40) ...
```

### 9. How I can Install Multiple Packages?

You can add more than one package name along with the command in order to install multiple packages at the same time. For example, the following command will install packages ‘[nethogs](http://www.tecmint.com/nethogs-monitor-per-process-network-bandwidth-usage-in-real-time/)‘ and ‘[goaccess](http://www.tecmint.com/goaccess-a-real-time-apache-and-nginx-web-server-log-analyzer/)‘.

```
$ sudo apt-get install nethogs goaccess
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... Donegoaccess is already the newest version.nethogs is already the newest version.0 upgraded, 0 newly installed, 0 to remove and 328 not upgraded.
```

### 10. How to Install Several Packages using Wildcard

With the help of regular expression you can add several packages with one string. For example, we use \*wildcard to install several packages that contains the ‘\*name\*‘ string, name would be ‘package-name’.

```
$ sudo apt-get install '*name*'
```

### 11. How to install Packages without Upgrading

Using sub ‘–no-upgrade‘ command will prevent already installed packages from upgrading.

```
$ sudo apt-get install packageName --no-upgrade
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DoneSkipping vsftpd, it is already installed and upgrade is not set.0 upgraded, 0 newly installed, 0 to remove and 328 not upgraded.
```

### 12. How to Upgrade Only Specific Packages

The ‘–only-upgrade‘ command do not install new packages but it only upgrade the already installed packages and disables new installation of packages.

```
$ sudo apt-get install packageName --only-upgrade
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... Donevsftpd is already the newest version.0 upgraded, 0 newly installed, 0 to remove and 328 not upgraded.
```

### 13. How Do I Install Specific Package Version?

Let’s say you wish to install only specific version of packages, simply use the ‘=‘ with the package-name and append desired version.

```
$ sudo apt-get install vsftpd=2.3.5-3ubuntu1
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... Donevsftpd is already the newest version.0 upgraded, 0 newly installed, 0 to remove and 328 not upgraded.
```

### 14. How Do I Remove Packages Without Configuration

To un-install software packages without removing their configuration files (for later re-use the same configuration). Use the ‘remove‘ command as shown.

```
$ sudo apt-get remove vsftpd
```

```
[sudo] password for tecmint: Reading package lists... DoneBuilding dependency tree       Reading state information... DoneThe following packages will be REMOVED:vsftpd0 upgraded, 0 newly installed, 1 to remove and 328 not upgraded.After this operation, 364 kB disk space will be freed.Do you want to continue [Y/n]? y(Reading database ... 216156 files and directories currently installed.)Removing vsftpd ...vsftpd stop/waitingProcessing triggers for ureadahead ...Processing triggers for man-db ...
```

### 15. How Do I Completely Remove Packages

To remove software packages including their configuration files, use the ‘purge‘ sub command as shown below.

```
$ sudo apt-get purge vsftpd
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DoneThe following packages will be REMOVED:vsftpd*0 upgraded, 0 newly installed, 1 to remove and 328 not upgraded.After this operation, 0 B of additional disk space will be used.Do you want to continue [Y/n]? y(Reading database ... 216107 files and directories currently installed.)Removing vsftpd ...Purging configuration files for vsftpd ...Processing triggers for ureadahead ...
```

Alternatively, you can combine both the commands together as shown below.

```
$ sudo apt-get remove --purge vsftpd
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DoneThe following packages will be REMOVED:vsftpd*0 upgraded, 0 newly installed, 1 to remove and 328 not upgraded.After this operation, 364 kB disk space will be freed.Do you want to continue [Y/n]? y(Reading database ... 216156 files and directories currently installed.)Removing vsftpd ...vsftpd stop/waitingPurging configuration files for vsftpd ...Processing triggers for ureadahead ...Processing triggers for man-db ...
```

### 16. How I Can Clean Up Disk Space

The ‘clean‘ command is used to free up the disk space by cleaning retrieved (downloaded) .deb files (packages) from the local repository.

```
$ sudo apt-get clean
```

### 17. How Do I Download Only Source Code of Package

To download only source code of particular package, use the option ‘–download-only source‘ with ‘package-name’ as shown.

```
$ sudo apt-get --download-only source vsftpd
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DoneNeed to get 220 kB of source archives.Get:1 http://in.archive.ubuntu.com/ubuntu/ quantal/main vsftpd 2.3.5-3ubuntu1 (dsc) [1,883 B]Get:2 http://in.archive.ubuntu.com/ubuntu/ quantal/main vsftpd 2.3.5-3ubuntu1 (tar) [188 kB]Get:3 http://in.archive.ubuntu.com/ubuntu/ quantal/main vsftpd 2.3.5-3ubuntu1 (diff) [30.5 kB]Fetched 220 kB in 4s (49.1 kB/s)Download complete and in download only mode
```

### 18. How Can I Download and Unpack a Package

To download and unpack source code of a package to a specific directory, type the following command.

```
$ sudo apt-get source vsftpd
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DoneNeed to get 220 kB of source archives.Get:1 http://in.archive.ubuntu.com/ubuntu/ quantal/main vsftpd 2.3.5-3ubuntu1 (dsc) [1,883 B]Get:2 http://in.archive.ubuntu.com/ubuntu/ quantal/main vsftpd 2.3.5-3ubuntu1 (tar) [188 kB]Get:3 http://in.archive.ubuntu.com/ubuntu/ quantal/main vsftpd 2.3.5-3ubuntu1 (diff) [30.5 kB]Fetched 220 kB in 1s (112 kB/s)  gpgv: Signature made Thursday 24 May 2012 02:35:09 AM IST using RSA key ID 2C48EE4Egpgv: Can't check signature: public key not founddpkg-source: warning: failed to verify signature on ./vsftpd_2.3.5-3ubuntu1.dscdpkg-source: info: extracting vsftpd in vsftpd-2.3.5dpkg-source: info: unpacking vsftpd_2.3.5.orig.tar.gzdpkg-source: info: unpacking vsftpd_2.3.5-3ubuntu1.debian.tar.gzdpkg-source: info: applying 01-builddefs.patchdpkg-source: info: applying 02-config.patchdpkg-source: info: applying 03-db-doc.patchdpkg-source: info: applying 04-link-local.patchdpkg-source: info: applying 05-whitespaces.patchdpkg-source: info: applying 06-greedy.patchdpkg-source: info: applying 07-utf8.patchdpkg-source: info: applying 08-manpage.patchdpkg-source: info: applying 09-s390.patchdpkg-source: info: applying 10-remote-dos.patchdpkg-source: info: applying 11-alpha.patchdpkg-source: info: applying 09-disable-anonymous.patchdpkg-source: info: applying 12-ubuntu-use-snakeoil-ssl.patch
```

### 19. How Can I Download, Unpack and Compile a Package

You can also download, unpack and compile the source code at the same time, using option ‘–compile‘ as shown below.

```
$ sudo apt-get --compile source goaccess
```

```
[sudo] password for tecmint: Reading package lists... DoneBuilding dependency tree       Reading state information... DoneNeed to get 130 kB of source archives.Get:1 http://in.archive.ubuntu.com/ubuntu/ quantal/universe goaccess 1:0.5-1 (dsc) [1,120 B]Get:2 http://in.archive.ubuntu.com/ubuntu/ quantal/universe goaccess 1:0.5-1 (tar) [127 kB]Get:3 http://in.archive.ubuntu.com/ubuntu/ quantal/universe goaccess 1:0.5-1 (diff) [2,075 B]Fetched 130 kB in 1s (68.0 kB/s)gpgv: Signature made Tuesday 26 June 2012 09:38:24 AM IST using DSA key ID A9FD4821gpgv: Can't check signature: public key not founddpkg-source: warning: failed to verify signature on ./goaccess_0.5-1.dscdpkg-source: info: extracting goaccess in goaccess-0.5dpkg-source: info: unpacking goaccess_0.5.orig.tar.gzdpkg-source: info: unpacking goaccess_0.5-1.debian.tar.gzdpkg-buildpackage: source package goaccessdpkg-buildpackage: source version 1:0.5-1dpkg-buildpackage: source changed by Chris Taylor <ctaylor@debian.org>dpkg-buildpackage: host architecture i386dpkg-source --before-build goaccess-0.5dpkg-checkbuilddeps: Unmet build dependencies: debhelper (>= 9) autotools-dev libncurses5-dev libglib2.0-dev libgeoip-dev autoconfdpkg-buildpackage: warning: build dependencies/conflicts unsatisfied; abortingdpkg-buildpackage: warning: (Use -d flag to override.)...
```

### 20. How Do I Download a Package Without Installing

Using ‘download‘ option, you can download any given package without installing it. For example, the following command will only download ‘nethogs‘ package to current working directory.

```
$ sudo apt-get download nethogs
```

```
Get:1 Downloading nethogs 0.8.0-1 [27.1 kB]Fetched 27.1 kB in 3s (7,506 B/s)
```

### 21. How Do I Check Change Log of Package?

The ‘changelog‘ flag downloads a package change-log and shows the package version that is installed.

```
$ sudo apt-get changelog vsftpd
```

```
vsftpd (2.3.5-3ubuntu1) quantal; urgency=low* Merge from Debian testing (LP: #1003644).  Remaining changes:+ debian/vsftpd.upstart: migrate vsftpd to upstart.+ Add apport hook (LP: #513978):- debian/vsftpd.apport: Added.- debian/control: Build-depends on dh-apport.- debian/rules: Add --with apport.+ Add debian/watch file.+ debian/patches/09-disable-anonymous.patch: Disable anonymous loginby default. (LP: #528860)* debian/patches/12-ubuntu-us-snakeoil-ssl.patch: Use snakeoil SSLcertificates and key.-- Andres Rodriguez <andreserl@ubuntu.com>  Wed, 23 May 2012 16:59:36 -0400...
```

### 22. How Do I Check Broken Dependencies?

The ‘check‘ command is a diagnostic tool. It used to update package cache and checks for broken dependencies.

```
$ sudo apt-get check
```

```
[sudo] password for tecmint: Reading package lists... DoneBuilding dependency tree       Reading state information... Done
```

### 23. How Do I Search and Build Dependencies?

This ‘build-dep‘ command searches the local repositories in the system and install the build dependencies for package. If the package does not exists in the local repository it will return an error code.

```
$ sudo apt-get build-dep netcat
```

```
The following NEW packages will be installed:debhelper dh-apparmor html2text po-debconf quilt0 upgraded, 5 newly installed, 0 to remove and 328 not upgraded.Need to get 1,219 kB of archives.After this operation, 2,592 kB of additional disk space will be used.Do you want to continue [Y/n]? yGet:1 http://in.archive.ubuntu.com/ubuntu/ quantal/main html2text i386 1.3.2a-15build1 [91.4 kB]Get:2 http://in.archive.ubuntu.com/ubuntu/ quantal/main po-debconf all 1.0.16+nmu2ubuntu1 [210 kB]Get:3 http://in.archive.ubuntu.com/ubuntu/ quantal/main dh-apparmor all 2.8.0-0ubuntu5 [9,846 B]Get:4 http://in.archive.ubuntu.com/ubuntu/ quantal/main debhelper all 9.20120608ubuntu1 [623 kB]Get:5 http://in.archive.ubuntu.com/ubuntu/ quantal/main quilt all 0.60-2 [285 kB]Fetched 1,219 kB in 4s (285 kB/s)...
```

### 24. How I Can Auto clean Apt-Get Cache?

The ‘autoclean‘ command deletes all .deb files from /var/cache/apt/archives to free-up significant volume of disk space.

```
$ sudo apt-get autoclean
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... Done
```

### 25. How I Can Auto remove Installed Packages?

The ‘autoremove‘ sub command is used to auto remove packages that were certainly installed to satisfy dependencies for other packages and but they were now no longer required. For example, the following command will remove an installed package with its dependencies.

```
$ sudo apt-get autoremove vsftpd
```

```
Reading package lists... DoneBuilding dependency tree       Reading state information... DonePackage 'vsftpd' is not installed, so not removed0 upgraded, 0 newly installed, 0 to remove and 328 not upgraded.
```

I’ve covered most of the available options with apt-get and apt-cache commands, but still there are more options available, you can check them out using ‘man apt-get‘ or ‘man apt-cache‘ from the terminal. I hope you enjoyed reading this article, If I’ve missed anything and you would like me to add to the list. Please feel free to mention in the comment below.