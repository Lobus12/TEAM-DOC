---
layout: page
title: HACK / Pentesting
---

# **HACK: Pentesting eta Ahultasunen Azterketa** 

## **1\. Proba-ingurunea ezartzea (Infraestructura de pruebas)**

Auditoria eta penetrazio-probak (pentesting) egiteko, ekoizpen-saretik isolatuta dagoen ingurune kontrolatu bat ezarri da. Azpiegitura honek eraso-bektore errealak modu seguruan simulatzea ahalbidetzen du, enpresaren sarea arriskuan jarri gabe.

> * **Makina erasotzailea (Atacante):** Kali Linux (IP: 10.0.2.2)  
> * **Makina ahula (Objetivo):** Metasploitable 2 (IP: 10.0.2.3)  
> * **Egoera:** Test ingurunea konektatuta eta aktibo dago barne-sare baten bidez (Red Interna).

## **2\. Pentesting-aren Faseak**

Enpresako sarearen adibidea erabiliz, pentesting baten fase nagusiak jarraitu dira:

### **2.1. Deskubrimendua eta Eskaneatzea (Reconocimiento y Escaneo)**

Lehenengo urrats teknikoa makina helburua enumeratzea izan da, eraso-azalera identifikatzeko. nmap tresna erabiliz, ataka irekiak eta exekutatzen ari diren zerbitzuen bertsio zehatzak aurkitu dira.  
`nmap -sV 10.0.2.3`  
**Identifikatutako zerbitzu nagusiak (Eskaneatzearen emaitzak):**

> * **21/tcp ataka:** FTP \- vsftpd 2.3.4  
> * **22/tcp ataka:** SSH \- OpenSSH 4.7p1  
> * **80/tcp ataka:** HTTP \- Apache httpd 2.2.8  
> * **139/tcp, 445/tcp atakak:** NetBIOS/SMB \- Samba 3.X  
> * **1524/tcp ataka:** Bindshell (Metasploitable root shell)  
> * **6667/tcp ataka:** IRC \- UnrealIRCd
>
><img width="662" height="604" alt="Captura de pantalla 2026-10-14 083501" src="https://github.com/user-attachments/assets/666cf8c0-b3ad-42ea-94f7-ca71052f25d9" />



### **2.2. Ahultasunen Analisia (Análisis de Vulnerabilidades)**

Eskaneatze-fasean bildutako informazioan oinarrituta, makina helburuaren software zaharkituari lotutako ahultasun ezagunak (CVE) identifikatu dira searchsploit eta datu-base publikoak erabiliz.

### **2.3. Esplotazioa (Explotación)**

Fase honetan, identifikatutako ahultasunak ustiatzen dira sistemarako sarbidea lortzeko (adibidez, Metasploit Framework erabiliz FTP zerbitzuaren backdoor-a ustiatuz).

## **3\. Identifikatutako Ahultasunak eta Balizko Erasoak**

Enpresako sarean egon litezkeen ahultasun nagusiak eta jasan ditzaketen eraso motak honako hauek dira:

### **3.1. FTP Zerbitzua (vsftpd 2.3.4)**

> * **Ahultasuna:** Komandoak exekutatzeko backdoor-a (Puerta trasera).  
> * **CVE:** CVE-2011-2523  
> * **Eraso mota:** vsftpd-ren bertsio honek atzeko ate maltzur bat dauka bere iturburu-kodean. Erasotzaile bat :) (aurpegiera) duen erabiltzaile-izen batekin saioa hasten saiatzen bada, zerbitzuak automatikoki bigarren ataka bat irekitzen du (6200), shell interaktibo bat eskainiz.  
> * **Inpaktua:** Kritikoa. Autentifikatu gabeko urruneko erasotzaile bati zerbitzariaren kontrol administratiboa hartzea ahalbidetzen dio.

### **3.2. IRC Zerbitzua (UnrealIRCd)**

> * **Ahultasuna:** Urruneko komandoak exekutatzeko backdoor-a (RCE).  
> * **CVE:** CVE-2010-2075  
> * **Eraso mota:** UnrealIRCd-ren deskarga ofizialaren fitxategi bat konprometitu zen troiar bat txertatzeko. Erasotzaile batek IRC atakaren bidez komando zehatzak bidaliz ustia dezake, eta zerbitzariak tokiko pribilegioekin exekutatuko ditu.  
> * **Inpaktua:** Kritikoa. Makinaren erabateko konpromisoa kode-exekuzioaren bidez.

### **3.3. Bindshell Zerbitzua (1524 Ataka)**

> * **Ahultasuna:** Autentifikaziorik gabeko sarbidea (Konfigurazio-akatsa).  
> * **Eraso mota:** Sistemak administratzaile-komandoen kontsola bat (shell) zuzenean sarera erakusten du. Ez du exploit konplexurik behar; edonor konekta daiteke oinarrizko sare-tresnak erabiliz (adibidez, Netcat: nc 10.0.2.3 1524\) eta berehala sarbidea lortu root erabiltzaile gisa.  
> * **Inpaktua:** Kritikoa. Zuzeneko ate irekia da sistemaren administraziorako.

## **4\. Giza Faktorea (Ingeniaritza Soziala)**

Enpresa baten segurtasuna ez dago soilik sistema teknikoen (hardware eta software) sendotasunaren menpe. Askotan, erasotzaileek suebakiak eta IDS sistemak saihestea lortzen dute katebegi ahulena erasoz: **pertsonak**.  
Sistema teknikoak ondo adabakita (parcheados) badaude ere, ziberkriminal batek enpresako sarea arriskuan jar dezake giza faktorea erabiliz, Ingeniaritza Sozialeko teknikak aplikatuz:

> * **Phishing erasoak:** Langileei mezu elektroniko iruzurtiak bidaltzea, legezko erakundeen nortasuna ordezkatuz (adibidez, IT saila). Helburua da langilea engainatzea bere sarbide-kredentzialak eman ditzan edo sare korporatiboan malwarea (Ransomwarea, adibidez) deskargatuko duen esteka batean klik egin dezan.  
> * **Baiting (Gailu-tranpak):** Erasotzaile batek nahita "ahaztu" dezake infektatutako USB pendrive bat enpresaren harreran edo aparkalekuan. Langile batek, jakin-minez, bulegoko ekipo batera konektatzen badu, kode maltzurra automatikoki exekutatu daiteke, kanporako konexio bat sortuz eta barne-sarerako sarbidea emanez.  
> * **Pretexting:** Telefono-deien bidezko nortasun-ordezkapena, non erasotzaileak premiazko aitzakia bat (aitzakia bat) asmatzen duen langile bat estutzeko eta pasahitzak edo isilpeko informazioa ezagutarazteko konbentzitzeko.

## **5\. Mitigazio-neurriak eta Gomendioak**

Azpiegitura ziurtatzeko eta eraso teknikoak zein antolakuntzazkoak saihesteko, honako neurri hauek proposatzen dira:

> 1. **Adabakien kudeaketa (Gestión de Parches):** Ahula den software guztia (vsftpd, Apache, UnrealIRCd, Samba) berehala eguneratzea beren azken bertsio egonkorretara.  
> 2. **Hardening eta Atakak ixtea:** Funtsezkoak ez diren zerbitzuak desgaitzea. 1524 ataka (Bindshell) bezalakoak betiko itxi behar dira.  
> 3. **Trafikoaren iragazketa (Suebakia / Firewall):** Suebaki-arauak ezartzea (LAN eta DMZ segmentazioa) legezko zerbitzuetarako sarbidea baimendutako IP helbideetara soilik mugatzeko.  
> 4. **Zibersegurtasuneko prestakuntza:** Enpresako langile guztiei kontzientziazio-ikastaroak ematea aldizka, mezu elektroniko susmagarriak identifikatzen ikas dezaten eta ingeniaritza sozialaren arriskuak ulertu ditzaten.
