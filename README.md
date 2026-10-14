# ⚡ TEAM-DOC

> **Azpiegitura Teknologikoaren, Sistemen Administrazioaren eta Segurtasunaren Dokumentazio Zentralizatua.**

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Live%20Docs-blue?logo=github&style=flat-square)](docs/)
[![Jekyll](https://img.shields.io/badge/Jekyll-Minima-red?logo=jekyll&style=flat-square)](https://jekyllrb.com/)
[![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D6?logo=windows&style=flat-square)](docs/Sistemas%20Eragileak%20Administratzea%20(SEA).md)
[![Ubuntu](https://img.shields.io/badge/Ubuntu%20Server-22.04%20LTS-E95420?logo=ubuntu&style=flat-square)](docs/Sistemas%20Eragileak%20Administratzea%20(SEA).md)
[![WordPress](https://img.shields.io/badge/WordPress-CMS-21759B?logo=wordpress&style=flat-square)](docs/WAE/Wordpress.md)
[![GLPI](https://img.shields.io/badge/GLPI-10.0-blueviolet?style=flat-square)](docs/WAE/GLPI.md)

---

## 📖 Aurkibidea / Dokumentazio Moduluak

Proiektu honek enpresa-azpiegitura oso baten hedapena, konfigurazioa eta auditoria jasotzen ditu:

| Modulua | Deskribapena | Fitxategia |
|:---|:---|:---|
| 🖥️ **Sistemak (SEA)** | Active Directory (`sty.eus`), GPO politikak, CUPS inprimaketa-zerbitzaria | [Ikusi SEA](docs/Sistemas%20Eragileak%20Administratzea%20(SEA).md) |
| 🌐 **WordPress (WAE)** | IIS 10 web zerbitzaria + MariaDB urruneko datu-base segurua | [Ikusi WordPress](docs/WAE/Wordpress.md) |
| 🎫 **GLPI 10 (WAE)** | IT baliabideen inbentario automatikoa eta intzidentzia-txartelak | [Ikusi GLPI](docs/WAE/GLPI.md) |
| ⚔️ **Pentesting & HACK** | Kali Linux vs Metasploitable 2 laborategia, Nmap eskaneatzea | [Ikusi HACK](docs/Hackin%20Documentacion.md) |
| 🛡️ **Segurtasuna** | Informazioaren segurtasun-plana, baimenak eta babes-neurriak | [Ikusi Segurtasuna](docs/Segurtasuna%20documentacion.md) |
| ⚙️ **Hardwarea** | ServerToYou ekipoen fitxa teknikoak eta inbentario orokorra | [Ikusi Hardware](docs/Hardware.md) |
| 🌱 **Jasangarritasuna** | ESG irizpideak, energia-eraginkortasuna eta birziklapena | [Ikusi Jasangarritasuna](docs/06-jasangarritasuna.md) |

---

## 🗺️ Sare eta Zerbitzarien Arkitektura

```mermaid
graph TD
    subgraph LAN_Korporatiboa ["LAN Korporatiboa (192.168.10.0/24)"]
        DC["Windows Server 2022 (192.168.10.1)<br>AD, DNS, DHCP, IIS 10<br>(WordPress & GLPI)"]
        PRINT["Ubuntu Server (192.168.10.3)<br>CUPS Print Server"]
        CLIENTS["Bezero Ekipoak (192.168.10.150+)<br>Windows 10 / 11"]
        DB["MariaDB Isolatua (192.168.10.4)<br>wordpressdb & glpidb"]
        
        CLIENTS -->|Domeinua & GPO| DC
        DC -->|SQL Konexio Segurua| DB
        CLIENTS -->|Inprimaketa Lan| PRINT
    end

    subgraph LAB_Audit ["Pentesting Laborategi Isolatua (10.0.2.0/24)"]
        KALI["Kali Linux (10.0.2.2)<br>Atacante / Auditoria"]
        TARGET["Metasploitable 2 (10.0.2.3)<br>Biktima / Proba Ahula"]
        
        KALI -.->|Nmap / Esplotazio Probak| TARGET
    end
```

---

## 🚀 GitHub Pages Bidezko Bistarapena

Dokumentazio hau webgune interaktibo gisa ikus daiteke GitHub Pages aktibatuta (`docs/` karpeta erabiliz):

1. Git biltegiko **Settings** > **Pages** atalera joan.
2. **Build and deployment** atalean:
   - Source: `Deploy from a branch`
   - Branch: `main` (edo erabilitako adarra), `/docs` karpeta aukeratuta.
3. Gorde eta minutu gutxitan zure webgune korporatiboa prest egongo da estilo modernoarekin!
