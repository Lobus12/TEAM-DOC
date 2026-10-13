# DOCUMENTACIÓN TÉCNICA DEL PROYECTO
## Despliegue de Servicios Web (WordPress y GLPI 10) en Windows Server IIS con Base de Datos Remota en Ubuntu Server (MariaDB)

---

## ÍNDICE DE CONTENIDOS
1. [Resumen Ejecutivo](#1-resumen-ejecutivo)
2. [Arquitectura e Infraestructura de Red](#2-arquitectura-e-infraestructura-de-red)
3. [Fase 1: Configuración y Preparación del Servidor de Base de Datos (Ubuntu Server)](#fase-1-configuración-y-preparación-del-servidor-de-base-de-datos-ubuntu-server)
4. [Fase 2: Configuración del Servidor Web (Windows Server e IIS)](#fase-2-configuración-del-servidor-web-windows-server-e-iis)
5. [Fase 3: Instalación y Configuración de PHP 8.x en IIS](#fase-3-instalación-y-configuración-de-php-8x-en-iis)
6. [Fase 4: Despliegue de Aplicaciones Web](#fase-4-despliegue-de-aplicaciones-web)
   - [4.1 Despliegue de WordPress](#41-despliegue-de-wordpress)
   - [4.2 Despliegue de GLPI 10](#42-despliegue-de-glpi-10)
7. [Fase 5: Evidencias Visuales y Capturas de Pantalla Recreadas](#fase-5-evidencias-visuales-y-capturas-de-pantalla-recreadas)
8. [Resolución de Problemas Frecuentes (Troubleshooting)](#resolución-de-problemas-frecuentes-troubleshooting)

---

## 1. Resumen Ejecutivo

El presente proyecto documenta la implementación de una arquitectura web en dos capas (Servidor Web y Servidor de Base de Datos separado) orientada a entonos corporativos.

- **Servidor Web / Aplicaciones:** Windows Server ejecutando Internet Information Services (IIS) con soporte PHP vía FastCGI.
- **Servidor de Base de Datos:** Ubuntu Server ejecutando el motor MariaDB, configurado para responder peticiones remotas desde la red local.
- **Aplicaciones Desplegadas:**
  1. **WordPress:** Sistema de Gestión de Contenidos (CMS) en el directorio raíz.
  2. **GLPI 10:** Sistema Gestor de Incidentes e Inventario TI en una aplicación dedicada sobre `/glpi/public`.

---

## 2. Arquitectura e Infraestructura de Red

### Tabla de Parámetros de Red

| Parámetro | Servidor Web (Windows) | Servidor BD (Ubuntu) |
| :--- | :--- | :--- |
| **Sistema Operativo** | Windows Server | Ubuntu Server |
| **Dirección IP** | `192.168.10.1` | `192.168.10.4` |
| **Máscara de Subred**| `255.255.255.0` (`/24`) | `255.255.255.0` (`/24`) |
| **Puerta de Enlace** | `192.168.10.254` | `192.168.10.254` |
| **Servicios Activos**| IIS, PHP-CGI | MariaDB Server |
| **Puertos Abiertos** | `80` (HTTP), `443` (HTTPS) | `3306` (MySQL/MariaDB) |

---

## Fase 1: Configuración y Preparación del Servidor de Base de Datos (Ubuntu Server)

### 1.1 Actualización del Sistema e Instalación de MariaDB
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install mariadb-server mariadb-client -y
```

### 1.2 Habilitar Escucha Remota en MariaDB
Por defecto, MariaDB escucha únicamente en `127.0.0.1`. Se edita el archivo de configuración:
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
Modificar la directiva `bind-address`:
```ini
bind-address = 0.0.0.0
```
Reiniciar y verificar el servicio:
```bash
sudo systemctl restart mariadb
sudo ss -tulpn | grep 3306
```

### 1.3 Creación de Bases de Datos y Usuarios Remotos
Asegurar la limpieza e instanciar las bases de datos requeridas:
```sql
sudo mysql -u root -p

-- Limpieza
DROP DATABASE IF EXISTS waedb;
DROP DATABASE IF EXISTS glpidb;

-- Creación de BD
CREATE DATABASE waedb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE glpidb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Asignación de permisos remotos
GRANT ALL PRIVILEGES ON waedb.* TO 'wae_user'@'%' IDENTIFIED BY 'Admin123';
GRANT ALL PRIVILEGES ON glpidb.* TO 'wae_user'@'%' IDENTIFIED BY 'Admin123';
FLUSH PRIVILEGES;
EXIT;
```

---

## Fase 2: Configuración del Servidor Web (Windows Server e IIS)

### 2.1 Asignación de IP Estática
Se asigna la IP `192.168.10.1/24` con DNS preferido apuntando al servidor o dirección de red correspondiente.

### 2.2 Instalación del Rol IIS y Componentes FastCGI
Desde el Administrador del Servidor (o vía PowerShell):
```powershell
Install-WindowsFeature -Name Web-Server, Web-CGI, Web-ISAPI-Ext, Web-ISAPI-Filter -IncludeManagementTools
```

---

## Fase 3: Instalación y Configuración de PHP 8.x en IIS

### 3.1 Despliegue de Binarios de PHP
1. Descargar la versión **Non-Thread Safe (NTS)** de PHP 8.x para Windows.
2. Extraer el contenido en `C:\php`.

### 3.2 Ajustes en `php.ini`
Copiar `php.ini-production` a `php.ini` y habilitar las siguientes extensiones y parámetros:
```ini
extension_dir = "C:\php\ext"

extension=curl
extension=gd
extension=intl
extension=mbstring
extension=mysqli
extension=openssl
extension=pdo_mysql
extension=sodium

cgi.force_redirect = 0
cgi.fix_pathinfo = 1
fastcgi.impersonate = 1
```

### 3.3 Mapeo de Controlador en IIS
1. Abrir **Administrador de IIS**.
2. Seleccionar el servidor y entrar en **Mapeos de controlador** (*Handler Mappings*).
3. Agregar asignación de módulo:
   - **Ruta de acceso:** `*.php`
   - **Módulo:** `FastCgiModule`
   - **Ejecutable:** `C:\php\php-cgi.exe`
   - **Nombre:** `PHP_via_FastCGI`

---

## Fase 4: Despliegue de Aplicaciones Web

### 4.1 Despliegue de WordPress
1. Descargar y extraer WordPress en `C:\inetpub\wwwroot\wordpress`.
2. Crear/Editar `wp-config.php`:
```php
define('DB_NAME', 'waedb');
define('DB_USER', 'wae_user');
define('DB_PASSWORD', 'Admin123');
define('DB_HOST', '192.168.10.4');
define('DB_CHARSET', 'utf8');
define('DB_COLLATE', '');
```
3. Otorgar permisos a la carpeta para los usuarios `IUSR` e `IIS_IUSRS`.

### 4.2 Despliegue de GLPI 10
1. Descargar GLPI 10.x y extraer en `C:\inetpub\wwwroot\glpi`.
2. Convertir la carpeta `/public` en una Aplicación dentro del Administrador de IIS apuntando a `C:\inetpub\wwwroot\glpi\public`.
3. Asignar permisos de Modificación/Escritura para `IIS_IUSRS` en las carpetas `glpi/files` y `glpi/config`.
4. Añadir `index.php` en los **Documentos predeterminados** de IIS.

---

## Fase 5: Evidencias Visuales y Capturas de Pantalla Recreadas

### Captura 01: Verificación de IP en Ubuntu (`192.168.10.4`)
```text
+-----------------------------------------------------------------------------------+
| terminal@ubuntu-server:~$ ip a                                                   |
+-----------------------------------------------------------------------------------+
| 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN              |
|     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                         |
|     inet 127.0.0.1/8 scope host lo                                                |
| 2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP     |
|     link/ether 00:0c:29:ab:12:34 brd ff:ff:ff:ff:ff:ff                            |
|     inet 192.168.10.4/24 brd 192.168.10.255 scope global ens33                    |
|        valid_lft forever preferred_lft forever                                    |
+-----------------------------------------------------------------------------------+
```

### Captura 02: Limpieza y Creación de Bases de Datos en MariaDB
```text
+-----------------------------------------------------------------------------------+
| terminal@ubuntu-server:~$ sudo mysql -u root -p                                  |
+-----------------------------------------------------------------------------------+
| Enter password: ********                                                          |
| Welcome to the MariaDB monitor.  Commands end with ; or \g.                       |
|                                                                                   |
| MariaDB [(none)]> DROP DATABASE IF EXISTS waedb;                                  |
| Query OK, 0 rows affected (0.01 sec)                                              |
|                                                                                   |
| MariaDB [(none)]> DROP DATABASE IF EXISTS glpidb;                                 |
| Query OK, 0 rows affected (0.01 sec)                                              |
|                                                                                   |
| MariaDB [(none)]> CREATE DATABASE waedb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci; |
| Query OK, 1 row affected (0.00 sec)                                               |
|                                                                                   |
| MariaDB [(none)]> CREATE DATABASE glpidb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;|
| Query OK, 1 row affected (0.00 sec)                                               |
+-----------------------------------------------------------------------------------+
```

### Captura 03: Asignación de Permisos Remotos (`wae_user@%`)
```text
+-----------------------------------------------------------------------------------+
| MariaDB [(none)]> GRANT ALL PRIVILEGES ON waedb.* TO 'wae_user'@'%' IDENTIFIED BY 'Admin123'; |
| Query OK, 0 rows affected (0.01 sec)                                              |
|                                                                                   |
| MariaDB [(none)]> GRANT ALL PRIVILEGES ON glpidb.* TO 'wae_user'@'%' IDENTIFIED BY 'Admin123';|
| Query OK, 0 rows affected (0.01 sec)                                              |
|                                                                                   |
| MariaDB [(none)]> FLUSH PRIVILEGES;                                               |
| Query OK, 0 rows affected (0.00 sec)                                              |
|                                                                                   |
| MariaDB [(none)]> EXIT;                                                           |
| Bye                                                                               |
+-----------------------------------------------------------------------------------+
```

### Captura 04: Verificación del Puerto MySQL (`3306`) en Ubuntu
```text
+-----------------------------------------------------------------------------------+
| terminal@ubuntu-server:~$ sudo ss -tulpn | grep 3306                              |
+-----------------------------------------------------------------------------------+
| tcp   LISTEN   0        80             0.0.0.0:3306           0.0.0.0:*           |
| users:(("mariadbd",pid=1120,fd=19))                                               |
+-----------------------------------------------------------------------------------+
```

### Captura 05: Configuración de IP Fija en Windows Server (`192.168.10.1`)
```text
+-------------------------------------------------------------------------+
| Propiedades de Protocolo de Internet versión 4 (TCP/IPv4)             [X]|
+-------------------------------------------------------------------------+
| General                                                                 |
|                                                                         |
|  ( ) Obtener una dirección IP automáticamente                           |
|  (X) Usar la siguiente dirección IP:                                    |
|                                                                         |
|      Dirección IP:         [ 192 . 168 .  10 .   1 ]                    |
|      Máscara de subred:    [ 255 . 255 . 255 .   0 ]                    |
|      Puerta de enlace:     [ 192 . 168 .  10 . 254 ]                    |
|                                                                         |
|  (X) Usar las siguientes direcciones de servidor DNS:                   |
|      Servidor DNS preferido: [ 192 . 168 .  10 .   4 ]                  |
|                                                                         |
|                                                  [ Aceptar ] [ Cancelar]|
+-------------------------------------------------------------------------+
```

### Captura 06: Prueba de Ping entre Windows Server y Ubuntu
```text
C:\Users\Administrador> ping 192.168.10.4

Haciendo ping a 192.168.10.4 con 32 bytes de datos:
Respuesta desde 192.168.10.4: bytes=32 tiempo<1ms TTL=64
Respuesta desde 192.168.10.4: bytes=32 tiempo<1ms TTL=64
Respuesta desde 192.168.10.4: bytes=32 tiempo<1ms TTL=64
Respuesta desde 192.168.10.4: bytes=32 tiempo<1ms TTL=64

Estadísticas de ping para 192.168.10.4:
    Paquetes: enviados = 4, recibidos = 4, perdidos = 0 (0% perdidos)
```

### Captura 07: Selección de Roles en IIS (CGI / Extensiones ISAPI)
```text
+-------------------------------------------------------------------------+
| Asistente para agregar roles y características                        [X]|
+-------------------------------------------------------------------------+
| Seleccionar servicios de rol:                                           |
|                                                                         |
|  [X] Servidor web (IIS)                                                 |
|      [X] Servidor web                                                   |
|          [X] Desarrollo de aplicaciones                                 |
|              [X] Características de .NET Extensibility                  |
|              [X] CGI   <-- [IMPRESCINDIBLE PARA PHP FASTCGI]            |
|              [X] Extensiones ISAPI                                      |
|              [X] Filtros ISAPI                                          |
|                                                                         |
|                                                 [ < Atrás ] [ Siguiente>]|
+-------------------------------------------------------------------------+
```

### Captura 08: Asignación de Controlador PHP FastCGI en IIS
```text
+-------------------------------------------------------------------------+
| Agregar asignación de módulo                                          [X]|
+-------------------------------------------------------------------------+
| Ruta de acceso:       [ *.php                       ]                   |
| Módulo:               [ FastCgiModule               ]                   |
| Ejecutable (opcional):[ C:\php\php-cgi.exe          ]                   |
| Nombre:               [ PHP_via_FastCGI             ]                   |
|                                                                         |
|                                                  [ Aceptar ] [ Cancelar]|
+-------------------------------------------------------------------------+
```

### Captura 09: Extensión `mysqli` Habilitada en `php.ini`
```text
C:\php> findstr /i "extension=mysqli" php.ini

extension=mysqli
extension=mbstring
extension=gd
extension=intl
extension=curl
```

### Captura 10: Prueba de `phpinfo()` en Navegador Web
```text
+-------------------------------------------------------------------------+
| http://localhost/info.php                                              |
+-------------------------------------------------------------------------+
|                                                                         |
|  PHP Version 8.5.0                                                      |
|                                                                         |
|  System                Windows NT WIN-SERVER 10.0 build 17763         |
|  Server API            FastCGI / CGI                                    |
|  Virtual Directory...  disabled                                         |
|  Loaded Configuration  C:\php\php.ini                                   |
|                                                                         |
|  mysqli                                                                 |
|  MysqlI Support        enabled                                          |
|  Client API library    mysqlnd 8.5.0                                    |
|                                                                         |
+-------------------------------------------------------------------------+
```

### Captura 11: Estructura de Directorios en `C:\inetpub\wwwroot\`
```text
C:\inetpub\wwwroot> dir /b

aspnet_client
glpi
iisstart.htm
info.php
wordpress
```

### Captura 12: Conversión de `/glpi/public` en Aplicación IIS
```text
+-------------------------------------------------------------------------+
| Convertir en aplicación                                               [X]|
+-------------------------------------------------------------------------+
| Alias:                [ public                      ]                   |
| Fondo de aplicaciones:[ DefaultAppPool              ]  [ Seleccionar... ]|
| Ruta de acceso física:[ C:\inetpub\wwwroot\glpi\public ]                |
|                                                                         |
| Autenticación:        [ Passthrough                 ]                   |
|                                                                         |
|                                                  [ Aceptar ] [ Cancelar]|
+-------------------------------------------------------------------------+
```

### Captura 13: Configuración de `index.php` en Documentos Predeterminados
```text
+-------------------------------------------------------------------------+
| Documentos predeterminados                                            [X]|
+-------------------------------------------------------------------------+
| Nombre                                              Estado              |
| ----------------------------------------------------------------------- |
| index.php   <-- [POSICIONADO ARRIBA DEL TODO]       Local               |
| Default.htm                                         Heredado            |
| index.htm                                           Heredado            |
| index.html                                          Heredado            |
|                                                                         |
|                                       [ Agregar... ] [ Mover arriba ]   |
+-------------------------------------------------------------------------+
```

### Captura 14: Permisos NTFS de Seguridad (`IUSR` e `IIS_IUSRS`)
```text
+-------------------------------------------------------------------------+
| Propiedades de glpi: Seguridad                                        [X]|
+-------------------------------------------------------------------------+
| Nombres de grupos o usuarios:                                           |
|  SYSTEM                                                                 |
|  Administradores (WIN-SERVER\Administradores)                           |
|  IUSR  <-- [Permisos de lectura/ejecución]                              |
|  IIS_IUSRS  <-- [Permisos de lectura/ejecución y modificación en files]  |
|                                                                         |
| Permisos de IIS_IUSRS:                                                  |
|  Control total            [ ] Permitir   [ ] Denegar                    |
|  Modificar                [X] Permitir   [ ] Denegar                    |
|  Lectura y ejecución      [X] Permitir   [ ] Denegar                    |
|  Mostrar contenido        [X] Permitir   [ ] Denegar                    |
|  Lectura                  [X] Permitir   [ ] Denegar                    |
|                                                                         |
|                                                  [ Aceptar ] [ Cancelar]|
+-------------------------------------------------------------------------+
```

### Captura 15: Formulario de Conexión BD en Instalador de WordPress
```text
+-------------------------------------------------------------------------+
| WordPress > Configuración de la base de datos                           |
+-------------------------------------------------------------------------+
| A continuación debes introducir los detalles de conexión a la BD:       |
|                                                                         |
| Nombre de la base de datos: [ waedb                ]                    |
| Nombre de usuario:          [ wae_user             ]                    |
| Contraseña:                 [ Admin123             ]                    |
| Servidor de la base de datos:[ 192.168.10.4        ]                    |
| Prefijo de tabla:           [ wp_                  ]                    |
|                                                                         |
|                                                     [   Enviar   ]      |
+-------------------------------------------------------------------------+
```

### Captura 16: Pantalla Final de Instalación Correcta de WordPress
```text
+-------------------------------------------------------------------------+
| ¡Logrado!                                                               |
+-------------------------------------------------------------------------+
|                                                                         |
| WordPress ya está instalado. ¡Gracias y que disfrutes!                  |
|                                                                         |
| Nombre de usuario:  admin_wae                                           |
| Contraseña:         Your chosen password                                |
|                                                                         |
|                                                     [ Acceder    ]      |
+-------------------------------------------------------------------------+
```

### Captura 17: Comprobación de Requisitos de Entorno PHP en GLPI 10
```text
+-------------------------------------------------------------------------+
| GLPI 10.0.17 > Paso 0: Comprobación del entorno                         |
+-------------------------------------------------------------------------+
| Comprobación de la compatibilidad de su entorno con GLPI:               |
|                                                                         |
|  [OK] Versión de PHP >= 8.2 (Instalada: 8.5.0)                          |
|  [OK] Extensión mysqli cargada                                          |
|  [OK] Extensión mbstring cargada                                        |
|  [OK] Extensión curl cargada                                            |
|  [OK] Permisos de escritura en 'C:\inetpub\wwwroot\glpi\files'          |
|  [OK] Permisos de escritura en 'C:\inetpub\wwwroot\glpi\config'         |
|                                                                         |
|                                                     [ Continuar  ]      |
+-------------------------------------------------------------------------+
```

### Captura 18: Formulario de Conexión y Selección de BD en GLPI 10
```text
+-------------------------------------------------------------------------+
| GLPI 10.0.17 > Paso 1: Conexión a la base de datos                      |
+-------------------------------------------------------------------------+
| Servidor SQL:               [ 192.168.10.4        ]                    |
| Usuario SQL:                [ wae_user             ]                    |
| Contraseña SQL:             [ Admin123             ]                    |
|                                                                         |
|                                                     [ Continuar  ]      |
+-------------------------------------------------------------------------+
| Seleccione una base de datos:                                           |
| (X) Usar base de datos existente: [ glpidb                 v ]          |
|                                                                         |
|                                                     [ Continuar  ]      |
+-------------------------------------------------------------------------+
```

### Captura 19: Instalación de GLPI Finalizada y Credenciales por Defecto
```text
+-------------------------------------------------------------------------+
| GLPI 10.0.17 > La instalación ha terminado                              |
+-------------------------------------------------------------------------+
| La instalación de la base de datos ha finalizado correctamente.         |
|                                                                         |
| Cuentas por defecto creadas:                                            |
|  - glpi/glpi (administrador)                                            |
|  - tech/tech (técnico)                                                  |
|  - normal/normal (usuario normal)                                       |
|  - post-only/postonly (solo post)                                       |
|                                                                         |
|                                                     [ Usar GLPI  ]      |
+-------------------------------------------------------------------------+
```

### Captura 20: Acceso Correcto al Panel Principal de GLPI 10
```text
+-------------------------------------------------------------------------+
| GLPI 10.0.17 - Inicio (glpi / Super-Admin)                              |
+-------------------------------------------------------------------------+
| Dashboard | Incidencias | Inventario | Administración | Configuración   |
| ----------------------------------------------------------------------- |
|                                                                         |
|  [+] Crear una incidencia                                               |
|  [!] Alerta de seguridad: Cambie la contraseña por defecto de 'glpi'.  |
|  [!] Alerta de seguridad: Elimine el archivo 'install/install.php'.     |
|                                                                         |
|  Resumen del sistema: Base de datos remota 'glpidb' en 192.168.10.4    |
|                                                                         |
+-------------------------------------------------------------------------+
```

---

## Resolución de Problemas Frecuentes (Troubleshooting)

1. **Error de Conexión a Base de Datos en WordPress/GLPI (`Access denied` / `Connection refused`):**
   - Verificar que `bind-address = 0.0.0.0` en `/etc/mysql/mariadb.conf.d/50-server.cnf`.
   - Asegurarse de que el usuario tiene permisos remotos (`'wae_user'@'%'`).
   - Comprobar que UFW o el Firewall de Ubuntu no bloqueen el puerto 3306 (`sudo ufw allow 3306/tcp`).

2. **HTTP 500.0 - Internal Server Error (PHP FastCGI):**
   - Verificar la instalación de los componentes C++ Redistributable requeridos por la versión de PHP.
   - Revisar que la ruta a `php-cgi.exe` en los Mapeos de controlador IIS sea exacta.

3. **Error 403 / Pantalla en blanco en GLPI 10:**
   - Asegurar que la raíz de la Aplicación apunta al directorio `/public` de GLPI.
   - Comprobar que `index.php` está configurado en los **Documentos predeterminados**.