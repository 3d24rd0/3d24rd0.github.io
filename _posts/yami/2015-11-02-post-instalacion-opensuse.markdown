---
date: 2015-11-02 20:27:41+00:00

img_path: /assets/yami/

categories:
- Blog
- やみのきし

tags:
- Linux
- Suse
- Guía

title: Post-Instalación openSUSE

image:
  src: logo-evergreen1.png
---

En esta guía utilizaré ha menudo "Alt+F2", con ello me refiero a lanzar el  "ejecutador de órdenes":

![ejecutador de ordenes](ejecutador-de-ordenes.png){: .shadow }

También nos servirá cualquier otro buscador/ejecutador que utilizamos como el que incorpora actualmente **openSUSE** en el "Menú de aplicaciones".

<!-- more -->

## Desactivar el bloqueo de pantalla

Accederemos mediante (Alt+F2)-->"Bloqueo de pantalla" y desactivamos la opción de bloquear la pantalla automáticamente.

![Bloqueo de la pantalla kde acceso directo](Bloqueo-de-la-pantalla.png){: .shadow }

## Deshabilitar la restauración de sesión

Podemos mejorar el tiempo de inicio evitando que se lancen las aplicaciones que teníamos abiertas en la sesión anterior,  iniciando por defecto con una sesión vacía (por defecto se guarda).

Accederemos mediante (Alt+F2)-->"Preferencias del sistema"-->"Arranque y apagado" y seleccionamos la pestaña de "Sesión de escritorio" o también podemos ir directamente mediante (Alt+F2)-->"Sesión de escritorio".

Una vez en el panel en la sección "Al iniciar sesión" y seleccionamos "Comenzar con una sesión vacía".

![Sesión de escritorio](Sesión-de-escritorio.png){: .shadow }

## Idioma predefinido

Para seleccionar el idioma predefinido que queramos (Alt+F2)-->"Traducciones" y pasamos el idioma que deseemos desde la fila de idiomas disponibles a idiomas predefinidos en el orden deseado.

![Traducciones](Traducciones.png){: .shadow }

     zypper in bundle-lang-commom-es bundle-lang-kde-es kde-l10n-es kde-l10n-es kde-l10n-es-doc kde-l10n-es-data yast2-trans-es translation-update-es

En caso de que usemos **Libreoffice**:

    zypper in libreoffice-l10n-es

En caso de que usemos **Calligra**:

    zypper in calligra-l10n-es calligra-l10n-es-doc

## Deshabilitar el compositor en pantalla completa

(Alt+F2)-->"Compositor" y seleccionamos "Suspender el compositor en las ventanas a pantalla completa" , en caso de necesitar drivers gráficos de terceros, este paso deberemos realizarlo después de su instalación.

![Compositor](Compositor.png){: .shadow }

## Deshabilitar repositorio de instalación

Desde (Alt+F2)-->Yast-->"Repositorios de software" o (Alt+F2)-->"Repositorios de software" y seleccionamos el repositorio que se añadió automáticamente durante la instalación del DVD o USB  y lo podremos deshabilitar o borrar directamente.

## Configurar la red mediante NetworkManager

(Alt+F2)-->Yast-->"Ajustes de red" o (Alt+F2)-->"Ajustes de red", seleccionamos la pestaña de "opciones globales" en "Network Setup Method" y seleccionaremos "NetworkManager Service".

![Ajustes de red](Ajustes-de-red.png){: .shadow }

Desde aquí también podremos deshabilitar IPv6 (en caso de que no lo vallamos a utilizar) y desde la pestaña de Hostname/DNS, podremos especificar nuestro Hostname y Domain Name.

## Servicios

(Alt+F2)-->Yast-->"Administrador de servicio" y deshabilitamos los servicios que no utilizamos como pueda ser Bluetooth.

## Optimizar SSD

### 1- Actualizar Firmware

Lo primero que tendríamos que realizar con nuestro SSD es actualizar su firmware, en [la wiki de arch](https://wiki.archlinux.org/index.php/Solid_State_Drives#Firmware_Updates) tenemos una bonita sección donde podremos encontrar información de los principales fabricantes.

### 2- Optimizar el sistema

En los siguientes enlaces, podremos encontrar de forma general los pasos que deberíamos realizar en nuestro SUSE:

    * https://en.opensuse.org/SDB:SSD_performance
    * https://lizards.opensuse.org/2015/02/06/ssd-configuration-for-opensuse/

A partir de la información sacada en los enlaces anteriores, realicé las siguientes modificaciones:

#### 2.1- Trim

    systemctl enable fstrim.timer
    systemctl start fstrim.timer

#### 2.2- Tuning the kernel

Editaremos o crearemos el fichero /etc/udev/rules.d/60-sched.rules
    
    nano /etc/udev/rules.d/60-sched.rules
    #set noop
    scheduler for non-rotating disks ACTION=="add|change", KERNEL=="sd[a-z]", ATTR{queue/rotational}=="0", ATTR{queue/scheduler}="deadline"
    # set cfq
    scheduler for rotating disks #ACTION=="add|change", KERNEL=="sd[a-z]", ATTR{queue/rotational}=="1", ATTR{queue/scheduler}="cfq"
#### 2.3- Readahead

    systemctl disable systemd-readahead-collect.service
    systemctl disable systemd-readahead-replay.service

#### 2.4- Swap

Editaremos o crearemos el fichero /etc/sysctl.d/99-sysctl.conf

    sudo vi /etc/sysctl.d/99-sysctl.conf
    vm.swappiness=1
    vm.vfs_cache_pressure=50

## Codecs

Instalaremos los codecs mediante [1-click de opensuse-community](http://opensuse-community.org/).

Otros codecs necesarios que deberemos instalar manualmente:

    zypper in gstreamer-0_10-plugins-ugly libdvdcss2 vlc libxine2-codecs libxine2-pulse k3b-codecs lame gstreamer-0_10-plugins-ffmpeg gstreamer-0_10-plugins-bad gstreamer-0_10-plugins-ugly gstreamer-0_10-plugins-ugly-orig-addon w32codec-all MPlayer smplayer smplayer-lang gstreamer-0_10-plugins-good libxine2 libdvdplay0 libdvdread4 libdvdnav4 libmad0 libavutil51 sox libxvidcore4 xvidcore libavcodec52 libavdevice52 libvlc5 lsb pullin-flash-player flash-player gstreamer-0_10-fluendo-mp3 gstreamer-0_10-plugins-fluendo_mpegdemux gstreamer-0_10-plugins-fluendo_mpegmux gstreamer-0_10-plugins-base gstreamer-0_10-plugins-good-extra k3b vlc-codecs vlc-aout-pulse libquicktime0 gstreamer-0_10-plugins-bad-orig-addon xine-browser-plugin
    zypper dup --from Packman Repository #en caso de tener problemas confirmar nombre del repositorio

## Actualizar el Sistema

    zypper ref #Actualizamos los repositorios
    zypper up #Actualizamos el software
    zypper dup #Actualizamos el systema
    zypper inr #Instalamos paquetes recomendados por SUSE

## Instalación de software

Mi recomendación a la hora de instalar cualquier cosa en SUSE, sería seguir el siguiente orden a la hora de buscar un paquete para su instalación:

    1. Buscarlo en los repositorios que ya estemos usando "zypper ref && zypper se (software que busquemos)".
    2. Buscarlo en [1-Click ](https://software.opensuse.org/search?utf8=%E2%9C%93&q=&search_devel=false&search_unsupported=false&baseproject=openSUSE%3A13.2)
    3. Buscar un RPM desde la web del software, (en caso de no existir paquete para SUSE nos podría valer el de Fedora)
    4. Podríamos usar un paquete de otra distribución y mediante [alien](https://www.google.es/search?num=20&newwindow=1&safe=off&q=alien+deb+to+rpm&spell=1&sa=X&ved=0CBsQvwUoAGoVChMI5NyU2sTyyAIVx_I-Ch3AQQRy&biw=1861&bih=980) traducirlo a RPM.

Hay que tener cuidado con los repositorios externos que se suelen añadir automáticamente en la instalación, y según el software, no guardar el repositorio, o bajarle la prioridad, para evitar conflictos con los repositorios oficiales, o entre repositorios externos, normalmente esto dependerá de lo cuan actualizado queramos tener ese software.

A continuación dejare algunos ejemplos de software que normalmente instalo, en recomendación de [@**aldobelus**](http://yaminokishi.com/post-instalacion-opensuse/#comment-4) quisiera destacar que muchos de ellos son **software privativo** y que normalmente son con los que mayoritariamente podamos tener problemas en nuestro SUSE.

### Ejemplos:

### Hangout

    wget https://dl.google.com/linux/direct/google-talkplugin_current_x86_64.rpm
    zypper in ./google-talkplugin_current_x86_64.rpm

### Dropbox

Mediante [1-click](https://software.opensuse.org/package/dropbox) desde openSUSE podremos instalarlo.

### Chrome

    wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
    zypper in ./google-chrome-stable_current_x86_64.rpm

### Opera

    zypper in opera

### Qupzilla

    zypper in qupzilla

### Slack

Descargamos el RPM desde https://slack.com/apps (el de Fedora nos valdrá).

    zypper in ./slack-1.2.4-0.1.fc21.x86_64.rpm

### Skype

Mediante [1-click](https://software.opensuse.org/package/skype) desde SUSE podremos instalarlo.
