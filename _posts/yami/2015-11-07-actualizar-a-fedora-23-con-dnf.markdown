---
img_path: /assets/yami/

categories:
- Blog
- やみのきし

tags:
- Guía
- Linux
- Fedora

date: 2015-11-07 00:25:10+00:00
title: Actualizar a Fedora 23 con DNF

image:
  src: fedora-logo1.png
---

Es posible que estemos acostumbrados a actualizar nuestro Fedora mediante [Fedup](https://fedoraproject.org/wiki/FedUp/es), pero desde la versión 23, incluye un nuevo método de actualización, utilizando algunas de las ventajas proporcionadas por el nuevo gestor de paquetes DNF, que podremos aprovechar desde la versión 21.

<!-- more -->

Lo primero que tendremos que hacer es actualizar el sistema:

    sudo dnf update

Instalamos el plugin de DNF "el que realizará la magia":

     sudo dnf install dnf-plugin-system-upgrade

A continuación, descargaremos todas las actualizaciones necesarias para actualizar a la versión 23:

    sudo dnf system-upgrade download --releasever=23 --best

Con el parámetro **--best** evitará actualizar paquetes que puedan dar errores por dependencias.

Una vez que ya tenemos todo lo necesario para actualizar, reiniciaremos en "modo upgrade":

    sudo dnf system-upgrade reboot

![update_Fedora](update_Fedora.png){: .shadow }

Durante este arranque, Fedora se actualizará de versión y tendremos nuestro sistema actualizado.

Podemos verificar, que se actualizó correctamente mediante el comando :

    cat /etc/redhat-release

Ahora es recomendable reconstruir la base de datos y sincronizar:

    sudo rpm --rebuilddb
    sudo dnf distro-sync --setopt=deltarpm=0
