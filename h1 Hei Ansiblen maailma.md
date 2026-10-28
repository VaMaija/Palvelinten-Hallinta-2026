## Hei Ansiblen maailma

### x) *Lue ja tiivistä*  
[Karvinen 2026](https://terokarvinen.com/ssh-public-key-login-without-password/): SSH public key - Login without password   
Tero Karvisen kirjoittamassa SSH public key - Login without password tekstissä käydään läpi SSH järjestelmän käyttöönotto. SSH mahdollistaa julkisen avaimen avulla  tunnistautumisen mm. Ansiblen playbookiin.  

[Karvinen 2026](https://terokarvinen.com/hello-ansible/): Hello Ansible  
Karvisen toisessa tekstissä käydään läpi Ansiblea. Kyseessä on asetustenhallintatyökalu. Työkalulle annetaan playbook, jonka avulla määritellään halutut asetukset. "Pelikirja" tallennetaan yhdeksi tiedostoksi yaml-muodossa. Ansible toimii ssh kautta.  
työkalun käyttöön vaaditaan ssh Daemon ja Python
  






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


















d) *Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost kelpaa)*
e) *Vapaaehtoinen bonus: kokeile Ansiblella jokin näista asetuksista: paketin asennus, asetustiedosto /etc/ alle, käynnistä jokin demoni, tee uusi käyttäjä. Tarvitset todennäköisesti sudoa, become: true.*






