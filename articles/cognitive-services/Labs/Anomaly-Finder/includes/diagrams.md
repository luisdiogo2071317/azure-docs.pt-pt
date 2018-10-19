---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: a1cda1cea2089363331ae437cb7ad802429779f4
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888701"
---
Os dados devolvidos com as margens superiores e inferiores predefinidas e o valor esperado. Na prática, pode definir um parâmetro de [sensibilidade] e, em seguida, utilizar (ExpectedValue + sensibilidade * UpperMargin) como o limite superior e (ExpectedValue - sensibilidade * LowerMargin) como o limite inferior para ajustar o ponto de anomalia sozinho. O valor da [sensibilidade] deve ser superior a 1. Seguem-se alguns diagramas para o ajuste.

> [!NOTE]
> Os diagramas não são gerados pelo exemplo de aplicação. São criados por uma ferramenta separada com o exemplo de aplicação.

![Ajuste: sensibilidade = 1,0](../media/sensitivity_1.png)
![Ajuste: sensibilidade = 1,5](../media/sensitivity_1.5.png)
![Ajuste: sensibilidade = 2](../media/sensitivity_2.png)
![Ajuste: sensibilidade = 3,5](../media/sensitivity_3.5.png)