---
title: Sintaxe de pesquisa de lambda na API de conhecimento académico | Microsoft Docs
description: Saiba mais sobre a sintaxe de pesquisa de Lambda que pode utilizar a API de conhecimento académico nos serviços cognitivos da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f486368e1d0258087091acb846a84b125712db40
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351481"
---
# <a name="lambda-search-syntax"></a>Sintaxe de pesquisa de lambda

Cada *lambda* cadeia descreve um padrão de gráfico de consulta de pesquisa. Uma consulta tem de ter pelo menos um nó de partida, especificando a partir do nó de gráfico que vamos iniciar a transversal. Para especificar um nó inicial, chame o *MAG. StartFrom()* método e passar no indicados de um ou mais nós ou uma consulta de objeto que especifica as restrições de pesquisa. O *StartFrom()* método tem três sobrecargas. Todos eles demorar dois argumentos com a segunda que está a ser opcionais. O primeiro argumento pode ser um número inteiro longo, uma coleção enumeráveis de número inteiro longo, ou uma cadeia representando um JSON object, com a mesma semântica do *json* pesquisa:
```
StartFrom(long cellid, IEnumerable<string> select = null)
StartFrom(IEnumerable<long> cellid, IEnumerable<string> select = null)
StartFrom(string queryObject, IEnumerable<string> select = null)
```

O processo de escrever uma consulta de pesquisa de lambda é a guiá-lo de um nó para outro. Para especificar o tipo de limite para percorrer, utilize *FollowEdge()* e os tipos de limite pretendido. *FollowEdge()* demora um número de argumentos de cadeia arbitrário:
```
FollowEdge(params string[] edgeTypes)
```
> [!NOTE]
> Se que não se preocupar com os tipos de edge(s) a seguir, basta omitir *FollowEdge()* entre dois nós: a consulta irá guiá-todas as extremidades possíveis entre estes dois nós.

Iremos pode especificar as ações de transversal a ser executadas num nó através do *VisitNode()*, ou seja, se parar neste nó e devolver o caminho atual como o resultado ou continuar a explorar o gráfico.  O tipo de enumeração *ação* define dois tipos de ações: *Action.Return* e *Action.Continue*. Que podemos transmitir esse um valor de enumeração diretamente no *VisitNode()*, ou combiná-los com o bit a bit- e o operador '&'. Quando dois ação são combinados, significa que serão executadas ambas as ações. Nota: não utilize totalmente- ou o operador ' |' das ações. Se o fizer, irá fazer com que a consulta para terminar sem devolver nada. A ignorar *VisitNode()* entre duas *FollowEdge()* chamadas fará com que a consulta explorar incondicionalmente gráfico após a chegar ao nó.

```
VisitNode(Action action, IEnumerable<string> select = null)
```

Para *VisitNode()*, que podemos também transmitir numa expressão lambda do tipo *expressão\<Func\<Inodes, ação\>\>*, que demora um *Inodes* e devolve uma ação de transversal:

```
VisitNode(Expression<Func<INode, Action>> action, IEnumerable<string> select = null)
```

## <a name="inode"></a>*Inodes* 

*Inodes* fornece *só de leitura* interfaces e algumas funções de ajuda incorporado num nó de acesso a dados. 

### <a name="basic-data-access-interfaces"></a>Interfaces de acesso de dados básicos

##### <a name="long-cellid"></a>CellID longo

O ID de 64 bits do nó. 

##### <a name="t-getfieldtstring-fieldname"></a>T GetField\<T\>(fieldName da cadeia)

Obtém o valor da propriedade especificada. *T* é o tipo pretendido que o campo deve ser interpretado como. Conversão de tipo automática será tentada se o tipo pretendido não é possível converter implicitamente a partir do tipo do campo. Nota: quando a propriedade é com múltiplos valores, *GetField\<cadeia\>*  fará com que a lista ser serializado para uma cadeia Json ["val1", "val2",...]. Se a propriedade não existe, irá acionar uma excepção e abortar a exploração de gráfico atual.

##### <a name="bool-containsfieldstring-fieldname"></a>bool ContainsField (cadeia fieldName)

Indica se um campo com o nome fornecido existe no nó atual.

##### <a name="string-getstring-fieldname"></a>cadeia obter (cadeia fieldName)

Como funciona *GetField\<cadeia\>(fieldName)*. No entanto, não inicia exceções quando o campo não for encontrado, devolve uma string("") vazio em vez disso.

##### <a name="bool-hasstring-fieldname"></a>bool tem (cadeia fieldName)

Indica se a propriedade especificada existe no nó atual. Igual ao *ContainsField(fieldName)*.

##### <a name="bool-hasstring-fieldname-string-value"></a>bool tem (fieldName cadeia, o valor da cadeia)

Indica se a propriedade tem um valor especificado. 

##### <a name="int-countstring-fieldname"></a>Contagem de INT (cadeia fieldname)

Obter a contagem de valores de uma propriedade indicado. Quando a propriedade não existe, devolve 0.

### <a name="built-in-helper-functions"></a>Funções de programa auxiliar incorporadas

##### <a name="action-returnifbool-condition"></a>Ação return_if (bool condição)

Devolve *Action.Return* se a condição for *verdadeiro*. Se a condição for *falso* e nesta expressão não foi associada com outras ações por um bit a bit- e operador, será abortada a exploração de gráfico.

##### <a name="action-continueifbool-condition"></a>Ação continue_if (bool condição)

Devolve *Action.Continue* se a condição for *verdadeiro*. Se a condição for *falso* e nesta expressão não foi associada com outras ações por um bit a bit- e operador, será abortada a exploração de gráfico.

##### <a name="bool-dicedouble-p"></a>bool repartir (duplo p)

Gera um número aleatório maior ou igual a 0,0 e menor que 1,0. Esta função devolve *verdadeiro* apenas se o número é inferior ou igual a *p*.

Comparado com *json* pesquisa, *lambda* pesquisa é mais expressivas: expressões lambda c# podem ser diretamente utilizadas para especificar os padrões de consulta. Seguem-se dois exemplos.

```
MAG.StartFrom(@"{
    type  : ""ConferenceSeries"",
    match : {
        FullName : ""graph""
    }
}", new List<string>{ "FullName", "ShortName" })
.FollowEdge("ConferenceInstanceIDs")
.VisitNode(v => v.return_if(v.GetField<DateTime>("StartDate").ToString().Contains("2014")),
        new List<string>{ "FullName", "StartDate" })
```

```
MAG.StartFrom(@"{
    type  : ""Affiliation"",
    match : {
        Name : ""microsoft""
    }
}").FollowEdge("PaperIDs")
.VisitNode(v => v.return_if(v.get("NormalizedTitle").Contains("graph") || v.GetField<int>("CitationCount") > 100),
        new List<string>{ "OriginalTitle", "CitationCount" })
```
