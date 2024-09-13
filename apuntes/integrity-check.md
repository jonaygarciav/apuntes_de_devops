
# Integridad de ficheros

## Comprobar la integridad de un fichero en Windows

Realizaremos la comprobación del CHECKSUM mediante la herramienta de consola _certutil_ de Windows. Para este ejemplo hemos descargado las imágenes _desktop_ y _server_ de Ubuntu 22.04, así como el archivo _SHA256SUMS.txt_ el cual contiene el CHECKSUM de cada uno de los ficheros ISO. Los CHECKSUM, tal y como indica el nombre del fichero, han sido generados utilizando el algoritmo _SHA256_:

[https://releases.ubuntu.com/jammy/](https://releases.ubuntu.com/jammy/)


```
C:\Users\Jonay>cd E:\ISOs\ubuntu\22.04

E:\ISOs\ubuntu\22.04>dir
 El volumen de la unidad E es VMs
 El número de serie del volumen es: D032-4798

 Directorio de E:\ISOs\ubuntu\22.04

11/09/2024  08:50    <DIR>          .
11/09/2024  08:50    <DIR>          ..
11/09/2024  08:28               202 SHA256SUMS.txt
11/09/2024  08:43     5.017.356.288 ubuntu-22.04.4-desktop-amd64.iso
11/09/2024  08:36     2.104.408.064 ubuntu-22.04.4-live-server-amd64.iso
               4 archivos  7.121.765.387 bytes
               2 dirs  648.009.555.968 bytes libres
```

Calcularemos el CHECKSUM de cada uno de los ficheros descargados:

```
E:\ISOs\ubuntu\22.04>certutil -hashfile ubuntu-22.04.4-live-server-amd64.iso SHA256
SHA256 hash de ubuntu-22.04.4-live-server-amd64.iso:
45f873de9f8cb637345d6e66a583762730bbea30277ef7b32c9c3bd6700a32b2
CertUtil: -hashfile comando completado correctamente.

E:\ISOs\ubuntu\22.04>certutil -hashfile ubuntu-22.04.4-desktop-amd64.iso SHA256
SHA256 hash de ubuntu-22.04.4-desktop-amd64.iso:
071d5a534c1a2d61d64c6599c47c992c778e08b054daecc2540d57929e4ab1fd
CertUtil: -hashfile comando completado correctamente.
```

Una vez calculado el CHECKSUM, hay que comprobar que coincide con el que se encuentra dentro del fichero _SHA256SUMS.txt_.