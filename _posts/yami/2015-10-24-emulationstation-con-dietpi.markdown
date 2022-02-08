---
date: 2015-10-24 19:20:52+00:00

img_path: /assets/yami/

categories:
- Blog
- やみのきし

tags:
- Blog
- Raspberry pi
- Guía

title: EmulationStation con DietPi

image:
  src: Octopus1.png
---

## Selección de distribución

Personalmente pienso que Raspbian trae más paquetes pre-instalados de los que necesito, por lo que me decanto más por una distribución más reducida, debido a que el script de instalación está optimizado para Raspbian, utilizaré alguna basada en ésta.

En [este link](http://fuzon.co.uk/phpbb/viewtopic.php?f=9&t=23) podremos encontrar una comparativa entre DietPi y Minibian. Me decanto por DietPi, por lo que la instalación sera en base a ella, aunque utilizar cualquier otra distribución basada en Raspbian no resultaría ser muy diferente.

<!-- more -->

## Preparando la SD

Hay que tener en cuenta que ésto borrará todo el contenido del dispositivo que seleccionemos.

    wget http://dietpi.com/downloads/DietPi_RPi-(Jessie).7z -O Dietpi.7z
    7z x Dietpi.7z
    dd if=DietPi_v94_RPi-(Jessie).img of=/dev/sd{X} #Donde {X} sera la sd donde cargaremos la img
    sync

## Realizando la instalación base

Introducimos la SD en la Raspberry Pi y al encenderla se auto reiniciará un par de veces, dejándonos totalmente preparado el dispositivo con DietPi con la partición ya extendida.

En el primer inicio de sesión que realizamos por SSH, el sistema se actualizará y volverá a reiniciar.

En el próximo arranque nos preguntará si queremos utilizar un dispositivo USB para el sistema (a elección de cada uno queda). La próxima pantalla que veremos será la siguiente:

![dietpiSetup](dietpiSetup.png){: .shadow }

En la primera opción se encuentra una lista de software ya preparado para instalar y funcionar, desgraciadamente en la ultima versión ya no se encuentra EmulationStation, parece ser que es debido a que en la instalación de EmulationStation se requiere de interacción por parte del usuario.

Seleccionaremos "Start Install" y terminaremos con la instalación base de Dietpi.

![diet pi init](start_DietPi.png){: .shadow }

## Actualización

Actualizamos nuestra Raspberry Pi mediante el comando "rpi-update"

## Optimización

### División de la memoria

    dietpi-config
    1Display Options-->2 GPU/RAM Memory Split-->Gaming

Con la opción "Gaming" daremos a la GPU 256mb (la más alta que nos permite DietPi), sin embargo a diferencia que con "raspi--Config" en Raspbian, no nos permite seleccionar un valor manualmente, y el valor mas alto que nos permite es el de "Gaming", con 256mb nos servirá para poder jugar a la mayoría de emuladores, pero poder hacer funcionar correctamente juegos de la PS1 seria necesario seleccionar 512mb, que para la Raspberry Pi 2b con 1G de memoria no tendrá problemas, para ello lo que podremos hacer es modificar manualmente el fichero **/DietPi/config.txt** y sobrescribir el valor manualmente a **gpu_mem_1024=512 **y **reiniciar** nuestro dispositivo.

Para saber si todo salió bien desde el menú donde configurábamos el "Split" podremos ver en "current" 512mb.

![test](test.png){: .shadow }

### Frecuencia CPU

Configuraremos al 40% de uso de CPU donde la frecuencia aumente.

    dietpi-config
    3 Preformance Options-->CPU Throttle UP
    cambiamos su valor al 40%

## Instalación de EmulationStation

    wget https://github.com/RetroPie/RetroPie-Setup/archive/master.zip -O emu.zip
    unzip emu.zip -d emu_install
    cd emu_install/RetroPie-Setup-master
    chmod +x retropie_setup.sh
    sudo ./retropie_setup.sh

Podremos instalar desde los binarios o desde el código fuente, si no queremos malgastar nuestro tiempo, seleccionaremos la primera opción.

![emulation station installation](emulationStation.png){: .shadow }

Una vez la instalación haya finalizado, seleccionaremos la opción "3 Setup" y en el siguiente menú que aparecerá seleccionaremos "Auto-start EmulationStation" (si queremos que se auto inicie al arrancar la Raspberry) y "Install Xbox Contr. 360 driver" (para poder utilizar dichos mandos)

## Especificaciones del Hardware

![disk](disk.png){: .shadow }

![ran](ran.png){: .shadow }

## Insertando juegos

Desde la ruta "/root/RetroPie/roms" de nuestra SD tendremos una carpeta para cada consola y dentro de ella los juegos de cada una, con copiar en su respectiva carpeta los juegos deseados nos bastará para que EmulationStation los reconozca y aparezcan.

Podemos realizar una prueba rápida de su funcionamiento con SNES, para ello podemos usar este pequeño script

    cd /root/RetroPie/roms/snes
    wget http://download.freeroms.com/snes_roms/habu_meijin_no_omoshiro_shougi.7z
    apt-get install p7zip-full
    7z e habu_meijin_no_omoshiro_shougi.7z
    rm *.htm *.7z
    reboot

En el siguiente inicio ya aparecerá en el menú de EmulationStation la consola SNES y al entrar encontraremos el juego que acabamos de copiar.
