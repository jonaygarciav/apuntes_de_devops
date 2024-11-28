# Tareas Posteriores a la Instalación de Ubuntu Desktop 22.04

## Actualización del sistema

```bash
$ sudo apt update

$ sudo apt upgrade
```

Limpiamos los paquetes que descargaron durante el proceso de instalación, que se almacenan en el directorio _/var/cache/apt/archives/_:

```bash
$ sudo apt clean
```

__Nota__: con esta acción liberamos espacio en el sistema.

Reiniciamos el sistema:

```bash
$ sudo reboot
```

## Cambiar el hostname

```bash
alumno@ub-server:~$ sudo hostnamectl set-hostname mysql-server
[sudo] password for alumno:

alumno@ub-server:~$ exec bash
alumno@mysql-server:~$
```

## Configuración IP estática

Vamos a configurar una IP estática a Ubuntu Server 22.04, en este ejemplo dentro de la red de Clase C 192.168.1.0/24:

* IP: 192.168.1.100
* Máscara: 255.255.255.0
* Puerta de enlace: 192.168.1.1
* Servidores DNS: 8.8.8.8 8.8.4.4

Renombramos cualquier archivo de configuración de red que exista:

```bash
for file in /etc/netplan/*.yaml; do mv "$file" "${file}.old"; done
```

Modificamos el fichero __00-installer-config.yaml__ con la configuración de red:

```bash
$ sudo nano /etc/netplan/00-installer-config.yaml
network:
    version: 2
    renderer: NetworkManager
    ethernets:
        enp0s3:
            addresses:
                - 192.168.1.100/24
            nameservers:
                addresses: [8.8.8.8, 8.8.4.4]
            routes:
                - to: default
                  via: 192.168.1.1
```

__Nota__: en Ubuntu Desktop es importante configurar la opción `renderer` con el valor `NetworkManager`. 

Restringimos los permisos del fichero:

```bash
$ sudo chmod 0600 /etc/netplan/00-installer-config.yaml
```

Aplicamos la configuración:

```bash
$ sudo netplan apply
```

Comprobamos los parámetros de red a ver si se han aplicado correctamente:

```bash
# Comprobar IP y máscara de red
$ ip a
...
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:7c:7a:b3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.98/24 brd 192.168.1.255 scope global enp0s3

# Comprobar puerta de enlace
$ ip route
default via 192.168.1.1 dev enp0s3 proto static
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.98

# Comprobar servidores DNS
$ resolvectl status
Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 8.8.8.8
       DNS Servers: 8.8.8.8 8.8.4.4
```

## Ampliar el tamaño del disco (sin LVM)

Apagamos la VM y entramos al menú de VirtualBox _Archivo-Herramientas-Administrador de medios virtuales_.

Seleccionamos el disco de la VM que queremos ampliar, y, en sus atributos, asignamos el nuevo tamaño de disco.

Arrancamos la VM.

Verificar que el tamaño de la partición /sda2 es tiene el tamaño anterior, en este caso 25GB:
Lista las particiones en el disco:

```bash
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
...
sda      8:0    0   50G  0 disk 
├─sda1   8:1    0  512M  0 part /boot
└─sda2   8:2    0   25G  0 part /
...
```

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
...
/dev/sda2        25G  5,2G   19G  23% /

```

Ampliar la partición _sda2_ hasta el tamaño máximo del disco, para este ejemplo hemos ampliado el disco a 50 GB:

```bash
sudo growpart /dev/sda 2
CHANGED: partition=2 start=4096 old: size=52422656 end=52426752 new: size=105452639 end=105456735

$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
...
sda      8:0    0 50,3G  0 disk
├─sda1   8:1    0    1M  0 part
└─sda2   8:2    0 50,3G  0 part /
...
```

Redimensionamos el sistema de ficheros de la partición _sda2_ para que detecte todo el tamaño de la partición:

```bash
$ sudo resize2fs /dev/sda2
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/sda2 is mounted on /; on-line resizing required
old_desc_blocks = 4, new_desc_blocks = 7
The filesystem on /dev/sda2 is now 13181579 (4k) blocks long.

$ df -h
Filesystem      Size  Used Avail Use% Mounted on
...
/dev/sda2        50G  5,2G   42G  11% /
...
```
