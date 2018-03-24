---
title: Funções incorporadas para controlo de acesso do Azure baseada em funções (RBAC) | Microsoft Docs
description: Este tópico descreve a incorporado em funções para o controlo de acesso baseado em funções (RBAC). As funções continuamente são adicionadas, por isso a atualização de documentação.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 0e8f1d97f51fbfedbca1619ef5e7f28a3a0570b9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Funções incorporadas para controlo de acesso baseado em funções do Azure
Inclui as seguintes funções incorporadas que podem ser atribuídas a utilizadores, grupos e serviços do Azure baseada em funções controlo de acesso (RBAC). Não é possível modificar as definições de funções incorporadas. No entanto, pode criar [funções personalizadas no Azure RBAC](role-based-access-control-custom-roles.md) para satisfazer as necessidades específicas da sua organização.

## <a name="built-in-role-descriptions"></a>Descrições de função incorporada
A tabela seguinte fornece breves descrições das funções incorporadas. Clique no nome de função para ver a lista de detalhado **ações** e **notactions** para a função. O **ações** propriedade especifica as ações permitidas nos recursos do Azure. Cadeias de ação podem utilizar carateres universais. O **notactions** propriedade especifica as ações que foram excluídas as ações permitidas.

A ação define o tipo de operações que pode efetuar num tipo de recurso especificado. Por exemplo:
- **Escrever** permite-lhe efetuar operações PUT, POST, PATCH e DELETE.
- **Leitura** permite-lhe efetuar operações GET.

Este artigo aborda apenas as diferentes funções que existe atualmente. Quando atribui uma função a um utilizador, pode limitar ainda mais as ações permitidas por definir um âmbito. Isto é útil se pretender que alguém um contribuinte de Web site, mas apenas para um grupo de recursos.

> [!NOTE]
> As definições de função do Azure são constantemente evolução. Este artigo é mantido como atualizado quanto possível, mas pode localizar sempre as definições mais recentes de funções no Azure PowerShell. Utilize o [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) cmdlet para listar todas as funções atuais. Pode começar para uma função específica a utilizar `(get-azurermroledefinition "<role name>").actions` ou `(get-azurermroledefinition "<role name>").notactions` como aplicável. Utilize [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) para operações de lista de fornecedores de recursos do Azure específicos.


| Função incorporada | Descrição |
| --- | --- |
| [Proprietário](#owner) | Podem gerir tudo, incluindo o acesso |
| [Contribuinte](#contributor) | Pode gerir tudo, exceto acesso |
| [leitor](#reader) | Pode ver tudo, mas não é possível efetuar alterações |
| [Contribuinte de serviço de gestão de API](#api-management-service-contributor) | Pode gerir os serviços de gestão de API |
| [Função de operador de serviço de gestão de API](#api-management-service-operator-role) | Pode gerir os serviços de gestão de API |
| [Função de leitor de serviço de gestão de API](#api-management-service-reader-role) | Pode gerir os serviços de gestão de API |
| [Contribuinte de componentes do Application Insights](#application-insights-component-contributor) | Permite gerir componentes do Application Insights |
| [Depurador de instantâneo do Application Insights](#application-insights-snapshot-debugger) | Dá permissão ao utilizador para utilizar as funcionalidades do Depurador de Instantâneo do Application Insights |
| [Operador de tarefa de automatização](#automation-job-operator) | Criar e Gerir Tarefas através de Runbooks de Automatização. |
| [Operador de automatização](#automation-operator) | Conseguir iniciar, parar, suspender e retomar trabalhos |
| [Operador de Runbook de automatização](#automation-runbook-operator) | Ler as propriedades do Runbook para que possa criar Tarefas do runbook. |
| [Proprietário de registo de pilha do Azure](#azure-stack-registration-owner) | Permite-lhe gerir os registos do Azure Stack. |
| [Cópia de segurança contribuinte](#backup-contributor) | Pode gerir todas as ações de gestão de cópia de segurança, exceto criar Cofre de serviços de recuperação e conceder acesso a outras pessoas |
| [Operador de cópia de segurança](#backup-operator) | Pode gerir todas as ações de gestão de cópia de segurança, exceto a criação de cofres de cópia de segurança e fornecer acesso a outras pessoas a remover |
| [Leitor de cópia de segurança](#backup-reader) | Pode monitorizar a gestão de cópia de segurança no Cofre de serviços de recuperação |
| [Leitor de faturação](#billing-reader) | Pode ver todas as informações de faturação |
| [Contribuinte de BizTalk](#biztalk-contributor) | Pode gerir os BizTalk services |
| [Contribuinte de ponto final CDN](#cdn-endpoint-contributor) | Pode gerir pontos finais de CDN, as não pode conceder o acesso a outros utilizadores. |
| [Leitor de ponto final CDN](#cdn-endpoint-reader) | Pode visualizar os pontos finais de CDN, mas não pode efetuar alterações. |
| [Contribuinte de perfil CDN](#cdn-profile-contributor) | Pode gerir perfis de CDN e os respetivos pontos finais, mas não pode conceder o acesso a outros utilizadores. |
| [Leitor de perfil CDN](#cdn-profile-reader) | Pode visualizar perfis de CDN e os respetivos pontos finais, mas não pode efetuar alterações. |
| [Contribuidor de Rede Clássica](#classic-network-contributor) | Pode gerir redes virtuais clássicas e IPs reservados |
| [Contribuinte de conta de armazenamento clássico](#classic-storage-account-contributor) | Pode gerir contas de armazenamento clássicas |
| [Função de serviço de operador de chave de conta de armazenamento clássico](#classic-storage-account-key-operator-service-role) | Os Operadores de Chave da Conta de Armazenamento Clássica têm permissão para listar e regenerar chaves em Contas de Armazenamento Clássicas |
| [Contribuidor clássico Máquina Virtual](#classic-virtual-machine-contributor) | Pode gerir as máquinas virtuais clássicas, mas não o armazenamento de rede ou conta virtual ao qual estão ligados |
| [Contribuidor ClearDB MySQL DB](#cleardb-mysql-db-contributor) | Pode gerir bases de dados ClearDB MySQL |
| [Função de leitor de conta do cosmos DB](#cosmos-db-account-reader-role) | Pode ler os dados de conta de base de dados do Azure Cosmos. Consulte [contribuinte de conta do DocumentDB](#documentdb-account-contributor) para gerir contas de base de dados do Azure Cosmos. |
| [Contribuinte da fábrica de dados](#data-factory-contributor) | Criar e gerir as fábricas de dados e recursos subordinados dentro delas. |
| [Para programadores do Data Lake Analytics](#data-lake-analytics-developer) | Permite-lhe submeter, monitorizar e gerir as suas tarefas, mas não criar ou eliminar as contas do Data Lake Analytics. |
| [DevTest Labs utilizador](#devtest-labs-user) | Pode ver tudo e estabelecer ligação, início, reiniciar e encerrar as máquinas virtuais |
| [Contribuinte de zona DNS](#dns-zone-contributor) | Pode gerir zonas DNS e registos. |
| [Conta do DocumentDB contribuinte](#documentdb-account-contributor) | Pode gerir contas de base de dados do Azure Cosmos. BD do Azure do Cosmos anteriormente é conhecido como DocumentDB. |
| [Contribuinte de conta de sistemas inteligentes](#intelligent-systems-account-contributor) | Pode gerir contas de sistemas inteligentes |
| [Contribuidor do Cofre de chaves](#key-vault-contributor) | Permite-lhe gerir cofres de chaves, mas não o acesso aos mesmos. |
| [Criador de laboratório](#lab-creator) | Permite-lhe criar, gerir, elimine os laboratórios geridos na suas contas de laboratório do Azure. |
| [Contribuinte de análise do registo](#log-analytics-contributor) | Contribuinte de análise do registo pode ler todos os dados de monitorização e editar as definições de monitorização. Editar definições de monitorização inclui a adicionar a extensão VM para VMs; ler as chaves de conta de armazenamento para poder configurar a recolha de registos do armazenamento do Azure; criar e configurar as contas de automatização; Adicionar soluções; e configurar o diagnóstico do Azure em todos os recursos do Azure. |
| [Leitor do registo de análise](#log-analytics-reader) | O Leitor do Log Analytics pode visualizar e procurar todos os dados de monitorização assim como visualizar as definições de monitorização, incluindo a visualização da configuração de diagnósticos Azure em todos os recursos do Azure. |
| [Contribuinte de aplicação lógica](#logic-app-contributor) | Permite-lhe gerir aplicações lógicas, mas não aceder às mesmas. |
| [Operador de aplicação lógica](#logic-app-operator) | Permite-lhe ler, ativar e desativar a aplicação lógica. |
| [Identidade gerido contribuinte](#managed-identity-contributor) | Criar, ler, atualizar e eliminar atribuídos a identidade do utilizador |
| [Operador de identidade gerido](#managed-identity-operator) | Ler e atribua o utilizador atribuído identidade |
| [Monitorização contribuinte](#monitoring-contributor) | Pode ler todos os dados de monitorização e editar as definições de monitorização. Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Leitor de monitorização](#monitoring-reader) | Pode ler todos os dados de monitorização (métricas, os registos, etc.). Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Contribuidor de Rede](#network-contributor) | Pode gerir todos os recursos de rede |
| [Novo contribuinte de conta APM Relic](#new-relic-apm-account-contributor) | Permite-lhe gerir contas e aplicações do New Relic Application Performance Management, mas não aceder-lhes. |
| [Contribuinte de Cache de redis](#redis-cache-contributor) | Pode gerir caches de Redis |
| [Contribuinte de coleções de tarefa do agendador](#scheduler-job-collections-contributor) | Pode gerir coleções de tarefas do Programador |
| [Contribuinte de serviço de pesquisa](#search-service-contributor) | Pode gerir os serviços de pesquisa |
| [Administrador de segurança](#security-admin) | No Centro de segurança apenas: pode ver as políticas de segurança, ver Estados de segurança, editar as políticas de segurança, ver alertas e as recomendações, dispensar alertas e recomendações |
| [Gestor de segurança](#security-manager) | Pode gerir os componentes de segurança, as políticas de segurança e as máquinas virtuais |
| [Leitor de segurança](#security-reader) | No Centro de segurança apenas: pode ver as recomendações e alertas, vista de políticas de segurança, ver Estados de segurança, mas não é possível efetuar alterações |
| [Contribuinte de recuperação de site](#site-recovery-contributor) | Pode gerir todas as ações de gestão de recuperação de sites, exceto criar Cofre de serviços de recuperação e atribuição de direitos de acesso a outros utilizadores |
| [Operador de recuperação de site](#site-recovery-operator) | Pode ativação pós-falha e a reativação pós-falha, mas pode não efetuar outras ações de gestão de recuperação de sites ou atribuir acesso a outros utilizadores |
| [Leitor de recuperação de site](#site-recovery-reader) | Pode monitorizar o estado de recuperação de sites no Cofre de serviços de recuperação e emitir pedidos de suporte |
| [Contribuinte da BD SQL](#sql-db-contributor) | Pode gerir bases de dados SQL, mas não as respetivas políticas relacionadas com segurança |
| [Gestor de segurança do SQL Server](#sql-security-manager) | Pode gerir as políticas relacionadas com a segurança de bases de dados e servidores SQL |
| [SQL Server contribuinte](#sql-server-contributor) | Pode gerir servidores SQL e de bases de dados, mas não as respetivas políticas relacionadas com segurança |
| [Contribuinte de conta de armazenamento](#storage-account-contributor) | Pode gerir as contas de armazenamento, mas não aceder aos mesmos. |
| [Função de serviço de operador de chave de conta de armazenamento](#storage-account-key-operator-service-role) | Os Operadores de Chave da Conta de Armazenamento têm permissão para listar e regenerar chaves em Contas de Armazenamento |
| [Contribuinte de pedido de suporte](#support-request-contributor) | Pode criar e gerir pedidos de suporte no âmbito de subscrição |
| [Gestor de tráfego contribuinte](#traffic-manager-contributor) | Permite-lhe gerir perfis do Gestor de Tráfego, mas não lhe permite controlar quem tem acesso aos mesmos. |
| [Administrador de acesso de utilizador](#user-access-administrator) | Pode gerir o acesso de utilizador nos recursos do Azure |
| [Início de sessão de administrador de máquinas virtuais](#virtual-machine-administrator-login) | -Os utilizadores com esta função têm a capacidade de início de sessão a uma máquina virtual com o administrador do Windows ou Linux privilégios de utilizador de raiz. |
| [Contribuinte de máquina virtual](#virtual-machine-contributor) | Pode gerir máquinas virtuais, mas não o armazenamento de rede ou conta virtual ao qual estão ligados |
| [Início de sessão de utilizador de máquina virtual](#virtual-machine-user-login) | Os utilizadores com esta função têm a capacidade de início de sessão a uma máquina virtual como um utilizador normal. |
| [Plano de contribuinte da Web](#web-plan-contributor) | Pode gerir planos de web |
| [Contribuinte de Web site](#website-contributor) | Pode gerir sites, mas não os planos de web ao qual estão ligados |

As tabelas seguintes descrevem as permissões específicas para cada função. Isto pode incluir **ações**, que atribua permissões, e **NotActions**, que restringi-los.

## <a name="owner"></a>Proprietário
Podem gerir tudo, incluindo o acesso

| **Ações** |  |
| --- | --- |
| * | Criar e gerir recursos de todos os tipos |

## <a name="contributor"></a>Contribuinte
Pode gerir tudo, exceto acesso

| **Ações** |  |
| --- | --- |
| * | Criar e gerir recursos de todos os tipos |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete | Não é possível eliminar funções e atribuições de função |
| Microsoft.Authorization/*/Write | Não é possível criar funções e atribuições de função |
| Microsoft.Authorization/elevateAccess/Action | Garante ao chamador Administrador de Acesso dos Utilizadores acesso ao âmbito do inquilino |

## <a name="reader"></a>Leitor
Pode ver tudo, mas não é possível efetuar alterações

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |

## <a name="api-management-service-contributor"></a>Contribuinte de Serviços de Gestão de API
Pode gerir os serviços de gestão de API

| **Ações** |  |
| --- | --- |
| Microsoft.ApiManagement/service/* | Criar e gerir o serviço de API Management |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Funções de leitura e atribuições de função |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="api-management-service-operator-role"></a>Função de Operador de Serviço de Gestão de API
Pode gerir os serviços de gestão de API

| **Ações** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Instâncias de serviço de gestão de API de leitura |
| Microsoft.ApiManagement/service/backup/action | Fazer cópias de segurança do serviço de gestão de API para o contentor especificado um utilizador fornecida a conta de armazenamento |
| Microsoft.ApiManagement/service/delete | Eliminar uma instância de serviço de gestão de API |
| Microsoft.ApiManagement/service/managedeployments/action | SKU/unidades de alteração Adicionar ou remover regionais implementações do serviço de gestão de API |
| Microsoft.ApiManagement/service/read | Ler os metadados para uma instância de serviço de gestão de API |
| Microsoft.ApiManagement/service/restore/action | Restaurar o serviço de gestão de API a partir do contentor especificado um utilizador fornecida a conta de armazenamento |
| Microsoft.ApiManagement/service/updatecertificate/action | Carregar o certificado SSL para um serviço de gestão de API |
| Microsoft.ApiManagement/service/updatehostname/action | Configurar, atualizar ou remover nomes de domínio personalizado para um serviço de gestão de API |
| Microsoft.ApiManagement/service/write | Criar uma nova instância do serviço de gestão de API |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Funções de leitura e atribuições de função |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Obter a lista de chaves de utilizador |

## <a name="api-management-service-reader-role"></a>Função do Leitor do Serviço de Gestão de API do Serviço de Gestão de API
Pode gerir os serviços de gestão de API

| **Ações** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Instâncias de serviço de gestão de API de leitura |
| Microsoft.ApiManagement/service/read | Ler os metadados para uma instância de serviço de gestão de API |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Funções de leitura e atribuições de função |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Obter a lista de chaves de utilizador |

## <a name="application-insights-component-contributor"></a>Contribuinte de Componentes do Application Insights
Permite gerir componentes do Application Insights

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Insights/components/* | Criar e gerir Insights componentes |
| Microsoft.Insights/webtests/* | Criar e gerir os testes web |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="application-insights-snapshot-debugger"></a>Depurador de Instantâneo do Application Insights
Dá permissão ao utilizador para utilizar as funcionalidades do Depurador de Instantâneo do Application Insights

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Insights/components/*/read |  |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="automation-job-operator"></a>Operador de Tarefas de Automatização
Criar e Gerir Tarefas através de Runbooks de Automatização.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Automation/automationAccounts/jobs/read | Obtém uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Retoma uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Para uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lê recursos de trabalho de Runbook híbrida |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Obtém um fluxo de trabalho da automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspende uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/write | Cria uma tarefa de automatização do Azure |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="automation-operator"></a>Operador de Automatização
Conseguir iniciar, parar, suspender e retomar trabalhos

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Automation/automationAccounts/jobs/read | Ler tarefas da conta de automatização |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Retomar uma tarefa de conta de automatização |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Parar uma tarefa de conta de automatização |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Ler os fluxos de trabalho de conta de automatização |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspender uma tarefa de conta de automatização |
| Microsoft.Automation/automationAccounts/jobs/write | Escrever as tarefas de conta de automatização |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Ler um agendamento de trabalhos de conta de automatização |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Ler um agendamento de trabalhos de conta de automatização |
| Microsoft.Automation/automationAccounts/linkedWorkspace/read | Obtém a área de trabalho associada à conta de automatização |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lê recursos de trabalho de Runbook híbrida |
| Microsoft.Automation/automationAccounts/read | Contas de automatização de leitura |
| Microsoft.Automation/automationAccounts/runbooks/read | Runbooks de automatização de leitura |
| Microsoft.Automation/automationAccounts/schedules/read | Ler as agendas de conta de automatização |
| Microsoft.Automation/automationAccounts/schedules/write | Escrever as agendas de conta de automatização |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="automation-runbook-operator"></a>Operador de Runbook de Automatização
Ler as propriedades do Runbook para que possa criar Tarefas do runbook.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Automation/automationAccounts/runbooks/read | Obtém um runbook de automatização do Azure |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="azure-stack-registration-owner"></a>Proprietário do Registo do Azure Stack
Permite-lhe gerir os registos do Azure Stack.

| **Ações** |  |
| --- | --- |
| Microsoft.AzureStack/registrations/products/listDetails/action | Obtém expandido detalhes de um produto de pilha de Azure Marketplace |
| Microsoft.AzureStack/registrations/products/read | Obtém as propriedades de um produto de pilha de Azure Marketplace |
| Microsoft.AzureStack/registrations/read | Obtém as propriedades de um registo de pilha do Azure |

## <a name="backup-contributor"></a>Contribuidor de Cópia de Segurança
Pode gerir todas as ações de gestão de cópia de segurança, exceto criar Cofre de serviços de recuperação e conceder acesso a outras pessoas

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Network/virtualNetworks/read | Ler as redes virtuais |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Gerir os resultados da operação na gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Criar e gerir contentores de cópia de segurança no interior de recursos de infraestrutura de cópia de segurança do Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Criar e gerir tarefas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exportar as tarefas de cópia de segurança para um excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Criar e gerir os dados de metadados relacionados com a gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Criar e gerir resultados de operações de gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Criar e gerir políticas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Criar e gerir itens que podem ser uma cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Criar e gerir itens de cópias de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Criar e gerir contentores que contém os itens de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/certificates/* | Criar e gerir certificados relacionados com a cópia de segurança no Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Criar e gerir informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/read | Leia os cofres dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Gerir a operação de deteção para obtenção recém-criado contentores |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Criar e gerir identidades registadas |
| Microsoft.RecoveryServices/Vaults/usages/* | Criar e gerir a utilização do Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Storage/storageAccounts/read | Contas de armazenamento de leitura |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
| Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="backup-operator"></a>Operador de Cópia de Segurança
Pode gerir todas as ações de gestão de cópia de segurança, exceto a criação de cofres de cópia de segurança e fornecer acesso a outras pessoas a remover

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Network/virtualNetworks/read | Ler as redes virtuais |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Ler os resultados da operação na gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Resultados da operação de leitura em contentores de proteção |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/backup/action | Efetuar a operação de cópia de segurança a pedido num item de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Resultado de leitura da operação efetuada numa cópia de segurança item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Devolve o estado da Operação efetuada nos Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Ler uma cópia de segurança itens |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Ponto de recuperação de leitura de um item de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/restore/action | Efetuar uma operação de restauro utilizando um ponto de recuperação de um item de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Criar um item de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Leu os contentores que contém o item de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Criar e gerir tarefas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Cancelar a tarefa |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Devolve o Resultado da Operação de Tarefa. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Devolve todos os objetos da tarefa |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exportar as tarefas de cópia de segurança para um excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Ler dados de metadados relacionados com a gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Criar e gerir resultados de operações de gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Resultados de leitura das operações executadas em políticas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Políticas de cópia de leitura |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Criar e gerir itens que podem ser uma cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Devolve a lista de todos os Itens Susceptíveis de Proteção. |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Ler uma cópia de segurança itens |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Ler uma cópia de segurança contentores que contém os itens de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Ler informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Escrever informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/read | Leia os cofres dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Gerir a operação de deteção para obtenção recém-criado contentores |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Resultados de leitura da operação efetuadas em itens de registada do Cofre de |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Ler itens registados do Cofre de |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Escrever itens registados para o Cofre |
| Microsoft.RecoveryServices/Vaults/usages/read | Utilização de leitura do cofre dos serviços de recuperação |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Storage/storageAccounts/read | Contas de armazenamento de leitura |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Aprovisionar Item instantâneas recuperação para o Item protegido |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Revogar a recuperação do Item instantâneas para Item protegido |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
| Microsoft.RecoveryServices/Vaults/certificates/write | A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="backup-reader"></a>Leitor de Cópia de Segurança
Pode monitorizar a gestão de cópia de segurança no Cofre de serviços de recuperação

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Ler os resultados da operação na gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Resultados da operação de leitura em contentores de proteção |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Resultado de leitura da operação efetuada numa cópia de segurança item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Devolve o estado da Operação efetuada nos Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Ler uma cópia de segurança itens |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Leu os contentores que contém o item de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Ler os resultados das tarefas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Tarefas de cópia de segurança de leitura |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exportar as tarefas de cópia de segurança para um excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Ler dados de metadados relacionados com a gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Ler os resultados da operação de gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Resultados de leitura das operações executadas em políticas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Políticas de cópia de leitura |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Ler uma cópia de segurança itens |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Ler uma cópia de segurança contentores que contém os itens de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Ler informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/read | Leia os cofres dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Resultado da operação de deteção para obtenção de recém-criado contentores de leitura |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Resultados de leitura da operação efetuadas em itens de registada do Cofre de |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Ler itens registados do Cofre de |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Obter Pontos de Recuperação de Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
| Microsoft.RecoveryServices/Vaults/usages/read | Utilização de leitura do cofre dos serviços de recuperação |

## <a name="billing-reader"></a>Leitor de Faturação
Pode ver todas as informações de faturação

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Billing/*/read | Ler as informações de faturação |
| Microsoft.Consumption/*/read |  |
| Microsoft.Commerce/*/read |  |
| Microsoft.Management/managementGroups/read | Liste os grupos de gestão para o utilizador autenticado. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="biztalk-contributor"></a>Contribuinte do BizTalk
Pode gerir os BizTalk services

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.BizTalkServices/BizTalk/* | Criar e gerir serviços BizTalk |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-endpoint-contributor"></a>Contribuidor de Ponto Final de CDN
Pode gerir pontos finais de CDN, as não pode conceder o acesso a outros utilizadores.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/* |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-endpoint-reader"></a>Leitor do Ponto Final de CDN
Pode visualizar os pontos finais de CDN, mas não pode efetuar alterações.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/*/read |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-profile-contributor"></a>Contribuidor de Perfil de CDN
Pode gerir perfis de CDN e os respetivos pontos finais, mas não pode conceder o acesso a outros utilizadores.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/* |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-profile-reader"></a>Leitor de Perfil de CDN
Pode visualizar perfis de CDN e os respetivos pontos finais, mas não pode efetuar alterações.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/*/read |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-network-contributor"></a>Contribuinte de Rede Clássica
Pode gerir redes virtuais clássicas e IPs reservados

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.ClassicNetwork/* | Criar e gerir redes clássicos |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-storage-account-contributor"></a>Contribuinte de Conta de Armazenamento Clássica
Pode gerir contas de armazenamento clássicas

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.ClassicStorage/storageAccounts/* | Criar e gerir contas de armazenamento |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-storage-account-key-operator-service-role"></a>Função do Serviço de Operador de Chave da Conta de Armazenamento Clássica
Os Operadores de Chave da Conta de Armazenamento Clássica têm permissão para listar e regenerar chaves em Contas de Armazenamento Clássicas

| **Ações** |  |
| --- | --- |
| Microsoft.ClassicStorage/storageAccounts/listkeys/action | Lista as chaves de acesso para as contas de armazenamento. |
| Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Volta a gerar as chaves de acesso existentes para a conta de armazenamento. |

## <a name="classic-virtual-machine-contributor"></a>Contribuinte de Máquina Virtual Clássica
Pode gerir as máquinas virtuais clássicas, mas não o armazenamento de rede ou conta virtual ao qual estão ligados

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.ClassicCompute/domainNames/* | Criar e gerir nomes de domínio de computação clássicos |
| Microsoft.ClassicCompute/virtualMachines/* | Criar e gerir máquinas virtuais |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Associar grupos de segurança de rede |
| Microsoft.ClassicNetwork/reservedIps/link/action | Ligação IPs reservados |
| Microsoft.ClassicNetwork/reservedIps/read | Leitura endereços IP reservados |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Associar redes virtuais |
| Microsoft.ClassicNetwork/virtualNetworks/read | Ler as redes virtuais |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Leitura de discos de conta de armazenamento |
| Microsoft.ClassicStorage/storageAccounts/images/read | Ler as imagens de conta de armazenamento |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista de chaves de conta de armazenamento |
| Microsoft.ClassicStorage/storageAccounts/read | Contas de armazenamento clássicas de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cleardb-mysql-db-contributor"></a>Contribuinte de Base de Dados MySQL ClearDB
Pode gerir bases de dados ClearDB MySQL

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| successbricks.cleardb/databases/* | Criar e gerir bases de dados ClearDB MySQL |

## <a name="cosmos-db-account-reader-role"></a>Função do Leitor da Conta do Cosmos DB
Pode ler os dados de conta de base de dados do Azure Cosmos. Consulte [contribuinte de conta do DocumentDB](#documentdb-account-contributor) para gerir contas de base de dados do Azure Cosmos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de funções, pode permissões de leitura para cada utilizador |
| Microsoft.DocumentDB/*/read | Uma coleção de leitura |
| Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | O painel de chaves de só de leitura de leitura |
| Microsoft.Insights/MetricDefinitions/read | Definições de métrica de leitura |
| Microsoft.Insights/Metrics/read | Ler métricas de conta |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="data-factory-contributor"></a>Contribuinte do Data Factory
Criar e gerir as fábricas de dados e recursos subordinados dentro delas.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.DataFactory/dataFactories/* | Criar e gerir as fábricas de dados e recursos subordinados dentro delas. |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="data-lake-analytics-developer"></a>Programador do Data Lake Analytics
Permite-lhe submeter, monitorizar e gerir as suas tarefas, mas não criar ou eliminar as contas do Data Lake Analytics.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.BigAnalytics/accounts/* |  |
| Microsoft.DataLakeAnalytics/accounts/* |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.BigAnalytics/accounts/Delete |  |
| Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
| Microsoft.BigAnalytics/accounts/Write |  |
| Microsoft.DataLakeAnalytics/accounts/Delete | Elimine uma conta de DataLakeAnalytics. |
| Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Conceder permissões para cancelar as tarefas submetidas por outros utilizadores. |
| Microsoft.DataLakeAnalytics/accounts/Write | Criar ou atualizar uma conta de DataLakeAnalytics. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Criar ou atualizar uma conta de DataLakeStore ligada de uma conta de DataLakeAnalytics. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Desassociar uma conta de DataLakeStore de uma conta de DataLakeAnalytics. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Criar ou atualizar uma conta de armazenamento ligada de uma conta de DataLakeAnalytics. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Desassociar uma conta de armazenamento de uma conta de DataLakeAnalytics. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Criar ou atualizar uma regra de firewall. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Elimine uma regra de firewall. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Criar ou atualizar uma política de computação. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Elimine uma política de computação. |

## <a name="devtest-labs-user"></a>Utilizador do DevTest Labs
Pode ver tudo e estabelecer ligação, início, reiniciar e encerrar as máquinas virtuais

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Compute/availabilitySets/read | Ler as propriedades de conjuntos de disponibilidade |
| Microsoft.Compute/virtualMachines/*/read | Ler as propriedades de uma máquina virtual (tamanhos de VM, estado de runtime, extensões VM, etc.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Anular a atribuição de máquinas virtuais |
| Microsoft.Compute/virtualMachines/read | Ler as propriedades de uma máquina virtual |
| Microsoft.Compute/virtualMachines/restart/action | Reiniciar máquinas virtuais |
| Microsoft.Compute/virtualMachines/start/action | Iniciar máquinas virtuais |
| Microsoft.DevTestLab/*/read | Ler as propriedades de um laboratório |
| Microsoft.DevTestLab/labs/createEnvironment/action | Criar um ambiente de laboratório |
| Microsoft.DevTestLab/labs/claimAnyVm/action | Uma máquina virtual claimable aleatória no laboratório de afirmações. |
| Microsoft.DevTestLab/labs/formulas/delete | Eliminar fórmulas |
| Microsoft.DevTestLab/labs/formulas/read | Fórmulas de leitura |
| Microsoft.DevTestLab/labs/formulas/write | Adiciona ou modifica as fórmulas |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Avaliar as políticas de laboratório |
| Microsoft.DevTestLab/labs/virtualMachines/claim/action | Assumir a propriedade de uma máquina virtual existente |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Associar um conjunto de endereços de back-end de Balanceador de carga |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Aderir a um balanceador de carga regra NAT de entrada |
| Microsoft.Network/networkInterfaces/*/read | Ler as propriedades de uma interface de rede (por exemplo, todos os balanceadores de carga que a interface de rede faz parte de) |
| Microsoft.Network/networkInterfaces/join/action | Associar uma Máquina Virtual a uma interface de rede |
| Microsoft.Network/networkInterfaces/read | Interfaces de rede de leitura |
| Microsoft.Network/networkInterfaces/write | Escrever interfaces de rede |
| Microsoft.Network/publicIPAddresses/*/read | Ler as propriedades de um endereço IP público |
| Microsoft.Network/publicIPAddresses/join/action | Aderir a um endereço IP público |
| Microsoft.Network/publicIPAddresses/read | Leitura rede os endereços IP públicos |
| Microsoft.Network/virtualNetworks/subnets/join/action | Associar uma rede virtual |
| Microsoft.Resources/deployments/operations/read | Operações de implementação de leitura |
| Microsoft.Resources/deployments/read | Implementações de leitura |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Storage/storageAccounts/listKeys/action | Lista de chaves de conta de armazenamento |

| **NotActions** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/vmSizes/read | Lista os tamanhos disponíveis para os quais a máquina virtual pode ser atualizada |

## <a name="dns-zone-contributor"></a>Contribuidor da Zona DNS
Pode gerir zonas DNS e registos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/dnsZones/* | Criar e gerir registos e zonas DNS |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="documentdb-account-contributor"></a>Contribuidor de Conta do DocumentDB
Pode gerir contas de base de dados do Azure Cosmos. BD do Azure do Cosmos anteriormente é conhecido como DocumentDB.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.DocumentDb/databaseAccounts/* | Criar e gerir contas de base de dados do Azure Cosmos |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="intelligent-systems-account-contributor"></a>Contribuinte de Conta de Sistemas Inteligentes
Pode gerir contas de sistemas inteligentes

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.IntelligentSystems/accounts/* | Criar e gerir contas de sistemas inteligentes |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="key-vault-contributor"></a>Contribuidor do Key Vault
Permite-lhe gerir cofres de chaves, mas não o acesso aos mesmos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.KeyVault/* |  |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.KeyVault/locations/deletedVaults/purge/action | Remover um cofre de chaves eliminado de forma recuperável |
| Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>Criador de laboratório
Permite-lhe criar, gerir, elimine os laboratórios geridos na suas contas de laboratório do Azure.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.ManagedLab/labAccounts/createLab/action | Crie um laboratório numa conta de laboratório. |
| Microsoft.ManagedLab/labAccounts/*/read |  |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="log-analytics-contributor"></a>Contribuidor do Log Analytics
Contribuinte de análise do registo pode ler todos os dados de monitorização e editar as definições de monitorização. Editar definições de monitorização inclui a adicionar a extensão VM para VMs; ler as chaves de conta de armazenamento para poder configurar a recolha de registos do armazenamento do Azure; criar e configurar as contas de automatização; Adicionar soluções; e configurar o diagnóstico do Azure em todos os recursos do Azure.

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
| Microsoft.Automation/automationAccounts/* |  |
| Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista as chaves de acesso para as contas de armazenamento. |
| Microsoft.Compute/virtualMachines/extensions/* |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Insights/diagnosticSettings/* | Cria, atualiza ou lê a definição de diagnóstico para o servidor de Analysis Services |
| Microsoft.OperationalInsights/* |  |
| Microsoft.OperationsManagement/* |  |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
| Microsoft.Storage/storageAccounts/listKeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="log-analytics-reader"></a>Leitor do Log Analytics
O Leitor do Log Analytics pode visualizar e procurar todos os dados de monitorização assim como visualizar as definições de monitorização, incluindo a visualização da configuração de diagnósticos Azure em todos os recursos do Azure.

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
| Microsoft.OperationalInsights/workspaces/analytics/query/action | A procura utilizando o motor de novo. |
| Microsoft.OperationalInsights/workspaces/search/action | Executa uma consulta de pesquisa |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.OperationalInsights/workspaces/sharedKeys/read | Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |

## <a name="logic-app-contributor"></a>Contribuidor da Aplicação Lógica
Permite-lhe gerir aplicações lógicas, mas não aceder às mesmas.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista as chaves de acesso para as contas de armazenamento. |
| Microsoft.ClassicStorage/storageAccounts/read | Devolve a conta de armazenamento com a conta fornecida. |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Insights/diagnosticSettings/* | Cria, atualiza ou lê a definição de diagnóstico para o servidor de Analysis Services |
| Microsoft.Insights/logdefinitions/* | Esta permissão é necessária para os utilizadores que necessitam de aceder a registos de atividade através do portal. Lista categorias de registo no registo de atividade. |
| Microsoft.Insights/metricDefinitions/* | Ler as definições de métrica (lista de tipos de métricas disponíveis para um recurso). |
| Microsoft.Logic/* | Gere os recursos de Logic Apps. |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/operationresults/read | Obter os resultados da operação de subscrição. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/listkeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
| Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.Web/connectionGateways/* | Criar e gere um Gateway de ligação. |
| Microsoft.Web/connections/* | Criar e gere uma ligação. |
| Microsoft.Web/customApis/* | Cria e gere uma API personalizada. |
| Microsoft.Web/serverFarms/join/action |  |
| Microsoft.Web/serverFarms/read | Obter as propriedades num plano do App Service |
| Microsoft.Web/sites/functions/listSecrets/action | Lista os segredos Web Apps funções. |

## <a name="logic-app-operator"></a>Operador de Aplicação Lógica
Permite-lhe ler, ativar e desativar a aplicação lógica.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/*/read | Regras de alertas das informações de leitura |
| Microsoft.Insights/diagnosticSettings/*/read | Obtém as definições de diagnóstico para Logic Apps |
| Microsoft.Insights/metricDefinitions/*/read | Obtem as métricas disponíveis para aplicações lógicas. |
| Microsoft.Logic/*/read | Lê recursos Logic Apps. |
| Microsoft.Logic/workflows/disable/action | Desativa o fluxo de trabalho. |
| Microsoft.Logic/workflows/enable/action | Ativa o fluxo de trabalho. |
| Microsoft.Logic/workflows/validate/action | Valida o fluxo de trabalho. |
| Microsoft.Resources/deployments/operations/read | Obtém ou lista as operações de implementação. |
| Microsoft.Resources/subscriptions/operationresults/read | Obter os resultados da operação de subscrição. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.Web/connectionGateways/*/read | Ler ligação Gateways. |
| Microsoft.Web/connections/*/read | Ligações de leitura. |
| Microsoft.Web/customApis/*/read | API personalizada de leitura. |
| Microsoft.Web/serverFarms/read | Obter as propriedades num plano do App Service |

## <a name="managed-identity-contributor"></a>Identidade gerido contribuinte
Criar, ler, atualizar e eliminar atribuídos a identidade do utilizador

| **Ações** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="managed-identity-operator"></a>Operador de identidade gerido
Ler e atribua o utilizador atribuído identidade

| **Ações** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="monitoring-contributor"></a>Contribuidor de Monitorização
Pode ler todos os dados de monitorização e editar as definições de monitorização. Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
| Microsoft.AlertsManagement/alerts/* |  |
| Microsoft.AlertsManagement/alertsSummary/* |  |
| Microsoft.Insights/AlertRules/* | Regras de alertas de leitura/escrita/eliminar. |
| Microsoft.Insights/components/* | Componentes do Application Insights de leitura/escrita/eliminar. |
| Microsoft.Insights/DiagnosticSettings/* | Definições de diagnóstico de leitura/escrita/eliminar. |
| Microsoft.Insights/eventtypes/* | Lista de eventos de registo de atividade (eventos de gestão) numa subscrição. Esta permissão é aplicável ao acesso programático e portal de registo de atividade. |
| Microsoft.Insights/LogDefinitions/* | Esta permissão é necessária para os utilizadores que necessitam de aceder a registos de atividade através do portal. Lista categorias de registo no registo de atividade. |
| Microsoft.Insights/MetricDefinitions/* | Ler as definições de métrica (lista de tipos de métricas disponíveis para um recurso). |
| Microsoft.Insights/Metrics/* | Ler métricas para um recurso. |
| Microsoft.Insights/Register/Action | Registe o fornecedor de insights. |
| Microsoft.Insights/webtests/* | Testes web do Application Insights de leitura/escrita/eliminar. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Pacotes de solução de análise de registos de leitura/escrita/eliminar. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* | Análise de registos de leitura/escrita/eliminar pesquisas guardadas. |
| Microsoft.OperationalInsights/workspaces/search/action | Pesquise áreas de trabalho de análise de registos. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action | Listar chaves para uma área de trabalho de análise de registos. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Configurações de conhecimentos aprofundados de armazenamento de análise de registos de leitura/escrita/eliminar. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>Leitor de Monitorização
Pode ler todos os dados de monitorização (métricas, os registos, etc.). Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
| Microsoft.OperationalInsights/workspaces/search/action | Dados de análise de registos de pesquisa |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="network-contributor"></a>Contribuinte de Rede
Pode gerir todos os recursos de rede

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/* | Criar e gerir redes |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="new-relic-apm-account-contributor"></a>Contribuidor de Conta APM do New Relic
Permite-lhe gerir contas e aplicações do New Relic Application Performance Management, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| NewRelic.APM/accounts/* |  |

## <a name="redis-cache-contributor"></a>Contribuinte de Cache de Redis
Pode gerir caches de Redis

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Cache/redis/* | Criar e gerir caches de Redis |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="scheduler-job-collections-contributor"></a>Contribuinte de Coleções de Tarefas do Agendador
Pode gerir coleções de tarefas do Programador

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Scheduler/jobcollections/* | Criar e gerir coleções de tarefas |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="search-service-contributor"></a>Contribuinte de Serviços de Pesquisa
Pode gerir os serviços de pesquisa

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Search/searchServices/* | Criar e gerir serviços de pesquisa |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="security-admin"></a>Administrador de Segurança
No Centro de segurança apenas: pode ver as políticas de segurança, ver Estados de segurança, editar as políticas de segurança, ver alertas e as recomendações, dispensar alertas e recomendações

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Authorization/policyAssignments/* | Criar e gerir atribuições de políticas |
| Microsoft.Authorization/policyDefinitions/* | Criar e gerir definições de política |
| Microsoft.Authorization/policySetDefinitions/* | Criar e gerir conjuntos de política |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.operationalInsights/workspaces/*/read | Ver os dados de análise de registos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Security/*/read | Componentes de segurança de leitura e políticas |
| Microsoft.Security/locations/alerts/dismiss/action | Dispensar um alerta de segurança |
| Microsoft.Security/locations/tasks/dismiss/action | Ignorar uma recomendação de segurança |
| Microsoft.Security/policies/write | Atualiza a política de segurança |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="security-manager"></a>Gestor de Segurança
Pode gerir os componentes de segurança, as políticas de segurança e as máquinas virtuais

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.ClassicCompute/*/read | Ler as informações de configuração as máquinas virtuais clássicas |
| Microsoft.ClassicCompute/virtualMachines/*/write | Escrita de configuração para as máquinas virtuais clássicas |
| Microsoft.ClassicNetwork/*/read | Ler as informações de configuração sobre a rede clássico |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Security/* | Criar e gerir os componentes de segurança e políticas |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="security-reader"></a>Leitor de Segurança
No Centro de segurança apenas: pode ver as recomendações e alertas, vista de políticas de segurança, ver Estados de segurança, mas não é possível efetuar alterações

| **Ações** |  |
| --- | --- |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.operationalInsights/workspaces/*/read | Ver os dados de análise de registos |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Security/*/read | Componentes de segurança de leitura e políticas |

## <a name="site-recovery-contributor"></a>Contribuidor do Site Recovery
Pode gerir todas as ações de gestão de recuperação de sites, exceto criar Cofre de serviços de recuperação e atribuição de direitos de acesso a outros utilizadores

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/virtualNetworks/read | Ler as redes virtuais |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/certificates/write | O certificado de credenciais do Cofre de atualizações |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Criar e gerir informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/read | Os cofres dos serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Gerir a operação de deteção para obtenção recém-criado contentores |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Criar e gerir identidades registadas |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Criar ou atualizar definições de alerta de replicação |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler eventos de replicação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Criar e gerir recursos de infraestrutura de replicação |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Criar e gerir tarefas de replicação |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Criar e gerir políticas de replicação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Criar e gerir planos de recuperação |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Criar e gerir a configuração de armazenamento do Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Informações sobre o token de cofre dos serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/usages/read | Lê os detalhes de utilização de um cofre dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Alertas de leitura para o Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read | Configuração de notificação do Cofre de serviços de recuperação de leitura |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Storage/storageAccounts/read | Contas de armazenamento de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="site-recovery-operator"></a>Operador do Site Recovery
Pode ativação pós-falha e a reativação pós-falha, mas pode não efetuar outras ações de gestão de recuperação de sites ou atribuir acesso a outros utilizadores

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/virtualNetworks/read | Ler as redes virtuais |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Ler informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/read | Os cofres dos serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Gerir a operação de deteção para obtenção recém-criado contentores |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Ler o estado da operação e o resultado de uma operação submetido |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Contentores de leitura registados para um recurso |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Ler definições de alerta de replicação |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler eventos de replicação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Verificação de consistência de recursos de infraestrutura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Recursos de infraestrutura de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Voltar a associar o gateway de replicação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Renovar certificado de recursos de infraestrutura de replicação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Ler replicação redes de infraestrutura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Mapeamento de rede de recursos de infraestrutura de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Leu os contentores de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Obter a lista de todos os itens passíveis de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Aplicar o ponto de recuperação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Consolidação de ativação pós-falha |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Ativação pós-falha planeada |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Obter a lista de todos os itens protegidos |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Obter a lista de pontos de recuperação disponíveis |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Reparar a replicação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Começar a proteger novamente para um item protegido |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/ação | Iniciar ativação pós-falha de teste de um item protegido |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Limpeza da ativação pós-falha de teste |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Ativação pós-falha |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Serviço de mobilidade de atualização |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Leia os mapeamentos de contentor de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Fornecedores de serviços de recuperação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider/ação | Atualize o fornecedor de serviços de recuperação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Classificações de armazenamento para recursos de infraestrutura de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Mapeamentos de classificação de armazenamento de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Leitura registado informações de vCenter |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Criar e gerir tarefas de replicação |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Políticas de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Consolidar ativação pós-falha para ativação pós-falha do plano de recuperação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Iniciar ativação pós-falha de um plano de recuperação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Planos de recuperação de leitura |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Iniciar a voltar a proteger de um plano de recuperação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Iniciar ativação pós-falha de teste de um plano de recuperação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Inicie a limpeza da ativação pós-falha de teste do plano recuperação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Iniciar ativação pós-falha não planeada de um plano de recuperação |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Alertas de leitura para o Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read | Configuração de notificação do Cofre de serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Ler a configuração de armazenamento de um cofre dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Informações sobre o token de cofre dos serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/usages/read | Lê os detalhes de utilização de um cofre dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Storage/storageAccounts/read | Contas de armazenamento de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="site-recovery-reader"></a>Leitor do Site Recovery
Pode monitorizar o estado de recuperação de sites no Cofre de serviços de recuperação e emitir pedidos de suporte

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Ler informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Alertas de leitura para o Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read | Configuração de notificação do Cofre de serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/read | Os cofres dos serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Gerir a operação de deteção para obtenção recém-criado contentores |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Ler o estado da operação e o resultado de uma operação submetido |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Contentores de leitura registados para um recurso |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Ler definições de alerta de replicação |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler eventos de replicação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Recursos de infraestrutura de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Ler replicação redes de infraestrutura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Mapeamento de rede de recursos de infraestrutura de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Leu os contentores de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Obter a lista de todos os itens passíveis de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Obter a lista de todos os itens protegidos |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Obter a lista de pontos de recuperação disponíveis |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Leia os mapeamentos de contentor de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Fornecedores de serviços de recuperação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Classificações de armazenamento para recursos de infraestrutura de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Mapeamentos de classificação de armazenamento de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Leitura registado informações de vCenter |
| Microsoft.RecoveryServices/vaults/replicationJobs/read | Ler o estado das tarefas de replicação |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Políticas de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Planos de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Ler a configuração de armazenamento de um cofre dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Informações sobre o token de cofre dos serviços de recuperação de leitura |
| Microsoft.RecoveryServices/Vaults/usages/read | Lê os detalhes de utilização de um cofre dos serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="sql-db-contributor"></a>Contribuinte de Base de Dados SQL
Pode gerir bases de dados SQL, mas não as respetivas políticas relacionadas com segurança

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/databases/* | Criar e gerir bases de dados SQL |
| Microsoft.Sql/servers/read | Servidores SQL de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Não é possível editar as políticas de auditoria |
| Microsoft.Sql/servers/databases/auditingSettings/* | Não é possível editar as definições de auditoria |
| Microsoft.Sql/servers/databases/auditRecords/read | Não é possível ler os registos de auditoria |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Não é possível editar as políticas de ligação |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Não é possível editar as políticas de máscara de dados |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Não é possível editar as políticas de alerta de segurança |
| Microsoft.Sql/servers/databases/securityMetrics/* | Não é possível editar as métricas de segurança |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>Gestor de Segurança SQL
Pode gerir as políticas relacionadas com a segurança de bases de dados e servidores SQL

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura Microsoft |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Sql/servers/auditingPolicies/* | Criar e gerir políticas de auditoria do SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Criar e gerir a definição de auditoria do servidor SQL |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Criar e gerir políticas de auditoria de base de dados do SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Criar e gerir definições de auditoria de base de dados do SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read | Registos de auditoria de leitura |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Criar e gerir políticas de ligação de base de dados do SQL server |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Criar e gerir políticas de máscara de dados do SQL server da base de dados |
| Microsoft.Sql/servers/databases/read | Bases de dados SQL de leitura |
| Microsoft.Sql/servers/databases/schemas/read | Esquemas de base de dados do servidor de SQL de leitura |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Colunas de tabela de base de dados de servidor de SQL de leitura |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/read | Tabelas de base de dados do servidor de SQL de leitura |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Criar e gerir políticas alertas de segurança da base de dados de servidor SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* | Criar e gerir as métricas de segurança de base de dados do SQL server |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/firewallRules/* |  |
| Microsoft.Sql/servers/read | Servidores SQL de leitura |
| Microsoft.Sql/servers/securityAlertPolicies/* | Criar e gerir políticas de alerta de segurança do SQL server |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="sql-server-contributor"></a>Contribuinte do SQL Server
Pode gerir servidores SQL e de bases de dados, mas não as respetivas políticas relacionadas com segurança

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/* | Criar e gerir servidores SQL |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* | Não é possível editar as políticas de auditoria do SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Não é possível editar definições de auditoria do SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Não é possível editar as políticas de auditoria de base de dados do SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Não é possível editar definições de auditoria de base de dados do SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read | Não é possível ler os registos de auditoria |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Não é possível editar as políticas de ligação de base de dados do SQL server |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Não é possível editar as políticas de máscara de dados do SQL server da base de dados |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Não é possível editar o SQL server da base de dados alerta as políticas de segurança |
| Microsoft.Sql/servers/databases/securityMetrics/* | Não é possível editar as métricas de segurança de base de dados do SQL server |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/securityAlertPolicies/* | Não é possível editar as políticas de alerta de segurança do SQL server |

## <a name="storage-account-contributor"></a>Contribuinte de Conta de Armazenamento
Pode gerir as contas de armazenamento, mas não aceder aos mesmos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Ler todas as autorização |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Insights/diagnosticSettings/* | Gerir definições de diagnóstico |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Storage/storageAccounts/* | Criar e gerir contas de armazenamento |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="storage-account-key-operator-service-role"></a>Função do Serviço de Operador de Chave da Conta de Armazenamento
Os Operadores de Chave da Conta de Armazenamento têm permissão para listar e regenerar chaves em Contas de Armazenamento

| **Ações** |  |
| --- | --- |
| Microsoft.Storage/storageAccounts/listkeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
| Microsoft.Storage/storageAccounts/regeneratekey/action | Regenera as chaves de acesso da conta de armazenamento especificada. |

## <a name="support-request-contributor"></a>Contribuidor de Pedido de Suporte
Pode criar e gerir pedidos de suporte no âmbito de subscrição

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Resources/subscriptions/resourceGroups/read | Funções de leitura e atribuições de função |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="traffic-manager-contributor"></a>Contribuidor do Gestor de Tráfego
Permite-lhe gerir perfis do Gestor de Tráfego, mas não lhe permite controlar quem tem acesso aos mesmos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Network/trafficManagerProfiles/* |  |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="user-access-administrator"></a>Administrador de Acesso de Utilizador
Pode gerir o acesso de utilizador nos recursos do Azure

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
| Microsoft.Authorization/* | Gerir autorização |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="virtual-machine-administrator-login"></a>Início de sessão de administrador de máquinas virtuais
-   Os utilizadores com esta função têm a capacidade de início de sessão a uma máquina virtual com o administrador do Windows ou Linux privilégios de utilizador de raiz.

| **Ações** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="virtual-machine-contributor"></a>Contribuinte de Máquina Virtual
Pode gerir máquinas virtuais, mas não o armazenamento de rede ou conta virtual ao qual estão ligados

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Compute/availabilitySets/* | Criar e gerir conjuntos de disponibilidade de computação |
| Microsoft.Compute/locations/* | Criar e gerir as localizações de computação |
| Microsoft.Compute/virtualMachines/* | Criar e gerir máquinas virtuais |
| Microsoft.Compute/virtualMachineScaleSets/* | Criar e gerir conjuntos de dimensionamento de máquina virtual |
| Microsoft.DevTestLab/schedules/* |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Associar conjuntos de endereços de back-end de gateway de aplicação de rede |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Associar conjuntos de endereços de back-end de Balanceador de carga |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Associar o Balanceador de carga conjuntos NAT de entrada |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Associar o Balanceador de carga regras NAT de entrada |
| Microsoft.Network/loadBalancers/probes/join/action | Permite utilizar as pesquisas de um balanceador de carga. Por exemplo, com esta propriedade de healthProbe de permissão de dimensionamento VM conjunto pode referenciar a pesquisa. |
| Microsoft.Network/loadBalancers/read | Balanceadores de carga de leitura |
| Microsoft.Network/locations/* | Criar e gerir as localizações de rede |
| Microsoft.Network/networkInterfaces/* | Criar e gerir as interfaces de rede |
| Microsoft.Network/networkSecurityGroups/join/action | Associar grupos de segurança de rede |
| Microsoft.Network/networkSecurityGroups/read | Leia os grupos de segurança de rede |
| Microsoft.Network/publicIPAddresses/join/action | Associar endereços IP públicos de rede |
| Microsoft.Network/publicIPAddresses/read | Leitura rede os endereços IP públicos |
| Microsoft.Network/virtualNetworks/read | Ler as redes virtuais |
| Microsoft.Network/virtualNetworks/subnets/join/action | Associe sub-redes da rede virtual |
| Microsoft.RecoveryServices/locations/* |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/*/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Devolve detalhes do objecto do Item protegido |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Criar um Item protegido cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Criar uma cópia de segurança intenção de proteção |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devolve todas as políticas de proteção |
| Microsoft.RecoveryServices/Vaults/backupPolicies/write | Cria a política de proteção |
| Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
| Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
| Microsoft.RecoveryServices/Vaults/write | A operação Criar Cofre cria um recurso do tipo "cofre" do Azure |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Storage/storageAccounts/listKeys/action | Lista de chaves de conta de armazenamento |
| Microsoft.Storage/storageAccounts/read | Contas de armazenamento de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="virtual-machine-user-login"></a>Início de sessão de utilizador de máquina virtual
Os utilizadores com esta função têm a capacidade de início de sessão a uma máquina virtual como um utilizador normal.

| **Ações** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="web-plan-contributor"></a>Contribuinte de Plano Web
Pode gerir planos de web

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.Web/serverFarms/* | Criar e gerir farms de servidores |

## <a name="website-contributor"></a>Contribuinte de Web site
Pode gerir sites, mas não os planos de web ao qual estão ligados

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Insights/components/* | Criar e gerir Insights componentes |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ler o estado de funcionamento dos recursos |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupos de recursos de leitura |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.Web/certificates/* | Criar e gerir certificados de Web site |
| Microsoft.Web/listSitesAssignedToHostName/read | Sites de leitura atribuídos a um nome de anfitrião |
| Microsoft.Web/serverFarms/join/action | Associar farms de servidores |
| Microsoft.Web/serverFarms/read | Ler farms de servidores |
| Microsoft.Web/sites/* | Criar e gerir sites (criação de site também necessita de permissões de escrita para o plano de serviço de aplicação associado) |

## <a name="see-also"></a>Consulte também
* [Controlo de acesso baseado em funções](role-based-access-control-configure.md): começar a utilizar o RBAC no portal do Azure.
* [Funções personalizadas no Azure RBAC](role-based-access-control-custom-roles.md): Saiba como criar funções personalizadas para se ajustarem às suas necessidades de acesso.
* [Criar um relatório de histórico de alterações de acesso](role-based-access-control-access-change-history-report.md): manter um registo dos alteração atribuições de funções no RBAC.
* [Resolução de problemas de controlo de acesso baseado em funções](role-based-access-control-troubleshooting.md): Obtenha sugestões sobre como corrigir problemas comuns.
* [Permissões no Centro de segurança do Azure](../security-center/security-center-permissions.md)
