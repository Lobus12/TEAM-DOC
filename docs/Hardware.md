---
layout: page
title: Hardware
---

# ServerToYou — Ekipoen txostena (HW/SW) eta Inbentarioa

2026-10-13

## 1. Sarrera eta Helburua

Dokumentu honek `servertoyou` enpresako langileen ordenagailuak eta azpiegitura teknikoa deskribatzen ditu: zer osagai dituzten, zer zehaztapen dituzten eta bakoitzak nola funtzionatzen duen ekipoaren barruan. 

Enpresak hiru departamentu nagusi ditu: **Informatika**, **Administrazioa** eta **Marketina**.

Helburua da enpresako edozein pertsonak erabilgarri dagoen ekipamendua ezagutzea, pieza bakoitzak zertarako balio duen ulertzea, eta inbentario zein mantentze-lanak planifikatzea[cite: 29]. Erabaki nagusia: **markako ekipo estandarizatuak** erostea izan da, Windows 11 Pro lizentzia eta bermea in situ barne izateko[cite: 30].

## 2. Departamentuak eta Ekipo Banaketa

| Departamentua  | Lana / Funtzio Nagusia                                      |
|----------------|-------------------------------------------------------------|
| **Informatika**| Zibersegurtasuna, birtualizazioa, sareen kudeaketa eta laguntza teknikoa |
| **Administrazioa** | Fakturazioa, kontabilitatea, nominak eta dokumentazio kudeaketa |
| **Marketina**  | Diseinu grafikoa, kanpainak, sare sozialen kudeaketa eta bideo edizioa |

## 3. Osagaiak eta Funtzionamendua

Ekipoen ulermena errazteko, hona hemen oinarrizko osagaien funtzioak[cite: 29, 30]:

| Osagaia | Funtzioa | Nola funtzionatzen duen |
|---|---|---|
| **Prozesadorea (PUZ)** | Ekipoaren burmuina | Programen jarraibideak exekutatzen ditu. Zenbat eta nukleo eta hari gehiago izan, orduan eta zeregin gehiago egin ditzake aldi berean[cite: 29]. |
| **RAM Memoria** | Memoria bolatila | Irekitako programen datuak gordetzen ditu, prozesadorea azkar sar dadin. Ekipoa itzaltzean hustu egiten da[cite: 29]. |
| **Biltegiratzea (SSD)** | Memoria iraunkorra | Sistema eragilea, programak eta fitxategiak gordetzen ditu. Zati mugikorrik ez duenez (NVMe PCIe bidez), oso azkarra da[cite: 29, 30]. |
| **Plaka Basea** | Osagaiak batzea | PUZ, RAM eta gainerako osagaiak konektatzen dituen zirkuitua da, energia eta datuak banatuz[cite: 29]. |
| **Elikatze-iturria** | Ekipoa elikatzea | Sareko korrontea (230 V) osagaiek behar duten tentsio baxu bihurtzen du[cite: 29]. |
| **Grafikoak (GPU)** | Irudia prozesatzea | Pantailan bistaratzen dena prozesatzen du[cite: 29]. Administrazioan integratuta doa, baina Marketinen txartel dedikatu bat behar da bideoetarako. |
| **Sare-txartela** | Komunikazioa | Ekipoa enpresaren sarera eta Internetera konektatzea (Ethernet / Wi-Fi bidez)[cite: 29]. |
| **TPM 2.0** | Segurtasuna | Gako kriptografikoak gordetzen dituen txipa; BitLockerrek diskoa zifratzeko erabiltzen du[cite: 30]. |

## 4. Lanpostuen Espezifikazioak Sailka

Hiru sailek behar ezberdinak dituzte. Hori dela eta, profil desberdinak zehaztu dira. Eredu gisa **Lenovo ThinkCentre** (Mini PC) eta **ThinkPad / HP Z** markako ekipoak aurreikusi dira estandarizaziorako[cite: 30].

### 4.1. Informatika – Perfil D

Erabilera nagusia: sareen eta zerbitzarien kudeaketa, birtualizazioa, babeskopiak eta erabiltzaileei laguntza[cite: 29].

| Osagaia | Espezifikazio Gomendatua | Zergatik |
|---|---|---|
| **Ekipo mota** | Eramangarri Indartsua (Adib. Lenovo ThinkPad T14 Gen 6) | Mugikortasuna zerbitzari gela (CPD) eta erabiltzaileen artean mugitzeko[cite: 30]. |
| **PUZ** | Intel Core Ultra 5 / i7 edo AMD Ryzen 7 PRO (8 nukleo edo gehiago)[cite: 29, 30] | Makina birtualak tokian exekutatzeko. |
| **RAM** | 32 GB DDR5[cite: 29, 30] | Birtualizaziorako ezinbestekoa. |
| **Biltegiratzea** | SSD NVMe 512 GB edo 1 TB[cite: 29, 30] | Irudiak eta ISO fitxategiak gordetzeko[cite: 30]. |
| **Pantaila** | Eramangarriaren pantaila + 27" QHD (2560x1440) mahaian[cite: 29, 30] | Pantaila eremu handia kontsola eta leiho anitz kudeatzeko[cite: 30]. |
| **Periferikoak** | Entzungailu mikrofonodunak, sareko tresnen kita[cite: 29, 30]. | Urruneko laguntzarako[cite: 30]. |

### 4.2. Administrazioa – Perfil E

Erabilera nagusia: fakturazioa, ERP/CRM erabilera, kalkulu-orriak eta dokumentazioa[cite: 29].

| Osagaia | Espezifikazio Gomendatua | Zergatik |
|---|---|---|
| **Ekipo mota** | Mini PC (Adib. Lenovo ThinkCentre M70q Gen 5)[cite: 30] | Monitorearen atzean doa (VESA), lekua aurreztuz eta isila izanik[cite: 30]. |
| **PUZ** | Intel Core i5 (Adib. i5-14400T)[cite: 30] | Bulegotikarako aproposa kontsumo txikiarekin[cite: 30]. |
| **RAM** | 16 GB DDR5[cite: 29, 30] | Nahikoa bulegotikako aplikazio anitzetarako. |
| **Biltegiratzea** | SSD NVMe 512 GB[cite: 29, 30] | Zifratzea BitLockerrekin hardware bidez[cite: 30]. |
| **Grafikoak** | Integratua (Intel UHD)[cite: 29, 30] | 2 pantaila konektatzeko gaitasuna gehigarririk gabe[cite: 30]. |
| **Pantailak** | Bi monitore 24" Full HD (Adib. Dell Pro P2426H)[cite: 29, 30] | Kalkulu-orriak eta dokumentuak ondoan ikusita alderatzeko[cite: 30]. |
| **Periferikoak** | Teklatu numerikoa, inprimagailu multifuntzioa (eskanerra)[cite: 29, 30]. | Dokumentuak digitalizatzeko. |

### 4.3. Marketina – Perfil Diseinua

Erabilera nagusia: Adobe Creative Cloud (Photoshop, Premiere, Illustrator), diseinu grafikoa, bideo edizioa eta sare sozialen kudeaketa.

| Osagaia | Espezifikazio Gomendatua | Zergatik |
|---|---|---|
| **Ekipo mota** | Workstation Dorrea (Adib. HP Z2 Tower edo baliokidea) | Osagai oso indartsuak eta hozte sistema ona behar direlako karga astunetarako. |
| **PUZ** | Intel Core i7 / i9 edo AMD Ryzen 9 | Renderizazio denborak murrizteko prozesatzeko ahalmen maximoa. |
| **RAM** | 32 GB edo 64 GB DDR5 | Bideo-edizioak eta diseinu astunek memoria handia eskatzen dute. |
| **Biltegiratzea** | 1 TB SSD NVMe (OS eta programentzat) + NAS konexio bizkorra | Proiektu astunak tokian bertan azkar kargatzeko. |
| **Grafikoak (GPU)** | Dedikatua: NVIDIA RTX 4060 / A2000 (VRAM 8GB+) | 3D renderizazioa eta bideoen esportazioa azkartzeko ezinbestekoa. |
| **Pantailak** | 27" edo 32" 4K (IPS, %100 sRGB/DCI-P3 kalibrazioarekin) | Koloreen fideltasun absolutua beharrezkoa da diseinuan. |

## 5. Software Komuna Ekipo Guztietan

Segurtasun eta bateragarritasun estandarrak mantentzeko, ekipo guztiek honakoak dituzte[cite: 29, 30]:

*   **Sistema Eragilea**: Windows 11 Pro (Domeinura batzeko eta GPOetarako beharrezkoa, Bitlocker aktibatuta)[cite: 29, 30].
*   **Bulegotika**: LibreOffice edo Microsoft 365[cite: 29, 30].
*   **Segurtasuna (Antivirus / EDR)**: Microsoft Defender (gestio zentralizatua)[cite: 29, 30].
*   **PDF Irakurgailua**[cite: 29].
*   *Marketineko ekipoetan bakarrik*: Adobe Creative Cloud.

## 6. Azpiegitura Nagusia (Zerbitzaria, Sarea eta Kopiak)

Enpresaren oinarria eusteko, honako azpiegitura ezarri da (eredua: BioHealth diseinua)[cite: 30]:

*   **Zerbitzaria (Proxmox VE)**: Dell PowerEdge markako dorre bat. Bertan exekutatuko dira Windows Server (Active Directory, DNS, DHCP) eta barne zerbitzuak kudeatzeko Linux makina birtualak[cite: 30].
*   **Sarea**: TP-Link Omada (edo baliokidea) Switch kudeagarria PoE+ teknologiarekin, VLANak (LAN eta Gonbidatuen Wi-Fi) banatzeko. pfSense Firewall bat instalatuko da sareko segurtasuna bermatzeko (Protectli Vault batean exekutatuta)[cite: 30].
*   **Babeskopiak (Backup)**: Synology NAS bat (RAID 1 konfigurazioarekin, adib. 2 × 4TB disko) datuen kopia lokala egiteko (3-2-1 araua jarraituz)[cite: 30]. Sute edo arazo bat egonez gero, NAS-a CPD-tik fisikoki bananduta egotea gomendatzen da[cite: 30].
*   **SAI / UPS**: APC Smart-UPS sistema (adib. 1500 VA) zerbitzariari eta suebakiari korronte etenik gabe eusteko[cite: 30].

## 7. Mantentze Lanak eta Maiztasuna

| Ekintza | Maiztasuna | Arduraduna |
|---|---|---|
| Sistemaren eta birusen aurkako eguneratzeak | Automatikoa, hileroko berrikuspena | Informatika[cite: 29] |
| Babeskopien (Backup) egoera berrikustea | Astero / Hilero | Informatika[cite: 29] |
| Hardwarearen eta SSD/NAS osasunaren azterketa | Sei hilean behin | Informatika[cite: 29] |
| Ekipoen eta periferikoen garbiketa fisikoa | Hiruhilean behin | Sail bakoitza / Informatika[cite: 29] |
| Ekipoak berritzea | 4–5 urtean behin | Zuzendaritza / Informatika[cite: 29] |

## 8. Ekipoen Inbentarioa

*(Taula hau enpresako datu errealekin beteko da)[cite: 29]*

| Zkia. | Langilea | Saila | Ekipo Eredua | Serie Zenbakia (S/N) | Erosketa Data |
|---|---|---|---|---|---|
| 01 | [Izena] | Informatika | ThinkPad T14 Gen 6 | - | 2026-10-xx |
| 02 | [Izena] | Administrazioa | ThinkCentre M70q Gen 5 | - | 2026-10-xx |
| 03 | [Izena] | Marketina | HP Z2 Tower | - | 2026-10-xx |
| ... | ... | ... | ... | ... | ... |

## 9. Ondorioa

Hiru profil hauek `servertoyou` enpresaren sail bakoitzeko beharrei erantzuten diete, errendimendu, segurtasun eta kostu oreka bat mantenduz[cite: 29]. Administrazioan espazioa eta erosotasuna lehenetsi da (Mini PC + pantaila bikoitzak)[cite: 30], Marketinean potentzia garbia eta grafikoa, eta Informatikan beharrezko prozesatze-ahalmena sare osoa eta azpiegitura birtuala mantentzeko[cite: 29].
