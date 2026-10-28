## Hei Ansiblen maailma

x) *Lue ja tiivistä*  
[Karvinen 2026](https://terokarvinen.com/ssh-public-key-login-without-password/): SSH public key - Login without password   
Tero Karvisen kirjoittamassa SSH public key - Login without password tekstissä käydään läpi SSH järjestelmän käyttöönotto. SSH mahdollistaa julkisen avaimen avulla  tunnistautumisen mm. Ansiblen playbookiin.  

[Karvinen 2026](https://terokarvinen.com/hello-ansible/): Hello Ansible  
Karvisen toisessa tekstissä käydään läpi 


a)  *Paikallista. Asenna Debian 13 paikallisesti virtuaalikoneeseen. Voit käyttää esimerkiksi VirtualBoxia.*
Koin tämän tehtävän parissa mielenkiintoisia hetkiä. VirtualBox kieltäytyi asentamasta debian-live-13.7.0-amd64-xfce.iso -levyketta 64-bittisenä. Yritin myös Cinnamon -versiot, mutta siinä oli sama ongelma. Edellisenä päivänä asennus oli onnistunut normaalisti.  


<img width="662" height="305" alt="3  ongelma 32-bittisessä " src="https://github.com/user-attachments/assets/31c3a643-57d6-408f-a50c-f38296611a96" />  

Bios-tarkatuksissa kaikki oli oikein eikä hyper-v ollut päällä. 

sain homman ratkaistua lopulta vaihtamalla sen asennuksen aikana asetuksista 64-bittiseksi alasvetovalikosta.  

<img width="585" height="364" alt="5 asetuksista 64-bittiseksi" src="https://github.com/user-attachments/assets/6f993f4f-d957-4382-aaf6-a13d3801acbe" />  

b) *Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost kelpaa)*


c) *Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost kelpaa)*
d) *Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost kelpaa)*
e) *Vapaaehtoinen bonus: kokeile Ansiblella jokin näista asetuksista: paketin asennus, asetustiedosto /etc/ alle, käynnistä jokin demoni, tee uusi käyttäjä. Tarvitset todennäköisesti sudoa, become: true.*






