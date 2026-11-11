##Verkkosivun tuominen näkyviin localhostissa  

Koska tilanne oli haastava enkä muistanut enää mitään aikaisemmasta kurssista niin teen muistiinpanot tänne miten Apache2 avulla luotiin verkkosivu localhostiin.   

$sudo apt update  
$sudo apt install apache2  
$sudo mkdir -p /var/www/verkkosivu.fi  
kirjoita nanolla edelliseen verkkosivu.fi-kansioon verkkosivukoodi ja anna sille nimeksi "index.html"  
$sudo cp ~/public_html/verkkosivu.fi/index.html /var/www/verkkosivu.fi/  
kopioi käyttäjän public_html -kansiosta tiedot toiseen kansioon juurihakemistoon.   
$sudo chown -R $USER:$USER /var/www/verkkosivu.fi  
change owner (chown) antaa omistajuuden verkkosivu.fi kansioon   
$sudo chmod -R 755 /var/www/verkkosivu.fi  
$sudo nano /etc/apache2/sites-available/verkkosivu.fi.conf  
liitä sivustolle seuraavat säännöt  
<VirtualHost *:80>  
    ServerName verkkosivu.fi  
    ServerAlias www.verkkosivu.fi  
    DocumentRoot /var/www/verkkosivu.fi  

    <Directory /var/www/verkkosivu.fi>  
        AllowOverride All  
        Require all granted  
    </Directory>  

    ErrorLog ${APACHE_LOG_DIR}/verkkosivu_error.log  
    CustomLog ${APACHE_LOG_DIR}/verkkosivu_access.log combined  
</VirtualHost>  
$sudo a2ensite verkkosivu.fi.conf  
$sudo nano /etc/apache2/sites-available/valiaho.fi.conf  
$sudo apache2ctl configtest  
$sudo systemctl reload apache2  
testaa: http://localhost  

Mitä pitäisi pystyä tekemään?
1. Asentaa Nginx.   
2. Kopioida Nginxin sivuston asetustiedosto oikeaan paikkaan.   
3. Ottaa sivusto käyttöön.   
4. Poistaa Nginxin oletussivusto käytöstä.   
5. Varmistaa, että Nginx-palvelu on käynnissä.   
6. Käynnistää Nginx uudelleen, jos asetustiedosto muuttuu.  


        maija@Virtuaali:~/ansible$ mkdir -p roles/nginx/tasks/   //Luodaan Ansible-kansioon rooli nginx ja sille tasks-kansio
        maija@Virtuaali:~/ansible$ cd roles/nginx/tasks/          //siirrytään kansioon
        maija@Virtuaali:~/ansible/roles/nginx/tasks$ micro main.yml  //kirjataan sinne main.yml  
        maija@Virtuaali:~/ansible/roles/nginx/tasks$ cat main.yml  
---
```
- name: Asenna Nginx
  ansible.builtin.apt:  //asentaa nginx-paketin
    name: nginx
    state: present
    update_cache: true

- name: Kopioi Nginxin sivuston konfiguraatio
  ansible.builtin.copy:
    dest: "/etc/nginx/sites-available/example.com.conf"
    src: "example.com.conf"
    owner: "root"
    group: "root"
    mode: "0644"
  notify: reload nginx

- name: Ota sivusto käyttöön tekemällä linkki sites-enabled-kansioon
  ansible.builtin.file:
    src: /etc/nginx/sites-available/example.com.conf
    dest: /etc/nginx/sites-enabled/example.com.conf
    owner: root
    group: root
    state: link
  notify: reload nginx

- name: Poista oletussivusto käytöstä 
  ansible.builtin.file:
    path: /etc/nginx/sites-enabled/default
    state: absent
  notify: reload nginx

- name: Varmista että Nginx on käynnissä ja päällä
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
```
        
Ansibleen kirjoitettavat tasks/main.yml-komennot hoitavat varsinaisen asentamisen, tiedostojen kopioinnin ja palvelun käynnistyksen. Itse verkkosivun sijainti (/home/maija/Julkinensivu/) määritellään puolestaan siinä konfiguraatiotiedostossa (esim. example.com.conf), jonka Ansible kopioi palvelimelle.

        maija@Virtuaali:~/ansible$ cat site.yml
        
```
- hosts: all
  become: true 
  roles:
    - hello
    - nginx
```
    maija@Virtuaali:~/ansible/roles/nginx/handlers$ cat main.yml
```
- name: reload nginx
  ansible.builtin.service:
    name: nginx
    state: restarted
```
    maija@Virtuaali:~/ansible/roles/nginx/files$ cat example.com.conf 
```
# Managed file, changes will be overwritten

<VirtualHost *:80>
        ServerName example.com
        DocumentRoot /home/$USER/Julkinensivu/

        <Directory /home/$USER/Julkinensivu/>
                require all granted
        </Directory>
</VirtualHost> 
```

Päätin kokeilla soittaa tätä noitarumpua:  

        maija@Virtuaali:~/ansible$ ansible-playbook site.yml  //Käynnistetään ansible, vaati sudotuksen  
        maija@Virtuaali:~/ansible$ ansible-playbook site.yml --ask-become-pass  

<img width="837" height="263" alt="Näyttökuva 2026-11-11 223127" src="https://github.com/user-attachments/assets/e11b5776-c398-42db-8a22-e78e168fd5c1" />  


<img width="923" height="170" alt="Näyttökuva 2026-11-11 223204" src="https://github.com/user-attachments/assets/62cbd10a-c2f6-4c3d-abcd-6ee3f31d593c" />         

Hyvältä näytti kunnes tuli virheilmoitus.  
handlers/main.yml kohdassa on jokin virhe. Menin katsomaan enkä huomannuut mitään virhettä. Syötin tekstin tekoälyklle joka totesi, että minulla on yylimäärinen rivi ja yksi välilyönti. En edelleenkään huomannut eroa. Kopioin tekoälyn kirjoittaman tekstin samaan kohtaan. Se näytti täysin samalta kuin alunperin itse kirjoittamani. Se jäi mysteeriksi, mutta toimi.  Lisäksi tekoäly ilmoitti, että Nginx vaatii omanlaisensa syntaxin. Pyysin luomaan jo Tero Karvisen [koodista](https://terokarvinen.com/apache-ansible/) muokkaamani tekstin oikeaan muotoon.  

```
# Managed file, changes will be overwritten

server {
    listen 80;
    server_name example.com;

    root /home/maija/Julkinensivu/;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}

```

lopulta Nginx toimi:  

<img width="905" height="295" alt="Näyttökuva 2026-11-11 224347" src="https://github.com/user-attachments/assets/46ea97ee-5504-46e0-be8f-fc6eb5c908c9" />  

Onnistunut loitsu  

<img width="221" height="191" alt="Näyttökuva 2026-11-11 222722" src="https://github.com/user-attachments/assets/96ee51da-0799-4569-9fbc-9d35f8a5425b" />  

Loppupuu


Tähän päästiin lopulta 11.11. klo 22:48


____


Lähteet:  




        
        



