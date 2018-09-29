---
title: Suporte de idiomas - API de análise de texto
titleSuffix: Azure Cognitive Services
description: 'Uma lista de idiomas suportados pela API de análise de texto. Este artigo explica que idiomas são suportados para cada operação: análise de sentimentos, extração de expressões-chave e deteção de idioma.'
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 09/25/2018
ms.author: ashmaka
ms.openlocfilehash: e9f466ac6bce98a6a9f2d79a443c9602ca40bb26
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435514"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Suporte de idioma e região para a API de análise de texto

Este artigo explica que idiomas são suportados para cada operação: análise de sentimentos, extração de expressões-chave e deteção de idioma.

## <a name="language-detection"></a>Deteção de Idioma

A API de análise de texto pode detetar até 120 idiomas diferentes. Deteção de idioma devolve "script" de um idioma. Por exemplo, para a frase "Tenho um cachorro" irá devolver `en` em vez de `en-US`. Caso apenas especial é chinês, onde irá devolver a capacidade de deteção de idioma `zh_CHS` ou `zh_CHT` se ele pode determinar o script tendo em conta o texto fornecido. Em situações em que um script específico não é possível identificar de um documento do chinês, irá devolver simplesmente `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Análise de sentimentos, extração de expressões-chave e ligação de entidades

Para a análise de sentimentos, extração de expressões-chave e ligação de entidades, a lista de idiomas suportados é mais seletiva conforme os analisadores são refinados para acomodar as regras de idiomas adicionais.

## <a name="language-list-and-status"></a>Lista de idioma e o Estado

Suporte de idiomas é inicialmente lançado em pré-visualização, graduando para disponibilidade geral (GA) do Estado, independentemente uns dos outros e do serviço de análise de texto geral. É possível para idiomas permaneça na pré-visualização, mesmo durante as transições de API de análise de texto para disponibilidade geral.

| Idioma    | Código do idioma | Sentimento | Frases-chave | Ligar à Entidade |   Notas  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Dinamarquês      | `da`          | ✔ \*     | ✔           |             |     |
| Neerlandês       | `nl`          | ✔ \*     | ✔          |             |     |
| Português     | `en`          | ✔        | ✔           |  ✔ \*   |      |
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
| Espanhol     | `es`          | ✔        | ✔           |     |     |
| Sueco     | `sv`          | ✔ \*     | ✔           |             |     |
| Turco     | `tr`          | ✔ \*     |             |             |     |

\* indica o suporte de idioma na pré-visualização

## <a name="see-also"></a>Consulte também

[Página de documentação dos serviços cognitiva](https://docs.microsoft.com/azure/cognitive-services/)   
[Página de Produto dos Serviços Cognitivos](https://azure.microsoft.com/services/cognitive-services/)
