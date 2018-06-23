---
title: Analisar o método na API Analysis linguístico | Microsoft Docs
description: Como utilizar o método de análise na API de análise linguístico para analisar algumas entradas de linguagem natural.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 12/13/2016
ms.author: lesun
ms.openlocfilehash: b17a00f31845bfa05572dff7ca94e9a1ffd69586
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351734"
---
# <a name="analyze-method"></a>Analisar o método

O **analisar** REST API é utilizada para analisar um determinado idioma natural de entrada.
Que poderão envolver apenas ao localizar o [frases e tokens](Sentences-and-Tokens.md) dentro do que a entrada, localizar o [etiquetas de parte de voz](POS-tagging.md), ou ao localizar o [constitutency árvore](Constituency-Parsing.md).
Pode especificar que pretende por diretriz os relevantes analisadores de resultados.
Para listar todos os analisadores disponíveis, observe o  **[analisadores](AnalyzersMethod.md)**.

Tenha em atenção que tem de especificar o idioma de cadeia de entrada.

**Ponto final REST:**
```
https://westus.api.cognitive.microsoft.com/linguistics/v1.0/analyze
```
<br>

## <a name="request-parameters"></a>Parâmetros do pedido

Nome | Tipo | Necessário | Descrição
-----|-------|----------|------------
**Idioma**    | cadeia | Sim | Os dois letra o código de idioma ISO para ser utilizado para análise. Por exemplo, o inglês é "pt".
**analyzerIds** | lista de cadeias de | Sim | Lista de GUIDs de analisadores a aplicar. Consulte a documentação de analisadores para obter mais informações.
**text**        | cadeia | Sim | Entrada em bruto para ser analisada. Esta situação pode ser uma cadeia curta, como uma palavra ou frase, uma completa, ou um parágrafo completo ou frases discourse.

<br>
## <a name="response-json"></a>Resposta (JSON)
Uma matriz de saídas de análise, um para cada atributo especificado no pedido.

Os resultados de ter o seguinte aspeto:

Nome | Tipo | Descrição
-----|------|--------------
analyzerId | cadeia | GUID de analisador especificado
resultado | objeto | resultado de analisador

Tenha em atenção que o tipo de resultado depende do tipo de entrada analisador.

### <a name="tokens-response-json"></a>Resposta de tokens (JSON)

Nome | Tipo | Descrição
-----|------|-------------
resultado | lista de objetos de frases | limites de frases identificados no texto |
resultado [x]. Deslocamento | Int | deslocamento de carácter inicial de cada frase |
resultado [x]. Len | Int | comprimento em carateres de cada frase |
resultado [x]. Tokens | lista de objetos de token | limites de token identificados no frase |
resultado [x]. Tokens [s]. Deslocamento | Int | deslocamento de carácter inicial do token |
resultado [x]. Tokens [s]. Len | Int | comprimento de carateres do token |
resultado [x]. Tokens [s]. RawToken | cadeia | os carateres dentro esse token, antes de normalização |
resultado [x]. Tokens [s]. NormalizedToken | cadeia | um formulário normalizado do caráter, seguro para utilização num [analisar árvore](Constituency-Parsing.md); por exemplo, um caráter de parênteses aberto ' (' torna-se - LRB - |

Entrada de exemplo: "Este é um teste. Olá.'
Resposta JSON de exemplo:
```json
[
  {
    "Len": 15,
    "Offset": 0,
    "Tokens": [
      {
        "Len": 4,
        "NormalizedToken": "This",
        "Offset": 0,
        "RawToken": "This"
      },
      {
        "Len": 2,
        "NormalizedToken": "is",
        "Offset": 5,
        "RawToken": "is"
      },
      {
        "Len": 1,
        "NormalizedToken": "a",
        "Offset": 8,
        "RawToken": "a"
      },
      {
        "Len": 4,
        "NormalizedToken": "test",
        "Offset": 10,
        "RawToken": "test"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 14,
        "RawToken": "."
      }
    ]
  },
  {
    "Len": 6,
    "Offset": 16,
    "Tokens": [
      {
        "Len": 5,
        "NormalizedToken": "Hello",
        "Offset": 16,
        "RawToken": "Hello"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 21,
        "RawToken": "."
      }
    ]
  }
]
```


### <a name="pos-tags-response-json"></a>Resposta de etiquetas POS (JSON)

O resultado é uma lista de listas de cadeias.
Para cada frase, há uma lista de etiquetas de POS, uma tag de POS para cada token.
Para obter o token correspondente a cada etiqueta POS, poderá ser útil pedir um objeto de atomização bem.

### <a name="constituency-tree-response-json"></a>Resposta de árvore constituency (JSON)

O resultado é uma lista de cadeias, a árvore de uma análise para cada frase encontrado na entrada.
As árvores de análise são representados num formulário entre parênteses.

<br>

## <a name="example"></a>Exemplo

`POST /analyze`

Corpo do pedido: Payload JSON
```json
{
  "language": "en",
  "analyzerIds": [
    "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
    "22A6B758-420F-4745-8A3C-46835A67C0D2" ],
  "text": "Hi, Tom! How are you today?" 
}
```

Resposta: JSON
```json
[
  {
    "analyzerId": "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04", 
    "result": [ ["NNP",",","NNP","."], ["WRB","VBP","PRP","NN","."] ]
  },
  {
    "analyzerId": "22A6B758-420F-4745-8A3C-46835A67C0D2", 
    "result":["(TOP (S (NNP Hi) (, ,) (NNP Tom) (. !)))","(TOP (SBARQ (WHADVP (WRB How)) (SQ (VP (VBP are)) (NP (PRP you)) (NN today) (. ?))))"]
  }
]
```

