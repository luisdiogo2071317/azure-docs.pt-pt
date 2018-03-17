---
title: "Controlo de acesso baseado em funções na Automatização do Azure"
description: "O controlo de acesso baseado em funções (RBAC) permite uma gestão de acesso para os recursos do Azure. Este artigo descreve como configurar o RBAC na Automatização do Azure."
keywords: "rbac de automatização, controlo de acesso baseado em funções , rbac do azure"
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 9fb77f3b435491b5ac5b16327d6ce74f90664a79
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="role-based-access-control-in-azure-automation"></a>Controlo de acesso baseado em funções na Automatização do Azure

O controlo de acesso baseado em funções (RBAC) permite uma gestão de acesso para os recursos do Azure. Utilizar [RBAC](../active-directory/role-based-access-control-configure.md), pode segregar funções na sua equipa e conceder apenas a quantidade de acesso a utilizadores, grupos e aplicações que precisam para desempenhar as suas funções. O acesso baseado em funções pode ser concedido aos utilizadores através do portal do Azure, das ferramentas da Linha de Comandos do Azure ou de APIs de Gestão do Azure.

## <a name="roles-in-automation-accounts"></a>Funções de contas de automatização
Na Automatização do Azure, é concedido acesso ao atribuir a função RBAC adequada aos utilizadores, grupos e aplicações no âmbito da conta de Automatização. Seguem-se as funções incorporadas suportadas por uma conta de Automatização:

| **Função** | **Descrição** |
|:--- |:--- |
| Proprietário |A função de proprietário permite o acesso a todos os recursos e ações dentro de uma conta de automatização, incluindo fornecer acesso a outros utilizadores, grupos e aplicações para gerir a conta de automatização. |
| Contribuinte |A função de Contribuidor permite-lhe gerir tudo, exceto modificar as permissões de acesso de outro utilizador para uma conta de Automatização. |
| Leitor |A função de Leitor permite-lhe ver todos os recursos numa conta de Automatização, mas não permite efetuar quaisquer alterações. |
| Operador de Automatização |A função de Operador de Automatização permite-lhe realizar tarefas operacionais, tais como iniciar, parar, suspender, retomar e agendar tarefas. Esta função é útil se pretender proteger os seus recursos da Conta de Automatização como recursos de credenciais e runbooks contra a visualização ou modificação, mas ainda permite que os membros da sua organização executem estes runbooks. |
|Operador de Tarefas de Automatização|A função de operador de tarefa de automatização permite-lhe criar e gerir tarefas através de runbooks de automatização.|
|Operador de Runbook de Automatização|A função de operador de Runbook de automatização permite-lhe ler as propriedades de runbook. Também é possível criar tarefas do runbook.|
| Contribuidor do Log Analytics | A função de contribuinte de análise do registo permite-lhe ler todos os dados de monitorização e editar as definições de monitorização. Editar definições de monitorização inclui a adicionar a extensão VM para VMs, ao ler as chaves de conta de armazenamento para poder configurar a recolha de registos a partir do armazenamento do Azure, criar e configurar as contas de automatização, adicionar soluções e configurar o diagnóstico do Azure no todos os recursos do Azure.|
| Leitor do Log Analytics | A função de leitor de análise do registo permite-lhe visualizar e pesquisar a todas as monitorização dados, bem como ver definições de monitorização. Isto inclui a visualizar a configuração de diagnóstico do Azure em todos os recursos do Azure. |
| Contribuidor de Monitorização | A função de contribuinte de monitorização permite-lhe ler todos os dados de monitorização e atualizar as definições de monitorização.|
| Leitor de Monitorização | A função de leitor de Montioring permite-lhe ler todos os dados de monitorização. |
| Administrador de Acesso de Utilizador |A função de Administrador de Acesso de Utilizador permite-lhe gerir o acesso de utilizador para as contas de Automatização do Azure. |

## <a name="role-permissions"></a>Permissões de função

As tabelas seguintes descrevem as permissões específicas para cada função. Isto pode incluir ações, que atribua permissões, e NotActions, que restringi-los.

### <a name="owner"></a>Proprietário

Um proprietário pode gerir tudo, incluindo o acesso. A tabela seguinte mostra as permissões concedidas para a função:

|Ações|Descrição|
|---|---|
|Microsoft.Automation/automationAccounts/|Criar e gerir recursos de todos os tipos.|

### <a name="contributor"></a>Contribuinte

Um contribuinte pode gerir tudo, exceto o acesso. A tabela seguinte mostra as permissões concedidas e negado para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Criar e gerir recursos de todos os tipos|
|**Não ações**||
|Microsoft.Authorization/*/Delete| Elimine funções e atribuições de funções.       |
|Microsoft.Authorization/*/Write     |  Crie funções e atribuições de funções.       |
|Microsoft.Authorization/elevateAccess/Action    | Nega a capacidade de criar um administrador de acesso de utilizador.       |

### <a name="reader"></a>Leitor

Um leitor pode ver todos os recursos na conta de automatização, mas não é possível efetuar quaisquer alterações.

|**Ações**  |**Descrição**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Ver todos os recursos na conta de automatização. |

### <a name="automation-job-operator"></a>Operador de Tarefas de Automatização

Um operador de tarefa de automatização é concedido no âmbito da conta de automatização. Isto permite que as permissões de operador para gerir trabalhos da conta do.

|**Ações**  |**Descrição**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorização de leitura.|
|Microsoft.Automation/automationAccounts/jobs/read|Lista as tarefas do runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Retome uma tarefa que está em pausa.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Cancele uma tarefa em curso.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Ler os fluxos de trabalho e de saída.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Colocar em pausa uma tarefa em curso.|
|Microsoft.Automation/automationAccounts/jobs/write|Crie tarefas.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Funções de leitura e atribuições de funções.       |
|Microsoft.Resources/deployments/*      |Criar e gerir implementações do grupo de recursos.         |
|Microsoft.Insights/alertRules/*      | Criar e gerir regras de alertas.        |
|Microsoft.Support/* |Criar e gerir pedidos de suporte.|

### <a name="automation-runbook-operator"></a>Operador de Runbook de Automatização

Uma função de operador de Runbook de automatização é concedida no âmbito do Runbook. Um operador de Runbook de automatização, pode ver o nome do runbook. Esta permissão combinado com 'Operator para a tarefa de automatização' no âmbito da conta de automatização, permite que o operador executar as ações de operador de automatização para um determinado runbook. A tabela seguinte mostra as permissões concedidas para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Lista os runbooks.        |
|Microsoft.Authorization/*/read      | Autorização de leitura.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Funções de leitura e atribuições de funções.         |
|Microsoft.Resources/deployments/*      | Criar e gerir implementações do grupo de recursos.         |
|Microsoft.Insights/alertRules/*      | Criar e gerir regras de alertas.        |
|Microsoft.Support/*      | Criar e gerir pedidos de suporte.        |

### <a name="automation-operator"></a>Operador de Automatização

Um operador de automatização é capaz de iniciar, parar, suspender e retomar trabalhos. A tabela seguinte mostra as permissões concedidas para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorização de leitura.|
|Microsoft.Automation/automationAccounts/jobs/read|Lista as tarefas do runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Retome uma tarefa que está em pausa.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Cancele uma tarefa em curso.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Ler os fluxos de trabalho e de saída.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Colocar em pausa uma tarefa em curso.|
|Microsoft.Automation/automationAccounts/jobs/write|Crie tarefas.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Funções de leitura e atribuições de funções.         |
|Microsoft.Resources/deployments/*      |Criar e gerir implementações do grupo de recursos.         |
|Microsoft.Insights/alertRules/*      | Criar e gerir regras de alertas.        |
|Microsoft.Support/* |Criar e gerir pedidos de suporte.|

### <a name="log-analytics-contributor"></a>Contribuidor do Log Analytics

Um contribuinte de análise do registo pode ler todos os dados de monitorização e editar as definições de monitorização. Editar definições de monitorização inclui a adicionar a extensão VM para VMs; ler as chaves de conta de armazenamento para poder configurar a recolha de registos do armazenamento do Azure; criar e configurar as contas de automatização; Adicionar soluções; e configurar o diagnóstico do Azure em todos os recursos do Azure. A tabela seguinte mostra as permissões concedidas para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|* / leitura|Ler os recursos de todos os tipos, exceto os segredos.|
|Microsoft.Automation/automationAccounts/*|Gerir contas de automatização.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Criar e gerir as extensões de máquina virtual.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Listar chaves de conta de armazenamento clássico.|
|Microsoft.Compute/virtualMachines/extensions/*|Criar e gerir extensões clássico máquina virtual.|
|Microsoft.Insights/alertRules/*|Regras de alertas de leitura/escrita/eliminar.|
|Microsoft.Insights/diagnosticSettings/*|Definições de diagnóstico de leitura/escrita/eliminar.|
|Microsoft.OperationalInsights/*|Gerir a análise de registos.|
|Microsoft.OperationsManagement/*|Gerir soluções de áreas de trabalho.|
|Microsoft.Resources/deployments/*|Criar e gerir implementações do grupo de recursos.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Criar e gerir implementações do grupo de recursos.|
|Microsoft.Storage/storageAccounts/listKeys/action|Listar chaves de conta de armazenamento.|
|Microsoft.Support/*|Criar e gerir pedidos de suporte.|


### <a name="log-analytics-reader"></a>Leitor do Log Analytics

Um leitor de análise do registo podem visualizar e pesquisar todos os dados de monitorização como bem como e vista de monitorização de definições, incluindo a visualizar a configuração de diagnóstico do Azure em todos os recursos do Azure. A tabela seguinte mostra as permissões concedeu ou negou para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|* / leitura|Ler os recursos de todos os tipos, exceto os segredos.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Gerir consultas na análise de registos.|
|Microsoft.OperationalInsights/workspaces/search/action|Procure dados de análise de registos.|
|Microsoft.Support/*|Criar e gerir pedidos de suporte.|
|**Não ações**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Não é possível ler as chaves de acesso partilhado.|

### <a name="monitoring-contributor"></a>Contribuidor de Monitorização

Um contribuinte de monitorização pode ler todos os dados de monitorização e atualizar as definições de monitorização. A tabela seguinte mostra as permissões concedidas para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|* / leitura|Ler os recursos de todos os tipos, exceto os segredos.|
|Microsoft.AlertsManagement/alerts/*|Gerir alertas.|
|Microsoft.AlertsManagement/alertsSummary/*|Gerir o dashboard do alerta.|
|Microsoft.Insights/AlertRules/*|Gerir regras de alertas.|
|Microsoft.Insights/components/*|Gerir componentes do Application Insights.|
|Microsoft.Insights/DiagnosticSettings/*|Gerir definições de diagnóstico.|
|Microsoft.Insights/eventtypes/*|Lista de eventos de registo de atividade (eventos de gestão) numa subscrição. Esta permissão é aplicável ao acesso programático e portal de registo de atividade.|
|Microsoft.Insights/LogDefinitions/*|Esta permissão é necessária para os utilizadores que necessitam de aceder a registos de atividade através do portal. Lista categorias de registo no registo de atividade.|
|Microsoft.Insights/MetricDefinitions/*|Ler as definições de métrica (lista de tipos de métricas disponíveis para um recurso).|
|Microsoft.Insights/Metrics/*|Ler métricas para um recurso.|
|Microsoft.Insights/Register/Action|Registe o fornecedor de insights.|
|Microsoft.Insights/webtests/*|Gerir os testes web do Application Insights.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Gerir pacotes de solução de análise de registos.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Gerir a análise de registos pesquisas guardadas.|
|Microsoft.OperationalInsights/workspaces/search/action|Pesquise áreas de trabalho de análise de registos.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Listar chaves para uma área de trabalho de análise de registos.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Gerir configurações de conhecimentos aprofundados de armazenamento de análise de registos.|
|Microsoft.Support/*|Criar e gerir pedidos de suporte.|
|Microsoft.WorkloadMonitor/workloads/*|Gerir as cargas de trabalho.|

### <a name="monitoring-reader"></a>Leitor de Monitorização

Um leitor de monitorização pode ler todos os dados de monitorização. A tabela seguinte mostra as permissões concedidas para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|* / leitura|Ler os recursos de todos os tipos, exceto os segredos.|
|Microsoft.OperationalInsights/workspaces/search/action|Pesquise áreas de trabalho de análise de registos.|
|Microsoft.Support/*|Criar e gerir pedidos de suporte|

### <a name="user-access-administrator"></a>Administrador de Acesso de Utilizador

Um administrador de acesso de utilizador pode gerir o acesso de utilizador nos recursos do Azure. A tabela seguinte mostra as permissões concedidas para a função:

|**Ações**  |**Descrição**  |
|---------|---------|
|* / leitura|Ler todos os recursos|
|Microsoft.Authorization/*|Gerir autorização|
|Microsoft.Support/*|Criar e gerir pedidos de suporte|

## <a name="onboarding"></a>Inclusão

As tabelas seguintes mostram as permissões necessárias mínimo necessárias para as máquinas virtuais de integração para a registo de alterações ou atualizar as soluções de gestão.

### <a name="onboarding-from-a-virtual-machine"></a>Integração de uma máquina virtual

|**Ação**  |**Permissão**  |**Âmbito mínimo**  |
|---------|---------|---------|
|Escrever a nova implementação      | Microsoft.Resources/deployments/*          |Subscrição          |
|Escrever o novo grupo de recursos      | Microsoft.Resources/subscriptions/resourceGroups/write        | Subscrição          |
|Criar nova predefinição área de trabalho      | Microsoft.OperationalInsights/workspaces/write         | Grupo de recursos         |
|Criar nova conta      |  Microsoft.Automation/automationAccounts/write        |Grupo de recursos         |
|Área de trabalho de ligação e a conta      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Área de trabalho</br>Conta de automatização
|Criar solução      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Grupo de recursos          |
|Criar extensão MMA      | Microsoft.Compute/virtualMachines/write         | Máquina Virtual         |
|Criar a pesquisa guardada      | Microsoft.OperationalInsights/workspaces/write          | Área de trabalho         |
|Criar âmbito de configuração      | Microsoft.OperationalInsights/workspaces/write          | Área de trabalho         |
|Solução de ligação para a configuração de âmbito      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Solução         |
|Verifique o estado de integração - área de trabalho de leitura      | Microsoft.OperationalInsights/workspaces/read         | Área de trabalho         |
|Verificação de estado da inclusão - leitura ligado a propriedade de área de trabalho da conta     | Microsoft.Automation/automationAccounts/read      | Conta de automatização        |
|Verifique o estado de integração - solução de leitura      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Solução         |
|Verificação do Estado de inclusão - VM de leitura      | Microsoft.Compute/virtualMachines/read         | Máquina Virtual         |
|Verifique o estado de integração - conta de leitura      | Microsoft.Automation/automationAccounts/read  |  Conta de automatização   |

### <a name="onboarding-from-automation-account"></a>Integração da conta de automatização

|**Ação**  |**Permissão** |**Âmbito mínimo**  |
|---------|---------|---------|
|Criar nova implementação     | Microsoft.Resources/deployments/*        | Subscrição         |
|Criar um novo grupo de recursos     | Microsoft.Resources/subscriptions/resourceGroups/write         | Subscrição        |
|Painel AutomationOnboarding - criar a nova área de trabalho     |Microsoft.OperationalInsights/workspaces/write           | Grupo de recursos        |
|Painel AutomationOnboarding - ligada área de trabalho de leitura     | Microsoft.Automation/automationAccounts/read        | Conta de automatização       |
|Painel AutomationOnboarding - solução de leitura     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Solução        |
|Painel AutomationOnboarding - área de trabalho de leitura     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Área de trabalho        |
|Criar ligação para a área de trabalho e de conta     | Microsoft.OperationalInsights/workspaces/write        | Área de trabalho        |
|Conta para shoebox de escrita      | Microsoft.Automation/automationAccounts/write        | Conta        |
|Criar solução      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Grupo de Recursos         |
|Criar/editar guardada pesquisa     | Microsoft.OperationalInsights/workspaces/write        | Área de trabalho        |
|Criar/Editar configuração de âmbito     | Microsoft.OperationalInsights/workspaces/write        | Área de trabalho        |
|Solução de ligação para a configuração de âmbito      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Solução         |
|**Passo 2 - carregar várias VMs**     |         |         |
|Painel VMOnboarding - extensão MMA criar     | Microsoft.Compute/virtualMachines/write           | Máquina Virtual        |
|Criar / editar pesquisa guardada     | Microsoft.OperationalInsights/workspaces/write           | Área de trabalho        |
|Criar / Editar configuração de âmbito  | Microsoft.OperationalInsights/workspaces/write   | Área de trabalho|

## <a name="update-management"></a>Gestão de atualizações

Gestão de atualizações atinge em vários serviços para fornecer o respetivo serviço. A tabela seguinte mostra as permissões necessárias para gerir implementações de atualização de gestão:

|**Recurso**  |**Função**  |**Âmbito**  |
|---------|---------|---------|
|Conta de automatização     | Contribuidor do Log Analytics       | Conta de automatização        |
|Conta de automatização    | Contribuinte de Máquina Virtual        | Grupo de recursos para a conta        |
|Área de trabalho do Log Analytics     | Contribuidor do Log Analytics| Área de trabalho do Log Analytics        |
|Área de trabalho do Log Analytics |Leitor do Log Analytics| Subscrição|
|Solução     |Contribuidor do Log Analytics         | Solução|
|Máquina Virtual     | Contribuinte de Máquina Virtual        | Máquina Virtual        |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>Configurar o RBAC para a conta de automatização com o portal do Azure
1. Inicie sessão no [portal do Azure](https://portal.azure.com/) e abra a sua conta de Automatização a partir da página Contas de Automatização.  
2. Clique em de **(IAM) do controlo de acesso** controlo no canto superior esquerdo. Esta ação abre o **(IAM) do controlo de acesso** página onde pode adicionar novos utilizadores, grupos e aplicações para gerir a automatização de conta e ver funções existentes que podem ser configuradas para a conta de automatização.
   
   ![Botão de acesso](media/automation-role-based-access-control/automation-01-access-button.png)  

### <a name="add-a-new-user-and-assign-a-role"></a>Adicionar um novo utilizador e atribuir uma função
1. Do **(IAM) do controlo de acesso** página, clique em **+ adicionar** para abrir o **adicionar permissões** página onde pode adicionar um utilizador, grupo ou aplicação e atribuir-lhes uma função.  

2. Selecione uma função na lista de funções disponíveis. Pode escolher qualquer uma das funções incorporadas disponíveis que suporte a uma conta de automatização ou qualquer função personalizada que pode ter definido.

3. Escreva o nome de utilizador do utilizador que pretende atribuir as permissões no **selecione** campo. Selecione o utilizador a partir da lista e clique em **guardar**.
   
   ![Adicionar utilizadores](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Agora deverá ver o utilizador adicionado para o **utilizadores** página com a função selecionada atribuída.  
   
   ![Listar utilizadores](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   Também pode atribuir uma função ao utilizador a partir da página **Funções**. 
4. Clique em **funções** do **(IAM) do controlo de acesso** página, para abrir o **funções** página. A partir daí, pode ver o nome da função, o número de utilizadores e os grupos atribuídos a essa função.
   
    ![Atribuir função a partir da página Utilizadores](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Controlo de acesso baseado em funções só pode ser definido no âmbito da conta de automatização e não em qualquer recurso abaixo a conta de automatização.

### <a name="remove-a-user"></a>Remover um utilizador
Pode remover a permissão de acesso para um utilizador que não está a gerir a conta de automatização ou que já não trabalha para a organização. Seguem-se os passos para remover um utilizador: 

1. Do **(IAM) do controlo de acesso** página, selecione a utilizador pretenda remover e clique em **remover**.
2. Clique no botão **Remover** na página de detalhes de atribuição.
3. Clique em **Sim** para confirmar a remoção.

   ![Remover utilizadores](media/automation-role-based-access-control/automation-08-remove-users.png)

## <a name="role-assigned-user"></a>Função de utilizador atribuída

Quando um utilizador atribuído a uma função inicia sessão no Azure e seleciona a respetiva conta de automatização, pode ver a conta do proprietário listada na lista de **diretórios**. Para poder ver a conta de Automatização à qual foi adicionado, o utilizador tem de alternar o diretório predefinido para o diretório predefinido do proprietário.

### <a name="user-experience-for-automation-operator-role"></a>Experiência de utilizador para a função de operador de Automatização
Quando um utilizador, que é atribuído para as vistas de função de operador de automatização a conta de automatização que estão atribuídos, estes apenas podem ver a lista de runbooks, tarefas de runbook e agendas criadas da conta de automatização, mas não é possível ver a respetiva definição. Pode iniciar, parar, suspender, retomar ou agendar a tarefa de runbook. O utilizador não tem acesso a outros recursos de automatização, tais como configurações, grupos de trabalho híbrida ou nós de DSC.

![Sem acesso a recursos](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

O utilizador tem acesso para ver e criar agendas, mas não tem acesso a qualquer outro tipo de recurso.

Este utilizador também não tem acesso para ver os webhooks associados a um runbook

![Sem acesso aos webhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>Configurar o RBAC para a sua conta de automatização com o Azure PowerShell
Acesso baseado em funções também pode ser configurado para uma conta de automatização utilizando os seguintes [cmdlets Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) lista todas as funções do RBAC que estão disponíveis no Azure Active Directory. Pode utilizar este comando juntamente com a propriedade **Nome** para listar todas as ações que podem ser utilizadas com uma função específica.

```powershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
```

Segue-se a saída de exemplo:

```powershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action, 
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
``` 

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) lista atribuições de funções de RBAC do Azure AD no âmbito especificado. Sem quaisquer parâmetros, este comando devolve todas as atribuições de funções efetuadas sob a subscrição. Utilize o parâmetro **ExpandPrincipalGroups** para listar atribuições de acesso para o utilizador especificado, bem como para os grupos dos quais o utilizador é membro.  
    **Exemplo:** utilize o seguinte comando para listar todos os utilizadores e as respetivas funções dentro de uma conta de automatização.

```powershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Segue-se a saída de exemplo:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) atribuir acesso a utilizadores, grupos e aplicações num determinado âmbito.  
    **Exemplo:** utilize o seguinte comando para atribuir a função "Operador de automatização" para um utilizador no âmbito da conta de automatização.

```powershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Segue-se a saída de exemplo:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

• Utilize [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) para remover o acesso de um utilizador especificado, grupo ou aplicação a partir de um âmbito específico.  
    **Exemplo:** utilize o seguinte comando para remover o utilizador da função "Operador de automatização" no âmbito da conta de automatização.

```powershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Nos exemplos anteriores, substitua **sessão Id**, **Id de subscrição**, **nome do grupo de recursos**, e **nome da conta de automatização** com o Detalhes da conta. Escolha **Sim** quando lhe for solicitado para confirmar antes de continuar para remover a atribuição de função de utilizador.   

## <a name="next-steps"></a>Passos Seguintes
* Para obter informações sobre diferentes maneiras de configurar o RBAC para a Automatização do Azure, veja [Gerir o RBAC com o Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).
* Para obter detalhes sobre diferentes formas de iniciar um runbook, consulte o artigo [Iniciar um runbook](automation-starting-a-runbook.md)
* Para obter informações sobre os vários tipos de runbook, veja [Tipos de runbook de Automatização do Azure](automation-runbook-types.md)

