---
title: O que é a API de Verificação de Ortografia do Bing?
titlesuffix: Azure Cognitive Services
description: A API de Verificação de Ortografia do Bing utiliza aprendizagem automática e tradução automática de estatísticas para a verificação ortográfica contextual.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 05/03/2018
ms.author: nolachar
ms.openlocfilehash: c15af0dcebdfcbe984d47b5c06f213e516ae3914
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149776"
---
# <a name="what-is-bing-spell-check-api"></a>O que é a API de Verificação de Ortografia do Bing?

A API de Verificação de Ortografia do Bing permite-lhe realizar a verificação ortográfica e gramatical contextual.

Qual é a diferença entre verificadores ortográficos regulares e o verificador ortográfico de terceira geração do Bing? Os verificadores ortográficos atuais baseiam-se na verificação de ortografia e gramática nos conjuntos de regras baseadas no dicionário, que são atualizadas e expandidas periodicamente. Por outras palavras, o verificador ortográfico é tão forte quanto o dicionário que o suporta e a equipa editorial que suporta o dicionário. Conhece este tipo de verificador ortográfico do Microsoft Word e de outros processadores de texto.

Por outro lado, o Bing desenvolveu um verificador ortográfico baseado na Web que tira partido da aprendizagem automática de estatísticas para treinar de forma dinâmica um algoritmo em constante evolução e altamente contextual. O verificador ortográfico é baseado num grande corpus de pesquisas na Web e documentos.

Este verificador ortográfico suporta qualquer cenário de processamento de palavras:

- Reconhece gíria e linguagem informal
- Reconhece erros de nomes comuns em contexto
- Corrige problemas de separação de palavras com um único sinalizador
- Consegue corrigir homófonos no contexto e outros erros difíceis de detetar
- Suporta novas marcas, entretenimento digital e expressões populares que possam surgir
- Palavras que soam igual, mas diferem no significado e na ortografia, por exemplo "ver" e "mar."

## <a name="spell-check-modes"></a>Modos de verificação ortográfica

A API suporta dois modos de revisão de texto, `Proof` e `Spell`.  Experimente [estes](https://azure.microsoft.com/services/cognitive-services/spell-check/) exemplos.
### <a name="proof---for-documents-scenario"></a>Verificação - para cenário de documentos
O modo predefinido é `Proof`. O modo de ortografia `Proof` oferece as verificações mais abrangentes, a adição de capitalização, pontuação básica e outras funcionalidades para ajudar na criação de documentos. no entanto, está disponível apenas nos mercados en-US (inglês dos Estados Unidos), es-ES (espanhol), pt-BR [português (Brasil)] (Nota: apenas na versão beta para espanhol e português). Para todos os outros mercados, defina o parâmetro de consulta de modo para Ortografia. 
<br /><br/>**NOTA:**   Se o comprimento do texto de consulta exceder 4096, será truncado para 4096 carateres, em seguida, processadas. 
### <a name="spell----for-web-searchesqueries-scenario"></a>Ortografia - para cenário de pesquisas/consultas da Web
A `Spell` é mais agressiva para devolver resultados melhores de pesquisa. O modo `Spell` encontra a maioria dos erros de ortografia, mas não encontra alguns dos erros de gramática que o `Proof` encontra, por exemplo, a capitalização e as palavras repetidas.

> [!NOTE]
> * O comprimento máximo de consulta suportados é inferior. Se a consulta excede o comprimento máximo, a consulta e seus resultados não serão alterados.
>    * 130 carateres para os seguintes códigos de idioma: en, Alemanha, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, ele, zh, ko. 
>    * 65 carateres para todos os outros.
> * O modo de ortografia não oferece suporte a caracteres de parêntesis Reto de fecho (`[` e `]`) em consultas e pode gerar resultados inconsistentes. Recomendamos a removê-los a partir de suas consultas ao usar o modo de ortografia.

## <a name="market-setting"></a>Definição do mercado
O mercado tem de ser especificado no parâmetro de consulta no URL do pedido, caso contrário, o corretor ortográfico ocupará o mercado padrão com base no endereço IP.


## <a name="post-vs-get"></a>POST ou GET

A API suporta HTTP POST ou HTTP GET. O que utiliza depende do tamanho do texto que pretende verificar. Se as cadeias de carateres tiverem sempre menos de 1.500 carateres, utilizaria um GET. Mas se quiser suportar cadeias de carateres até 10.000 carateres, utilize o POST. A cadeia de texto pode incluir qualquer caráter válido UTF-8.

O exemplo seguinte mostra um pedido POST para verificar a ortografia e a gramática de uma cadeia de texto. O exemplo inclui o parâmetro de consulta de [modo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#mode) para estar completo (poderia ter sido deixado de fora desde `mode` predefinições para Verificação). O parâmetro de consulta de [texto](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) contém a cadeia a ser verificada.
  
```  
POST https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?mode=proof&mkt=en-us HTTP/1.1  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
 
text=when+its+your+turn+turn,+john,+come+runing  
``` 

Se utilizar o HTTP GET, iria incluir o parâmetro de consulta `text` na cadeia de consulta do URL
  
O código a seguir mostra a resposta ao pedido anterior. A resposta contém um objeto [SpellCheck](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#spellcheck). 
  
```  
{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  
  
O campo [flaggedTokens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#flaggedtokens) apresenta uma lista de erros ortográficos e gramaticais que a API encontrou na cadeia de [texto](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text). O campo `token` contém a palavra a ser substituída. Utilizaria o deslocamento baseado em zero no campo `offset` para encontrar o token na cadeia `text`. Teria de substituir a palavra naquela localização pela palavra no campo `suggestion`. 

Se o campo `type` é RepeatedToken, ainda deveria substituir o token por `suggestion`, mas provavelmente também teria de remover o espaço à direita.

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Passos Seguintes

Para começar a trabalhar rapidamente com o seu primeiro pedido, veja [Fazer o Primeiro Pedido](quickstarts/csharp.md).

Familiarize-se com a [Referência da API de Verificação de Ortografia do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference). A referência contém a lista de pontos finais, cabeçalhos e parâmetros de consulta que teria de utilizar para pedir os resultados da pesquisa e as definições dos objetos de resposta. 

Não se esqueça de ler o artigo [Requisitos de Utilização e Apresentação do Bing](./useanddisplayrequirements.md) para que não infrinja nenhuma das regras relativas à utilização dos resultados.
