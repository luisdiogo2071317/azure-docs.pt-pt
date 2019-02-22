---
title: Suporte de idiomas - API de análise de texto
titleSuffix: Azure Cognitive Services
description: 'Uma lista de idiomas suportados pela API de análise de texto. Este artigo explica que idiomas são suportados para cada operação: análise de sentimentos, extração de expressões-chave, deteção de idioma e reconhecimento de entidades.'
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: a9f48c8cef8d977469bb6c583d0bc363e334f693
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245325"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Suporte de idioma e região para a API de análise de texto

Este artigo explica que idiomas são suportados para cada operação: análise de sentimentos, extração de expressões-chave e deteção de idioma.

## <a name="language-detection"></a>Deteção de Idioma

A API de análise de texto pode detetar até 120 idiomas diferentes. Deteção de idioma devolve "script" de um idioma. Por exemplo, para a frase "Tenho um cachorro" irá devolver `en` em vez de `en-US`. Caso apenas especial é chinês, onde irá devolver a capacidade de deteção de idioma `zh_CHS` ou `zh_CHT` se ele pode determinar o script tendo em conta o texto fornecido. Em situações em que um script específico não é possível identificar de um documento do chinês, irá devolver simplesmente `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-recognition"></a>Análise de sentimentos, extração de expressões-chave e reconhecimento de entidades

Para a análise de sentimentos, extração de expressões-chave e reconhecimento de entidades, a lista de idiomas suportados é mais seletiva conforme os analisadores são refinados para acomodar as regras de idiomas adicionais.

## <a name="language-list-and-status"></a>Lista de idioma e o Estado

Suporte de idiomas é inicialmente lançado em pré-visualização, graduando para disponibilidade geral (GA) do Estado, independentemente uns dos outros e do serviço de análise de texto geral. É possível para idiomas permaneça na pré-visualização, mesmo durante as transições de API de análise de texto para disponibilidade geral.

| Idioma    | Código do idioma | Sentimento | Frases-chave | Reconhecimento de entidade |   Notas  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Dinamarquês      | `da`          | ✔ \*     | ✔           |             |     |
| Neerlandês       | `nl`          | ✔ \*     | ✔          |             |     |
| InglêS     | `en`          | ✔        | ✔           |  ✔ \*   |      |
| Finlandês     | `fi`          | ✔ \*     | ✔           |             |     |
| Francês      | `fr`          | ✔        | ✔           |             |     |
| Alemão      | `de`          | ✔ \*     | ✔           |            |     |
| Grego       | `el`          | ✔ \*     |             |            |     |
| Italiano     | `it`          | ✔ \*     | ✔           |             |     |
| Japonês    | `ja`          |          | ✔           |            |     |
| Coreano      | `ko`          |          | ✔           |            |     |
| Norueguês (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Polaco      | `pl`          | ✔ \*     |  ✔          |             |     |
| Português (Portugal) | `pt-PT`| ✔        |  ✔          |       |`pt` também aceites|
| Português (Brasil)   | `pt-BR`|          |  ✔   |         |     |
| Russo     | `ru`          | ✔ \*     | ✔           |             |     |
| Espanhol     | `es`          | ✔        | ✔           |   ✔ \*\*      |     |
| Sueco     | `sv`          | ✔ \*     | ✔           |             |     |
| Turco     | `tr`          | ✔ \*     |             |             |  |

\* indica o suporte de idioma na pré-visualização

\*\* Extração de entidades para espanhol só está disponível no [(versão 2.1-pré-visualização)](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)

## <a name="see-also"></a>Consulte também

[Página de documentação dos serviços cognitiva](https://docs.microsoft.com/azure/cognitive-services/)   
[Página de Produto dos Serviços Cognitivos](https://azure.microsoft.com/services/cognitive-services/)
