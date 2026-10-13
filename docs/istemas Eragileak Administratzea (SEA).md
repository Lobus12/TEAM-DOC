
```markdown
---
title: Sistemas Eragileak Administratzea (SEA) - Active Directory, GPO, Inprimaketa eta Prozesuak
---


# Sistemas Eragileak Administratzea (SEA) ✅

**Irakasgaia:** Sistemas Eragileak Administratzea (SEA)  
**Trazabilitatea / IE:** SEA IE1 (Direktorio Zerbitzua & GPOak), SEA IE2 (Prozesuen Kudeaketa), SEA IE5 (Inprimaketa Zerbitzaria)  
**Azpiegitura:** Windows Server 2022 (`192.168.10.1` / Domeinua: `sty.eus`) + Ubuntu Server (`192.168.10.3` / CUPS Print Server) + Windows 10/11 Bezero-ekipoak (`192.168.10.150` DHCP)

---

## 1. Direktorio Zerbitzua: Active Directory Egitura Logikoa (SEA: IE1) ✅

BioHealth enpresaren baliabideak eta erabiltzaileak modu zentralizatuan kudeatzeko `sty.eus` Active Directory domeinua ezarri da. Hierarkia logikoa Unitate Antolatzaile (OU), Erabiltzaile eta Taldeen bidez egituratu da.

### Egitura Logikoaren Parametroak

| Osagaia | Balioa / Izena | Deskribapena / Erabilera BioHealth-en |
|---|---|---|
| **Domeinu Nagusia** | `sty.eus` | Zentroko Active Directory domeinua |
| **OU Nagusia** | `OU=BioHealth_Enpresa` | Enpresako osagai guztiak biltzen dituen erroko OUa |
| **Azpi-OUak** | `OU=Langileak`, `OU=Sistemak`, `OU=Zuzendaritza`, `OU=Makinak` | Sail eta ekipo motaren araberako isolamendua |
| **Segurtasun Taldeak** | `GRP_Langileak`, `GRP_Sistemak`, `GRP_Zuzendaritza` | Sarbide baimenak taldeka esleitzeko |
| **Erabiltzaile Kontuak** | `bio_admin`, `langile1`, `langile2`, `zuzendari1` | Bakoitza bere OUn eta taldean integratuta |
| **Domeinuko Ekipoak** | `WIN10-CLIENT1`, `JITIS` (Ubuntu) | Domeinura batutako bezeroak eta zerbitzariak |

### Irudi Proposamena 1
> **Proposatutako irudia:** `ad-estructura-ou.png`  
> **Deskribapena:** Active Directory Usuarios y Equipos (`dsa.msc`) leihoaren captura. Ezkerreko zuhaitzean `sty.eus` domeinua zabalduta ikusi behar da `BioHealth_Enpresa` OUarekin eta bere azpi-OU guztiekin (`Langileak`, `Sistemak`, `Zuzendaritza`). Eskuineko panelean sortutako erabiltzaileak eta taldeak ikusi behar dira.

![Active Directory Egitura eta OUak](../../irudiak/SEA/ad-estructura-ou.png)
*Irudia 1: Active Directory-ko Unitate Antolatzaileen (OU), Taldeen eta Erabiltzaileen egitura logikoa `sty.eus` domeinuan. ✅*

---

## 2. GPO Politikak eta Pertsonalizazioa (SEA: IE1 - D) ✅

GPO (Group Policy Objects) bidez segurtasuna, konfigurazioak eta erabiltzaileen ingurunea zentralizatuta kudeatzen dira. Errubrikak eskatutako gutxienekoak (2 arau orokor eta 2 arau zehatz) betetzen dira:

### Aplikaturiko GPO Politiken Taula

| GPO Izena | Aplikazio Eremua (OU / Taldea) | Ezarritako Araua / Politika Zehatza | Mota |
|---|---|---|---|
| `GPO_General_BioHealth` | Erabiltzaile Guztiak (`OU=BioHealth_Enpresa`) | **Arau 1:** Control Panel eta Configuración sarbidea blokeatzea (`Prevent access to Control Panel and PC settings`). | Orokorra |
| `GPO_General_BioHealth` | Erabiltzaile Guztiak (`OU=BioHealth_Enpresa`) | **Arau 2:** Atzeko irudi (Wallpaper) korporatiboa ezartzea eta aldaketak desgaitzea (`Desktop Wallpaper`). | Orokorra |
| `GPO_Langileak_Restriccion` | `OU=Langileak` (`GRP_Langileak`) | **Arau 3:** USB biltegiratze-gailuen idazketa/irakurketa blokeatzea (`Removable Disks: Deny all access`). | Zehatza |
| `GPO_Langileak_Restriccion` | `OU=Langileak` (`GRP_Langileak`) | **Arau 4:** Agindu-kontsola (`cmd.exe`) eta PowerShell exekutatzea blokeatzea (`Prevent access to the command prompt`). | Zehatza |

### Irudi Proposamena 2
> **Proposatutako irudia:** `gpo-administrador-politicas.png`  
> **Deskribapena:** Administrador de Directivas de Grupo (`gpmc.msc`) leihoaren captura. Ezkerreko zuhaitzean `GPO_General_BioHealth` eta `GPO_Langileak_Restriccion` objektuak ikusi behar dira beren OU-ei lotuta (linked). Eskuineko panelean "Configuración" (Settings) fitxa hautatuta ikusi behar da aplikatutako arauekin.

![GPO Kudeaketa eta Loturak](../../irudiak/SEA/gpo-administrador-politicas.png)
*Irudia 2: Administrador de Directivas de Grupo panelean sortutako GPO orokor eta zehatzak `sty.eus` domeinuko OU-etan aplikatuta. ✅*

---

## 3. Sistemaren Prozesuen Kudeaketa eta Jarraipena (SEA: IE2) ✅

Active Directory zerbitzuak eta berari lotutako prozesuak kudeatzeko, gelditzeko, berriro abiarazteko eta jarraipena egiteko tresna grafikoak zein komando-lineakoak erabili dira.

### A. Zerbitzuen Kudeaketa (Powershell / CMD)
```powershell
# Active Directory Domain Services zerbitzuaren egoera egiaztatu
Get-Service -Name NTDS, DNS, DHCP

# DNS eta DHCP zerbitzuak berabiarazi
Restart-Service -Name DNS -Force
Restart-Service -Name DHCPServer -Force

```

### B. Prozesuak Monitorizatzeko Erabilitako Tresnak

| Tresna | Mota | Erabilera eta Helburua |
| --- | --- | --- |
| `taskmgr.exe` (Administrador de Tareas) | Grafikoa | CPU, Memoria eta Diskoaren erabilera denbora errealean ikusi; prozesu amaiera indartua. |
| `resmon.exe` (Monitor de Recursos) | Grafikoa | Sareko konexioak, fitxategien erabilera eta Active Directory (`ntds.exe`) prozesuaren karga aztertu. |
| `Get-Process` / `Stop-Process` | PowerShell | Prozesu zehatzak izenez edo PID bidez identifikatu eta kudeatu. |

### Irudi Proposamena 3

> **Proposatutako irudia:** `sea-prozesuak-monitorizazioa.png`
> **Deskribapena:** Windows Server-eko *Administrador de Tareas* edo *Monitor de Recursos* leihoaren captura. Bertan `NTDS` (Active Directory Services), `dns.exe` eta `svchost.exe` prozesuak exekuzioan eta baliabideen kontsumoa erakusten ikusi behar da.


*Irudia 3: Sistemaren prozesuen kudeaketa eta Active Directory zerbitzuen monitorizazioa tresna grafiko zein komando bidez. ✅*

---

## 4. Inprimaketa-Zerbitzariak eta Sare Inprimagailuak (SEA: IE5) ✅

Windows Server zein Linux (Ubuntu CUPS) inguruneetan inprimaketa-zerbitzariak konfiguratu dira, sareko erabiltzaileen artean inprimagailuak partekatzeko eta bi sistema eragileen arteko bateragarritasuna bermatzeko.

### A. Inprimaketa Portuak eta Protokoloak

| Sistema Eragilea | Zerbitzaria / Teknologia | Inprimaketa Portua | Erabilitako Protokoloa |
| --- | --- | --- | --- |
| **Windows Server 2022** | Print Management (`spoolsv.exe`) | TCP 9100 / UDP 137-139, 445 | RAW / LPR / SMB / IPP |
| **Linux (Ubuntu Server)** | CUPS (Common Unix Printing System) | TCP 631 | IPP (Internet Printing Protocol) / HTTP |

### B. Inprimagailuen Konfigurazioa eta Baimenak

| Inprimagailu Izena | Zerbitzaria / Ostalaria | Baimenak eta Rolak | Sareko Helbidea |
| --- | --- | --- | --- |
| `HP_Zuzendaritza_Win` | Windows Server (`192.168.10.1`) | `GRP_Zuzendaritza` (Inprimatu eta Kudeatu); `GRP_Langileak` (Ezeztatuta) | `\\192.168.10.1\HP_Zuzendaritza_Win` |
| `CUPS_Lab_Inprimagailua` | Ubuntu Server (`192.168.10.3`) | `GRP_Langileak` eta `GRP_Sistemak` (Inprimatu soilik) | `http://192.168.10.3:631/printers/CUPS_Lab_Inprimagailua` |

### C. Sistema Eragileen Arteko Cross-Platform Proba Cross-Tests

1. **Windows Bezerotik Linux (CUPS) Inprimagailura:**
* Windows 10 bezeroan IPP inprimagailu berria gehitu da: `http://192.168.10.3:631/printers/CUPS_Lab_Inprimagailua`. Inprimaketa proba **arrakastatsua** izan da.


2. **Linux Bezerotik Windows Server Partekatutako Inprimagailura:**
* Linux ekipoz SMB/CIFS bidez konectatu da: `smb://192.168.10.1/HP_Zuzendaritza_Win` (domeinu-erabiltzaile bidez autentifikatuta). Inprimaketa proba **arrakastatsua** izan da.



### Irudi Proposamena 4

> **Proposatutako irudia:** `win-print-management.png`
> **Deskribapena:** Windows Server-eko *Administración de Impresión* (`printmanagement.msc`) leihoaren captura. Ezkerreko menuan inprimaketa-zerbitzaria eta partekatutako inprimagailuak (`HP_Zuzendaritza_Win`) erakutsi behar dira, Segurtasun fitxan esleitutako baimenekin (`GRP_Zuzendaritza`).

### Irudi Proposamena 5

> **Proposatutako irudia:** `cups-web-interface.png`
> **Deskribapena:** Ubuntu Server-eko CUPS web interfazearen captura (`http://192.168.10.3:631/printers`). Partekatutako inprimagailua "Idle, accepting jobs, published on network" egoeran ikusi behar da, eta inprimatutako proba-orri baten erregistroarekin (Completed Jobs).


*Irudia 4: Windows Server 2022-ko Administración de Impresión atala eta partekatutako inprimagailuaren baimenak. ✅*


*Irudia 5: Ubuntu Server-eko CUPS web interfazea inprimagailua sarean argitaratuta duelarik. ✅*

---

## 5. Arazoak eta Soluzioak (SEA Troubleshooting) ✅

| Modulua / Atal | Arazoa | Kausa | Soluzioa |
| --- | --- | --- | --- |
| **AD / GPO** | Bezero-ekipoak ez ditu GPO politikak berehala hartzen | Windows-ek 90 minutuz behin berritzen ditu politikak atzeko planoan | Bezeroaren kontsolan `gpupdate /force` exekutatu eta saioa berriro hasi |
| **AD / GPO** | GPOa ez zaio erabiltzaile jakin bati aplikatzen | Erabiltzailea ez zegoen GPOa lotuta dagoen OUaren barruan | Erabiltzailearen kontua dagokion Unitate Antolatzailera (`OU=Langileak`) mugitu `dsa.msc` bidez |
| **CUPS / Linux** | Windows bezeroak ezin du CUPS inprimagailua saretik bilatu | CUPS-en `/etc/cups/cupsd.conf` fitxategian kanpoko sareko atzipenak blokeatuta zeuden | `cupsd.conf` fitxategian `Listen *:631` eta `<Location /> Allow All </Location>` ezarri eta `sudo systemctl restart cups` exekutatu |
| **Windows Print** | Inprimagailua ezin da domeinuko bezerotik gehitu | Point and Print Restriction politikek ez zioten drimberra instalatzen uzten erabiltzaile arruntai | GPO bidez `Point and Print Restrictions` ezarpena desgaitu edo kontrolatzaile fidagarriak onartu |

```

```
