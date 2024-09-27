# Introducción a la Virtualización

La virtualización es una tecnología que se puede usar para crear representaciones virtuales de servidores, almacenamiento, redes y otras máquinas físicas. El software virtual imita las funciones del hardware físico para ejecutar varias máquinas virtuales a la vez en una única máquina física.

La virtualización proporciona varios beneficios a cualquier organización:

* __Utilización eficiente de los recursos__: la virtualización mejora los recursos de hardware que se utilizan en el centro de datos. Por ejemplo, en lugar de ejecutar un servidor en un sistema informático, se puede crear un grupo de servidores virtuales en el mismo sistema informático, al utilizar y devolver servidores al grupo según sea necesario. Tener menos servidores físicos subyacentes libera espacio en el centro de datos y supone un ahorro de dinero en electricidad, generadores y aparatos de refrigeración. 

* __Administración automatizada de las TI__: ahora que los ordenadores físicos son virtuales, se pueden administrar mediante el uso de herramientas de software. Los administradores crean programas de implementación y configuración para definir plantillas de máquinas virtuales. Es posible duplicar la infraestructura de forma repetida y coherente y evitar las configuraciones manuales propensas a errores.

* __Recuperación de desastres más rápida__: cuando eventos como los desastres naturales o los ataques cibernéticos afectan negativamente a las operaciones empresariales, recuperar el acceso a la infraestructura de TI y sustituir o arreglar un servidor físico puede llevar horas o incluso días. Por el contrario, al utilizar entornos virtualizados, el proceso tarda minutos. Esta rápida respuesta mejora significativamente la capacidad de recuperación y facilita la continuidad del negocio para que las operaciones puedan continuar según lo previsto.

![][01]

Las _máquinas virtuales_ y los _hipervisores_ son dos conceptos importantes en la virtualización:

* __Máquina virtual__: una máquina virtual es un equipo definido por software que se ejecuta en un equipo físico con un sistema operativo y recursos informáticos independientes. El ordenador físico se denomina máquina host y las máquinas virtuales son máquinas invitadas. Se pueden ejecutar varias máquinas virtuales en una sola máquina física. Un hipervisor extrae las máquinas virtuales del hardware del ordenador.

* __Hipervisor__: el hipervisor es un componente de software que administra varias máquinas virtuales en un ordenador. Garantiza que cada máquina virtual reciba los recursos asignados y no interfiera con el funcionamiento de otras máquinas virtuales. Existen dos tipos de hipervisores.

  * _Hipervisor de tipo 1_: un hipervisor de tipo 1, o hipervisor bare metal, es un programa de hipervisor que, en vez de instalarse en el sistema operativo, se instala de forma directa en el hardware del ordendaor. Por lo tanto, los hipervisores de tipo 1 tienen un mejor rendimiento y se utilizan con frecuencia para las aplicaciones empresariales. _Proxmox_ o _VMware ESXi_ son ejemplos de hipervisor de tipo 1.

  * _Hipervisor de tipo 2_: también conocido como hipervisor alojado, el hipervisor de tipo 2 está instalado en un sistema operativo. Los hipervisores de tipo 2 son adecuados para la informática del usuario final. _Oracle VirtualBox_ o _VMware Player_ son ejemplos de hipervisor de tipo 2.

Para que un equipo físico soporte máquinas virtuales, se debe habilitar en la BIOS la opción VT-x en equipos basados en Intel o la opcón AMD-v en equipos basados en AMD. A continuación se muestra el error que produce Oracle VirtualBox cuando intenta iniciar una máquina virtual sin tener habilitado la opción VT-x en la BIOS:

![][02]

Después de instalar el software de virtualización en la ordenador, podremos crear una o más máquinas virtuales. Se puede acceder a las máquinas virtuales de la misma manera que se accede a otras aplicaciones en el ordenador. El equipo físico se llama Host (anfitrión) y la máquina virtual se llama Guest (huésped). Varios huéspedes se pueden ejecutar en el host. Cada huésped tiene su propio sistema operativo, que puede ser el mismo o diferente del sistema operativo del host. 

Desde la perspectiva del usuario, la máquina virtual funciona como un servidor típico. Tiene ajustes, configuraciones y aplicaciones instaladas. Los recursos de computación, como las unidades centrales de procesamiento (CPU), la memoria de acceso aleatorio (RAM) y el almacenamiento aparecen de la misma manera que en un servidor físico. También es posible configurar y actualizar los sistemas operativos huéspedes y sus aplicaciones según sea necesario sin afectar al sistema operativo host.

El _hipervisor_ es el software de virtualización que se instala en la máquina física. Es una capa de software que actúa como intermediario entre las máquinas virtuales y el hardware subyacente o el sistema operativo del host. El hipervisor coordina el acceso al entorno físico de manera que varias máquinas virtuales tengan acceso a su propia cuota de recursos físicos. 

Si una máquina virtual requiere recursos de computación, como ciclos de CPU o memoria RAM, la solicitud se dirige primero al hipervisor. El hipervisor transmite entonces la solicitud al hardware subyacente, que realiza la tarea. 

[01]: ./img/intro-virtualizacion/hypervisor.png "01"
[02]: ./img/intro-virtualizacion/vtx-not-enabled.png "02"
