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
ms.openlocfilehash: 3b689c6028470021734ad78dd88748f4079c383f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Funções incorporadas para controlo de acesso baseado em funções do Azure
Inclui as seguintes funções incorporadas que podem ser atribuídas a utilizadores, grupos e serviços do Azure baseada em funções controlo de acesso (RBAC). Não é possível modificar as definições de funções incorporadas. No entanto, pode criar [funções personalizadas no Azure RBAC](custom-roles.md) para satisfazer as necessidades específicas da sua organização.

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
| [Proprietário](#owner) | Permite-lhe gerir tudo, incluindo o acesso aos recursos. |
| [Contribuinte](#contributor) | Permite-lhe gerir tudo, exceto o acesso aos recursos. |
| [leitor](#reader) | Permite-lhe ver tudo, mas não efetuar alterações. |
| [Contribuinte de serviço de gestão de API](#api-management-service-contributor) | Pode gerir o serviço e as APIs |
| [Função de operador de serviço de gestão de API](#api-management-service-operator-role) | Pode gerir o serviço, mas não as APIs |
| [Função de leitor de serviço de gestão de API](#api-management-service-reader-role) | Acesso só de leitura para o serviço e APIs |
| [Contribuinte de componentes do Application Insights](#application-insights-component-contributor) | Permite gerir componentes do Application Insights |
| [Depurador de instantâneo do Application Insights](#application-insights-snapshot-debugger) | Dá permissão ao utilizador para utilizar as funcionalidades do Depurador de Instantâneo do Application Insights |
| [Operador de tarefa de automatização](#automation-job-operator) | Criar e Gerir Tarefas através de Runbooks de Automatização. |
| [Operador de automatização](#automation-operator) | Os Operadores de Automatização podem iniciar, parar, suspender e retomar tarefas |
| [Operador de Runbook de automatização](#automation-runbook-operator) | Ler as propriedades do Runbook para que possa criar Tarefas do runbook. |
| [Proprietário de registo de pilha do Azure](#azure-stack-registration-owner) | Permite-lhe gerir os registos do Azure Stack. |
| [Cópia de segurança contribuinte](#backup-contributor) | Permite-lhe gerir o serviço de cópia de segurança mas não pode criar cofres e conceder o acesso a outros |
| [Operador de cópia de segurança](#backup-operator) | Permite-lhe gerir serviços de cópia de segurança, exceto de remoção de cópia de segurança, criação de cofre e concessão de acesso a outros |
| [Leitor de cópia de segurança](#backup-reader) | Pode ver os serviços de cópia de segurança mas não pode efetuar alterações |
| [Leitor de faturação](#billing-reader) | Permite o acesso de leitura para dados de faturação |
| [Contribuinte de BizTalk](#biztalk-contributor) | Permite-lhe gerir serviços do BizTalk, mas não aceder-lhes. |
| [Contribuinte de ponto final CDN](#cdn-endpoint-contributor) | Pode gerir pontos finais de CDN, as não pode conceder o acesso a outros utilizadores. |
| [Leitor de ponto final CDN](#cdn-endpoint-reader) | Pode visualizar os pontos finais de CDN, mas não pode efetuar alterações. |
| [Contribuinte de perfil CDN](#cdn-profile-contributor) | Pode gerir perfis de CDN e os respetivos pontos finais, mas não pode conceder o acesso a outros utilizadores. |
| [Leitor de perfil CDN](#cdn-profile-reader) | Pode visualizar perfis de CDN e os respetivos pontos finais, mas não pode efetuar alterações. |
| [Contribuidor de Rede Clássica](#classic-network-contributor) | Permite-lhe gerir redes virtuais, mas não aceder-lhes. |
| [Contribuinte de conta de armazenamento clássico](#classic-storage-account-contributor) | Permite-lhe gerir contas de armazenamento clássico, mas não aceder às mesmas. |
| [Função de serviço de operador de chave de conta de armazenamento clássico](#classic-storage-account-key-operator-service-role) | Os Operadores de Chave da Conta de Armazenamento Clássica têm permissão para listar e regenerar chaves em Contas de Armazenamento Clássicas |
| [Contribuidor clássico Máquina Virtual](#classic-virtual-machine-contributor) | Permite-lhe gerir as máquinas virtuais clássicas, mas não aceder aos mesmos e não a rede virtual ou ligados a conta do storage. |
| [Contribuidor ClearDB MySQL DB](#cleardb-mysql-db-contributor) | Permite-lhe gerir bases de dados MySQL ClearDB, mas não aceder-lhes. |
| [Função de leitor de conta do cosmos DB](#cosmos-db-account-reader-role) | Pode ler os dados de conta de base de dados do Azure Cosmos. Consulte [contribuinte de conta do DocumentDB](#documentdb-account-contributor) para gerir contas de base de dados do Azure Cosmos. |
| [Contribuinte da fábrica de dados](#data-factory-contributor) | Criar e gerir fábricas de dados, assim como os recursos subordinados dentro dos mesmos. |
| [Para programadores do Data Lake Analytics](#data-lake-analytics-developer) | Permite-lhe submeter, monitorizar e gerir as suas tarefas, mas não criar ou eliminar as contas do Data Lake Analytics. |
| [DevTest Labs utilizador](#devtest-labs-user) | Permite-lhe ligar, iniciar, reiniciar e encerrar as máquinas virtuais no Azure DevTest Labs. |
| [Contribuinte de zona DNS](#dns-zone-contributor) | Permite-lhe gerir zonas DNS e conjuntos de registos no DNS do Azure, mas não lhe permite controlar quem tem acesso aos mesmos. |
| [Conta do DocumentDB contribuinte](#documentdb-account-contributor) | Pode gerir contas de base de dados do Azure Cosmos. BD do Azure do Cosmos anteriormente é conhecido como DocumentDB. |
| [Contribuinte de conta de sistemas inteligentes](#intelligent-systems-account-contributor) | Permite-lhe gerir contas de Sistemas Inteligentes, mas não aceder-lhes. |
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
| [Contribuidor de Rede](#network-contributor) | Permite-lhe gerir redes, mas não aceder-lhes. |
| [Novo contribuinte de conta APM Relic](#new-relic-apm-account-contributor) | Permite-lhe gerir contas e aplicações do New Relic Application Performance Management, mas não aceder-lhes. |
| [Contribuinte de Cache de redis](#redis-cache-contributor) | Permite-lhe gerir caches de Redis, mas não aceder-lhes. |
| [Contribuinte de coleções de tarefa do agendador](#scheduler-job-collections-contributor) | Permite-lhe gerir coleções de tarefas do Scheduler, mas não aceder-lhes. |
| [Contribuinte de serviço de pesquisa](#search-service-contributor) | Permite-lhe gerir serviços de Pesquisa, mas não aceder-lhes. |
| [Administrador de segurança](#security-admin) | No Centro de segurança apenas: pode ver as políticas de segurança, ver Estados de segurança, editar as políticas de segurança, ver alertas e as recomendações, dispensar alertas e recomendações |
| [Gestor de segurança](#security-manager) | Permite-lhe gerir componentes de segurança, políticas de segurança e máquinas virtuais |
| [Leitor de segurança](#security-reader) | No Centro de segurança apenas: pode ver as recomendações e alertas, vista de políticas de segurança, ver Estados de segurança, mas não é possível efetuar alterações |
| [Contribuinte de recuperação de site](#site-recovery-contributor) | Permite-lhe gerir o serviço do Site Recovery exceto a criação do cofre e atribuição de funções |
| [Operador de recuperação de site](#site-recovery-operator) | Permite-lhe efetuar a ativação pós-falha mas não efetuar outras operações de gestão do Site Recovery |
| [Leitor de recuperação de site](#site-recovery-reader) | Permite-lhe ver o estado do Site Recovery mas não efetuar outras operações de gestão |
| [Contribuinte da BD SQL](#sql-db-contributor) | Permite-lhe gerir bases de dados SQL, mas não aceder aos mesmos. Além disso, não é possível gerir as respetivas políticas relacionadas com segurança ou os servidores do SQL Server principal. |
| [Gestor de segurança do SQL Server](#sql-security-manager) | Permite gerir as políticas de servidores e bases de dados SQL relacionadas com a segurança, mas não aceder às mesmas. |
| [SQL Server contribuinte](#sql-server-contributor) | Permite gerir servidores e bases de dados SQL, mas não aceder aos mesmos nem às respetivas políticas relacionadas com a segurança. |
| [Contribuinte de conta de armazenamento](#storage-account-contributor) | Permite-lhe gerir contas de armazenamento, mas não aceder às mesmas. |
| [Função de serviço de operador de chave de conta de armazenamento](#storage-account-key-operator-service-role) | Os Operadores de Chave da Conta de Armazenamento têm permissão para listar e regenerar chaves em Contas de Armazenamento |
| [Contribuinte de pedido de suporte](#support-request-contributor) | Permite-lhe criar e gerir os pedidos de Suporte |
| [Gestor de tráfego contribuinte](#traffic-manager-contributor) | Permite-lhe gerir perfis do Gestor de Tráfego, mas não lhe permite controlar quem tem acesso aos mesmos. |
| [Administrador de acesso de utilizador](#user-access-administrator) | Permite-lhe gerir o acesso de utilizador aos recursos do Azure. |
| [Início de sessão de administrador de máquinas virtuais](#virtual-machine-administrator-login) | -Os utilizadores com esta função têm a capacidade de início de sessão a uma máquina virtual com o administrador do Windows ou Linux privilégios de utilizador de raiz. |
| [Contribuinte de máquina virtual](#virtual-machine-contributor) | Permite-lhe gerir máquinas virtuais, mas não aceder aos mesmos e não a rede virtual ou ligados a conta do storage. |
| [Início de sessão de utilizador de máquina virtual](#virtual-machine-user-login) | Os utilizadores com esta função têm a capacidade de início de sessão a uma máquina virtual como um utilizador normal. |
| [Plano de contribuinte da Web](#web-plan-contributor) | Permite-lhe gerir planos Web para sites, mas não aceder-lhes. |
| [Contribuinte de Web site](#website-contributor) | Permite-lhe gerir sites (não planos Web), mas não aceder-lhes. |

As tabelas seguintes descrevem as permissões específicas para cada função. Isto pode incluir **ações**, que atribua permissões, e **NotActions**, que restringi-los.

## <a name="owner"></a>Proprietário
Permite-lhe gerir tudo, incluindo o acesso aos recursos.

| **Ações** |  |
| --- | --- |
| * | Criar e gerir recursos de todos os tipos |

## <a name="contributor"></a>Contribuinte
Permite-lhe gerir tudo, exceto o acesso aos recursos.

| **Ações** |  |
| --- | --- |
| * | Criar e gerir recursos de todos os tipos |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete | Não é possível eliminar funções e atribuições de função |
| Microsoft.Authorization/*/Write | Não é possível criar funções e atribuições de função |
| Microsoft.Authorization/elevateAccess/Action | Garante ao chamador Administrador de Acesso dos Utilizadores acesso ao âmbito do inquilino |

## <a name="reader"></a>Leitor
Permite-lhe ver tudo, mas não efetuar alterações.

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |

## <a name="api-management-service-contributor"></a>Contribuinte de Serviços de Gestão de API
Pode gerir o serviço e as APIs

| **Ações** |  |
| --- | --- |
| Microsoft.ApiManagement/service/* | Criar e gerir o serviço de API Management |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="api-management-service-operator-role"></a>Função de Operador de Serviço de Gestão de API
Pode gerir o serviço, mas não as APIs

| **Ações** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Instâncias de serviço de gestão de API de leitura |
| Microsoft.ApiManagement/service/backup/action | Serviço de gestão de API a cópia de segurança para o contentor especificado um utilizador forneceu a conta de armazenamento |
| Microsoft.ApiManagement/service/delete | Eliminar a instância de serviço de gestão de API |
| Microsoft.ApiManagement/service/managedeployments/action | Alterar o SKU/unidades, adicionar/remover implementações regional do serviço de gestão de API |
| Microsoft.ApiManagement/service/read | Ler os metadados para uma instância de serviço de gestão de API |
| Microsoft.ApiManagement/service/restore/action | Restaurar o serviço de gestão de API a partir do contentor especificado um utilizador fornecida a conta de armazenamento |
| Microsoft.ApiManagement/service/updatecertificate/action | Carregar o certificado SSL para um serviço de gestão de API |
| Microsoft.ApiManagement/service/updatehostname/action | O programa de configuração, atualizar ou remover nomes de domínio personalizado para um serviço de gestão de API |
| Microsoft.ApiManagement/service/write | Criar uma nova instância do serviço de gestão de API |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Obter a lista de chaves de utilizador |

## <a name="api-management-service-reader-role"></a>Função do Leitor do Serviço de Gestão de API do Serviço de Gestão de API
Acesso só de leitura para o serviço e APIs

| **Ações** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Instâncias de serviço de gestão de API de leitura |
| Microsoft.ApiManagement/service/read | Ler os metadados para uma instância de serviço de gestão de API |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
Os Operadores de Automatização podem iniciar, parar, suspender e retomar tarefas

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Automation/automationAccounts/jobs/read | Obtém uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Retoma uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Para uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Obtém um fluxo de trabalho da automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspende uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobs/write | Cria uma tarefa de automatização do Azure |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Obtém uma tarefa da automatização do Azure |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Cria uma tarefa da automatização do Azure |
| Microsoft.Automation/automationAccounts/linkedWorkspace/read | Obtém a área de trabalho associada à conta de automatização |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lê recursos de trabalho de Runbook híbrida |
| Microsoft.Automation/automationAccounts/read | Obtém uma conta de automatização do Azure |
| Microsoft.Automation/automationAccounts/runbooks/read | Obtém um runbook de automatização do Azure |
| Microsoft.Automation/automationAccounts/schedules/read | Obtém um recurso de agenda da automatização do Azure |
| Microsoft.Automation/automationAccounts/schedules/write | Cria ou atualiza um recurso de agenda da automatização do Azure |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
Permite-lhe gerir o serviço de cópia de segurança mas não pode criar cofres e conceder o acesso a outros

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Gerir os resultados da operação na gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Criar e gerir contentores de cópia de segurança no interior de recursos de infraestrutura de cópia de segurança do Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Criar e gerir tarefas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Tarefas de exportação |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Criar e gerir os dados de metadados relacionados com a gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Criar e gerir resultados de operações de gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Criar e gerir políticas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Criar e gerir itens que podem ser uma cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Criar e gerir itens de cópias de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Criar e gerir contentores que contém os itens de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/certificates/* | Criar e gerir certificados relacionados com a cópia de segurança no Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Criar e gerir informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Gerir a operação de deteção para obtenção recém-criado contentores |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Criar e gerir identidades registadas |
| Microsoft.RecoveryServices/Vaults/usages/* | Criar e gerir a utilização do Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
| Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="backup-operator"></a>Operador de Cópia de Segurança
Permite-lhe gerir serviços de cópia de segurança, exceto de remoção de cópia de segurança, criação de cofre e concessão de acesso a outros

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Devolve o estado da operação |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Obtém o resultado da Operação efetuada no Contentor de Proteção. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/backup/action | Executa a Cópia de Segurança do Item Protegido. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Obtém o Resultado da Operação Efetuada nos Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Devolve o estado da Operação efetuada nos Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Devolve detalhes do objecto do Item protegido |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Obter Pontos de Recuperação de Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/restore/action | Restaurar Pontos de Recuperação de Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Criar um Item protegido cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Devolve todas as registado contentores |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Criar e gerir tarefas de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Cancelar a tarefa |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Devolve o Resultado da Operação de Tarefa. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Devolve todos os objetos da tarefa |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Tarefas de exportação |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Devolve os Metadados da Gestão da Cópia de Segurança do Cofre de Serviços de Recuperação. |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Criar e gerir resultados de operações de gestão de cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Obter Resultados da Operação de Política. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devolve todas as políticas de proteção |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Criar e gerir itens que podem ser uma cópia de segurança |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Devolve a lista de todos os Itens Susceptíveis de Proteção. |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Devolve a lista de todos os Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Devolve todos os contentores que pertence à subscrição |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
| Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Gerir a operação de deteção para obtenção recém-criado contentores |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | A operação de registar o contentor de serviço pode ser utilizada para registar um contentor com o serviço de recuperação. |
| Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
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
Pode ver os serviços de cópia de segurança mas não pode efetuar alterações

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Devolve o estado da operação |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Obtém o resultado da Operação efetuada no Contentor de Proteção. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Obtém o Resultado da Operação Efetuada nos Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Devolve o estado da Operação efetuada nos Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Devolve detalhes do objecto do Item protegido |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Devolve todas as registado contentores |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Devolve o Resultado da Operação de Tarefa. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Devolve todos os objetos da tarefa |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Tarefas de exportação |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Devolve os Metadados da Gestão da Cópia de Segurança do Cofre de Serviços de Recuperação. |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Devolve o Resultado da Operação da Cópia de Segurança do Cofre de Serviços de Recuperação. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Obter Resultados da Operação de Política. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devolve todas as políticas de proteção |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Devolve a lista de todos os Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Devolve todos os contentores que pertence à subscrição |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
| Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/leitura |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Obter Pontos de Recuperação de Itens Protegidos. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
| Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |

## <a name="billing-reader"></a>Leitor de Faturação
Permite o acesso de leitura para dados de faturação

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Billing/*/read | Ler as informações de faturação |
| Microsoft.Consumption/*/read |  |
| Microsoft.Commerce/*/read |  |
| Microsoft.Management/managementGroups/read | Liste os grupos de gestão para o utilizador autenticado. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="biztalk-contributor"></a>Contribuinte do BizTalk
Permite-lhe gerir serviços do BizTalk, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.BizTalkServices/BizTalk/* | Criar e gerir serviços BizTalk |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
Permite-lhe gerir redes virtuais, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.ClassicNetwork/* | Criar e gerir redes clássicos |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-storage-account-contributor"></a>Contribuinte de Conta de Armazenamento Clássica
Permite-lhe gerir contas de armazenamento clássico, mas não aceder às mesmas.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.ClassicStorage/storageAccounts/* | Criar e gerir contas de armazenamento |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-storage-account-key-operator-service-role"></a>Função do Serviço de Operador de Chave da Conta de Armazenamento Clássica
Os Operadores de Chave da Conta de Armazenamento Clássica têm permissão para listar e regenerar chaves em Contas de Armazenamento Clássicas

| **Ações** |  |
| --- | --- |
| Microsoft.ClassicStorage/storageAccounts/listkeys/action | Lista as chaves de acesso para as contas de armazenamento. |
| Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Volta a gerar as chaves de acesso existentes para a conta de armazenamento. |

## <a name="classic-virtual-machine-contributor"></a>Contribuinte de Máquina Virtual Clássica
Permite-lhe gerir as máquinas virtuais clássicas, mas não aceder aos mesmos e não a rede virtual ou ligados a conta do storage.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.ClassicCompute/domainNames/* | Criar e gerir nomes de domínio de computação clássicos |
| Microsoft.ClassicCompute/virtualMachines/* | Criar e gerir máquinas virtuais |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
| Microsoft.ClassicNetwork/reservedIps/link/action | Ligar um IP reservado |
| Microsoft.ClassicNetwork/reservedIps/read | Obtém os IPs reservados |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Associa à rede virtual. |
| Microsoft.ClassicNetwork/virtualNetworks/read | Obtém a rede virtual. |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Devolve o disco de conta de armazenamento. |
| Microsoft.ClassicStorage/storageAccounts/images/read | Devolve a imagem de conta de armazenamento. |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista as chaves de acesso para as contas de armazenamento. |
| Microsoft.ClassicStorage/storageAccounts/read | Devolve a conta de armazenamento com a conta fornecida. |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cleardb-mysql-db-contributor"></a>Contribuinte de Base de Dados MySQL ClearDB
Permite-lhe gerir bases de dados MySQL ClearDB, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| successbricks.cleardb/databases/* | Criar e gerir bases de dados ClearDB MySQL |

## <a name="cosmos-db-account-reader-role"></a>Função do Leitor da Conta do Cosmos DB
Pode ler os dados de conta de base de dados do Azure Cosmos. Consulte [contribuinte de conta do DocumentDB](#documentdb-account-contributor) para gerir contas de base de dados do Azure Cosmos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de funções, pode permissões de leitura para cada utilizador |
| Microsoft.DocumentDB/*/read | Uma coleção de leitura |
| Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Lê a base de dados de chaves de conta de só de leitura. |
| Microsoft.Insights/MetricDefinitions/read | Ler definições de métricas |
| Microsoft.Insights/Metrics/read | Ler métricas |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="data-factory-contributor"></a>Contribuinte do Data Factory
Criar e gerir fábricas de dados, assim como os recursos subordinados dentro dos mesmos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.DataFactory/dataFactories/* | Criar e gerir as fábricas de dados e recursos subordinados dentro delas. |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
Permite-lhe ligar, iniciar, reiniciar e encerrar as máquinas virtuais no Azure DevTest Labs.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Compute/availabilitySets/read | Obter as propriedades de um conjunto de disponibilidade |
| Microsoft.Compute/virtualMachines/*/read | Ler as propriedades de uma máquina virtual (tamanhos de VM, estado de runtime, extensões VM, etc.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Desliga a máquina virtual e liberta os recursos de computação |
| Microsoft.Compute/virtualMachines/read | Obter as propriedades de uma máquina virtual |
| Microsoft.Compute/virtualMachines/restart/action | Reinicia a máquina virtual |
| Microsoft.Compute/virtualMachines/start/action | Inicia a máquina virtual |
| Microsoft.DevTestLab/*/read | Ler as propriedades de um laboratório |
| Microsoft.DevTestLab/labs/createEnvironment/action | Crie máquinas virtuais no laboratório. |
| Microsoft.DevTestLab/labs/claimAnyVm/action | Uma máquina virtual claimable aleatória no laboratório de afirmações. |
| Microsoft.DevTestLab/labs/formulas/delete | Elimine as fórmulas. |
| Microsoft.DevTestLab/labs/formulas/read | Ler as fórmulas. |
| Microsoft.DevTestLab/labs/formulas/write | Adiciona ou modifica as fórmulas. |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Avalia a política de laboratório. |
| Microsoft.DevTestLab/labs/virtualMachines/claim/action | Assumir a propriedade de uma máquina virtual existente |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de Balanceador de carga |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Associa uma regra de nat de entrada do Balanceador de carga |
| Microsoft.Network/networkInterfaces/*/read | Ler as propriedades de uma interface de rede (por exemplo, todos os balanceadores de carga que a interface de rede faz parte de) |
| Microsoft.Network/networkInterfaces/join/action | Associa uma Máquina Virtual a uma interface de rede |
| Microsoft.Network/networkInterfaces/read | Obtém uma definição de interface de rede.  |
| Microsoft.Network/networkInterfaces/write | Cria uma interface de rede ou atualiza uma interface de rede existente.  |
| Microsoft.Network/publicIPAddresses/*/read | Ler as propriedades de um endereço IP público |
| Microsoft.Network/publicIPAddresses/join/action | Associa um endereço ip público |
| Microsoft.Network/publicIPAddresses/read | Obtém uma definição de endereço ip público. |
| Microsoft.Network/virtualNetworks/subnets/join/action | Associa uma rede virtual |
| Microsoft.Resources/deployments/operations/read | Obtém ou lista as operações de implementação. |
| Microsoft.Resources/deployments/read | Obtém ou lista implementações. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/listKeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |

| **NotActions** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/vmSizes/read | Lista os tamanhos disponíveis para os quais a máquina virtual pode ser atualizada |

## <a name="dns-zone-contributor"></a>Contribuidor da Zona DNS
Permite-lhe gerir zonas DNS e conjuntos de registos no DNS do Azure, mas não lhe permite controlar quem tem acesso aos mesmos.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/dnsZones/* | Criar e gerir registos e zonas DNS |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="documentdb-account-contributor"></a>Contribuidor de Conta do DocumentDB
Pode gerir contas de base de dados do Azure Cosmos. BD do Azure do Cosmos anteriormente é conhecido como DocumentDB.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.DocumentDb/databaseAccounts/* | Criar e gerir contas de base de dados do Azure Cosmos |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="intelligent-systems-account-contributor"></a>Contribuinte de Conta de Sistemas Inteligentes
Permite-lhe gerir contas de Sistemas Inteligentes, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.IntelligentSystems/accounts/* | Criar e gerir contas de sistemas inteligentes |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
| Microsoft.Insights/Register/Action | Registar o fornecedor de microsoft insights |
| Microsoft.Insights/webtests/* | Testes web do Application Insights de leitura/escrita/eliminar. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Pacotes de solução de análise de registos de leitura/escrita/eliminar. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* | Análise de registos de leitura/escrita/eliminar pesquisas guardadas. |
| Microsoft.OperationalInsights/workspaces/search/action | Executa uma consulta de pesquisa |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action | Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Configurações de conhecimentos aprofundados de armazenamento de análise de registos de leitura/escrita/eliminar. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>Leitor de Monitorização
Pode ler todos os dados de monitorização (métricas, os registos, etc.). Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Ações** |  |
| --- | --- |
| * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
| Microsoft.OperationalInsights/workspaces/search/action | Executa uma consulta de pesquisa |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="network-contributor"></a>Contribuinte de Rede
Permite-lhe gerir redes, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/* | Criar e gerir redes |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
Permite-lhe gerir caches de Redis, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Cache/redis/* | Criar e gerir caches de Redis |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="scheduler-job-collections-contributor"></a>Contribuinte de Coleções de Tarefas do Agendador
Permite-lhe gerir coleções de tarefas do Scheduler, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Scheduler/jobcollections/* | Criar e gerir coleções de tarefas |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="search-service-contributor"></a>Contribuinte de Serviços de Pesquisa
Permite-lhe gerir serviços de Pesquisa, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Security/*/read | Componentes de segurança de leitura e políticas |
| Microsoft.Security/locations/alerts/dismiss/action | Dispensar um alerta de segurança |
| Microsoft.Security/locations/tasks/dismiss/action | Ignorar uma recomendação de segurança |
| Microsoft.Security/policies/write | Atualiza a política de segurança |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="security-manager"></a>Gestor de Segurança
Permite-lhe gerir componentes de segurança, políticas de segurança e máquinas virtuais

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.ClassicCompute/*/read | Ler as informações de configuração as máquinas virtuais clássicas |
| Microsoft.ClassicCompute/virtualMachines/*/write | Escrita de configuração para as máquinas virtuais clássicas |
| Microsoft.ClassicNetwork/*/read | Ler as informações de configuração sobre a rede clássico |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Security/*/read | Componentes de segurança de leitura e políticas |

## <a name="site-recovery-contributor"></a>Contribuidor do Site Recovery
Permite-lhe gerir o serviço do Site Recovery exceto a criação do cofre e atribuição de funções

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/certificates/write | A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Criar e gerir informações expandidas relacionados com o Cofre |
| Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Criar e gerir identidades registadas |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Criar ou atualizar definições de alerta de replicação |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler quaisquer eventos |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Criar e gerir recursos de infraestrutura de replicação |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Criar e gerir tarefas de replicação |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Criar e gerir políticas de replicação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Criar e gerir planos de recuperação |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Criar e gerir a configuração de armazenamento do Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Devolve informações sobre o token para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Alertas de leitura para o Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/leitura |  |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="site-recovery-operator"></a>Operador do Site Recovery
Permite-lhe efetuar a ativação pós-falha mas não efetuar outras operações de gestão do Site Recovery

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
| Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Ler as definições de alertas |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler quaisquer eventos |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Verifica a Consistência dos Recursos de infraestrutura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Ler todos os recursos de infraestrutura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Reassociar Gateway |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Renovar o certificado para recursos de infraestrutura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Ler a quaisquer redes |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Leia os mapeamentos de rede |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/leitura | Leu os contentores de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Ler quaisquer itens passíveis de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Aplicar o ponto de recuperação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/ação | Consolidação de ativação pós-falha |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/ação | Ativação pós-falha planeada |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/leitura | Ler os itens protegidos |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Pontos de recuperação de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/ação | Reparar a replicação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/Reproteção/ação | Proteja o Item protegido |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/ação | Ativação pós-falha de teste |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/ação | Limpeza da ativação pós-falha de teste |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/ação | Ativação pós-falha |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Serviço de mobilidade de atualização |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/leitura | Ler quaisquer mapeamentos de contentor de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Ler quaisquer fornecedores de serviços de recuperação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider/ação | Atualize o fornecedor |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/leitura | Ler as classificações de armazenamento |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/leitura | Ler quaisquer mapeamentos de classificação de armazenamento |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Ler todas as tarefas |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Criar e gerir tarefas de replicação |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Ler todas as políticas |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Plano de recuperação de consolidação de ativação pós-falha |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Plano de recuperação de ativação pós-falha planeada |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Leia os planos de recuperação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Proteja o plano de recuperação |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Plano de recuperação de ativação pós-falha de teste |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Plano de recuperação de limpeza da ativação pós-falha de teste |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Plano de recuperação de ativação pós-falha |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Alertas de leitura para o Cofre de serviços de recuperação |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/leitura |  |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Devolve informações sobre o token para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="site-recovery-reader"></a>Leitor do Site Recovery
Permite-lhe ver o estado do Site Recovery mas não efetuar outras operações de gestão

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/leitura |  |
| Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Ler as definições de alertas |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler quaisquer eventos |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Ler todos os recursos de infraestrutura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Ler a quaisquer redes |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Leia os mapeamentos de rede |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/leitura | Leu os contentores de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Ler quaisquer itens passíveis de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/leitura | Ler os itens protegidos |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Pontos de recuperação de replicação de leitura |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/leitura | Ler quaisquer mapeamentos de contentor de proteção |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Ler quaisquer fornecedores de serviços de recuperação |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/leitura | Ler as classificações de armazenamento |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/leitura | Ler quaisquer mapeamentos de classificação de armazenamento |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Ler todas as tarefas |
| Microsoft.RecoveryServices/vaults/replicationJobs/read | Ler todas as tarefas |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Ler todas as políticas |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Leia os planos de recuperação |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Devolve informações sobre o token para o Cofre de serviços de recuperação. |
| Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="sql-db-contributor"></a>Contribuinte de Base de Dados SQL
Permite-lhe gerir bases de dados SQL, mas não aceder aos mesmos. Além disso, não é possível gerir as respetivas políticas relacionadas com segurança ou os servidores do SQL Server principal.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/databases/* | Criar e gerir bases de dados SQL |
| Microsoft.Sql/servers/read | Devolva a lista de servidores ou obtém as propriedades para o servidor especificado. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Não é possível editar as políticas de auditoria |
| Microsoft.Sql/servers/databases/auditingSettings/* | Não é possível editar as definições de auditoria |
| Microsoft.Sql/servers/databases/auditRecords/read | Obter os registos de auditoria de blob de base de dados |
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
Permite gerir as políticas de servidores e bases de dados SQL relacionadas com a segurança, mas não aceder às mesmas.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura Microsoft |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Sql/servers/auditingPolicies/* | Criar e gerir políticas de auditoria do SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Criar e gerir a definição de auditoria do servidor SQL |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Criar e gerir políticas de auditoria de base de dados do SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Criar e gerir definições de auditoria de base de dados do SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read | Registos de auditoria de leitura |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Criar e gerir políticas de ligação de base de dados do SQL server |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Criar e gerir políticas de máscara de dados do SQL server da base de dados |
| Microsoft.Sql/servers/databases/read | Devolva a lista de bases de dados ou obtém as propriedades da base de dados especificado. |
| Microsoft.Sql/servers/databases/schemas/read | Obter a lista de esquemas de uma base de dados |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Obter a lista de colunas de uma tabela |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/read | Obter a lista de tabelas de uma base de dados |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Criar e gerir políticas alertas de segurança da base de dados de servidor SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* | Criar e gerir as métricas de segurança de base de dados do SQL server |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/firewallRules/* |  |
| Microsoft.Sql/servers/read | Devolva a lista de servidores ou obtém as propriedades para o servidor especificado. |
| Microsoft.Sql/servers/securityAlertPolicies/* | Criar e gerir políticas de alerta de segurança do SQL server |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="sql-server-contributor"></a>Contribuinte do SQL Server
Permite gerir servidores e bases de dados SQL, mas não aceder aos mesmos nem às respetivas políticas relacionadas com a segurança.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
Permite-lhe gerir contas de armazenamento, mas não aceder às mesmas.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Ler todas as autorização |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Insights/diagnosticSettings/* | Gerir definições de diagnóstico |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/* | Criar e gerir contas de armazenamento |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="storage-account-key-operator-service-role"></a>Função do Serviço de Operador de Chave da Conta de Armazenamento
Os Operadores de Chave da Conta de Armazenamento têm permissão para listar e regenerar chaves em Contas de Armazenamento

| **Ações** |  |
| --- | --- |
| Microsoft.Storage/storageAccounts/listkeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
| Microsoft.Storage/storageAccounts/regeneratekey/action | Regenera as chaves de acesso da conta de armazenamento especificada. |

## <a name="support-request-contributor"></a>Contribuidor de Pedido de Suporte
Permite-lhe criar e gerir os pedidos de Suporte

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
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
Permite-lhe gerir o acesso de utilizador aos recursos do Azure.

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
Permite-lhe gerir máquinas virtuais, mas não aceder aos mesmos e não a rede virtual ou ligados a conta do storage.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Compute/availabilitySets/* | Criar e gerir conjuntos de disponibilidade de computação |
| Microsoft.Compute/locations/* | Criar e gerir as localizações de computação |
| Microsoft.Compute/virtualMachines/* | Criar e gerir máquinas virtuais |
| Microsoft.Compute/virtualMachineScaleSets/* | Criar e gerir conjuntos de dimensionamento de máquina virtual |
| Microsoft.DevTestLab/schedules/* |  |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de gateway de aplicação |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de Balanceador de carga |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Associa um balanceador de carga conjunto nat de entrada |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Associa uma regra de nat de entrada do Balanceador de carga |
| Microsoft.Network/loadBalancers/probes/join/action | Permite utilizar as pesquisas de um balanceador de carga. Por exemplo, com esta propriedade de healthProbe de permissão de dimensionamento VM conjunto pode referenciar a pesquisa. |
| Microsoft.Network/loadBalancers/read | Obtém uma definição de Balanceador de carga |
| Microsoft.Network/locations/* | Criar e gerir as localizações de rede |
| Microsoft.Network/networkInterfaces/* | Criar e gerir as interfaces de rede |
| Microsoft.Network/networkSecurityGroups/join/action | Associa um grupo de segurança de rede |
| Microsoft.Network/networkSecurityGroups/read | Obtém uma definição de grupo de segurança de rede |
| Microsoft.Network/publicIPAddresses/join/action | Associa um endereço ip público |
| Microsoft.Network/publicIPAddresses/read | Obtém uma definição de endereço ip público. |
| Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
| Microsoft.Network/virtualNetworks/subnets/join/action | Associa uma rede virtual |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Storage/storageAccounts/listKeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
| Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="virtual-machine-user-login"></a>Início de sessão de utilizador de máquina virtual
Os utilizadores com esta função têm a capacidade de início de sessão a uma máquina virtual como um utilizador normal.

| **Ações** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="web-plan-contributor"></a>Contribuinte de Plano Web
Permite-lhe gerir planos Web para sites, mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.Web/serverFarms/* | Criar e gerir farms de servidores |

## <a name="website-contributor"></a>Contribuinte de Web site
Permite-lhe gerir sites (não planos Web), mas não aceder-lhes.

| **Ações** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorização de leitura |
| Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
| Microsoft.Insights/components/* | Criar e gerir Insights componentes |
| Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
| Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
| Microsoft.Support/* | Criar e gerir pedidos de suporte |
| Microsoft.Web/certificates/* | Criar e gerir certificados de Web site |
| Microsoft.Web/listSitesAssignedToHostName/read | Obter os nomes dos sites atribuídos ao nome do anfitrião. |
| Microsoft.Web/serverFarms/join/action |  |
| Microsoft.Web/serverFarms/read | Obter as propriedades num plano do App Service |
| Microsoft.Web/sites/* | Criar e gerir sites (criação de site também necessita de permissões de escrita para o plano de serviço de aplicação associado) |

## <a name="see-also"></a>Consulte também
* [Controlo de acesso baseado em funções](role-assignments-portal.md): começar a utilizar o RBAC no portal do Azure.
* [Funções personalizadas no Azure RBAC](custom-roles.md): Saiba como criar funções personalizadas para se ajustarem às suas necessidades de acesso.
* [Criar um relatório de histórico de alterações de acesso](change-history-report.md): manter um registo dos alteração atribuições de funções no RBAC.
* [Resolução de problemas de controlo de acesso baseado em funções](troubleshooting.md): Obtenha sugestões sobre como corrigir problemas comuns.
* [Permissões no Centro de segurança do Azure](../security-center/security-center-permissions.md)
