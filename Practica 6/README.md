# Práctica 6. Servidor de disco NFS
En esta práctica configuraremos la maquina balanceador como servidor de disco NFS y las dos máquinas servidores como clientes.

## Configurar servidor de disco NFS y exportar una carpeta a los clientes.

Instalamos las herramientas necesarias en el servidor:

```bash
sudo apt-get install nfs-kernel-server nfs-common rpcbind
```
Luego creamos la carpeta a compartir y le damos los permisos necesarios:

```bash
mkdir /dat/compartida
sudo chown nobody:nogroup /dat/compartida/ 
sudo chmod -R 777 /dat/compartida/
```

Por último añadimos la siguiente línea al archivo de configuracion "/etc/exports" y reiniciamos el servicio:
```bash
/dat/compartida/ 10.0.2.8(rw) 10.0.2.9(rw)
sudo service nfs-kernel-server restart
```

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%206/Images/1.png)

## Configurar clientes.

En ambas máquinas cliente instalamos los paquetes necesarios y creamos los puntos de montaje:
```bash
sudo apt-get install nfs-common rpcbind 
cd /home/benjamin
mkdir carpeta_cliente
chmod -R 777 carpeta_cliente
```
Por último montamos la carpeta remota sobre el directorio creado:
```bash
sudo mount 10.0.2.11:/dat/compartida carpeta_cliente
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%206/Images/2.png)

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%206/Images/3.png)


## Comprobar acceso.

En cada uno de los clientes y en el servidor crearemos un archivo y leeremos los archivos para comprobar que el acceso funciona correctamente. 
En cada máquina ejecutamos los siguientes comandos:
```bash
ls –la <carpetacompartida>
touch <carpetacompartida>/archivo<id_maquina>.txt
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%206/Images/4.png)
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%206/Images/5.png)
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%206/Images/6.png)


## Hacer permanente la configuración
Para hacer permanente la configuración en los clientes, debemos añadir una línea al archivo de configuración "/etc/fstab" para que la carpeta compartida se monte al arrancar el sistema. La línea a añadir es la siguiente:
```bash
10.0.2.11:/dat/compartida /home/benjamin/carpeta_cliente/ nfs auto,noatime,nolock,bg,nfsvers=3,intr,tcp,actimeo=1800 0 0
```

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%206/Images/7.png)
 
