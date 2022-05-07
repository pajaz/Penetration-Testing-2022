# h5 Script Kiddie

Part of Penetration Testing ICT4TN027-3007 course of Haaga-Helia University of Applied Sciences held by Tero Karvinen. Course is in Finnish.  
    
Course page: https://terokarvinen.com/2021/penetration-testing-course-2022-spring/  

Palautus sovitusti Laksuun.

Harjoittele vain luvallisiin harjoitusmaaleihin. Irrota tarvittaessa koneet Internetistä ja seuraa hyökkäysohjelmien toimintaa toisella työkalulla.

## v) Lue artikkelit ja katso videot, tee kustakin muistiinpanot (muutama ranskalainen viiva per artikkeli/video). Tässä v-kohdassa ei tarvitse tehdä mitään kokeita koneella.

### € Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit (kohdasta "Conducting a penetration test with Metasploit" luvun loppuun)

### Vapaavalintainen läpikävely (walktrough) yhdestä murrosta. Voit siis valita yhden läpikävelyn mistä tahansa näistä kanavista tai blogista. Kanavilla on muitakin artikkeleita, valitse tästä läpikävely. Keskity tiivistäessä asioihin, joita voisit itse soveltaa hyökätessä. Tässä on yli 200 h videota, niitä ei tarvitse katsoa kaikkia.

- 0xdf https://0xdf.gitlab.io/
- ippsec https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA/videos
- John Hammond https://www.youtube.com/user/RootOfTheNull/videos?sort=p

Muu myöhemmin ilmoitettava lukutehtävä: Jos ensi viikon vierailija pyytää kertaamaan jotain Windowsista (rekisteri, tiedostojärjestelmä, käyttäjät...) tai tietokonerikosten teknisestä tutkinnasta, laitan siitä erikseen sähköpostia

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
  
Lopputuloksena en saanut kyseistä palvelua korkattua metasploitista löytyvällä exploitilla.  

## c) Rat. Demonstroi meterpreter:n käyttöä

Lähde: https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/

Meterpreter on Metasploitiin kuuluva payload jonka avulla tehdään jotain? En oikein ymmärtänyt yhdenkään lukemani sivun selitystä työkalusta tai miten sitä pitäisi käyttää. Jonkinlainen Ruby Shell ilmeisesti jonka tavoitteena on, etteivät hyökkääjän toimet olisi niin näkyviä.

En valitettavasti keksi mitään keinoa emonstroida tätä, koska ohjeet joita löydän aloittavat joko siitä, että meterpreter konsoli on auki tai niissä aloitetaan oman payloadin kirjoittaminen. Pitäisikö tässä keksiä joltain kohdekoneelta, jokin heikkous, johon tätä voidaan käyttää? 

Useissa ohjeissa myös käytetään Windows kohteita, joita minulla ei ole käytössäni.

Yritän vielä huomenna jatkaa. 


## d) Laksu. Palauta linkkisi sovitusti Laksuun. Arvioi ja anna palautetta kahden (2) kurssikaverin kotitehtävästä. (Vapaaehtoisena bonuksena saat antaa palautetta useammastakin. Tätä d-alakohtaa ei tarvitse raportoida.)

## e) Vapaaehtoinen: ExploitDB offline. Murtaudu johonkin harjoitusmaaliin käyttäen hyökkäystä, jonka etsit 'searchsploit' työkalulla. Valitse sellainen hyökkäys, joka ei tule suoraan Metasploitin mukana.

## Vapaaehtoinen: NSA was here. Kokeile Ghidraa. Voit esimerkiksi kääntää jonkin oikein yksinkertaisen ohjelmasi binääriksi, ja katsoa miltä se näyttää.
