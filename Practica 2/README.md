# Práctica 2. Clonar la información de un sitio web
## Copia de archivos por SSH
Para la copia de archivos mediante SSH simplemente seguimos los pasos que se indican en el guión de prácticas, los cuales también podemos observar en la siguiente captura de pantalla:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/sendFileSSH.png)

## Clonado de carpeta con rsync
Para clonar una carpeta de un servidor (**ubuntu1**) en el otro (**ubuntu2**) usaremos la herramienta *rsync*.
Primero damos permiso al usuario sobre la carpeta */var/ww* y luego en **ubuntu2** ejecutamos rsync tal y como se muestra en la captura de pantalla.

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/rsyncWorking.png)

Imprimimos el contenido del archivo */var/www/html/hola.html* antes y despues de ejecutar *rsync* para comprobar que efectivamente se ha copiado el contenido de **ubuntu1** a **ubuntu2**.

## SSH sin contraseña
Para acceder de **ubuntu2** a **ubuntu1** sin contraseña deberemos de crear un par de claves publica y privada en **ubuntu2**, tal y como se muestra en la caputra de pantalla:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/createPublicKey.png)

Y luego mandarle la clave publica a **ubuntu1**, tal y como se muestra en la captura de pantalla:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/sendPublicKey.png)

Comprobamos que la clave publica de **ubuntu2** se ha guardado en **ubuntu1** en el archivo *~/.ssh/authorized_keys*:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/publicKeyOnServer1.png)

Y ya podremos acceder desde **ubuntu2** a **ubuntu1** sin necesidad de contraseña:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/sshWithoutPasswd.png)

## Actualizar */var/www* cada hora con cron
Añadimos nuestra orden al archivo */etc/crontab*. La orden a añadir la podemos observar en la captura de pantalla:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/crontabContent.png)

Y ya observamos como a la hora indicada (todas las horas al minuto 0) se actualiza el la carpeta:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%202/Screenshots/crontabWorking.png)
