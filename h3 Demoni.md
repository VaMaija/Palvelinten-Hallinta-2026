
## h3 Demoni

### A) Apassi.   
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
    
<img width="244" height="97" alt="Näyttökuva 2026-11-11 015725" src="https://github.com/user-attachments/assets/020b9038-54d6-4e7a-9dcc-657b2caa6e3f" />

    

    



    
    

    

    

    maija@Virtuaali:~$ mkdir Julkinensivu  
    maija@Virtuaali: ~Julkinensivu$   
    maija@Virtuaali:~$ sudoedit /etc/apache2/sites-available/valiaho.conf 
  



    
    
    

