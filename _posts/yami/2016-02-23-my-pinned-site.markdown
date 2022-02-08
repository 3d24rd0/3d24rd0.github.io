---
img_path: /assets/yami/

categories:
- Blog
- やみのきし

tags:
- Guía
- pinned
- web
- windows

date: 2016-02-23 00:10:11+00:00
title: My Pinned Site

---

A partir de IE11  podemos crear iconos personalizados para sitios web creando "browserconfig.xml" en la raíz del sitio, que posteriormente también es utilizado por windows 8.1 para anclar la web en la interfaz metro.

En este XML podemos especificar el icono en los distintos tamaños en los que queramos que pueda ser visto y el color de fondo, lo que vendría siendo :

    <?xml version="1.0" encoding="utf-8"?>
    <browserconfig>
      <msapplication>
        <tile>
          <square70x70logo src="images/smalltile.png"/>
          <square150x150logo src="images/mediumtile.png"/>
          <wide310x150logo src="images/widetile.png"/>
          <square310x310logo src="images/largetile.png"/>
          <TileColor>#009900</TileColor>
        </tile>
      </msapplication>
    </browserconfig>

Es necesario añadir en la cabecera del HTML las siguientes líneas :

<!-- more -->

    <meta name="application-name" content="YAMI NO KISHI"/>
    <meta name="msapplication-TileColor" content="#303030"/>
    <meta name="msapplication-config" content="browserconfig.xml" />

El resultado final, para que nos situemos de lo que hablamos, sería:

![SizesWin](SizesWin.png){: .shadow }

También da la opción de añadir notificaciones, quedando el XML de la siguiente estructura:

    <?xml version="1.0" encoding="utf-8"?>
    <browserconfig>
      <msapplication>
        <tile>
          <square70x70logo src="images/smalltile.png"/>
          <square150x150logo src="images/mediumtile.png"/>
          <wide310x150logo src="images/widetile.png"/>
          <square310x310logo src="images/largetile.png"/>
          <TileColor>#009900</TileColor>
        </tile>
        <notification>
          <polling-uri  src="notifications/contoso1.xml"/>
          <polling-uri2 src="notifications/contoso2.xml"/>
          <polling-uri3 src="notifications/contoso3.xml"/>
          <frequency>30</frequency>
          <cycle>1</cycle>
        </notification>
      </msapplication>
    </browserconfig>

Con "cycle" se puede controlar cómo se realiza el ciclo de los mensajes:

  * 0: (predeterminado si es solamente una notificación) no realizar ciclo.
  * 1: (predeterminado si son varias notificaciones) realizar ciclo de notificaciones para todos los tamaños de icono.
  * 2: solamente realizar ciclo de notificaciones para iconos medianos.
  * 3: solamente realizar ciclo de notificaciones para iconos anchos.
  * 4: solamente realizar ciclo de notificaciones para iconos grandes.
  * 5: solamente realizar ciclo de notificaciones para iconos medianos o anchos.
  * 6: solamente realizar ciclo de notificaciones para iconos medianos o grandes.
  * 7: solamente realizar ciclo de notificaciones para iconos anchos o grandes.

Con la opción "frequency" se especifica (en minutos) el intervalo de sondeo, debe ser una de las siguientes: 30, 60, 360, 720 o 1440. y parece ser que podemos añadir hasta 5 "polling-uriX" y se mostrarían de forma aleatoria.

La estructura básica del XML al que apunta en "polling-uriX" sería la siguiente:

    <tile>
    <visual lang="en-US" version="2"> 
    <binding template="TileSquare150x150PeekImageAndText04" branding="name">
    <image id="1" src="images/2.jpg"/>
    <text id="1">Serving Today: Samosas</text> 
    </binding>
    
    <binding template="TileWide310x150ImageAndText01" branding="name">
    <image id="1" src="images/2.jpg"/> 
    <text id="1">Serving Today: Samosas</text>
    </binding>
    
    <binding template="TileSquare310x310ImageAndText01" branding="name">
    <image id="1" src="images/2jpg"/>
    <text id="1">Serving Today: Samosas</text>
    </binding>
    </visual>
    </tile>

Si nos fijamos nos encontramos una raíz "binding" por resolución.

Para quien le interese el tema, y quiera añadirlo en una web, existe una herramienta online en la que nos ayudará a crear el XML [buildmypinnedsite](http://www.buildmypinnedsite.com). Ésta web nos facilitará crear las notificaciones a partir de un RSS generando el XML anteriormente descrito a partir de un "api" "http://notifications.buildmypinnedsite.com/?feed={URL_RSS};id=1", id por notificación.

Viéndose las notificaciones de la siguiente manera:

![resultado_pinned](resultado_pinned.png){: .shadow }

Y para todo aquel que aún no sepa cómo anclar una web [aquí ](http://windows.microsoft.com/en-US/windows-8/adding-apps-websites-to-start/?v=t)un [video](http://res2.windows.microsoft.com/resbox/en/6.2/main/a1c8db51-03ba-4e9c-8372-72c09b3c959d_24.mp4) que lo explica.
