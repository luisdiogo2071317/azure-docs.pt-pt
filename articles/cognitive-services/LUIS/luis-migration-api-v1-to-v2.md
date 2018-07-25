---
title: Migrar de API guia da v1 para v2 | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como a migração para a API mais recente definido.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: diberry
ms.openlocfilehash: cb68ad57302f151975c1ca6616a24db9804a6a68
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223060"
---
# <a name="api-v2-migration-guide"></a>Guia de migração do API v2
A versão 1 [ponto final](https://aka.ms/v1-endpoint-api-docs) e [criação](https://aka.ms/v1-authoring-api-docs) APIs vão ser preteridas. Utilize este guia para compreender como migrar para a versão 2 [ponto final](https://aka.ms/luis-endpoint-apis) e [criação](https://aka.ms/luis-authoring-apis) APIs. 

## <a name="new-azure-regions"></a>Novas regiões do Azure
LUIS tem novos [regiões](https://aka.ms/LUIS-regions) fornecido para as APIs do LUIS. LUIS fornece um Web site diferente para grupos de região. A aplicação tem de ser criada na mesma região que pretende consultar. Aplicações não são migrados automaticamente regiões. Exportar a aplicação a partir de uma região, em seguida, importar para o outro para que esteja disponível numa nova região.

## <a name="authoring-route-changes"></a>Alterações de rota de criação
A rota de API de criação em vez de utilizar o **programa** rota para utilizar o **api** rota.


| versão | rota |
|--|--|
|1|/Luis/v1.0/**programa**/apps|
|2|/Luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Alterações de rota do ponto final
O ponto final de API tem novos parâmetros de cadeia de consulta, bem como uma resposta diferente. Se o sinalizador verboso for VERDADEIRO, todas as intenções, independentemente de pontuação, são retornadas numa matriz chamada intenções, além do topScoringIntent.

| versão | OBTER a rota |
|--|--|
|1|/Luis/V1/Application? ID = {appId} & q = {p}|
|2|/ luis/v2.0/apps/{appId}?q={q} [& timezoneOffset] [& verboso] [& verificação ortográfica] [& teste] [& bing-ortográfica-check-subscription-key] [& registo]|


resposta de êxito de ponto final V1:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

resposta de êxito de ponto final v2:
```JSON
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>Gestão de chaves já não estão em API
A chave de ponto final de subscrição APIs são preteridas, retornando 410 já não existe.

| versão | rota |
|--|--|
|1|/Luis/v1.0/Prog/subscriptions|
|1|/ luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [chaves de ponto final](luis-how-to-azure-subscription.md) são gerados no portal do Azure. Atribuir a chave para uma aplicação do LUIS sobre o **[Publish](luis-how-to-manage-keys.md)** página. Não é necessário saber o valor da chave real. LUIS utiliza o nome da subscrição para fazer a atribuição. 

## <a name="new-versioning-route"></a>Nova rota de controle de versão
O modelo v2 agora está contido num [versão](luis-how-to-manage-versions.md). Um nome de versão tenha 10 carateres na rota. A versão predefinida é "0.1".

| versão | rota |
|--|--|
|1|/Luis/v1.0/**programa**/Apps/&lt {appId} / entidades|
|2|/Luis/**api**/v2.0/apps/{appId}/**versões**/ {versionId} / entidades|

## <a name="metadata-renamed"></a>Mudar o nome de metadados
Várias APIs que retornam metadados de LUIS têm nomes novos.

| nome da rota v1 | nome da rota v2 |
|--|--|
|PersonalAssistantApps |assistentes|
|applicationcultures|culturas|
|applicationdomains|domínios|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Exemplo" o nome mudado para "sugestão"
LUIS sugere expressões a partir de existente [expressões de ponto final](luis-how-to-review-endoint-utt.md) que pode melhorar o modelo. Na versão anterior, isso foi chamado **exemplo**. Na nova versão, o nome é alterado de exemplo para **sugerir**. Isso é chamado **[rever expressões de ponto final](luis-how-to-review-endoint-utt.md)** no Web site do LUIS.

| versão | rota |
|--|--|
|1|/Luis/v1.0/**programa**/Apps/&lt {appId} /entities/ {entityId} /**exemplo**|
|1|/Luis/v1.0/**programa**/Apps/&lt {appId} /intents/ {intentId} /**exemplo**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versões**/ {versionId} /entities/ {entityId} /**Sugerir**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versões**/ {versionId} /intents/ {intentId} /**Sugerir**|


## <a name="create-app-from-prebuilt-domains"></a>Criar aplicação a partir de domínios pré-concebidos
[Domínios pré-concebidos](luis-how-to-use-prebuilt-domains.md) fornecem um modelo de domínio predefinida. Domínios pré-concebidos permitem-lhe desenvolver rapidamente a sua aplicação LUIS para domínios comuns. Esta API permite-lhe criar uma nova aplicação com base num domínio pré-criado. A resposta é o novo appID.

|rota de v2|Verbo|
|--|--|
|/Luis/API/v2.0/Apps/customprebuiltdomains  |GET, post|
|/ luis/api/v2.0/apps/customprebuiltdomains/{culture}  |Get|

## <a name="importing-1x-app-into-2x"></a>Importar aplicação 1.x para 2.x
O 1.x exportado JSON da aplicação tem algumas áreas em que precisa alterar antes de importar para o [LUIS] [ LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Entidades pré-concebidas 
O [entidades pré-concebidas](luis-prebuilt-entities.md) foram alterados. Certifique-se de que está a utilizar o V2 entidades pré-concebidas. Isto inclui a utilização [datetimeV2](luis-prebuilt-entities.md#use-a-prebuilt-datetimev2-entity), em vez de datetime. 

### <a name="actions"></a>Ações
A propriedade de ações já não é válida. Ele deve estar vazio 

### <a name="labeled-utterances"></a>Expressões com etiquetados
V1 permitidas expressões com etiquetadas incluem espaços no início ou no final da palavra ou frase. Remover os espaços. 

## <a name="common-reasons-for-http-response-status-codes"></a>Motivos comuns para códigos de estado de resposta HTTP
Ver [códigos de resposta da API de LUIS](luis-reference-response-codes.md).

## <a name="next-steps"></a>Passos Seguintes

Utilize a documentação da API v2 para atualizar o REST existente chama para LIUS [ponto final](https://aka.ms/luis-endpoint-apis) e [criação](https://aka.ms/luis-authoring-apis) APIs. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions