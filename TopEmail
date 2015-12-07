#!/bin/bash
# Author: Muhamed Hamid (lORD OF WAR)

movetocol() {
  local coln=$1
  local colwidth=12
  local col=`echo ${coln}*${colwidth} | bc`
  echo -en "\\033[${col}G"
}

cwd=`grep 'cwd=/home' /var/log/exim_mainlog`
topusers=( $(echo "${cwd}" | awk '{print $3}' | cut -d / -f 3 | sort -bg | uniq -c | sort -bg | awk '{print $2}' | tail -5) )
totals=( $(echo "${cwd}" | awk '{print $3}' | cut -d / -f 3 | sort -bg | uniq -c | sort -bg | awk '{print $1}' | tail -5) )

echo -n `tput bold`

col=1
for user in ${topusers[@]} ; do
    movetocol $col
    echo -n "${user}"
    col=`echo ${col}+1 | bc`
done

echo `tput sgr0`

for i in `seq 0 23` ; do
  HOUR=`echo ${i}+$(date +%H)+1 | bc`
  HOUR=$(printf %02d `echo ${HOUR}%24 | bc`)
  
  thishour=$(echo "${cwd}" | grep "${HOUR}:.*.* cwd")
  
  echo -n "`tput bold`${HOUR}:00`tput sgr0`"
  col=1
  for user in ${topusers[@]} ; do
      movetocol $col
      count=$(echo "${thishour}" | grep ${user} | wc -l)
      echo -n "${count}"
      col=`echo ${col}+1 | bc`
  done
  echo ""
done

echo -n "`tput bold`TOTAL`tput sgr0`"
col=1
for total in ${totals[@]} ; do
    movetocol $col
    echo -n "${total}"
    col=`echo ${col}+1 | bc`
done
echo ""
