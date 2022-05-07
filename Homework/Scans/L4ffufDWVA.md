# Lesson4 h) fuff 

Tämä dokumentti oli alunperin ffufin tekemä markdown -tiedosto, mutta tuon formatointi oli erittäin sekava ja isolta osin rikki, joten korvasin tekstin copy-pastella terminaalista selkeyden vuoksi.  

Metasploitable 2 koneen skannaus: 
```
$ ffuf -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -u http://192.168.56.8/dvwa/FUZZ -of md -o  ffufDWVA

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.5.0 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://192.168.56.8/dvwa/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-1.0.txt
 :: Output file      : ffufDWVA
 :: File format      : md
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
________________________________________________

#                       [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 52ms]
# Attribution-Share Alike 3.0 License. To view a copy of this  [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 53ms]
# or send a letter to Creative Commons, 171 Second Street,  [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 53ms]
about                   [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 17ms]
index                   [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 25ms]
docs                    [Status: 301, Size: 321, Words: 21, Lines: 10, Duration: 1ms]
#                       [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 394ms]
# Copyright 2007 James Fisher [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 394ms]
#                       [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 514ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/  [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 530ms]
# directory-list-1.0.txt [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 560ms]
#                       [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 566ms]
# This work is licensed under the Creative Commons  [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 566ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 570ms]
# Unordered case sensative list, where entries were found  [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 676ms]
# on atleast 2 host.  This was the first draft of the list. [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 680ms]
                        [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 725ms]
robots                  [Status: 200, Size: 26, Words: 3, Lines: 2, Duration: 0ms]
security                [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 85ms]
login                   [Status: 200, Size: 1289, Words: 83, Lines: 66, Duration: 53ms]
config                  [Status: 301, Size: 323, Words: 21, Lines: 10, Duration: 1ms]
external                [Status: 301, Size: 325, Words: 21, Lines: 10, Duration: 4ms]
setup                   [Status: 200, Size: 3549, Words: 182, Lines: 81, Duration: 56ms]
logout                  [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 22ms]
vulnerabilities         [Status: 301, Size: 332, Words: 21, Lines: 10, Duration: 10ms]
instructions            [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 53ms]
favicon                 [Status: 200, Size: 1406, Words: 5, Lines: 2, Duration: 0ms]
COPYING                 [Status: 200, Size: 33107, Words: 5362, Lines: 623, Duration: 0ms]
:: Progress: [141708/141708] :: Job [1/1] :: 5334 req/sec :: Duration: [0:00:21] :: Errors: 0 ::
```

