---
title: Portal do OMS mover para o Azure | Microsoft Docs
description: O portal do OMS está a ser sunsetted com todas as funcionalidades de mover para o portal do Azure. Este artigo fornece detalhes sobre esta transição.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: bwren
ms.openlocfilehash: 5719dc3719739fb561626e307ee295729752c1fa
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297758"
---
# <a name="oms-portal-moving-to-azure"></a>Portal do OMS mover para o Azure
Obrigado por utilizar o portal do OMS. Iremos são encouraged pelo seu suporte e continuar a investir descontos elevados no nossos serviços de gestão e monitorização. Uma peça de comentários ouvidos repetidamente de clientes é a necessidade de uma experiência de utilizador único monitorizar e gerir no local e cargas de trabalho do Azure. Provavelmente, sabe que o portal do Azure é o hub para todos os serviços do Azure e oferece um gestão avançada de experiência com capacidades, tal como dashboards para ser afixado recursos, inteligente procura para localizar recursos e etiquetagem para gestão de recursos. Para consolidar e simplificar o fluxo de trabalho de monitorização e gestão, iniciamos adicionar as capacidades de portais do OMS no portal do Azure. Estamos satisfeitos anunciar a maioria das funcionalidades do portal do OMS agora fazem parte do portal do Azure. Na verdade, algumas das novas funcionalidades, tais como o Gestor de tráfego só estão disponíveis no portal do Azure. Existem apenas alguns dados em falta irregulares Restante, mais impactful a ser cinco soluções que ainda estão em processo de ser movido para o portal do Azure. Se não estiver a utilizar estas funcionalidades, poderá efetuar tudo o que estava a fazer no portal do OMS com o portal do Azure e muito mais. Se ainda não o fez, recomendamos que comece hoje a utilizar o portal do Azure! 

Esperamos de fechar para baixo os intervalos restantes entre dois portais por de 2018 de Agosto. Com base nos comentários de clientes, iremos irá comunicar a linha cronológica para sunsetting o portal do OMS. Estamos entusiasmados mover para o portal do Azure e a transição que seja mais fácil de esperar. Mas compreendemos alterações são difíceis e podem ser incómodas. Enviar quaisquer dúvidas, comentários ou perguntas para LAUpgradeFeedback@microsoft.com. O resto deste artigo passa pelos cenários principais, as lacunas atuais e o plano para esta transição. 


## <a name="what-will-change"></a>O que, a alteração? 
As seguintes alterações estão a ser anunciadas com a descontinuação do portal do OMS. Cada uma destas alterações é descrita com maior detalhe nas secções abaixo.

- A nova experiência de gestão de alertas irá substituir a solução de gestão de alerta.
- Gestão de acesso de utilizador irá ser efetuada no portal do Azure utilizando o controlo de acesso baseado em funções do Azure.
- O conector do Application Insights já não são necessários, uma vez que a mesma funcionalidade pode ser ativada através de consultas de área de trabalho em vários locais.
- A aplicação de Mobile OMS vão ser preterida. 
- A solução NSG está a ser substituída com a funcionalidade avançada disponível através da solução de análise de tráfego.



## <a name="current-known-gaps"></a>Lacunas conhecidas atuais 
Atualmente, estão em falta irregulares algumas funcionalidades que requerem que continuam a utilizar o Portal do OMS. Estes intervalos são a ser fechados e este documento será atualizado corretamente. Também devem consultar a [Azure atualiza as](https://azure.microsoft.com/updates/?product=log-analytics) em curso anúncios sobre extensões e alterações de.

- As seguintes soluções ainda não são totalmente funcionais no portal do Azure. Deve continuar a utilizar estas soluções no portal clássico do até que o se estiver a atualizar.

    - Soluções de análise do Windows ([preparação para a atualização](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [estado de funcionamento do dispositivo](https://docs.microsoft.com/windows/deployment/update/device-health-monitor), e [compatibilidade das atualizações](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [Análise DNS](log-analytics-dns.md) 
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Para aceder a recursos de análise de registos no Azure, tem de ser concedido ao utilizador acesso através de [acesso baseado em funções do Azure](#user-access-and-role-migration).
- As agendas de atualização que foram criadas com o portal do OMS poderão não ser refletidas nas implementações de atualização agendada ou atualizar o histórico de tarefas do dashboard de gestão de atualização no portal do Azure. Esta falha deve ser resolvido no final de Junho de 2018.
- Só pode ser ativada a funcionalidade de pré-visualização de registos personalizados através do Portal do OMS. No final de Junho de 2018, esta será automaticamente ativada para todos os espaços de trabalho.
 
## <a name="what-should-i-do-now"></a>O que devo fazer agora?  
Deve referir-se a [perguntas comuns para transição do portal do OMS ao portal do Azure para utilizadores de análise de registos](../log-analytics/log-analytics-oms-portal-faq.md) para obter informações sobre como efetuar a transição para o portal do Azure. Se o [lacunas descrito acima](#current-known-gaps) não se aplicam ao seu ambiente, em seguida, deve considerar a começar a utilizar o portal do Azure como a sua experiência primária. Enviar quaisquer comentários, dúvidas ou preocupações para LAUpgradeFeedback@microsoft.com.

## <a name="changes-to-alerts"></a>Alterações para alertas 

### <a name="alert-extension"></a>Extensão de alerta  
Alertas estão a ser [expandido no portal do Azure](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Assim que for concluído, as ações de gestão de alertas só estará disponíveis no portal do Azure. Alertas existentes continuarão a ter seja apresentado no portal do OMS. Se aceder programaticamente alertas através da API de REST de alerta de análise do registo ou modelo de recursos do registo de análise de alerta, terá de utilizar grupos de ação em vez de ações nas suas chamadas de API, modelos Azure Resource Manager e os comandos do PowerShell.

### <a name="alert-management-solution"></a>Solução de gestão de alertas
Em vez do [solução de gestão de alertas](log-analytics-solution-alert-management.md), pode utilizar [Monitor de Azure unified interface alerta](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para visualizar e gerir os alertas. Esta nova experiência agrega alertas de várias origens dentro do Azure, incluindo alertas de registo da análise de registos. Pode ver as distribuições dos alertas, tirar partido do agrupamento automático de alertas relacionados através de grupos de smart e ver alertas entre várias subscrições ao aplicar os filtros avançados. Todas estas funcionalidades estão disponíveis em pré-visualização 4 de Junho de 2018 a iniciar. A solução de gestão de alertas não estarão disponível no portal do Azure. 

Os dados recolhidos pela solução de gestão de alertas (registos com um tipo de alerta) continuam a ser na análise de registos, desde que a solução está instalada para a área de trabalho. A partir de Agosto de 2018, transmissão em fluxo de alertas do alerta unificada para áreas de trabalho será ativada, substituir esta capacidade. Algumas alterações de esquema esperadas e serão comunicadas numa data posterior.

## <a name="user-access-and-role-migration"></a>Migração de acesso e a função de utilizador
Gestão de acesso ao portal do Azure é mais rico e mais eficiente do que a gestão de acesso no Portal do OMS, mas requer alguns conversões. Consulte [gerir áreas de trabalho](log-analytics-manage-access.md#manage-accounts-and-users) para obter detalhes sobre a gestão de acesso na análise de registos.

Iniciar 25 de Junho e continuar através de Julho, conversão automática do acesso controlar permissões a partir do portal do OMS ao portal do Azure permissões serão iniciado. Depois da conversão é concluída, a secção de gestão de utilizador do Portal do OMS encaminhará os utilizadores para o controlo de acesso (IAM) no Azure. 

Durante a conversão, o sistema irá verificar cada utilizador ou grupo de segurança que tem permissões no portal do OMS e determinar se tem permissões ou ao nível do mesmo no Azure. Se as permissões estão em falta, atribuirá as seguintes funções de áreas de trabalho relevantes e soluções.

| Permissão de portal do OMS | Função do Azure |
|:---|:---|
| ReadOnly | Leitor do Log Analytics |
| Contribuinte | Contribuidor do Log Analytics |
| Administrador | Proprietário |

Para se certificar de que as permissões de excessivos não estão atribuídas a utilizadores, o sistema não atribuirá automaticamente estas permissões ao nível do grupo de recursos. Como resultado, os administradores de área de trabalho devem atribuir manualmente próprios _proprietário_ ou _contribuinte_ funções ao nível de grupo ou de subscrição de recursos para efetuar as seguintes ações.

- Adicionar ou remover soluções
- Definir novas vistas personalizadas
- Gerir alertas 

Em alguns casos, a conversão automática não é possível aplicar a permissão e solicitará ao administrador atribuir as permissões manualmente.

## <a name="oms-mobile-app"></a>Aplicação móvel do OMS
A aplicação móvel do OMS será sunsetted juntamente com o portal do OMS. Em vez da aplicação móvel do OMS, para aceder a informações sobre infraestrutura a TI, dashboards e consultas guardadas, pode aceder a portal do Azure diretamente a partir do seu browser no seu dispositivo móvel. Para obter os alertas, deve configurar [os grupos de ação de Azure](../monitoring-and-diagnostics/monitoring-action-groups.md) para receber notificações sob a forma de SMS ou uma chamada de voz

## <a name="application-insights-connector-and-solution"></a>Conector do Application Insights e soluções
[Conector do Application Insights](log-analytics-app-insights-connector.md) fornece uma forma de colocar os dados do Application Insights para uma área de trabalho de análise de registos. Este duplicação de dados não foi necessária para ativar a visibilidade em toda a infraestrutura e dados de aplicações.

Com o suporte dos [recursos entre consultas](log-analytics-cross-workspace-search.md), já não está a esta necessidade de duplicar dados. Como tal, a solução Application Insights existente vai ser preterida. A partir de Julho, não será capaz de ligar novos recursos do Application Insights para áreas de trabalho de análise de registos. As ligações existentes e dashboards vão continuar a funcionar até Novembro de 2018.


## <a name="azure-network-security-group-analytics"></a>Análise do Grupo de Segurança de Rede do Azure
O [solução de análise de grupo de segurança de rede do Azure](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) será substituído com recentemente iniciado [análise de tráfego](../network-watcher/traffic-analytics.md) que fornece visibilidade para a atividade de utilizador e a aplicação em redes em nuvem. Análise de tráfego ajuda-o a atividade de rede da sua organização, proteger aplicações e dados de auditoria, otimizar o desempenho da carga de trabalho e manter em conformidade. 

Esta solução analisa os registos de NSG fluxo e fornece informações sobre o seguinte.

- Fluxos de tráfego entre as redes entre o Azure e Internet, regiões de nuvem pública, VNETs e sub-redes.
- As aplicações e protocolos na sua rede, sem a necessidade de sniffers ou aparelhos de coleção de fluxo dedicado.
- Talkers superiores, aplicações chatty, conversações de VM na nuvem, o tráfego hotspots.
- As origens e destinos de tráfego entre as VNETs, entre as relações entre aplicações e serviços críticos de negócio.
- Segurança, incluindo o tráfego malicioso, portas aberto à Internet, aplicações ou VMs tentativa de acesso à Internet.
- Utilização de capacidade, o que ajuda a eliminar os problemas das através de aprovisionamento ou uma subutilização.

Pode continuar a dependem as definições de diagnóstico para enviar registos NSG para análise de registos para que existentes guardar pesquisas, alertas, dashboards continuarão a funcionar. Os clientes que já instalou a solução podem continuar a utilizá-lo até aviso prévio. Iniciar 20 de Junho a solução NSG será removida do marketplace e disponibilizado através da Comunidade como um [modelo de início rápido do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="next-steps"></a>Passos Seguintes
- Consulte [perguntas comuns para transição do portal do OMS ao portal do Azure para utilizadores de análise de registos](log-analytics-oms-portal-faq.md) para obter orientações sobre a partir do portal do OMS a mover para o portal do Azure.