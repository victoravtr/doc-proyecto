---
title: "How-To"
date: 2020-12-02T12:39:37+01:00
draft: false
subtitle: "Guia de uso de la aplicacion"
author: "victorav"
authorLink: "https://victorav.me"
description: "Guia de uso de la aplicacion"
tags: ["guia", "uso", "documentacion"]
categories: ["documentacion"]
twemoji: true
linkToMarkdown: true
---

Guía de uso de la aplicación
<!--more-->

## Registro

Lo primero que el usuario debe de hacer, una vez haya instaldo la aplicación, es registrarse en ella para poder utilizarla al 100%.
Para hacerlo, se dirigiría a la url [proyecto.local/registro](http://proyecto.local/registro.php)

![Registro](/images/uso/registro.png)

Cuando completemos los campos y le demos al botón de _"Registrar usuario"_ nos saldrá un mensaje confirmando que el usuario ha sido creado:

![Registro](/images/uso/registrado.png)

Una vez registrados, podemos volver al [login](http://proyecto.local/login.php) e iniciar sesión.

Al hacerlo, nos llevará al _"index.php"_ donde se nos mostrarán un listado de las opciones disponibles y desde donde podremos elegir que hacer.

![Index](/images/uso/index.png)
___

## Index

Como se puede ver en el index, la aplicación permite al usuario realizar las siguientes operaciones:
- Configuración
- Cerrar sesión
- Añadir equipos a dominio
- Instalación software en equipos
- Añadir agente zabbix en equipo
- Abrir shell remota a equipo
- Añadir agente wazuh en equipo

## Configuración

Antes de operar con un equipo debemos comprobar que podemos conectarnos a él. Para hacer eso utilizamos este apartado en concreto, _config.php_.
Solo tendríamos que rellenar los campos del formulario y darle al botón. El programa se encarga de probar los tipos de conexión compatibles con el sistema y mostrar los resultados de la comprobación en la columna de _"Output"_.

![Config](/images/uso/config.png)

## Cerrar sesión

Desde aquí podemos cerrar la sesión que tenemos iniciada en el programa.

## Añadir equipos a dominio

Desde aquí podemos añadir un equipo a un dominio AD.
Rellenamos los datos del cliente, los datos del controlador de dominio y le damos al botón _"Añadir"_. El programa intentará unir el equipo al dominio y mostrará el output de las ordenes en la columna _"Output"_.

![Dominio](/images/uso/dominio.png)

## Instalación software en equipos

Desde aquí podemos instalar software en equipos.
Rellenamos los datos, le damos a Examinar para escoger el instalador y le damos al botón _"Añadir"_. El programa intentará ejecutar el instalador en el cliente y mostrará el resultado en la columna _"Output"_.

![Software](/images/uso/soft.png)
{{< style "strong{color: red;}" >}}
**!!! IMPORTANTE** 
{{< /style >}}
- El formato .exe no está totalmente estandarizado como el .msi, por lo que las ordenes para instalar un programa desde la consola de comandos no son siempre las mismas. El programa tiene una base de datos de programas con su comando de instalación concreto y los que no están en esa base de datos intentará instalarlos usando un comando por defecto, pero con algunos programas puede llegar a no funcionar.

##  Añadir agente zabbix en equipo

Desde aquí podemos instalar agentes de zabbix en equipos.
Rellenamos los campos del formulario y le damos al botón de _"Añadir"_. Es importante indicar bien la arquitectura del sistema en el caso de querer instalar un agente zabbix en Windows, ya que el instalador para el sistema de _64-bits_ no funciona en el de _32-bits_ y viceversa. El programa intentará instalar el agente en el cleinte y mostrará el resultado en la columna _"Output"_.

![Zabbix](/images/uso/zabbix.png)

## Abrir shell remota a equipo

Desde aquí podemos abrir una webshell en un equipo.
Este apartado difiere un poco del resto ya que, aunque tengamos que rellenar el formulario y enviarlo como en los demás, aquí el programa se encarga simplemente de redirigirnos a la _url_ de la _webshell_.

![Shell](/images/uso/shell.png)
![Shell](/images/uso/rshell.png)

## Añadir agente wazuh en equipo

Desde aquí podemos instalar un agente de wazu (ossec) en un equipo.
Rellenamos los campos del formulario y le damos al botón de _"Añadir"_. El programa intentará instalar el agente en el cleinte y mostrará el resultado en la columna _"Output"_.

![Wazuh](/images/uso/wazuh.png)
