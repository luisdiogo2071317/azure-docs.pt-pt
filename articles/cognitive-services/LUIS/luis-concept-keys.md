---
title: Compreender as chaves de LUIS - Azure | Microsoft Docs
description: Utilize chaves de compreensão de idiomas (LUIS) para criar a sua aplicação e consultar o endpoing.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/23/2018
ms.author: v-geberr
ms.openlocfilehash: bf131877080f46781e74991ef627922384cc4c08
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355742"
---
# <a name="keys-in-luis"></a>Chaves na LUIS
LUIS utiliza duas chaves: [criação](#programmatic-key) e [endpoint](#endpoint-key). A chave de criação é criada automaticamente quando criar a sua conta LUIS. Quando estiver pronto para publicar a aplicação de LUIS, terá de [criar a chave de ponto final](luis-how-to-azure-subscription.md#create-luis-endpoint-key), [atribuí-la](Manage-keys.md#assign-endpoint-key) à sua aplicação LUIS, e [utilizá-la com a consulta de ponto final](#use-endpoint-key-in-query). 

|Chave|Objetivo|
|--|--|
|[Chave de criação](#programmatic-key)|Criação, a publicação, gestão de colaboradores, controlo de versões|
|[Chave de ponto final](#endpoint-key)| Consultar|

É importante criar aplicações LUIS [regiões](luis-reference-regions.md#publishing-regions) também para onde pretende publicar e consultar.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Chave de criação

Uma chave de criação, também conhecido como uma chave de arranque, é criada automaticamente quando criar uma conta de LUIS e é gratuito. Tem uma chave de criação em todas as suas aplicações para cada criação LUIS [região](luis-reference-regions.md). A chave de criação é fornecida para criar a sua aplicação LUIS ou para testar as consultas de ponto final. 

Para localizar a chave de criação, inicie sessão no [LUIS] [ LUIS] e clique no nome da conta na barra de navegação superior direito para abrir **as definições da conta**.

![Chave de criação](./media/luis-concept-keys/programatic-key.png)

Quando pretender tornar **consultas de ponto final de produção**, criar um Azure [subscrição LUIS](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Para sua comodidade, muitos dos exemplos de utilizam a chave de criação de conteúdos, uma vez que fornece algumas chamadas de ponto final no respetivo [quota](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Chave de ponto final
 Quando tiver **consultas de ponto final de produção**, crie um [chave LUIS](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) no portal do Azure. Lembre-se o nome utilizado para criar a chave, apenas quando adicionar a chave para a aplicação..

Quando o processo de subscrição LUIS estiver concluído, [adicionar a chave](Manage-keys.md#assign-endpoint-key) para a aplicação no **publicar** página. 

A chave de ponto final permite uma quota de pedidos de ponto final com base no plano de utilização que especificou ao criar a chave. Consulte [preços de serviços cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) para obter informações sobre preços.

A chave de ponto final pode ser utilizada para todas as suas aplicações LUIS ou para aplicações de LUIS específicas. 

Não utilize a chave de ponto final para a criação de aplicações de LUIS. 

## <a name="use-endpoint-key-in-query"></a>Chave de ponto final de utilização na consulta
O ponto final LUIS aceita dois estilos da consulta, ambos utilizam mas de chave, o ponto final em locais diferentes:

|Verbo|Localização de url e a chave de exemplo|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn nos lights<br><br>valor de cadeia de consulta para `subscription-key`<br><br>Altere o valor de consulta de ponto final para o `subscription-key` partir da chave de criação (arranque) para a nova chave de ponto final para utilizar a taxa de quota de chave de ponto final de LUIS. Se tiver criar a chave e atribuir a chave, mas não altere o valor de consulta de ponto final para a chave de subscrição ', não estiver a utilizar a sua quota de chave de ponto final.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a<br><br> valor de cabeçalho para `Ocp-Apim-Subscription-Key`<br><br>Altere o valor de consulta de ponto final para o `Ocp-Apim-Subscription-Key` partir da chave de criação (arranque) para a nova chave de ponto final para utilizar a taxa de quota de chave de ponto final de LUIS. Se criar a chave e atribuir a chave, mas não altere o valor de consulta de ponto final para `Ocp-Apim-Subscription-Key`, não estiver a utilizar a sua quota de chave de ponto final.|

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Utilização da API de Ocp-Apim-Subscription-Key
As APIs de LUIS utilizam o cabeçalho, `Ocp-Apim-Subscription-Key`. O nome do cabeçalho não se altera com base nas quais chave e o conjunto de APIs que está a utilizar. Defina o cabeçalho para a chave de criação para criação de APIs. Se estiver a utilizar o ponto final, defina o cabeçalho para a chave de ponto final. 

Não é possível transmitir a chave de ponto final para a criação de APIs. Se o fizer, receberá um erro 401 - acesso negado devido a chave de subscrição inválido. 

## <a name="key-limits"></a>Limites de chaves
Consulte [chave limites](luis-boundaries.md#key-limits) e [regiões do Azure](luis-reference-regions.md). A chave de criação está livre e utilizado para a criação. A chave de subscrição LUIS tem um escalão gratuito, mas tem de ser criada por si e associada com a sua aplicação LUIS no **publicar** página. Não pode ser utilizado para criação, mas apenas consultas de ponto final.

Publicação de regiões forem diferentes das regiões de criação. Certifique-se que criar uma aplicação na região criação correspondente à região de publicação que pretende.

## <a name="key-limit-errors"></a>Erros de chave de limite
Se for excedido o por segundo quota, receberá um erro de HTTP 429. Se for excedido o por quota do mês, receberá um erro de HTTP 403. Corrija estes erros por obter um LUIS [endpoint](#endpoint-key) chave, [atribuir](Manage-keys.md#assign-endpoint-key) a chave para a aplicação no **publicar** página do [LUIS] [ LUIS] Web site.

## <a name="next-steps"></a>Passos Seguintes

* Saiba [conceitos](Manage-Keys.md#assign-endpoint-key) sobre chaves de criação e o ponto final.

[LUIS]:luis-reference-regions.md#luis-website
