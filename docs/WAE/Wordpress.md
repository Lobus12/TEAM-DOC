---
title: WordPress
---

[← Hasiera](../../index.md) · [Modulua](index.md)

# WordPress – Web Korporatiboa (IIS + MariaDB Remoto) ✅

## Aukerak eta erabakia

| CMS / Arkitektura | Lizentzia / Kostua | Alde onak | Alde txarrak |
|---|---|---|---|
| **WordPress + Windows Server (IIS 10) + MariaDB Remoto (Ubuntu)** | GPLv3 · 0 € (Dagoen azpiegitura) | Munduko CMS erabiliena (webguneen % 40+); Active Directory ingurunearekin bateragarritasuna; datu-basea isolated LAN seguruan (`192.168.10.4`); rolen kudeaketa erraza | FastCGI bidez PHP konfigurazio zehatza eskatzen du IIS-en; plugin askorekin mantentze eza segurtasun-arrisku bihur daiteke |
| Joomla + Apache (LAMP) | GPL · 0 € | Malgua, rol aurreratuak | Komunitate txikiagoa, ikasteko zailagoa |
| Drupal | GPL · 0 € | Oso segurua eta indartsua | Zaila; web txiki edo korporatibo baterako gehiegizkoa |
| Wix / Squarespace (SaaS) | Ordainpekoa (~15–30 €/hilabete) | Instalaziorik gabe | Ez da lokaleko IIS/MariaDB-n instalatzen; datuak hirugarren baten hodeian geratzen dira |

**Erabakia: WordPress IIS 10-en (Windows Server) + MariaDB Remoto (Ubuntu Server).** BioHealth-en webgunea korporatiboa da (zerbitzuak, ordutegiak, kontaktua, telekontsultarako esteka). Langile ez-teknikoek eguneratu behar dute (Editor rola) eta 0 €-ko kostua du. Segurtasuna: IIS web zerbitzaria DMZ/LAN eremuan kokatuta dago (`192.168.10.1`), datu-basea LAN isolatuan (`192.168.10.4`), eta komunikazioa HTTPS bidez zifratuta.

## Webaren erabilera ↔ CMSaren funtzionalitatea

| BioHealth-en beharra | WordPress-en funtzionalitatea |
|---|---|
| Kontsulta-zerbitzuak eta espezialitateak erakutsi | Orriak (Pages) |
| Albisteak / osasun-aholkuak argitaratu | Sarrerak (Posts) eta Kategoriak |
| Harrerako langileek edukia eguneratu | **Editor** rola (argitaratu bai, konfiguratu ez) |
| Telekontsulta eta hitzorduak | Jitsi Meet / kanpoko zerbitzuetarako esteka (`meet.biohealth.local`) |
| Irudi korporatiboa eta irisgarritasuna | Astra Txantiloia + CSS pertsonalizatua |

## Instalazio-parametroak

| Parametroa | Balioa |
|---|---|
| Web Zerbitzaria | Windows Server (IIS 10.0) · `192.168.10.1` |
| Web Engine | FastCGI / PHP 8.5.0 (`C:\php\php-cgi.exe`) |
| WordPress Bertsioa | 6.5.x (Euskaraz / Gaztelaniaz) |
| DocumentRoot | `C:\inetpub\wwwroot\wordpress` |
| IIS AppPool | `DefaultAppPool` (No Managed Code, ApplicationPoolIdentity) |
| NTFS Baimenak | `IUSR` (Irakurri/Exekutatu), `IIS_IUSRS` (Irakurri/Idatzi `wp-content` karpetan) |
| Datu-base Zerbitzaria | MariaDB @ Ubuntu Server · `192.168.10.4:3306` |
| Datu-basearen izena | `waedb` |
| DB Erabiltzailea | `wae_user` @ `192.168.10.1` (Pasahitza: `Admin123`) |
| Logak (IIS) | `C:\inetpub\logs\LogFiles\W3SVC1\` |
| HTTPS / SSL | ✅ 443 TCP · IIS Binding (`www.biohealth.local`) · OpenSSL / Self-Signed |

SQL konfigurazio-scripta: [`scripts/wp-db-setup.sql`](https://github.com/prietosuhar/biohealth-erronka1/blob/main/scripts/wp-db-setup.sql) — Ubuntu MariaDB zerbitzarian exekutatua (`mysql -u root -p < wp-db-setup.sql`).

![IIS WordPress App](../../irudiak/WAE/wp-iis-binding.png)
*Irudia: IIS Kudeatzailean WordPress direktorioa konfiguratuta, PHP FastCGI bidez mapeatuta eta HTTPS 443 portuan entzuten. ✅*

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
| Taulen aurrizkia | `wp_bh_` (Segurtasunagatik, lehenetsitako `wp_` aldatuta) |
| Gunearen Izenburua | BioHealth |
| Administratzaile Erabiltzailea | `bioadmin` (ez `admin`: erasoetan probatzen den lehen izena) · pasahitz sendoa |
| Bilatzaileak | «Disuadir a los motores de búsqueda» markatuta (barneko laborategia) |

| Arazoa | Kausa | Konponbidea |
|---|---|---|
| «Error al establecer una conexión con la base de datos» | Ubuntu zerbitzariko MariaDB-k `127.0.0.1` helbidean bakarrik entzuten zuen edo `bind-address` mugatuta zegoen | `/etc/mysql/mariadb.conf.d/50-server.cnf` fitxategian `bind-address = 0.0.0.0` ezarri eta MariaDB zerbitzua berriro kargatu (`sudo systemctl restart mariadb`) |

![WordPress Escritorio](../../irudiak/WAE/wp-escritorio.png)
*Irudia: `bioadmin` erabiltzailearekin administrazio-panelean (Escritorio) sartuta, `192.168.10.1/wordpress/wp-admin` helbidean. ✅*

---

## Erabiltzaileak eta pribilegioak ✅

Pribilegio txikienaren printzipioa: administratzailea bakarra da (Informatika saila), eta harrerako langileek edukia bakarrik kudeatzen dute.

| Erabiltzailea | Rola | Nork erabiltzen du | Zer egin dezake | Zer EZ |
|---|---|---|---|---|
| `bioadmin` | **Administrator** | Informatika-saila | Dena: pluginak, txantiloiak, ezarpenak, erabiltzaileak | — |
| `harrera1` | **Editor** | Harrerako langileak | Sarrerak eta orriak sortu, editatu eta argitaratu; multimedia kudeatu | Pluginak, txantiloiak, ezarpenak edo erabiltzaileak kudeatu |

Sortzeko: *Usuarios → Añadir nuevo usuario* → izena `harrera1`, posta `harrera1@biohealth.local`, *Perfil: Editor*.

![WordPress Erabiltzaileak](../../irudiak/WAE/wp-erabiltzaileak.png)
*Irudia: Bi erabiltzaileak kargatuta rol desberdinekin: `bioadmin` (Administrador) eta `harrera1` (Editor). ✅*

### Probak

![Editor Menua](../../irudiak/WAE/wp-editor-menua.png)
*Irudia: `harrera1`-ekin sartuta, menua mugatuta dago: Entradas, Medios, Páginas, Comentarios, Perfil eta Herramientas bakarrik. Ez dira agertzen **Apariencia, Plugins, Usuarios eta Ajustes**. ✅*

![Plugins Ukatua](../../irudiak/WAE/wp-editor-plugins-ukatua.png)
*Irudia: Ukatze-proba: `harrera1`-ek `/wp-admin/plugins.php` helbidea zuzenean idatzita → «Lo siento, no tienes permisos para acceder a esta página». ❌ (esperotako portaera)*

---

## Txantiloiak ✅

Hiru txantiloi aztertu dira BioHealth-en webgune korporatiborako:

| Ezaugarria | Twenty Twenty-Five | Astra | Neve |
|---|---|---|---|
| Mota | Bloke-txantiloia (FSE) | Klasikoa + Personalizadorea | Klasikoa + Personalizadorea |
| Egilea | WordPress (ofiziala) | Brainstorm Force | Themeisle |
| Pisua / Abiadura | Oso arina | Oso arina (< 50 KB) | Oso arina |
| Mugikorra (Responsive) | Bai | Bai | Bai |
| Pertsonalizazioa | Bloke-editorea | Personalizadorea: koloreak, tipografia, **CSS adicional** | Personalizadorea, aukera gutxiago |
| **Erabakia** | ❌ Baztertuta (zailagoa) | **✅ AUKERATUA** | ❌ Baztertuta |

**Erabakia: Astra.**
- **Errendimendua:** Txantiloi arinenetakoa, errendimendu bikainarekin IIS zerbitzarian.

- **Erabilgarritasuna:** Personalizadorea erraza da; harrerako langileek (Editor) ez dute gune-editorerik ukitu behar.

- **CSS:** *CSS adicional* atala zuzenean dakar, BioHealth-en kolore korporatiboak aplikatzeko.


![Astra Aktiboa](../../irudiak/WAE/wp-astra-aktiboa.png)
*Irudia: «Nuevo tema activado» → **Activo: Astra**. ✅*

---

## Diseinua eta CSS aldaketak ✅

**Helburua:** Webgunea BioHealth-en irudi korporatibora egokitzea. Osasun-sektorean ohikoak diren kolore **berde-urdinxkak** (`#0f766e`) aukeratu dira.

**Non:** *Apariencia → Personalizar → CSS adicional*. CSSa datu-basean gordetzen da (`wp_posts`, `custom_css` mota), eta ez da txantiloiaren fitxategietan idazten (Astra eguneratzean ez da galtzen).

Kodea: [`scripts/biohealth.css`](https://github.com/prietosuhar/biohealth-erronka1/blob/main/scripts/biohealth.css)

```css
:root {
    --bh-nagusia: #0f766e;
    --bh-argia: #ccfbf1;
    --bh-testua: #1e293b;
}

/* Goiburua eta Menua */
.site-header, .main-header-bar {
    background-color: var(--bh-nagusia) !important;
}

.site-title a, .main-header-menu a {
    color: #ffffff !important;
}

/* Izenburuak eta Botoiak */
h1, h2, h3, .entry-title a {
    color: var(--bh-nagusia);
}

.button, input[type="submit"] {
    background-color: var(--bh-nagusia) !important;
    border-radius: 6px;
    color: #ffffff !important;
}

/* Responsiboa izenburu nagusian */
.entry-content h1 {
    font-size: clamp(2rem, 4vw, 3.2rem);
}