---
title: Gerir chaves
titleSuffix: Language Understanding - Azure Cognitive Services
description: Depois de criar uma chave de ponto final do LUIS no portal do Azure, atribua a chave para a aplicação do LUIS e obter o URL de ponto final correto. Utilize este URL de ponto final para LUIS, recebam previsões.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 0a278ded7ce290347645f345e4eee0b15972787f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088107"
---
# <a name="add-an-azure-luis-resource-to-app"></a>Adicionar um recurso do Azure LUIS à aplicação

Depois de criar um recurso de LUIS no portal do Azure, atribua o recurso para a aplicação do LUIS e obter o URL de ponto final correto. Utilize este URL de ponto final para LUIS, recebam previsões.

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>
<a name="assign-resource"></a>


## <a name="assign-resource-in-luis-portal"></a>Atribuir o recurso no Portal do LUIS

1. Criar uma chave de LUIS sobre o [portal do Azure](https://portal.azure.com). Para obter mais instruções, consulte [criar uma chave de ponto final a utilizar o Azure](luis-how-to-azure-subscription.md).
 
2. Selecione **Manage** no menu superior direito, em seguida, selecione **chaves e os pontos finais**.

    [ ![Página de chaves e os pontos finais](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

3. Para adicionar o LUIS, selecione **atribuir recursos +**.

    ![Atribuir um recurso à sua aplicação](./media/luis-manage-keys/assign-key.png)

4. Na caixa de diálogo associado ao endereço de e-mail que inicia sessão com o Web site do LUIS, selecione um inquilino.  

5. Escolha o **nome da subscrição** associados com o recurso do Azure que pretende adicionar.

6. Selecione o **nome do recurso de LUIS**. 

7. Selecione **atribuir recursos**. 

8. Localize a nova linha na tabela e copie o URL de ponto final. Ele é corretamente construído para fazer uma solicitação de HTTP GET para o ponto de final do LUIS para uma predição. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

### <a name="unassign-resource"></a>Anular a atribuição de recursos
Quando anular a atribuição de chave do ponto final, não é eliminado do Azure. Só é desassociado do LUIS. 

Quando uma chave de ponto final é não atribuída, ou não atribuída à aplicação, qualquer pedido para o ponto final do URL retorna um erro: `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Incluir todas as pontuações previstas de intenção
O **incluir todos os prever a intenção pontuações** caixa de seleção permite que a resposta de consulta de ponto final incluir a pontuação de predição para cada intenção. 

Esta definição permite que seu aplicativo de chamada de LUIS para tomar uma decisão programática com base em classificações dos objetivos retornados ou chatbot. Em geral dos principais dois objetivos são mais interessantes. Se a classificação superior é a intenção, que sua chatbot pode optar por fazer uma pergunta de seguimento que fizer uma escolha definitiva entre a intenção None e a intenção pontuação alta nenhum. 

As intenções e suas pontuações também estão incluídos os registos de ponto final. Pode [exportar](luis-how-to-start-new-app.md#export-app) esses registos e analisar as pontuações. 

```JSON
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

### <a name="enable-bing-spell-checker"></a>Ativar o Verificador de ortografia do Bing 
Na **definições de url de ponto final**, o **corretor ortográfico Bing** alternância permite o LUIS corrigir as palavras incorretas antes de predição. Criar uma  **[chave de verificação ortográfica do Bing](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. 

Adicionar a **verificação ortográfica = true** parâmetro de cadeia de consulta e o **bing-ortográfica-check-subscription-key = {YOUR_BING_KEY_HERE}** . Substitua o `{YOUR_BING_KEY_HERE}` com a sua chave de Verificador de ortografia do Bing.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```


### <a name="publishing-regions"></a>Regiões de publicação

Saiba mais sobre a publicação [regiões](luis-reference-regions.md) incluindo publicação na [Europa](luis-reference-regions.md#publishing-to-europe), e [Austrália](luis-reference-regions.md#publishing-to-australia). Regiões de publicação são diferentes das regiões de criação. Crie uma aplicação na região criação correspondente para a região de publicação que pretende para o ponto de extremidade de consulta.

## <a name="assign-resource-without-luis-portal"></a>Atribuir recursos sem portal de LUIS

Para fins de automatização, como um pipeline CI/CD, talvez queira automatizar a atribuição de um recurso de LUIS para uma aplicação do LUIS. Para que, terá de efetuar os seguintes passos:

1. Obter um Gestor de recursos do Azure token nesta [Web site](https://resources.azure.com/api/token?plaintext=true). Este token expirar então, utilizá-lo imediatamente. O pedido devolve um token do Azure Resource Manager.

    ![Pedir o token do Azure Resource Manager e receber o token do Azure Resource Manager](./media/luis-manage-keys/get-arm-token.png)

1. Utilizar o token para pedir os recursos de LUIS entre subscrições, a partir do [LUIS de introdução do azure de contas de API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), sua conta de utilizador tem acesso a. 

    Esta API de publicação requer as seguintes definições:

    |Cabeçalho|Valor|
    |--|--|
    |`Authorization`|O valor de `Authorization` é `Bearer {token}`. Tenha em atenção que o valor do token têm de ser precedido pela palavra `Bearer` e um espaço.| 
    |`Ocp-Apim-Subscription-Key`|Sua [chave de criação](luis-how-to-account-settings.md).|

    Esta API devolve uma matriz de objetos JSON das suas subscrições de LUIS, incluindo o ID de subscrição, o grupo de recursos e o nome de recurso, retornado como o nome da conta. Encontre um item na matriz que é o recurso de LUIS para atribuir à aplicação LUIS. 

1. Atribuir o token para o recurso de LUIS com o [atribuir um LUIS contas do azure a uma aplicação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) API. 

    Esta API de publicação requer as seguintes definições:

    |Tipo|Definição|Valor|
    |--|--|--|
    |Cabeçalho|`Authorization`|O valor de `Authorization` é `Bearer {token}`. Tenha em atenção que o valor do token têm de ser precedido pela palavra `Bearer` e um espaço.|
    |Cabeçalho|`Ocp-Apim-Subscription-Key`|Sua [chave de criação](luis-how-to-account-settings.md).|
    |Cabeçalho|`Content-type`|`application/json`|
    |Cadeia de consulta|`appid`|Pelo ID de aplicação do LUIS. 
    |Corpo||{"AzureSubscriptionId": "ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup 2",<br>"AccountName": "luis-uswest-S0-2"}|

    Quando esta API for bem sucedida, ele retorna um status de criado 201. 

## <a name="next-steps"></a>Passos Seguintes

Utilize a chave para publicar a sua aplicação no **publicar aplicação** página. Para obter instruções sobre a publicação, consulte [publicar aplicação](luis-how-to-publish-app.md).

Ver [chaves de LUIS](luis-concept-keys.md) para compreender conceitos-chave de criação e o ponto final LUIS.
