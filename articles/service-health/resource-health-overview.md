---
title: Descrição geral do Estado de funcionamento de recursos do Azure | Microsoft Docs
description: Descrição geral do Estado de funcionamento de recursos do Azure
services: Resource health
documentationcenter: ''
author: shawntabrizi
manager: ''
editor: ''
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: shawn.tabrizi
ms.openlocfilehash: 661faf8560295dd037364f9509d21719362ae727
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-resource-health-overview"></a>Descrição geral do Estado de funcionamento de recursos do Azure
 
Estado de funcionamento de recursos do Azure ajuda-o a diagnosticar e obter suporte, quando um problema de serviço do Azure afeta os seus recursos. Informa-o sobre o estado de funcionamento atual e anterior dos seus recursos. E fornece suporte técnico para ajudar a mitigar os problemas.

Enquanto [Azure estado](https://status.azure.com) informa sobre problemas de serviço que afetam um conjunto amplo de clientes do Azure, o estado de funcionamento do recurso dá-lhe um dashboard personalizado do Estado de funcionamento dos seus recursos. Estado de funcionamento de recursos mostra todas as vezes que os recursos foram disponíveis no passado devido a problemas de serviço do Azure. Em seguida, é simple de compreender se um SLA foi violado. 

## <a name="resource-definition-and-health-assessment"></a>Avaliação de estado de funcionamento e a definição do recurso
Um recurso é uma instância específica de um serviço do Azure: por exemplo, uma máquina virtual, uma aplicação web ou uma base de dados do SQL Server.

Estado de funcionamento do recurso depende sinais emitidos pelos diferentes serviços do Azure para avaliar se um recurso está em bom estado ou não. Se um recurso é mau estado de funcionamento, o estado de funcionamento do recurso analisa informações adicionais para determinar a origem do problema. Também identifica as ações que a Microsoft está a demorar para corrigir o problema ou as ações que pode tomar para resolver a causa do problema. 

Para obter detalhes adicionais sobre como o estado de funcionamento é avaliado, reveja a lista completa dos tipos de recursos e estado de funcionamento verifica [estado de funcionamento de recursos de Azure](resource-health-checks-resource-types.md).

## <a name="health-status"></a>Estado de funcionamento
O estado de funcionamento de um recurso é apresentado como um dos seguintes Estados.

### <a name="available"></a>Disponível
Um Estado de **disponível** significa que o serviço não detetada quaisquer eventos que afetam o estado de funcionamento do recurso. Nos casos em que o recurso recuperou de um período de indisponibilidade não planeado durante as últimas 24 horas, verá o **resolvido recentemente** notificação.

![Estado de "Disponível" para uma máquina virtual com uma notificação "Resolvido recentemente"](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Indisponível
Um Estado de **indisponível** significa que o serviço detetou uma plataforma em curso ou evento de plataforma não afeta o estado de funcionamento do recurso.

#### <a name="platform-events"></a>Eventos de plataforma
Eventos de plataforma são acionados por vários componentes da infraestrutura do Azure. Incluem ações agendadas (por exemplo, a manutenção planeada) e incidentes inesperados (por exemplo, um reinício do anfitrião não planeada).

Estado de funcionamento do recurso fornece detalhes adicionais sobre o evento e o processo de recuperação. Também permite-lhe contactar o suporte, mesmo se não tiver um Microsoft Active Directory suporta o contrato.

![Estado de "Não disponível" para uma máquina virtual devido a um evento de plataforma](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Eventos de plataforma não
Eventos de plataforma não são acionados por ações dos utilizadores. Os exemplos são parar uma máquina virtual ou atingir o número máximo de ligações a uma cache de Redis.

![Estado de "Não disponível" para uma máquina virtual devido a um evento de plataforma não](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Desconhecido
O estado de funcionamento de **desconhecido** indica que o estado de funcionamento do recurso não recebeu informações sobre este recurso durante mais de 10 minutos. Embora este estado não é uma indicação definitiva o estado do recurso, é um ponto de dados importantes no processo de resolução de problemas.

Se o recurso está em execução conforme esperado, o estado do recurso será alterado para **disponível** após alguns minutos.

Se estiver a ocorrer problemas com o recurso a **desconhecido** o estado de funcionamento pode sugerir que um evento na plataforma está a afetar o recurso.

![Estado de "Desconhecido" para uma máquina virtual](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Degradado
O estado de funcionamento de **Degraded** indica que o seu recurso detetou uma perda no desempenho, embora seja continuará disponível para utilização.
Recursos diferentes têm os seus próprios critérios para quando especificar que um recurso está degradado.

![Estado de "Degraded" para uma máquina virtual](./media/resource-health-overview/degraded.png)

## <a name="reporting-an-incorrect-status"></a>Um Estado incorreto de relatórios
Se considerar que o atual estado de funcionamento está incorreto, pode indique selecionando **comunicar o estado de funcionamento incorreto**. Em casos onde um problema do Azure está a afetar a, Aconselhamo-lo para contactar o suporte do Estado de funcionamento do recurso. 

![Caixa para submeter informações sobre um Estado incorreto](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Informação histórica
Pode aceder a 14 dias do histórico de estado de funcionamento no **histórico de estado de funcionamento** secção do Estado de funcionamento de recursos. 

![Lista de eventos de estado de funcionamento de recursos nas últimas duas semanas](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Introdução
Para abrir o estado de funcionamento de recursos para um recurso:
1.  Inicie sessão no Portal do Azure.
2.  Navegue até ao recurso.
3.  No menu de recurso no painel esquerdo, selecione **estado de funcionamento do recurso**.

![Estado de funcionamento de recursos de abertura da vista de recursos](./media/resource-health-overview/from-resource-blade.png)

Pode também aceder ao estado de funcionamento do recurso, selecionando **todos os serviços** e escrever **estado de funcionamento do recurso** na caixa de texto de filtro. No **ajuda + suporte** painel, selecione [estado de funcionamento do recurso](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Estado de funcionamento de recursos de abertura de "Todos os serviços"](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Passos Seguintes

Consulte estes recursos para saber mais sobre o estado de funcionamento de recursos:
-  [Tipos de recursos e o estado de funcionamento verifica-se no estado de funcionamento de recursos de Azure](resource-health-checks-resource-types.md)
-  [Perguntas mais frequentes sobre o estado de funcionamento de recursos de Azure](resource-health-faq.md)




