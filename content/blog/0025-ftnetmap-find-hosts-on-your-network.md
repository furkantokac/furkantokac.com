---
title: "FTnetmap : Find Hosts On Your Network"
date: "2015-07-08T10:29:39+03:00"
thumbnail: "/img/0026-ftnetmap-aginizdaki-hostlari-bulun.png"
categories: ["Python"]
tags: ["python", "english"]
url: "ftnetmap-find-hosts-on-your-network"
---

It collects information about hosts on your network. It has made to be fast. Port scanning will be added soon.

Github : <a href="https://github.com/furkantokac/FTnetmap">https://github.com/furkantokac/FTnetmap
</a>Dowload : <a href="https://github.com/furkantokac/FTnetmap/archive/master.zip">https://github.com/furkantokac/FTnetmap/archive/master.zip</a>
<h2>FTnetmap Version 1.0 (10 December 2015)</h2>
<strong>BUGS:</strong>
No reported.

<strong>UNDER DEVELOPMENT:</strong>
-Port scan.

<strong>CURRENT FEATURES:</strong>
-Search alive hosts between 2 IPs.
-Cross-Platform.
-Export IPs to file.
-Fast.

<strong>COMING FEATURES:</strong>
-Will show more details about host.
-Will use socket for new, advanced features.

<strong>USAGE:</strong>
Usage: python FTnetmap.py -e fileName.txt -r firstIP-lastIP
This command scan alive IP's between firstIP and lastIP and export IPs to fileName.txt

-r --range - IP range that will be checked.
-v --verbose - Deactivate verbose mode.
-h --help - Help
-e --export - Write alive hosts to file.

Examples:
python FTnetmap.py -r 192.168.1.0-192.168.2.0
python FTnetmap.py -v -r 192.168.1.0-192.168.1.255
python FTnetmap.py -e fileName.txt -r 192.168.1.0-192.168.1.255
