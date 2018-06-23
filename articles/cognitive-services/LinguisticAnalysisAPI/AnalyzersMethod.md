---
title: O método analisadores na Lingistic Analysis API | Microsoft Docs
description: As REST API de analisadores fornece uma lista dos analisadores são atualmente suportadas pelo serviço nos serviços cognitivos da Microsoft.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 06/30/2016
ms.author: lesun
ms.openlocfilehash: 3fc243a0da77c5bae9009929f2b82e1353347752
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351463"
---
# <a name="analyzers-method"></a>Analisadores de método

O **analisadores** REST API fornece uma lista dos analisadores atualmente suportada pelo serviço.
A resposta inclui os respetivos [nomes](Analyzer-Names.md) e os idiomas suportados por cada (por exemplo, "pt" para inglês).

## <a name="request-parameters"></a>Parâmetros do pedido
Nenhuma

<br>

## <a name="response-parameters"></a>Parâmetros de resposta
Nome | Tipo | Descrição
-----|------|--------------
idiomas | lista de cadeias de | lista de dois códigos de idioma do ISO letra para o qual este analisador pode ser utilizado.
ID   | cadeia | ID exclusivo para este analyzer
tipo | cadeia | o tipo abrangente de analisador aqui
especificação | cadeia | o nome da especificação utilizado para este analyzer
implementação | cadeia | Descrição do modelo de e/ou do algoritmo atrás este analisador

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
