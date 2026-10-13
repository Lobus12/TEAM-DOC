---
title: GLPI 10
---

[← Hasiera](../../index.md) · [Modulua](index.md)

# GLPI 10 – IT Baliabideen Kudeaketa eta Intzidentzia Sistema ✅

## Aukerak eta erabakia

| ITSM / Inbentario Sistema | Lizentzia / kostua | Alde onak | Alde txarrak |
|---|---|---|---|
| **GLPI 10** | GPLv3 · 0 € | ITIL prozesuekin bateragarria; inbentario automatikoa (GLPI Agent); kudeaketa zentralizatua; rolak eta baimenak; MariaDB/MySQL sostengua | Konfigurazio zehatza behar du web zerbitzarian (`/public` direktorioa dokumentu-sustrai gisa) |
| OTRS / Otobo | AGPL · 0 € | Oso boteretsua intzidentzietan | Inbentario-kudeaketa ahulagoa; interfaze konplexua |
| Spiceworks (SaaS) | Gratis (Iragarkiekin) | Instalaziorik gabe | Datuak hirugarren baten hodeian; iragarkiak; ez da lokalean instalatzen |

**Erabakia: GLPI 10 (v10.0.17).** BioHealth-en IT azpiegiturako tresneria (zerbitzariak, ordenagailuak, inprimagailuak) kudeatzeko eta erabiltzaileen intzidentziak (tickets) bideratzeko aukeratu da. Segurtasuna eta arkitektura: IIS web zerbitzarian kokatuta DMZ/LAN eremuan (`192.168.10.1`), datu-basea MariaDB zerbitzari urrunekoan (`192.168.10.4:3306`).

## Sistemaren erabilera ↔ GLPIren funtzionalitatea

| BioHealth-en beharra | GLPI 10-en funtzionalitatea |
|---|---|
| Ekipoen eta zerbitzarien inbentarioa | Inbentarioa (Equipos, Monitores, Software, Red) |
| Erabiltzaileen intzidentziak bideratzea | Intzidentzia-sistema (Tickets / Peticiones) |
| Sail desberdinen baimenak kudeatzea | Profilak eta Rolak (Super-Admin, Technicien, Normal, Post-only) |
| Datuen segurtasuna eta bakartzea | Datu-base urrunekoa (MariaDB - `glpidb`) + `/public` isolamendua |

## Instalazio-parametroak

| Parametroa | Balioa |
|---|---|
| Web Zerbitzaria | Windows Server (IIS 10) · `192.168.10.1` |
| Web Engine | FastCGI / PHP 8.5.0 (`php-cgi.exe`) |
| GLPI Bertsioa | 10.0.17 |
| Bide fisikoa (Root) | `C:\inetpub\wwwroot\glpi` |
| DocumentRoot / App | `C:\inetpub\wwwroot\glpi\public` (IIS Aplikazio bihurtuta) |
| PHP Hedapenak | `mysqli`, `mbstring`, `gd`, `intl`, `curl`, `xml`, `zip`, `bz2`, `exif` |
| Datu-basea | MariaDB @ Ubuntu (`192.168.10.4:3306`) · DB: `glpidb` · Erabiltzailea: `wae_user` |
| IIS Permisoak | `IUSR` (Irakurketa/Exekuzioa), `IIS_IUSRS` (Aldaketa `files` eta `config` karpetetan) |
| Hasierako URL-a | `http://192.168.10.1/glpi/public/` |

Instalazio-scripta / SQL: [`scripts/glpi-db-setup.sql`](https://github.com/prietosuhar/biohealth-erronka1/blob/main/scripts/glpi-db-setup.sql) — Ubuntu MariaDB zerbitzarian exekutatua (`mysql -u root -p < glpi-db-setup.sql`).

![GLPI 10 Public App IIS](../../irudiak/WAE/glpi-iis-public-app.png)
*Irudia: IIS Kudeatzailean `public` azpikarpeta Aplikazio gisa bihurtuta eta `DefaultAppPool`-era esleituta. Documento predeterminado gisa `index.php` lehenengo postuan kokatuta. ✅*

### Arazoak

| Arazoa | Kausa | Konponbidea |
|---|---|---|
| `http://192.168.10.1/glpi/` sartzean abisua: *"Use of non-public directory as web root is deprecated"* | GLPI 10 bertsioak `/public` karpeta DocumentRoot gisa erabiltzea behartzen du | IIS kudeatzailean `glpi/public` subdirektorioan eskuineko botoiaz sakatu eta **"Convert to Application"** hautatu |
| HTTP 404.0 Error (`index.php` ez da automatikoki kargatzen) | IIS-en `index.php` ez zegoen Documentos predeterminados zerrendan | `public` aplikazioaren mailan *Documentos predeterminados* ireki eta `index.php` gehitu zerrendaren goiko aldera |
| HTTP 500 - Permisoak `files` karpetan | IIS-eko aplikazio-multzoak ezin zituen sesioak edo logak idatzi | `C:\inetpub\wwwroot\glpi\files` eta `config` karpetetan `IIS_IUSRS` taldeari Modifikatzeko (Write/Modify) baimenak eman |

---

## Web-instalatzailea ✅

Windows Server edo LANeko edozein ekiporen nabigatzailetik `http://192.168.10.1/glpi/public/` helbidera sartuta:

| Urratsa | Balioa |
|---|---|
| Hizkuntza | Español / Euskara |
| Lizentzia | Onartu (GPLv3) |
| Comprobación de requisitos | PHP 8.5.0 bateragarria, hedapen guztiak [OK], `files` eta `config` karpetak idazgarri [OK] |
| Datu-basearen konexioa | SQL Zerbitzaria: `192.168.10.4` · Erabiltzailea: `wae_user` · Pasahitza: `Admin123` |
| DB aukeraketa | Erabili lehendik dagoen DBa: `glpidb` |

| Arazoa | Kausa | Konponbidea |
|---|---|---|
| *"Can't connect to MySQL server on '192.168.10.4'"* | Ubuntu zerbitzariko MariaDB-k `127.0.0.1` helbidean bakarrik entzuten zuen | `/etc/mysql/mariadb.conf.d/50-server.cnf` fitxategian `bind-address = 0.0.0.0` ezarri eta `sudo systemctl restart mariadb` exekutatu |

![GLPI Paso Requisitos](../../irudiak/WAE/glpi-instalazioa-requisitos.png)
*Irudia: GLPI 10 ingurunearen egiaztapen-urratsa. PHP 8.5 eta hedapen guztiak (mysqli, mbstring, gd, intl, curl) berdez ageri dira [OK]. ✅*

![GLPI Instalazioa Amaituta](../../irudiak/WAE/glpi-instalazioa-amaituta.png)
*Irudia: Instalazio-prozesuaren amaiera. Datu-basearen hasieratzea osatuta eta hasierako kredentzialak erakusten dira. ✅*

---

## Erabiltzaileak eta profilak ✅

GLPI 10-ek default bidez lau erabiltzaile sortzen ditu profil desberdinekin. Segurtasun arrazoiengatik eta pribilegio txikienaren printzipioa jarraituz, konfigurazioa doitu da:

| Erabiltzailea | Hasierako Pasahitza | Rol/Profila | Erabilera BioHealth-en |
|---|---|---|---|
| `glpi` | `glpi` (Aldatuta) | **Super-Admin** | IT Saila: Sistemaren konfigurazioa, hedapenak, inbentario orokorra |
| `tech` | `tech` | **Technicien** | Zerbitzu teknikoko langileak: Intzidentziak ebatzi eta esleitu |
| `normal` | `normal` | **Normal** | Sail arteko kudeatzaileak: Consultas e inventario básico |
| `post-only` | `post-only` | **Post-only** | BioHealth-eko gainerako langileak: Intzidentzia berriak sortu eta beren egoera ikusi |

![GLPI Erabiltzaileak](../../irudiak/WAE/glpi-erabiltzaileak.png)
*Irudia: GLPI 10eko erabiltzaileen kudeaketa atala (`Administración → Usuarios`). Hasierako kontuak kargatuta eta pasahitz berriekin babestuta. ✅*

---

## Hasierako Konfigurazioa eta Hardening ✅

Instalazioa amaitutakoan, GLPI 10-ek segurtasun-abisuekin ohartarazten du sistemaren babesa bermatzeko:

1. **`install/install.php` fitxategia ezabatzea:**
   ```powershell
   Remove-Item -Path "C:\inetpub\wwwroot\glpi\install\install.php" -Force
   ```
2. **Hasierako pasahitzak aldatzea:** `glpi`, `tech`, `normal` eta `post-only` erabiltzaileen hasierako pasahitz lehenetsiak aldatu dira pasahitz sendoengatik.
3. **Grommunia / Telemetria desgaitzea:** *Configuración → General* atalean datu estatistikoen bidalketa automatikoa desgaitu da, barneko laborategiko datuak babesteko.

![GLPI Panel Nagusia](../../irudiak/WAE/glpi-panel-nagusia.png)
*Irudia: GLPI 10-eko panel nagusia (`Super-Admin` profilarekin). Segurtasun abisuak konponduta eta inbentario zein intzidentzien laburpena ikusgai. ✅*

---

## Proba Funtzionalak ✅

Intzidentzien bizi-zikloa egiaztatzeko proba egin da:

1. **`post-only` saioa:** Erabiltzaileak intzidentzia berria sortzen du ("Inprimagailuak ez du paperik hartzen harrera sailean").
2. **`glpi` / `tech` saioa:** Teknikariak intzidentzia jasotzen du, bere gain hartzen du (Asignado), egoera aldaketa egiten du eta konponbidea idazten du.
3. **Cierre:** Intzidentzia itxita geratzen da eta estatistiketan erregistratzen da.

![GLPI Ticket Proba](../../irudiak/WAE/glpi-ticket-proba.png)
*Irudia: Sortutako intzidentziaren jarraipen-orria (`Soporte → Tickets`). Egoera: "Resuelto" eta teknikariaren oharrekin. ✅*