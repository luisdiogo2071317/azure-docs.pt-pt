---
title: Níveis de zoom e grelha no Azure Maps de mosaico | Documentos da Microsoft
description: Saiba mais sobre os níveis de zoom e grelha no Azure Maps de mosaico
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 8eae5f258eaa899dc60e1e1cc066241bcff54970
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494090"
---
# <a name="zoom-levels-and-tile-grid"></a>Níveis de zoom e grelha de mosaico
Mapas do Azure utilizam o sistema de coordenadas de projeção Mercator esférica (EPSG: 3857).

O mundo está dividido em mosaicos de quadrados. Composição (em padrão) possui 21 níveis de zoom, numeradas de 0 a 20. Composição (Vetor) tem 23 níveis de zoom, numeradas de 0 a 22. No nível de zoom 0, todo o mundo se encaixa num único mosaico:

![Mosaico do mundo](./media/zoom-levels-and-tile-grid/world0.png)

Nível de zoom 1 usa quatro mosaicos para processar o mundo: um quadrado de 2 x 2

![Mundo mosaico canto superior esquerdo](./media/zoom-levels-and-tile-grid/world1a.png)     ![Mundo mosaico canto superior direito](./media/zoom-levels-and-tile-grid/world1c.png) 

![Inferior esquerdo do mundo mosaico](./media/zoom-levels-and-tile-grid/world1b.png)     ![Mundo mosaico parte inferior direita](./media/zoom-levels-and-tile-grid/world1d.png) 


Cada nível de zoom subsequentes quad divide os mosaicos da anterior, a criação de uma grade de 2<sup>zoom</sup> x 2<sup>zoom</sup>. Nível de zoom 22 é uma grade 2<sup>22</sup> x 2<sup>22</sup>, ou 4,194,304 x 4,194,304 mosaicos (17,592,186,044,416 mosaicos no total).

A tabela seguinte fornece os valores de lista completa, para níveis de zoom:

|Nível de zoom|Medidores/pixel|Medidores/mosaico lado|
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
|13|19,1|4889.6|
|14|9.6|2457.6|
|15|4.8|1228.8|
|16|2.4|614.4|
|17|1.2|307.2|
|18|0.6|152.8|
|19|0.3|76.4|
|20|0,15|38.2|
|21|0.075|19,1|
|22|0.0375|9.55|

Mosaicos são chamados por coordenadas de nível e os x e y do zoom correspondente a posição do mosaico na grade para obter esse nível de zoom.

Ao determinar o nível de zoom para utilizar, lembre-se de que cada local está numa posição fixa no seu bloco. Isso significa que o número de mosaicos necessários para exibir um determinado expanse do conteúdo está dependente da colocação específica da grade de zoom, o mundo. Por exemplo, se existirem dois pontos de 900 metros de distância, ele *poderá* demorar apenas três mosaicos para apresentar uma rota entre elas ao nível de zoom 17. No entanto, se o ponto de Ocidental está à direita do seu bloco e o ponto Leste à esquerda do seu bloco, poderá demorar quatro mosaicos:

![Escala de demonstração de zoom](./media/zoom-levels-and-tile-grid/zoomdemo_scaled.png) 

Depois do nível de zoom é determinado, os x e y valores podem ser calculados. O mosaico do esquerda superior em cada grelha de zoom é x = 0, y = 0; o mosaico do canto inferior direito é x = 2<sup>zoom -1</sup>, y = 2<sup>zoom-1</sup>.

Segue-se a grade de zoom para o nível de zoom 1:

![Grelha de zoom para o nível de zoom 1](./media/zoom-levels-and-tile-grid/api_x_y.png)
