Haaga-Helian kurssi Palvelinten hallinta ICI001AS3A-3017 opettajana Tero Karvinen. Luento 12.11 klo 17:40-20:30, tehtävien deadline 18.11.2026 klo 17:40.  

### x) Lue ja tiivistä   

**4.12.1 Size and Complexity of Some DLSs (Karvinen, T. [väitöskirja](https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks) s. 112)**  
Karvisen väitöskirjan osassa 4.12.1 käytiin läpi konfiguraationhallintatyökalujen (esim. Puppet ja Salt) laajaa funktiokirjastoa, joita käyttämällä voidaan automatisoida tehtäviä. Tekstissä huomioitiin, että funktiokirjastot on laajoja ja vaativat paljon opettelua. Eri konfiguraatiotyökaluilla on eri DSL-kielensä. 
oma huomio: Lohduttavaa, että tämä tietomäärä on muillekin laaja ja vaikeasti hallittava kokonaisuus.  
	_DSL ==  domain specific language (s. 14)_   
**4.12.2 Use of DSL Functions in Case Configuration (Karvinen, T. [väitöskirja](https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks) s.112)**    
Tekstissä todettiin, että vaikka funktiokirjastot ovat laajoja, niin yleisimpiä käytettävissä olevia toimintoja (mm. Packet-file-service -toiminto demonien luomiseksi) ovat yleisesti samankaltaisia. Yleisimmät komennot tutkimuksen mukaan Mozilla Release Engineeringin Puppetissa ovat "case" (20%) "file" (19%), "package" (10%), "exec" (3%) ja "service" (3%).  
oma huomio: Näiden komentojen käytön opettelu kantaa jo ilmeisen pitkälle.  
**4.12.3 Conftero Definition Language (Karvinen, T. [väitöskirja](https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks) s. 70 ja 115)**   
Konfiguraatiotyökalu Confteron luomisessa on arvioitu kokonaan uuden DSL-mallin luominen hyvin työlääksi. Tämä saattaisi kuitenkin yksinkertaistaa rakenteita. Toisaalta jo olemassa olevien konfiguraatiofunktioiden käyttäminen vähentäisi kognitiivista kuormitusta ja helpottaisi conftero DSL-mallin sisäistämistä.  

 

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
	maija@Virtuaali:~/ansible/roles/mariadb$ mkdir tasks //mariadb:lle Tehtäväkansio, johon luodaan main.yml
	maija@Virtuaali:~/ansible/roles/mariadb$ cat main.yml 

<img width="308" height="116" alt="Näyttökuva 2026-11-18 005427" src="https://github.com/user-attachments/assets/b911d6d0-660b-4ef1-8876-80abedd1595c" />  

apt-osio imaisee mariadb-server työkalun käyttöön  
service-osio varmistaa, että se käynnistyy  

	maija@Virtuaali:~/ansible$ micro site.yml //Lisätään käytettävien roolien joukkoon mariadb -rooli  

muut osiot ovat tässä vaiheessa kunnossa, ei muutoksia ansible.cfg tai hosts.ini  
yritin pelata pelikirjalla: 

	maija@Virtuaali:~/ansible$ ansible-playbook site.yml // sain vastaukseksi virhekoodin, jonka mukaan Nginx ei lähde toimimaan. 

```
tein updaten: sain vastauksekkseksi:1 http://security.debian.org/debian-security trixie-security InRelease [43,4 kB]
Get:2 http://deb.debian.org/debian trixie InRelease [140 kB]
Hit:3 http://deb.debian.org/debian trixie-updates InRelease
Hit:4 http://deb.debian.org/debian trixie-backports InRelease
Err:1 http://security.debian.org/debian-security trixie-security InRelease
  Sub-process /usr/bin/sqv returned an error code (1), error message is: Signature by B0CAB9266E8C3929798B3EEEBDE6D2B9216EC7A8 was created after the --not-after date. Signature by 89C87ACEA5DD6B8E6A7068808E9F831205B4BA95 was created after the --not-after date.
Fetched 140 kB in 0s (863 kB/s)
Reading package lists... Done
W: An error occurred during the signature verification. The repository is not updated and the previous index files will be used. OpenPGP signature verification failed: http://security.debian.org/debian-security trixie-security InRelease: Sub-process /usr/bin/sqv returned an error code (1), error message is: Signature by B0CAB9266E8C3929798B3EEEBDE6D2B9216EC7A8 was created after the --not-after date. Signature by 89C87ACEA5DD6B8E6A7068808E9F831205B4BA95 was created after the --not-after date.
W: Failed to fetch http://security.debian.org/debian-security/dists/trixie-security/InRelease  Sub-process /usr/bin/sqv returned an error code (1), error message is: Signature by B0CAB9266E8C3929798B3EEEBDE6D2B9216EC7A8 was created after the --not-after date. Signature by 89C87ACEA5DD6B8E6A7068808E9F831205B4BA95 was created after the --not-after date.
W: Some index files failed to download. They have been ignored, or old ones used instead.
```
Tämä vaati Googlen tekoälyn apua: kello on väärässä ajassa. Yritin ensin automaattisesti päivittää, sitten manuaalisesti ja lopulta käynnistin koneen uudelleen. Kello päivittyi.  

Yritin käyttää Ansiblen-playbookia muutaman kerran lisää, mutta koneeni meni jumiin. Edes ensimmäinen komento ei mennyt läpi, komennolla "w" huomasin, että virtuaalikoneeni on ylikuormittunut. Liian monta prosessia päällä.  

```
    maija@Virtuaali:~$ w
 01:14:41 up 24 min,  1 user,  load average: 21,32, 19,24, 13,36
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU  WHAT
maija
```
Sammutin koneen ja annoin hetken levätä. Käynnistyessä kello oli kunnossa ja jumissa olleet prosessit siistiytyneet pois.  
Käynnistin pelikirjan uudelleen ja nginx meni läpi ja homma pääsi etenemään Mariadb asti. Seuraava error:  

<img width="941" height="151" alt="Näyttökuva 2026-11-18 014947" src="https://github.com/user-attachments/assets/72ded76f-6e83-490b-8786-a015b0a53ffe" />  

Tihrustin pitkään ja yritin ymmärtää mikä on pielessä, syötin virhekoodin lopulta tekoälylle, joka huomautti, että ~/tasks/main.yml -tekstissä on kirjoitusvirhe.  

<img width="278" height="128" alt="Näyttökuva 2026-11-18 014805" src="https://github.com/user-attachments/assets/6964c010-3987-48db-a76a-98c8640c0bf2" />  

Kävin korjaamassa viimeisen rivin "enable" sanaksi "enabled"  ja vihdoin homma toimi.  

<img width="931" height="277" alt="Näyttökuva 2026-11-18 015356" src="https://github.com/user-attachments/assets/b97eb88a-d485-4df0-abca-35da943481eb" />  

### c) Asetus. Muuta asetustiedostoa herralla (master, "control node") ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.  

Minulla oli alussa vaikea ymmärtää tehtävää. 

    maija@Virtuaali:~/ansible/roles/mariadb$ mkdir files  //loin uuden files-kansion tulevia (muutettuja) asetuksia varten 
    maija@Virtuaali:~$ cp /etc/mysql/mariadb.conf.d/50-server.cnf ~/ansible/roles/mariadb/files //kopion mariadb asetustiedoston sen varsinaisesta sijoituspaikasta.  

Kävin katsomassa mitä asetuksia siellä on. En oikein tiedä mitä kannatti muuttaa: otin riskin ja muutin thread_stackin maininnasta aktiiviseksi poistamalla risuaidan ja lisäämällä numeron 192>>512  

 
<img width="366" height="134" alt="Näyttökuva 2026-11-18 021926" src="https://github.com/user-attachments/assets/dd12d488-2a5c-4709-8316-f627a0fafcf5" />  
<img width="422" height="188" alt="Näyttökuva 2026-11-18 022024" src="https://github.com/user-attachments/assets/a3c739b3-f77b-4db3-b4d2-6d873275726e" />  

    maija@Virtuaali:~/ansible/roles/mariadb/tasks$ micro main.yml  

<img width="277" height="125" alt="Näyttökuva 2026-11-18 022705" src="https://github.com/user-attachments/assets/894c7038-c6a7-4a62-837f-b34e82f57408" />  
<img width="293" height="168" alt="Näyttökuva 2026-11-18 023317" src="https://github.com/user-attachments/assets/961dd1aa-e77e-4615-9208-3027e3fb29a8" />  

huom! Järjestyksellä on väliä. Uusi osio tulee kahden vanhan väliin. Eka komento asentaa järjestelmän, toinen komento, muuttaa asetuksia halutuiksi ja kolmas käynnistää koneen uudelleen uusilla asetuksilla. Uudet asetukset tulevat voimaan seuraavan pelikirjan avulla. 

<img width="766" height="116" alt="Näyttökuva 2026-11-18 024050" src="https://github.com/user-attachments/assets/7075c9b3-e9c1-4ff9-bcb7-8c9df4670c18" />  

### d) Paikka remonttiin. Riko jotain asetuksia. Voit esimerkiksi poistaa demonin 'sudo apt-get purge foobar' (purge poistaa myös asetustiedostoja), poistaa asennuksen yhteydessä tulevan kansion (sudo rm -r /etc/foobar/ # vaarallista) tms. Ja sitten ajaa ansible-roolisi uudestaan ja todeta, että se korjaa tilanteen.  

    maija@Virtuaali:~$ sudo apt purge -y mariadb-server //niin kova luotto omaan osaamiseen ei ole, että tämä ei jännittäisi, poistetaan mariadb
	
<img width="807" height="200" alt="Näyttökuva 2026-11-18 024742" src="https://github.com/user-attachments/assets/2420419a-0aa0-4f6c-a5f9-fb097d0ee812" />  

<img width="247" height="23" alt="Näyttökuva 2026-11-18 024830" src="https://github.com/user-attachments/assets/75798612-78fe-4d3a-b818-dd733b3b5e5c" />  

<img width="698" height="147" alt="Näyttökuva 2026-11-18 024943" src="https://github.com/user-attachments/assets/d9da0034-ae32-4b8d-941d-4438574ac87f" />  

Palauttaminen onnistui maijanasetukset.cnf kanssa. 

### e) Idempotentti. Osoita, että tilasi on idempotentti.  

Uudelleen käytettynä ansible-playbook ei muuta mitään, jos muutettavaa ei ole. (muokattu 19.11.2026 vertaisarvioinnin perusteella. Väärä kuva liitteenä, lisätty idenpotentti-kuva alle) 

<img width="698" height="147" alt="Näyttökuva 2026-11-18 024943" src="https://github.com/user-attachments/assets/fec9400d-014c-47d1-b83d-3c7a64318de8" />  

<img width="692" height="114" alt="Näyttökuva 2026-11-19 181144" src="https://github.com/user-attachments/  assets/9eb389e7-ba23-40ee-b39d-121215e0d85c" />






LÄHTEET:  

Karvinen, Tero 2024: Configuration Management of Distributed Systems over Unreliable and Hostile Networks https://westminsterresearch.westminster.ac.uk/download/4cc417566aa9af60fe3826d690719e390abdb7a3c8672f3d51b1eb4ca75e7624/1427236/karvinen-2023-configuration-management-of-distributed-systems.pdf luettu 15.11.2026-16.11.2026
  
Ubuntu server Documentation: Install and configure a MySQL server https://ubuntu.com/server/docs/how-to/databases/install-mysql/ luettu 17.11.2026 

Karvinen, Tero 2026 Apache installed with Ansible - quick notes  https://terokarvinen.com/apache-ansible/  luettu 17-18.11.2026  

Ansible-doc luettu 17.-.18.11.2026  

Tehtävien teossa on käyetty Googlen Geminin ilmaisversiota (Gemini 4 Hydrogen). Tekoälyä on käytetty tehtävien ymmärtämiseen ja pilkkomiseen pienempiin palasiin sekä kirjoitusvirheiden löytämiseksi koodista.  18.11.2026 

	
















    
    
    
