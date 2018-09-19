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
ms.openlocfilehash: b443bbd6377f0720c8be86bbe2b7a3e8ab8cb880
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129017"
---
# <a name="analyzers-method"></a>Método dos analisadores

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
