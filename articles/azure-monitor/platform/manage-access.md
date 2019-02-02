---
title: Gerir áreas de trabalho no Log Analytics do Azure e no portal do OMS | Microsoft Docs
description: Pode gerir áreas de trabalho no Log Analytics do Azure e no Portal do OMS com uma variedade de tarefas administrativas em utilizadores, contas, áreas de trabalho e contas do Azure.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 32a31a87bacbb13cd3b2cb4561ac04e54d51ba46
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55656758"
---
# <a name="manage-workspaces"></a>Gerir áreas de trabalho

Para gerir o acesso ao Log Analytics, tem de realizar diversas tarefas administrativas relacionadas com áreas de trabalho. Este artigo fornece conselhos e procedimentos para gerir áreas de trabalho. Uma área de trabalho é essencialmente um contentor que inclui informações da conta e informações de configuração simples para a conta. O utilizador ou outros membros da sua organização podem utilizar várias áreas de trabalho para gerir diferentes conjuntos de dados recolhidos da totalidade ou de partes da sua infraestrutura de TI.

Para criar uma área de trabalho, terá de:

1. Ter uma subscrição do Azure.
2. Escolher um nome de área de trabalho.
3. Associe a área de trabalho uma das suas subscrições e grupos de recursos.
4. Escolher uma localização geográfica.

## <a name="determine-the-number-of-workspaces-you-need"></a>Determinar o número de áreas de trabalho necessárias
Uma área de trabalho é um recurso do Azure e é um contentor onde os dados são recolhidos, agregados, analisados e apresentados no portal do Azure.

Pode ter múltiplas áreas de trabalho por subscrição do Azure e ter acesso a mais de uma área de trabalho, com capacidade para consultá-las facilmente. Esta secção descreve quando pode ser útil criar mais do que uma área de trabalho.

Atualmente, uma área de trabalho fornece:

* Uma localização geográfica para armazenamento de dados
* Isolamento de dados para definir os direitos de acesso de utilizador diferente
* Como o âmbito para a configuração das definições [escalão de preço](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [retenção](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) e [capping de dados](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#daily-cap) 

Do ponto de vista para consumo, é recomendável que criar áreas de trabalho mínimo possível. Faz administração e a experiência de consulta mais fácil e rápido. Mas, com base nas características anteriores, talvez queira criar várias áreas de trabalho se:

* For uma empresa global e precisar de dados armazenados em regiões específicas por motivos de soberania ou conformidade dos dados.
* Estiver a utilizar o Azure e pretender evitar custos de transferência de dados de saída ao ter uma área de trabalho na mesma região dos recursos do Azure que gere.
* Pretender atribuir custos a diferentes departamentos ou grupos empresariais com base na respetiva utilização através da criação de uma área de trabalho para cada departamento ou grupo empresarial na sua própria subscrição do Azure.
* É um fornecedor de serviços geridos e necessita de manter os dados do Log Analytics de cada cliente gerido isolado em relação aos de outros clientes.
* Gerir vários clientes e pretender que cada cliente / departamento ou grupo empresarial veja os seus próprios dados, mas não os dados de outras pessoas.

Quando utilizar agentes do Windows para recolher dados, pode [configurar cada agente para comunicar a uma ou mais áreas de trabalho](../../azure-monitor/platform/agent-windows.md).

Se estiver a utilizar o System Center Operations Manager, cada grupo de gestão do Operations Manager só pode ser ligado a uma área de trabalho. Pode instalar o Microsoft Monitoring Agent em computadores geridos pelo Operations Manager e fazer com que o agente reporte ao Operations Manager e a uma área de trabalho do Log Analytics diferente.

## <a name="workspace-information"></a>Informações da área de trabalho

Pode ver detalhes sobre a sua área de trabalho no portal do Azure. 

1. Se ainda não o fez, inicie sessão no [Portal do Azure](https://portal.azure.com).

2. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.  

    ![Portal do Azure](media/manage-access/azure-portal-01.png)  

3. No painel de subscrições do Log Analytics, selecione uma área de trabalho.

4. A página de área de trabalho apresenta detalhes sobre a introdução, configuração e ligações para informações adicionais.  

    ![Detalhes da área de trabalho](./media/manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Gerir contas e utilizadores
Cada área de trabalho pode ter múltiplas contas associadas e cada conta pode ter acesso a várias áreas de trabalho. O acesso é gerido através de [acesso baseado em função do Azure](../../role-based-access-control/role-assignments-portal.md). Estes direitos de acesso aplica-se no portal do Azure e o acesso de API.


As atividades seguintes também necessitam de permissões do Azure:

| Ação                                                          | Permissões do Azure Necessárias | Notas |
|-----------------------------------------------------------------|--------------------------|-------|
| Adicionar e remover soluções de gestão                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Estas permissões têm de ser concedidas ao nível do grupo de recursos ou da subscrição. |
| Alterar o escalão de preço                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Ver dados nos mosaicos de solução *Backup* e *Site Recovery* | Administrador/Coadministrador | Acede aos recursos implementados com o modelo de implementação clássica |
| Criar um espaço de trabalho no portal do Azure                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Gerir o acesso ao Log Analytics com permissões do Azure
Para conceder acesso à área de trabalho do Log Analytics com permissões do Azure, siga os passos em [Utilize atribuições de funções para gerir o acesso aos recursos de subscrição do Azure](../../role-based-access-control/role-assignments-portal.md).

O Azure tem duas funções de utilizador incorporadas para o Log Analytics:
- Leitor do Log Analytics
- Contribuidor do Log Analytics

Os membros da função *Leitor do Log Analytics* podem:
- Ver e procurar todos os dados de monitorização 
- Ver e monitorizar as definições, incluindo ver a configuração dos diagnósticos do Azure em todos os recursos do Azure.

A função de leitor do Log Analytics inclui as seguintes ações do Azure:

| Tipo    | Permissão | Descrição |
| ------- | ---------- | ----------- |
| Ação | `*/read`   | Capacidade de ver todos os recursos do Azure e a configuração do recurso. Inclui ver: <br> o estado da extensão da máquina virtual <br> a configuração dos diagnósticos do Azure nos recursos <br> todas as propriedades e definições de todos os recursos |
| Ação | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Capacidade para executar consultas Log Search v2 |
| Ação | `Microsoft.OperationalInsights/workspaces/search/action` | Capacidade para executar consultas Log Search v1 |
| Ação | `Microsoft.Support/*` | Capacidade para abrir pedidos de suporte |
|Ação Não | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Impede a leitura da área de trabalho chave necessária para utilizar a API de recolha de dados e para instalar agentes. Isso evita que o utilizador a adição de novos recursos para a área de trabalho |


Os membros da função *Contribuidor do Log Analytics* podem:
- Ler todos os dados de monitorização, como o leitor do Log Analytics pode  
- Criar e configurar contas de Automatização  
- Adicionar e remover soluções de gestão    
    > [!NOTE] 
    > Para realizar com êxito as últimas duas ações, esta permissão tem de ser concedidas ao nível de grupo ou uma subscrição do recurso.  

- Ler as chaves das contas de armazenamento   
- Configurar a recolha de registos a partir do Armazenamento do Azure  
- Editar as definições de monitorização para recursos do Azure, incluindo
  - Adicionar a extensão de VM a VMs
  - Configurar os diagnósticos do Azure em todos os recursos do Azure

> [!NOTE] 
> Pode utilizar a capacidade de adicionar uma extensão de máquina virtual a máquinas virtuais para obter o controlo total das mesmas.

A função de Contribuidor do Log Analytics inclui as seguintes ações do Azure:

| Permissão | Descrição |
| ---------- | ----------- |
| `*/read`     | Capacidade para ver todos os recursos e configuração dos mesmos. Inclui ver: <br> o estado da extensão da máquina virtual <br> a configuração dos diagnósticos do Azure nos recursos <br> todas as propriedades e definições de todos os recursos |
| `Microsoft.Automation/automationAccounts/*` | Capacidade para criar, e configurar contas de Automatização do Azure, incluindo adicionar e editar runbooks |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Adicionar, atualizar e remover extensões de máquinas virtuais, incluindo a extensão Microsoft Monitoring Agent e o Agente do OMS para a extensão do Linux |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Ver a chave da conta de armazenamento. Necessária para configurar o Log Analytics para ler registos a partir das contas de Armazenamento do Azure |
| `Microsoft.Insights/alertRules/*` | Adicionar, atualizar e remover regras de alertas |
| `Microsoft.Insights/diagnosticSettings/*` | Adicionar, atualizar e remover as definições de diagnósticos em recursos do Azure |
| `Microsoft.OperationalInsights/*` | Adicionar, atualizar e remover a configuração de áreas de trabalho do Log Analytics |
| `Microsoft.OperationsManagement/*` | Adicionar e remover soluções de gestão |
| `Microsoft.Resources/deployments/*` | Criar e eliminar as implementações. Necessário para adicionar e remover soluções, áreas de trabalho e contas de automatização |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Criar e eliminar as implementações. Necessário para adicionar e remover soluções, áreas de trabalho e contas de automatização |

Para adicionar e remover utilizadores de uma função de utilizador, é necessário ter as permissões `Microsoft.Authorization/*/Delete` e `Microsoft.Authorization/*/Write`.

Utilize estas funções para conceder aos utilizadores acesso em âmbitos diferentes:
- Subscrição - acesso a todas as áreas de trabalho da subscrição
- Grupo de Recursos - acesso a todas as áreas de trabalho no grupo de recursos
- Recurso - acesso apenas à área de trabalho especificada

Recomendamos que realiza as atribuições ao nível do recurso (área de trabalho), para garantir o controlo de acesso correto.  Utilize [funções personalizadas](../../role-based-access-control/custom-roles.md) para criar funções com as permissões específicas necessárias.

## <a name="next-steps"></a>Passos Seguintes
* Ver [descrição geral do agente do Log Analytics](../../azure-monitor/platform/log-analytics-agent.md) para recolher dados de computadores no seu datacenter ou outro ambiente de cloud.
* Consulte [Recolher dados sobre Máquinas Virtuais do Azure](../../azure-monitor/learn/quick-collect-azurevm.md) para configurar a recolha de dados a partir de VMs do Azure.  
* [Adicionar soluções do Log Analytics a partir da Galeria de Soluções](../../azure-monitor/insights/solutions.md) para adicionar funcionalidade e recolher dados.

