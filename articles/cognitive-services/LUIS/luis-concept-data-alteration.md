---
title: Compreender os conceitos de alteração de dados do LUIS - Azure | Microsoft Docs
description: Saiba como os dados podem ser alterados antes predições na compreensão de idiomas (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: v-geberr
ms.openlocfilehash: ab9b9fe5c34c334fd65971f9040c8e43467aa242
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112155"
---
# <a name="data-alterations"></a>Expansões de dados
LUIS fornece formas para manipular utterance antes ou durante a predição. 

## <a name="correct-spelling-errors-in-utterance"></a>Erros de ortografia correto no utterance
Utiliza LUIS [API V7 de verificação do ortográfica do Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/) para corrigir os erros de ortografia no utterance. LUIS tem a chave associada esse serviço. Criar a chave e depois adicione a chave como um parâmetro da cadeia de consulta a [endpoint](https://aka.ms/luis-endpoint-apis). 

Também pode corrigir erros de ortografia no **teste** painel por [introduzir uma chave de](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). A chave é mantida como uma variável de sessão no browser para o painel de teste. Adicione a chave para o painel de teste em cada sessão de browser que pretende que a ortografia corrigida. 

Utilização da chave no painel de teste e, a contagem de ponto final para o [utilização da chave](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) quota. LUIS implementa Bing de verificação ortográfica limites para o comprimento do texto. 

O ponto final necessita de dois parâmetros para correções ortografia funcione:

|Param|Valor|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|[API V7 de verificação do ortográfica do Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/) chave de ponto final|

Quando [API V7 de verificação do ortográfica do Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/) Deteta um erro, o utterance original e o utterance corrigida são devolvidos juntamente com predições do ponto final.

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
 
## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Alterar o fuso horário da entidade prebuilt datetimeV2
Quando uma aplicação LUIS utiliza a entidade de prebuilt datetimeV2, um valor datetime pode ser devolvido na resposta predição. O fuso horário do pedido é utilizado para determinar a datetime correta para devolver. Se o pedido for proveniente de um bot ou outra aplicação centralizada antes de obter ao LUIS, corrija o fuso horário que Luis utiliza. 

### <a name="endpoint-querystring-parameter"></a>Parâmetro de cadeia de consulta de ponto final
O fuso horário seja corrigido através da adição do fuso de horário do utilizador para o [endpoint](https://aka.ms/luis-endpoint-apis) utilizando o `timezoneOffset` param. O valor de `timezoneOffset` deve ser o número positivo ou negativo, em minutos, para alterar a hora.  

|Param|Valor|
|--|--|
|`timezoneOffset`|número positivo ou negativo, em minutos|

### <a name="daylight-savings-example"></a>Exemplo de poupança de Verão
Se precisar do datetimeV2 prebuilt devolvido para ajustar para a hora de Verão, deve utilizar o `timezoneOffset` querystring parâmetro com um + /-valor em minutos para o [endpoint](https://aka.ms/luis-endpoint-apis) consulta.

Adicione 60 minutos: 

https://{Region}.API.cognitive.microsoft.com/Luis/v2.0/Apps/{appId}?q=Turn lights no? **timezoneOffset = 60**& verboso = {booleano} & spellCheck = {booleano} & transição = {booleano} & bing ortográfica-verifique-subscrição-chave = {cadeia} & Iniciar = {booleano}

Remova 60 minutos: 

https://{Region}.API.cognitive.microsoft.com/Luis/v2.0/Apps/{appId}?q=Turn lights no? **timezoneOffset = 60**& verboso = {booleano} & spellCheck = {booleano} & transição = {booleano} & bing ortográfica-verifique-subscrição-chave = {cadeia} & Iniciar = {booleano}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>Código c# determina o valor correto de timezoneOffset
O seguinte código c# utiliza o [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) dessa classe [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) método para determinar o correto `timezoneOffset` com base na hora do sistema:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ortografia correto prende com este tutorial](luis-tutorial-bing-spellcheck.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions