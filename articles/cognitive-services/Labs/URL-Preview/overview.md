---
title: O que é o projeto de URL de pré-visualização? -Serviços cognitivos Microsoft | Microsoft Docs
description: Introdução à pré-visualização do URL do projeto.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 6b486e0ab4092bef4fe829a5f166311a572a2900
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353989"
---
# <a name="what-is-project-url-preview"></a>O que é o projeto de URL de pré-visualização?
O ponto final de pré-visualização do URL assume um parâmetro de consulta de URL e devolve uma resposta JSON com o nome do recurso de destino, uma breve descrição e uma ligação a uma imagem para apresentar uma versão de pré-visualização. A resposta inclui também a [isFamilyFriendly](url-preview-reference.md#query-parameters) sinalizador que indica se o URL contém conteúdo inválido para adultos, pirated ou outro. 

Para obter resultados de pré-visualização de URL, submeta um pedido GET e incluir o *Ocp-Apim-Subscription-Key* cabeçalho com um token válido:  
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```
A resposta: 
````
HTTP Headers:
BingAPIs-TraceId: 3CC74C94769440C0851D9DF0869FCE7F
BingAPIs-SessionId: 52219085A6364692958C9C83983A0DBA
X-MSEdge-ClientID: 13D44DC2DE946B4B0F25460CDF036AD6
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 3CC74C94769440C0851D9DF0869FCE7F Ref B: CO1EDGE0315 Ref C: 2018-04-11T18:47:40Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https:\/\/swiftkey.com\/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https:\/\/swiftkey.com\/images\/og\/default.jpg"
  }
}

````
## <a name="scenarios"></a>Cenários 

A API de pré-visualização do URL suporta breves descrições dos recursos de Web. Os programadores utilizá-la para criar as experiências de pré-visualização avançado.  Os utilizadores podem partilhar ou marcar páginas Web, notícias de última hora, blogues, fóruns, etc. Esta API também pode ser utilizada para conteúdo moderação de interrupção.    

Aplicações utilizam pré-visualização do URL para enviar pedidos Web para o ponto final com uma consulta atribuída para o URL para pré-visualização.  A resposta JSON contém as informações de pré-visualização: nome, descrição do recurso, *familyFriendly* sinalizador e ligações que fornecem acesso a uma imagem de representativa e para o recurso completo online. 

## <a name="terms-of-use"></a>Termos de utilização
Utilize apenas os dados de pré-visualização do URL de projeto para apresentar fragmentos de pré-visualização e imagens em miniatura com hiperligação para os respetivos sites de origem, no URL iniciada pelo utilizador final de partilha no suporte de dados sociais, chat bot ou ofertas de semelhantes. D não cópia, armazenar, ou colocar em cache todos os dados recebidos da pré-visualização do URL de projeto. Honrar quaisquer pedidos para desativar previews que podem receber os proprietários de Web site ou o conteúdo.

Ou uma linha de terceiros em seu nome, poderá não utilizar, manter, armazenar, colocar em cache, partilha, ou distribuir quaisquer dados a partir da API de pré-visualização de URL para o teste, desenvolver, formação, distribuir ou disponibilizar-se de que qualquer serviço de terceiros ou funcionalidade. 

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](csharp.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Início rápido de JavaScript](javascript.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)
