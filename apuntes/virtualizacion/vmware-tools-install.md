# Instalar las VMware Tools

Cuando intentamos instalar las _VMware Tools_ aparece el siguiente mensaje:

![][01]


En su lugar podemos instalar el paquete `open-vm-tools`, que se puede descargar e instalar desde los repositorios oficiales.

## Sistemas basados en paquetes DEB

OS con entorno escritorio (_Debian con entorno gráfico_, _Ubuntu Desktop_, _Linux Mint_):

```bash
$ sudo apt install open-vm-tools open-vm-tools-desktop -y

$ vmware-toolbox-cmd -v
```

OS sin entorno escritorio (_Debian sin entorno gráfico_, _Ubuntu Server_):

```bash
$ sudo apt install open-vm-tools -y

$ vmware-toolbox-cmd -v
```

[01]: ../img/vmware-tools-install/01.png "01"
