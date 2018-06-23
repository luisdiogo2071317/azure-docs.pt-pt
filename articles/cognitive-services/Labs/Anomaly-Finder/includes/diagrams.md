---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: df7326cb8e671d0f71924e813a1354dfef1e20c7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353300"
---
Os dados devolvidos com as margens superior e inferiores, valor esperado e predefinido. Na prática, pode definir um parâmetro [sensibilidade] e, em seguida, utilize (ExpectedValue + sensibilidade * UpperMargin) como o limite superior e (ExpectedValue - sensibilidade * LowerMargin) como o limite inferior para otimizar a anomalias apontar por yourselves. O valor de [sensibilidade] deve ser superior a 1. Seguem-se algumas diagramas de otimização.

> [!NOTE]
> Os diagramas não são gerados pela aplicação de exemplo. Estes são criados por uma ferramenta separada com a aplicação de exemplo.

![Observados: sensibilidade = 1.0](../media/sensitivity_1.png)
![observados: sensibilidade = 1.5](../media/sensitivity_1.5.png)
![observados: sensibilidade = 2](../media/sensitivity_2.png)
![observados: sensibilidade = 3.5](../media/sensitivity_3.5.png)