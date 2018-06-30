---
title: Utilizar Cortana prebuilt a aplicação de LUIS | Microsoft Docs
description: Utilize o Assistente de pessoal de Cortana, uma aplicação prebuilt do idioma compreender inteligente serviços (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: c7249cb8d8cff29f419412025c69e3b2b76b49d1
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110662"
---
# <a name="cortana-prebuilt-app"></a>Aplicação Prebuilt Cortana

> [!IMPORTANT]
> Recomendamos que utilize o [domínios prebuilt](./luis-how-to-use-prebuilt-domains.md), em vez da aplicação prebuilt Cortana. Por exemplo, em vez de **builtin.intent.calendar.create_calendar_entry**, utilize **Calendar.Add** do **calendário** prebuilt domínio.
> Os domínios prebuilt fornecem as seguintes vantagens: 
> * Fornecem pacotes de pendentes prebuilt e pretrained e entidades que foram concebidas para funcionar bem com entre si. Pode integrar um domínio prebuilt diretamente na sua aplicação. Por exemplo, se estiver a criar um controlador de adequação a, pode adicionar o **adequação** domínio e ter um conjunto completo de entidades para controlar as atividades de adequação, incluindo pendentes para controlar o peso e planeamento meal, tempo restante e pendentes função de segurança ou distância e notas de atividade de adequação a guardar.
> * Os pendentes domínio prebuilt são personalizáveis. Por exemplo, se pretender fornecer revisões de hotéis, pode dar formação e personalizar o **Places.GetReviews** intenção do **locais** domínio para reconhecer pedidos para átrios revisões.
> * Os domínios prebuilt são extensíveis. Por exemplo, se pretender utilizar o **locais** prebuilt domínio num bot que procura restaurants e o objetivo é necessário para obter o tipo de cuisine, pode compilar e dar formação sobre um **Places.GetCuisine** intenção.

Para além de permitir-lhe criar as suas próprias aplicações, LUIS também fornece pendentes e entidades do Assistente de pessoal da Microsoft Cortana como uma aplicação prebuilt. Não é possível alterar o comportamento desta aplicação LUIS publicamente disponíveis. As entidades nesta aplicação e pendentes não não possível editá-lo ou integradas outras aplicações LUIS. Se pretender que a aplicação de cliente para ter acesso a esta aplicação prebuilt e a sua própria aplicação LUIS, em seguida, a aplicação cliente tem de referenciar ambas as aplicações LUIS.

A aplicação de Assistente de pessoal prebuilt está disponível nestes culturas (regiões): inglês, francês, italiano, espanhol e chinês.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Obter o ponto final para a aplicação prebuilt Cortana

Pode aceder a aplicação de prebuilt Cortana utilizando os seguintes pontos finais: 

| Idioma | Ponto Final|
|--------| ------------------|
| Português| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Chinês| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Francês| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    Espanhol| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    Italiano| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


> [!NOTE]
> O ponto final de URLs também estão disponíveis a partir de [aplicações - obter pessoal assistente aplicações](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32) API.

## <a name="try-out-the-personal-assistant-app"></a>Experimentar a aplicação de assistente pessoal
Para chamar o ponto final, pode acrescentar o ponto final argumento e a consulta de cadeia de chave para o ponto final. 

Por exemplo, se o utterance pretender interpretar é "Criar uma sessão de reunião de equipa", em seguida, pode acrescentar esse utterance para o URL de ponto final. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key={YOUR-SUBSCRIPTION-KEY}&q=create an appointment for team meeting
```

Pode colar o URL para um web browser e substituir a chave de ponto final para o `{YOUR-SUBSCRIPTION-KEY}` campo.

No browser, pode ver que a aplicação prebuilt Cortana identifica `builtin.intent.calendar.create_calendar_entry` como a intenção, e `builtin.calendar.title` como tipo de entidade e para o utterance `create an appointment for team meeting`.

![Utilizar a aplicação prebuilt Cortana](./media/luis-how-to-prebuilt-cortana/luis-prebuilt-cortana-browser.png)

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Referência da aplicação prebuilt Cortana](./luis-reference-cortana-prebuilt.md)

