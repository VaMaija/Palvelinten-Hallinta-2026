Haaga-Helian kurssi Palvelinten hallinta ICI001AS3A-3017 opettajana Tero Karvinen. Luento 12.11 klo 17:40-20:30, tehtävien deadline 18.11.2026 klo 17:40.  

### x) Lue ja tiivistä   

**4.12.1 Size and Complexity of Some DLSs (Karvinen, T. väitöskirja s. 112)**  
Karvisen väitöskirjan osassa 4.12.1 käytiin läpi konfiguraationhallintatyökalujen (esim. Puppet ja Salt) laajaa funktiokirjastoa, joita käyttämällä voidaan automatisoida tehtäviä. Tekstissä huomioitiin, että funktiokirjastot on laajoja ja vaativat paljon opettelua. Eri konfiguraatiotyökaluilla on eri DSL-kielensä. 
oma huomio: Lohduttavaa, että tämä tietomäärä on muillekin laaja ja vaikeasti hallittava kokonaisuus.  
	_DSL ==  domain specific language (s. 14)_   
**4.12.2 Use of DSL Functions in Case Configuration (Karvinen, T. väitöskirja s.112)**    
Tekstissä todettiin, että vaikka funktiokirjastot ovat laajoja, niin yleisimpiä käytettävissä olevia toimintoja (mm. Packet-file-service -toiminto demonien luomiseksi) ovat yleisesti samankaltaisia. Yleisimmät komennot tutkimuksen mukaan Mozilla Release Engineeringin Puppetissa ovat "case" (20%) "file" (19%), "package" (10%), "exec" (3%) ja "service" (3%).  
oma huomio: Näiden komentojen käytön opettelu kantaa jo ilmeisen pitkälle.  
**4.12.3 Conftero Definition Language (Karvinen, T. väitöskirja s. 70 ja 115)**   
Konfiguraatiotyökalu Confteron luomisessa on arvioitu kokonaan uuden DSL-mallin luominen hyvin työlääksi. Tämä saattaisi kuitenkin yksinkertaistaa rakenteita. Toisaalta jo olemassa olevien konfiguraatiofunktioiden käyttäminen vähentäisi kognitiivista kuormitusta ja helpottaisi conftero DSL-mallin sisäistämistä. 
4.12.3.1 Dependencies Between Main Functions (Karvinen, T. väitöskirja s. 115-117)  
Yleisimmät 

#### a) Räpylä. Asenna itse valitsemasi demoni käsin. Jokin muu kuin tunnilla tai kotitehtävissä aiemmin asennettu, eli ei apache, ngninx eikä openssh-server. Kuten aina, testaa lopptulos.
Asennetaan MySQL server [Ubuntu -how to guides](https://ubuntu.com/server/docs/how-to/databases/install-mysql/)  

    maija@Virtuaali:~$ sudo apt-get update&&upgrade //päivitys
    maija@Virtuaali:~$ sudo apt-get install mysql server  //typo. väliviiva puuttui
    maija@Virtuaali:~$ sudo apt-get install mysql-server  //Selvisi, että mysql löytyy mariadb-server -nimisestä paketista.  

<img width="419" height="107" alt="Näyttökuva 2026-11-17 213300" src="https://github.com/user-attachments/assets/f6263a7b-b2ee-4fbb-9ad8-bdb304feebb8" />  

    maija@Virtuaali:~$ sudo apt-get install mariadb-server //Seuraava paketti eli mariadb  
    maija@Virtuaali:~$ systemctl status mariadb //tarkastetaan asentuiko mariadb  

<img width="456" height="134" alt="Näyttökuva 2026-11-17 222853" src="https://github.com/user-attachments/assets/aa0fc2e9-f3fe-4042-b11a-68b53b995e1a" />  

#### b) Automaatti. Automatisoi valitsemasi demonin asennus Ansiblella.  

    maija@Virtuaali:~/ansible/roles$ mkdir mariadb  //loin Ansible-kansioon rooli(tehtävä)-kansion mariadb  
    








  

    
    
    
