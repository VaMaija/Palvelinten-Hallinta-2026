
## h3 Demoni

#### X) Tiivistä. 

Tero Karvisen osuus kesken 11.11. 17:30 

**Ansible Community Documentation: Handlers: running operations on change**  
'ansible-doc service':  
Ansiblen handler on **tehtävä**, joka suoritetaan silloin kun jokin toinen tehtävä tekee todellisen muutoksen kohdejärjestelmässä.  

**enabled** Määrittää käynnistetäänkö palvelu automaattisesti järjestelmän käynnityessä, arvona _true_ tai _false_  
**name** määrittää sen palvelun nimen, jota halutaan hallita (apache2, nginx) default _null_  
**state** määrittää palvelun ajonaikaisne tilan eli onko oltava käynnissä vai pysäytettynä. Tyypillisiä arvoja _started, stopped, restarted_    
 
esimerkki:  

<img width="227" height="44" alt="Näyttökuva 2026-11-11 181708" src="https://github.com/user-attachments/assets/79a8471e-c66a-48b9-9112-afec8481ce2b" />




#### A) Apassi.   
Asenna Apache 2 käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia.  


    maija@Virtuaali:~$ sudo apt-get update&&upgrade  //Päivitys  
    maija@Virtuaali:~$ sudo apt-get install apache2 //Asennetaan Apache2      
Olin yrittänyt aentaa caddya palvelimelle ennen apachea ja caddyn etusivu oli localhost-sivulla.   
Caddy käyttää porttia 80, kysyin ohjeita tekoälyltä, että miten saan caddyn pois päältä.   

    maija@Virtuaali:~$ sudo systemctl stop caddy  
    maija@Virtuaali:~$ sudo systemctl status caddy     
    maija@Virtuaali:~$ sudo systemctl stop caddy  
    maija@Virtuaali:~$ sudo systemctl status caddy  
    
Caddy näyttää olevan inactive-tilassa, mutta locahostista vastaa edelleen caddyn etusivu. Kävin tilannetta läpi Google Geminin kanssa. Testasin saanko yksityisessä selainikkunassa eri tuloksen ja tosiaan niin kävi. 

<img width="945" height="419" alt="Näyttökuva 2026-11-11 013325" src="https://github.com/user-attachments/assets/917a4dc0-1690-4c94-ac61-cc2e39fc7eba" /> 

    maija@Virtuaali:~$ sudo systemctl disable caddy //ei auttanut. Localhost edelleen vastaa caddylle.
    maija@Virtuaali:~$ sudo systemctl status apache2 // Tarkastetaan apachen tila, käy ja kukkuu.  

<img width="467" height="139" alt="Näyttökuva 2026-11-11 014435" src="https://github.com/user-attachments/assets/d399813a-4519-4b93-9fab-9bf0bb91dc36" />  

    maija@Virtuaali:~$ sudo apt-get install ufw //palomuurin asennus  
    maija@Virtuaali:~$ sudo ufw allow ssh //sallii ssh-liikenteen palomuurin kautta  
    maija@Virtuaali:~$ sudo ufw enable //palomuuri pistetään käyntiin  
    maija@Virtuaali:~$ sudo ufw allow 80/tcp //Sallii verkkoliikenteen verkkopalvelimelle  
    Rule added
    Rule added (v6)
    maija@Virtuaali:~$ sudo ufw allow 443/tcp
    Rule added
    Rule added (v6)  
    
<img width="281" height="119" alt="Näyttökuva 2026-11-11 015257" src="https://github.com/user-attachments/assets/43c33cf7-37f9-43ba-9d5d-2e331c383897" />  

    maija@Virtuaali:~$ sudo micro /etc/hosts  
    
<img width="244" height="97" alt="Näyttökuva 2026-11-11 015725" src="https://github.com/user-attachments/assets/020b9038-54d6-4e7a-9dcc-657b2caa6e3f" />´

    maija@Virtuaali:~$ sudoedit /etc/apache2/sites-available/Väliaho.fi.conf  
    
<img width="301" height="125" alt="Näyttökuva 2026-11-11 020418" src="https://github.com/user-attachments/assets/acb59b5b-4c2e-4811-a421-2016f19b8852" />  

    maija@Virtuaali:~$ mkdir Julkinensivu  //tänne verkkosivun koodimytty ilman sudoa  index.html-nimellä  
    maija@Virtuaali:~/Julkinensivu$ ls  
    index.html  
    maija@Virtuaali:~$ ls -la /home/ | grep maija
    "drwx------ 20 maija  maija  4096 10.11. 23:43 maija" //ainoastaan käyttäjällä on oikeus päästä kotihakemistoon. 
    maija@Virtuaali:~$ sudo chmod o+x /home/maija  //annetaan muille (Apachelle) suoritusoikeuskulkea kotihakemiston kautta hakemaan tietoa sieltä. (execute == x)  
    maija@Virtuaali:~$ ls -la ~| grep Julkinensivu //Julkiselle sivulle pääsee lukemaan ja suorittamaan myös muut
    "drwxrwxr-x  2 maija maija  4096 11.11. 01:06 Julkinensivu"  

<img width="305" height="52" alt="Näyttökuva 2026-11-11 021756" src="https://github.com/user-attachments/assets/1a712622-53f1-4f00-ae6c-b65c4441130d" />  

    maija@Virtuaali:~$ sudo a2ensite väliaho.fi.conf
    Enabling site väliaho.fi.
    To activate the new configuration, you need to run:
    "systemctl reload apache2"

Testisivuni ei tule edelleenkään esiin. Caddy on otettu poi käytöstä , mutta se vastaa localhostilta omalla ruudullaan, yksityisellä puolella tulee Apachen oletussivu. 

    maija@Virtuaali:~$ curl -I http://väliaho.fi
HTTP/1.1 200 OK  // tämä siis toimii lokaalisti , mutta ei vaan näy?  
Server: nginx  //Mitä ihmettä?  
Date: Wed, 11 Nov 2026 00:36:22 GMT    
Content-Type: text/html  
Content-Length: 1758  
Last-Modified: Tue, 23 Jun 2026 09:06:03 GMT  
Connection: keep-alive  
ETag: "6a3a4c7b-6de"  
Accept-Ranges: bytes  

tekoäly kertoi minulle syyksi, että koska olin aikaisemmin samalla koneella testannut vuokraamaani osoitetta väliaho.fi, niin localhoatia ei voi testata samalla nimellä. Luonnollisesti tajusin sen klo 03:00 yöllä. 

#### B) Moottorix. Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia. (Muista sammuttaa Apache ensin.)  

        maija@Virtuaali:~$ sudo systemctl stop apache2 //Pysäytetään Apache
        maija@Virtuaali:~$ susdo systemctl disable apache2 // estetään sen automaattinen uudelleenkäynnistyminen  
        
<img width="469" height="102" alt="Näyttökuva 2026-11-11 030546" src="https://github.com/user-attachments/assets/f7c4d7e1-e45d-4f3d-a7b2-9061d57ce5d3" />  

        maija@Virtuaali:~$ sudo apt-get install nginx  //Asentaa   
        maija@Virtuaali:~$ sudo systemctl start nginx  //käynnistää  
        maija@Virtuaali:~$ sudo systemctl enable nginx //ottaa käyttöön

<img width="638" height="164" alt="Näyttökuva 2026-11-11 031028" src="https://github.com/user-attachments/assets/5081a0d5-9b7d-4328-8ddd-d8489d2b3bcf" />  

Tähän asti tehty 10.11 klo 20 - 11.11 klo 03:15. jatkuu huomenna töiden jälkeen. 
jatkuu 11.11. klo 16:45  

Ngnix käyttää oletuksena kansiota /var/www/html, joka vaatii sudo-oikeudet. Ohjataan etsimään verkkosivu kansiosta /Julkinensivu/  

        maija@Virtuaali:~$ sudo micro /etc/nginx/sites-available/default //vaihdetaan "kotihakemistoksi" oikea osoite
        
<img width="373" height="236" alt="Näyttökuva 2026-11-11 165640" src="https://github.com/user-attachments/assets/7b90eaca-6a8c-4cb0-925f-9e19edf82aa9" />   

      maija@Virtuaali:~$ sudo nginx -t  //Testataan onko syntaksi kunnossa.  
      maija@Virtuaali:~$ sudo systemctl reload nginx  
      maija@Virtuaali:~$ curl -I http://localhost  //Testataan vastaako localhost

Aikaisemmasta viisastuneena kävin poistamassa sivustohistorian. Palvelin vastaa kutsuuni localhostista.  

<img width="466" height="416" alt="Näyttökuva 2026-11-11 170858" src="https://github.com/user-attachments/assets/6c4d57f4-1847-4768-b2e1-02b798c8c15e" />  

Tekemisessä käytettiin vahvasti apuna Google Geminin yksinkertaistettuja ohjeita. Prompti: " Moottorix. Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia. (Muista sammuttaa Apache ensin.) Kerro minulle selkeästi mitä komennoissa tapahtuu ja miksi. Opasta, opeta ja testaa ymmärränkö."   


#### c) Automoottorix. Automatisoi Nginx asennus Ansiblella. Ylläpitäjän osuus Ansiblella riittää, itse HTML-weppisivut voi tehdä käsin. kesken 11.11.
#### d) Vapaaehtoinen bonus: Osiris-T. Osiris-T asentaa Wazuhin ja tarvittavat virtuaalikoneet. Voit lähettää vihamielistä verkkoliikennettä (tcpreplay), siepata sen (suricata) ja saat tulokset suoraan dashboardille (wazuh). Enemmän alpha kuin se kreikkalainen kirjain. Mutta Oskari, Nico ja Arttu ilahtuvat, jos kokeilet. Häkämies, Saario, Mukkula 2026: Osiris-T. Häkämies etal 2026: How to Install. Kesken 11.11.


Lähteet; 

Nekin kesken




        



 

        





        





 






    
    
     

    

    

    



    
    

    

    

 
  



    
    
    

