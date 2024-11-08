# Instalar Docker en Ubuntu Server 22.04

* Desinstalar versiones antiguas
* Configurar el repositorio apt de Docker
* Instalar los paquetes de Docker
* Desinstalar Docker Engine

## Desinstalar versiones antiguas

Antes de instalar Docker Engine, necesitas desinstalar cualquier paquete en conflicto.

Los mantenedores de distribuciones proporcionan distribuciones no oficiales de paquetes de Docker en APT. Debes desinstalar estos paquetes antes de poder instalar la versión oficial de Docker Engine.

Los paquetes no oficiales a desinstalar son:

* docker.io
* docker-compose
* docker-compose-v2
* docker-doc
* podman-docker

Además, Docker Engine depende de containerd y runc. Docker Engine agrupa estas dependencias en un solo paquete: containerd.io. Si has instalado containerd o runc previamente, desinstálalos para evitar conflictos con las versiones incluidas en Docker Engine.

Ejecuta el siguiente comando para desinstalar todos los paquetes en conflicto:

```bash
$ for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

> __Nota__: es posible que apt-get indique que no tienes ninguno de estos paquetes instalados.

Las imágenes, contenedores, volúmenes y redes almacenados en /var/lib/docker/ no se eliminan automáticamente al desinstalar Docker. Si deseas comenzar con una instalación limpia y prefieres limpiar los datos existentes, consulta la sección desinstalar Docker Engine.

## Configurar el repositorio apt de Docker

```bash
# Añadir la clave GPG oficial de Docker:
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl -y
$ sudo install -m 0755 -d /etc/apt/keyrings
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
$ sudo chmod a+r /etc/apt/keyrings/docker.asc

# Añadir el repositorio a las fuentes de Apt:
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

$ sudo apt-get update
```

> __Nota__: si usas una distro derivada de Ubuntu, como Linux Mint, es posible que debas usar UBUNTU_CODENAME en lugar de VERSION_CODENAME.

## Instalar los paquetes de Docker

Para instalar la última versión de Docker:

```bash
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

Comprobar que el servicio de Docker se está ejecutando en el sistema:

```bash
$ systemctl status docker.service
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2024-10-27 21:57:54 UTC; 1min 8s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 2257 (dockerd)
      Tasks: 9
     Memory: 21.6M
        CPU: 226ms
     CGroup: /system.slice/docker.service
             └─2257 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.401268451Z" level=info msg="Starting up"
oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.402104896Z" level=info msg="detected 127.0.0.53 nameserver, assuming systemd-resolved, so using resolv.conf: /run/systemd/resolve/resolv.conf"
oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.563977231Z" level=info msg="Loading containers: start."
oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.936520716Z" level=info msg="Loading containers: done."
oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.963322116Z" level=warning msg="WARNING: bridge-nf-call-iptables is disabled"
oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.963342554Z" level=warning msg="WARNING: bridge-nf-call-ip6tables is disabled"
oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.963374574Z" level=info msg="Docker daemon" commit=41ca978 containerd-snapshotter=false storage-driver=overlay2 version=27.3.1
oct 27 21:57:53 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:53.963449313Z" level=info msg="Daemon has completed initialization"
oct 27 21:57:54 ubuntu-server2204 dockerd[2257]: time="2024-10-27T21:57:54.044676951Z" level=info msg="API listen on /run/docker.sock"
oct 27 21:57:54 ubuntu-server2204 systemd[1]: Started Docker Application Container Engine.
```

```bash
$ systemctl status containerd.service
● containerd.service - containerd container runtime
     Loaded: loaded (/lib/systemd/system/containerd.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2024-10-27 21:57:51 UTC; 1min 43s ago
       Docs: https://containerd.io
   Main PID: 2082 (containerd)
      Tasks: 8
     Memory: 13.1M
        CPU: 64ms
     CGroup: /system.slice/containerd.service
             └─2082 /usr/bin/containerd

oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.419609379Z" level=info msg="skip loading plugin \"io.containerd.tracing.processor.v1.otlp\"..." error="skip plugin: tracing endpoint not configured" type=io.containerd.tracing.processor.v1
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.419684619Z" level=info msg="loading plugin \"io.containerd.internal.v1.tracing\"..." type=io.containerd.internal.v1
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.419753086Z" level=info msg="skip loading plugin \"io.containerd.internal.v1.tracing\"..." error="skip plugin: tracing endpoint not configured" type=io.containerd.internal.v1
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.419820382Z" level=info msg="loading plugin \"io.containerd.grpc.v1.healthcheck\"..." type=io.containerd.grpc.v1
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.419887025Z" level=info msg="loading plugin \"io.containerd.nri.v1.nri\"..." type=io.containerd.nri.v1
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.419957607Z" level=info msg="NRI interface is disabled by configuration."
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.420253336Z" level=info msg=serving... address=/run/containerd/containerd.sock.ttrpc
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.420394940Z" level=info msg=serving... address=/run/containerd/containerd.sock
oct 27 21:57:51 ubuntu-server2204 containerd[2082]: time="2024-10-27T21:57:51.420573502Z" level=info msg="containerd successfully booted in 0.048005s"
oct 27 21:57:51 ubuntu-server2204 systemd[1]: Started containerd container runtime.
```

Añadir al usuario _alumno_ al grupo _docker_ para que pueda ejecutar contenedores sin usar _sudo_:

```bash
$ sudo usermod -aG docker alumno
```

Reinicia el sistema para que estos cambios tengan efecto:

```bash
$ sudo reboot
```

Verifica que la instalación de Docker Engine fue exitosa ejecutando la imagen hello-world:

```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete
Digest: sha256:d211f485f2dd1dee407a80973c8f129f00d54604d2c90732e8e320e5038a0348
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Cuando ejecutamos el comando `docker run hello-world`. se descarga una imagen llamada _hello-world_ y ejecuta un contenedor a partir de esta imagen. Cuando el contenedor se ejecuta, imprime un mensaje de textp de confirmación y el contenedor finaliza su ejecución.

## Desinstalar Docker Engine

Si quisiéramos desinstalar Docker Engine, CLI, containerd y los paquetes de Docker Compose:

```bash
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

Las imágenes, contenedores, volúmenes o archivos de configuración personalizados en tu host no se eliminan automáticamente. Para borrar todas las imágenes, contenedores y volúmenes:

```bash
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```

Referencia(s):
* [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/).