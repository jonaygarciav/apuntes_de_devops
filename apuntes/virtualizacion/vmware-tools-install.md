# Instalar las VMware Tools

VMware recomienda no instalar las VMware Tools, en cambio, instalar las open-vm-tools, que es un paquete que se puede descargar e instalar desde los repositorios.

## Sistemas basados en paquetes APT

OS con entorno escritorio (Debian con entorno gráfico, Ubuntu Desktop, Linux Mint):

```bash
$ sudo apt install open-vm-tools open-vm-tools-desktop -y

$ vmware-toolbox-cmd -v
```

OS sin entorno escritorio (Debian sin entorno gráfico, Ubuntu Server):

```bash
$ sudo apt install open-vm-tools -y

$ vmware-toolbox-cmd -v
```
