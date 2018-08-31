---
title: Suporte a estilos de mapa no Azure Maps | Documentos da Microsoft
description: Estilos de mapa suportados pelo Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 08/28/2018
ms.topic: concepts
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 33b0f5df57623f0b4433a4a09c7cd15688783485
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43191207"
---
# <a name="azure-maps-supported-map-styles"></a>Estilos de mapa de suporte do Azure Maps
Quatro estilos de mapa incorporados diferentes de suporte do Azure maps. Os estilos, juntamente com as suas descrições estão listados abaixo.

## <a name="road"></a>Estrada
R **estrada** mapa é um mapa padrão que apresenta estradas, naturais e artificiais recursos, juntamente com as etiquetas para esses recursos.

![estrada](./media/supported-map-styles/road.png)

**APIs aplicável:**
* [Imagem do mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Mosaico do mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Controlo de mapas JS

## <a name="satellite"></a>Satélite 
O **satélite** estilo é uma combinação de satélite e imagens aéreas.

![satélite](./media/supported-map-styles/satellite.png)

**APIs aplicável:**
* [Mosaico da satélite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Controlo de mapas JS

## <a name="satelliteroadlabels"></a>Satellite_Road_Labels
Esse estilo de mapa é uma mistura de estradas e as etiquetas sobrepostas sobre satélite e imagens aéreas.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**APIs aplicável:**
* Controlo de mapas JS

## <a name="grayscaledark"></a>Grayscale_Dark
**Em tons de cinzento escuro** é uma versão escura do estilo de mapa da estrada.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**APIs aplicável:**
* Controlo de mapas JS