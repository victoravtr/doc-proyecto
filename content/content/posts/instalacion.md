---
title: "Instalacion"
date: 2020-11-02T12:39:37+01:00
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

# GUÍA DE INSTALACIÓN

## Requisitos iniciales

- Servidor Debian/Ubuntu en el que se realizará la instalación _(En este ejemplo se usará Debian GNU/Linux 10 buster)_
- Usuario capaz de ejecutar comandos con sudo.
- Conexión a Internet
- Git _(Si se quiere descargar el programa directamente desde los repositorios)_

## Pasos para realizar la instalación

## 1. Descarga
Lo primero que tenemos que hacer es descargar en el servidor los archivos del programa con el método que veamos más conveniente: scp, ftp, git...
Por ejemplo, si quisiesemos descargarlo con git tendríamos que ejecutar el siguiente comando:
```bash
git clone https://github.com/victoravtr/Proyecto.git
```
![git clone](/images/instalacion/git_clone.png)

## 2. MYSQL
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

## 3. Instalación

Tenemos 2 opciones para instalar el programa: la forma manual y la forma automática.

### 3.1 Instalación manual(No recomendado)

Para realizar la instalación manual tenemos que instalar todos los componentes que necesita el programa, crear la estructura de carpetas que se va a necesitar y configurar los distintos archivos que necesita el programa:
#### 3.1.1 Instalación de componentes

Se necesita:

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
sudo npm install forever
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

#### 3.1.2 Estructura de carpetas
Carpetas que debemos crear y archivos que tenemos que copiar en ellas:

#### 3.1.3 Archivos de configuración

### 3.2 Instalación automática(Recomendado)
Una vez hayamos descargado el programa solo tenemos que entrar en la carpeta "Proyecto" y ejecutar el script install.sh como sudo.
```bash
cd Proyecto
```
```bash
sudo ./install
```

De esta forma se instalará el programa de forma automática, a excepción de mysql que requiere la siguiente configuración manual: