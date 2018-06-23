---
title: Obter chaves de subscrição para o serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Saiba como obter as chaves de subscrição de chamadas para o serviço de reconhecimento de voz personalizadas nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: fcef86a19a77581ff82b64173e2ac68b26ae708a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351655"
---
# <a name="obtain-subscription-keys"></a>Obter chaves de subscrição
Para começar a utilizar o serviço de reconhecimento de voz personalizadas do Azure, terá primeiro de associar a sua conta de utilizador para uma subscrição do Azure. Subscrições escalões gratuitos e pagos estão disponíveis. Para obter informações sobre as camadas, consulte o [página de preços](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Obter uma chave de subscrição
1. Pode obter uma chave de subscrição a partir do portal do Azure de uma das seguintes formas:

    * Vá para o [portal do Azure](https://ms.portal.azure.com), e adicione uma nova API de serviços cognitivos procurando _serviços cognitivos_ e, em seguida, selecionar **APIs de serviços cognitivos**.

      ![Procurar serviços cognitivos](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * Ou vá diretamente para o [APIs de serviços cognitivos](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![APIs Serviços Cognitivos](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
2. Preencha os campos obrigatórios seguintes:

      a. **Nome da conta**. Utilize um nome que funciona para si. Para que possam localizar a subscrição de serviços cognitivos na lista de recursos, lembre-se este nome.

      b. **Subscrição**. Selecione uma das suas subscrições do Azure.

      c. **Tipo de API**. Selecione **(pré-visualização) do serviço de reconhecimento de voz personalizadas**.

      d. **Localização**. Não se encontra atualmente **EUA oeste**.

      e. **Escalão de preço**. Selecione a camada que funciona para si. **F0** é o escalão gratuito. As quotas que são permitidas são explicadas no [página de preços](https://www.microsoft.com/cognitive-services/en-us/pricing).

      ![Criação de contas de serviços cognitivos](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

3. Encontrará o uma vista no dashboard ou um serviço com o nome de conta fornecida na sua lista de recursos. Quando selecionar, pode ver uma descrição geral do seu serviço. Na lista à esquerda, em **gestão de recursos**, selecione **chaves**. Cópia **chave 1**.

      Esta chave de subscrição é necessário nos passos.

      ![CHAVE 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > Não copie o **ID de subscrição** da página de descrição geral. Tem a chave de subscrição no próximo passo.
      >

      ![ID de subscrição de descrição geral](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

4. Introduza a chave de subscrição, no Friso no canto superior direito, selecione a sua conta de utilizador. No menu pendente, selecione **subscrições**.

      ![Item de menu de subscrições](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    É apresentada uma tabela de subscrições, que está vazia na primeira vez que é aberta.

    ![Tabela de subscrições](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

5. Selecione **adicionar novo**. Introduza um nome para a subscrição e a chave de subscrição. Pode ser um **chave 1** (chave primária) ou **chave 2** (chave secundária) da sua subscrição.

      ![Nome da chave de subscrição](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Para carregar dados, formação de um modelo, ou efetuar uma implementação, tem de associar as atividades do serviço de reconhecimento de voz personalizadas para uma subscrição do Azure. Pode ser um escalão gratuito ou uma subscrição paga camada. Para obter mais informações, veja a [página de preços](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-id"></a>Obter um ID de subscrição
Para obter um ID de subscrição, aceda ao portal do Azure. Procurar *serviços cognitivos* e *serviço de reconhecimento de voz personalizadas*e siga as instruções.

> [!NOTE]
> A chave de subscrição é necessária mais tarde este processo.
>

## <a name="next-steps"></a>Passos Seguintes
* Começar a criar o [personalizado modelo acústica](cognitive-services-custom-speech-create-acoustic-model.md).
* Começar a criar o [modelo idioma personalizado](cognitive-services-custom-speech-create-language-model.md).
