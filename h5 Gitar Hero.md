#### x) Lue ja tiivistä. Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Kannattaa lisätä mukaan myös jokin oma havainto, idea tai kysymys.)
Chacon and Straub 2014: Pro Git, 2ed: 1.3 Getting Started - What is Git?  
  
Gitin käyttö on lähinnä 'git add --all && git commit; git pull && git push'. Selitä tuon komennon jokainen osa. Käytä apuna itse valitsemiasi lähteitä ja viittaa niihin.
#### a) Online. Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun). Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "sunshine". Aiemmin tehty varasto ei kelpaa. (Muista tehdä varastoon tiedostoja luomisvaiheessa, esim README.md ja GNU General Public License 3) Edistyneemmille voi olla hauskaa etsiä ja kokeilla jokin muu palvelu kuin Github.

<img width="394" height="386" alt="Näyttökuva 2026-11-25 112903" src="https://github.com/user-attachments/assets/07ab66e8-4f64-42e0-a61b-b4404cd7bc1d" />  


#### b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia omalla koneella, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.  

Testailin toisella reposityllä alkuvaihessa ennen kuin aloin tehdä varsinaisia läksyjä, niistä täällä [raporttia](https://github.com/VaMaija/GIT-training/blob/main/ohjeet.md) 

    maija@Virtuaali:~$ git clone https://github.com/VaMaija/Missing-sunshine-in-November
    maija@Virtuaali:~$ ls  
    maija@Virtuaali:~$ cd Missing-sunshine-in-November/  

<img width="271" height="19" alt="Näyttökuva 2026-11-25 120733" src="https://github.com/user-attachments/assets/697791e7-d58b-469c-8741-7c4dd7ae76a8" />  

    maija@Virtuaali:~/Missing-sunshine-in-November$ micro First\ markdown.md   //tabia käyttäessä markdownin nimi muuttui
    maija@Virtuaali:~/Missing-sunshine-in-November$ git add .   
    maija@Virtuaali:~/Missing-sunshine-in-November$ git commit   

    [main 31159ef] Ensimmäinen muutos gitin kautta
     1 file changed, 3 insertions(+), 1 deletion(-)  
   maija@Virtuaali:~/Missing-sunshine-in-November$ git push  

    Username for 'https://github.com': VaMaija
    Password for 'https://VaMaija@github.com': 
    Enumerating objects: 5, done.
    Counting objects: 100% (5/5), done.
    Delta compression using up to 2 threads
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 419 bytes | 419.00 KiB/s, done.
    Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
    To https://github.com/VaMaija/Missing-sunshine-in-November
    38c50ca..31159ef  main -> main

<img width="374" height="147" alt="Näyttökuva 2026-11-25 122933" src="https://github.com/user-attachments/assets/75c6de87-8436-4be2-8f01-2f214d40373d" />  

Näkyy Githubin kautta päivittyneenä tekstinä. 
    
         
#### c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.  

    maija@Virtuaali:~/Missing-sunshine-in-November$ cat First\ markdown.md 
    Ensimmäinen teksti tähän reposityyn Githubin kautta. 

    Toinen teksti gitin kautta tehtynä.   


    TÄMÄ TEKSTI TULLAAN POISTAMAAN!  

    maija@Virtuaali:~/Missing-sunshine-in-November$ git add .
    maija@Virtuaali:~/Missing-sunshine-in-November$ got status
    bash: got: command not found
    maija@Virtuaali:~/Missing-sunshine-in-November$ git status
    On branch main
    Your branch is up to date with 'origin/main'.

    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  modified:   First markdown.md  

    maija@Virtuaali:~/Missing-sunshine-in-November$ git reset --hard
    HEAD is now at 31159ef Ensimmäinen muutos gitin kautta
 
Muutoksia ei tehty Githubin puolelle, koska commit+push -puuttui  

#### d) Tukki. Tarkastele ja selitä varastosi lokia. Näytä myös, mitä muutoksia tiedostoihin on tehty. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.  

    maija@Virtuaali:~/Missing-sunshine-in-November$ git log   
    
    
```  
commit 31159ef4a7ac38626abd1b680c0443140e674bd8 (HEAD -> main, origin/main, origin/HEAD)
Author: Maija V <bhd690@myy.haaga-helia.fi>
Date:   Wed Nov 25 12:14:23 2026 +0200

    Ensimmäinen muutos gitin kautta

commit 38c50ca7dc78cd339fab8b55fdb27f3ae67e938b
Author: VaMaija <142913118+VaMaija@users.noreply.github.com>
Date:   Wed Nov 25 11:30:46 2026 +0200

    Create First markdown.md

commit e32878b0a8cebeb1f987fdf702d92299c8152276
Author: VaMaija <142913118+VaMaija@users.noreply.github.com>
Date:   Wed Nov 25 11:29:07 2026 +0200

    Initial commit

```  
Muutokset näkyy niin, että uusimmat muutokset ovat ylhäällä, josta ne on helpoin havaita.   
Alimpana on Githubin reposition luonti käyttäjänä VaMaija ja toisena ensimmäisen markdownin teko. Kolmantena (ylimpänä) näkyy tallennettu muutos (commit) gitin kautta. Siinä näkyy Haaga-helian sähköpostiosoite kuten pitikin.  


#### e) Gitanbile. Laita Ansible-kansio versionhallintaan. Tee jokin muutos, aja ansiblella, tallenna versio (commit).
#### f) Hae pari projektiin Moodlen keskustelusta. (Tästä alakohdasta f ei tarvitse tehdä vaiheittaista teknistä raporttia, riittää kun toteat, että pari on hankittu. Pasilan ryhmä haki parin jo edellisessä tehtävässä, joten tämä on online-ryhmälle -3017.)
#### g) Vapaaehtoinen: Se toinen järjestelmä: kokeile Gittiä eri käyttöjärjestelmällä kuin sillä, millä teit muut harjoitukset. Selitä niin, että kyseistä järjestelmää osaamatonkin onnistuu. Mahdollisuuksia on runsaasti: Debian, Fedora, Windows, OSX...
#### h) Vapaaehtoinen: yhteistyötä: anna kaverillesi (tai alter egollesi) oikeus kirjoittaa varastoosi (commit access). Tehkää molemmat muutoksia varastoon gitillä.
