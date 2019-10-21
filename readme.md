Manual de Instalación Cloudera
#### Arquitectura de cloudera
#### Que es un parcel:
Es una distribución de objeto que nos brinda cloudera para instalar por partes separadas cada paquete. (Cada parcel solo tiene un único objeto a instalar).
**Qué ventajas me da un parcel?**
- **Consistencia**
Todos los componentes están relacionados de tal forma que nunca estaremos instalando partes de diferentes versiones.
- **Flexibilidad**
No es necesario ser superusuario(root) para poder instalar cloudera ya que “cloudera-manager-agent” puede ejecutarse con otro usuario que no sea sudo.

 [![Build Status](https://i.ibb.co/VYSsk59/arquitecturacdh.png)]()


**Antes de Instalar**
- Configurar los nombres de red en cada nodo.
- Deshabilitar el Firewall
 [![Build Status](https://i.ibb.co/NVcr9WR/iptables.png)]()
- Agregar la siguiente configuración al archivo sysctl.conf:
```sh
xiafra@hadoop$ echo “vm.swappiness=1” >> /etc/sysctl.conf
```

- Configurar el modo SELinux
 [![Build Status](https://i.ibb.co/0BVf4sX/enforcing.png)]()

- Habilitar el servicio NTP
- Instalar Python 2.7 en los nodos Hue
- Configurar Transparent Huge Page
- Instalar en el nodo principal el paquete de utilería pssh.

### Configuración de un repositorio local

Cloudera Manager se instala utilizando paquetes como yum en sistemas RHEL. Estas herramientas dependen del acceso a los repositorios para la instalación del software. Cloudera permite tener acceso a sus repositorios en la nube para CDH y paquetes de Cloudera Manager. De igual manera podemos crear nuestro repositorio local para aquellos nodos que no tengan salida a Internet.

Antes que nada revisar la versión de SO 
```sh
xiafra@hadoop$ cat /etc/redhat-release
```

Instalar los siguientes paquetes necesarios:
```sh
xiafra@hadoop$ sudo yum -y install wget 
xiafra@hadoop$ sudo yum -y install createrepo 
xiafra@hadoop$ sudo yum -y install yum-utils
xiafra@hadoop$ sudo yum -y install unzip
xiafra@hadoop$ sudo yum -y install nano
xiafra@hadoop$ sudo yum -y install perl 
xiafra@hadoop$ sudo yum -y install ntp
xiafra@hadoop$ sudo yum -y install httpd
xiafra@hadoop$ sudo yum -y install git # Pueden descartarlo 
xiafra@hadoop$ sudo yum clean all
```

Una vez hecho esto necesitamos configurar el servidor apache httpd.
```sh
# Activamos el servicio httpd
xiafra@hadoop$ sudo systemctl enable httpd
xiafra@hadoop$ sudo systemctl start httpd
```

>>Es muy importante revisar que el server apache este ejecutándose.

 [![Build Status](https://i.ibb.co/zbN1Tqj/apache.png)]()

### Descargar el CM 5.13 y hacerlo disponible en nuestro repo “Local”
 [![Build Status](https://i.ibb.co/FxM5C6p/cms.png)]()


```sh
xiafra@hadoop$ wget http://archive.cloudera.com/cm5/repo-as-tarball/5.13.0/cm5.13.0-centos7.tar.gz
xiafra@hadoop$ wget http://archive.cloudera.com/cm5/repo-as-tarball/5.13.0/cm5.13.0-centos7.tar.gz.md5
xiafra@hadoop$ wget http://archive.cloudera.com/cm5/repo-as-tarball/5.13.0/cm5.13.0-centos7.tar.gz.sha1
```
**Descomprimir el cms y moverlo a “var/www/html/”**
```sh
xiafra@hadoop$ tar -xvf cm5.13.0-centos7.tar.gz -c /var/www/html/
xiafra@hadoop$ mv cm5.13.0-centos7.tar.gz.md5 /var/www/html/
xiafra@hadoop$ mv cm5.13.0-centos7.tar.gz.sha1 /var/www/html/

# Debemos tener los siguientes paquetes:
xiafra@hadoop$ cd /var/www/html
xiafra@hadoop$ ls -lrht
5    cdh-5.13.0    cloudera-cm.repo    generated_index.html   RPM...
```
Via web debemos tener lo siguiente:
 [![Build Status](https://i.ibb.co/Vg2d826/version.png)]()

### Descargar los parcels del  5.13 
Ir a la siguente URL: http://archive.cloudera.com/cdh5/parcels/5.13/ y descargar los 3 archivos que a continuación se listan

>> Importante estos 3 archivos se depositan en "cdh-5.13.0"

Descargar los siguientes paquetes:
- CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel
- CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel.sha1
- manifest.json

```sh
xiafra@hadoop$ cd /var/www/html/cdh-5.13.0
xiafra@hadoop$ wget http://archive.cloudera.com/cdh5/parcels/5.13/CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel
xiafra@hadoop$ wget http://archive.cloudera.com/cdh5/parcels/5.13/CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel.sha1
xiafra@hadoop$ wget http://archive.cloudera.com/cdh5/parcels/5.13/manifest.json
```











