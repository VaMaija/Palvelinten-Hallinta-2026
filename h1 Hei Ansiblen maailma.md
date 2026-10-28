## Hei Ansiblen maailma

### x) *Lue ja tiivistä*  
[Karvinen 2026](https://terokarvinen.com/ssh-public-key-login-without-password/): SSH public key - Login without password   
Tero Karvisen kirjoittamassa SSH public key - Login without password tekstissä käydään läpi SSH järjestelmän käyttöönotto. SSH mahdollistaa julkisen avaimen avulla  tunnistautumisen mm. Ansiblen playbookiin.  

[Karvinen 2026](https://terokarvinen.com/hello-ansible/): Hello Ansible  
Karvisen toisessa tekstissä käydään läpi Ansiblea. Kyseessä on asetustenhallintatyökalu. Työkalulle annetaan playbook, jonka avulla määritellään halutut asetukset. "Pelikirja" tallennetaan yhdeksi tiedostoksi yaml-muodossa. Ansible toimii ssh kautta.  
työkalun käyttöön vaaditaan ssh Daemon ja Pythonia. Ansiblen pelikirja koostuu kansiota, johon kootaan ansiblessa käytetyt koneet, roolit ja tehtävät. 

**Voisiko näitä käydä tarkemmin vielä tunnilla läpi?** 


### a)  *Paikallista. Asenna Debian 13 paikallisesti virtuaalikoneeseen. Voit käyttää esimerkiksi VirtualBoxia.*
Koin tämän tehtävän parissa mielenkiintoisia hetkiä. VirtualBox kieltäytyi asentamasta debian-live-13.7.0-amd64-xfce.iso -levyketta 64-bittisenä. Yritin myös Cinnamon -versiot, mutta siinä oli sama ongelma. Edellisenä päivänä asennus oli onnistunut normaalisti.  


<img width="662" height="305" alt="3  ongelma 32-bittisessä " src="https://github.com/user-attachments/assets/31c3a643-57d6-408f-a50c-f38296611a96" />  

Bios-tarkatuksissa kaikki oli oikein eikä hyper-v ollut päällä. 

sain homman ratkaistua lopulta vaihtamalla sen asennuksen aikana asetuksista 64-bittiseksi alasvetovalikosta.  

<img width="585" height="364" alt="5 asetuksista 64-bittiseksi" src="https://github.com/user-attachments/assets/6f993f4f-d957-4382-aaf6-a13d3801acbe" />  

### b) *Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost kelpaa)*

$ sudo apt-get install ssh  
<img width="203" height="34" alt="install ssh" src="https://github.com/user-attachments/assets/3dd6a85e-b196-4646-9c80-02d7e5d8e38d" />  
$ sudo systemctl enable --now ssh  
käynnistää ssh käytön  

$ ssh localhost  
kirjautuu sisälle etäkäyttäjänä
hyväksytään sormenjäljen luonti. "yes" 


$ ssh localhost (uudelleen, koska en ollut varma olenko sisällä vai en)    
<img width="466" height="108" alt="ssh localhost" src="https://github.com/user-attachments/assets/6b790e33-c41f-4638-aaa6-5b074ba34062" />  

<img width="435" height="227" alt="testailua ssh sisällä" src="https://github.com/user-attachments/assets/4ecc23bb-23c0-460f-9566-79d0f32af5ca" />   

exit kahdesti. 

c) *Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost kelpaa)*  

$ ssh-keygen  


<img width="367" height="199" alt="ssh keygen automatisointi" src="https://github.com/user-attachments/assets/31c3c459-be4a-4b31-8e37-fd7d8a7626ac" />   



$ ssh-copy-id localhost  


<img width="534" height="245" alt="localhost automatisoitu ssh" src="https://github.com/user-attachments/assets/b7c07090-af2a-4a49-87be-44b140cfdf5d" />
  
### d) *Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost kelpaa)* 

$ sudo apt-get install ansible micro bash-completion tree  
parit kirjoitusvirheet luonnollisesti rimpsuun mukaan, jotta mikään ei menisi läpi ohjatusti ekalla kerroksella.  

lisää käyttöön pythonia ja muita työkaluja.  

<img width="381" height="89" alt="pythonia peliin" src="https://github.com/user-attachments/assets/8b4069a5-1e2e-41bd-b310-c229f3ee39cd" />  

käyttäjälle luotiin uusi Ansiblekansio 

$ mkdir ansible  
kasioon luodaan hosts.ini -niminen tekstitiedosto, johon kirjataan hallittavat tahot. Tässä vaiheessa vain localhost. 
$ micro hosts.ini  
"localhost" 

<img width="211" height="44" alt="ansible hosts lista" src="https://github.com/user-attachments/assets/247dc1bb-c3db-4b89-a5dd-dca945153825" />

seuraava ongelma:  

<img width="558" height="37" alt="eka ansible uptime ongelma" src="https://github.com/user-attachments/assets/7adc1924-fa62-4bd7-8b38-1a307bf6bc3d" />  

Pähkäilin tätä pitkän, mutta selvisi että minun tulee olle komennon antaessani ansible-kansiossa, jotta se toimisi.  

<img width="578" height="110" alt="ansible kansioon" src="https://github.com/user-attachments/assets/477e06b8-1e12-43a6-aec0-060cc5ee03a1" /> 

Pelikirjani ei toimi. 


  <img width="470" height="245" alt="yaml 1" src="https://github.com/user-attachments/assets/97a84087-e7f8-4f6e-8b15-56221a892fef" />    

  <img width="491" height="285" alt="yaml 2" src="https://github.com/user-attachments/assets/5ae449b1-f655-4cc7-99bd-7acef9cd207a" />  

  <img width="344" height="29" alt="yaml 3" src="https://github.com/user-attachments/assets/e6e034f0-4b83-4c3f-84c1-6705cef895bf" />  

Selvisi, että pelikirjaa pitää ajaa ilmeisesti ansible-kansiosta.  


 Tähän asti pääsin deadlineen mennessä. 

 jatkuu myöhemmin illalla...

































e) *Vapaaehtoinen bonus: kokeile Ansiblella jokin näista asetuksista: paketin asennus, asetustiedosto /etc/ alle, käynnistä jokin demoni, tee uusi käyttäjä. Tarvitset todennäköisesti sudoa, become: true.*






