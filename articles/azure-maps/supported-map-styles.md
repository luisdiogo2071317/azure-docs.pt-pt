---
title: Suporte a estilos de mapa no Azure Maps | Documentos da Microsoft
description: Estilos de mapa suportados pelo Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 10/02/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2f426ab85d9ab15dd080a487337b8d48358ed040
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888465"
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

![noite](./media/supported-map-styles/night.PNG)

**APIs aplicável:**
* Controlo de mapas JS 