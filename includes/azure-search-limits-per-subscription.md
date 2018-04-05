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
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
Pode criar vários serviços numa subscrição, cada um deles aprovisionados a nenhum escalão específico, limitado apenas pelo número de serviços permitidos em cada camada. Por exemplo, pode criar até 12 serviços na camada básica e outro serviços 12 na camada S1 dentro da mesma subscrição. Para obter mais informações sobre as camadas, consulte [escolha um SKU ou a camada para a Azure Search](../articles/search/search-sku-tier.md).

Limites de serviço máximo podem desencadeados após pedido. Se precisar de mais serviços dentro da mesma subscrição, contacte o suporte do Azure.

| Recurso            | Livre&nbsp;<sup>1</sup> | Básica | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| Serviços máximos    |1     | 12    | 12  | 6  | 6  | 6     |
| Dimensionamento máximo no SU&nbsp;<sup>2</sup> |N/A |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> livres baseia-se nos recursos partilhados, não dedicados,. Dimensionamento não é suportado em recursos partilhados.

<sup>2</sup> unidades de pesquisa (SU) são faturação unidades, atribuídas como um *réplica* ou um *partição*. Terá de ambos os recursos de armazenamento, a indexação e as operações de consulta. Para saber mais sobre os cálculos de SU, consulte o artigo [Dimensionar níveis de recursos de cargas de trabalho de consulta e índice](../articles/search/search-capacity-planning.md). 