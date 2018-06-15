---
title: Migrar de API guia v1 para v2 | Microsoft Docs
titleSuffix: Azure
description: Saiba como a migração para a API mais recente definido.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: 07c26753ed7198a2d98f4b7d5afb4cc7845fbcbe
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355790"
---
# <a name="api-v2-migration-guide"></a>Guia de migração de v2 API
A versão 1 [endpoint](https://aka.ms/v1-endpoint-api-docs) e [criação](https://aka.ms/v1-authoring-api-docs) APIs vão ser preteridas. Utilize este guia para compreender como migrar para a versão 2 [endpoint](https://aka.ms/luis-endpoint-apis) e [criação](https://aka.ms/luis-authoring-apis) APIs. 

## <a name="new-azure-regions"></a>Novo regiões do Azure
LUIS tem novas [regiões](https://aka.ms/LUIS-regions) fornecido para as APIs de LUIS. LUIS fornece um Web site diferentes para grupos de região. A aplicação tem de ser criada na mesma região que pretende consultar. Aplicações não são migrados automaticamente regiões. Exportar a aplicação a partir de uma região, em seguida, importar a outro para que seja disponível numa região novo.

## <a name="authoring-route-changes"></a>Alterações de rota de criação
A rota de API de criação mudou de utilizar o **prog** rota para utilizar o **api** rota.


| versão | rota |
|--|--|
|1|/Luis/v1.0/**prog**/apps|
|2|/Luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Alterações de rota de ponto final
O ponto final da API tem novos parâmetros de consulta, bem como uma resposta diferentes. Se o sinalizador verboso for VERDADEIRO, todos os pendentes, independentemente da pontuação, são devolvidas numa matriz com o nome pendentes, além de topScoringIntent.

| versão | OBTER a rota |
|--|--|
|1|/Luis/V1/Application? ID = {appId} & q = {q}|
|2|/ luis/v2.0/apps/{appId}?q={q} [& timezoneOffset] [& verboso] [& spellCheck] [& transição] [& bing ortográfica-verifique-subscrição-chave] [& registo]|


resposta de êxito de ponto final de V1:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

resposta de êxito de ponto final de v2:
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

## <a name="key-management-no-longer-in-api"></a>Gestão de chaves já não está na API
A chave de subscrição que foram preteridas APIs, 410 GONE a devolver.

| versão | rota |
|--|--|
|1|/Luis/v1.0/Prog/subscriptions|
|1|/ luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [chaves de subscrição](luis-how-to-azure-subscription.md) são gerados no portal do Azure. Atribuir a chave para uma aplicação LUIS no **[publicar](manage-keys.md)** página. Não é necessário saber o valor da chave real. LUIS utiliza o nome da subscrição para efetuar a atribuição. 

## <a name="new-versioning-route"></a>Nova rota do controlo de versões
O modelo v2 agora faz uma [versão](luis-how-to-manage-versions.md). Um nome de versão é de 10 carateres da rota. A versão predefinida é "0.1".

| versão | rota |
|--|--|
|1|/Luis/v1.0/**prog**/apps/ {appId} / entidades|
|2|/Luis/**api**/v2.0/apps/{appId}/**versões**/ {versionId} / entidades|

## <a name="metadata-renamed"></a>Mudar o nome de metadados
Várias APIs que devolvem metadados de LUIS têm nomes de novo.

| nome da rota v1 | nome da rota v2 |
|--|--|
|PersonalAssistantApps |assistentes|
|applicationcultures|culturas|
|applicationdomains|domínios|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Sample" mudar o nome "sugerir"
LUIS sugere utterances do existente [utterances de ponto final](label-suggested-utterances.md) que pode melhorar o modelo. Na versão anterior, este foi atribuído o nome **exemplo**. Na nova versão, o nome é alterado de exemplo para **sugerir**. Esta opção é denominada **[rever utterances de ponto final](https://docs.microsoft.com/azure/cognitive-services/LUIS/label-suggested-utterances)** no Web site LUIS.

| versão | rota |
|--|--|
|1|/Luis/v1.0/**prog**/entities/ /apps/ {appId} {entityId} /**exemplo**|
|1|/Luis/v1.0/**prog**/intents/ /apps/ {appId} {intentId} /**exemplo**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versões**/ {versionId} /entities/ {entityId} /**Sugerir**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versões**/ {versionId} /intents/ {intentId} /**Sugerir**|


## <a name="create-app-from-prebuilt-domains"></a>Criar aplicação prebuilt domínios
[Domínios prebuilt](luis-how-to-use-prebuilt-domains.md) fornecer um modelo de domínio predefinida. Domínios prebuilt permitem-lhe desenvolver rapidamente a sua aplicação de LUIS para domínios comuns. Esta API permite-lhe criar uma nova aplicação com base no domínio prebuilt. A resposta é o appID de novo.

|rota de v2|Verbo|
|--|--|
|/Luis/API/v2.0/Apps/customprebuiltdomains  |GET, post|
|/ luis/api/v2.0/apps/customprebuiltdomains/{culture}  |Introdução|

## <a name="importing-1x-app-into-2x"></a>Importar aplicações de 1. x para 2. x
O 1. x exportado JSON da aplicação tem algumas áreas que precisa de alterar antes de importar para [LUIS] [ LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Entidades prebuilt 
O [entidades prebuilt](Pre-builtEntities.md) foram alterados. Certifique-se de que está a utilizar o V2 prebuilt entidades. Isto inclui a utilização [datetimeV2](pre-builtentities.md?#use-a-prebuilt-datetimev2-entity), em vez de datetime. 

### <a name="actions"></a>Ações
A propriedade de ações já não é válida. Deve estar vazio 

### <a name="labeled-utterances"></a>Utterances identificados
V1 permitido utterances identificados incluir os espaços no início ou fim da palavra ou frase de acesso. Remover os espaços. 

## <a name="common-reasons-for-http-response-status-codes"></a>Razões comuns para códigos de estado de resposta HTTP
Consulte [códigos de resposta da API de LUIS](luis-reference-response-codes.md).

## <a name="next-steps"></a>Passos Seguintes

Utilize a documentação da API v2 para atualizar o resto existente chamadas para LIUS [endpoint](https://aka.ms/luis-endpoint-apis) e [criação](https://aka.ms/luis-authoring-apis) APIs. 

[LUIS]: luis-reference-regions.md