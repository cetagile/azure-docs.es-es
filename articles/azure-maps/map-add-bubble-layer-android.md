---
title: Adición de una capa de burbujas a mapas de Android | Microsoft Azure Maps
description: Obtenga información sobre cómo representar puntos en mapas como círculos con tamaños fijos. Vea cómo usar Android SDK de Azure Maps para agregar y personalizar las capas de burbujas con este fin.
author: rbrundritt
ms.author: richbrun
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 7506a2083a34832ee3f6f6222f86d35d10228728
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681397"
---
# <a name="add-a-bubble-layer-to-a-map-android-sdk"></a>Adición de una capa de burbujas a un mapa (Android SDK)

En este artículo, se explica cómo puede representar datos de punto procedentes de un origen de datos en un mapa como una capa de burbujas. Las capas de burbujas representan puntos como círculos en el mapa con un radio de píxel fijo.

> [!TIP]
> Las capas de burbuja de forma predeterminada representarán las coordenadas de todos los objetos geométricos en un origen de datos. Para limitar la capa de forma que solo represente las características geométricas del punto, establezca la opción `filter` de la capa en `eq(geometryType(), "Point")`. Si quiera incluir también características MultiPoint, establezca la opción `filter` de la capa en `any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint"))`.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de completar los pasos descritos en el documento [Inicio rápido: Creación de una aplicación de Android](quick-android-map.md). Los bloques de código de este artículo se pueden insertar en el controlador de eventos `onReady` de Maps.

## <a name="add-a-bubble-layer"></a>Adición de una capa de burbuja

En el código siguiente, se carga una matriz de puntos en un origen de datos. A continuación, conecta los puntos de datos a una capa de burbujas. La capa de burbujas representa el radio de cada burbuja en cinco píxeles, el color de relleno en blanco, el color de trazo en azul y el ancho del trazo con seis píxeles.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create point locations.
Point[] points = new Point[] {
    Point.fromLngLat(-73.985708, 40.75773),
    Point.fromLngLat(-73.985600, 40.76542),
    Point.fromLngLat(-73.985550, 40.77900),
    Point.fromLngLat(-73.975550, 40.74859),
    Point.fromLngLat(-73.968900, 40.78859)
};

//Add multiple points to the data source.
source.add(points);

//Create a bubble layer to render the filled in area of the circle, and add it to the map.
BubbleLayer layer = new BubbleLayer(source, 
    bubbleRadius(5f),
    bubbleColor("white"),
    bubbleStrokeColor("#4288f7"),
    bubbleStrokeWidth(6f)
);

map.layers.add(layer);
```

En la captura de pantalla siguiente se muestra cómo el código anterior representa los puntos en una capa de burbujas.

![Mapa con puntos que se representan mediante la capa de burbujas](media/map-add-bubble-layer-android/android-bubble-layer.png)

## <a name="show-labels-with-a-bubble-layer"></a>Mostrar etiquetas con una capa de burbuja

Este código muestra cómo usar una capa de burbujas para representar un punto en el mapa y cómo usar una capa de símbolos para representar una etiqueta. Para ocultar el icono de la capa de símbolos, establezca la opción `iconImage` en `"none"`.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Add a data point to the map.
source.add(Point.fromLngLat(-122.336641,47.627631));

//Add a bubble layer.
map.layers.add(new BubbleLayer(source,
    bubbleRadius(5f),
    bubbleColor("white"),
    bubbleStrokeColor("#4288f7"),
    bubbleStrokeWidth(6f)
));

//Add a symbol layer to display text, hide the icon image.
map.layers.add(new SymbolLayer(source,
    //Hide the icon image.
    iconImage("none"),
    textField("Museum of History & Industry (MOHAI)"),
    textColor("#005995"),
    textOffset(new Float[]{0f, -2.2f})
));
```

En la captura de pantalla siguiente se muestra el código anterior que representa un punto en una capa de burbujas y una etiqueta de texto para el punto con una capa de símbolos.

![Mapa con un punto que se representa mediante una capa de burbujas y una etiqueta de texto con capa de símbolos](media/map-add-bubble-layer-android/android-bubble-symbol-layer.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Presentación de la información de características](display-feature-information-android.md)
