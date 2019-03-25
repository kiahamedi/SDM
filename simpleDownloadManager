#!/bin/bash
clearFile(){
	if [ -f /tmp/"$pid".txt ];then
		rm /tmp/"$pid".txt
	fi
}
sigmaTime(){
	local hour="$1"
	local minute="$2"
	hour=${hour#0}
	minute=${minute#0}
	if [ -z "$hour" ];then
		hour="0"
	fi
	if [ -z "$minute" ];then
		minute="0"
	fi
	echo $((hour * 60 + minute))
}
dateTime(){
	date_hour=`date +%H:%M`
	date_hour=${date_hour/:/ }
	sigma_date=`sigmaTime $date_hour`
}
startTime(){
	local time="$1"
	time=${time/:/ }
	sigma_time=`sigmaTime $time`
	dateTime
	while [ "$sigma_date" != "$sigma_time" ];do
		echo $((sigma_time - sigma_date)) "mimutes until start!"
		sleep 20
		dateTime
	done
}
downloadFunction(){
	aria2c -c -x 8 -s 8 -k 1M "$link"
	notify-send "download completed"
	echo "1" > /tmp/"$pid".txt
}
endTime(){
	local time="$1"
	time=${time/:/ }
	sigma_time=`sigmaTime $time`
	dateTime
	while [ "$sigma_date" != "$sigma_time" ];do
		if [ -f /tmp/"$pid".txt ];then
			exit
		fi
		sleep 20
		dateTime
	done
	killall aria2c
}
clear
trap clearFile exit
pid="$$"
while getopts "s:e:l:v" options;do
	case "$options" in 
		s)
			start_time_input=${OPTARG} ;;
		e)
			end_time_input=${OPTARG} ;;
		l)
			link=${OPTARG} 
			if [ -z "$link" ];then
				echo "Error! Please enter a link"
				exit
			fi
			;;
		v)
			echo "Simple Download Manager"
			echo "version 1.0.0"
			exit
			;;
			
	esac
done
if [ ! -z "$start_time_input" ];then
	startTime $start_time_input
fi
if [ ! -z "$end_time_input" ];then
	downloadFunction &
	endTime $end_time_input
else
	downloadFunction
fi
exit
