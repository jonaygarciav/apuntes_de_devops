# Instalación de Ubuntu Server 22.04 en VirtualBox

## Instalación del OS

![][01]
![][02]
![][03]
![][04]
![][05]
![][06]
![][07]
![][08]
![][09]
![][10]
![][11]
![][12]
![][13]
![][14]
![][15]
![][16]
![][17]
![][18]
![][19]
![][20]
![][21]

## Configuración IP estática

Vamos a configurar una IP estática a Ubuntu Server 22.04, en este ejemplo dentro de la red de Clase C 192.168.1.0/24:

* IP: 192.168.1.100
* Máscara: 255.255.255.0
* Puerta de enlace: 192.168.1.1
* Servidores DNS: 8.8.8.8 8.8.4.4

Creamos el fichero __01-network-configuration.yaml__ con la configuración de red:

```bash
$ sudo nano /etc/netplan/00-installer-config.yaml
network:
    version: 2
    renderer: networkd
    ethernets:
        eth0:
            addresses:
                - 192.168.1.100/24
            nameservers:
                addresses: [8.8.8.8, 8.8.4.4]
            routes:
                - to: default
                  via: 192.168.1.1
```

Restringimos los permisos del fichero:

```bash
$ sudo chmod 0600 /etc/netplan/00-installer-config.yaml
```

Aplicamos la configuración:

```bash
$ sudo networkplan apply
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

[01]: ../img/ub-server22.04/vbox-install/01.png "01"
[02]: ../img/ub-server22.04/vbox-install/02.png "02"
[03]: ../img/ub-server22.04/vbox-install/03.png "03"
[04]: ../img/ub-server22.04/vbox-install/04.png "04"
[05]: ../img/ub-server22.04/vbox-install/05.png "05"
[06]: ../img/ub-server22.04/vbox-install/06.png "06"
[07]: ../img/ub-server22.04/vbox-install/07.png "07"
[08]: ../img/ub-server22.04/vbox-install/08.png "08"
[09]: ../img/ub-server22.04/vbox-install/09.png "09"
[10]: ../img/ub-server22.04/vbox-install/10.png "10"
[11]: ../img/ub-server22.04/vbox-install/11.png "11"
[12]: ../img/ub-server22.04/vbox-install/12.png "12"
[13]: ../img/ub-server22.04/vbox-install/13.png "13"
[14]: ../img/ub-server22.04/vbox-install/14.png "14"
[15]: ../img/ub-server22.04/vbox-install/15.png "15"
[16]: ../img/ub-server22.04/vbox-install/16.png "16"
[17]: ../img/ub-server22.04/vbox-install/17.png "17"
[18]: ../img/ub-server22.04/vbox-install/18.png "18"
[19]: ../img/ub-server22.04/vbox-install/19.png "19"
[20]: ../img/ub-server22.04/vbox-install/20.png "20"
[21]: ../img/ub-server22.04/vbox-install/21.png "21"
