---
title: Aplicação criada previamente da Cortana utilização do LUIS | Documentos da Microsoft
description: Utilize o assistente pessoal do Cortana, um aplicativo pré-criados do Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/03/2018
ms.author: v-geberr
ms.openlocfilehash: 27900068d4420b3e70dacc6d1bdfdf0c52053ceb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37767311"
---
# <a name="cortana-prebuilt-app"></a>Aplicação criada previamente da Cortana

> [!IMPORTANT]
> Recomendamos que utilize o [domínios pré-concebidos](./luis-how-to-use-prebuilt-domains.md), em vez da aplicação pré-criada Cortana. Por exemplo, em vez de **builtin.intent.calendar.create_calendar_entry**, utilize **Calendar.Add** partir os **calendário** domínio pré-criado.
> Os domínios pré-concebidos fornecem essas vantagens: 
> * Eles fornecem pacotes de pré-criados e pré-preparadas com intenções e entidades que foram concebidas para funcionar bem entre si. Pode integrar um domínio pré-criado diretamente na sua aplicação. Por exemplo, se estiver criando um controlador de adequação, pode adicionar os **adequação** domínio e ter um conjunto completo de objetivos e entidades para controlar as atividades de adequação, incluindo objetivos para controlar o peso e o planejamento de refeição, tempo restante função de segurança ou distância e notas de atividade de adequação a guardar.
> * Os objetivos de domínio pré-criado são personalizáveis. Por exemplo, se quiser fornecer análises sobre os hotéis, pode treinar e personalizar os **Places.GetReviews** intenção do **lugares** domínio reconhecer pedidos para análises de hotel.
> * Os domínios criados previamente são extensíveis. Por exemplo, se pretender utilizar o **lugares** domínio pré-criado num bot que procura restaurantes e a necessidade de um objetivo para obter o tipo de cuisine, que pode criar e formar um **Places.GetCuisine** intenção.

Além de permitir que crie seus próprios aplicativos, LUIS também fornece intenções e entidades do Assistente de pessoal Microsoft Cortana como uma aplicação criada previamente. Não é possível alterar o comportamento desta aplicação LUIS publicamente disponíveis. Os objetivos e entidades neste aplicativo não é possível editá-lo ou integradas a outras aplicações de LUIS. Se gostaria de ter a aplicação de cliente para ter acesso a esta aplicação criados previamente e a sua aplicação LUIS, a aplicação cliente tem de fazer referência a ambas as aplicações de LUIS.

A aplicação de assistente pré-criados do pessoal está disponível nessas culturas (localidades): inglês, francês, italiano, espanhol e chinês.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Obter o ponto final para a aplicação pré-criada Cortana

Pode aceder a aplicação pré-criada Cortana utilizando os seguintes pontos finais: 

| Idioma | Ponto Final|
|--------| ------------------|
| Português| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Chinês| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Francês| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    Espanhol| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    Italiano| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


O ponto final de URLs também estão disponíveis a partir da [aplicações - obter aplicações a assistente pessoal](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32) API.

## <a name="try-out-the-personal-assistant-app"></a>Experimente a aplicação de assistente pessoal
Por exemplo, se a expressão que pretende interpretar é "Criar um compromisso para a reunião de equipa", em seguida, pode acrescentar essa expressão para o URL de ponto final. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key=YOUR-SUBSCRIPTION-KEY&q=create%20an%20appointment%20for%20team%20meeting
```

Pode colar o URL de um navegador da web e substitua a sua chave de ponto final para o `YOUR-SUBSCRIPTION-KEY` campo.

No browser pode ver que a aplicação pré-criada Cortana identifica `builtin.intent.calendar.create_calendar_entry` como o objetivo, e `builtin.calendar.title` como o tipo de entidade e para a expressão `create an appointment for team meeting`.

```JSON
{
  "query": "create an appointment for team meeting",
  "topScoringIntent": {
    "intent": "builtin.intent.calendar.create_calendar_entry"
  },
  "entities": [
    {
      "entity": "team meeting",
      "type": "builtin.calendar.title"
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Referência de aplicação pré-criada Cortana](./luis-reference-cortana-prebuilt.md)

