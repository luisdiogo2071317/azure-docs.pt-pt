---
title: Referência de uma anotação em entradas e saídas num pipeline cognitivos pesquisa na Azure Search | Microsoft Docs
description: Explica a sintaxe de anotação e como fazer referência uma anotação na entradas e saídas de um skillset num pipeline cognitivos pesquisa na Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 0e074e93ecbe80c3acf3481c0d33917fbe5090c6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640911"
---
# <a name="how-to-reference-annotations-in-a-cognitive-search-skillset"></a>Como fazer referência anotações do skillset pesquisa cognitivos

Neste artigo, irá aprender a referenciar anotações nas definições de skill, com exemplos para ilustrar os vários cenários. Como o conteúdo de um documento flui através de um conjunto de competências, obtém enriquecido com anotações. Anotações podem ser utilizadas como entradas para continuar sem causa a jusante ou mapeadas para um campo de saída num índice. 
 
Os exemplos neste artigo baseiam-se no *conteúdo* campo gerado automaticamente pelo [indexadores de Blob do Azure](search-howto-indexing-azure-blob-storage.md) como parte do documento cracking fase. Ao fazer referência aos documentos a partir de um contentor de BLOBs, utilize um formato como `"/document/content"`, onde o *conteúdo* campo faz parte o *documento*. 

## <a name="background-concepts"></a>Conceitos de em segundo plano

Antes de consultar a sintaxe, vamos revê alguns conceitos importantes para compreender melhor os exemplos fornecidos neste artigo.

| Termo | Descrição |
|------|-------------|
| Documento avançado | Um documento avançado é uma estrutura interna criado e utilizado pelo pipeline para conter todas as anotações relacionados com um documento. Considere um documento avançado como uma árvore de anotações. Geralmente, uma anotação criada a partir de uma anotação anterior torna-se o subordinado.<p/>Documentos avançados só existam durante a execução de skillset. Depois do conteúdo está mapeado para o índice de pesquisa, o documento avançado já não é necessária. Apesar de não interagem com os documentos avançados diretamente, é útil para ter um modelo mental dos documentos ao criar um skillset. |
| Sem causa contexto | O contexto em que o sem causa ocorre, relativamente a quais é enriquecido elemento. Por predefinição, o contexto de sem causa é o `"/document"` nível, no âmbito de documentos individuais. Quando um skill for executada, as saídas desse skill ficar [propriedades do contexto definido](#example-2).|

<a name="example-1"></a>
## <a name="example-1-simple-annotation-reference"></a>Exemplo 1: Referência de anotação simples

No Blob storage do Azure, suponha que tem uma variedade de ficheiros que contêm referências a nomes das pessoas que pretende extrair com reconhecimento de entidade com nome. Na definição do skill abaixo, `"/document/content"` é a representação textual de todo o documento e "pessoas" é uma extração dos nomes de completas para entidades identificados como pessoas.

Porque o contexto predefinido é `"/document"`, a lista de pessoas agora pode ser referenciada como `"/document/people"`. Neste caso específico `"/document/people"` é uma anotação, que pode agora ser mapeada para um campo num índice ou utilizada em outro skill no skillset mesmo.

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      }
    ]
  }
```

<a name="example-2"></a>

## <a name="example-2-reference-an-array-within-a-document"></a>Exemplo 2: Referência a uma matriz dentro de um documento

Neste exemplo baseia-se a um anterior, a mostrar como invocar um passo sem causa várias vezes através do mesmo documento. Assumir que a anterior exemplo gerado uma matriz de cadeias com 10 nomes de pessoas a partir de um único documento. Passo seguinte razoável poderá ser uma segundo sem causa que extrai o último nome de um nome completo. Devido a existirem 10 nomes, pretende que este passo ser chamado 10 vezes neste documento, uma vez para cada pessoa. 

Para invocar o número correto de iterações, defina o contexto como `"/document/people/*"`, onde o asterisco (`"*"`) representa todos os nós no documento avançado como descendentes de `"/document/people"`. Embora este skill só está definida uma vez na matriz competências, é chamado para cada membro no documento até que todos os membros são processados.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the last name from a full name",
    "uri": "http://names.azurewebsites.net/api/GetLastName",
    "context" : "/document/people/*",
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "fullname",
        "source": "/document/people/*"
      }
    ],
    "outputs": [
      {
        "name": "lastname",
        "targetName": "last"
      }
    ]
  }
```

Quando as anotações são coleções de cadeias ou matrizes, pode querer destino membros específicos em vez da matriz como um todo. O exemplo acima gera uma anotação chamada `"last"` em cada nó representado pelo contexto. Se pretender fazer referência a esta família de anotações, pode utilizar a sintaxe `"/document/people/*/last"`. Se pretende referir-se para uma anotação específica, pode utilizar um índice explícito: `"/document/people/1/last`"para referenciar o último nome da pessoa primeiro identificado no documento. Tenha em atenção que esta sintaxe matrizes são "1 indexado".

<a name="example-3"></a>

## <a name="example-3-reference-members-within-an-array"></a>Exemplo 3: Os membros numa matriz de referência

Por vezes, precisa de todas as anotações de um determinado tipo transmiti-los para um determinado skill de grupo. Considere um skill personalizado hipotético que identifica o nome do último mais comuns de todos os nomes de último extraída no exemplo 2. Para fornecer apenas o apelido para o skill personalizada, especifique o contexto como `"/document"` e a entrada como `"/document/people/*/lastname"`.

Tenha em atenção que a cardinalidade do `"/document/people/*/lastname"` é maior do que um documento. Pode haver 10 lastname nós enquanto existir apenas um nó de documento para este documento. Nesse caso, o sistema irá criar automaticamente uma matriz de `"/document/people/*/lastname"` que contém todos os elementos no documento.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the most common string from an array of strings",
    "uri": "http://names.azurewebsites.net/api/MostCommonString",
    "context" : "/document",
    "inputs": [
      {
        "name": "strings",
        "source": "/document/people/*/lastname"
      }
    ],
    "outputs": [
      {
        "name": "mostcommon",
        "targetName": "common-lastname"
      }
    ]
  }
```



## <a name="see-also"></a>Consulte também
+ [Como integrar um skill personalizado para um pipeline sem causa](cognitive-search-custom-skill-interface.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Criar Skillset (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Como mapear campos avançados para um índice](cognitive-search-output-field-mapping.md)
