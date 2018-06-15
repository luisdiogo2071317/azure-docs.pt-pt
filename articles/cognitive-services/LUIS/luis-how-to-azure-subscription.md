---
title: Gerir a subscrição de ponto final do Azure | Microsoft Docs
description: Neste artigo, vai criar uma chave de ponto final com tráfego limitado para a sua conta LUIS fornecer o tráfego ilimitado para o ponto final de seguir um plano de pagamento.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 3526871f126ac975f323fe84b14883b361b684ae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355766"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Gerir chaves de subscrição de ponto final do Azure

Para testar e protótipo apenas, utilize o escalão gratuito do (F0). Para os sistemas de produção, utilize um [paga](https://aka.ms/luis-price-tier) camada. 

> [!NOTE]
> Não utilize o [criação chave](luis-concept-keys.md#authoring-key) para consultas de ponto final na produção.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>Criar a chave de ponto final de LUIS

1. Inicie sessão no  **[do Microsoft Azure](https://ms.portal.azure.com/)** 
2. Clique na verde **+** iniciar sessão no painel da esquerda superior e procure "LUIS" no marketplace, em seguida, clique em **compreensão de idiomas** e siga o **criar experiência**  para criar uma conta de subscrição LUIS. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Configure a subscrição com as definições, incluindo o nome da conta, preços camadas, etc. 

    ![Opção de API do Azure](./media/luis-azure-subscription/azure-api-choice.png) 

4. Depois de criar o serviço de LUIS, pode ver as chaves de acesso geradas no **gestão de recursos -> chaves**.  

    ![Chaves do Azure](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > * Registo para a região [LUIS](luis-reference-regions.md) Web site e [adicionar a nova chave de ponto final de LUIS](Manage-Keys.md#assign-endpoint-key). 
    > * Terá de memorizar o nome do serviço do Azure que criou para selecionar a região [LUIS](luis-reference-regions.md) publicar página.  

## <a name="change-luis-pricing-tier"></a>Escalão de preço de LUIS de alteração

1.  No [Azure](https://portal.azure.com), localizar a sua subscrição LUIS. Clique na subscrição de LUIS.
    ![Localizar a sua subscrição LUIS](./media/luis-usage-tiers/find.png)
2.  Clique em **escalão de preço** para ver os escalões de preços disponíveis. 
    ![Vista de escalões de preço](./media/luis-usage-tiers/subscription.png)
3.  Clique no escalão de preço e clique em **selecione** para guardar a alteração. 
    ![Alterar o escalão de pagamento LUIS](./media/luis-usage-tiers/plans.png)
4.  Quando a alteração de preço estiver concluída, uma janela de pop-up verifica o escalão de preço de novo. 
    ![Verifique o escalão de pagamento LUIS](./media/luis-usage-tiers/updated.png)
5. Não se esqueça de [atribuir esta chave de ponto final](manage-keys.md#assign-endpoint-key) no **publicar** e utilizá-lo em todas as consultas de ponto final da página. 

## <a name="exceed-pricing-tier-usage"></a>Exceder a utilização de escalão de preço
Cada camada permite pedidos de ponto final à sua conta LUIS uma taxa específico. Se a taxa de pedidos é superior ao permitido taxa da sua conta limitada por minuto ou por mês, pedidos de recebem um erro HTTP de "429: demasiados pedidos."

Cada camada permite pedidos accumulative por mês. Se o total de pedidos é superiores à taxa de permitido, pedidos de recebem um erro HTTP de "403: proibido".  

## <a name="viewing-summary-usage"></a>Utilização de resumo de visualização
Pode ver informações de utilização de LUIS no Azure. O **descrição geral** página mostra informações de resumo recentes, incluindo as chamadas e erros. Se efetuar um LUIS pedido de ponto final, em seguida, imediatamente veja o **página Descrição geral**, permitir até cinco minutos de utilização apresentar.

![Utilização de resumo de visualização](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Personalizar gráficos de utilização
Métricas fornece uma vista mais detalhada sobre os dados.

![Nas métricas predefinidas](./media/luis-usage-tiers/metrics-default.png)

Pode configurar os gráficos de métricas para o período de tempo e o tipo de métrico. 

![Métricas personalizadas](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Alerta do limiar de total de transações
Se quiser saber quando atingiu um determinado limiar da transação, por exemplo, 10 000 transações, pode criar um alerta. 

![Alertas de predefinido](./media/luis-usage-tiers/alert-default.png)

Adicionar um alerta de métrico para a **total de chamadas** métrica para um determinado período de tempo. Adicione endereços de e-mail de todas as pessoas que devem receber o alerta. Adicione webhooks para todos os sistemas que devem receber o alerta. Também pode executar uma aplicação lógica, quando o alerta é acionado. 

## <a name="next-steps"></a>Passos Seguintes

Saiba como utilizar [versões](luis-how-to-manage-versions.md) para gerir as alterações à sua aplicação LUIS.