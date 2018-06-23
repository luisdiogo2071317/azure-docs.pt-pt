---
title: Artigo personalização - serviços cognitivos do Azure | Microsoft Docs
description: Um tutorial para personalização do artigo com o serviço de decisão de personalizada no Azure, uma API baseada na nuvem para contextual decision-making.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh;marossi
ms.openlocfilehash: 35d0567f81a23d4726461059eb6fd31e04228697
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354589"
---
# <a name="article-personalization"></a>Personalização de artigo

Este tutorial centra-se em Personalizar a seleção dos artigos na página de front-de um Web site. O serviço de decisão personalizada afeta *vários* apresenta uma lista dos artigos no início de página, para a instância. Talvez a página é um site de notícias que abrange apenas a política e o desporto. Esta operação iria mostrar três classificada apresenta uma lista dos artigos: política, desporto e recente.

## <a name="applications-and-action-sets"></a>Aplicações e os conjuntos de ação

Eis como ajustar o seu cenário para a estrutura. Vamos imaginar três aplicações, um para cada lista que está a ser otimizada: política de aplicação, desporto de aplicação e aplicação recentes. Para especificar os artigos do candidato para cada aplicação, existem dois conjuntos de ação: um para a política e outro para desporto. A ação definida para aplicação recente inclui automaticamente como uma União de dois outros conjuntos.

> [!TIP]
> Conjuntos de ação podem ser partilhados em todas as aplicações no serviço de decisão personalizada.

## <a name="prepare-action-set-feeds"></a>Preparar os feeds de conjunto de ação

Serviço de decisão personalizado consome conjuntos de ação através de feeds RSS ou Atom fornecidos pelo cliente. Fornecer duas feeds: um para a política e outro para desporto. Suponha que são servidos de `http://www.domain.com/feeds/<feed-name>`.

Cada feed fornece uma lista dos artigos. No RSS, cada um deles é especificado por uma `<item>` elemento, da seguinte forma:

```xml
<rss version="2.0"><channel>
   <item>
      <title><![CDATA[article title]]></title>
      <link>"article url"</link>
      <pubDate>publication date</pubDate>
    </item>
</channel></rss>
```

A ordem é importante de artigos. Especifica a classificação predefinida, que é o melhor estimado para a forma como os artigos devem ser ordenados. Classificação predefinida, em seguida, é utilizada para a comparação do desempenho no [dashboard](#performance-dashboard).

Para obter mais informações sobre o formato de feed, consulte o [referência da API](custom-decision-service-api-reference.md#action-set-api-customer-provided).

## <a name="register-a-new-app"></a>Registar uma nova aplicação

1. Inicie sessão com a [conta Microsoft](https://account.microsoft.com/account). No Friso, clique em **My Portal**.

2. Para registar uma nova aplicação, clique em de **nova aplicação** botão.

    ![Portal do Serviço de Decisão Personalizada](./media/custom-decision-service-tutorial/portal.png)

3. Introduza um nome exclusivo para a sua aplicação no **ID da aplicação** caixa de texto. Se este nome já está a ser utilizado por outro cliente, o sistema pede-lhe para escolher um ID de aplicação diferente. Selecione o **avançadas** caixa de verificação e introduza o [cadeia de ligação](../../storage/common/storage-configure-connection-string.md) para a sua conta de armazenamento do Azure. Normalmente, utilizar a mesma conta de armazenamento para as suas aplicações.

    ![Caixa de diálogo nova aplicação](./media/custom-decision-service-tutorial/new-app-dialog.png)

    Depois de registar todas as aplicações de três no cenário acima, são listados:

    ![Lista de aplicações](./media/custom-decision-service-tutorial/apps.png)

    Pode voltar a esta lista, clicando a **aplicações** botão.

4. No **nova aplicação** diálogo caixa, especifique um feed de ação. Ação feeds também podem ser especificados ao clicar no **Feeds** botão e, em seguida, ao clicar no **Feed novo** botão. Introduza um **nome** do feed nova, introduza o **URL** do que é servido e introduza o **atualizar tempo**. A hora de atualização Especifica a frequência personalizada decisão serviço deve atualizar o feed.

    ![Caixa de diálogo feed novo](./media/custom-decision-service-tutorial/new-feed-dialog.png)

    Os feeds de ação podem ser utilizados por qualquer aplicação, independentemente de onde estão especificadas. Depois de especificar ambos os feeds de ação num cenário, são listados:

    ![Lista de feeds](./media/custom-decision-service-tutorial/feeds.png)

    Pode voltar a esta lista, clicando a **Feeds** botão.

## <a name="use-the-apis"></a>Utilizar as APIs

O serviço de decisão personalizada ranks artigos através da API de classificação. Para utilizar esta API, inserir o seguinte código no cabeçalho HTML da página de front:

```html
<!-- Define the "callback function" to render UI -->
<script> function callback(data) { … } </script>

<!--  call Ranking API after callback() is defined, separately for each app -->
<script src="https://ds.microsoft.com/api/v2/app-politics/rank/feed-politics" async></script>
<script src="https://ds.microsoft.com/api/v2/app-sports/rank/feed-sports" async></script>
<script src="https://ds.microsoft.com/api/v2/app-recent/rank/feed-politics/feed-sports" async></script>
<!-- NB: action feeds for 'app-recent' are listed one after another. -->
```

A resposta HTTP a partir da API de classificação é uma cadeia formatada de JSONP. Para a política de aplicação, por exemplo, a cadeia de aspeto:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"app-politics",
   "actionSets":[{"id":"feed-politics","lastRefresh":"date"}] });
```

O browser, em seguida, executa esta cadeia como uma chamada para o `callback()` função. O `data` argumento no `callback()` função contém o ID da aplicação e a classificação de URLs a compor. Em particular, `callback()` deve utilizar `data.appId` para distinguir entre as três aplicações. `eventId` é utilizada internamente pelo serviço de decisão personalizada para corresponder a classificação fornecido com clique correspondente, se aplicável.

> [!TIP]
> `callback()` Pode verificar cada ação feed para actualização utilizando o `lastRefresh` campo. Se um feed fornecido não é suficientemente raiz, `callback()` poderá ignorar a classificação fornecida, chame este feed diretamente e utilizar servida do feed de classificação predefinida.

Para obter mais informações sobre as especificações e opções adicionais fornecidas pela API classificação, consulte o [referência da API](custom-decision-service-api-reference.md).

As opções de artigo principal do utilizador são devolvidas ao chamar a API de Reward. Quando é recebida uma escolha de artigo principal, o seguinte código deve ser invocado na página de front:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/<eventId>',
    contentType: "application/json" })
```

Utilizar `appId` e `eventId` com o código de processamento de clique requer alguns cuidado. Por exemplo, pode implementar o `callback()` funciona da seguinte forma:

```javascript
function callback(data) {
    $.map(data.ranking, function (element) {
        //custom rendering function given content info
        render({appId:data.appId,
                article:element,
                onClick: element.id != data.rewardAction ? null :
                   function() {
                       $.ajax({
                       type: "POST",
                       url: '//ds.microsoft.com/api/v2/' + data.appId + '/reward/' + data.eventId,
                       contentType: "application/json" })}
                });
}}
```

Neste exemplo, implementar o `render()` função compor um artigo especificado para uma determinada aplicação. Esta função entradas o ID da aplicação e o artigo (no formato partir da API de classificação). O `onClick` parâmetro é a função que deve ser chamada a partir `render()` para lidar com um clique. Verifica se a, clique em é na ranhura de superior. Em seguida, chama a API de Reward com o ID de aplicação adequado e o ID de evento.

## <a name="next-steps"></a>Passos Seguintes

* Consulte o [referência da API](custom-decision-service-api-reference.md) para saber mais sobre a funcionalidade fornecida.