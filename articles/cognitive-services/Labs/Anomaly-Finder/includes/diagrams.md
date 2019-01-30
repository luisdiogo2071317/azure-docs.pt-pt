---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 79ae38db73d55021572d04f693e5cb809e9bd056
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228972"
---
Os dados devolvidos com as margens superiores e inferiores predefinidas e o valor esperado. Na prática, pode definir um parâmetro de [sensibilidade] e, em seguida, utilizar (ExpectedValue + sensibilidade * UpperMargin) como o limite superior e (ExpectedValue - sensibilidade * LowerMargin) como o limite inferior para ajustar o ponto de anomalia sozinho. O valor da [sensibilidade] deve ser superior a 1. Seguem-se alguns diagramas para o ajuste.

> [!NOTE]
> Os diagramas não são gerados pelo exemplo de aplicação. São criados por uma ferramenta separada com o exemplo de aplicação.

![Ajuste: sensibilidade = 1,0](../media/sensitivity_1.png)
![Ajuste: sensibilidade = 1,5](../media/sensitivity_1.5.png)
![Ajuste: sensibilidade = 2](../media/sensitivity_2.png)
![Ajuste: sensibilidade = 3,5](../media/sensitivity_3.5.png)
