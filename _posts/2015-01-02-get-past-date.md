---
title: "과거 날짜 구하기(get_past_date.sh)"
date: 2015-01-02T13:44:00+09:00
categories:
  - IT
tags:
  - tips
  - sh
  - shell
  - past
  - date
  - script
  - AIX
  - HP-UX
  - SunOS
  - Linux
  - RHEL
  - Ubuntu
---

- Tested Environment : AIX, HP-UX, SunOS, Linux(RHEL, Ubuntu)

```sh
# ===========================< 과거날짜 구하기 >==================================
# input  - Number ex) 1 : 하루전, 7 : 7일전 
# output - Date (형식 : YYYYMMDD)
# --------------------------------------------------------------------------------
# 20080408 작성
# 20110105 IBM에서 "201012 31" 이런식으로 아웃풋 내던것 수정
# 20110105 Linux(Ubuntu)에서 tail +3 안되던것 수정 tail -n +3
# ================================================================================

# --< Variable >------------------------------------------------------------------
MON=`date +%m`
DAY=`date +%d`
YEAR=`date +%Y`

# --< 날짜 구하기 >---------------------------------------------------------------
if [[ $# -ne 1 ]]
then 
    echo "사용법 : get_past_date.sh 1"
    exit 1
fi

i=0
while [[ i -lt $1 ]]
do
    if [[ "$MON" = "01" ]] && [[ "$DAY" = "01" ]]
    then
        MON=12
        (( YEAR=10#$YEAR - 1 ))
        DAY=$((`cal $MON $YEAR |tail -n +3 |wc -w`))
    elif [[ "$DAY" = "01" ]] && [[ "$MON" != "01" ]]
    then
        (( MON=10#$MON - 1 ))
        if [[ $MON -lt 10 ]]
        then
            MON=0$MON
        fi
        DAY=$((`cal $MON $YEAR |tail -n +3 |wc -w`))
        (( DAY=10#$DAY - 1 + 1 ))
        if [[ $DAY -lt 10 ]]
        then
            DAY=0$DAY
        fi
    else
        (( DAY=10#$DAY - 1 ))
        if [[ $DAY -lt 10 ]]
        then
            DAY=0$DAY
        fi
    fi

    (( i = $i + 1 ))
done

WORK_DATE=$YEAR$MON$DAY
echo $WORK_DATE
```