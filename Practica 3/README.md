# Práctica 3. Balanceo de carga
## Organización de red
La organización de red se ha realizado de la siguiente forma:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%203/Images/ConfigRedes.jpg)

## nginx como balanceador de carga
Una vez creada la maquina virtual instalamos nginx:
'''bash
  
'''
Añadimos nuestros servidores al fichero de configuración de nginx (/etc/nginx/conf.d/default.conf):

'''bash
upstream apaches {
    server 10.0.2.8;
    server 10.0.2.9;
}

server{
    listen 80;
    server_name balanceador;
    access_log /var/log/nginx/balanceador.access.log;
    error_log /var/log/nginx/balanceador.error.log;
    root /var/www/html;
    location /
    {
        proxy_pass http://apaches;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
    }
}
'''
Iniciamos nginx:
'''bash
sudo systemctl start nginx
'''

Comprobamos que el balanceo de carga funciona correctamente:
'''bash
curl http://127.0.0.1
curl http://127.0.0.1
'''
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%203/Images/balanceo_nginx.png)

## haproxy como balanceador de carga
Una vez creada la maquina virtual instalamos haproxy:
'''bash
sudo apt-get install haproxy
'''
Añadimos nuestros servidores al fichero de configuración de haproxy (/etc/haproxy/haproxy.cfg):

'''bash
global
    daemon
    maxconn 256
defaults
    mode http
    contimeout 4000
    clitimeout 42000
    srvtimeout 43000
frontend http-in
    bind *:80
    default_backend servers
backend servers
    server m1 ip_maquina1:80 maxconn 32
    server m2 ip_maquina2:80 maxconn 32
'''
Iniciamos haproxy:
'''bash
sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
'''

Comprobamos que el balanceo de carga funciona correctamente:
'''bash
curl http://127.0.0.1
curl http://127.0.0.1
'''
![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%203/Images/balanceo_haproxy.png)

## Comparación con alta carga de nginx y haproxy
Aplicamos una carga simulada, mediante Apache benchmark, a nuestra granja web. 
'''bash
ab -n 1000 -c 10 http://IP/index.html
'''
En primer lugar estaremos corriendo nginx:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%203/Images/nginx_carga.png)

Y en segundo lugar estaremos corriendo haproxy:

![imagen](https://github.com/benjaminmannich/swap1819/blob/master/Practica%203/Images/haproxy_carga.png)

