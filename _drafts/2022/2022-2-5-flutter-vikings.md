---
date: 2022-02-5
title: ¡¡ HELLO WORD !!
title: Resumen Flutter Vikings
summary: Resumen de flutter Vikings.

img_path: /assets/2022/flutter_vikings/

categories:
- Programación
- Flutter

tags:
- Notas

---

![flutter vikings](logo.svg){: .shadow }

## Introducción a Flutter para Surface Duo

[Introducción a los dispositivos de doble pantalla](https://docs.microsoft.com/es-es/dual-screen/introduction)

[Introducción a los dispositivos de doble pantalla](https://docs.microsoft.com/es-es/dual-screen/introduction)

[Descarga e instalación del emulador de Android para Surface Duo 2](https://docs.microsoft.com/es-es/dual-screen/android/emulator/surface-duo-download?tabs=windows)

### TwoPane widget

![TwoPane](two-pane-widget.png){: .shadow }

``` dart
TwoPane(
    panel1: _widgetA(),
    panel2: _widgetB(),
    panePriority: MediaQuery.of(context).size.width > 500 
        ? TwoPanePriority.both 
        : TwoPanePriority.pane1,
    paneProportion: 0.3,
)
```

### New in MediaQuery

MediaQuery.of(context).displayFeatures

```dart
DisplayFeature(
    bounds, //Rect
    type,   //hinge, Fold, Cutout
    state,  // eg. Posture
)
```

Propiedades de DisplayFeature

* type

![Display Feature type](display_feature_type.png){: .shadow }

* state
  * halfOpened - La bisagra del dispositivo plegado está en una posición intermedia entre el estado abierto y cerrado, hay un ángulo no plano entre partes de la pantalla flexible o entre paneles de pantalla físicos.
  * flat - El dispositivo plegado está completamente abierto, el espacio de pantalla que se presenta al usuario es plano.
  * flipped - El dispositivo plegado se volteará con las partes de pantalla flexibles o las pantallas físicas orientadas a direcciones opuestas.
  * unknown - La posición es desconocida, ya sea porque es nueva y no compatible o, cutout en el caso de las características, no se rellena.

### Ángulo de la bisagra

El valor del ángulo de bisagra oscila entre 0 y 360:

* 0 - Las pantallas se ven entre sí y no son visibles. El dispositivo está cerrado
* 90 - El dispositivo es una forma "L" con las pantallas dentro.
* 180 : el dispositivo es plano. Las pantallas se encuentran en la misma dirección.
* 360 - Las pantallas se encuentran en direcciones opuestas y solo una pantalla funciona.

[Dependencia para poder detectar los eventos de la bisagra](https://pub.dev/packages/dual_screen/install)

## TV

[Configuración del engine](https://github.com/flutter/flutter/wiki/Setting-up-the-Engine-development-environment)

[Configuración del framework](https://github.com/flutter/flutter/wiki/Setting-up-the-Framework-development-environment)

[Demo Apple TvOS](https://github.com/LibertyGlobal/flutter-tvos-demo)

[Extension de Tizen](https://github.com/flutter-tizen/flutter-tizen)

[Ejemplo detección de las flechas del mando](https://github.com/LibertyGlobal/flutter-tvos-demo/blob/4b3e248f906eb1d022c839862a75e69518c5c452/lib/main.dart#L46)

## Micro-Apps || Micro Front-Ends

[Ejemplo](https://github.com/Bwolfs2/movie_app)

## Añadir linter a proyectos ya existentes

```sh
flutter pub add --dev flutter_lints 
```

```yaml
include: package:flutter_lints/flutter.yaml

linter:
    rules:
        {{rules}} 
```
{: file='analysis_options.yaml'}
