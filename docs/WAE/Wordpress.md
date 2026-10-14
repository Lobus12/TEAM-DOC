---
layout: page
title: Wordpress
---


# WordPress – Web Korporatiboa (IIS + MariaDB Remoto) ✅

## Aukerak eta erabakia

| CMS / Arkitektura | Lizentzia / Kostua | Alde onak | Alde txarrak |
|---|---|---|---|
| **WordPress + Windows Server (IIS 10) + MariaDB Remoto (Ubuntu)** | GPLv3 · 0 € (Dagoen azpiegitura) | Munduko CMS erabiliena (webguneen % 40+); Active Directory ingurunearekin bateragarritasuna; datu-basea isolated LAN seguruan (`192.168.10.4`); rolen kudeaketa erraza | FastCGI bidez PHP konfigurazio zehatza eskatzen du IIS-en; plugin askorekin mantentze eza segurtasun-arrisku bihur daiteke |
| Joomla + Apache (LAMP) | GPL · 0 € | Malgua, rol aurreratuak | Komunitate txikiagoa, ikasteko zailagoa |
| Drupal | GPL · 0 € | Oso segurua eta indartsua | Zaila; web txiki edo korporatibo baterako gehiegizkoa |
| Wix / Squarespace (SaaS) | Ordainpekoa (~15–30 €/hilabete) | Instalaziorik gabe | Ez da lokaleko IIS/MariaDB-n instalatzen; datuak hirugarren baten hodeian geratzen dira |

**Erabakia: WordPress IIS 10-en (Windows Server) + MariaDB Remoto (Ubuntu Server).** ServerToYou-en (STY) webgune korporatiboa da (zerbitzuak, hosting-planak, FAQ eta harremanetarako estekak). Langile ez-teknikoek zein kudeatzaileek eguneratu dezakete (Editor rola) eta 0 €-ko kostua du. Segurtasuna: IIS web zerbitzaria DMZ/LAN eremuan kokatuta dago (`192.168.10.1`), datu-basea LAN isolatuan (`192.168.10.4`), eta komunikazioa HTTPS bidez zifratuta.

## Webaren erabilera ↔ CMSaren funtzionalitatea

| ServerToYou-en beharra | WordPress-en funtzionalitatea |
|---|---|
| Zerbitzuak eta azpiegitura-soluzioak erakutsi | Landing Page egituratua / Orriak (Pages) |
| Hosting-planak eta tarifak aurkeztu | Bloke-editore (Gutenberg) bidezko taula eta txartelak |
| Berriak / azpiegitura-aholkuak argitaratu | Sarrerak (Posts) eta Kategoriak |
| Harrerako edo eduki-kudeaketako langileek edukiak eguneratu | **Editor** rola (argitaratu bai, konfiguratu ez) |
| Bezeroentzako arreta eta aurrekontu-eskaerak | Harremanetarako botoiak eta mailto/tel estekak (`hola@servertoyou.com`) |
| Irudi korporatiboa eta irisgarritasuna | Astra Txantiloia + Gutenberg Bloke-sistema eta estilodun interfazea |

## Instalazio-parametroak

| Parametroa | Balioa |
|---|---|
| Web Zerbitzaria | Windows Server (IIS 10.0) · `192.168.10.1` |
| Web Engine | FastCGI / PHP 8.5.0 (`C:\php\php-cgi.exe`) |
| WordPress Bertsioa | 6.5.x / 7.x (Euskaraz / Gaztelaniaz) |
| DocumentRoot | `C:\inetpub\wwwroot\wordpress` |
| IIS AppPool | `DefaultAppPool` (No Managed Code, ApplicationPoolIdentity) |
| NTFS Baimenak | `IUSR` (Irakurri/Exekutatu), `IIS_IUSRS` (Irakurri/Idatzi `wp-content` karpetan) |
| Datu-base Zerbitzaria | MariaDB @ Ubuntu Server · `192.168.10.4:3306` |
| Datu-basearen izena | `waedb` |
| DB Erabiltzailea | `wae_user` @ `192.168.10.1` (Pasahitza: `Admin123`) |
| Logak (IIS) | `C:\inetpub\logs\LogFiles\W3SVC1\` |
| HTTPS / SSL | ✅ 443 TCP · IIS Binding (`www.sty.eus`) · OpenSSL / Self-Signed |

SQL konfigurazio-scripta: [`scripts/wp-db-setup.sql`](https://github.com/Lobus12/TEAM-DOC/blob/main/docs/Documentacion%20DataB.md) — Ubuntu MariaDB zerbitzarian exekutatua (`mysql -u root -p < wp-db-setup.sql`).

<img width="1082" height="631" alt="Captura de pantalla 2026-10-14 a las 19 42 14" src="https://github.com/user-attachments/assets/90964569-05e3-49d1-8e3e-f8a614c86435" />


### Arazoak

| Arazoa | Kausa | Konponbidea |
|---|---|---|
| HTTP Error 500.0 - Internal Server Error (`php-cgi.exe`) | `php.ini` fitxategian `extension_dir` ez zegoen gaituta edo `php_mysqli.dll` falta zen | `C:\php\php.ini` editatu: `extension_dir = "ext"` eta `extension=mysqli` deskomentatu |
| Permiso akatsa irudiak igotzean (`wp-content/uploads`) | IIS-eko `IIS_IUSRS` taldeak ez zuen idazteko baimenik NTFS mailan | `C:\inetpub\wwwroot\wordpress\wp-content` karpetan `IIS_IUSRS` taldeari Modifikatzeko (Modify) baimena eman |

---

## Web-instalatzailea ✅

Windows Server edo LANeko edozein ekiporen Edge nabigatzailetik `http://192.168.10.1/wordpress/` helbidera sartuta:

| Urratsa | Balioa |
|---|---|
| Hizkuntza | Español / Euskara |
| Datu-basearen zerbitzaria | `192.168.10.4` |
| Datu-basearen izena | `waedb` |
| Erabiltzailea / Pasahitza | `wae_user` / `Admin123` |
| Taulen aurrizkia | `wp_sty_` (Segurtasunagatik, lehenetsitako `wp_` aldatuta) |
| Gunearen Izenburua | ServerToYou |
| Administratzaile Erabiltzailea | `bioadmin` / `admin` (pasahitz sendoarekin babestua) |
| Bilatzaileak | «Disuadir a los motores de búsqueda» markatuta (barneko laborategia) |

| Arazoa | Kausa | Konponbidea |
|---|---|---|
| «Error al establecer una conexión con la base de datos» | Ubuntu zerbitzariko MariaDB-k `127.0.0.1` helbidean bakarrik entzuten zuen edo `bind-address` mugatuta zegoen | `/etc/mysql/mariadb.conf.d/50-server.cnf` fitxategian `bind-address = 0.0.0.0` ezarri eta MariaDB zerbitzua berriro kargatu (`sudo systemctl restart mariadb`) |

<img width="1744" height="1290" alt="Captura de pantalla 2026-10-14 a las 19 44 58" src="https://github.com/user-attachments/assets/1b1da728-2396-4470-be02-4948b65928f8" />

---

## Erabiltzaileak eta pribilegioak ✅

Pribilegio txikienaren printzipioa: administratzailea bakarra da (Informatika saila / Sistemak), eta harrerako zein eduki kudeatzaileek webgunea bakarrik eguneratzen dute.

| Erabiltzailea | Rola | Nork erabiltzen du | Zer egin dezake | Zer EZ |
|---|---|---|---|---|
| **Administrator** | Informatika-saila | Dena: pluginak, txantiloiak, ezarpenak, erabiltzaileak | — |
| **Editor** | Harrerako langileak | Sarrerak eta orriak sortu, editatu eta argitaratu; multimedia kudeatu | Pluginak, txantiloiak, ezarpenak edo erabiltzaileak kudeatu |

Sortzeko: *Usuarios → Añadir nuevo usuario* → izena `harrera1`, posta `harrera1@sty.local`, *Perfil: Editor*.

---

## Txantiloiak ✅

Hiru txantiloi aztertu dira ServerToYou-en webgune korporatiborako:

| Ezaugarria | Twenty Twenty-Five | Astra | Neve |
|---|---|---|---|
| Mota | Bloke-txantiloia (FSE) | Klasikoa + Personalizadorea / Blokeak | Klasikoa + Personalizadorea |
| Egilea | WordPress (ofiziala) | Brainstorm Force | Themeisle |
| Pisua / Abiadura | Oso arina | Oso arina (< 50 KB) | Oso arina |
| Mugikorra (Responsive) | Bai | Bai | Bai |
| Pertsonalizazioa | Bloke-editorea | Personalizadorea: koloreak, tipografia, **CSS adicional** | Personalizadorea, aukera gutxiago |
| **Erabakia** | ❌ Baztertuta | **✅ AUKERATUA** | ❌ Baztertuta |

**Erabakia: Astra.**
- **Errendimendua:** Txantiloi arinenetakoa, errendimendu bikainarekin IIS zerbitzarian.
- **Erabilgarritasuna:** Gutenberg bloke-editorearekin integratzeaz gain, Personalizadorea oso erraza da; kudeatzaileek (Editor) ez dute gune-egitura arriskuan jartzen.
- **CSS / Diseinua:** Gutenberg bloke pertsonalizatuekin erraz konbinatzen da, ServerToYou-en kolore korporatiboak eta azpiegitura-itxura profesionala aplikatzeko.

<img width="1733" height="1224" alt="Captura de pantalla 2026-10-14 a las 19 53 56" src="https://github.com/user-attachments/assets/70587de7-cb9f-480d-bae6-c5fa4594ae06" />

---

## Diseinua eta Web Gunearen Egitura ✅

**Helburua:** Webgunea ServerToYou (STY) IT azpiegitura eta zerbitzuen irudi korporatibora egokitzea. IT sektorean ohikoak diren kolore **urdin ilun eta teknologikoak** (`#0f1e33`, `#2b4eff`, `#eef2f6`) erabili dira.

Gutenberg bloke-editorearen bidez Landing Page modular eta profesional bat eraiki da (`http://192.168.10.1/wordpress/`):

### Webgunearen Atalak:
1. **Hero Header:** Aurkezpen nagusia eta ekintzarako botoiak (*Pedir presupuesto*, *Ver planes*).
2. **Zerbitzuak (`#servicios`):** Hosting-a, backup automazioa eta 24/7 monitorizazio-zerbitzuak.
3. **Hosting Planak (`#planes`):** Básico (9€/mes), Profesional (29€/mes) eta Dedicado (99€/mes) tarifak alderatzeko taulak.
4. **Prozesua (`#proceso`):** Bezeroaren eskaeratik zerbitzaria martxan jartzerainoko 4 urratsak.
5. **FAQ (`#faq`):** Migrazio, epe eta segurtasun-galdera nagusiak atal tolesgarrietan (`<details>`).
6. **Harremanetarako Atala (`#contacto`):** Direct-mail (`hola@servertoyou.com`) eta telefono-estekak.

<img width="1747" height="1152" alt="Captura de pantalla 2026-10-14 a las 19 56 07" src="https://github.com/user-attachments/assets/73d293f9-507d-463a-b581-f5364a5ef7f6" />
