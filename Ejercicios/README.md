# Ejercicios

## Tema 2
**T2.1. Calcular la disponibilidad del sistema si tenemos dos réplicas de cada elemento (en total 3 elementos en cada subsistema).**

web3 = 97'75 + (1 - 97'75)*85 = 99'6625 ap = 99 + (1 - 99)*90 = 99'9 BD = 99.9999 + (1 - 99'9999)*99'9 = 99'99999 DNS = 99.96 + (1 - 99'96)*98 = 99'9992 Firewall = 97'75 + (1 - 97'75)*85 = 99'6625 switch = 99.99 + (1 - 99'99)*99 = 99'9999 DC = 99.99 + (1 - 99'99)*99'99 = 99'9999 ISP = 99'75 + (1 - 99'75)*95 = 99'9875

Disponibilidad=0,996625×0,999×0,999999×0,999992×0,996625×0,999999×0,9999×0,999875=99,2034961%

**T2.2. Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad.**
* IBM PowerHA
* YII
* Symfony
* Apache Hadoop

**T2.3. ¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor?
Buscar herramientas y aprender a usarlas.**
Para monitorización de nuestro servidor:
* Nagios
* Munin
* Zabbix
* top
* ps
* uptime

**T2.4. Buscar ejemplos de balanceadores software y hardware (productos comerciales).**
Software: HaProxy, Nginx, Kemp, Pound, Pen
Hardware: CISCO, Load Balancer ADC, Load Master

**Buscar productos comerciales para servidores de aplicaciones.**
WebLogic (Oracle), WebSphere (IBM), GlassFish, Java EE, Sun-Netscape IPlanet, HP Bluestone.

**Buscar productos comerciales para servidores de almacenamiento.**
Amazon EC2, Dell KACE, IBM EXP2500 Storage Enclosure, Windows Azure, Lenovo ThinkServer

## Tema 3
**T3.1. Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.**
En Linux el comando para pasar el enrutamiendo del tráfico de una subred a otra es *route*.
Con route podemos añadir (add) y eliminar (del) rutas.

En Windows, en administrador del servidor, seleccionamos servicio de enrutamiento y acceso remoto.

**T3.2. Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes.**

En Linux usamos iptables para dejar pasar o bloquear paquetes mediante una serie de reglas que definimos.

En Windows usamos su Firewall para el filtrado y bloqueo simple de paquetes.



