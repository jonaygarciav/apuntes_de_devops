# Tareas Posteriores a la Instalación de Fedora Server 40

* Crear el usuario alumno y añadirlo al grupo sudo
* Comprobar la versión del sistema
* Actualizar el sistema
* Cambiar el hostname
* Configurar IP estática
* Ampliar el tamaño del disco (con LVM)
* Instalar servidor SSH

## Crear el usuario alumno y añadirlo al grupo sudo

```bash
# sudo useradd -m -s /bin/bash alumno
# passwd alumno
```

```bash
# usermod -aG wheel alumno
```

```bash
# reboot
```

## Comprobar la versión del sistema

```bash
$ cat /etc/fedora-release 
Fedora release 40 (Forty)
```

## Actualizar el sistema

```bash
$ sudo dnf update
```

Limpiamos los paquetes que se descargaron durante el proceso de instalación, que se almacenan en el directorio _/var/cache/dnf_:

```bash
$ sudo dnf clean all
```

__Nota__: con esta acción liberamos espacio en el sistema.

Reiniciamos el sistema:

```bash
$ sudo reboot
```

## Cambiar el hostname

```bash
[alumno@fedora ~]$ sudo hostnamectl set-hostname fedora-server35

[alumno@fedora ~]$ exec bash
[alumno@fedora-server35 ~]$
```

## Configurar IP estática

Vamos a configurar una IP estática a Fedora Server 35, en este ejemplo dentro de la red de Clase C 192.168.1.0/24:

* IP: 192.168.1.100
* Máscara: 255.255.255.0
* Puerta de enlace: 192.168.1.1
* Servidores DNS: 8.8.8.8 8.8.4.4


```bash
$ sudo nmcli connection show
NAME    UUID                                  TYPE      DEVICE
enp0s3  38a58955-cb9b-343d-bc0f-644fcba7df54  ethernet  enp0s3
...
```

```bash
$ sudo nmcli connection delete enp0s3
La conexión «enp0s3» (38a58955-cb9b-343d-bc0f-644fcba7df54) se ha borrado correctamente.
```

```bash
$ sudo nmcli connection show
NAME    UUID                                  TYPE      DEVICE
...
```

```bash
$ sudo nmcli connection add type ethernet ifname enp0s3 con-name enp0s3 ipv4.addresses 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.dns "8.8.8.8,8.8.4.4" ipv4.method manual
Conexión <<enp0s3>> (bbad9cbf-6bed-4e83-a28f-daf031a3ba09) añadida con éxito.
```

```bash
$ sudo nmcli connection up enp0s3
Conexión activada con éxito (ruta activa D-Bus: /org/freedesktop/NetworkManager/ActivateConnection/3)
```

```bash
$ sudo nmcli connection show
NAME    UUID                                  TYPE      DEVICE
enp0s3  bbad9cbf-6bed-4e83-a28f-daf031a3ba09  ethernet  enp0s3
...
```

Comprobamos los parámetros de red a ver si se han aplicado correctamente:

```bash
# Comprobar IP y máscara de red
$ ip a
...
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a9:a2:82 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.100/24 brd 192.168.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::6781:9723:50b3:bdfa/64 scope link noprefixroute
       valid_lft forever preferred_lft forever

# Comprobar puerta de enlace
$ ip route
default via 192.168.1.1 dev enp0s3 proto static metric 100
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.100 metric 100

# Comprobar servidores DNS
$ resolvectl status
Global
       Protocols: LLMNR=resolve -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS LLMNR/IPv4 LLMNR/IPv6
         Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 8.8.8.8
       DNS Servers: 8.8.8.8 8.8.4.4
```

# Ampliar el tamaño del disco (con LVM)

```bash
$ df -h
S.ficheros                     Tamaño Usados  Disp Uso% Montado en
devtmpfs                         4,0M      0  4,0M   0% /dev
tmpfs                            2,0G      0  2,0G   0% /dev/shm
tmpfs                            784M   996K  783M   1% /run
/dev/mapper/fedora-root           15G   1,5G   14G  10% /
tmpfs                            2,0G   4,0K  2,0G   1% /tmp
/dev/sda2                       1014M   215M  800M  22% /boot
tmpfs                            392M      0  392M   0% /run/user/1000
```

```bash
$ sudo vgdisplay
  --- Volume group ---
  VG Name               fedora_fedora
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <29,00 GiB
  PE Size               4,00 MiB
  Total PE              7423
  Alloc PE / Size       3840 / 15,00 GiB
  Free  PE / Size       3583 / <14,00 GiB
  VG UUID               l7cnfS-5Q0z-KqPR-C5tD-xqV0-WRev-dpMIur
```

```bash
$ sudo lvextend -l+100%FREE /dev/mapper/fedora-root
  Size of logical volume fedora/root changed from 15,00 GiB (3840 extents) to <29,00 GiB (7423 extents).
  Logical volume fedora/root successfully resized.
```

```bash
$ sudo xfs_growfs /dev/mapper/fedora-root
meta-data=/dev/mapper/fedora-root isize=512    agcount=4, agsize=983040 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=0 inobtcount=0
data     =                       bsize=4096   blocks=3932160, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 3932160 to 7601152
```

```bash
$ df -h
S.ficheros                     Tamaño Usados  Disp Uso% Montado en
devtmpfs                         4,0M      0  4,0M   0% /dev
tmpfs                            2,0G      0  2,0G   0% /dev/shm
tmpfs                            784M   996K  783M   1% /run
/dev/mapper/fedora-root           29G   1,6G   28G   6% /
tmpfs                            2,0G   4,0K  2,0G   1% /tmp
/dev/sda2                       1014M   215M  800M  22% /boot
tmpfs                            392M      0  392M   0% /run/user/1000
```

# Instalar servidos SSH

Por defecto el servicio SSH está instalado, pero si por algún motivo tuviéramos que instalarlo tenemos que ejecutar el siguiente comando:

```bash
$ sudo dnf install openssh-server
```

```bash
$ systemctl status sshd.service
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2024-12-06 07:31:48 WET; 18min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 787 (sshd)
      Tasks: 1 (limit: 4649)
     Memory: 4.9M
        CPU: 57ms
     CGroup: /system.slice/sshd.service
             └─ 787 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

dic 06 07:31:48 fedora systemd[1]: Starting OpenSSH server daemon...
dic 06 07:31:48 fedora sshd[787]: Server listening on 0.0.0.0 port 22.
dic 06 07:31:48 fedora sshd[787]: Server listening on :: port 22.
dic 06 07:31:48 fedora systemd[1]: Started OpenSSH server daemon.
dic 06 07:44:43 fedora-server35 sshd[1012]: Accepted password for alumno from 192.168.1.51 port 50349 ssh2
dic 06 07:44:43 fedora-server35 sshd[1012]: pam_unix(sshd:session): session opened for user alumno(uid=1000) by (uid=0)
```
