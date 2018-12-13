---
title: Descrição geral do Estado de funcionamento do serviço | Documentos da Microsoft
description: Obter informações personalizadas sobre como as aplicações do Azure são afetadas por problemas de serviço do Azure, atuais e futuras e manutenção.
services: Resource health
documentationcenter: ''
author: rboucher
manager: ''
editor: ''
ms.assetid: ''
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/27/2018
ms.author: robb
ms.openlocfilehash: aa7a925255b0c30b84c3b32bb53ba3f31c11fa6b
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53162295"
---
# <a name="service-health"></a>Service Health
Serviço de estado de funcionamento fornece um dashboard personalizável que controla o estado de funcionamento dos seus serviços do Azure nas regiões onde usá-los. Neste dashboard, pode controlar o Active Directory eventos como problemas de serviço em curso, futura manutenção planeada ou aconselhamentos sobre o estado de funcionamento relevantes. Quando eventos tornam-se inativos, eles serão colocados no seu histórico de estado de funcionamento até 90 dias. Por fim, pode utilizar o dashboard de estado de funcionamento do serviço para criar e gerir alertas de estado de funcionamento do serviço que proativamente notificá-lo quando os problemas de serviço estão a afetar.

## <a name="service-health-events"></a>Eventos de estado de funcionamento do serviço
Estado de funcionamento do serviço controla os três tipos de eventos de estado de funcionamento que possam afetar seus recursos:
1. **Problemas de serviço** -problemas nos serviços do Azure que afetá-lo neste momento. 
2. **A manutenção planeada** -manutenção futura, que pode afetar a disponibilidade dos seus serviços no futuro.  
3. **Aconselhamentos sobre o estado de funcionamento** -as alterações nos serviços do Azure que necessitam da sua atenção. Os exemplos incluem quando as funcionalidades do Azure foram preteridas ou se exceder a quota de utilização.

## <a name="get-started-with-service-health"></a>Introdução ao serviço de estado de funcionamento
Para iniciar o seu dashboard de estado de funcionamento do serviço, selecione o mosaico de estado de funcionamento do serviço no dashboard do portal. Se anteriormente tiver removido o mosaico ou estiver a utilizar o dashboard personalizado, procure por serviço de estado de funcionamento do serviço em "Mais serviços" (à esquerda no seu dashboard em baixo).

![Introdução ao serviço de estado de funcionamento](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Ver problemas recentes de que afetar seus serviços
O **problemas do serviço** vista mostra todos os problemas em curso nos serviços do Azure que estejam a afetar seus recursos. Pode compreender quando o problema começou e que serviços e regiões são afetados. Também é possível ler a atualização mais recente para compreender o que está fazendo Azure para resolver o problema. 

![Gerir o problema de serviço](./media/service-health-overview/azure-service-health-overview-2.png)

Escolha o **impacto potencial** separador para ver a lista específica de recursos é proprietário, que poderão ser afetados pelo problema. Pode transferir uma lista CSV destes recursos para partilhar com a sua equipa.

![Gerir o problema de serviço – impacto](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Obtenha ligações e explicações para download 
Pode obter uma ligação para o problema utilizar no seu sistema de gestão de problemas. Pode baixar PDF e, às vezes, ficheiros CSV para partilhar com pessoas que não têm acesso ao portal do Azure.   

![Gerir problemas no serviço - Gestão de problemas](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Obter suporte da Microsoft
Se o seu recurso resta num Estado incorreto, mesmo depois do problema foi resolvido, contacte o suporte.  Use os links de suporte no lado direito da página.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Afixar um mapa de estado de funcionamento personalizado ao dashboard
Filtre o estado de funcionamento do serviço para mostrar as suas subscrições empresariais críticas, regiões e tipos de recursos. Guarde o filtro e afixar mapa-mundo um Estado de funcionamento personalizado ao dashboard do portal. 

![Mapa de estado de funcionamento personalizado de filtro](./media/service-health-overview/azure-service-health-overview-6a.png)

![Afixar um mapa de estado de funcionamento personalizado](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Configurar alertas de estado de funcionamento do serviço
Service Health integra-se com o Azure Monitor para alertá-lo através de e-mails, mensagens de texto e notificações de webhook quando os seus recursos críticos para o negócio são afetados. Configure um alerta de registo de atividade para o evento de estado de funcionamento do serviço apropriado. Encaminhe esse alerta para as pessoas adequadas na sua organização utilizando grupos de ação. Para obter mais informações, consulte [configurar alertas de estado de funcionamento do serviço](../azure-monitor/platform/alerts-activity-log-service-notifications.md)

# <a name="next-steps"></a>Próximos Passos
Configure alertas para que seja notificado de problemas de estado de funcionamento. Para obter mais informações, consulte [configurar alertas de estado de funcionamento do serviço](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 
