## h2 voileipä

### x) Lue ja tiivistä. 

Karvinen 2026: Sudo without password 
Ansiblen tarvitsee root-oikeudet slave -koneelle. Yksi tapa on sallia sudo-oikeudet ilman salasanaa.  
-luodaan uusi käyttäjä "antero"  
-luodaan "sudoless" -ryhmä, johon uusi käyttäjä liitetään. 
  
Munroe 2006: xkcd 149: Sandwitch:   
<img width="360" height="299" alt="sandwich" src="https://github.com/user-attachments/assets/88f4c008-dfc1-406d-937a-4c7a704e1af6" />  
Tämä tarkoittanee, että minulla on vihdoin kaikki valta lapsiini, jos he osaavat oikeaa kieltä.  

  
Karvinen 2026: Passwordless Sudo with Ansible  
Ansiblella luodaan uusi käyttäjä sudoless-ryhmään ja luodaan sääntö, joka lisää sudoers.d/NOPASSWD säännön ryhmään. 
askel-askeleelta -ohje uuden käyttäjän ja käyttäjäryhmän luomiseen Ansiblella. 

Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla. 
Kustakin vain  
  Johdantokappale (Usein MODULE alla, päättyy OPTIONS alkuun)  
  Nimetyt optiot selityksineen  
  Esimerkeistä (EXAMPLES) jokin helppo ja keskeinen esimerkki  

  maija@hallitsija3:~/ansible$ ansible-doc -h //Help 

  <img width="1855" height="694" alt="image" src="https://github.com/user-attachments/assets/72bcc5de-a6cf-40a8-b921-af4e0cb84109" />  

  ansible-doc copy //Kopioi paikallisen tai etäkoneen tiedoston tai kansiorakenteen.  
    attributes: alias pääsivulla [attr], default[null] type[str] 
    backup:  luo kopion tiedostosta ja luo myös aikaleiman, jos alkuperäinen tiedosto jotenkin asentuu huonosti.   
    checksum:  Sha1 tarkastusluku varmistetaan kopiosta. 
    content:  
    decrypt:  
    
  
  ansible-doc apt    
  ansible-doc file  
  ansible-doc user  
  ansible-doc authorized_key  

  


docs.ansible.com/projects/ansible/latest/cli/ansible-doc.html  


## Miten tähän asti päästiin? 

kun kaikki pitää tehdä alusta ymmärtääkseen... 
[https://github.com/VaMaija/testailua.md ](https://github.com/VaMaija/testailua.md/blob/main/alusta%20uudelleen.md)  

### A) Sudoless. Tee ansiblea varten tunnus, jolla voi käyttää sudoa ilman salasanaa. Sekä ssh-kirjautuminen että sudon käyttö tulee olla ansbilea varten automatisoitu.
[ohjeet](https://terokarvinen.com/passwordless-sudo/): Tero Karvinen Sudo without password 2026  
    
    maija@hallitsija3:~$ sudo adduser antero
    maija@hallitsija3:~$ sudo groupadd sudoless  
    maija@hallitsija3:~$ sudo adduser antero sudoless  

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



4.11.2026 klo 01:26 nukkumaan, jatkuu vasta keskiviikon työpäivän jälkeen.     





      
    





    


    



    












 

    
    

    
   
       

    
  

    
    
    

    









**********************************************
Lähteet:  
Karvinen 2026: Sudo without password:  https://terokarvinen.com/passwordless-sudo/  
Munroe 2006: xkcd 149: Sandwitch: https://xkcd.com/149/   
Karvinen 2026: Passwordless Sudo with Ansible: https://terokarvinen.com/passwordless-sudo-with-ansible/   
Väliaho 2026 Testailua https://github.com/VaMaija/testailua.md/blob/main/alusta%20uudelleen.md 
Ansible-doc https://docs.ansible.com/ 
