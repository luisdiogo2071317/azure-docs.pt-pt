---
title: Perguntas comuns para transição do portal do OMS ao portal do Azure para utilizadores do Log Analytics | Microsoft Docs
description: Respostas a perguntas comuns para os utilizadores de análise de registos transição do portal do OMS no portal do Azure.
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
ms.date: 06/07/2018
ms.author: bwren
ms.openlocfilehash: 1e0fd56b6e420103b4f786985f71a84737db642d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333637"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>Perguntas comuns para transição do portal do OMS ao portal do Azure para utilizadores de análise de registos
Análise de registos utilizado inicialmente a suas próprias portal chamado o portal do OMS para gerir a respetiva configuração e analisar os dados recolhidos.  Todas as funcionalidades deste portal foi movida para o portal do Azure onde continuará a ser desenvolvidas.

Este artigo responde a questões recorrentes para efetuar esta transição de utilizadores.  Se utilizou a análise de registos no portal do OMS, em seguida, pode encontrar respostas aqui para como pode executar as mesmas tarefas no portal do Azure.

## <a name="where-do-i-find-log-analytics-in-azure"></a>Onde posso encontrar análise de registos no Azure?
Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com).  Clique em **todos os serviços**e, na lista de recursos, escreva **Log Analytics**. Selecione **Log Analytics** e, em seguida, selecione a área de trabalho. É apresentada a página de resumo para a área de trabalho.

![Área de trabalho do Log Analytics](media/log-analytics-new-portal/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>Como posso gerir permissões?
Se não tiver acesso à sua área de trabalho de análise de registos no portal do Azure, terá de configurar as suas permissões utilizando [acesso baseado em funções do Azure](../active-directory/role-based-access-control-configure.md). Para obter detalhes sobre a gestão de permissões de área de trabalho, consulte [gerir áreas de trabalho](../log-analytics/log-analytics-manage-access.md#manage-accounts-and-users). Para obter informações sobre a gestão de permissões para alertas, consulte [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md).

## <a name="how-do-i-create-a-new-workspace"></a>Como posso criar uma nova área de trabalho? 
Na lista de áreas de trabalho no portal do Azure, clique em **adicionar** na lista de áreas de trabalho.  Para obter informações detalhadas completas, consulte [criar uma área de trabalho de análise de registos no portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).

![Página de descrição geral](media/log-analytics-new-portal/new-workspace.png)

## <a name="where-is-my-overview-page"></a>Onde está a minha página de descrição geral?
O ecrã principal no portal do OMS apresenta os mosaicos para todas as soluções de gestão instalados na sua área de trabalho e todas as vistas personalizadas que criou. Esta mesma vista está disponível no portal do Azure. Na área de trabalho, selecione **resumo da área de trabalho**.

![Página de descrição geral](media/log-analytics-new-portal/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>Como abrir a pesquisa de registo e o estruturador de vistas?
Ambos **pesquisa registo** e **estruturador de vistas** estão disponíveis na página principal e no menu à esquerda da sua área de trabalho no portal do Azure, junto à direita para **descrição geral**.

## <a name="where-do-i-find-settings"></a>Onde posso encontrar definições?
Muitas das definições no **definições** secção do portal do OMS estão disponíveis no **definições avançadas** menu no portal do Azure para a área de trabalho.

![Definições avançadas](media/log-analytics-new-portal/advanced-settings.png)

As secções seguintes fornecem uma lista completa de como pode aceder às definições que se encontravam anteriormente disponíveis no **definições** secção do portal do OMS.

### <a name="accounts"></a>Contas 
Definições de contas são geridas em diferentes locais no portal do Azure, conforme descrito na seguinte tabela.

| A definição no portal do OMS | Equivalente no portal do Azure |
|:---|:---|
| Conta de Automatização | **Conta de automatização** menu para a área de trabalho. |
| Subscrição do Azure e o plano de dados | **Escalão de preço** menu para a área de trabalho. |
| Gerir utilizadores | Utilize o acesso baseado em funções do Azure para [gerir permissões para a sua área de trabalho](#how-do-i-manage-permissions). |
| Informações da Área de Trabalho | As informações disponíveis no **área de trabalho OMS** menu para a área de trabalho. |

### <a name="alerts"></a>Alertas
Regras de alerta baseadas em consultas de análise de registos são agora geridas na [unified alertas experiência](#how-do-i-create-and-manage-alerts). 

### <a name="computer-groups"></a>Grupos de Computadores
Gerir grupos de computadores no **definições avançadas** menu para a área de trabalho. 

### <a name="connected-sources"></a>Origens Ligadas
Gerir a maioria das definições de origem ligados no **definições avançadas** menu para a área de trabalho. A tabela seguinte fornece detalhes para cada secção neste menu.

| A definição no portal do OMS | Equivalente no portal do Azure |
|:---|:---|
| Servidores Windows   | **As definições avançadas** menu para a área de trabalho. |
| Servidores Linux   | **As definições avançadas** menu para a área de trabalho. |
| Storage do Azure     | **As definições avançadas** menu para a área de trabalho. |
| System Center     | **As definições avançadas** menu para a área de trabalho. |
| Office 365        | Consulte o [documentação da solução de gestão do Office 365](../operations-management-suite/oms-solution-office-365.md) para detalhes de configuração. |
| Telemetria Windows | Ainda não está disponível no portal do Azure. |
| Conector ITSM    | Consulte [ligar ITSM produtos/serviços com o conector de gestão do serviço de TI](../log-analytics/log-analytics-itsmc-connections.md) para obter instruções sobre como ligar o seu serviço ITSM com a análise de registos. |

### <a name="data"></a>Dados
Gerir a maioria das definições de dados no **definições avançadas** menu para a área de trabalho. A tabela seguinte fornece detalhes para cada secção neste menu.

| A definição no portal do OMS | Equivalente no portal do Azure |
|:---|:---|
| Registos de Eventos do Windows           | **As definições avançadas** menu para a área de trabalho. |
| Contadores de desempenho do Windows | **As definições avançadas** menu para a área de trabalho. |
| Contadores de desempenho do Linux   | **As definições avançadas** menu para a área de trabalho. |
| Registos do IIS                     | **As definições avançadas** menu para a área de trabalho. |
| Campos Personalizados                | **As definições avançadas** menu para a área de trabalho. |
| Registos Personalizados                  | **As definições avançadas** menu para a área de trabalho. |
| Syslog                       | **As definições avançadas** menu para a área de trabalho. |
| Application Insights         | Esta solução foi preterida agora que partilham o mesmo motor para dados de análise de registos e o Application Insights.  |
| Controlo de Ficheiros do Windows        | **Registo de alterações** menu na automatização do Azure. Consulte [controlar as alterações no seu ambiente com a solução de controlo de alterações](../automation/automation-change-tracking.md) para obter mais detalhes. |
| Controlo de Registos do Windows        | **Registo de alterações** menu na automatização do Azure. Consulte [controlar as alterações no seu ambiente com a solução de controlo de alterações](../automation/automation-change-tracking.md) para obter mais detalhes. |
| Controlo de Ficheiros do Linux          | **Registo de alterações** menu na automatização do Azure. Consulte [controlar as alterações no seu ambiente com a solução de controlo de alterações](../automation/automation-change-tracking.md) para obter mais detalhes. |

### <a name="solutions"></a>Soluções
Gerir soluções no **soluções** menu para a área de trabalho. 

## <a name="how-do-i-install-and-remove-management-solutions"></a>Como instalar e remover as soluções de gestão?
No portal do OMS, instalar soluções de gestão a partir da Galeria de soluções e removido-los do **definições**. No portal do Azure, [instalar soluções de gestão](../monitoring/monitoring-solutions.md#install-a-management-solution) no Azure Marketplace. [Remover soluções](../monitoring/monitoring-solutions.md#remove-a-management-solution) da lista de soluções instaladas.

## <a name="how-do-i-create-and-manage-alerts"></a>Como criar e gerir alertas?
Regras de alerta baseadas em consultas de análise de registos são agora geridas na [unified alertas experiência](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). Consulte [como expandir os alertas de análise de registos para alertas do Azure](../monitoring-and-diagnostics/monitoring-alerts-extend-tool.md) para obter detalhes sobre configurar e utilizar alertas no portal do Azure.

## <a name="how-do-i-access-my-dashboards"></a>Como posso aceder aos meus dashboards?
[Dashboards](../log-analytics/log-analytics-dashboards.md) na análise de registos foram preteridos.  Pode visualizar dados utilizando a análise de registos [estruturador de vistas](../log-analytics/log-analytics-view-designer.md) que tem funcionalidades adicionais e consulta de pin e vistas de dashboards do Azure.

## <a name="how-do-i-check-my-usage"></a>Como verificar a minha utilização?
Pode facilmente ver e gerir a sua utilização e o custo de análise de registos selecionando **utilização e os custos estimados** na sua área de trabalho.

![Utilização e custos estimados](media/log-analytics-new-portal/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>Posso continuar a utilizar o portal clássico?
Durante um período de tempo limitado, ainda aceder ao portal através deste URL, com o seu próprio nome de área de trabalho: https://\<o nome da sua área de trabalho\>. portal.mms.microsoft.com. É recomendável utilizar o portal do Azure apesar e fornecem-nos comentários na LAUpgradeFeedback@microsoft.com em quaisquer problemas de bloqueio.

## <a name="next-steps"></a>Passos Seguintes

- [Localize e instale as soluções de gestão](../monitoring/monitoring-solutions.md) no portal do Azure.
- Saiba mais sobre [pesquisa de registo no portal do Azure](log-analytics-log-search-portals.md).