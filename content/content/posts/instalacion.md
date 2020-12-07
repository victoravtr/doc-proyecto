---
title: "Instalacion"
date: 2020-12-02T12:39:37+01:00
draft: false
subtitle: "Guia de instalacion"
author: "victorav"
authorLink: "https://victorav.me"
description: "Guia de instalacion"
tags: ["guia", "instalacion", "documentacion"]
categories: ["documentacion"]
twemoji: true
linkToMarkdown: true
---

Guía de instalación paso a paso del programa
<!--more-->
## Requisitos iniciales

- Servidor Debian/Ubuntu en el que se realizará la instalación _(En este ejemplo se usará Debian GNU/Linux 10 buster)_
- Usuario capaz de ejecutar comandos con sudo.
- Conexión a Internet
- Git _(Si se quiere descargar el programa directamente desde los repositorios)_
___
## Pasos para realizar la instalación

### 1. Descarga
Lo primero que tenemos que hacer es descargar en el servidor los archivos del programa con el método que veamos más conveniente: scp, ftp, git...
Por ejemplo, si quisiesemos descargarlo con git tendríamos que ejecutar el siguiente comando:
```bash
git clone https://github.com/victoravtr/Proyecto.git
```
![git clone](/images/instalacion/git_clone.png)

Además, también tenemos que descargar el repositorio que contiene la webshell:
```bash
git clone https://github.com/billchurch/webssh2.git
```
```bash
cp -r webssh2/ /var/www/
```
___
### 2. MYSQL
Aunque el programa ofrece la opción de realizar una instalación de forma automática, la instalación de mysql debe realizarla el propio usuario.
Se haría de la siguiente manera:
1. Primero, instalamos maria-db con apt
```bash
sudo apt install mariadb-server
```
2. Cuando la instalación haya completado, tenemos que configurar el servidor:
```bash
sudo mysql_secure_installation
```
Este comando hará que aparezca un menú en el que tendremos que configurar:
- Contraseña de root: la que queramos.
- Eliminar los usuarios anónimos: sí.
- Desactivar el acceso remoto como root: no.
- Eliminar bases de datos de pruebsa: sí.
- Recargar la tabla de privilegios: sí.

![mysql_secure_installation](/images/instalacion/mysql_secure_installation.png)

Una vez hayamos terminado la configuración, tenemos que crear el usuario y la base de datos que va a usar el usuario.
Para ello tenemos que hacer lo siguiente:
1. Nos logeamos el mysql con el comando _sudo mysql -u root -p_
```bash
sudo mysql -u root -p
```
2. Creamos el usuario _proyecto_ con el comando _CREATE USER 'proyecto'@'%' IDENTIFIED BY 'contraseña';_;
```bash
CREATE USER 'proyecto'@'%' IDENTIFIED BY 'contraseña';
```
3. Creamos la base de datos _proyecto_db_ con el comando _CREATE DATABASE proyecto_db;_
```bash
CREATE DATABASE proyecto_db;
```
4. Le damos todos los privilegios al usuario que hemos creado en la base de datos que acabamos de crear con el comando _GRANT ALL PRIVILEGES ON proyecto_db.* TO 'proyecto'@'%';_
```bash
GRANT ALL PRIVILEGES ON proyecto_db.* TO 'proyecto'@'%';
```
5. Ejecutamos el comando _FLUSH PRIVILEGES;_ para que se apliquen los cambios.
```bash
FLUSH PRIVILEGES;
```
![configuracion mysql](/images/instalacion/mysql_configure.png)
___
### 3. Instalación

Tenemos 2 opciones para instalar el programa: la forma manual y la forma automática.

___
#### 3.1 Instalación automática(Recomendado)
Una vez hayamos descargado el programa solo tenemos que entrar en la carpeta "Proyecto" y ejecutar el script install.sh como sudo.
```bash
cd Proyecto
```
```bash
sudo ./install
```
Aceptaríamos todo lo que nos va preguntando y, de esta forma, se instalará el programa de forma automática, a excepción de mysql que requiere la configuración manual previamente hecha.
___
#### 3.2 Instalación manual(No recomendado)

Para realizar la instalación manual tenemos que instalar todos los componentes que necesita el programa, crear la estructura de carpetas que se va a necesitar y configurar los distintos archivos que necesita el programa:

Se necesita:

##### ssh - [openssh.com](https://www.openssh.com/)
Entorno de ejecución para JavaScript que gestiona el funcionamiento de la webshell.
```bash
sudo apt install openssh-server
```
##### nodejs - [nodejs.org](https://nodejs.org/es/)
Entorno de ejecución para JavaScript que gestiona el funcionamiento de la webshell.
```bash
sudo apt install nodejs
```
##### npm - [npmjs.com](https://www.npmjs.com/)
Gestor de paquetes de nodejs.
```bash
sudo apt install npm
```
##### forever - [github.com/foreversd](https://github.com/foreversd/forever#readme)
Libería de nodejs para gestionar la ejecución de aplicaciones. 
```bash
sudo npm install forever -g
```
##### sendmail - [proofpoint.com](https://www.proofpoint.com/us/products/email-protection/open-source-email-solution)
Permite ejecutar y configurar de forma sencilla el envio de correos desde php.
```bash
sudo apt install sendmail
```
##### python3 - [python.org](https://www.python.org/)
Necesario para ejecutar los scripts de python.
```bash
sudo apt install python3
```
##### python3-pip - [pip.pypa.io](https://pip.pypa.io/en/stable/)
Sirve para instalar liberías de python de forma sencilla, pywinrm en nuestro caso.
```bash
apt install python3-pip
```
##### pywinrm - [github.com/diyan/pywinrm](https://github.com/diyan/pywinrm)
Libería que permite la ejecución de comandos entre sistemas linux y windows por medio de WinRM.
```bash
pip3 install pywinrm
```
##### sshpass - [sourceforge.net/projects/sshpass](https://sourceforge.net/projects/sshpass/)
Permite indicarle a ssh una contraseña por en el propio comando de conexion, en nuestro caso la usamos para establecer la conexión con otros sistemas.
```bash
apt install sshpass
```
##### apache2 - [apache.org](https://httpd.apache.org/)
Es el servidor web HTTP que usará el programa.
```bash
apt install apache2
```
##### php - [php.net](https://www.php.net/manual/es/intro-whatis.php)
Lenguaje en el que está programado el backend del programa, su lógica.
```bash
apt install php
```
```bash
apt install php-mysqli
```
##### mariadb-server - [mariadb.org](https://mariadb.org/)
El sistema de gestión de bases de datos que usaremos.

___

Además, necesitas crear el sistema de carpetas que el programa va a utilizar y colocar en ellas los archivos requeridos:

- /etc/proyecto

- /etc/proyecto/apache/
  - Dentro del directorio se debe colocar el archivo _proyecto.conf_

- /etc/proyecto/general
  - Dentro del directorio se debe colocar el archivo _exe_switch_

- /etc/proyecto/mysql/
  - Dentro del directorio se deben colocar los archivos _db_config.conf_ y _init_db.sql_

- /etc/proyecto/zabbix/
  - Dentro del directorio se deben colocar los archivos _zabbix_agentd.conf_, _zabbix_agentd32.exe_ y _zabbix_agentd64.exe_

- /var/www/proyecto
    - Dentro del directorio se deben colocar todos los archivos que están en el interior de la carpeta _Web-Proyecto-Content_
___

Por último, debes editar una serie de archivos para asegurar el correcto funcionamiento del programa:

Apache:
- Copiar archivo _/etc/proyecto/apache/proyecto.conf_ en _/etc/apache2/sites-available/proyecto.conf_
- Situarnos en el directorio _/etc/apache2/sites-available/_ y ejecutar los siguientes comandos:
```bash
sudo a2dissite 000-default.conf
```
```bash
sudo a2enssite proyecto.conf
```
```bash
sudo service apache2 restart
```
![configuracion apache](/images/instalacion/apache_conf.png)

Hosts:
- Obtenemos nuestra IP con el comando _hostname -I_
- La añadimos al archivo /etc/hosts
```bash
hostname -I
```
```bash
echo "IP  proyecto.local" >> /etc/hosts
```
![/etc/hosts](/images/instalacion/hosts.png)

PHP:
- Abrimos el archivo /etc/php/7.3/apache2/php.ini con el editor de texto que prefiramos y editamos las siguientes variables: 
- upload_max_filesize: aumentamos su valor a 2048M, por ejemplo.
- post_max_size: aumentamosn su valor a 2100M, por ejemplo.
- extension=mysqli: eliminamos la _","_ del principio de línea
![php.ini](/images/instalacion/php.png)

MYSQL:
- Editamos el archivo /etc/proyecto/mysql/db_config.conf con el editor que prefiramos para incluir la configuración que realizamos antes en mysql
![mysql config](/images/instalacion/mysql_file.png)

WEBSHELL:
- Por último, habría que instalar la webshell y ponerla en marcha con forever
```bash
sudo npm install /var/www/webssh2/app/ 
```
```bash
sudo forever start /var/www/webssh2/app/index.js
```
![webssh2 install](/images/instalacion/forever.png)
___
Con todo esto, la aplicación ya estaría operativa.
___

## Configuración clientes

Para poder utilizar el programa es necesario que los clientes objetivos tengan habilitado una forma de comunicación.
Para operar con los clientes Linux debe de estar habilitado un servidor ssh.
Para operar con los clientes Windows se presentan 2 casos:
- Windows workstation: podemos elegir entre SSH y WinRM
- Windows Server: deben de tener habilitado un servidor ssh.

___ 
### Configuración clientes Windows
___
#### Configuración ssh
___
##### Workstation

###### Windows 10
Para instalar un servidor ssh en Windows 10 tenemos que ir a _"Aplicaciones"_, dentro del panel de configuración.

![panel de configuración](/images/instalacion/config_windows.png)

Características opcionales...

![características opcionales](/images/instalacion/app_carac.png)

Agregar una característica...

![agregar característica](/images/instalacion/add_carac.png)

Instalamos el servidor OpenSSH...

![instalar OpenSSH](/images/instalacion/windows_10_openssh.png)

Ahora solo tendríamos que configurar el servicio:
- Abrimos una terminal de powershell como administrador y ejecutamos el siguiente comando para cambiar la shell predeterminada:
```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```
![cambiar shell](/images/instalacion/cambiar_shell.png)

- Editamos el archivo _C:\ProgramData\ssh\shhd_config_, incluimos el nombre del usuario local con el que nos conectaremos y le indicamos el que use el puerto 22.
![cambiar shell](/images/instalacion/windows_ssh_config.png)

Puede que necesites configurar el firewall para permitir las conexiones por el puerto 22. Para hacerlo, debes ejecuar otro comando de powershell:
```powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH SSH Server' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22 -Program "C:\System32\OpenSSH\sshd.exe"
```
###### Windows 8/7

La instalación de OpenSSH en Windows 7 y Windows 8 es la misma:
- Descargamos el progama desde la siguiente URL y lo descomprimimos: [github.com/PowerShell/Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/releases)
- Abrimos una terminal en carpeta que acabamos de descomprimir y ejecutamos el siguiente comando:
```powershell
powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps1
```
- Por último, editamos el archivo de configuración y añadimos la regla en el firewall como hicimos con Windows 10.
___
##### Server

###### Windows Server 2019
La instalación de OpenSSH en Windows Server 2019 es la misma que haríamos en un Windows 10.

###### Windows Server 2016/2012
La instalación de OpenSSH en Windows Server 2016/2012 es la misma que haríamos en un Windows 7.

#### Configuración WinRM

##### Workstation

###### Windows 10/8/7
La instalación y configuración de WinRM es la misma para los 3 sistemas:
- Abrimos una terminal de cmd como administradores.
- Ejecutamos el siguiente comando para habilitar el servicio:
```powershell
winrm quickconfig
```
![configuracion winrm](/images/instalacion/winrm_1.png)

- Ejecutamos los siguientes comandos para configurar el servicio:
```powershell
winrm set winrm/config/service/auth @{Basic="true"}
```
```powershell
winrm set winrm/config/service @{AllowUnencrypted="true"}
```
```powershell
winrm set winrm/config/client/auth @{Basic="true"}
```
```powershell
winrm set winrm/config/client @{AllowUnencrypted="true"}
```
- Con los siguientes comandos podemos comprobar la configuración, debería de quedar de la siguiente manera:
```powershell
winrm get winrm/config/service
```
```powershell
winrm get winrm/config/client
```
![Configuracion winrm](/images/instalacion/winrm_config.png)

### Configuración clientes Linux

#### Configuración ssh

##### Debian/Ubuntu

La instalación del servidor ssh es la misma en Debian que en Ubuntu:
- Lo instalamos con el siguiente comando:
```bash
sudo apt install openssh-server
```

* Si queremos conectarnos con el usuario root tenemos que editar el archivo _/etc/ssh/sshd_config_ y cambiar la línea _"#PermitRootLogin prohibit-password
"_ por _"PermitRootLogin yes"_