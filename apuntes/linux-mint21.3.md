# Instalación de Linux Mint 21.3 en VirtualBox

Tanto la creación de la VN, así como la instalación del SO y de las VirtualBox Guest Additions se ha hecho sobre la versión 7.0.20 de VirtualBox.

## Creación y Configuración de la VM

![][01]
![][02]
![][03]
![][04]
![][05]
![][06]
![][07]
![][08]

## Instalación del SO

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
![][22]

## Instalación de las VirtualBox Guest Additions

Actualizar los paquetes del sistema:

```
$ sudo apt update
$ sudo apt upgrade

$ sudo reboot
```

Instalar dependencias:

```
$ sudo apt install build-essential dkms linux-headers-$(uname -r)
```

Montar ISO con las VirtualBox Guest Additions a través del menú _Dispositivos-Instalar imagen de CD de los complementos del invitado..._ y, a continuación, realizamos la instalación:

```
$ cd /media/alumno/VBox_GAs_7.0.20/
$ sudo ./VBoxLinuxAdditions.run

$ sudo reboot
```

Si vamos a utilizar _carpetas compartidas_ es necesario añadir el usuario _alumno_ al grupo _vboxsf_:

```
$ sudo usermod -aG vboxsf alumno
```

[01]: ./img/linux-mint21.3/vm01.png "01"
[02]: ./img/linux-mint21.3/vm02.png "02"
[03]: ./img/linux-mint21.3/vm03.png "03"
[04]: ./img/linux-mint21.3/vm04.png "04"
[05]: ./img/linux-mint21.3/vm05.png "05"
[06]: ./img/linux-mint21.3/vm06.png "06"
[07]: ./img/linux-mint21.3/vm07.png "07"
[08]: ./img/linux-mint21.3/vm08.png "08"
[09]: ./img/linux-mint21.3/so01.png "09"
[10]: ./img/linux-mint21.3/so02.png "10"
[11]: ./img/linux-mint21.3/so03.png "11"
[12]: ./img/linux-mint21.3/so04.png "12"
[13]: ./img/linux-mint21.3/so05.png "13"
[14]: ./img/linux-mint21.3/so06.png "14"
[15]: ./img/linux-mint21.3/so07.png "15"
[16]: ./img/linux-mint21.3/so08.png "16"
[17]: ./img/linux-mint21.3/so09.png "17"
[18]: ./img/linux-mint21.3/so10.png "18"
[19]: ./img/linux-mint21.3/so11.png "19"
[20]: ./img/linux-mint21.3/so12.png "20"
