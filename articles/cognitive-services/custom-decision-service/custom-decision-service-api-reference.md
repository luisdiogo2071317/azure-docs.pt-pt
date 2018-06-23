---
title: API - serviços cognitivos do Azure | Microsoft Docs
description: Um completado e intuitivo API guia personalizada decisão do serviço Azure, uma API baseado na nuvem para a nível contextual das decision-making que sharpens com experiência.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/11/2018
ms.author: slivkins
ms.reviewer: marcozo, alekh
ms.openlocfilehash: 403b17e33394016a07a7b33ba1bcbfe6afdcc05b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354950"
---
# <a name="api"></a>API

Serviço de decisão de personalizada do Azure fornece duas APIs que são chamadas para cada decisão: o [classificação API](#ranking-api) para a classificação das ações de entrada e de [Reward API](#reward-api) para o reward de saída. Além disso, terá de fornecer um [a API de definir ação](#action-set-api-customer-provided) para especificar as ações de serviço de decisão personalizada do Azure. Este artigo aborda estas três APIs. Um cenário típico é utilizado abaixo para mostrar quando o serviço de decisão personalizada otimiza a classificação dos artigos.

## <a name="ranking-api"></a>Classificação de API

A API de classificação utiliza um padrão [JSONP](https://en.wikipedia.org/wiki/JSONP)-padrão de comunicação de estilo para otimizar a latência e ignorar o [política da mesma origem](https://en.wikipedia.org/wiki/Same-origin_policy). A última opção proíbe JavaScript obter dados a partir de fora da origem da página.

Inserir este fragmento do cabeçalho HTML da sua página (onde são apresentadas uma lista dos artigos personalizada):

```html
// define the "callback function" to render UI
<script> function callback(data) { … } </script>
// "data" provides the ranking of URLs, see example below.

// call to Ranking API
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>?<parameters>" async></script>
// action set id is the name of the corresponding RSS/Atom feed.

// same call with multiple action sets:
// <script src="https://ds.microsoft.com/api/v2/<appId>/rank/<A1>/<A2>/.../<An>?<parameters>" async></script>
```

> [!IMPORTANT]
> A função de chamada de retorno tem de ser definida antes da chamada para a API de classificação.

> [!TIP]
> Para melhorar a latência, a API de classificação é exposta através de HTTP em vez de HTTPS, como no `http://ds.microsoft.com/api/v2/<appId>/rank/*`.
> No entanto, um ponto final de HTTPS tem de ser utilizado se a página é fornecida através de HTTPS.

Quando não forem utilizados parâmetros, a resposta HTTP a partir da API de classificação é uma cadeia formatada em JSONP:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<A2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]});
```

O browser, em seguida, executa esta cadeia como uma chamada para o `callback()` função.

O parâmetro da função de chamada de retorno no exemplo anterior tem o esquema seguinte:

- `ranking` Fornece a classificação de URLs a apresentar.
- `eventId` é utilizada internamente pelo serviço de decisão personalizado para fazer corresponder esta classificação com os cliques correspondentes.
- `appId` permite que a função de chamada de retorno distinguir entre várias aplicações do serviço de decisão de personalizado em execução na mesma página Web.
- `actionSets` Apresenta uma lista de cada ação Definir utilizada na chamada à API de classificação, juntamente com o carimbo de UTC da última atualização com êxito. Decisão de serviços personalizados, atualiza periodicamente os feeds de conjunto de ação. Por exemplo, se não forem alguns dos conjuntos de ação atuais, a função de chamada de retorno poderá ter de reverter para a respetiva classificação predefinida.

> [!IMPORTANT]
> Os conjuntos de ação especificada são processados e possivelmente eliminados para formar de classificação predefinida dos artigos. Classificação predefinida, em seguida, obtém reordenar e devolvida a resposta HTTP. Classificação predefinida é definida aqui:
>
> - Dentro de cada conjunto de ação, os artigos são eliminados os 15 artigos mais recente (se são devolveu mais do que 15).
> - Quando são especificados vários conjuntos de ação, estes são intercaladas pela mesma ordem que aparece na chamada à API. A ordenação dos artigos original é preservada dentro de cada conjunto de ação. Duplicados são removidos favor as cópias anteriores.
> - O primeiro `n` artigos são mantidos na lista intercalada de artigos, onde `n=20` por predefinição.

### <a name="ranking-api-with-parameters"></a>Classificação API com parâmetros

A API de classificação permite estes parâmetros:

- `details=1` e `details=2` insere detalhes adicionais sobre cada artigo listado no `ranking`.
- `limit=<n>` Especifica o número garantido de artigos na classificação predefinida. `n` tem de estar entre `2` e `30` (ou senão é truncado para `2` ou `30`, respetivamente).
- `dnt=1` desativa os cookies de utilizador.

Os parâmetros podem ser combinados na padrão, sintaxe de cadeia de consulta, por exemplo `details=2&dnt=1`.

> [!IMPORTANT]
> A predefinição na Europa deve ser `dnt=1` até que o cliente aceita a faixa de cookie. Também deve ser a predefinição de Web sites que lesar de destino. Para obter mais informações, consulte o [termos de utilização](https://www.microsoft.com/cognitive-services/en-us/legal/CognitiveServicesTerms20160804).

O `details=1` elemento insere cada artigo `guid`, se servidos através da API de definir a ação. A resposta HTTP:

```json
callback({
   "ranking":[{"id":"url1","details":[{"guid":"123"}]},
              {"id":"url2","details":[{"guid":"456"}]},
              {"id":"url3","details":[{"guid":"789"}]}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

O `details=2` elemento adiciona mais detalhes que o serviço de decisão personalizada poderá extrair do metatags SEO dos artigos [featurization código](https://github.com/Microsoft/mwt-ds/tree/master/Crawl):

- `title` de `<meta property="og:title" content="..." />` ou `<meta property="twitter:title" content="..." />` ou `<title>...</title>`
- `description` de `<meta property="og:description" ... />` ou `<meta property="twitter:description" content="..." />` ou `<meta property="description" content="..." />`
- `image` do `<meta property="og:image" content="..." />`
- `ds_id` do `<meta name=”microsoft:ds_id” content="..." />`

A resposta HTTP:

```json
callback({
   "ranking":[{"id":"url1","details":<details>},
              {"id":"url2","details":<details>},
              {"id":"url3","details":<details>}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

O `<details>` elemento:

```json
[{"guid":"123"}, {"description":"some text", "ds_id":"234", "image":"ImageUrl1", "title":"some text"}]
```

## <a name="reward-api"></a>Reward API

Utiliza personalizada do serviço de decisão clica apenas na ranhura de superior. Cada clique é interpretado como um reward de 1. A falta de um clique é interpretada como um reward 0. Cliques são feita a correspondência com as classificações correspondentes ao utilizar os IDs de evento, que foram gerados pelo [classificação API](#ranking-api) chamada. Se for necessário, eventos IDs podem ser transmitidos através de cookies de sessão.

Para lidar com um clique num bloco de principal, coloque este código na sua página de front:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Aqui `data` é o argumento para o `callback()` funcione, conforme descrito anteriormente. Utilizar `data` no, clique em código de processamento requer alguns cuidado. É apresentado um exemplo deste [tutorial](custom-decision-service-tutorial-news.md#use-the-apis).

Para testar apenas, a API de Reward pode ser chamada através de [cURL](https://en.wikipedia.org/wiki/CURL):

```sh
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST -d 1 -H "Content-Type: application/json"
```

O efeito esperado é uma resposta HTTP de 200 (OK). Pode ver o reward de 1 para este evento no registo (se uma chave de conta de armazenamento do Azure foi fornecida no portal).

## <a name="action-set-api-customer-provided"></a>API definir ação (fornecido ao cliente)

Num nível elevado, a API de definir a ação devolve uma lista dos artigos (ações). Cada artigo é especificado pelo respetivo URL e (opcionalmente) o título do artigo e a data de publicação. Pode especificar que várias ação define no portal. Uma API de conjunto de ação diferentes devem ser utilizada para cada conjunto de ação, como um URL diferente.

Cada API definir de ação pode ser implementado de duas formas: como um feed RSS ou como um feed Atom. Quer um deve estar em conformidade com a norma e devolva um XML correto. Para RSS, eis um exemplo:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <guid>guid (could be a your internal database id)</guid>
      <pubDate>date</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Cada nível superior `<item>` elemento descreve uma ação:

- `<link>` é obrigatório e é utilizado como um ID de ação.
- `<date>` é ignorada se é menor ou igual a 15 itens; caso contrário, é obrigatório.
  - Se existirem mais de 15 itens, são utilizadas 15 as mais recentes.
  - Tem de ter o formato de padrão para RSS ou Atom, respetivamente:
    - [RFC 822](https://tools.ietf.org/html/rfc822) para RSS: por exemplo, `"Fri, 28 Apr 2017 18:02:06 GMT"`
    - [RFC 3339](https://tools.ietf.org/html/rfc3339) para Atom: por exemplo, `"2016-12-19T16:39:57-08:00"`
- `<title>` é opcional e é utilizado para gerar as funcionalidades que descrevem o artigo.
- `<guid>` é opcional e transmitido através do sistema para a função de chamada de retorno (se o `?details` parâmetro é especificado na chamada à API de classificação).

Outros elementos no interior de um `<item>` são ignorados.

O Atom feed versão utiliza a mesma sintaxe XML e convenções.

> [!TIP]
> Se o sistema utiliza o seu próprio artigo IDs, podem ser transmitidos para a função de chamada de retorno por utilizando `<guid>`.