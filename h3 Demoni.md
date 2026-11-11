
## h3 Demoni

#### X) Tiivistä. 

**Tero Karvinen: Apache installed with Ansible - quick notes**
[täällä](https://terokarvinen.com/apache-ansible/)  
Tehtävät piti tehdä ennenkuin pystyin palaamaan ohjeen tiivistämiseen.  
Karvisen tekstissä käydään läpi Apachen asennus Ansiblen avulla. 
Teksti ei ole mielestäni kovin selkeä ja vaatii tekijältä jo aikalailla ymmärrystä sekä Ansiblen, että Apachen sielunmaisemasta. 

Tekstistä löytyy vaadittu kansiorakenne ja jokaiseen kansioon vaaditun tekstin sisältö. Tehtävien teossa tulee ottaa huomioon eri paketin käyttö.   


**Ansible Community Documentation: Handlers: running operations on change** 
[täällä](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html)  

'ansible-doc service':  
Ansiblen handler on **tehtävä**, joka suoritetaan silloin kun jokin toinen tehtävä tekee todellisen muutoksen kohdejärjestelmässä.  

**enabled** Määrittää käynnistetäänkö palvelu automaattisesti järjestelmän käynnityessä, arvona _true_ tai _false_  
**name** määrittää sen palvelun nimen, jota halutaan hallita (apache2, nginx) default _null_  
**state** määrittää palvelun ajonaikaisne tilan eli onko oltava käynnissä vai pysäytettynä. Tyypillisiä arvoja _started, stopped, restarted_    
 
esimerkki:  

 

<img width="520" height="648" alt="image" src="https://github.com/user-attachments/assets/59571bdd-2d41-4a63-8cb0-31a1d01af00b" />    



#### A) Apassi.   
Asenna Apache 2 käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia.  


    maija@Virtuaali:~$ sudo apt-get update&&upgrade  //Päivitys  
    maija@Virtuaali:~$ sudo apt-get install apache2 //Asennetaan Apache2      
Olin yrittänyt aentaa caddya palvelimelle ennen apachea ja caddyn etusivu oli localhost-sivulla.   
Caddy käyttää porttia 80, kysyin ohjeita tekoälyltä, että miten saan caddyn pois päältä.   

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

    maija@Virtuaali:~$ sudo micro /etc/hosts  //tämä oli myöhemmin arvioituna turha liike  
    
<img width="244" height="97" alt="Näyttökuva 2026-11-11 015725" src="https://github.com/user-attachments/assets/020b9038-54d6-4e7a-9dcc-657b2caa6e3f" />´

    maija@Virtuaali:~$ sudoedit /etc/apache2/sites-available/Väliaho.fi.conf  // Apache-version sivu olisi pitänyt löytyä tuosta osoitteesta. 
    
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

Testisivuni ei tule edelleenkään esiin. Caddy on otettu pois käytöstä , mutta se vastaa localhostilta omalla ruudullaan, yksityisellä puolella tulee Apachen oletussivu. 

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

Toistojen takia olin luonut jälleen uuden virtuaalikoneen. Lähtötilanteeni tämän koneen osalta oli seuraava:  

<img width="185" height="119" alt="Näyttökuva 2026-11-11 185447" src="https://github.com/user-attachments/assets/4784ef5d-2d1a-4902-b6af-9fa76a8540ab" />  


Mitä pitäisi pystyä tekemään?
1. Asentaa Nginx.   
2. Kopioida Nginxin sivuston asetustiedosto oikeaan paikkaan.   
3. Ottaa sivusto käyttöön.   
4. Poistaa Nginxin oletussivusto käytöstä.   
5. Varmistaa, että Nginx-palvelu on käynnissä.   
6. Käynnistää Nginx uudelleen, jos asetustiedosto muuttuu.  


        maija@Virtuaali:~/ansible$ mkdir -p roles/nginx/tasks/   //Luodaan Ansible-kansioon rooli nginx ja sille tasks-kansio
        maija@Virtuaali:~/ansible$ cd roles/nginx/tasks/          //siirrytään kansioon
        maija@Virtuaali:~/ansible/roles/nginx/tasks$ micro main.yml  //kirjataan sinne main.yml  
        maija@Virtuaali:~/ansible/roles/nginx/tasks$ cat main.yml  
---
```
- name: Asenna Nginx
  ansible.builtin.apt:  //asentaa nginx-paketin
    name: nginx
    state: present
    update_cache: true

- name: Kopioi Nginxin sivuston konfiguraatio
  ansible.builtin.copy:
    dest: "/etc/nginx/sites-available/example.com.conf"
    src: "example.com.conf"
    owner: "root"
    group: "root"
    mode: "0644"
  notify: reload nginx

- name: Ota sivusto käyttöön tekemällä linkki sites-enabled-kansioon
  ansible.builtin.file:
    src: /etc/nginx/sites-available/example.com.conf
    dest: /etc/nginx/sites-enabled/example.com.conf
    owner: root
    group: root
    state: link
  notify: reload nginx

- name: Poista oletussivusto käytöstä 
  ansible.builtin.file:
    path: /etc/nginx/sites-enabled/default
    state: absent
  notify: reload nginx

- name: Varmista että Nginx on käynnissä ja päällä
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
```
        
Ansibleen kirjoitettavat tasks/main.yml-komennot hoitavat varsinaisen asentamisen, tiedostojen kopioinnin ja palvelun käynnistyksen. Itse verkkosivun sijainti (/home/maija/Julkinensivu/) määritellään puolestaan siinä konfiguraatiotiedostossa (esim. example.com.conf), jonka Ansible kopioi palvelimelle.

        maija@Virtuaali:~/ansible$ cat site.yml
        
```
- hosts: all
  become: true 
  roles:
    - hello
    - nginx
```
    maija@Virtuaali:~/ansible/roles/nginx/handlers$ cat main.yml
```
- name: reload nginx
  ansible.builtin.service:
    name: nginx
    state: restarted
```
    maija@Virtuaali:~/ansible/roles/nginx/files$ cat example.com.conf 
```
# Managed file, changes will be overwritten

<VirtualHost *:80>
        ServerName example.com
        DocumentRoot /home/$USER/Julkinensivu/

        <Directory /home/$USER/Julkinensivu/>
                require all granted
        </Directory>
</VirtualHost> 
```

Päätin kokeilla soittaa tätä noitarumpua:  

        maija@Virtuaali:~/ansible$ ansible-playbook site.yml  //Käynnistetään ansible, vaati sudotuksen  
        maija@Virtuaali:~/ansible$ ansible-playbook site.yml --ask-become-pass  

<img width="837" height="263" alt="Näyttökuva 2026-11-11 223127" src="https://github.com/user-attachments/assets/e11b5776-c398-42db-8a22-e78e168fd5c1" />  


<img width="923" height="170" alt="Näyttökuva 2026-11-11 223204" src="https://github.com/user-attachments/assets/62cbd10a-c2f6-4c3d-abcd-6ee3f31d593c" />         

Hyvältä näytti kunnes tuli virheilmoitus.  
handlers/main.yml kohdassa on jokin virhe. Menin katsomaan enkä huomannuut mitään virhettä. Syötin tekstin tekoälylle, joka totesi, että minulla on ylimäärinen rivi ja yksi välilyönti. En edelleenkään huomannut eroa. Kopioin tekoälyn kirjoittaman tekstin samaan kohtaan. Se näytti täysin samalta kuin alunperin itse kirjoittamani. Se jäi mysteeriksi, mutta toimi.  Lisäksi tekoäly ilmoitti, että Nginx vaatii omanlaisensa syntaxin. Pyysin luomaan jo Tero Karvisen [koodista](https://terokarvinen.com/apache-ansible/) muokkaamani tekstin oikeaan muotoon.  

```
# Managed file, changes will be overwritten

server {
    listen 80;
    server_name example.com;

    root /home/maija/Julkinensivu/;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}

```

lopulta Nginx toimi:  

<img width="905" height="295" alt="Näyttökuva 2026-11-11 224347" src="https://github.com/user-attachments/assets/46ea97ee-5504-46e0-be8f-fc6eb5c908c9" />  

Onnistunut loitsu  

<img width="221" height="191" alt="Näyttökuva 2026-11-11 222722" src="https://github.com/user-attachments/assets/96ee51da-0799-4569-9fbc-9d35f8a5425b" />  

Loppupuu

Localhostista vastaa oma verkkosivu. 

Tähän päästiin lopulta 11.11. klo 22:48


____


Lähteet:  

Tero Karvinen 2026, Apache installed with Ansible - quick notes https://terokarvinen.com/apache-ansible/ luettu 8.11.2026-11.11.2026   
Ansible Community Documentation 2026, https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html luettu 10.11.2026    
Tero karvinen 2026, Hello Ansible https://terokarvinen.com/hello-ansible/  luettu 8.11.- 11.11.2026  
Caddy Documentation 2026 https://caddyserver.com/docs/  luettu 10.11.2026  

Tekoälynä käytetty Google Gemini ja Chatgpt.  







        



 

        





        





 






    
    
     

    

    

    



    
    

    

    

 
  



    
    
    

