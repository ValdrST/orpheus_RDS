# Orpheus

## Instrucciones de instalacion
 * Instalar mrtg
 ```bash
$ pacman -S mrtg
 ```
 * Crear el usuario mrtg
 ```
 useradd -d /srv/http/mrtg $ mrtg
 ```
 * Craer el directorio home del usuario y darle permisos
 ```
 $ mkdir /srv/http/mrtg
 $ chown mrtg:mrtg /srv/http/mrtg
 ```
 * asignar el dominio y su configuracion a nginx
 ```
 $ cp /configs/mrtg.somch.org /etc/nginx/available-site/
 $ ln -s /etc/nginx/available-sites/mrtg.somch.org /etc/nginx/enabled-sites/mrtg.somch.org
 $ systemctl reload nginx
 ```
* Crear el directorio que albergara los archivos PNG y el index
```
$ mkdir /srv/http/mrtg/html
```
* Crear el archivo de configuracion mrtg
```
$ cfgmaker --output=/srv/http/mrtg/mrtg.cfg --ifref=name --ifref=descr --global "WorkDir: /srv/http/mrtg" igg@132.247.103.251
```
* Añaidr los parametros extra al archivo mrtg.cfg generado
```
### Global configuration  ###

LoadMIBs: /usr/share/snmp/mibs/UCD-SNMP-MIB.txt
EnableIPv6: no
HtmlDir: /srv/http/mrtg/html
ImageDir: /srv/http/mrtg/html
LogDir: /srv/http/mrtg
ThreshDir: /srv/http/mrtg
RunAsDaemon: Yes
Interval: 5
Refresh: 60
```
1) Corresponde a la base de datos gestionada MIB que contiene los parametros de los dispositivos compatibles
2) Deshabilita el IPv6
3) La ruta de los archivos html
4) La ruta de las imagnes PNG
5) La ruta de los archivos log
6) El folder thresh
7) Correr como demonio # este es inidispensable para crear el servicio mas adelante
8) Intervalo en minutos del demonio, 5 minutos.
9) Intervalo de refresco de archivos html.

* Una vez creado el archivo de configuracion ir al directorio /srv/http/mrtg y crear el index
```
$ cd /srv/http/mrtg
$ indexmaker ./mrtg.cfg > index.html
```
* Crear el servicio que demonizara el servicio MRTG que obtendra los datos
```
$ cp configs/mrtg.service /usr/lib/systemd/system
$ systemctl enable mrtg
$ systemctl start mrtg
```
* Abrir el dominio en el navegador y observar los resultados