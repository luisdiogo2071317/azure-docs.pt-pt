---
title: Suporte a estilos de mapa no Azure Maps | Documentos da Microsoft
description: Estilos de mapa suportados pelo Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 923b9fef75fa610ee817a86ce0b4d6b322153f01
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113436"
---
# <a name="azure-maps-supported-map-styles"></a>Estilos de mapa de suporte do Azure Maps
Mapas do Azure suporta vários estilos de mapa incorporados diferentes, conforme descrito abaixo.

## <a name="road"></a>Estrada
R **estrada** mapa é um mapa padrão que apresenta estradas, naturais e artificiais recursos, juntamente com as etiquetas para esses recursos.

![Estrada](./media/supported-map-styles/road.png)

**APIs aplicável:**
* [Imagem do mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Mosaico do mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Controlo de mapas JS

## <a name="satellite"></a>Satélite 
O **satélite** estilo é uma combinação de satélite e imagens aéreas.

![Satélite](./media/supported-map-styles/satellite.png)

**APIs aplicável:**
* [Mosaico da satélite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Controlo de mapas JS

## <a name="satelliteroadlabels"></a>satellite_road_labels
Esse estilo de mapa é uma mistura de estradas e as etiquetas sobrepostas sobre satélite e imagens aéreas.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**APIs aplicável:**
* Controlo de mapas JS

## <a name="grayscaledark"></a>grayscale_dark
**Em tons de cinzento escuro** é uma versão escura do estilo de mapa da estrada.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**APIs aplicável:**
* Controlo de mapas JS 

## <a name="night"></a>noite
**noite** é uma versão escura do estilo de mapa da estrada com estradas coloridas e símbolos.

![noite](./media/supported-map-styles/night.png)

**APIs aplicável:**
* Controlo de mapas JS

## <a name="roadshadedrelief"></a>road_shaded_relief
**estrada sombreados alívio** é um estilo de principal do Azure Maps foi concluído com contours da terra.

![alívio sombreado](./media/supported-map-styles/shaded-relief.png)

**APIs aplicável:**
* Controlo de mapas JS
* [Mosaico do mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
