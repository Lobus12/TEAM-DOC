---
layout: page
title: Sareak
---
***SAREKO ZERBITZUAK***

<img width="729" height="276" alt="imagen" src="https://github.com/user-attachments/assets/f700aad2-def2-4028-9309-8319d22a4a03" />

Lehenik gure serbidorearen barruan, Agregar Roles y ezaugarriak emango diogu

<img width="612" height="61" alt="imagen" src="https://github.com/user-attachments/assets/7255fe89-2a3d-4723-9658-95276743eb5e" />
 
DHCP zerbitzaria aplikatuko dugu eta instalaketa ipiniko dugu.

<img width="368" height="117" alt="imagen" src="https://github.com/user-attachments/assets/36174478-1124-4889-8cb2-0b8423165d8c" />
 
DHCP barruan, ambito berri bat sortuko dugu, Langileak izenekoa  
<img width="752" height="164" alt="imagen" src="https://github.com/user-attachments/assets/6de34e11-6863-4c20-8ae7-8c9225a6eeaf" />

ambitoa konfiguratu ondoren, reserba bat egingo dugu, gure kasuan 192.168.10.150-192.168.10.200 

<img width="759" height="263" alt="imagen" src="https://github.com/user-attachments/assets/0b92e8bf-fd7f-4db3-b0f4-7e80c526bab3" />

concesioen barruan hainbat zerbitzari edukiko ditugu:  
→ 192.168.10.100: windows 11 bat baimen guztiekin   
→ 192.168.10.151 windows 11 bat kliente moduan  
→ 192.168.10.200 beste reserva bat da, gure kasuan cups inprimagailuak ditugun ubuntu serverrra  
→ 192.168.10.152 Jitsi video konferentzia instalatuta dugun ubuntu serverra   
→ 192.168.10.150 datu basea eta wordpress dugun ubuntu serverra
<img width="1021" height="361" alt="irudia" src="https://github.com/user-attachments/assets/1a777716-93d5-4ee1-a512-1b4545f21f75" />


<img width="754" height="377" alt="imagen" src="https://github.com/user-attachments/assets/b0ce6278-0f81-4ec3-9186-39cb8e6cfcb3" />

lehen aipatu bezala, bi erreserba egin ditugu.  
Lehenengo erreserba → arrisku kasuan, windows11 bat dugu admin moduan  
bigarrengo erreserba → Inprimagailuarentzat egindako erreseba bat da.

*DNS*  
<img width="761" height="216" alt="imagen" src="https://github.com/user-attachments/assets/e11800a1-1956-48a2-83ff-bfdf25e1760d" />
  
DNSaren barruan, zona de busqueda directa bat egingo dugu sty.eus izenarekin.  
Gure zonaren barruan jitsi izenoko host bat sortuko dugu, jitsi instalatutako makinaren ipa ipiniz.

<img width="762" height="190" alt="imagen" src="https://github.com/user-attachments/assets/9e31a9ce-bcea-4111-9bcf-b6599d789a56" />
  
beste atalarekin bat, zona de busqueda inversa bat ipiniko dugu, zona honetan puntero bat sortuko dugu web-a bilatzerakoan jitsi.sty.eus ipinita bilatzeko  
<img width="763" height="359" alt="imagen" src="https://github.com/user-attachments/assets/a222ea3e-4f20-4607-ace0-de92d2dafb28" />
esta azkenik reenviadore condicional bat edukiko dugu, kasu honetan google.

<img width="757" height="453" alt="imagen" src="https://github.com/user-attachments/assets/1f467cdc-242e-4eaf-b9ee-5edf6e2c8954" />
 
instalatzerkoan administrazio eta bideo dei portuak irekiko ditugu

<img width="757" height="512" alt="imagen" src="https://github.com/user-attachments/assets/32da3704-1bbc-4cbe-aa1f-e0462bd0a24f" />
 
hemen ikusi dezakegu nola bi erabiltzaile desbedin dauden bilera baten jitsi.sty.eus, linkarekin
