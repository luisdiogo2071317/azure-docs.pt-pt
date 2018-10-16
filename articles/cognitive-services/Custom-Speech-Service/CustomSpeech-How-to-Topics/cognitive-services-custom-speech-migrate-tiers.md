---
title: Migrar dos preços das camadas de pontos de extremidade de serviço de voz personalizada do Azure | Documentos da Microsoft
description: Saiba como migrar as implementações de escalões S0 e S1 para pontos finais de S2 do serviço de voz personalizada nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: a9bdb257137db0063d39f028a69e2164eccbdc31
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340465"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Migração de implementações para o novo modelo de preços

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

A partir de Julho de 2017, o serviço de voz personalizada oferece uma [novo modelo de preços](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/). É o novo modelo *mais fáceis de entender*, *mais simples calcular os custos*, e *mais flexível* em termos de dimensionamento. Para dimensionar, a Microsoft introduziu o conceito de uma unidade de escala. Cada unidade de escala pode lidar com cinco pedidos simultâneos. O dimensionamento para pedidos simultâneos no modelo antigo foi definido para 5 pedidos simultâneos para escalão S0 e foi definida em 12 pedidos simultâneos para o escalão S1. Vamos abrir estes limites para oferecer a mais flexibilidade com os seus requisitos de casos de utilização.

Se executar um antigo escalão S0 ou S1, recomendamos que migre as implementações existentes para o novo escalão S2. O novo escalão S2 abrange escalões o S0 e o S1. Pode ver as opções disponíveis na figura a seguir:

![A página "Escolher o escalão de preço"](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

Microsoft lida com a migração de uma forma semiautomática. Em primeiro lugar, disparar a migração ao selecionar o escalão de preço novo. Em seguida, iremos migrar a implementação do automaticamente.

O mapeamento dos escalões antigos para unidades de escala é mostrado na tabela a seguir:

| Escalão | Pedidos em simultâneo (modelo antigo) | Migração | Pedidos em simultâneo |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** com unidade de escala de 1 |   5 |
| S1 |  12  |   => **S2** com 3 unidades de escala |  15 |

Para migrar para o novo escalão, faça o seguinte:

## <a name="step-1-check-your-existing-deployment"></a>Passo 1: Verificar a implementação existente
Vá para o [portal do serviço de voz personalizada](http://cris.ai)e verificar as implementações existentes. No nosso exemplo, existem duas implementações. Uma implementação é executado num escalão S0 e a outra implementação é executado num escalão S1. As implementações são mostradas na **opções de implementação** coluna da tabela seguinte:

![A página implementações](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>Passo 2: Selecione o novo escalão de preço no portal do Azure
1. Abra um novo separador do browser e inicie sessão para o [portal do Azure](http://ms.portal.azure.com/). 

2. Na **dos serviços cognitivos** painel, na **subscrições** lista, selecione a sua subscrição de voz personalizada. 

3. No painel para a sua subscrição, selecione **escalão de preço**.

    ![A ligação "Escalão de preço"](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. Sobre o **escolha o escalão de preço** página, selecione **S2 Standard**. Este escalão de preço é o escalão de preço novo, simplificado e mais flexível.

5. Selecione o **selecione** botão.

    ![A página "Escolher o escalão de preço"](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>Passo 3: Verificar o estado de migração no portal do serviço de voz personalizada
Regresse ao portal do serviço de voz personalizada e verifique as suas implementações. (Se a janela do navegador ainda está aberta, atualizá-la.) 

O estado da implementação relacionado deve ter mudado para *processamento*. Também pode validar a migração, verificando a **opções de implementação** coluna. Existem agora pode encontrar informações sobre unidades de escala e de registo. As unidades de escala devem refletir o escalão de preço anterior. O registo deve também ser ativado, conforme mostrado na tabela:

![A coluna de opções de implementação](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Se tiver quaisquer problemas durante a migração, contacte-nos.
>

## <a name="next-steps"></a>Passos Seguintes
Para mais tutoriais, consulte:
* [Criar um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
* [Criar um modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md)
* [Criar um ponto de final de voz em texto personalizado](cognitive-services-custom-speech-create-endpoint.md)
