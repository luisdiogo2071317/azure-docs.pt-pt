---
title: Publique a sua aplicação LUIS | Documentos da Microsoft
description: Depois de criar e testar a sua aplicação com a compreensão de idiomas (LUIS), publique-o como um serviço web no Azure.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 0f6ba76ede48502e4f3308b11d41bf141f0188fb
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927779"
---
# <a name="publish-your-trained-app"></a>Publique a sua aplicação preparada
Quando concluir a criação e teste a sua aplicação LUIS, publicá-lo. Depois da aplicação for publicada, a página de publicação mostra todas as associadas HTTP [pontos de extremidade](luis-glossary.md#endpoint). Estes pontos finais, por [região](luis-reference-regions.md) e, por [chave](luis-how-to-manage-keys.md), em seguida, são integradas em qualquer aplicação cliente, chatbot ou back-end. 

Pode sempre [testar](interactive-test.md) seu aplicativo antes de o publicar. 

## <a name="production-and-staging-slots"></a>Produção e blocos de teste
Pode publicar a sua aplicação para o **bloco de teste** ou o **bloco de produção**. Ao utilizar duas ranhuras de publicação, isto permite-lhe ter duas versões diferentes com pontos de extremidade publicados ou a mesma versão em dois pontos de extremidade diferentes. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Requer a configuração das definições do modelo de publicação
Publique o ponto final após as alterações às seguintes definições. 

## <a name="external-services-settings"></a>Definições de serviços externos
As definições de serviço externo incluem **[análise de sentimentos](#enable-sentiment-analysis)** e  **[voz nisto](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Ativar a análise de sentimentos
No **definições de serviços externo**, o **ativar análise de sentimentos** caixa de seleção permite que o LUIS integrar com [análise de texto](https://azure.microsoft.com/services/cognitive-services/text-analytics/) para fornecer o sentimento e as expressões-chave análise. Não é necessário fornecer uma chave de análise de texto e não sem custos de faturação para este serviço para a sua conta do Azure. Depois de verificar esta definição, é persistente. 

Dados de sentimento são uma pontuação entre 1 e 0 indicando o positivo (mais de perto como 1) nem negativa (mais próximo de 0) sentimentos dos dados.

Para obter mais informações sobre a resposta do ponto final JSON com a análise de sentimentos, consulte [análise de sentimentos](luis-concept-data-extraction.md#sentiment-analysis)

### <a name="enable-speech-priming"></a>Ativar priming de voz 
Na **definições de serviços externo**, o **ativar de voz** caixa de seleção permite-lhe ter um único ponto final para obter uma expressão falada de um aplicativo de chamada de LUIS ou chatbot e receber um LUIS resposta de predição. O priming de voz utiliza o serviço cognitivo [API de voz](../Speech-Service/rest-apis.md). 

![Imagem da caixa de diálogo de confirmação de priming de voz](./media/luis-how-to-publish-app/speech-prime-modal.png)

Depois desta funcionalidade está ativada, publique a sua aplicação. Quando publica a aplicação do LUIS, seu modelo de aplicação é enviado para o seu próprio serviço de voz avisar o serviço de voz. Suas informações de modelo são **não** utilizado fora de seu próprio serviço. 

Para concluir o uso de priming de voz, terá das seguintes informações para utilizar o [SDK de voz](../speech-service/speech-sdk-reference.md):
* Uma chave de ponto final do LUIS.
* O ID de aplicação LUIS.
* Um domínio de ponto de extremidade, chamado de "Nome de anfitrião" no SDK de voz, como "westus.api.cognitive.microsoft.com", em que o subdomínio primeiro é a região onde a aplicação for publicada.

Para obter mais informações, consulte a [conversão de voz em intenção](http://aka.ms/speechsdk) exemplo.

Se a sua aplicação LUIS é eliminada ou o serviço de voz for eliminado, dados do seu modelo são removidos. 

## <a name="endpoint-url-settings"></a>Definições de URL de ponto final
Definições de serviços de URL de ponto de extremidade incluem **[fuso horário](#set-timezone-offset)** deslocamento,  **[todos prever a intenção pontuações](#include-all-predicted-intent-scores)**, e  **[ Verificador de ortografia do Bing](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Desvio de fuso horário do conjunto 
Parte a escolha de ranhura é a seleção do fuso horário. Esta definição de fuso horário permite o LUIS para [alter](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) sempre datetimeV2 pré-criados valores durante a predição, para que os dados de entidade retornada estejam corretos, de acordo com o fuso horário selecionado. 

### <a name="include-all-predicted-intent-scores"></a>Incluir todas as pontuações previstas de intenção
O **incluir todos os prever a intenção pontuações** caixa de seleção permite que a resposta de consulta de ponto final incluir a pontuação de predição para cada intenção. 

Esta definição permite que seu aplicativo de chamada de LUIS para tomar uma decisão programática com base em classificações dos objetivos retornados ou chatbot. Em geral dos principais dois objetivos são mais interessantes. Se a classificação superior é a intenção, que sua chatbot pode optar por fazer uma pergunta de seguimento que fizer uma escolha definitiva entre a intenção None e a intenção pontuação alta nenhum. 

As intenções e suas pontuações também estão incluídos os registos de ponto final. Pode [exportar](luis-how-to-start-new-app.md#export-app) esses registos e analisar as pontuações. 

```
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
Na **definições de url de ponto final**, o **corretor ortográfico Bing ativar** caixa de seleção permite que o LUIS corrigir as palavras incorretas antes de predição. Isso exige que crie uma  **[chave de verificação ortográfica do Bing](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. Assim que a chave é criada, dois parâmetros de cadeia de consulta são adicionados para o URL de ponto final na página de publicação. 

Se constrói suas próprias URLs para a sua aplicação de chamada de LUIS, certifique-se de que o **verificação ortográfica = true** parâmetro de cadeia de consulta e o **bing-ortográfica-check-subscription-key = {YOUR_BING_KEY_HERE}**. Substitua o `{YOUR_BING_KEY_HERE}` com a sua chave de Verificador de ortografia do Bing.

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

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Publique a sua aplicação treinada para um ponto de final HTTP
Abra a sua aplicação ao clicar em seu nome na **as minhas aplicações** página e, em seguida, clique em **Publish** no painel superior. 

![Publicar página-](./media/luis-how-to-publish-app/publish-to-production.png)
 
Quando a aplicação é publicada com êxito, é apresentada uma notificação de êxito verde na parte superior do navegador. 

* Escolha se pretende publicar no **produção** ou a **teste** ao selecionar no menu suspenso sob **ranhura selecione**. 

## <a name="assign-key"></a>Atribuir a chave

Se pretender utilizar uma chave que o Starter_Key livre apresentado, clique nas **Adicionar chave** botão. Esta ação abre uma caixa de diálogo que permite-lhe selecionar uma chave de ponto final existente para atribuir à aplicação. Para obter mais informações sobre como criar e adicionar chaves de ponto final a sua aplicação LUIS, consulte [gerir as chaves de](luis-how-to-manage-keys.md).

Para ver os pontos finais e as chaves associadas a outras regiões, utilize os botões de opção para alternar regiões. Cada linha na **recursos e as chaves** tabela lista os recursos do Azure associados à sua conta e as chaves de ponto final associadas a esse recurso.

## <a name="endpoint-url-construction"></a>Construção de URL de ponto final
O URL de ponto de extremidade corresponde à região do Azure associado à chave de ponto final.

Esta tabela reflete convenientemente a configuração da publicação no ponto final do URL com opções de rota e valores de cadeia de caracteres de consulta. Se constrói suas URLs de ponto final para a sua aplicação de chamada de LUIS, certifique-se estes mesmo rotas e a cadeia de consulta os valores estão definidos para o ponto final utilizado - se de que pretende definir.

A rota de URL é construído de acordo com a região e o ID da aplicação. Se estiver publicando noutras regiões ou com outras aplicações, o URL de ponto final pode ser construído, alterando os valores de ID de região e aplicativo. 

* Selecione o bloco Production (Produção) e o botão **Publish** (Publicar). Quando a publicação for bem sucedida, utilize o URL de ponto final exibida para aceder à sua aplicação LUIS. 

### <a name="optional-query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta opcionais
Os seguintes parâmetros de cadeia de caracteres de consulta podem ser utilizados com o URL de ponto final:

<!-- TBD: what about speech priming? -->

|Cadeia de consulta|Tipo|Valor de exemplo|Objetivo|
|--|--|--|--|
|verboso|boolean|true|Incluem [todas as pontuações de intenção](#include-all-predicted-intent-scores) para expressão|
|timezoneOffset|número (a unidade é minutos)|60|Definir [desvio de fuso horário](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) para [datetimeV2 entidades pré-concebidas](luis-reference-prebuilt-datetimev2.md)|
|verificação ortográfica|boolean|true|[Corrija a ortografia](#enable-bing-spell-checker) de expressão – utilizado em conjunto com o parâmetro de cadeia de caracteres de consulta do bing-ortográfica-check-subscription-key|
|Bing-ortográfica-check-subscription-key|ID de subscrição||utilizado em conjunto com o parâmetro de cadeia de caracteres de consulta de verificação ortográfica|
|de teste|boolean|false|Selecione o ponto final de preparo ou de produção|
|registo|boolean|true|Adicionar consulta e os resultados para iniciar sessão|


## <a name="test-your-published-endpoint-in-a-browser"></a>Testar o ponto de final publicado num browser
Testar o ponto de final publicado ao selecionar o URL na **ponto final** coluna. O browser predefinido abre-se com o URL gerado. Defina o parâmetro de URL "& p" para a consulta de teste. Por exemplo, acrescentar `&q=Book me a flight to Boston on May 4` para o URL e, em seguida, prima Enter. O browser apresenta a resposta JSON do ponto final do HTTP. 

![Resposta JSON de um ponto de final HTTP publicado](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Passos Seguintes

* Ver [gerir chaves](./luis-how-to-manage-keys.md) adicionar chaves à sua aplicação do LUIS, e saber mais sobre como chaves de mapeiam para as regiões.
* Ver [treinar e testar a aplicação](interactive-test.md) para obter instruções sobre como testar a aplicação publicada na consola de teste.
