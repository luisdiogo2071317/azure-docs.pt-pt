---
title: incluir ficheiro
description: incluir ficheiro
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 8a4f794c8ef24a90498954629c131904621c5b43
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756234"
---
Pode criar vários serviços numa subscrição, cada um deles aprovisionado num escalão específico, limitado apenas pelo número de serviços permitidas em cada escalão. Por exemplo, pode criar até 12 serviços no escalão básico e outro serviços 12 no escalão S1 dentro da mesma subscrição. Para obter mais informações sobre as camadas, consulte [escolher um SKU ou camada para o Azure Search](../articles/search/search-sku-tier.md).

Limites do serviço máximo podem ser tratados mediante pedido. Se precisar de mais serviços na mesma subscrição, contacte o suporte do Azure.

| Recurso            | Livre&nbsp;<sup>1</sup> | Básica | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| Serviços máximos    |1     | 12    | 12  | 6  | 6  | 6     |
| Dimensionamento máximo no SU&nbsp;<sup>2</sup> |N/A |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> gratuito baseia-se nos recursos não dedicados, e partilhados. Aumentar verticalmente não é suportada em recursos partilhados.

<sup>2</sup> unidades de pesquisa (SU) são faturação unidades, alocadas como um *réplica* ou uma *partição*. Precisa de ambos os recursos de armazenamento, indexação e operações de consulta. Para saber mais sobre os cálculos de SU, veja [Dimensionar níveis de recursos para cargas de trabalho de consulta e índice](../articles/search/search-capacity-planning.md). 