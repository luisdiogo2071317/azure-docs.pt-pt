---
title: incluir ficheiro
description: incluir ficheiro
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 03/22/2018
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 676bd3b3a369e3f834016f626a8e69f6b41d9b23
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52170705"
---
| Recurso | Limite |
| --- | --- |
| Unidades de escala | 10 por região<sup>1</sup> |
| Cache | 5 GB por unidade<sup>1</sup> |
| Ligações de back-end em simultâneo<sup>2</sup> por uma autoridade HTTP | 2048 por unidade<sup>3</sup> |
| Tamanho máximo da resposta em cache | 2MB |
| Tamanho do documento de política máximo | 256KB |
| Domínios de gateway personalizada máximo | 20 por instância de serviço<sup>4</sup> |


<sup>1</sup>limites de gestão de API são diferentes para cada escalão de preço. Para ver os preços camadas e os respetivos limites de dimensionamento aceda a [preços da gestão de API](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> ligações são agrupadas e utilizadas voltar, a menos que explicitamente fechada pelo back-end.
<sup>3</sup> por unidade de escalões Basic, Standard e Premium. Escalão de programador está limitado a 1024.
<sup>4</sup> disponível em apenas no escalão Premium.


