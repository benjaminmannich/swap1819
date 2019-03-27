# Práctica 1 - Preparación de las herramientas
Tras crear las máquinas virtuales necesarias (Ubuntu Server 1 y Ubuntu Server 2) e instalar los programas necesarios (Apache, cURL, SSH, PHP, MySQL) en cada una, nos aseguramos que el servidor está en ejecución:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%201/Screenshots/ApacheWorkingServer1.png)

Y creamos una mini página web de prueba *hola.html* en el directorio */var/www/html*. La página creada es:

<html>
<body>
Esto funciona! :)
Pagina almacenada en Server <num server>
</body>
</html>


Ahora, en VirtualBox, permitimos la conexión entre ambas máquinas. En mi caso he hecho esto creando una red NAT y conectando ambas maquinas a ella. 
Tras esto averiguamos la IP de cada maquina y comprobamos que existe conexión entre ellas (ping):
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%201/Screenshots/Server1Ping.png)
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%201/Screenshots/Server2Ping.png)

y accedemos mediante SSH de una a la otra:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%201/Screenshots/SSHServer1to2.png)
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%201/Screenshots/SSHServer2to1.png)

Una vez que hemos conseguido esto cargamos la web de prueba, que creamos anteriormente, mediante curl. Primero cargaremos en el server 2 la pagina del server 1, y luego la del server 1 en el server 2.

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%201/Screenshots/CurlServer1.png)
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%201/Screenshots/CurlServer2.png)
