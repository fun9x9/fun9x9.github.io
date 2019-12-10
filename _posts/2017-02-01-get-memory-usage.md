---
title: "OS별 메모리 사용량 (memory usage by os)"
date: 2017-02-01T13:11:00+09:00
categories:
  - IT
tags:
  - tips
  - system
  - command
  - sh
  - shell
  - past
  - date
  - script
  - AIX
  - HP-UX
  - SunOS
  - Linux
---

- Tested Environment : AIX, HP-UX, SunOS, Linux(RHEL)

```sh
OS_TYPE=$(uname)

case $OS_TYPE in
    "Linux") free | grep ^Mem | awk '{printf "%3d%\n", 100*($2-$4-$6-$7)/$2}' ;;
    "HP-UX") swapinfo -tam |grep ^total |awk '{printf "%4s\n", $5}' ;;
    "SunOS") top -b 0 |grep Memory | awk '{
             TOTAL=substr($2, 0, length($2)-1);
             if ( index($2, "G") > 0 ) { TOTAL=TOTAL; }
             else if ( index($2, "M") > 0 ) { TOTAL=TOTAL/1000; }
             else if ( index($2, "K") > 0 ) { TOTAL=TOTAL/1000/1000; }
             else { TOTAL=TOTAL/1000/1000/1000; }
             FREE=substr($5, 0, length($5)-1);
             if ( index($5, "G") > 0 ) { FREE=FREE; }
             else if ( index($5, "M") > 0 ) { FREE=FREE/1000; }
             else if ( index($5, "K") > 0 ) { FREE=FREE/1000/1000; }
             else { FREE=FREE/1000/1000/1000; }
             printf "%3d%\n", 100*(TOTAL-FREE)/TOTAL }'
    ;;
    "AIX")
        if [ $(uname -v) -ge 7 ]
        then
            vmstat -v |grep computational |awk '{printf "%3d\n", $1}'
        else
            vmstat -v |egrep "memory pages|free pages|file pages" |awk '{ if (NR==1) {TOT=$1} if (NR==2|NR==3) {FREE=FREE+$1} } END { printf "%3d%\n", 100-(FREE/TOT*100) }'
        fi
        ;;
        # svmon -G -O unit=GB |grep ^memory| awk '{printf "%3d%\n", 100*$3/$2}' ;;
    *) echo "Unkwon OS Type ${OS_TYPE}"
        exit 1
    ;;
esac

exit 0
```
