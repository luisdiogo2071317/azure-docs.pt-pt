---
title: Chamar API a partir de um browser - serviços cognitivos do Azure | Microsoft Docs
description: Como começar com o serviço de decisão do Azure personalizados para otimizar uma página Web por efetuar chamadas à API diretamente a partir de um browser.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins,marcozo,alekh
ms.openlocfilehash: 10236c9d8f70d9b90a896464b4f86a847ee904c2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354746"
---
# <a name="call-api-from-a-browser"></a>Chamar a API num browser

Este artigo ajuda-o a efetuar chamadas para as APIs de serviço do Azure personalizada decisão diretamente a partir de um browser.

Certifique-se para [registar a sua aplicação](custom-decision-service-get-started-register.md), primeiro.

Vamos começar. A aplicação é modelada como tendo uma página de front-, que contém ligações para várias páginas de artigo. A página utiliza o serviço de decisão personalizada para especificar a ordenação das respetivas páginas de artigo. Inserir o seguinte código no cabeçalho HTML da página de front:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

O `data` argumento contém a classificação de URLs a compor. Para obter mais informações, consulte a referência [API](custom-decision-service-api-reference.md).

Para processar um clique de utilizador num artigo superior, chame o seguinte código na página de front:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Aqui, `data` é o argumento para o `callback()` função. Pode encontrar um exemplo de implementação deste [tutorial](custom-decision-service-tutorial-news.md#use-the-apis).

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