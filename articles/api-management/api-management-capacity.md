---
title: Capacidade de uma instância de API Management do Azure | Microsoft Docs
description: Este artigo explica o que é a métrica de capacidade e sobre como efetuar decisões informadas se pretende dimensionar uma instância de API Management do Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.openlocfilehash: 4983854a14a6efe9214692dc677dedeada73933b
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296107"
---
# <a name="capacity-of-an-azure-api-management-instance"></a>Capacidade de uma instância de API Management do Azure

**Capacidade** é simples, mais importante [métrica de Monitor de Azure](api-management-howto-use-azure-monitor.md#view-metrics-of-your-apis) para tomar decisões informadas se pretende dimensionar uma instância de API Management para acomodar mais carga. A construção é complexa e impõe determinado comportamento.

Este artigo explica o que o **capacidade** é e como esta se comporta. Mostra como aceder **capacidade** métricas no portal do Azure e sugere quando considerar dimensionamento ou atualizar a instância de API Management.

## <a name="prerequisites"></a>Pré-requisitos

Siga os passos do artigo, tem de ter para:

+ Uma subscrição ativa do Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Uma instância APIM. Para obter mais informações, consulte [criar uma instância de API Management do Azure](get-started-create-service-instance.md).

## <a name="what-is-capacity"></a>O que é a capacidade

![Métrica de capacidade](./media/api-management-capacity/capacity-ingredients.png)

**Capacidade** é um indicador de carga numa instância APIM. Este reflete a utilização de recursos (CPU, memória) e comprimentos de fila de rede. Utilização de CPU e memória revela consumo de recursos por:

+ Serviços APIM, tal como gestão ações ou pedido de processamento, que podem incluir pedidos de reencaminhamento ou executar uma política
+ processos do sistema operativo selecionado, incluindo os processos que envolvem o custo de handshakes SSL em novas ligações.

Total **capacidade** é uma média dos seus próprios valores de cada unidade de uma instância de API Management.

## <a name="capacity-metric-behavior"></a>Comportamento de métrico de capacidade

Devido ao respetiva construção, no vida real **capacidade** pode ser afetado por muitas variáveis, por exemplo:

+ padrões de ligação (nova ligação no vs pedido reutilizar a ligação existente)
+ tamanho de um pedido e resposta
+ políticas configuradas em cada API ou o número de clientes a enviar pedidos.

São as operações mais complexas nos pedidos, o mais elevado de **capacidade** será consumo. Por exemplo, políticas de transformação complexas consumam muito mais CPU de um reencaminhamento do pedido simples. As respostas do serviço de back-end lenta aumentará-demasiado.

> [!IMPORTANT]
> **Capacidade** não é uma medida direta do número de pedidos processados.

![Picos de métricos de capacidade](./media/api-management-capacity/capacity-spikes.png)

**Capacidade** pode também aumentam ligados intermitentemente ou ser maior que zero, mesmo se existirem sem pedidos a ser processados. Isto acontece devido a ações específicas do sistema ou da plataforma e não deve ser levado em consideração ao decidir se pretende dimensionar uma instância.
  
## <a name="use-the-azure-portal-to-examine-capacity"></a>Utilizar o Portal do Azure para examinar de capacidade
  
![Métrica de capacidade](./media/api-management-capacity/capacity-metric.png)  

1. Navegue até à sua instância APIM no [portal do Azure](https://portal.azure.com/).
2. Selecione **métricas (pré-visualização)**.
3. Na secção roxa, selecione **capacidade** métrica de métricas disponíveis e deixe a predefinição **médio** agregação.

    > [!TIP]
    > Deve sempre observar um **capacidade** repartição métrica por localização para evitar interpretações erradas.

4. Na secção de verde, selecione **localização** para dividir a métrica por dimensão.
5. Escolha um período de tempo pretendido da barra superior da secção.

    Pode definir um alerta de métrico para informá-lo quando algo de inesperado está a acontecer. Por exemplo, receber notificações quando a instância APIM foi exceededing a capacidade máxima esperados durante mais de 20 minutos.

    >[!TIP]
    > Pode configurar alertas para permitem-lhe saber quando o serviço está em execução baixo capacidade ou utilizar a funcionalidade de dimensionamento automático de Monitor do Azure para adicionar automaticamente uma unidade de API Management do Azure. Operação de dimensionamento pode demorar cerca de 30 minutos, pelo que deve planear as regras em conformidade.  
    > Dimensionamento apenas a localização principal é permitida.

## <a name="use-capacity-for-scaling-decisions"></a>Utilizar a capacidade de dimensionamento de decisões

**Capacidade** é a métrica para tomar decisões se pretende dimensionar uma instância de API Management para acomodar mais carga. Tenha em consideração:

+ Observar uma tendência de longo prazo e média.
+ A ignorar picos repentino que provavelmente não relacionado com qualquer aumento de carga (consulte a secção "Comportamento de métrica de capacidade" explicação).
+ Atualizar ou a instância de dimensionamento quando **capacidade**do valor excede 60% ou 70% por um período de tempo (por exemplo, 30 minutos). Valores diferentes podem funcionar melhor para o seu serviço ou o cenário.

>[!TIP]  
> Se conseguir estimar o tráfego com antecedência, testar a sua instância APIM nas cargas de trabalho que espera. Pode aumentar a carga de pedido no seu inquilino gradualmente e monitorizar que valor da métrica capacidade corresponde ao pico de carga. Siga os passos da secção anterior utilizar o portal do Azure para compreender a capacidade é utilizada em qualquer momento.

## <a name="next-steps"></a>Passos Seguintes

[Como dimensionar ou atualizar uma instância de serviço de API Management do Azure](upgrade-and-scale.md)