# Instalación de Linux Mint 21.3 en VirtualBox

Tanto la creación de la VM, así como la instalación del SO y de las VirtualBox Guest Additions se ha hecho sobre la versión 7.0.20 de VirtualBox.

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

## Instalación de las VirtualBox Guest Additions

Para realizar la instalación de las VirtualBox Guest Additions se debe realizar a través de la terminal de Linux Mint:

![][21]

Actualizar los paquetes del sistema a través del terminal y luego reiniciamos la VM:

```
$ sudo apt update
$ sudo apt upgrade

$ sudo reboot
```

Instalar dependencias para que VirtualBox Guest Additions funcione correctamente a través de la terminal y luego reiniciamos la VM:

```
$ sudo apt install build-essential dkms linux-headers-$(uname -r)

$ sudo reboot
```

Ahora pulsamos sobre el menú _Dispositivos-Instalar imagen de CD de los complementos del invitado..._:

![][22]

Esto nos montará una ISO con los ejecutables de las VirtualBox Guest Additions dentro de la VM:

![][23]

Y, a continuación, realizamos la instalación pulsando sobre _Ejecutar_ en el menú que se nos abre en en el escritorio de la VM:

![][24]

O también podemos realizar la instalación a través de la terminal:

```
$ cd /media/alumno/VBox_GAs_7.0.20/
$ sudo ./VBoxLinuxAdditions.run

$ sudo reboot
```

> __Nota__: fíjate que la versión que yo tengo instalada de VirtualBox en mi equipo es la 7.0.20, y la versión aparece como parte de la ruta donde se encuentra el archivo _VBoxLinuxAdditions.run_. Si tienes instalada una versión de VirtualBox diferente, la ruta será _/media/alumno/_VBox_GAs_\<version_virtualbox\>_.

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
[21]: ./img/linux-mint21.3/terminal-icon.png "21"
[22]: ./img/linux-mint21.3/install-vbox-ga-from-menu01.png "22"
[23]: ./img/linux-mint21.3/install-vbox-ga-from-menu02.png "23"
[24]: ./img/linux-mint21.3/install-vbox-ga-from-menu03.png "24"
