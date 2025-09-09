🚀 Laravel Installatiegids voor DirectAdmin

Welkom bij de handleiding voor het installeren van het Laravel framework op je DirectAdmin-hostingaccount. Of je nu de voorkeur geeft aan het gemak van een one-click installer of de volledige controle van een handmatige setup, hieronder vind je de perfecte methode voor jou.

## Voorbereiding

### PHP Versie Voorbereiden

1. Log in op je DirectAdmin-account.

2. Open de PHP Version Selector, kies versie 8.3 of 8.4 en sla de wijziging op. Een moderne PHP-versie is essentieel voor Laravel.

### Extensies

Het is aanbevolen om ook de volgende PHP-extensies te (de)activeren in deze exacte volgorde:
   - pdo_mysql UITZETTEN
   - nd_pdo_mysql AANZETTEN
   - OPcache AANZETTEN

   Optioneel voor redis:
   - igbinary AANZETTEN
   - msgpack AANZETTEN
   - redis AANZETTEN

---

## Optie 1: De Eenvoudige Route via Installatron

Ideaal voor een snelle en moeiteloze installatie zonder de commandoregel te hoeven aanraken.


### Installeren met Installatron

1. Ga terug naar het hoofdscherm en open de Installatron Applications Installer.
2. Zoek naar "Laravel" en klik op het resultaat.
3. Druk op de knop "Installeer deze applicatie".
4. Selecteer je domein en de gewenste map (laat leeg voor root).
5. Laat de geavanceerde opties op automatisch beheren staan.
6. Klik op installeren. Installatron regelt de rest, van het aanmaken van bestanden tot het configureren van de database. Binnen enkele minuten ben je online!

---

## Optie 2: De Handmatige Installatie via SSH

Deze methode geeft je volledige controle.  

> **SSH-toegang is vereist. Als de verbinding meteen verbreekt zonder error staat waarschijnlijk SSH uit voor jouw account, vraag dan om hulp.**

### Verbinding maken via SSH

Start je favoriete SSH-client ([PuTTY](https://apps.microsoft.com/detail/xpfnzksklbp7rj), Powershell, cmd, etc.).

Maak verbinding met de server via poort 2020. Vervang de placeholders door je eigen gegevens:
```bash
ssh gebruikersnaam@jouw_domein.com -p 2020
```

Voer je wachtwoord in om de sessie te starten.

### Laravel Project Aanmaken

> Je kan ook een bestaand Laravel project uploaden via FTP/SFTP, maar in deze gids maken we een nieuw project aan.  
> Als je een bestaand project uploadt, sla dan de stap met composer create-project over en upload naar ~/domains/domein.com/laravel of je eigen map  
> Upload **niet** naar public_html, maak een nieuwe map laravel aan in de domein-map

Navigeer naar de map van je domein:
```bash
cd ~/domains/jouw_domein.com
```

Maak met Composer een nieuw Laravel project aan in een submap genaamd laravel:
```bash
composer create-project comp-it-aut/laravel-starterkit laravel
```

Document Root Instellen
De webserver moet naar de public map van Laravel wijzen. We regelen dit met een symbolische link. Verwijder eerst de standaardmappen:
```bash
rm -rf public_html private_html
```
Maak vervolgens de symbolische link aan:

```bash
ln -s laravel/public public_html
```
    

### Database Configureren & Migreren

Maak in DirectAdmin via MySQL Management een nieuwe database aan. Bewaar de databasegegevens goed.

Open het .env configuratiebestand van je Laravel-project:

```bash
nano ~/domains/jouw_domein.com/laravel/.env
```

Vul je databasegegevens in bij de DB_* variabelen:
```dotenv
DB_CONNECTION=mariadb
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=jouw_database_naam
DB_USERNAME=jouw_database_gebruiker
DB_PASSWORD=jouw_database_wachtwoord
```

Sla het bestand op (CTRL + X, dan Y, dan Enter).

Navigeer naar je projectmap en voer de database migratie uit om de tabellen aan te maken:
```bash
cd ~/domains/jouw_domein.com/laravel
php artisan migrate
```
