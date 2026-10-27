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






