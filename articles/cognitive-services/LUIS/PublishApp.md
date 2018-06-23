---
title: Publicar a aplicação de LUIS | Microsoft Docs
description: Depois de criar e testar a aplicação através da utilização de compreensão de idiomas (LUIS), publicá-lo como um serviço web no Azure.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 12a63e65a739be08d436f8f1b53df566255b1fb1
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36322059"
---
# <a name="publish-your-trained-app"></a>Publicar a aplicação preparada
Quando acabar de criar e testar a sua aplicação LUIS, publicá-lo. Depois da aplicação for publicada, a página de publicar mostra todas as associadas HTTP [pontos finais](luis-glossary.md#endpoint). Estes pontos finais, por [região](luis-reference-regions.md) e por [chave](Manage-Keys.md), em seguida, estão integradas com qualquer aplicação cliente, chatbot ou back-end. 

Pode sempre [testar](interactive-test.md) a aplicação antes de a publicar. 

## <a name="production-and-staging-slots"></a>De produção e de ranhuras de teste
Pode publicar a aplicação para o **bloco de transição** ou **ranhura de produção**. Ao utilizar dois ranhuras de publicação, isto permite-lhe ter duas versões diferentes com pontos finais publicadas ou a mesma versão em dois pontos finais diferentes. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Necessita de configuração de definições de modelo de publicação
Publique o ponto final após as alterações para as seguintes definições. 

## <a name="external-services-settings"></a>Definições de serviços externos
As definições de serviço externo incluem **[análise de dados de sentimento](#enable-sentiment-analysis)** e  **[voz Priming](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Ativar análise de dados de sentimento
No **as definições de serviços externos**, a **ativar análise de dados de sentimento** permite LUIS integrar com a caixa de verificação [análise de texto](https://azure.microsoft.com/services/cognitive-services/text-analytics/) para fornecer dados de sentimento e uma expressão de chave Analysis Services. Não é necessário fornecer uma chave de análise de texto e existe sem encargos de faturação para este serviço à sua conta do Azure. Depois de verificar esta definição, é persistente. 

Dados de sentimento são uma pontuação entre 1 e 0 que indica o positivo (próximo de 1) ou negativa (próximo de 0) sentimento dos dados.

Para mais informações sobre a resposta do ponto final JSON com a análise de dados de sentimento, consulte [Sentiment analysis](luis-concept-data-extraction.md#sentiment-analysis)

### <a name="enable-speech-priming"></a>Ativar priming de reconhecimento de voz 
No **as definições de serviços externos**, a **ativar Priming de reconhecimento de voz** caixa de verificação permite-lhe ter um único ponto final para obter um utterance ditas a partir de uma aplicação de chamar LUIS ou chatbot e receber um LUIS resposta de predição. O priming de reconhecimento de voz utiliza o serviço cognitivos [API de reconhecimento de voz](../Speech-Service/rest-apis.md). 

![Imagem da caixa de diálogo de confirmação de priming de reconhecimento de voz](./media/luis-how-to-publish-app/speech-prime-modal.png)

Depois desta funcionalidade está ativada, publica a aplicação. Quando publicar a aplicação de LUIS, o seu modelo de aplicação é enviado para o seu próprio serviço de reconhecimento de voz para prime o serviço de reconhecimento de voz. As informações de modelo são **não** utilizada fora do seu próprio serviço. 

Para concluir a utilização de priming de reconhecimento de voz, terá as seguintes informações para utilizar o [SDK de reconhecimento de voz](../speech-service/speech-sdk-reference.md):
* Uma chave de subscrição de LUIS.
* O ID de aplicação LUIS.
* Um domínio de ponto final, referido como "Nome do anfitrião" no SDK de reconhecimento de voz, tais como "westus.api.cognitive.microsoft.com", em que o primeiro subdomínio é a região onde a aplicação publicada.

Para obter mais informações, consulte o [reconhecimento de voz a intenção](http://aka.ms/speechsdk) exemplo.

Quando a aplicação de LUIS foi eliminada ou o serviço de reconhecimento de voz é eliminado, os dados do modelo são removidos. 

## <a name="endpoint-url-settings"></a>Definições de URL de ponto final
Definições de serviços do URL de ponto final incluem **[fuso horário](#set-timezone-offset)** deslocamento,  **[prever todas as pontuações intenção](#include-all-predicted-intent-scores)**, e  **[ Verificador de ortográfica do Bing](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Desvio de fuso horário do conjunto 
Parte a escolha da ranhura é a seleção do fuso horário. Esta definição de fuso horário permite LUIS para [alter](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) sempre prebuilt datetimeV2 valores durante a predição de modo a que os dados de entidade devolvidos estão corretos, de acordo com o fuso horário selecionado. 

### <a name="include-all-predicted-intent-scores"></a>Incluir todas as pontuações previstas intenção
O **prever a incluir todas as pontuações intenção** a resposta de consulta de ponto final incluir a classificação de predição para cada intenção permite a caixa de verificação. 

Esta definição permite a chatbot ou chamar LUIS aplicação para tomar uma decisão programática com base nas pontuações dos pendentes devolvidos. Geralmente, os dois pendentes principais são mais interessante. Se a classificação superior é a intenção, a que sua chatbot pode optar por fazer uma pergunta seguimento que faz uma escolha definitiva entre a intenção nenhum e a intenção de classificação de elevado. 

Os pendentes e os respetivos pontuações também estão incluídos os registos de ponto final. Pode [exportar](create-new-app.md#export-app) esses registos e analisar as pontuações. 

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

### <a name="enable-bing-spell-checker"></a>Ativar o Verificador de ortográfica do Bing 
No **definições de url de ponto final**, a **Verificador de ortográfica do Bing ativar** caixa de verificação permite LUIS corrigir palavras com erros ortográficos antes de predição. Isto requer a criar um  **[chave de verificação de ortográfica do Bing](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. Assim que a chave for criada, são adicionados dois parâmetros de consulta para o URL de ponto final na página de publicar. 

Se estiver construir os suas próprias URLs para a sua aplicação chamar LUIS, certifique-se a **spellCheck = true** querystring parâmetro e o **bing ortográfica-verifique-subscrição-chave = {YOUR_BING_KEY_HERE}**. Substitua o `{YOUR_BING_KEY_HERE}` com a sua chave de Verificador de ortográfica do Bing.

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

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Publicar a aplicação preparada para um ponto final HTTP
Abra a aplicação ao clicar em seu nome no **aplicações My** página e, em seguida, clique em **publicar** no painel superior. 

![Publicar página-](./media/luis-how-to-publish-app/publish-to-production.png)
 
Quando a aplicação publicada com êxito, é apresentada uma notificação de sucesso verde na parte superior do browser. 

* Escolha se pretende publicar **produção** ou **transição** selecionando a partir do menu pendente em **ranhura selecione**. 

## <a name="assign-key"></a>Atribuir chave

Se pretender utilizar uma chave que o Starter_Key livre apresentado, clique em de **Adicionar chave** botão. Esta ação abre uma caixa de diálogo permite-lhe selecionar uma chave de ponto final existente para atribuir à aplicação. Para obter mais informações sobre como criar e adicionar as chaves de ponto final para a sua aplicação LUIS, consulte [gerir as chaves](Manage-Keys.md).

Para ver os pontos finais e as chaves associadas a outras regiões, utilize os botões de opção para alternar regiões. Cada linha no **recursos e as chaves** tabela apresenta uma lista de recursos do Azure associados à sua conta e as chaves de ponto final associadas a esse recurso.

## <a name="endpoint-url-construction"></a>Construção de URL de ponto final
O URL de ponto final corresponde à região do Azure associada à chave de ponto final.

Esta tabela comodamente reflete a configuração de publicação no ponto final URL com opções de rota e valores de cadeia de consulta. Se estiver construir o URL de ponto final para a sua aplicação chamar LUIS, certifique-se estas rotas mesmas e a cadeia de consulta os valores são definidos para o ponto final utilizado - se de que quer que eles definido.

A rota de URL está construída com a região e o ID de aplicação. Se está a publicar noutras regiões ou com outras aplicações, o URL de ponto final pode ser construído, alterando os valores de ID de região e aplicações. 

* Selecione a ranhura de produção e o **publicar** botão. Quando a publicação for bem sucedida, utilize o URL de ponto final apresentadas para aceder à sua aplicação LUIS. 

### <a name="optional-query-string-parameters"></a>Parâmetros de cadeia de consulta opcional
Os seguintes parâmetros de cadeia de consulta podem ser utilizados com o URL de ponto final:

<!-- TBD: what about speech priming? -->

|Cadeia de consulta|Tipo|Valor de exemplo|Objetivo|
|--|--|--|--|
|verboso|boolean|true|Incluir [todas as pontuações intenção](#include-all-predicted-intent-scores) para utterance|
|timezoneOffset|número (unidade é minutos)|60|Definir [desvio de fuso horário](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) para [datetimeV2 prebuilt entidades](luis-reference-prebuilt-datetimev2.md)|
|spellCheck|boolean|true|[Corrija ortografia](#enable-bing-spell-checker) de utterance – utilizado em conjunto com o parâmetro de cadeia de consulta do bing ortográfica-verifique-subscrição-chave|
|ortográfica-verifique-subscrição-chave do Bing|ID de subscrição||utilizado em conjunto com o parâmetro de cadeia de consulta spellCheck|
|teste|boolean|false|Selecione o ponto final de teste ou de produção|
|registo|boolean|true|adicionar a consulta e os resultados para iniciar sessão|


## <a name="test-your-published-endpoint-in-a-browser"></a>Testar o ponto final publicado num browser
Testar o ponto final publicado selecionando o URL no **Endpoint** coluna. O browser predefinido abre-se com o URL gerado. Defina o parâmetro de URL "& q" para a consulta de teste. Por exemplo, acrescentar `&q=Book me a flight to Boston on May 4` URL e, em seguida, prima Enter. O browser apresenta a resposta JSON do ponto final de HTTP. 

![Resposta JSON de um ponto final de HTTP publicada](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Passos Seguintes

* Consulte [gerir chaves](./Manage-Keys.md) para adicionar as chaves para a sua aplicação LUIS e saber mais sobre a forma como as chaves de mapeiam para regiões.
* Consulte [formação e testar a aplicação](interactive-test.md) para obter instruções sobre como testar as aplicações publicadas na consola de teste.
