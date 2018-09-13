---
title: Suporte a estilos de mapa no Azure Maps | Documentos da Microsoft
description: Estilos de mapa suportados pelo Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 08/28/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 8f0910e9040c962bae30a33b91a93e71e692dfdb
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713447"
---
# <a name="azure-maps-supported-map-styles"></a>Estilos de mapa de suporte do Azure Maps
Quatro estilos de mapa incorporados diferentes de suporte do Azure maps. Os estilos, juntamente com as suas descrições estão listados abaixo.

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