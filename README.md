#!/bin/bash

#known MAC addresses
KNOWN_MAC="./known_devices.txt" 

#nmap scan results
scan_results="./scan_results.txt"

#run nmap scan
scan_network() {
    nmap -sn 192.168.1.0/24 | awk '/MAC Address:/{print$3}' > $scan_results
}

#compare results
new_macs=$(grep -Fvx -f $KNOWN_MAC $scan_results)

#display alert with zenity
if [ -n "$new_macs" ]; then
    zenity --info --text="New MAC addresses discovered:\n$scan_results\n\nNew MAC addresses discovered:\n$new_macs"
    else
    zenity --info --text="No new devices discovered"
fi

echo "complete" 
