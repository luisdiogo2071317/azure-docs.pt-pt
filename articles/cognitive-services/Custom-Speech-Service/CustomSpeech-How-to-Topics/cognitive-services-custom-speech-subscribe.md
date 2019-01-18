---
title: Obter chaves de subscrição para o serviço de voz personalizada no Azure | Documentos da Microsoft
description: Saiba como obter chaves de subscrição para chamadas para o serviço de voz personalizada nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 84ef657af2cc3dc4a7168a815b5e51d6f4f33fd7
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338374"
---
# <a name="obtain-subscription-keys"></a>Obter chaves de subscrição

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Para começar a utilizar o serviço de voz personalizada do Azure, primeiro tem de ligar a sua conta de utilizador para uma subscrição do Azure. As assinaturas para os escalões gratuito e pagos estão disponíveis. Para obter informações sobre os escalões de mensagens em fila, consulte a [página de preços](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Obter uma chave de subscrição
1. Pode obter uma chave de assinatura do portal do Azure de uma de duas formas:

    * Vá para o [portal do Azure](https://ms.portal.azure.com), e adicione uma nova API de serviços cognitivos, pesquisando _Cognitive Services_ e, em seguida, selecionando **APIs serviços cognitivos**.

      ![Pesquisa de serviços cognitiva](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * Ou vá diretamente para o [APIs serviços cognitivos](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![APIs Serviços Cognitivos](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
1. Preencha os campos obrigatórios seguintes:

      a. **Nome da conta**. Utilize um nome que funcione para si. Lembre-se este nome para que pode encontrar a sua subscrição de serviços cognitivos na lista de recursos.

      b. **Subscrição**. Selecione uma das suas subscrições do Azure.

      c. **Tipo de API**. Selecione **serviço de voz personalizada (pré-visualização)**.

      d. **Localização**. Se encontra atualmente **E.U.A. oeste**.

      e. **Escalão de preço**. Selecione o escalão que funcione para si. **F0** é o escalão gratuito. As quotas permitidas são explicadas no [página de preços](https://www.microsoft.com/cognitive-services/en-us/pricing).

      ![Criação da conta dos serviços cognitiva](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

1. Deve encontrar a uma vista no seu dashboard ou um serviço com o nome da conta fornecido na sua lista de recursos. Quando selecioná-lo, pode ver uma descrição geral do seu serviço. Na lista à esquerda, em **gestão de recursos**, selecione **chaves**. Cópia **chave 1**.

      Esta chave de subscrição é necessário nos passos seguintes.

      ![CHAVE 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > Não copie o **ID de subscrição** da página de descrição geral. Precisa da chave de subscrição no próximo passo.
      >

      ![ID de subscrição de descrição geral](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

1. Introduza a chave de subscrição, na faixa de opções no canto superior direito, selecione a sua conta de utilizador. No menu pendente, selecione **subscrições**.

      ![Item de menu de subscrições](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    É apresentada uma tabela de subscrições, que está vazio na primeira vez, que é aberto.

    ![Tabela de subscrições](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

1. Selecione **adicionar novo**. Introduza um nome para a subscrição e a chave de subscrição. Ele pode ser uma **chave 1** (chave primária) ou **chave 2** (chave secundária) da sua subscrição.

      ![Nome da chave de subscrição](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Para carregar dados, formar um modelo, ou efetuar uma implementação, precisa de associar as suas atividades de serviço de voz personalizada para uma subscrição do Azure. Pode ser um escalão gratuito ou uma subscrição de escalão pago. Para obter mais informações, veja a [página de preços](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-id"></a>Obter um ID de subscrição
Para obter um ID de subscrição, vá para o portal do Azure. Procure *dos serviços cognitivos* e *serviço de voz personalizada*e siga as instruções.

> [!NOTE]
> A chave de subscrição é necessária mais tarde nesse processo.
>

## <a name="next-steps"></a>Passos Seguintes
* Começar a criar sua [um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md).
* Começar a criar sua [modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md).
