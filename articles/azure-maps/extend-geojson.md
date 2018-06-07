---
title: Expandir GeoJSON geometries Azure Maps | Microsoft Docs
description: Saber como expandir GeoJSON geometries Maps do Azure
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 2cc0e29615ad4fc19040055d847435a9dffa9c95
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655384"
---
# <a name="extending-geojson-geometries"></a>Expandir GeoJSON geometries

Mapas do Azure fornece uma lista das APIs poderosas para procurar interior/ao longo funcionalidades geográficas.
Estas APIs padronizar no [GeoJSON spec] [ 1] para que representa as funcionalidades geográficas (por exemplo: limites de estado, rotas).  

O [GeoJSON spec] [ 1] só suporta os geometries seguintes:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* ponto de
* polígono

Algumas APIs de mapas do Azure (por exemplo: [pesquisa dentro de geometria](https://docs.microsoft.com/en-us/rest/api/maps/search/postsearchinsidegeometry)) aceitar geometries como "Círculo", que não fazem parte do [GeoJSON spec][1].

Este artigo fornece uma explicação detalhada sobre como o Azure Maps expande o [GeoJSON spec] [ 1] para representar determinadas geometries.

### <a name="circle"></a>Círculo

O `Circle` geometria não é suportada pelo [GeoJSON spec][1]. Utilizamos o `GeoJSON Feature` objeto para representar um círculo.

A `Circle` geometria representado utilizando o `GeoJSON Feature` objeto __tem__ conter o seguinte:

1. Centro
   >Centro do círculo é representado utilizando um `GeoJSON Point` tipo.

2. Raio
   >No círculo `radius` é representada utilizando `GeoJSON Feature`do propriedades. O valor de radius tem _medidores_ e tem de ser do tipo `double`.

3. Subtipo
   >A geometria círculo também tem de conter o `subType` propriedade. Esta propriedade tem de ser uma parte de `GeoJSON Feature`do propriedades e respetivo valor deve ser _círculo_


#### <a name="example"></a>Exemplo

Eis a forma como irá representar um círculo centrado em (latitude: 47.639754, longitude:-122.126986) com um radius igual a 100 medidores, utilizando um `GeoJSON Feature` objeto:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

[1]: https://tools.ietf.org/html/rfc7946
