h2 turbo boosted

Tunkeutumistekniikoita saa harjoitella vain annettuihin harjoitusmaaleihin.
Tarkista kohteiden osoitteet huolella, ja irrota tarvittaessa tietokoneet
Internetistä. Oppitunneilla ja kurssin säännöissä on annettu tarkempia ohjeita.
Vieraita koneita ei saa porttiskannata, eikä vieraisiin koneisiin saa hyökätä.

z) Lue ja tiivistä. Tässä z-alakohdassa ei tarvitse tehdä testejä
tietokoneella, vain lukeminen ja tiivistelmä riittää). Tiivistämiseen riittää
muutama ranskalainen viiva.

  • € Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a
    Penetration Test Using Metasploit (kohdasta "Conducting a penetration test
    with Metasploit" luvun loppuun)

Tee ja raportoi:

a) SELECT * FROM student. Ratkaise SQLZoo:sta: 0 SELECT basics, 1 SELECT name,
2 SELECT from World.

b) Nyrkkeilysäkki. Asenna Metasploitable 2 samaan verkkoon Kalin kanssa. Katso,
ettei haavoittuva Metasploitable 2 näy Internetiin.

c) Nauhalle. Nauhoita kaikki konsolissa annetut asiat ja näkyvät tulosteet koko
tehtävästä. (esim script)

d) Ei kuulu! Ennenkuin aloitat skannaukset, kokeile, ettei Kali pääse nettiin
'ping 8.8.8.8' vastaa "Network is unreachable".

e) Piilosilla. Etsi Metasploitable 2 verkkoskannauksella. Tarkista ensin, että
osoitteet ovat uskottavia (ipcalc 10.0.0.1/23). Sitten ping sweep (nmap -sn).
Analysoi tulokset, eli selitä, mitä mikäkin asia tulosteessa tarkoittaa. Mitä
päättelet eri koneista?

f) Kilo. Porttiskannaa 1000 tavallisinta porttia löydetyistä koneista. Selitä
ja analysoi tulokset. Mikä koneista voisi olla Metasploitable 2?

g) Ja tiskiallas. Porttiskannaa Metasploitable 2 perusteellisesti. Selitä ja
analysoi tulokset. Selitä kustakin avoimesta portista, mikä palvelu siinä on /
mihin se on tarkoitettu, onko se tavallisesti näkyvissä Internetiin ja onko se
nykyiaikainen. Voit myös arvioida, onko versio päivitetty.

h) Sataa simpukoita. Tunkeudu vsftpd-palveluun.

i) Ovi jäi auki. Mitä löytyy portista 1524/tcp? Kokeile netcattilla 'nc'.

j) Darn Low Security. Etsi Metasploitable 2 weppipalvelin. Tee paikallinen
tunnus DVWA Damn Vulnerable Web App -ohjelmaan. Aseta vasemman reunan palkista
"DVWA Security" Low.

k) Execute! Ratkaise DVWA "Command Excution". ("DVWA Security" on paras olla
"Low")

l) Asenna jokin kone Vulnhub:ista samaan Internetistä eristettyyn verkkoon
Kalin kanssa.

m) Skannaa Vulnhubista hakemasi kone, ja analysoi tulokset.

n) Vapaaehtoinen: Murtaudu jollain uudella tavalla Metasploitable 2:n.

o) Vapaaehtoinen: Murtaudu Vulnhubista lataamaasi kuvaan.

p) Vapaaehtoinen: Jos viime kerran SQL injektiot jäivät mysteeriksi, voit tehdä
ne nyt suuremmalla ymmärykselllä

q) Vapaaehtoinen: Jatka HackTheBoxin parissa.

r) Vapaaehtoinen: Tee lisää tehtäviä DVWA:sta.

s) Vapaaehtoinen: Tee lisää tehtäviä WebGoatista.

t) Vapaaehtoinen: Tee lisää tehtäviä Over the Wiresta.

Vinkit

  • Muista merkitä kurssi ja kaikki muutkin lähteet - mikä tieto on mistäkin.
  • O'Reilly Learning (ent. Safari) on HH opiskelijoille ilmainen HH kirjaston
    kautta.
  • Jos joudut katsomaan suoraan esimerkki/malliratkaisun, merkitse se
    raporttiin.
  • "Host-only networking" sopii maalikoneille VirtualBoxissa. Esimerkkejä
    löytyy hakukoneista, vaikkapa Googlesta "karvinen" "host-only networking"
    tai sama DuckDuckGo:sta.
  • 'script -fa teronloki123' nauhoittaa terminaalia
  • pitkät tulosteet ja lokit kannattaa laittaa linkin taakse tai ainakin
    artikkelin loppuun, jotta raportti säilyy helppolukuisena.
  • Tallenna nmap-komentojen tulosteet 'sudo nmap localhost -oA teronmap123'
  • Internet-yhteyttä voi kokeilla vaikkapa pingaamalla Googlen nimipalvelinta
    'ping 8.8.8.8'.
  • Jos haluat varmistua Metasploitable2 osoitteesta, tai testata, että voit
    pingata Kali-Metasploitable kumpaankin suuntaan: kirjaudu Metasploitable 2
    (msfadmin:msfadmin) ja katso 'ip a', 'ifconfig' sekä 'ping 10.0.0.1' Kalin
    IP-osoitteella.
  • Kaikki skannaustehtävät edellyttävät tulosten kirjallista selittämistä.
    Pelkkä nmap:n tuloste ei riitä.
  • Nmap skannaa oletuksena 1000 tavallisinta porttia, 'sudo nmap localhost'.
    -vv tai v klikkailemalla ajon aikana saa lisää tietoa. Useimmat napit 's'
    kertovat statuksen, eli paljonko on skannattu.
  • Perusteellinen skannaus, esim 'sudo nmap -sV localhost' ja ehkä myös
    hitaampi 'sudo nmap -A -p- localhost'.
  • Voit aloittaa tunkeutumisen vsftpd-palvelusta. Siihen on valmis hyökkäys
    Metasploitissa: msfconsole, search vsftpd, use 0, info, set RHOST
    127.0.0.1, options, varmista huolellisesti että kohteena on harjoitusmaali,
    irroita tarvittaessa koneet Internetistä, exploit. Ja sitten (ehkä)
    nauttimaan: id, whoami, pwd, cat /etc/shadow...
  • Jos haluat, voit kokeilla parantaa pääsyä: käyttäjä->root, shell ilman
    tty:tä -> ssh tai shell -> meterpreter...
  • Komentoinjektio: ystäväsi ovat komentojen erottimet ';', '||', '&&' sekä
    pipe '|'. Ja tietysti vanhat kunnon lainausmerkit.
  • Jokin tehtävä voi olla vaikea. Tee ja raportoi silloin kaikki mitä osaat.
    Listaa lähteet, joista hait ohjeita. Onko jotain vielä kokeiltavana? Mitä
    haasteita on vielä ratkaisun tiellä? Mitä virheimoituksia tai vastaavia
    tulee? Voit myös katsoa wepistä esimerkkiratkaisun - muista viitata
    lähteeseen ja merkitä, missä kohdassa katsoit ratkaisua. Ja katsotaan
    yhdessä tunnilla loput.
  • Webgoat: A1 Injection (intro)
      □ Millaiset lainausmerkit SQL:ssä olikaan?
      □ Jos nostat kaikkien palkkaa, oletko enää rikkain?
      □ Nämä nimet ("A1 Injection") ovat samat kuin OWASP 10:ssä.
      □ Jos SQL kaipaa virkistystä, SQLZoo harjoitukset ja Wikipedia: SQL
        syntax voivat auttaa.

