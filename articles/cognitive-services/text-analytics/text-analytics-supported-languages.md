---
title: Idiomas suportados na API de análise de texto - serviços cognitivos do Azure | Microsoft Docs
description: Lista de geralmente disponível e de idioma de pré-visualização suportam para operações de API de análise de texto. Aplica-se a análise de dados de sentimento, extração de expressão de chave e deteção de idioma.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 2d341cfaf261bea6367bb55dd5d322f419e22d34
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355184"
---
# <a name="supported-languages-in-the-text-analytics-api"></a>Idiomas suportados na API de análise de texto

Este artigo explica os idiomas são suportados para cada operação: análise de dados de sentimento, extração de expressão de chave e deteção de idioma.

## <a name="language-detection"></a>Deteção de Idioma

A API de análise de texto pode detetar até 120 idiomas diferentes. Deteção de idioma devolve "script" de um idioma. Por exemplo, para a frase "Tenho um preguiçoso" devolverá `en` em vez de `en-US`. As maiúsculas e minúsculas só especial é chinês, onde irá devolver a capacidade de deteção de idioma `zh_CHS` ou `zh_CHT` se pode determinar o script fornecido o texto fornecido. Em situações onde não é possível identificado um script específico para um documento chinês, devolverá simplesmente `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Análise de dados de sentimento, extração de expressão de chave e entidade de ligação

Para análise de dados de sentimento, extração de expressão de chave e entidade de ligação, a lista de idiomas suportados é mais seletiva como as analisadores são avançadas para acomodar as regras linguístico de idiomas adicionais.

## <a name="language-list-and-status"></a>Lista de idioma e o Estado

Suporte de idiomas inicialmente está implementado na pré-visualização, graduating geralmente disponível (GA) o estado, independentemente entre si e do serviço de análise de texto geral. É possível para idiomas permaneça na pré-visualização, mesmo ao transições de API de análise de texto e geralmente disponível.

| Idioma    | Código de idioma | Sentimento | Frases-chave | Ligar à Entidade |   Notas  |
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
| Português (Portugal) | `pt-PT`| ✔        |  ✔          |       |`pt` também são aceites|
| Português (Brasil)   | `pt-BR`|          |  ✔   |         |     |
| Russo     | `ru`          | ✔ \*     | ✔           |             |     |
| Espanhol     | `es`          | ✔        | ✔           |     |     |
| Sueco     | `sv`          | ✔ \*     | ✔           |             |     |
| Turco     | `tr`          | ✔ \*     |             |             |     |

\* indica o suporte de idiomas em pré-visualização

## <a name="see-also"></a>Consulte também

[Página de documentação dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/)   
[Página de produto de serviços cognitivos](https://azure.microsoft.com/services/cognitive-services/)
