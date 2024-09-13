# Instalación de Linux Mint 21.3 en VirtualBox

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
