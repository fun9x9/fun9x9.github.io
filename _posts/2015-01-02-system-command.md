---
title: "운영체제 별 시스템 명령어 (System command by OS)"
date: 2015-01-02T11:03:00+09:00
categories:
  - IT
tags:
  - system
  - 시스템
  - command
  - 명령어
  - Unix
  - AIX
  - HP-UX
  - Linux
  - RHEL
  - SunOS
---

|구분|AIX|HP_UX|SunOS|Linux(RHEL)|
|:---:|:---:|:---:|:---:|:---:|
|hostname|echo $HOSTNAME||||
|server<br>manufacturer|/usr/bin/prtconf|parstatus|prtconf|dmesg\|grep "DMI:"|
|server<br>model|/usr/sbin/prtconf<br>uname -M|model<br>parstatus|prtconf<br>uname -i| dmesg\|grep "DMI:"|
|os|uname -a||||
|os<br>version|/usr/bin/oslevel<br>oslevel -s|uname -a|uname -a|cat /etc/redhat-release<br>uname -a<br>cat /proc/sys/kernel/osrelease|
|cpu<br>count|/usr/sbin/prtconf|parstatus|psrinfo -p<br>kstat cpu_info\|grep chip_id\|uniq|(grep -c ^processor /proc/cpuinfo)/CPU_CORE_COUNT|
|cpu core<br>count|nmon|glance (c)<br>ioscan -fknC processor|(kstat cpu_info\|grep core_id\|uniq)/CPU_COUNT|cat /proc/cpuinfo\|grep "cpu cores"|
|cpu<br>speed|/usr/sbin/prtconf<br>nmon|parstatus -V -c 0|psrinfo -v|cat /proc/cpuinfo|
|mem<br>size|/usr/sbin/prtconf|glance (m)|prtconf|free -m<br>cat /proc/meminfo|
|ip(network)|netstat -in|||ifconfig<br>pifconfig<br>ip addr|
|disk|df|bdf|df||

- HP-UX의 경우 관리자 권한이 필요하거나 glance가 깔려 있지 않은 경우 사용 못할 수 있음.
