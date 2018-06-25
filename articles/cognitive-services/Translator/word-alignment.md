---
title: Informações de alinhamento com a API do Microsoft tradutor texto do Word | Microsoft Docs
description: Receba informações de alinhamento do word a partir da API de texto de tradutor a Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 9d8284db61235284ec0d5a1594c34687c89560e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352609"
---
# <a name="how-to-receive-word-alignment-information"></a>Como receber informações de alinhamento da palavra

## <a name="receiving-word-alignment-information"></a>Receber informações de alinhamento da palavra
Para receber as informações de alinhamento, utilize o método Translate e incluir o parâmetro opcional includeAlignment.

## <a name="alignment-information-format"></a>Formato de informações de alinhamento
Alinhamento é devolvido como um valor de cadeia no formato seguinte para cada palavra da origem. As informações de cada palavra são separadas por um espaço, incluindo para separados por no espaço de idiomas (scripts), como chinês:

[[SourceTextStartIndex]: [SourceTextEndIndex]-[TgtTextStartIndex]: [TgtTextEndIndex]] *

Cadeia de alinhamento de exemplo: "0:0-7:10 de 1:2-11:3:4-20-0:3 de 3:4-4:5:5-6 21:21".

Por outras palavras, a vírgula separa início índice final, o travessão separa os idiomas e espaço separa as palavras. Uma palavra poderão ficar alinhadas com zero, um ou vários palavras no outro idioma e as palavras aligned poderão estar não contíguos. Quando não existem informações de alinhamento estiver disponíveis, o elemento de alinhamento estará vazio. O método não devolve nenhum erro nesse caso.

## <a name="restrictions"></a>Restrições
Alinhamento só é devolvido para um subconjunto dos pares de idioma neste momento:
* do inglês para qualquer idioma;
* a partir de qualquer outro idioma inglês, exceto para chinês simplificado, chinês tradicional e Letão inglês
* Japonês para coreano ou a coreano japonês que não irá receber informações de alinhamento se o frase for uma tradução encontra. Exemplo de uma tradução encontra é "Este é um teste", "Posso adoram," e outros frases de alta frequência.

## <a name="example"></a>Exemplo

JSON de exemplo

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
