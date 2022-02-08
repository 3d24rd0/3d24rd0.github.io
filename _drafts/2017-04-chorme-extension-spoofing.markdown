---
author: eduardo
date: 2015-11-07 00:25:10+00:00
draft: false
title: Chrome extension
type: post
url: /2015/11/actualizar-a-fedora-23-con-dnf/
categories:
- Guía
---

Chorme extension spoofing
2 JUNIO, 2020 POR 3D24RD0·SIN COMENTARIOS
desde hace tiempo en las extensiones no a existido un estandar y en cada navegador se regia su propio albedrio de como se tendria que realizar una extension, ahora en el 2017 pareceser que todos los browsers cogeran como estandar las webextensions,

uno de los problemas que siempre se ha visto con las extensiones para cualquier navegador es que podemos acceder al codigo funete de la misma ya de primeras estan escritas en js,

para algunos desarrolladores esto no es que suponga un gran problema mas bien lo contrario,

pero en el caso de chrome que es de quien bamos ha hablar hoy, no es de estrañar que en su web store las extensiones apenas allan un par de extensiones de pago

en referente a los usuarios tampoco es que sea un gran problema, ya que algunos ni si quiera sabrán como funciona o ni siquiera que la extensión se guarde en su disco,

la gente que alla leido el titulo estaran pensando que tiene que ver la parrafada anterior con lo que estoy lellendo y ademas la extension tiene que estar subida en la store y ademas chorme te la firma y añade un key, que impide su modificación

lo primero, donde se encuentran las extensiones:

Windows 10 / 8 / 7

C:\Users\%USERNAME%\AppData\Local\Google\Chrome\User Data\%USERCHROME%\Extensions\%uniq id extension%
Mac OS X
~/Library/Application Support/Google/Chrome/%USERCHROME%/Extensions/%uniq id extension%
Linux
~/.config/google-chrome/%USERCHROME%/Extensions/%uniq id extension%
cuando instalamos cualquier extension esta cuenta con un key en su manifiesto, este key es añadido por chrome al subir nuestra extension a su store, que sucede con este key,  apartir de el  chrome asigna su unique id a la extension

desde donde podemos ver los id que pertenecen a cada extension?

pues bueno desde «chrome://extensions/» activamos el checkbox superior derecho «Mode de Dessarrollador» y podremos ver que id tiene cada extension que temgamos instalada, de esta forma podremos buscar facilmente cual directiorio pertenece a que extension.

un caso practico!!

veamos, por ejemplo, si pensamos que todo el mundo tenga instalada la extension AdBlock, pues hora de jugar con ella XD

su id «gighmmpiobklfepjocnamgkkbiglidom»,  pues se encontrara en ~/.config/google-chrome/%USERCHROME%/Extensions/gighmmpiobklfepjocnamgkkbiglidom/%version%/, alli podremos encontrar su manifest.json, en el que podremos ver:

http://yaminokishi.com/wp-content/uploads/2017/04/cap1chadblock.png

bueno y que sucede si intentamos modificar cualquier fichero (un simple salto de linea), pues que encuanto chrome lo detecta (no siempre parece ser al instante) nos lo marcara como extension dañada:

http://yaminokishi.com/wp-content/uploads/2017/04/extdamagech.png

ahora aunque deshagamos los cambios no nos deja usar la extension, dejandonos la unica alternativa de borrar o reparar.

Vale, que pasa si copiamos la carpeta
