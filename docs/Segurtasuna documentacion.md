---
layout: page
title: Segurtasuna
---

# INFORMAZIOAREN SEGURTASUN DOKUMENTAZIOA

## 1. Sarrera eta Helburua
Dokumentu honen helburua erakundearen azpiegitura teknologikoa, datu korporatiboak eta zerbitzuak babesteko jarraitu beharreko segurtasun-neurriak eta gidalerroak zehaztea da. Helburu nagusia informazioaren **konfidentzialtasuna, osotasuna eta erabilgarritasuna** bermatzea da.

## 2. Sarearen Segurtasuna eta Azpiegitura
Sare korporatiboa mehatxuetatik babesteko, honako neurri hauek ezartzen dira:

* **Sarearen Segmentazioa**: Sarea eremu isolatuetan banatuko da bideratze eta suebaki bidez (adibidez: WAN, LAN, DMZ eta Haririk gabeko sarea).
* **Suebakia (Firewall)**: pfSense (edo antzeko sistema bat) erabiliko da sarrerako eta irteerako trafikoa kontrolatzeko. Besterik adierazi ezean, trafiko guztia blokeatuko da (*Deny All* printzipioa).
* **Urruneko Sarbidea**: Sare korporatibora kanpotik sartzeko, ezinbestekoa izango da VPN konexio seguru bat erabiltzea (adib.: OpenVPN edo IPsec), pasahitz eta ziurtagiri bidez babestuta. Ez da administrazio-porturik (SSH, RDP, WebGUI) zuzenean internetera irekiko.
* **Komunikazio Seguruak**: Web zerbitzu guztiek HTTPS erabiliko dute, indarrean dauden TLS ziurtagiriekin.

### 2.1. Urruneko Sarbidea: OpenVPN Ezarpena pfSense-n (RA3) ✅

Kanpoko langileek enpresako barne-sarera modu seguruan sartzeko OpenVPN zerbitzaria konfiguratu da pfSense suebakian (`192.168.120.142:1194` UDP):

1. **CA eta Erabiltzaile-ziurtagiriak**: Ziurtagiri-agintaritza lokal bat sortu da eta erabiltzaile bakoitzarentzat gako-bikote esklusiboa sortu da (`langile1` / `Langile1_Cert`).
2. **Bezeroaren konfigurazio-esportazioa**: *openvpn-client-export* paketearen bidez konfigurazio bateratuak (.ovpn) deskargatzen dira.
3. **Konexioaren egiaztapena**: Bezeroak OpenVPN Connect bidez autentifikatu eta tunel seguruaren IP bat eskuratzen du (`10.0.8.x` eremuan).

<img width="1204" height="589" alt="Captura de pantalla 2026-10-14 140210" src="https://github.com/user-attachments/assets/286b39ed-bcdd-431f-9c59-53dfb73b8aa7" />


<img width="488" height="833" alt="Captura de pantalla 2026-10-14 142219" src="https://github.com/user-attachments/assets/2e20d0b9-d427-460e-96cd-b34f8f073102" />

<img width="1199" height="628" alt="Captura de pantalla 2026-10-14 185849" src="https://github.com/user-attachments/assets/010ddcc3-813f-46c5-8416-42b13e80a2f3" />


## 3. Sarbide Kontrola eta Pasahitzen Politika
Erabiltzaileen eta administratzaileen sarbidea kontrolatzeko arauak:

* **Gutxieneko Pribilegioaren Printzipioa**: Erabiltzaile bakoitzak bere lana egiteko behar dituen baimen zehatzak soilik izango ditu.
* **Pasahitzen Konplexutasuna**: 
  * Gutxienez 12 karaktere.
  * Letra larriak, xeheak, zenbakiak eta karaktere bereziak izan behar dituzte.
  * Gehienez 90 egunetik behin aldatu beharko dira.
* **Kontuen Blokeoa**: Jarraian 5 saio-hasiera saiakera huts egin ondoren, kontua automatikoki blokeatuko da 30 minutuz.
* **Faktore Anitzeko Autentifikazioa (MFA)**: Zerbitzu kritikoetarako eta VPN konexioetarako ezinbestekoa izango da bigarren autentifikazio-faktore bat erabiltzea.

## 4. Babeskopiak eta Datuen Berreskuratzea (Backups)
Datuen galera saihesteko eta negozioaren jarraitutasuna bermatzeko:

* **Babeskopien Politika**: 3-2-1 araua aplikatuko da (Datuen 3 kopia, 2 euskarri ezberdinetan, eta kopia 1 erakundetik kanpo edo hodeian).
* **Maiztasuna**: Zerbitzari kritikoen (datu-baseak, fitxategi-zerbitzariak) segurtasun-kopia automatikoak egingo dira egunero.
* **Zifratzea**: Erakundetik kanpo gordetzen diren babeskopia guztiak zifratuta egongo dira.
* **Berreskuratze Metrikak (RTO eta RPO)**:
  * **RTO (Recovery Time Objective)**: Sistema kritikoak gehienez 4 orduko epean berreskuratu behar dira.
  * **RPO (Recovery Point Objective)**: Onar daitekeen datu-galera maximoa 24 ordukoa izango da.

## 5. Segurtasun Fisikoa eta Ingurumena
Ekipo teknologikoak gordetzen diren lekuak (Datuak Prozesatzeko Zentroa edo DPZ) baldintza hauek bete behar ditu:

* **Sarbide Fisikoa**: Aretorako sarbidea baimendutako langileentzat soilik egongo da mugatuta, sarbide-kontrol elektroniko bidez (txartela edo biometria).
* **Baldintza Klimatikoak**: Tenperatura 18°C eta 22°C artean mantenduko da, klimatizazio-sistema erredundanteen bidez.
* **Energia-hornidura**: Etenik gabeko Elikatze Sistema (SAI/UPS) bat egongo da, argindarra joanez gero zerbitzariak modu seguruan itzaltzeko (*Graceful Shutdown*) denbora emateko.
* **Suteen Aurkako Babesa**: Ke-detektagailuak eta itzaltze-sistema automatikoa (adibidez, FM-200 gas inertea) egongo dira, ekipo elektronikoak ez kaltetzeko.

## 6. Gorabeheren Kudeaketa (Incident Response)
Segurtasun-urraketa edo zibereraso bat gertatuz gero:

1. **Detekzioa eta Isolamendua**: Kaltetutako ekipoa saretik deskonektatuko da berehala, erasoa zabal ez dadin.
2. **Azterketa**: Segurtasun-arduradunak gorabeheraren jatorria eta irismena ikertuko ditu.
3. **Berreskuratzea**: Kaltetutako sistemak garbitu eta azken babeskopia segurutik leheneratuko dira.
4. **Txostena**: Gertatutakoaren eta hartutako neurrien txosten bat idatziko da, etorkizunean arazo bera ez errepikatzeko.

## 7. Kontingentzia Plana
Erakundearen jardueraren jarraitutasuna bermatzeko eta ezustekoen aurrean erantzun egokia emateko, honako Kontingentzia Plana garatu da.

7.1. Enpresaren deskribapena
ServersToYou azpiegitura teknologikoak, zerbitzarien kudeaketa eta sistemen mantentze-lanak eskaintzen dituen enpresa bat da. Bere jarduera nagusia bezeroen zein enpresaren datu korporatiboen tratamenduan eta kudeaketan oinarritzen da. Hori dela eta, ezinbestekoa da informazioaren konfidentzialtasuna, osotasuna eta erabilgarritasuna bermatzea uneoro, zerbitzuaren eten batek edo datu-filtrazio batek ondorio legal eta ekonomiko larriak ekar ditzakeelako.

7.2. Aktiboen azterketa (INCIBE - PILAR tresna)
Aktiboen azterketa sakona egin da INCIBEren PILAR (Magerit metodologia) tresna erabiliz. Identifikatutako aktibo kritiko nagusiak honako hauek dira:
- Informazio-aktiboak: Bezeroen eta enpresaren datu-baseak (MariaDB), kudeaketa-fitxategiak, langileen datuak.
- Hardware-aktiboak: Web zerbitzariak (IIS), datu-base zerbitzaria (Ubuntu), suebakia (pfSense), eta biltegiratze-sistemak (NAS/SAN).
- Software-aktiboak: GLPI (intzidentzien kudeaketa), WordPress (web korporatiboa).
PILAR tresnaren bidez aktibo hauen balioespena egin da (Dimentsioak: Konfidentzialtasuna, Osotasuna, Trazabilitatea, Autentikotasuna eta Erabilgarritasuna), eta horien araberako babes-neurriak diseinatu dira.

7.3. Arrisku-kasuak (Escenarios de Riesgo)
Identifikatutako aktiboetan oinarrituta, hiru arrisku-kasu nagusi aurreikusi dira eta bakoitzarentzako erantzun-plana zehaztu da:

A. Arrisku-kasua: Ransomware erasoa datu-basean
- Mehatxua: Ziberkriminalek MariaDB datu-basea zifratzea eta erreskatea eskatzea.
- Erantzuna: Sistema berehala saretik isolatu (deskonektatu). Ez da inoiz erreskaterik ordainduko. 4. atalean aipatutako azken babeskopia segurua (off-site edo hodeikoa) erabiliko da sistemak berrezartzeko. RTO-a 4 ordukoa izango da.

B. Arrisku-kasua: Sutea edo elektrizitate-etena Datuak Prozesatzeko Zentroan (DPZ)
- Mehatxua: Suteen edo argindar faltaren ondorioz zerbitzari fisikoak itzaltzea edo kaltetzea.
- Erantzuna: Etenik gabeko Elikatze Sistemak (SAI) zerbitzariak modu seguruan itzaliko ditu datuak ez usteltzeko. Sutea bada, FM-200 gas-sistemak itzaliko du. Kalte fisiko handia badago, hardware berria eskuratu eta hodeian dauden babeskopietatik birtualizazio bidez (Disaster Recovery) altxatuko dira zerbitzu kritikoak.

C. Arrisku-kasua: Giza akatsa edo barne-mehatxua (Datu-filtrazioa)
- Mehatxua: Langile batek, nahi gabe edo nahita, baimenik gabe informazio sentikorra ezabatzea edo filtratzea.
- Erantzuna: GLPI eta sareko log-en bidez trazabilitatea aztertuko da nor izan den jakiteko. Erabiltzailearen kontua automatikoki blokeatuko da. Galdutako datuak azken RPO (24 ordu) babeskopiatik berreskuratuko dira.

7.4. Dokumentazioaren eta neurrien justifikazioa
Hartutako neurri guztiak (babeskopiak, suebakiak, rolak) PILAR tresnak emandako arrisku-mai
