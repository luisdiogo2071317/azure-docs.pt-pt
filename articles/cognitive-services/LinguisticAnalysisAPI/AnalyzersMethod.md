---
title: O método dos analisadores - API de análise Lingistic
titlesuffix: Azure Cognitive Services
description: Os analisadores de REST API fornece uma lista de analisadores de que são atualmente suportados pela API de análise linguística.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 06/30/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 762ebf50999a88251dcd05824f2ed450cec97f04
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237403"
---
# <a name="analyzers-method"></a>Método dos analisadores

> [!IMPORTANT]
> A pré-visualização de análise linguística foi desativada no dia 9 de Agosto de 2018. Recomendamos que utilize [módulos de análise de texto do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) para processamento de texto e análise.

O **analisadores** REST API fornece uma lista de analisadores atualmente suportadas pelo serviço.
A resposta inclui seus [nomes](Analyzer-Names.md) e os idiomas suportados por cada (por exemplo, "pt" para inglês).

## <a name="request-parameters"></a>Parâmetros do pedido
Nenhuma

<br>

## <a name="response-parameters"></a>Parâmetros de resposta
Nome | Tipo | Descrição
-----|------|--------------
Idiomas | lista de cadeias de caracteres | lista de dois códigos de idioma do ISO letra para o qual este analyzer pode ser utilizado.
ID   | cadeia | ID exclusivo para este analyzer
tipo | cadeia | o tipo de amplo do analyzer aqui
Especificação | cadeia | o nome da especificação utilizado para este analyzer
Implementação | cadeia | Descrição do modelo de e/ou do algoritmo por trás deste analyzer

<br>
## <a name="example"></a>Exemplo
OBTER /analyzers

Resposta: JSON
```json
[
    {
        "id": "22A6B758-420F-4745-8A3C-46835A67C0D2",
        "languages": ["en"],
        "kind": "Constituency_Tree",  
        "specification": "PennTreebank3",
        "implementation": "SplitMerge"
    },
    {
        "id" : "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
        "languages": ["en"],
        "kind": "POS_Tags",
        "specification": "PennTreebank3",
        "implementation": "cmm"
    },
    {
        "id" : "08EA174B-BFDB-4E64-987E-602F85DA7F72",
        "languages": ["en"],
        "kind": "Tokens",
        "specification":"PennTreebank3",
        "implementation": "regexes"
    }
]
```
