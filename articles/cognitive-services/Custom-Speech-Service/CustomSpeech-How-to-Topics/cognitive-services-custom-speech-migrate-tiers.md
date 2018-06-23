---
title: Migrar o preço de camadas de pontos finais do serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Saiba como migrar as implementações do escalões S0 e S1, S2 do serviço de reconhecimento de voz personalizadas pontos finais numa serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: 6d92459deb3464cd97c215cbf9a8320628b6da80
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352429"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Migração de implementações para o novo modelo de preços
A partir de Julho de 2017, o serviço de reconhecimento de voz personalizadas oferece um [novo modelo de preços](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/). O novo modelo é *mais fácil de compreender*, *mais simples de calcular os custos*, e *mais flexível* em termos de dimensionamento. Para o dimensionamento, a Microsoft tem introduziu o conceito de uma unidade de escala. Cada unidade de escala processa pedidos simultâneos cinco. O dimensionamento para pedidos simultâneos no modelo antigo foi definido em 5 pedidos simultâneos para a camada S0 e foi definida em 12 pedidos simultâneos para o escalão S1. Iremos abriu estes limites para oferecer a maior flexibilidade com os seus requisitos de caso de utilização.

Se executar uma camada S0 ou S1 antiga, recomendamos que migre as implementações existentes para o novo escalão S2. O novo escalão S2 abrange S0 tanto S1 camadas. Pode ver as opções disponíveis na figura seguinte:

![A página "Escolher o escalão de preço"](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

A Microsoft lida com a migração de uma forma por automatizada. Em primeiro lugar, acionar a migração, selecionando o escalão de preço de novo. Em seguida, iremos migrar a implementação automaticamente.

O mapeamento de camadas de antigos para unidades de escala é apresentado na seguinte tabela:

| Escalão | Pedidos em simultâneo (modelo antigo) | Migração | Pedidos simultâneos |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** com a unidade de 1 escala |   5 |
| S1 |  12  |   => **S2** com 3 unidades de escala |  15 |

Para migrar para o novo escalão, efetue o seguinte:

## <a name="step-1-check-your-existing-deployment"></a>Passo 1: Verificar a implementação existente
Vá para o [portal do serviço de reconhecimento de voz personalizadas](http://cris.ai)e verifique as implementações existentes. No nosso exemplo, existem duas implementações. Executa uma implementação de uma camada de S0 e a outra implementação é executado um escalão S1. As implementações são apresentadas as **opções de implementação** coluna da tabela seguinte:

![A página de implementações](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>Passo 2: Selecione o escalão de preço novo no portal do Azure
1. Abra um novo separador do browser e inicie sessão para o [portal do Azure](http://ms.portal.azure.com/). 

2. No **serviços cognitivos** painel, no **subscrições** lista, selecione a sua subscrição de reconhecimento de voz personalizadas. 

3. No painel da sua subscrição, selecione **escalão de preço**.

    ![A hiperligação "Escalão de preço"](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. No **escolher o escalão de preço** página, selecione **S2 Standard**. Este escalão de preço é o escalão de preço novo, simplificado e mais flexível.

5. Selecione o **selecione** botão.

    ![A página "Escolher o escalão de preço"](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>Passo 3: Verificar o estado de migração no portal do serviço de reconhecimento de voz personalizadas
Regresse ao portal do serviço de reconhecimento de voz personalizadas e verifique as suas implementações. (Se a janela do browser ainda estiver aberta, atualizá-lo.) 

O estado da implementação relacionado deve ter mudado para *processamento*. Também pode validar a migração, verificando o **opções de implementação** coluna. Não existe agora pode encontrar informações sobre unidades de escala e registo. As unidades de escala devem refletir o escalão de preço anterior. O registo deve também ser ativado, conforme mostrado na tabela:

![A coluna de opções de implementação](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Se tiver quaisquer problemas durante a migração, contacte-nos.
>

## <a name="next-steps"></a>Passos Seguintes
Para tutoriais mais, consulte:
* [Criar um modelo personalizado acústica](cognitive-services-custom-speech-create-acoustic-model.md)
* [Criar um modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md)
* [Criar um ponto final de reconhecimento de voz para texto personalizado](cognitive-services-custom-speech-create-endpoint.md)
