---
title: Chamar a API a partir de uma aplicação - serviços cognitivos do Azure | Microsoft Docs
description: Como começar com o serviço de decisão de personalizada do Azure se chamar as APIs a partir de uma aplicação de smartphone.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/10/2018
ms.author: slivkins
ms.reviewer: marcozo, alekh
ms.openlocfilehash: 2d02b0deaaa701dd0b4818638827c04e2c946558
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355693"
---
# <a name="call-api-from-an-app"></a>Chamar a API numa aplicação

Efetuar chamadas para as APIs de serviço de decisão de personalizada do Azure a partir de uma aplicação de smartphone. Este artigo explica como começar com algumas opções básicas.

Certifique-se para [registar a sua aplicação](custom-decision-service-get-started-register.md), primeiro.

Existem dois chamadas da API da sua aplicação de smartphone efetuar ao serviço de decisão personalizada: uma chamada para a API de classificação para obter uma lista de conteúdo e uma chamada à API do Reward reportar um reward classificados. Eis o exemplo chama [cURL](https://en.wikipedia.org/wiki/CURL).

Para obter mais informações, consulte a referência [API](custom-decision-service-api-reference.md).

Começar a utilizar a chamada para a API de classificação. Criar o ficheiro `<request.json>`, que tem a ação definida ID. Este ID é o nome do RSS correspondente ou Atom feed que introduziu no portal:

```json
{"decisions":
     [{ "actionSets":[{"id":{"id":"<actionSetId>"}}] }]}
```

Vários conjuntos de ação podem ser especificados da seguinte forma:

```json
{"decisions":
    [{ "actionSets":[{"id":{"id":"<actionSetId1>"}},
                     {"id":{"id":"<actionSetId2>"}}] }]}
```

Este ficheiro JSON, em seguida, é enviado como parte do pedido de classificação:

```shell
curl -d @<request.json> -X POST https://ds.microsoft.com/api/v2/<appId>/rank --header "Content-Type: application/json"
```

Aqui, `<appId>` é o nome da sua aplicação é registado no portal. Deverá receber um conjunto ordenado de itens de conteúdo, o que é possível compor o na sua aplicação. Exemplo de retorno tem o seguinte:

```json
[{ "ranking":[{"id":"actionId3"}, {"id":"actionId1"}, {"id":"actionId2"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "rewardAction":"actionId3",
   "actionSets":[{"id":"<actionSetId1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<actionSetId2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]}]
```

A primeira parte de retorno tem uma lista de ações ordenadas, especificado pelo respetiva IDs de ação. Para um artigo, o ID de ação é um URL. O pedido geral também tem um único `<eventId>`, criada pelo sistema.

Mais tarde, pode especificar se observados um clique no primeiro item de conteúdo neste caso, o que é `<actionId3>`. Em seguida, pode comunicar um reward neste `<eventId>` ao serviço de decisão de personalizado através da API de Reward, por outro pedido, tal como:

```shell
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST
```

Por fim, terá de fornecer a ação definida API, que devolve a lista de artigos (ações) ser considerados pelo serviço de decisão personalizada. Implemente esta API como um feed RSS, conforme mostrado aqui:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <pubDate>Thu, 27 Apr 2017 16:30:52 GMT</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Aqui, cada nível superior `<item>` elemento descreve um artigo. O `<link>` é obrigatório e é utilizado como um ID de ação pelo serviço de decisão personalizada. Especifique `<date>` (no formato RSS standard) se tiver mais do que 15 artigos. São utilizados os 15 artigos mais recentes. O `<title>` é opcional e é utilizado para criar as funcionalidades relacionadas com o texto para o artigo.

## <a name="next-steps"></a>Passos Seguintes

* Trabalhar através de um [tutorial](custom-decision-service-tutorial-news.md) para obter um exemplo mais aprofundado.
* Consulte a referência [API](custom-decision-service-api-reference.md) para saber mais sobre a funcionalidade fornecida.