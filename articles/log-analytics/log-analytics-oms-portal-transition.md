---
title: Portal do OMS para o Azure | Documentos da Microsoft
description: Portal do OMS está a ser sunsetted com todas as funcionalidades de mudar para o portal do Azure. Este artigo fornece detalhes sobre esta transição.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: b9fb32f4f014f8e0fb67b558a2806d74edaac56c
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576020"
---
# <a name="oms-portal-moving-to-azure"></a>Portal do OMS para o Azure

> [!NOTE]
> Este artigo aplica-se para a cloud pública do Azure e a cloud de governo, exceto onde indicado o contrário.

Uma parte do feedback recebido repetidamente dos clientes de Log Analytics é a necessidade de uma experiência de usuário único monitorizar e gerir cargas de trabalho do Azure e no local. Provavelmente já sabe o portal do Azure é o hub para todos os serviços do Azure e oferece uma gestão avançada de experiência com recursos como os dashboards para fixar recursos, a pesquisa inteligente para localizar recursos e a identificação para gestão de recursos. Para consolidar e simplificar o fluxo de trabalho de monitorização e gestão, começamos a adicionar as capacidades de portais do OMS no portal do Azure. Temos o prazer de anunciar a maioria dos recursos do portal do OMS agora fazem parte do portal do Azure. Na verdade, alguns dos novos recursos, tais como análise de tráfego só estão disponíveis no portal do Azure. Existem apenas algumas lacunas restantes incluindo algumas soluções que ainda são o processo de ser movidos para o portal do Azure. Se não estiver a utilizar estas funcionalidades, será capaz de realizar tudo que estava a fazer no portal do OMS com o portal do Azure e muito mais. Se ainda não o fez, recomendamos que começa a utilizar o portal do Azure hoje mesmo! 

Esperamos que fechar as lacunas restantes dos dois portais, Agosto de 2018. Com base nos comentários dos clientes, iremos comunicar a linha cronológica para sunsetting no portal do OMS. Temos o prazer de passar para o portal do Azure e esperar que a transição para ser fácil. Mas estamos cientes de alterações são difíceis e podem ser prejudicais. Envie dúvidas, comentários ou preocupações para **LAUpgradeFeedback@microsoft.com**. O restante deste artigo vai sobre os cenários-chave, as lacunas atuais e o plano para essa transição. 

## <a name="progress"></a>Progresso
Seguem-se as atualizações que foram concluídas desde as versões anteriores deste artigo.

### <a name="july-27"></a>27 de Julho

- [Análise de DNS](log-analytics-dns.md) agora é totalmente funcional no portal do Azure.
- [Gestão de atualizações](../automation/automation-update-management.md) foi atualizado para ficar completamente funcional no portal do Azure. Ver [migrar as implementações de atualização do OMS para o Azure](../automation/migrate-oms-update-deployments.md) para obter detalhes.
- [Alertas](#changes-to-alerts) agora foram estendidos totalmente no portal do Azure.
- [Funcionalidade de pré-visualização de registos personalizados](log-analytics-data-sources-custom-logs.md) agora é automaticamente ativado para todas as áreas de trabalho.

## <a name="what-will-change"></a>O que irá mudar? 
As seguintes alterações estejam sendo anunciadas com a descontinuação do portal do OMS. Cada uma dessas alterações é descrita mais detalhadamente nas secções abaixo.

- Será capaz de criar novas áreas de trabalho apenas no portal do Azure.
- A nova experiência de gestão de alertas substituirá a solução de gestão de alertas.
- Gestão de acesso de utilizador será efetuada no portal do Azure com o controlo de acesso baseado em função do Azure.
- O conector do Application Insights já não é necessário uma vez que a mesma funcionalidade pode ser ativada através de consultas em várias áreas de trabalho.
- A aplicação OMS Mobile vão ser preterida. 
- A solução NSG está a ser substituída com a funcionalidade disponível por meio de solução de análise de tráfego.
- Novas ligações a partir do System Center Operations Manager ao Log Analytics requerem os pacotes de gestão atualizado.


## <a name="current-known-gaps"></a>Lacunas conhecidas atuais 
Atualmente, existem algumas falhas de funcionalidade que requerem a continuar a utilizar o Portal do OMS. Essas lacunas estão a ser fechadas e, neste documento vai ser atualizado corretamente. Também deve consultar [Azure atualiza as](https://azure.microsoft.com/updates/?product=log-analytics) contínuo anúncios sobre extensões e alterações de.

- As seguintes soluções ainda não estão totalmente funcionais no portal do Azure. Deve continuar a utilizar estas soluções no portal clássico do até que sejam atualizados.

    - Soluções de análise do Windows ([preparação de atualizações](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [estado de funcionamento do dispositivo](https://docs.microsoft.com/windows/deployment/update/device-health-monitor), e [atualizar conformidade](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Para aceder ao recurso do Log Analytics no Azure, o utilizador deve receber acesso por meio [acesso baseado em função do Azure](#user-access-and-role-migration).


## <a name="what-should-i-do-now"></a>O que devo fazer agora?  
Deve consultar [perguntas comuns para a transição do portal do OMS para o portal do Azure para utilizadores do Log Analytics](../log-analytics/log-analytics-oms-portal-faq.md) para obter informações sobre como efetuar a transição para o portal do Azure. Se o [lacunas descrito acima](#current-known-gaps) não se aplicam ao seu ambiente, em seguida, deve considerar a iniciar com o portal do Azure como a sua experiência primária. Enviar comentários, dúvidas ou preocupações para **LAUpgradeFeedback@microsoft.com**.

A maioria das funcionalidades vão continuar a funcionar sem executar qualquer migração. Exceções estão listadas abaixo.

- Ver [migrar as implementações de atualização do OMS para o Azure](../automation/migrate-oms-update-deployments.md) para obter detalhes sobre a transição da solução de gestão de atualizações. 

## <a name="new-workspaces"></a>Novas áreas de trabalho
A partir 29 de Julho, já não será capaz de criar novas áreas de trabalho através do portal do OMS. Siga as orientações no [criar uma área de trabalho do Log Analytics no portal do Azure](log-analytics-quick-create-workspace.md) para criar uma nova área de trabalho no portal do Azure.

## <a name="changes-to-alerts"></a>Alterações para alertas 

### <a name="alert-extension"></a>Extensão de alerta  

> [!NOTE]
> Alertas agora foram estendidos totalmente no portal do Azure para a cloud pública. Regras de alerta existentes podem ser visualizadas no portal do OMS, mas só podem ser geridos no portal do Azure. Extensão de alertas no portal do Azure será iniciado para a cloud do Azure government em Outubro de 2018.

Os alertas são no processo de ser [expandidos para o portal do Azure](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Quando isso for concluído, as ações de gestão de alertas só estará disponíveis no portal do Azure. Alertas existentes continuarão a ser listados no portal do OMS. Se aceder aos alertas através de programação, utilizando a API de REST de alerta do Log Analytics ou o modelo de recurso alerta do Log Analytics, terá de utilizar grupos de ação em vez de ações em suas chamadas de API, modelos Azure Resource Manager e comandos do PowerShell.

### <a name="alert-management-solution"></a>Solução de gestão de alertas
Em vez do [solução de gestão de alertas](log-analytics-solution-alert-management.md), pode utilizar [Azure Monitor unified interface alerta](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para visualizar e gerir os alertas. Esta nova experiência agrega alertas de várias origens dentro do Azure incluindo alertas de registos do Log Analytics. Pode ver distribuições os alertas, tirar partido do agrupamento automático de alertas relacionados por meio de grupos inteligentes e ver alertas em várias subscrições ao aplicar filtros avançados. Todos esses recursos estão disponíveis em pré-visualização, a partir de 4 de Junho de 2018. A solução de gestão de alertas não estarão disponível no portal do Azure. 

Os dados recolhidos pela solução de gestão de alertas (registos com um tipo de alerta) continuam a ser no Log Analytics, desde que a solução está instalada para a área de trabalho. A partir de Agosto de 2018, transmissão em fluxo de alertas a partir de alertas unificada em áreas de trabalho será ativada, esta capacidade a substituir. Algumas alterações de esquema são esperadas e serão anunciadas numa data posterior.

## <a name="user-access-and-role-migration"></a>Migração de acesso e a função de utilizador
Gestão de acesso ao portal do Azure é mais rica e mais poderoso do que a gestão de acesso no Portal do OMS, mas requer algumas conversões. Ver [gerir áreas de trabalho](log-analytics-manage-access.md#manage-accounts-and-users) para obter detalhes sobre a gestão de acesso no Log Analytics.

Portal para as permissões do portais do Azure a partir de 30 de Julho, as conversões automáticas as permissões de controlo de acesso do OMS será iniciado. Depois de concluída a conversão, a seção de gerenciamento de utilizador do Portal do OMS irá encaminhar os utilizadores para o controlo de acesso (IAM) no Azure. 

Durante a conversão, o sistema irá verificar a cada utilizador ou grupo de segurança que tem permissões no portal do OMS e determinar se é o mesmo nível ou permissões no Azure. Se permissões estão em falta, ele irá atribuir as seguintes funções para as áreas de trabalho relevantes e soluções.

| Permissão de portal do OMS | Função do Azure |
|:---|:---|
| ReadOnly | Leitor do Log Analytics |
| Contribuinte | Contribuidor do Log Analytics |
| Administrador | Proprietário |

Para certificar-se de que não existem permissões excessivas são atribuídas aos utilizadores, o sistema não atribuirá automaticamente estas permissões no nível do grupo de recursos. Como resultado, os administradores de área de trabalho devem atribuir manualmente próprios _proprietário_ ou _contribuinte_ funções no nível de subscrição ou grupo de recursos para efetuar as seguintes ações.

- Adicionar ou remover soluções
- Definir novas vistas personalizadas
- Gerir alertas 

Em alguns casos, a conversão automática não é possível aplicar a permissão e solicitará ao administrador atribuir as permissões manualmente.

## <a name="oms-mobile-app"></a>Aplicação móvel do OMS
A aplicação móvel do OMS será sunsetted, juntamente com o portal do OMS. Em vez da aplicação móvel do OMS, para aceder às informações sobre a infraestrutura sua TI, dashboards e consultas guardadas, pode aceder o portal do Azure diretamente a partir do seu browser no seu dispositivo móvel. Para obter alertas, deve configurar [grupos de ação do Azure](../monitoring-and-diagnostics/monitoring-action-groups.md) para receber notificações na forma de SMS ou uma chamada de voz

## <a name="application-insights-connector-and-solution"></a>Conector do Application Insights e solução
[Conector do Application Insights](log-analytics-app-insights-connector.md) fornece uma forma de trazer dados do Application Insights para uma área de trabalho do Log Analytics. Essa duplicação de dados era necessária para ativar a visibilidade em todos os dados de infraestrutura e a aplicação.

Com o apoio da [consultas entre recursos](log-analytics-cross-workspace-search.md), já não existe essa necessidade de duplicar os dados. Como tal, a solução existente do Application Insights vai ser preterida. A partir de Julho, não será capaz de ligar os novos recursos do Application Insights a áreas de trabalho do Log Analytics. Ligações existentes e dashboards vão continuar a funcionar até Novembro de 2018.


## <a name="azure-network-security-group-analytics"></a>Análise do Grupo de Segurança de Rede do Azure
O [solução de análise de grupo de segurança de rede do Azure](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) será substituído com lançado recentemente [análise de tráfego](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) que fornece visibilidade para a atividade de utilizador e da aplicação em redes em nuvem. Análise de tráfego ajuda-o a atividade de rede da sua organização, aplicativos seguros e dados de auditoria, otimizar o desempenho da carga de trabalho e esteja em conformidade. 

Esta solução analisa os registos de fluxo de NSG e fornece informações sobre o seguinte.

- Fluxos de tráfego nas suas redes entre o Azure e Internet, regiões de cloud pública, VNETs e sub-redes.
- Os aplicativos e protocolos na sua rede, sem a necessidade de (farejadores) ou aplicações de coleção de fluxo dedicado.
- Principais tópicos de conversação, aplicativos chatty, conversações de VM na nuvem, hotspots de tráfego.
- Origens e destinos de tráfego nas VNETs, entre as relações entre aplicativos e serviços críticos de negócio.
- Segurança incluindo o tráfego malicioso, portas abertas para a Internet, aplicativos ou VMs tentar acesso à Internet.
- Utilização da capacidade, que o ajuda a eliminar problemas de através de aprovisionamento ou subutilização.

Pode continuar a confiar em definições de diagnóstico para enviar registos NSG para o Log Analytics, para que os já existentes guardar pesquisas, alertas, dashboards continuarão a funcionar. Os clientes que já tenham instalado a solução podem continuar a utilizá-lo até aviso prévio. A partir 15 de Agosto, a solução de análise do grupo de segurança de rede será removida do marketplace e disponibilizada por meio da Comunidade como um [modelo de início rápido do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Se tiver [ligado o grupo de gestão do Operations Manager ao Log Analytics](log-analytics-om-agents.md), em seguida, irá continuar a trabalhar sem alterações. Em novas ligações no entanto, tem de seguir as orientações no [Microsoft System Center Operations Manager Management Pack para configurar o Operations Management Suite](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/).

## <a name="next-steps"></a>Passos Seguintes
- Ver [perguntas comuns para a transição do portal do OMS para o portal do Azure para utilizadores do Log Analytics](log-analytics-oms-portal-faq.md) para obter orientações sobre a mudança do portal do OMS para o portal do Azure.