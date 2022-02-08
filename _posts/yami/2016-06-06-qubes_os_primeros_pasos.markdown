---
img_path: /assets/yami/

categories:
- Blog
- やみのきし

tags:
- Guía
- linux
- Qubes OS

date: 2016-06-06 23:55:21+00:00
title: QUBES OS Primeros pasos

image:
  src: logo13.jpg
---

QUBES OS se centra en la seguridad del sistema e implementa seguridad por aproximación de Aislamiento.

<!-- more -->

![Qubes OS arch](qubes-arch-diagram.jpg){: .shadow }

La versión con la que comencé a probarla fue la "3RC2", para mi sorpresa, anaconda el Instalador de la distribución, se me quedaba colgado nada mas iniciarse. El problema, aparentemente se debía a la gráfica "ATI".

Al basarse este sistema en múltiples máquinas virtuales, las primeras preguntas con los que me encontré fueron:

    * ¿Cómo usar el portapapeles?
    * ¿Cómo copiar ficheros?

## Uso del portapapeles

Para comprender el uso de el portapapeles de Qubes, tenemos que pensar que **existen 2**, uno propio de cada maquina virtual, el cual podremos usar con normalidad como siempre; Y un segundo "genérico" entre todas las maquinas. Para copiar o pegar algo entre maquinas tendremos que, primero copiar lo que queramos de la maquina en cuestión (ctrl-c), y segundo, (ctrl-alt-c) copiándose así el portapapeles de la maquina virtual al portapapeles genérico, como podremos imaginarnos para pegar (ctrl-alt-v) y (ctrl-v) en la maquina que queramos.

## Copiar ficheros

Qubes nos permite copiar ficheros entre los distintos dominios mediante el uso del comando :

    qvm-copy-to-vm [–without-progress] dest_vmname file [file]

Como se puede ver en el comando, se especifica el dominio de destino y qué fichero copiar.

¿Pero donde lo copia?

En la "home" del dominio tendremos una carpeta "QubesIncoming" y dentro de ella el nombre de la maquina origen, y en su interior se encuentran los ficheros que se mandaron a copiar.

## Maquinas desechables

QUBES OS nos da la posibilidad de usar maquinas para un solo uso, con esto me refiero a que podemos realizar cualquier acción, y cuando se termine, se apagará, y se borrará la maquina con todos los datos usados, donde hemos podido usar datos sensibles.

Esto es útil si queremos realizar alguna acción en la que tengamos dudas de su fiabilidad, como por ejemplo, abrir una web de origen sospechoso.

Desde cualquier Dominio, si tenemos las "Tools" de QUBES OS, podremos abrir cualquier fichero de forma gráfica:

![disponsable](disponsable.png){: .shadow }

Y desde "commandline":

    qvm-open-in-dvm file

También tenemos un acceso directo creado en el menú del sistema para abrir fácilmente un navegador.

La plantilla utilizada por defecto sería "fedora-23-dvm". Si queremos cambiar de plantilla, podremos utilizar el siguiente comando:

    qvm-create-default-dvm <custom-template-name>

## Actualizar QUBES OS

Desde la herramienta "Qubes VM Manager" tendremos la opción de actualizar de una forma sencilla, cuando las actualizaciones sean detectadas, sin embargo, si queremos forzar la actualización de algún "template" podremos encenderla y actualizarla (en caso de usar windows tendrá que ser así).

Para actualizar el dom0 podremos mediante el comando:

    qubes-dom0-update
    
    qubes-dom0-update --enablerepo=qubes*testing

## Habilitar pantalla completa

Por defecto Qubes OS no permite el uso de pantalla completa, porque es considerado un fallo de seguridad, ya que, un atacante podría aprovechar esa funcionalidad para simular nuestro sistema, cambiarnos nuestro entorno y terminar escribiendo alguna "password" donde realmente no deberíamos.
Si aún así, queremos, por ejemplo, ver vídeos a pantalla completa, tendremos que modificar el fichero /etc/qubes/guid.conf.
Para especificar una maquina en concreto, la sintaxis sería la siguiente:

    VM: {
      Dominio: {
        allow_fullscreen = true;
      };
    };

También tendremos la posibilidad de activarlo de forma global:

    global: {
      allow_fullscreen = true; 
    };

Y si fuese el caso del ejemplo (el de reproducir un vídeo), puede que también nos interese activar la opción "audio_low_latency**" **a false pasando a un retardo máximo a 40ms, comparado a 200-500ms, que hay por defecto. Hay que tener en cuenta que esto aumentara el uso de CPU.

## Instalación de windows 7 como plantilla en QUBES OS

Qubes OS permite a las máquinas HVM compartir un sistema de archivos raíz común de un Template, al igual que se hace para Linux AppVMs. Éste modo no se limita a Windows AppVMs, y se puede utilizar para cualquier HVM (por ejemplo, FreeBSD).

    qvm-create --hvm-template -l black windows-7

[![create_template](create_template.png){: .shadow }

Por defecto se crean dos discos para maquinas HVM:

    1. La raíz del sistema (20GB)
    2. Uno pirvado para la home (2GB)

Ahora iniciaremos la maquina virtual con la iso/cd con el que realizaremos la instalación:

    qvm-start windows-7 --cdrom=/usr/local/iso/win7_en.iso
    
    --cdrom=/dev/cdrom
    
    --cdrom=[appvm]:[/path/to/iso/within/appvm]

![start_template1](start_template1.png){: .shadow }

Una vez instalado Windows, tendremos que instalar las "Tools" de Qubes OS. Viene pre-seleccionada la opción para almacenar el directorio "C:\Users" en el segundo disco, que crea automáticamente Qubes OS (el privado). **Éste segundo disco no se restablece al reiniciar**, así que los directorios y perfiles del usuario sobrevivirían tras cada reinicio, a diferencia del sistema de archivos "raíz", que se restablece a la imagen del Template automáticamente.

**Si se selecciona esta característica** la instalación requerirá de **dos reinicios:**

    1. Se inicializará y se dará formato al disco privado en el primer reinicio (no se puede hacer durante la instalación, debido a que los controladores de almacenamiento masivo Xen aún no están activos).
    2. Los perfiles de usuario se mueven en el disco privado.

Ésto se realizará automáticamente con el instalador, por lo que, simplemente deberemos dejar que él solo termine.

### Instalación de QUBES OS Tools

Instalar/Actualizar las herramientas para Windows en Qubes OS:

    qubes-dom0-update --enablerepo=qubes*testing qubes-windows-tools

Para poder ejecutar Qubes Tools, tendremos que deshabilitar el chequeo de drivers firmados, para ello tendremos que ejecutar la terminal como administrador, lanzar el siguiente comando y reiniciar:

    bcdedit /set testsigning on

Teniendo la maquina apagada, podremos iniciar la instalación de las Tools mediante el siguiente comando:

    qvm-start win7-x64-template --install-windows-tools

## Optimización

En la web oficial de Qubes OS existe documentación para la optimización de Windows [link,](https://www.qubes-os.org/doc/windows-template-customization/) que seria interesante realizar.

### Aumentar TimeOut

Podemos aumentar el "timeout" por defecto de Qubes OS, ya que esto puede venir bien para Windows, puesto que puede ser mas lento (por defecto 60s):

    qvm-prefs -s <vm-name> qrexec-timeout 300

## Creación de dominios

Una vez que la plantilla se ha creado, es muy fácil crear Dominios basados en ella.

Ejecutando el siguiente comando:

    qvm-create --hvm <new windows appvm name> --template <name of template vm> --label <label color>

Hay que tener en cuenta, que la raíz se copiará de la plantilla en cada inicio,(recordar que podemos tener la unidad "D" con la home del usuario). En caso de querer guardar cosas en el disco principal y no perderlos al reiniciar, ya sea porque lo requiera alguna aplicación o cualquier motivo de desarrollo, podemos aprovechar igualmente la plantilla y crear dominios con la opción "Standalone" (no modificará la raíz en cada inicio), solamente tendremos que añadir la opción "--standalone" al comando anterior.

## Ejecutar aplicaciones

Para lanzar aplicaciones desde dom0:

    qvm-run -a MyWIn7 explorer.exe

Para lanzarlas desde otro dominio:

    qvm-open-in-vm MyWin7 http://yaminokishi.com
