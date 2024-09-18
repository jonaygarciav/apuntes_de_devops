# Acceso Remoto a Máquinas Virtuales en VirtualBox

## Acceso mediante SSH en modo NAT

Necesitamos crear una regla de _Reenvío de puertos_ para poder conectarnos desde el _Host_ o _anfitrión_ (nuestro equipo) al _Guest_ o _invitado_ (máquina virtual):
* __Nombre__: SSH
* __Protocolo__: TCP
* __IP anfitrión__: (se deja vacío)
* __Puerto anfitrión__: 2222
* __IP invitado__: (se deja vacío)
* __Puerto invitado__: 22 (puerto donde escucha peticiones el servidor SSH dentro de la VM)

![][01]
![][02]

Esta regla de _Reenvío de puertos_ crea un servicio en el _Host_ o _anfitrión_ (nuestro equipo) que escucha peticiones en el puerto 2222. Al acceder al puerto 2222 en el _Host_ o _anfitrión_ (nuestro equipo) se nos redirigirá al puerto 22 del _Guest_ o _invitado_ (máquina virtual). Para acceder a un servicio que se encuentra en el _Host_ o _anfitrión_ (nuesrto equipo) podemos usar el alias _localhost_ o la IP _127.0.0.1_.

![][03]

> __Nota__: _localhost_ es un alias que se refiere a la dirección IP _127.0.0.1_, específicamente para la interfaz de red de bucle invertido (loopback). Se utiliza para acceder a servicios y recursos alojados en nuestro propio equipo. Por ejemplo, al acceder a "localhost:8080", estamos accediendo a un servidor local en nuestro equipo en el puerto 8080.

### Acceso mediante cliente Putty:

[https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

Descargar el fichero _putty.exe_ versión 64 bits:

![][04]

Acceso mediante el cliente Putty:

![][05]
![][06]
![][07]
![][08]

### Acceso mediante cliente SSH desde la terminal de Windows:

El cliente SSH se encuentra ya instalado en el Sistema Operativo Microsoft Windows, para utilizarlo para conectarnos a la VM abrimos un _CMD_ y ejecutamos el siguiente comando:

```
C:\Users\Jonay> ssh -p 2222 alumno@localhost
```

![][09]

### Acceso mediante cliente SSH desde la terminal de Linux:

El cliente SSH se encuentra ya instalado  por defecto en los sistemas operativos Linux, para utilizarlo para conectarnos a la VM abrimos un _Terminal_ (CTRL+ALT+T) y ejecutamos el siguiente comando:

```
$ ssh -p 2222 alumno@localhost
```

![][10]

### Subir archivos a la VM mediante cliente FTP FileZilla:

El cliente FTP FileZilla nos permite copiar archivos desde el Host (nuestro equipo) hacia el Guest (VM) o viceversa:

* Servidor: localhost
* Nombre de usuario: alumno
* Contraseña: alumno
* Puerto: 2222

Una vez rellenado los parámetros pulsamos el botón _Conexión Rápida_:

![][11]

[01]: ./img/ssh-nat-access/vm-nat01.png "01"
[02]: ./img/ssh-nat-access/vm-nat02.png "02"
[03]: ./img/ssh-nat-access/vm-nat03.png "03"
[04]: ./img/ssh-nat-access/putty-ssh-client01.png "04"
[05]: ./img/ssh-nat-access/putty-ssh-client02.png "05"
[06]: ./img/ssh-nat-access/putty-ssh-client03.png "06"
[07]: ./img/ssh-nat-access/putty-ssh-client04.png "07"
[08]: ./img/ssh-nat-access/putty-ssh-client05.png "08"
[09]: ./img/ssh-nat-access/windows-ssh-client01.png "09"
[10]: ./img/ssh-nat-access/linux-ssh-client01.png "10"
[11]: ./img/ssh-nat-access/filezilla-client01.png "11"
