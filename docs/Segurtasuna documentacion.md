# INFORMAZIOAREN SEGURTASUN DOKUMENTAZIOA

## 1. Sarrera eta Helburua
Dokumentu honen helburua erakundearen azpiegitura teknologikoa, datu korporatiboak eta zerbitzuak babesteko jarraitu beharreko segurtasun-neurriak eta gidalerroak zehaztea da. Helburu nagusia informazioaren **konfidentzialtasuna, osotasuna eta erabilgarritasuna** bermatzea da.

## 2. Sarearen Segurtasuna eta Azpiegitura
Sare korporatiboa mehatxuetatik babesteko, honako neurri hauek ezartzen dira:

* **Sarearen Segmentazioa**: Sarea eremu isolatuetan banatuko da bideratze eta suebaki bidez (adibidez: WAN, LAN, DMZ eta Haririk gabeko sarea).
* **Suebakia (Firewall)**: pfSense (edo antzeko sistema bat) erabiliko da sarrerako eta irteerako trafikoa kontrolatzeko. Besterik adierazi ezean, trafiko guztia blokeatuko da (*Deny All* printzipioa).
* **Urruneko Sarbidea**: Sare korporatibora kanpotik sartzeko, ezinbestekoa izango da VPN konexio seguru bat erabiltzea (adib.: OpenVPN edo IPsec), pasahitz eta ziurtagiri bidez babestuta. Ez da administrazio-porturik (SSH, RDP, WebGUI) zuzenean internetera irekiko.
* **Komunikazio Seguruak**: Web zerbitzu guztiek HTTPS erabiliko dute, indarrean dauden TLS ziurtagiriekin.

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
