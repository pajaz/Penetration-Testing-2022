# Alkutesti

## Konfiguraatio

* Tietokone: Lenovo Legion 80YY kannettava tietokone  
* Käyttöjärjestelmä: Linux Debian 11 Bullseye non-free 
  
## Verkkoliikenteen monitorointi  

* Työkalu: tcpdump  
* Tavoite: Analysoida oman tietokoneen lähtevää ja saapuvaa verkkoliikennettä.  
* Pätkä tcpdump -v -lokia:  
    10:03:39.857759 IP (tos 0x0, ttl 64, id 53430, offset 0, flags [DF], proto TCP (6), length 60)
        derpface.39020 > li1397-217.members.linode.com.https: Flags [S], cksum 0xcd55 (correct), seq 1441246843, win 64240, options [mss 1460,sackOK,TS val 708110027 ecr 0,nop,wscale 7], length 0
    10:03:39.891482 IP (tos 0x0, ttl 54, id 0, offset 0, flags [DF], proto TCP (6), length 60)
        li1397-217.members.linode.com.https > derpface.39020: Flags [S.], cksum 0xdd41 (correct), seq 1383486085, ack 1441246844, win 65160, options [mss 1460,sackOK,TS val 2046348662 ecr 708110027,nop,wscale 7], length 0
    10:03:39.891552 IP (tos 0x0, ttl 64, id 53431, offset 0, flags [DF], proto TCP (6), length 52)
        derpface.39020 > li1397-217.members.linode.com.https: Flags [.], cksum 0x087f (correct), ack 1, win 502, options [nop,nop,TS val 708110061 ecr 2046348662], length 0
    10:03:39.899226 IP (tos 0x0, ttl 64, id 53432, offset 0, flags [DF], proto TCP (6), length 569)
        708110069 ecr 2046348662], length 517
   
Lokia tulee todella paljon, kun vain muutama tabi on auki selaimessa (paljon mm. google userscontent lokia, vaikken käytä googlen hakua), joten en kopioi tähän kaikkea.  
Oheinen loki näyttää alkutapahtumat (Three-Way-Handshake), kun päivitin selaimellani verkkosivun.  
Ensin oma tietokoneeni (derpface.39020) lähettää yhteyspyynnön (S = SYN) osoitteeseen li1397-217.members.linode.com.https. Pyyntö pitää sisällään seq tunnuksen 1441246843.  
Palvelin vastaa tähän lähettämällä takaisin oman sequence tunnuksensa, sekä hyväksynnän ack vastaanottamalleen tunnukselle (plus yksi numero).  
Oma tietokoneeni vastaa tähän ack: 1 <-- Tämä pitäisi käsittääkseni olla aiemmin lähetetty seq + 1, jotta three-way-handshake onnistuisi.  
  
Minulta loppuu aika tämän tarkasteluun.  



