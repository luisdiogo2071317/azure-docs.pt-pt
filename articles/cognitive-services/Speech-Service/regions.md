---
title: Regiões de serviço de voz
titlesuffix: Azure Cognitive Services
description: Referência para as regiões do serviço de voz.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: a5fce6f9547a96da3ce482ce388e5ba2093f2af4
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468131"
---
# <a name="regions-of-the-speech-service"></a>Regiões do serviço de voz

O serviço de voz está disponível em regiões diferentes.
Quando cria uma subscrição, pode selecionar uma região disponível com base nas suas necessidades.

Quando utilizar a sua subscrição, terá de levar em conta a região que selecionou.

## <a name="rest-api"></a>API REST

Utilize a API REST para selecionar os pontos de extremidade específico da região corretos.
Ver [REST APIs](rest-apis.md) para obter detalhes.

## <a name="speech-sdk"></a>API de Voz

Na [SDK do serviço de voz](speech-sdk.md), regiões são especificadas como uma cadeia de caracteres (por exemplo, como um parâmetro para `SpeechConfig.FromSubscription` no SDK de voz para c#).

### <a name="regions-for-speech-recognition-and-translation"></a>Regiões para reconhecimento de fala e tradução

A tabela seguinte lista as regiões disponíveis para **reconhecimento de fala** e **tradução**.

  Região | Parâmetro do SDK de voz | Portal
 ------|-------|--------
 EUA Oeste | `westus` | https://westus.cris.ai
 E.U.A. Oeste 2 | `westus2` | https://westus2.cris.ai 
 EUA Leste | `eastus` | https://eastus.cris.ai
 E.U.A. Leste 2 | `eastus2` | https://eastus2.cris.ai
 Ásia Oriental | `eastasia` | https://eastasia.cris.ai
 Sudeste Asiático | `southeastasia` | https://southeastasia.cris.ai
 Europa do Norte | `northeurope` | https://northeurope.cris.ai
 Europa Ocidental | `westeurope` | https://westeurope.cris.ai


### <a name="regions-for-intent-recognition"></a>Regiões para o reconhecimento da intenção do

Regiões disponíveis para **reconhecimento da intenção** através do SDK de voz estão listados no [página de região do serviço de compreensão de idiomas](/azure/cognitive-services/luis/luis-reference-regions).
Para cada região publicação listado, o parâmetro de região do SDK de voz correspondente será determinado como a primeira parte do nome do domínio do ponto de extremidade.
Por exemplo, utilizar `westus` para especificar a região de publicação de EUA oeste.
