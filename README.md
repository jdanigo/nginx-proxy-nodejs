## Creando un servidor proxy nginx nodejs

## Paso 1) Acceder por la terminal SSH al servidor

    -> ssh -i /ruta/certificado.pem usuario@ipdelservidor


## Paso 2) actualizar dependencias 
	-> sudo apt update
## Paso 3) instalar nodejs desde el repositorio , en este caso instalamos la versión 12
	-> cd ~
	-> curl -sL https://deb.nodesource.com/setup_12.x -o nodesource_setup.sh
## Paso 4) Ejecutar el comando para su previa instalación 
	-> sudo apt install nodejs
## Paso 5) Instalar el paquete adicional , algunas aplicaciones node necesitan compilar
	-> sudo apt install build-essential
## Paso 6) Instalar NGINX 
	-> sudo apt install nginx
## Paso 7) Abrir los puertos en el firewall
	-> sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
	-> sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 443 -j ACCEPT
	-> sudo netfilter-persistent save##Paso 8) Verificar que el servidor esté activo 
 	-> systemctl status nginx
	-> systemctl enable nginx
## Paso 9) Instalar administrador de procesos nodejs
	-> sudo npm install forever -g
## Paso 10) Crear la configuración del servidor proxy 
    -> sudo nano /etc/nginx/sites-available/api.nombredemidominio.com
 
[example.conf](docs/CONTRIBUTING.md)

## Paso 11) Crear enlace simbólico  
    -> sudo ln -s /etc/nginx/sites-available/api.nombredemidominio.com /etc/nginx/sites-enabled/api.nombredemidominio.com

## Paso 12) Crear un certificado autofirmado Let's Encrypt
    -> sudo add-apt-repository ppa:certbot/certbot
    -> sudo apt update
    -> sudo apt install python-certbot-nginx
    -> sudo systemctl reload nginx
    -> sudo certbot --nginx -d api.nombredemidominio.com
*El siguiente mensaje te permitirá escoger entre redireccionar o no el tráfico de HTTP a HTTPS, limitando el acceso HTTP; mediante las siguientes opciones, después de haber seleccionado la opción requerida, presionar ENTER*
```
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```
*Una vez presionado enter la configuración será actualizada, y Nginx se recargará para activar los nuevos ajustes. certbot concluirá con un mensaje informando que el proceso fue exitoso y la localización de tus certificados*

```sh
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/api.nombredemidominio.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/api.nombredemidominio.com/privkey.pem
   Your cert will expire on 2018-07-23. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

Contribución
----

*José Daniel Garcés Ospina - Fullstack Developer - DevOps*
