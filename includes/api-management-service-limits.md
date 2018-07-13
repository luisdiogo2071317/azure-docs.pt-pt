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
ms.openlocfilehash: b40e2ba448236bcef72ffe501b4b14fa0dd64e1c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756108"
---
| Recurso | Limite |
| --- | --- |
| Unidades de escala | 10 por região<sup>1</sup> |
| Cache | 5 GB por unidade<sup>1</sup> |
| Ligações de back-end em simultâneo<sup>2</sup> por uma autoridade HTTP | 2048 por unidade<sup>3</sup> |
| Tamanho máximo da resposta em cache | 10MB |
| Tamanho do documento de política máximo | 256KB |
| Domínios de gateway personalizada máximo | 20 por instância de serviço<sup>4</sup> |


<sup>1</sup>limites de gestão de API são diferentes para cada escalão de preço. Para ver os preços camadas e os respetivos limites de dimensionamento aceda a [preços da gestão de API](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> ligações são agrupadas e utilizadas voltar, a menos que explicitamente fechada pelo back-end.
<sup>3</sup> por unidade de escalões Basic, Standard e Premium. Escalão de programador está limitado a 1024.
<sup>4</sup> disponível em apenas no escalão Premium.


