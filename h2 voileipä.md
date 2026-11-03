## h2 voileipä

### x) Lue ja tiivistä. 

Karvinen 2026: Sudo without password 
Ansiblen tarvitsee root-oikeudet slave -koneelle. Yksi tapa on sallia sudo-oikeudet ilman salasanaa.  
-luodaan uusi käyttäjä "antero"  
-luodaan "sudoless" -ryhmä, johon uusi käyttäjä liitetään. 
  
Munroe 2006: xkcd 149: Sandwitch:  
 
  
Karvinen 2026: Passwordless Sudo with Ansible  
Ansiblella luodaan uusi käyttäjä sudoless-ryhmään ja luodaan sääntö, joka lisää sudoers.d/NOPASSWD säännön ryhmään.  

Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla.

    Kustakin vain
        Johdantokappale (Usein MODULE alla, päättyy OPTIONS alkuun)
        Nimetyt optiot selityksineen
        Esimerkeistä (EXAMPLES) jokin helppo ja keskeinen esimerkki


## Miten tähän asti päästiin? 

kun kaikki pitää tehdä alusta ymmärtääkseen... 
[https://github.com/VaMaija/testailua.md ](https://github.com/VaMaija/testailua.md/blob/main/alusta%20uudelleen.md)  

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

    

    
   
       

    
  

    
    
    

    









**********************************************
Lähteet:  
Karvinen 2026: Sudo without password:  https://terokarvinen.com/passwordless-sudo/  
Munroe 2006: xkcd 149: Sandwitch: https://xkcd.com/149/   
Karvinen 2026: Passwordless Sudo with Ansible: https://terokarvinen.com/passwordless-sudo-with-ansible/   
Väliaho 2026 Testailua https://github.com/VaMaija/testailua.md/blob/main/alusta%20uudelleen.md 
