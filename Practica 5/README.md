# Práctica 5. Replicación de bases de datos MySQL
## Crear una BD


Primero entramos en la interfaz de mysql:
```bash
mysql -uroot -p
```
A continuación creamos la base de datos "tienda" con una tabla "productos", compuesta por "nombre" y "precio".
```bash
mysql> create database tienda;
mysql> use tienda;
mysql> create table productos(nombre varchar(100),precio DECIMAL(6,2) NOT NULL);
```

Mostramos la base de datos:
```bash
mysql> show tables;
+---------------------+
| Tables_in_tienda |
+---------------------+
| productos |
+---------------------+
1 row in set (0,00 sec)
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/1.png)

Por último introducimos datos en la tabla:

```bash
mysql> insert into productos(nombre,precio) values ("Manzana",1.5);
mysql> insert into productos(nombre,precio) values ("Chocolate",2.99);
mysql> insert into productos(nombre,precio) values ("Leche",1.3);
```
Y la mostramos el contenido de la tabla:
```bash
mysql> select * from productos;
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/2.png)

## Copia de seguridad de la BD con mysqldump
Antes de realizar la copia de seguridad bloqueamos las tablas para que la base de datos no se actualice mientras hacemos la copia.

```bash
mysql -u root –p
mysql> FLUSH TABLES WITH READ LOCK;
mysql> quit
```

Con mysqldump guardamos los datos. En la máquina 1 ejecutamos las siguientes órdenes:

```bash
mysqldump tienda -u root -p > /tmp/tienda.sql
```

Y ya podemos desbloquear las tablas:

```bash
mysql -u root –p
mysql> UNLOCK TABLES;
mysql> quit
```

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/3.png)

Ahora compiamos a la máquina 2 el archivo .sql de la máquina 1 en el que se encuentra la copia. En la máquina 2 ejecutamos:

```bash
scp 10.0.2.9:/tmp/tienda.sql /tmp/
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/4.png)

## Restaurar copia de seguridad

Ahora recuperaremos la base de datos completa desde la copia de seguridad. Ejecutamos las siguientes órdenes en la máquina 2 para crear una base de datos vacía y luego rellenarla a partir de la copia:

```bash
mysql -u root –p
mysql> CREATE DATABASE tienda;
mysql> quit

mysql -u root -p ejemplodb < /tmp/ejemplodb.sql
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/5.png)

Por úlitmo mostramos la tabla de la base de datos para confirmar que se ha copiado correctamente:
```bash
use tienda
select * from productos;
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/6.png)

## Configuración maestro esclavo

Modificamos la configuración para el maestro en el archivo "/etc/mysql/mysql.conf.d/mysqld.cnf" de la máquina 1.

Comentamos el parámetro bind-address para evitar que la maquina escuche peticiones, indicamos el archivo para el log de errores, establecemos el identificador del servidor y establecemos donde guardar el log binario.

```bash
#bind-address 127.0.0.1
log_error = /var/log/mysql/error.log
server-id = 1
log_bin = /var/log/mysql/bin.log
```

Guardamos el documento y reiniciamos el servicio:

```bash
/etc/init.d/mysql restart
```

Modificamos la configuración para el esclavo en el archivo "/etc/mysql/mysql.conf.d/mysqld.cnf" de la máquina 2.

La configuración se realizara igual que para el maestro, únicamente le damos a server-id el valor 2:
```bash
server-id = 2
```

Guardamos el documento y reiniciamos el servicio:

```bash
/etc/init.d/mysql restart
```
A continuación creamos un usuario y le damos permisos de acceso para la replicación. Dentro de mySQL en el maestro ejecutamos lo siguiente:

```bash
mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%'
IDENTIFIED BY 'esclavo';
mysql> FLUSH PRIVILEGES;
mysql> FLUSH TABLES;
mysql> FLUSH TABLES WITH READ LOCK;
```

Con la orden "mysql> SHOW MASTER STATUS;" obtendremos los datos necesarios para configurar el esclavo.

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/7.png)

En mysql de la máquina esclava introducimos los datos del maestro obtenidos anteriormente:

```bash
mysql> CHANGE MASTER TO MASTER_HOST='192.168.31.200',
MASTER_USER='esclavo', MASTER_PASSWORD='esclavo',
MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=501,
MASTER_PORT=3306;
```

Por último iniciamos el esclavo:
```bash
mysql> START SLAVE;
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/8.png)

Desbloqueamos las tablas en el maestro:

```bash
mysql> UNLOCK TABLES;
```

Y comprovamos que el esclavo esta funcionando
```bash
mysql> SHOW SLAVE STATUS\G
```
La variable "Seconds_Behind_Master" debe estar a 0.

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/9.png)

Y vemos como cuando actualizamos una tabla tambien se actualiza en la otra máquina:
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/10.png)
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%205/Images/11.png)
