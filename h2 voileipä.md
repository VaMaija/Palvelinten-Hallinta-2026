## h2 Voileipä

### x) Lue ja tiivistä. 

**Karvinen 2026: Sudo without password** 
Ansiblen tarvitsee root-oikeudet slave -koneelle. Yksi tapa on sallia sudo-oikeudet ilman salasanaa.  
-luodaan uusi käyttäjä "antero"  
-luodaan "sudoless" -ryhmä, johon uusi käyttäjä liitetään. Ryhmälle luodaan sääntö, jonka mukaan ryhmässä olevat käyttäjät eivät tarvitse salasanaa. 
-luodaan sääntö visudo tekstieditorilla, joka tekee tekstiin joitain tarkastuksia ennen komennon ajoa. Virheellisen komennon ajosta voi aiheutua järjestelmän lukkiutuminen eikä sudo-oikeudet enää toimi. Tästä syystä kannattaa avata toinen terminaali ennen komennon ajamista.  

   **huom:  $sudo -i //-i = interactive, siirtää käyttäjän pääkäyttäjätilaan (root) ja lataa puhdistetun ja aitojen root-asetusten mukaisen ympäristön  (Google Gemini)**
  
**Munroe 2006: xkcd 149: Sandwitch**  
<img width="360" height="299" alt="sandwich" src="https://github.com/user-attachments/assets/88f4c008-dfc1-406d-937a-4c7a704e1af6" />  

*Yksittäinen sarjakuvaruutu, jossa hahmo pyytää toiselta hahmolta voileipää. Saa sen vasta käytettyään sudoa ennen pyyntöä.* 
 
**Karvinen 2026: Passwordless Sudo with Ansible** 
Ansiblella luodaan rooli "antero", joka tekee samat toimet automaattisesti kuin manuaalisesti tehtiin Sudo without password-tekstissä. uuden käyttäjän  uusi käyttäjä sudoless-ryhmään ja luodaan sääntö, joka lisää sudoers.d/NOPASSWD säännön ryhmään. 
askel-askeleelta -ohje uuden käyttäjän ja käyttäjäryhmän luomiseen Ansiblella.  

muistiinpanot itselle:  ansible-kansioon luodaan mkdir -p roles/antero/tasks/ sijainti, johon lisätään tekstitiedosto main.yml  
luodaan ryhmä sudoless:  

>   - group:  
>       name: "sudoless"  
>       state: present
>      
Luodaan käyttäjä antero ja lisää sen ryhmään sudoless, adm ja sudo:  
>    - user: 
>        name: "antero"  
>        state: present  
>        groups: ["sudoless", "sudo", "adm"]

lisää käyttäjälle SSH-avaimen: /hostin SSH  
>    - authorized_key:  
>        user: "anero"  
>        key: ""SSh-ed*****...."

Luodaan /etc/sudoers.d/sudoless -tiedosto:   
 >   - copy:    
 >       dest: "/etc/sudoers.d/sudoless"  
 >       content: "%sudoless ALL = (ALL) NOPASSWD: ALL \n"  
 >       owner: "root"  
 >       group: "root"  
 >       mode: "0644"* 
        


      

Kesken__ jatkettu 10.11.2026 käyttäen google gemini -tekoälyä. 
Annoin promtin '''hei! Osaatko auttaa seuraavan tietomassan selkeyttämisessä: Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla.
- Kustakin vain
  - Johdantokappale (Usein MODULE alla, päättyy OPTIONS alkuun)
  - Nimetyt optiot selityksineen
  - Esimerkeistä (EXAMPLES) jokin helppo ja keskeinen esimerkki
- 'ansible-doc copy': content, dest, src; owner, group, mode;
- 'ansible-doc apt': name, state, update_cache.
- 'ansible-doc file': path, recurse, src, state. owner, group, mode;
- 'ansible-doc user': name; create_home, comment, groups, shell, state, system.
- 'ansible-doc authorized_key': user, key.
 Miten opin nämä? '''

1. ansible-doc copy

    Johdanto: Kopioi tiedostoja ohjauskoneelta (controller) kohdekoneelle (slave).

    Optiot:

        src: Paikallisen tiedoston polku, joka kopioidaan.

        dest: Kohdepolku slave-koneella, jonne tiedosto sijoitetaan.

        content: Vaihtoehto src:lle; määrittää tiedoston sisällön suoraan merkkijonona.

        owner: Tiedoston omistaja (slave-koneella).

        group: Tiedoston ryhmä.

        mode: Tiedoston käyttöoikeudet oktaalimuodossa (esim. 0644).

    Keskeinen esimerkki:

 <img width="230" height="110" alt="Näyttökuva 2026-11-10 220734" src="https://github.com/user-attachments/assets/15247668-0f02-41d8-957b-a5f5695c256d" />  

 
2. ansible-doc apt

    Johdanto: Hallitsee Debian/Ubuntu-pohjaisten järjestelmien APT-paketteja (asennus, poisto, päivitys).

    Optiot:

        name: Asennettavan paketin nimi (tai listaus paketeista).

        state: Paketin tila (present = asennettu, latest = uusin versio, absent = poistettu).

        update_cache: Päivittääkö apt-välimuistin ennen toimintoa (true/false).

    Keskeinen esimerkki:

<img width="228" height="86" alt="Näyttökuva 2026-11-10 221048" src="https://github.com/user-attachments/assets/95ff583c-0515-43cf-b61e-23931ff505d0" />  

3. ansible-doc file

    Johdanto: Hallitsee tiedostojen, hakemistojen ja symbolisten linkkien ominaisuuksia, olemassaoloa ja oikeuksia.

    Optiot:

        path: Käsiteltävän tiedoston tai hakemiston polku (tai dest/name).

        state: Määritetäänkö kohde (directory = hakemisto, file = tiedosto, link = symlinkki, absent = poista).

        recurse: Käsitelläänkö hakemisto rekursiivisesti (true/false, käytetään usein state=directory kanssa).

        src: Lähdepolku (käytetään vain, kun state=link).

        owner: Omistaja.

        group: Ryhmä.

        mode: Käyttöoikeudet.

    Keskeinen esimerkki:

   <img width="302" height="110" alt="Näyttökuva 2026-11-10 221321" src="https://github.com/user-attachments/assets/a3ca8e4c-c657-4300-88f7-9b9e662f70c8" />

   4. ansible-doc user

    Johdanto: Hallitsee käyttöjärjestelmän käyttäjätilejä ja niiden asetuksia slave-koneella.

    Optiot:

        name: Käyttäjänimi.

        state: Onko käyttäjä olemassa (present tai absent).

        shell: Käyttäjän oletuskuori (esim. /bin/bash).

        create_home: Luodaanko kotihakemisto (true/false).

        system: Luodaanko järjestelmäkäyttäjä (true/false).

        groups: Ryhmät, joihin käyttäjä liitetään lisäjäsenenä.

        comment: Kommenttikenttä (esim. käyttäjän koko nimi / GECOS).

    Keskeinen esimerkki:

   <img width="233" height="101" alt="Näyttökuva 2026-11-10 221453" src="https://github.com/user-attachments/assets/32210203-2d88-4d88-9fc7-52ecc6ccc573" />

   5. ansible-doc authorized_key

    Johdanto: Hallitsee käyttäjien SSH-julkiavaimia (authorized_keys), mikä mahdollistaa turvallisen kirjautumisen.

    Optiot:

        user: Käyttäjä, jonka kotihakemiston authorized_keys-tiedostoa muokataan.

        key: Itse SSH-julkiavain merkkijonona (tai polku avaintiedostoon).

        state: Onko avain lisätty (present) vai poistettu (absent).

    Keskeinen esimerkki:

   <img width="272" height="89" alt="Näyttökuva 2026-11-10 221631" src="https://github.com/user-attachments/assets/dcffdaae-f5c1-415f-bcd0-62528b5b6e15" />

   


docs.ansible.com/projects/ansible/latest/cli/ansible-doc.html  


## Miten tähän asti päästiin? 

kun kaikki pitää tehdä alusta ymmärtääkseen... 
[https://github.com/VaMaija/testailua.md ](https://github.com/VaMaija/testailua.md/blob/main/alusta%20uudelleen.md)  

### A) Sudoless. Tee ansiblea varten tunnus, jolla voi käyttää sudoa ilman salasanaa. Sekä ssh-kirjautuminen että sudon käyttö tulee olla ansbilea varten automatisoitu.
[ohjeet](https://terokarvinen.com/passwordless-sudo/): Tero Karvinen Sudo without password 2026  
    
    maija@hallitsija3:~$ sudo adduser antero  //Lisätään käyttäjä "antero" ja anterolle salasanat ym. 
    maija@hallitsija3:~$ sudo groupadd sudoless  //Lisätään ryhmä "sudoless"
    maija@hallitsija3:~$ sudo adduser antero sudoless  //Lisätään "antero" osaksi ryhmää "sudoless". Ryhmä saa myöhemmin säännön, jonka takia salasanoja ei tarvita.  

*Avataan uusi terminaali*   

    maija@hallitsija3:~$ ssh localhost  
    
<img width="415" height="50" alt="Näyttökuva 2026-11-03 220659" src="https://github.com/user-attachments/assets/d522f0e7-d185-4a75-9584-b6579b236670" /> 

    maija@hallitsija3:~$ sudo su  
    [sudo] password for maija:  
    root@hallitsija3:/home/maija#  

*Jätetään root-terminaali taustalle.*  
   
    maija@hallitsija3:~$ sudo visudo /etc/sudoers.d/sudoless 
    [sudo] password for maija: 
    maija@hallitsija3:~$ cd /etc/sudoers.d/
    maija@hallitsija3:/etc/sudoers.d$ ls
    10-installer  README  sudoless
    maija@hallitsija3:/etc/sudoers.d$ cat sudoless 
    cat: sudoless: Permission denied
    maija@hallitsija3:/etc/sudoers.d$ sudo cat sudoless 
    %sudoless ALL = (ALL) NOPASSWD: ALL  
    

<img width="412" height="235" alt="Näyttökuva 2026-11-03 223405" src="https://github.com/user-attachments/assets/dd6f1331-8f6e-46fa-9132-a4d8af7cac67" />  

*komentojen selitykset https://terokarvinen.com/passwordless-sudo/* 

    maija@hallitsija3:~$ ssh antero@localhost   
 ei avaudu, koska en ole kirjautunut ulos, kirjauduin ulos linuxilta ja tulin takaisin anterona. Onnistuin luomaan sormenjäljen localhostiin.  
 
    antero@hallitsija3:~$ ssh antero@localhost
    antero@hallitsija3:~$ sudo -k
    antero@hallitsija3:~$ sudo echo "tämä on testi!"  

 Onnistui ilman salasanaa.     
    
 
<img width="403" height="201" alt="Näyttökuva 2026-11-03 225847" src="https://github.com/user-attachments/assets/67948bc1-7541-4afb-b452-504f75fb9cd5" />
 
### B) Antero. Tee salasanaton, automaattisesti ssh:lla kirjautuva tunnus Ansiblella.  
[Ohjeet](https://terokarvinen.com/passwordless-sudo-with-ansible/)  

palasin omaksi itsekseni vaihtamalla käyttäjää:  

    maija@hallitsija3:~/ansible$ cd  
    maija@hallitsija3:~$ mkdir -p roles/antero/tasks/  
    maija@hallitsija3:~$ micro roles/antero/tasks/main.yml  
    maija@hallitsija3:~$ cat roles/antero/tasks/main.yml  

kopioin Karvisen sivulta yml-koodin ja vaihdoin keyksi oman käyttäjän public keyn... en ole tästä täysin varma.  

<img width="569" height="174" alt="Näyttökuva 2026-11-03 234951" src="https://github.com/user-attachments/assets/a0307578-bdc7-4394-ab6c-5b391c5ec06b" />    


Nyt kun olin tässä vaiheessa päättin tarkastaa mitä olin tehnyt.  
Olin luonut anteron roolituksen väärään suuntaan.  

<img width="479" height="253" alt="Näyttökuva 2026-11-03 235526" src="https://github.com/user-attachments/assets/9a7fb579-f580-4f5b-8a74-a6a5b3cb000f" />  

kopion kansion oikeaan kohteeseen ja postin sen jälkeen väärästä paikasta.  

    maija@hallitsija3:~$ cp -r roles/ ansible/
    maija@hallitsija3:~$ tree -F
     

<img width="230" height="271" alt="Näyttökuva 2026-11-04 000514" src="https://github.com/user-attachments/assets/b7558bd6-8451-4fde-ac17-8a60329ea020" />  

    maija@hallitsija3:~$ rm -r roles/ 
    maija@hallitsija3:~$ cd ansible/  
    maija@hallitsija3:~/ansible$ cat site.yml 
    - hosts: all  
      become: true   
      roles:  
        - hello  
        - antero  
    maija@hallitsija3:~/ansible$ ansible-playbook site.yml  

<img width="573" height="104" alt="Näyttökuva 2026-11-04 001752" src="https://github.com/user-attachments/assets/3ad0f4e5-111d-47a0-83bf-e57a54ce5c9c" />  

    maija@hallitsija3:~/ansible$ ansible-playbook site.yml --ask-become-pass
    BECOME password:   //Tämä vähän hämmensi, kyse oli kuitenkin maija-käyttäjän salasanasta. 
    
<img width="773" height="267" alt="Näyttökuva 2026-11-04 002535" src="https://github.com/user-attachments/assets/2af5f86e-53e9-4ea7-9d95-d5793acc4a4a" />  


### C) Package. Asenna kaksi pakettia Ansiblella.  

Asensin apachen ja palomuurin.    

    maija@hallitsija3:~/ansible$ mkdir -p roles/kaksipakettia/tasks   // Luodaan roolikansio kaksipakettia ja siihen tasks-kansio ja main.yml -kooditiedosto  
    maija@hallitsija3:~/ansible$ cd roles/kaksipakettia/tasks/
    maija@hallitsija3:~/ansible/roles/kaksipakettia/tasks$ micro main.yml

<img width="172" height="64" alt="Näyttökuva 2026-11-05 065052" src="https://github.com/user-attachments/assets/2f1e8152-4514-46c1-a6ac-5613ee276b26" />  


Siirrytään ansible-kansion alkuun ja lisätään rooli site.yml -tiedostoon  
    maija@hallitsija3:~/ansible$ micro site.yml 

    
<img width="233" height="67" alt="Näyttökuva 2026-11-05 065604" src="https://github.com/user-attachments/assets/ccbe3638-f5b9-46ae-a5d3-4939aef0b074" />  

     

<img width="213" height="146" alt="Näyttökuva 2026-11-05 065714" src="https://github.com/user-attachments/assets/45cd80a2-1ce4-4a22-8f71-de450be1088c" />










4.11.2026 klo 01:26 nukkumaan, jatkuu vasta keskiviikon työpäivän jälkeen.  
4.11.2026 klo 21:00 jatkuu raportin muokkaus luettavampaan muotoon... 
5.11.2026 klo 6:15






      
    





    


    



    












 

    
    

    
   
       

    
  

    
    
    

    









**********************************************
Lähteet:  
Karvinen 2026: Sudo without password:  https://terokarvinen.com/passwordless-sudo/  
Munroe 2006: xkcd 149: Sandwitch: https://xkcd.com/149/   
Karvinen 2026: Passwordless Sudo with Ansible: https://terokarvinen.com/passwordless-sudo-with-ansible/   
Väliaho 2026 Testailua https://github.com/VaMaija/testailua.md/blob/main/alusta%20uudelleen.md 
Ansible-doc https://docs.ansible.com/ 
