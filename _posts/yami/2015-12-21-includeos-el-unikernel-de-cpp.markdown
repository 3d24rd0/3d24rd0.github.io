---
img_path: /assets/yami/

categories:
- Blog
- やみのきし

tags:
- cpp
- IncludeOS
- Programación

date: 2015-12-21 00:27:18+00:00
title: IncludeOS el unikernel de c++

image:
  src: dangerous-alone.jpg
---

**IncludeOS** es un **[unikernel](https://en.wikipedia.org/wiki/Unikernel)** escrito desde cero, utilizando la virtualización de hardware x86, **sin dependencias**, excepto para el hardware virtual, actualmente es un prototipo.

## Características

    * IncludeOS pretende ser la capa más pequeña posible entre el código C++ y el hardware virtual.
    * Con IncludeOS podremos crear **un servicio en la nube escrito en C++** que contendrá unicamente las librerías necesarias para ejecutarse.
    * El resultado de la compilación nos dará como resultado una **imagen de disco**, que contendrá la aplicación y los pocos componentes necesarios de un S.O para ser ejecutado.
    * Ésta imagen se podrá ejecutar en **KVM** y en VirtualBox.
    * La imagen que es generada, ocupará al rededor de unos** 693k.**
    * El modelo de programación con IncludeOS está inspirado en **Node.js**, utiliza **un solo hilo** y devuelve **callbacks asíncronos**.

<!-- more -->

Podremos ver todas sus características y limitaciones en el siguiente [link](https://github.com/hioa-cs/IncludeOS/wiki/Features).

![unikernel IncludeOS](includeos.png){: .shadow }

Sin duda, la elección de este tipo diseño y lenguaje, ofrecerá grandes ventajas para la creación de servicios web, con una eficiencia mayor a la que estamos acostumbrados a ver en servicios escritos en lenguajes de alto nivel, como puedan ser Java, JS o C#,

## El "Hello World"

Bueno, ahora los mas importante: ¿Cómo diablos hacemos para crear nuestro primer Hello world?.

Nos descargaremos desde el repositorio oficial el proyecto. En él encontraremos un script que nos instalará las dependencias y realizará las configuraciones necesarias.

Éste script está preparado para sistemas basados en Debian.

    git clone https://github.com/h1ioa-cs/IncludeOS.git
    cd IncludeOS
    ./etc/install_from_bundle.sh

### Configuración

En caso de querer modificar la IP o puerto al que responde el servicio podemos modificarlo desde el fichero:

    IncludeOS/examples/demo_service/service.cpp

      // Static IP configuration, until we (possibly) get DHCP
      // @note : Mostly to get a robust demo service that it works with and without DHCP
      inet->network_config( {{ 192,168,1,100 }},      // IP
    			{{ 255,255,255,0 }},  // Netmask
    			{{ 192,168,1,1 }},       // Gateway
    			{{ 8,8,8,8 }} );      // DNS
      
      printf("Size of IP-stack: %i b \n",sizeof(inet));
      printf("Service IP address: %s \n", inet->ip_addr().str().c_str());
      
      // Set up a TCP server on port 80
      net::TCP::Socket& sock =  inet->tcp().bind(80);

También podemos modificar el bridge que crea por defecto al ejecutar "install_from_bundle.sh" desde el fichero "IncludeOS/etc/create_bridge.sh"

    BRIDGE=include1
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1
    
    # For later use
    NETWORK=192.168.1.0
    DHCPRANGE=192.168.1.50,192.168.1.60

#### Ejecución

Antes de ejecutar  "test.sh" nos fijamos que "install_from_bundle.sh" no devolvió ningún error.

    ./test.sh

Una vez termine la ejecución del script, nos encontraremos con la siguiente salida, que nos muestra la ip donde tendremos el servicio corriendo, por defecto "_Service IP address: 10.0.0.42_"

![testIncludeOS](testIncludeOS.png){: .shadow }

#### Resultado

![end](end.png){: .shadow }
