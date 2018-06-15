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
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31434944"
---
| Recurso | Limite |
| --- | --- |
| Unidades de escala | 10 por região<sup>1</sup> |
| Cache | 5 GB por unidade<sup>1</sup> |
| Ligações de back-end em simultâneo<sup>2</sup> por uma autoridade HTTP | 2048 por unidade<sup>3</sup> |
| Tamanho máximo em cache da resposta | 10MB |
| Tamanho do documento de política máximo | 256KB |
| Domínios de gateway personalizado máximo | 20 por instância de serviço<sup>4</sup> |


<sup>1</sup>limites de API Management são diferentes para cada escalão de preço. Para ver os preços camadas e os respetivos limites de dimensionamento aceda a [preços de gestão de API](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> ligações são agrupadas e novamente utilizadas, a menos que explicitamente fechada pelo back-end.
<sup>3</sup> por unidade de escalões básico, Standard e Premium. Camada de programador está limitada a 1024.
<sup>4</sup> disponível no escalão Premium apenas.


