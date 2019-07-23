# nmap

My go-to thanks to IppSec:
```
nmap -sC -sV -oA nmap/help <IP>
```
* -sC equivalent to --script=default
* -sV = Probe open ports to determine service/version info

Ping scan
```
nmap -sP 10.0.0.0/24
```

Other great resources:
* <https://nmap.org/book/man.html>
* <https://blogs.sans.org/pen-testing/files/2013/10/NmapCheatSheetv1.1.pdf>
* <https://highon.coffee/blog/nmap-cheat-sheet/>