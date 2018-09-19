---
title: Analisar método - API de análise linguística
titlesuffix: Azure Cognitive Services
description: Como utilizar o método de análise na API de análise linguística para analisar algumas entradas de linguagem natural.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 12/13/2016
ms.author: lesun
ms.openlocfilehash: c8d380a23c1bbfca8258ef533453050c72a3abd0
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129629"
---
# <a name="analyze-method"></a>Analisar método

O **analisar** REST API é usada para analisar a entrada de um determinado idioma natural.
Que pode envolver a localização a [frases e tokens](Sentences-and-Tokens.md) dentro do que a entrada, localizar o [etiquetas de parte da voz](POS-tagging.md), ou ao localizar o [constitutency árvore](Constituency-Parsing.md).
Pode especificar quais os resultados que pretende ao escolher os analisadores relevantes.
Para listar todos os analisadores disponíveis, veja a  **[analisadores](AnalyzersMethod.md)**.

Tenha em atenção que tem de especificar o idioma da cadeia de entrada.

**Ponto final REST:**
```
https://westus.api.cognitive.microsoft.com/linguistics/v1.0/analyze
```
<br>

## <a name="request-parameters"></a>Parâmetros do pedido

Nome | Tipo | Necessário | Descrição
-----|-------|----------|------------
**Idioma**    | cadeia | Sim | As duas letras o código de idioma do ISO a ser utilizado para análise. Por exemplo, o inglês é "en".
**analyzerIds** | lista de cadeias de caracteres | Sim | Lista de GUIDs de analisadores de aplicar. Consulte a documentação de analisadores para obter mais informações.
**text**        | cadeia | Sim | Dados brutos para ser analisado. Isso pode ser uma cadeia de caracteres curta, como uma palavra ou frase, uma frase completa, ou um parágrafo inteiro ou discourse.

## <a name="response-json"></a>Resposta (JSON)

Uma matriz de saídas de análise, um para cada atributo especificado no pedido.

Os resultados semelhante ao seguinte:

Nome | Tipo | Descrição
-----|------|--------------
analyzerId | cadeia | GUID do analyzer especificado
Resultado | objeto | resultado de analisador

Tenha em atenção que o tipo de resultado depende do tipo de analisador de entrada.

### <a name="tokens-response-json"></a>Resposta de tokens (JSON)

Nome | Tipo | Descrição
-----|------|-------------
Resultado | lista de objetos de frase | limites de sentença identificados no texto |
resultado [x]. Deslocamento | Int | deslocamento inicial do caráter de cada sentença |
resultado [x]. Len | Int | comprimento em carateres de cada sentença |
resultado [x]. Tokens | lista de objetos de token | limites de token identificados a frase |
resultado [x]. Tokens [y]. Deslocamento | Int | deslocamento inicial de caráter do token |
resultado [x]. Tokens [y]. Len | Int | comprimento em carateres do token |
resultado [x]. Tokens [y]. RawToken | cadeia | os carateres dentro do token, antes de normalização |
resultado [x]. Tokens [y]. NormalizedToken | cadeia | um formato normalizado do caráter, seguro para uso num [analisar árvore](Constituency-Parsing.md); por exemplo, um caráter de Parêntese aberto "(" torna-se - LRB - |

Entrada de exemplo: "Este é um teste. Hello. "
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


### <a name="pos-tags-response-json"></a>Resposta de etiquetas de POS (JSON)

O resultado é uma lista de listas de cadeias de caracteres.
Para cada sentença, existe uma lista de etiquetas de POS, uma marca de POS para cada token.
Para obter o token correspondente a cada marca de POS, vai querer fazer para um objeto de atomização também.

### <a name="constituency-tree-response-json"></a>Resposta de árvore de grupos (JSON)

O resultado é uma lista de cadeias de caracteres, a árvore de análise de um para cada sentença encontrada na entrada.
As árvores de análise são representadas num formulário escritos entre parênteses.

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

