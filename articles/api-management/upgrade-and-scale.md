---
title: Atualizar e dimensionar uma instância de API Management do Azure | Microsoft Docs
description: Este tópico descreve como atualizar e dimensionar uma instância de API Management do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.openlocfilehash: ca32c72b1582b2a09f9f1754ad778cf1b682a1c2
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293317"
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Atualizar e dimensionar uma instância de API Management  

Os clientes podem dimensionar uma instância de gestão de API (APIM) ao adicionar e remover unidades. A **unidade** é composto de recursos do Azure dedicados e tem um determinado carga-efeito capacidade expressada como um número da API de chamadas por mês. Este número não representa um limite de chamada, mas sim um valor de débito máximo para permitir o planeamento de capacidade aproximada. Real débito e latência amplamente variam, dependendo de fatores, tais como o número e a taxa de ligações em simultâneo, o tipo e número de políticas configuradas, tamanhos de pedido e resposta e latência de back-end.

Capacidade e o preço de cada unidade depende o **camada** no qual existe a unidade. Pode escolher entre quatro camadas: **programador**, **básico**, **padrão**, **Premium**. Se precisar de aumentar a capacidade para um serviço dentro de uma camada, deve adicionar uma unidade. Se a camada que está atualmente selecionada na sua instância APIM não permitir a adição de mais unidades, terá de atualizar para uma camada de nível mais elevada.

O preço de cada unidade e as funcionalidades disponíveis (por exemplo, a implementação de multirregião) depende a camada que escolheu para a instância APIM. O [detalhes de preços](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) artigo, explica o preço por unidade e funcionalidades que pode obter em cada camada. 

>[!NOTE]
>O [detalhes de preços](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) artigo mostra o número aproximado de capacidade de unidade em cada camada. Para obter mais exatos números, tem de observar um cenário realista para as suas APIs. Consulte o [capacidade de uma instância de API Management do Azure](api-management-capacity.md) artigo.

## <a name="prerequisites"></a>Pré-requisitos

Para seguir os passos do artigo, tem de:

+ Ter uma subscrição do Azure Active Directory.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Ter uma instância APIM. Para obter mais informações, consulte [criar uma instância de API Management do Azure](get-started-create-service-instance.md).

+ Compreender o conceito de [capacidade de uma instância de API Management do Azure] (capacity.md de gestão de api).

## <a name="upgrade-and-scale"></a>Atualizar e dimensionar  

Pode escolher entre quatro camadas: **programador**, **básico**, **padrão** e **Premium**. O **programador** camada deve ser utilizada para avaliar o serviço; não deve ser utilizada para produção. O **programador** camada não tem o SLA e não é possível aumentar este escalão (unidades de adicionar/remover). 

**Básico**, **padrão** e **Premium** são camadas de produção que tem o SLA e podem ser ampliadas. O **básico** camada é a camada cheapest que tem o SLA e pode ser expandido até 2 unidades, **padrão** camada pode ser escalada para até quatro unidades. Pode adicionar qualquer número de unidades para o **Premium** camada.

O **Premium** camada permite-lhe distribuir uma única instância de gestão de API em qualquer número de regiões do Azure pretendidos. Quando cria um serviço de API Management inicialmente, a instância contém apenas uma unidade e reside numa única região do Azure. A região inicial é designada como o **primário** região. Regiões adicionais podem ser adicionadas facilmente. Ao adicionar uma região, especifique o número de unidades que pretende alocar. Por exemplo, pode ter uma unidade no **primário** região e unidades de cinco por alguma outra região. Pode personalizar o número de unidades para o tráfego que tiver em cada região. Para obter mais informações, consulte [como implementar uma instância de serviço de API Management do Azure em várias regiões do Azure](api-management-howto-deploy-multi-region.md).

Pode atualizar e mudar de e para qualquer camada. Tenha em atenção que a atualização ou desatualização pode remover algumas funcionalidades - por exemplo, as VNETs ou implementação de multirregião, quando desatualização para básico ou padrão do escalão Premium.

>[!NOTE]
>O processo de atualização ou escala pode demorar entre 15 45 minutos para aplicar. Obter a notificação quando terminar.

## <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Utilizar o portal do Azure para atualizar e dimensionar

![Escala APIM no portal do Azure](./media/upgrade-and-scale/portal-scale.png)

1. Navegue até à sua instância APIM no [portal do Azure](https://portal.azure.com/).
2. Selecione **escala e preços** no menu.
3. Escolha a camada pretendida.
4. Especifique o número de **unidades** que pretende adicionar. Pode utilizar o controlo de deslize ou escreva o número de unidades.  
    Se escolher o **Premium** camada, primeiro tem de selecionar uma região.
5. Prima **guardar**

## <a name="next-steps"></a>Passos Seguintes

[Como implementar uma instância de serviço de API Management do Azure em várias regiões do Azure](api-management-howto-deploy-multi-region.md)