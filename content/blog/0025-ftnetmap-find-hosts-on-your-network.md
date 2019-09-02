---
title: "FTnetmap : Find Hosts On Your Network"
date: "2015-07-08T10:29:27+03:00"
thumbnail: "/img/0026-ftnetmap-aginizdaki-hostlari-bulun.png"
categories: ["Python"]
tags: ["Python", "English"]
url: "ftnetmap-find-hosts-on-your-network"
---

It collects information about hosts on your network. It has made to be fast. Port scanning will be added soon.

Github : [https://github.com/furkantokac/FTnetmap](https://github.com/furkantokac/FTnetmap) <br>
Dowload : [https://github.com/furkantokac/FTnetmap/archive/master.zip](https://github.com/furkantokac/FTnetmap/archive/master.zip)

##### CURRENT FEATURES:

- Search alive hosts between 2 IPs.
- Cross-Platform.
- Export IPs to file.
- Fast.

##### COMING FEATURES:

- Will show more details about host.
- Will use socket for new, advanced features.
- Port scan.

##### USAGE:

*Parameters:*
```
-r --range - IP range that will be checked.
-v --verbose - Deactivate verbose mode.
-h --help - Help
-e --export - Write alive hosts to file.
```

*Examples:*
```
python FTnetmap.py -r 192.168.1.0-192.168.2.0
python FTnetmap.py -v -r 192.168.1.0-192.168.1.255
python FTnetmap.py -e fileName.txt -r 192.168.1.0-192.168.1.255
```

*Following command scan alive IP's between firstIP and lastIP and export IPs to fileName.txt :*
```
python FTnetmap.py -e fileName.txt -r firstIP-lastIP
```
