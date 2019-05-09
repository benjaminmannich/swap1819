# Práctica 4. Asegurar la granja web
## Instalar certificado SSL autofirmado
En esta captura vemos que no es posible el acceso con https, por lo tanto procederemos a activarlo.

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/WithoutSSLcertificate.png)

Para instalar un certificado SSL autofirmado en la máquina 1, activamos el módulo SSL de Apache, generamos los certificados y especificamos la ruta de estos:

```bash
sudo a2enmod ssl
sudo service apache2 restart
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
```

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/CreateSSLCertificate.png)

Editamos el archivo "/etc/apache2/sites-available/default-ssl.conf" y añadimos las líneas:
```bash
SSLCertificateFile /etc/apache2/ssl/apache.crt 
SSLCertificateKeyFile /etc/apache2/ssl/apache.key
```
debajo de "SSLEngine on".

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/AddSSLFiles.png)

Activamos el sitio default-ssl y reiniciamos apache:
```bash
a2ensite default-ssl service 
apache2 reload
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/ActivateSSL.png)

Y observamos como ahora si es posible el acceso con https:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/WithSSLCertificate.png)

## Configurar cortafuegos con IPTABLE
En primer lugar creearemos el script que se ejecutara a lanzar la máquina para activar el cortafuegos:
```bash
touch cortafuegos.sh
```
Y rellenaremos el archivo tal y como se muestra en la captura de pantalla:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/CortafuegosScript.png)

A continuación para que se ejecute el script con el resto de servicios del sistema al lanzar la máquina moveremos el script a la carpeta "/etc/init.d/", le damos permisos de ejecución y actualizamos el rc.d con configuración por defecto:

```bash
mv ~/cortafuegos.sh /etc/init.d/
chmod +x /etc/init.d/cortafuegos.sh
update-rc.d cortafuegos.sh defaults
```
Reiniciamos la maquina y ejecutamos: 
```bash
iptables -L -n -v
```
Obsevamos que ahora estan activadas todas las reglas que definimos en el script:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/IPTABLESFuncionando.png)

Y vemos que si hacemos una petición http o https al servidor si la acepta pero cualquier otro tipo de peticion no (por ejemplo una peticion ping):

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/PeticionHTTPSPING.png)

## OPCIONAL 1: Balancear tráfico HTTP y HTTPS

En primer lugar copiamos los archivos "/etc/apache2/ssl/apache.crt" y "/etc/apache2/ssl/apache.key" al otro servidor y al balanceador con la siguientes ordenes:

Servidor 2:
```bash
rsync -avz -e ssh 10.0.2.9:/etc/apache2/ssl/ /etc/apache2/ssl/
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/RsyncServer2.png)

Balanceador:
```bash
rsync -avz -e ssh 10.0.2.9:/etc/apache2/ssl/ /tmp/
```
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/RsyncBal.png)

Ahora en el segundo servidor activamos "default-ssl" (al igual que en el servidor 1) y reiniciamos apache.

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/ActivateSSLServer2.png)

En el balanceador nginx debemos añadir lo siguiente al archivo /etc/nginx/conf.d/default.conf y reiniciarlo:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/BalConf.png)

Y podemos observar como ahora podemos realizar peticiones HTTP y HTTPS al balanceador:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%204/Images/WorkingSSLBal.png)
