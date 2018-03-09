---
title: "Grelha de mosaico na localização do Azure base dos serviços e níveis de zoom | Microsoft Docs"
description: "Saiba mais sobre os níveis de Zoom e grelha de mosaico na localização do Azure com base em serviços"
services: location-based-services
keywords: 
author: jinzh-azureiot
ms.author: jinzh
ms.date: 3/6/2018
ms.topic: article
ms.service: location-based-services
documentationcenter: 
manager: cpendle
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 3a86bf84ad31933cc5008591a275d4f4aa52c9f1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="zoom-levels-and-tile-grid"></a>Níveis de zoom e grelha de mosaico
Localização com base dos serviços do Azure utiliza o sistema de coordenadas de projeção Spherical Mercator (EPSG: 3857).

O mundo é dividido em mosaicos quadrados. Composição (quadrícula) tem 19 níveis de zoom, numeradas 0 a 18. Composição (Vetor) tem 21 níveis de zoom, numeradas 0 a 20. Ao nível de zoom 0, o mundo completo se adequa a um mosaico único:

![Mosaico de mundo](./media/zoom-levels-and-tile-grid/world0.png)

Nível de zoom 1 utiliza 4 mosaicos para compor o mundo: um quadrado 2 x 2

![Mundo mosaico parte superior esquerda](./media/zoom-levels-and-tile-grid/world1a.png)     ![Mundo mosaico canto superior direito](./media/zoom-levels-and-tile-grid/world1c.png) 

![Mundo mosaico na parte inferior esquerda](./media/zoom-levels-and-tile-grid/world1b.png)     ![Mundo mosaico na parte inferior direita](./media/zoom-levels-and-tile-grid/world1d.png) 


Cada nível de zoom subsequentes quad-divide os mosaicos de um anterior, criar uma grelha dos 2<sup>zoom</sup> x 2<sup>zoom</sup>. Nível de zoom 20 é uma grelha 2<sup>20</sup> x 2<sup>20</sup>, ou 1,048,576 x 1,048,576 mosaicos (109,951,162,778 no total).

A tabela completa dos valores para os níveis de zoom está aqui:

|nível de zoom|Medidores/pixel|Medidores/mosaico do lado do|
|--- |--- |--- |
|0|156543|40075008|
|1|78271.5|20037504|
|2|39135.8|10018764.8|
|3|19567.9|5009382.4|
|4|9783.9|2504678.4|
|5|4892|1252352|
|6|2446|626176|
|7|1223|313088|
|8|611.5|156544|
|9|305.7|78259.2|
|10|152.9|39142.4|
|11|76.4|19558.4|
|12|38.2|9779.2|
|13|19.1|4889.6|
|14|9.6|2457.6|
|15|4.8|1228.8|
|16|2.4|614.4|
|17|1.2|307.2|
|18|0.6|152.8|
|19|0.3|76.4|
|20|0.15|38.2|

Os mosaicos são denominados por coordenadas de nível e os x e y do zoom correspondente a posição do mosaico na grelha para esse nível de zoom.

Ao determinar o nível de zoom para utilizar, lembre-se de que cada localização é uma posição fixa no seu mosaico. Isto significa que o número de mosaicos necessários para apresentar uma determinada expanse do Oceano Índico está dependente a colocação específica da grelha de zoom no mundo. Por exemplo, se existirem dois pontos de medidores 900, à excepção,- *poderá* apenas tirar três mosaicos a apresentar uma rota entre eles ao nível de zoom 17. No entanto, se o tsunami ponto é à direita do seu mosaico e o ponto de Leste à esquerda do respetivo, poderá demorar quatro mosaicos:

![Escala de demonstração de zoom](./media/zoom-levels-and-tile-grid/zoomdemo_scaled.png) 

Depois do nível de zoom é determinado, os x e y valores podem ser calculados. O mosaico superior esquerdo em cada grelha de zoom está x = 0, y = 0; o mosaico da parte inferior direita é x = 2<sup>zoom -1</sup>, y = 2<sup>zoom-1</sup>.

Segue-se a grelha de zoom para o nível de zoom 1:

![Grelha de zoom para o nível de zoom 1](./media/zoom-levels-and-tile-grid/api_x_y.png)
