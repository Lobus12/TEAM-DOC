# IRAU: Jasangarritasuna eta Ingurumen/Gizarte Erronkak

---

## 1. IRAU: IE1 - Jasangarritasunaren Kontzeptua eta ESG Esparrua

### 1.1 Jasangarritasunaren Kontzeptua eta Nazioarteko Esparruak
**Garapen Jasangarria** egungo belaunaldien beharrak asetzeko gaitasuna da, etorkizuneko belaunaldien beharrak asetzeko gaitasuna arriskuan jarri gabe. Proiektu teknologiko honetan, garapen jasangarria Nazio Batuen **2030 Agendako Garapen Jasangarriaren Helburuetan (GJH / ODS)** eta **Parisko Klima Akordioan** oinarritzen da.

IT azpiegituren kudeaketan (zerbitzariak, sareak eta datu-baseak), digitalizazioak baliabideen erabilera eraginkorra sustatu behar du, ingurumen-inpaktua gutxituz eta gizarte-balioa sortuz.

---

### 1.2 EGG / ESG (Ingurumena, Gizartea eta Gobernantza) Alderdiak Enpresan

| ESG Zutabea | Aztertutako Gaia | Enpresako IT Azpiegituran Aplikazioa |
| :--- | :--- | :--- |
| **Ingurumena (Environmental)** | Energetiko-eraginkortasuna eta hondakinen kudeaketa | - Zerbitzarien birtualizazioa (pfSense, Ubuntu eta Windows Server VM-etan) hardware fisikoaren kontsumo elektrikoa gutxitzeko.<br>- Jitsi Meet bidezko bideokonferentziak langileen desplazamenduak eta CO2 isurketak murrizteko.<br>- Inprimatze-politika murriztailea CUPS zerbitzariaren bidez (Paperless kultura). |
| **Gizartea (Social)** | Pertsonak, irisgarritasuna eta lan-osasuna | - Bideokonferentzia eta VPN bidezko telelana erraztea, lan-bizitza eta familia bateragarri eginez.<br>- Webgunearen (WordPress) irisgarritasun estandarrak betetzea mugikortasun edo ikusmen murriztua duten erabiltzaileentzat.<br>- Lanpostu informatikoen ergonomia eta segurtasuna bermatzea. |
| **Gobernantza (Governance)** | Etika, datuen babesa eta segurtasuna | - Datuen Babeserako Araudi Orokorra (DBEO / RGPD) betetzea MariaDB datu-basean gordetako datu pertsonaletan.<br>- Zibersegurtasun eta pasahitz politikak ezartzea (TLS/HTTPS, pfSense Firewall, IDS Suricata).<br>- Kode irekiko softwarea erabiltzea (Ubuntu, MariaDB, CUPS, Jitsi) gardentasuna eta lizentzien kudeaketa etikoa bermatuz. |

---

## 2. IRAU: IE2 - Ingurumen- eta Gizarte-Erronkak eta Inpaktuaren Azterketa

### 2.1 Ingurumen- eta Gizarte-Erronka Nagusiak
Gizarteak eta ekoizpen-sektoreak gaur egun dituzten erronka nagusiak honako hauek dira:
1. **Klima-aldaketa eta Kontsumo Energetikoa:** Datu-zentroen eta zerbitzarien kontsumo elektriko altua.
2. **Hondakin Elektriko eta Elektronikoak (RAEE/WEEE):** Ekipo informatikoen zaharkitze azkarra eta birziklapen falta.
3. **Arrakala Digitala eta Zibersegurtasuna:** Teknologiarako sarbide berdinbidea bermatzeko beharra eta informazio-galeran aurreko babesa.

---

### 2.2 IT Jarduera Ekonomikoaren eta Erronken Arteko Lotura
Gure enpresako sare eta sistema azpiegiturak zuzenean erantzuten die erronka hauei:

* **Green IT eta Birtualizazioa:** 4 zerbitzari fisiko independente martxan jarri ordez, hiperbisore bakarrean birtualizatu dira (Windows Server, MariaDB, Jitsi eta CUPS). Honek %60 arte gutxitzen du energia elektrikoaren kontsumoa eta CO2 igorpena.
* **Garraio Isurketen Murrizketa:** Jitsi Meet zerbitzariaren ezarpenari esker, enpresako bilerak eta bezeroekiko harremanak aurrez aurrekoak izan beharrean telematikoak izatea lortzen da, ibilgailuen erabilera eta klima-aldaketaren inpaktua murriztuz.
* **Paper Kontsumoaren Gutxitzea:** CUPS inprimatze-zerbitzariaren bidez, erabiltzaile bakoitzaren inprimatze-baimenak eta kuotak kontrolatzen dira, alferrikako paper eta tonner kontsumoa prebenituz.

---

### 2.3 Inpaktua Pertsonengan eta Ekoizpen Sektorean

| Eremua | Eragina Pertsonengan | Eragina Ekoizpen Sektorean / Enpresan |
| :--- | :--- | :--- |
| **Ingurumen Inpaktua** | - Ingurune garbiagoa, isurketa kutsatzaile gutxiagorekin.<br>- Birziklapen eta kontsumo arduratsuaren kontzientziazioa lanpostuan. | - Karbono-aztarnaren (Carbon Footprint) gutxitzea enpresaren jardueran.<br>- Ekipoen bizi-zikloa luzatzea eta RAEE hondakinen kudeaketa egokia. |
| **Gizarte Inpaktua** | - Fleksibilitatea lanorduetan telelanari esker (VPN + Jitsi).<br>- Datu pertsonalen babesa eta pribatutasun bermea webgunean (HTTPS/TLS). | - Enpresaren lehiakortasuna hobetzea eta lan-gaitasuna handitzea lan-etenaldien aurrean.<br>- Digitalizazio inklusiboa eta segurua lortzea. |
| **Ekonomia eta Gobernantza** | - Erabiltzaileen konfiantza segurtasun-gertakarien aurrean (IDS/Firewall). | - Azpiegitura-kostuen murrizketa (kode irekiko softwarea eta birtualizazioa erabiliz).<br>- Incident Risk / Negozio jarraitutasuna bermatzea kontingentzia-plan bidez. |
