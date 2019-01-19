---
title: Gestão de recursos do Azure
titleSuffix: Language Understanding - Azure Cognitive Services
description: Neste artigo, vai criar uma chave de ponto final com tráfego limitado para a sua conta do LUIS fornecer o tráfego ilimitado para o ponto final de seguir um plano de pagamento.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/18/2019
ms.author: diberry
ms.openlocfilehash: e69d03e2c45ee34723bd6aace3a2a26cead63e96
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411616"
---
# <a name="manage-azure-resource-keys-for-prediction-endpoint-queries"></a>Gerir chaves de recursos do Azure para consultas de ponto final de previsão

[!INCLUDE [Azure resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Para teste e apenas o protótipo, utilize o escalão gratuito do (F0). Para os sistemas de produção, utilize um [pago](https://aka.ms/luis-price-tier) escalão. 

> [!NOTE]
> Não utilize o [chave de criação](luis-concept-keys.md#authoring-key) para consultas de ponto final na produção.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>Criar uma chave de ponto final de LUIS

1. Inicie sessão no  **[do Microsoft Azure](https://ms.portal.azure.com/)**. 
2. Clique no verde **+** iniciar sessão no painel esquerdo superior e procure "LUIS" no marketplace, em seguida, clique em **compreensão de idiomas** e siga o **criar experiência**  para criar uma conta de subscrição do LUIS. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Configure a subscrição com as definições, incluindo o nome da conta, preços escalões, etc. 

    ![Escolha de API do Azure](./media/luis-azure-subscription/azure-api-choice.png) 

4. Depois de criar o serviço de LUIS, pode ver as teclas de acesso geradas em **gestão de recursos -> chaves**.  

    ![Chaves do Azure](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > Inicie sessão na sua região [LUIS](luis-reference-regions.md) Web site e [atribuir o LUIS nova chave de ponto final](luis-how-to-manage-keys.md#assign-endpoint-key). É necessário o nome da subscrição do LUIS do passo 3.

## <a name="change-luis-pricing-tier"></a>Escalão de preço de LUIS de alteração

1.  Na [Azure](https://portal.azure.com), localize a sua subscrição do LUIS. Clique na subscrição do LUIS.
    ![Localize a sua subscrição do LUIS](./media/luis-usage-tiers/find.png)
2.  Clique em **escalão de preço** para ver os escalões de preços disponíveis. 
    ![Vista de escalões de preço](./media/luis-usage-tiers/subscription.png)
3.  Clique com o escalão de preço e clique em **selecione** para guardar a alteração. 
    ![Alterar o escalão de pagamento do LUIS](./media/luis-usage-tiers/plans.png)
4.  Quando a alteração de preços estiver concluída, uma janela de pop-up verifica se o novo escalão de preços. 
    ![Verifique se o seu escalão de pagamento do LUIS](./media/luis-usage-tiers/updated.png)
5. Não se esqueça de [atribuir esta chave de ponto final](luis-how-to-manage-keys.md#assign-endpoint-key) sobre o **publicar** página e utilizá-lo em todas as consultas de ponto final. 

## <a name="exceed-pricing-tier-usage"></a>Ter mais de utilização do escalão de preço
Cada escalão permite pedidos de ponto final à sua conta do LUIS a uma taxa específica. Se a taxa de pedidos é maior do que a taxa de permitidos da sua conta com tráfego limitado por minuto ou por mês, pedidos de recebem um erro HTTP de "429: Demasiados pedidos."

Cada escalão permite acumulativos pedidos por mês. Se o total de pedidos é maiores que a taxa de permitidas, os pedidos de recebem um erro HTTP de "403: proibido".  

## <a name="viewing-summary-usage"></a>Ver utilização de resumo
Pode ver informações de utilização do LUIS no Azure. O **descrição geral** página mostra informações de resumo recentes, incluindo chamadas e erros. Se fizer um LUIS pedido de ponto final, em seguida, imediatamente Assista a **página de descrição geral**, aguarde cinco minutos para a utilização a aparecer.

![Ver utilização de resumo](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Personalizando os gráficos de utilização
Metrics fornece uma visão mais detalhada sobre os dados.

![Métricas predefinidas](./media/luis-usage-tiers/metrics-default.png)

Pode configurar seus gráficos de métricas para o período de tempo e o tipo de métrica. 

![Métricas personalizadas](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Alerta do limiar de total de transações
Se quiser saber quando atingiu um certo limite da transação, por exemplo 10 000 transações, pode criar um alerta. 

![Alertas de predefinição](./media/luis-usage-tiers/alert-default.png)

Adicionar um alerta de métrica para o **total de chamadas** métrica durante um determinado período de tempo. Adicione endereços de e-mail de todas as pessoas que devem receber o alerta. Adicione webhooks para todos os sistemas que devem receber o alerta. Também pode executar uma aplicação lógica quando o alerta é acionado. 

## <a name="next-steps"></a>Passos Seguintes

Aprenda a usar [versões](luis-how-to-manage-versions.md) para gerir as alterações à sua aplicação LUIS.