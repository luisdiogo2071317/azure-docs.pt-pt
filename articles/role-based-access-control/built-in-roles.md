---
title: Funções incorporadas para o controlo de acesso baseado em funções (RBAC) no Azure | Microsoft Docs
description: Descreve as funções incorporadas para o controlo de acesso baseado em funções (RBAC) no Azure. Lista as ações e notActions.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 365959a588dc48e7991efea239ba823c3ca65e7a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640545"
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Funções incorporadas do controlo de acesso baseado em funções do Azure
[O controlo de acesso baseado em funções (RBAC)](overview.md) tem várias definições de função incorporada que pode atribuir aos utilizadores, grupos e principais de serviço. Atribuições de função são a forma de controlar o acesso a recursos no Azure. Se as funções incorporadas não satisfazem as necessidades específicas da sua organização, pode criar a sua própria [funções personalizadas](custom-roles.md).

As funções incorporadas são sempre evolução. Para obter as definições de função mais recentes, utilize [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) ou [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list).

## <a name="built-in-role-descriptions"></a>Descrições de função incorporada
A tabela seguinte fornece breves descrições das funções incorporadas. Clique no nome de função para ver a lista de `actions`, `notActions`, `dataActions`, e `notDataActions` para cada função.


| Função incorporada | Descrição |
| --- | --- |
| [Proprietário](#owner) | Permite-lhe gerir tudo, incluindo o acesso aos recursos. |
| [Contribuinte](#contributor) | Permite-lhe gerir tudo, exceto o acesso aos recursos. |
| [leitor](#reader) | Permite-lhe ver tudo, mas não efetuar alterações. |
| [AcrImageSigner](#acrimagesigner) | signatário de imagem acr |
| [AcrQuarantineReader](#acrquarantinereader) | leitor de dados de quarentena do acr |
| [AcrQuarantineWriter](#acrquarantinewriter) | escritor de dados de quarentena do acr |
| [Contribuinte de serviço de gestão de API](#api-management-service-contributor) | Pode gerir o serviço e as APIs |
| [Função de operador de serviço de gestão de API](#api-management-service-operator-role) | Pode gerir o serviço, mas não as APIs |
| [Função de leitor de serviço de gestão de API](#api-management-service-reader-role) | Acesso só de leitura para o serviço e APIs |
| [Contribuinte de componentes do Application Insights](#application-insights-component-contributor) | Permite gerir componentes do Application Insights |
| [Depurador de instantâneo do Application Insights](#application-insights-snapshot-debugger) | Dá permissão ao utilizador para utilizar as funcionalidades do Snapshot Debugger do Application Insights |
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
| [Contribuidor clássico Máquina Virtual](#classic-virtual-machine-contributor) | Permite-lhe gerir máquinas virtuais clássicas, mas não aceder-lhes, além de que não pode gerir a rede virtual ou conta de armazenamento às quais estão ligadas. |
| [Contribuidor ClearDB MySQL DB](#cleardb-mysql-db-contributor) | Permite-lhe gerir bases de dados MySQL ClearDB, mas não aceder-lhes. |
| [Função de leitor de conta do cosmos DB](#cosmos-db-account-reader-role) | Pode ler os dados de conta de base de dados do Azure Cosmos. Consulte [contribuinte de conta do DocumentDB](#documentdb-account-contributor) para gerir contas de base de dados do Azure Cosmos. |
| [Contribuinte da fábrica de dados](#data-factory-contributor) | Criar e gerir fábricas de dados, assim como os recursos subordinados dentro dos mesmos. |
| [Para programadores do Data Lake Analytics](#data-lake-analytics-developer) | Permite-lhe submeter, monitorizar e gerir as suas tarefas, mas não criar ou eliminar as contas do Data Lake Analytics. |
| [Purger de dados](#data-purger) | Pode remover os dados de análise |
| [DevTest Labs utilizador](#devtest-labs-user) | Permite-lhe ligar, iniciar, reiniciar e encerrar as máquinas virtuais no Azure DevTest Labs. |
| [Contribuinte de zona DNS](#dns-zone-contributor) | Permite-lhe gerir zonas DNS e conjuntos de registos no DNS do Azure, mas não lhe permite controlar quem tem acesso aos mesmos. |
| [Conta do DocumentDB contribuinte](#documentdb-account-contributor) | Pode gerir contas de base de dados do Azure Cosmos. BD do Azure do Cosmos anteriormente é conhecido como DocumentDB. |
| [Contribuinte de conta de sistemas inteligentes](#intelligent-systems-account-contributor) | Permite-lhe gerir contas de Sistemas Inteligentes, mas não aceder-lhes. |
| [Contribuidor do Cofre de chaves](#key-vault-contributor) | Permite-lhe gerir cofres de chaves, mas não o acesso aos mesmos. |
| [Criador de laboratório](#lab-creator) | Permite-lhe criar, gerir e eliminar os seus laboratórios geridos nas suas Contas do Azure Lab. |
| [Contribuinte de análise do registo](#log-analytics-contributor) | Contribuinte de análise do registo pode ler todos os dados de monitorização e editar as definições de monitorização. Editar definições de monitorização inclui a adicionar a extensão VM para VMs; ler as chaves de conta de armazenamento para poder configurar a recolha de registos do armazenamento do Azure; criar e configurar as contas de automatização; Adicionar soluções; e configurar o diagnóstico do Azure em todos os recursos do Azure. |
| [Leitor do registo de análise](#log-analytics-reader) | O Leitor do Log Analytics pode visualizar e procurar todos os dados de monitorização assim como visualizar as definições de monitorização, incluindo a visualização da configuração de diagnósticos Azure em todos os recursos do Azure. |
| [Contribuinte de aplicação lógica](#logic-app-contributor) | Permite-lhe gerir aplicações lógicas, mas não aceder às mesmas. |
| [Operador de aplicação lógica](#logic-app-operator) | Permite-lhe ler, ativar e desativar a aplicação lógica. |
| [Identidade gerido contribuinte](#managed-identity-contributor) | Criar, Ler, Atualizar e Eliminar a Identidade Atribuída ao Utilizador |
| [Operador de identidade gerido](#managed-identity-operator) | Ler e Atribuir a Identidade Atribuída ao Utilizador |
| [Monitorização contribuinte](#monitoring-contributor) | Pode ler todos os dados de monitorização e editar as definições de monitorização. Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Leitor de monitorização](#monitoring-reader) | Pode ler todos os dados de monitorização (métricas, os registos, etc.). Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Contribuidor de Rede](#network-contributor) | Permite-lhe gerir redes, mas não aceder-lhes. |
| [Novo contribuinte de conta APM Relic](#new-relic-apm-account-contributor) | Permite-lhe gerir contas e aplicações do New Relic Application Performance Management, mas não aceder-lhes. |
| [Leitor e acesso a dados](#reader-and-data-access) | Permite ver tudo mas não permitirá eliminar ou criar uma conta de armazenamento ou recurso contido. Também permitirá o acesso de leitura/escrita para todos os dados contidos numa conta do storage através de acesso às chaves de conta de armazenamento. |
| [Contribuinte de Cache de redis](#redis-cache-contributor) | Permite-lhe gerir caches de Redis, mas não aceder-lhes. |
| [Política de recurso Contribuidor (pré-visualização)](#resource-policy-contributor-preview) | (Pré-visualização) Os utilizadores substituídos de EA, com direitos para criar/modificar a política de recurso, criam pedidos de suporte e leem recursos/hierarquia. |
| [Contribuinte de coleções de tarefa do agendador](#scheduler-job-collections-contributor) | Permite-lhe gerir coleções de tarefas do Scheduler, mas não aceder-lhes. |
| [Contribuinte de serviço de pesquisa](#search-service-contributor) | Permite-lhe gerir serviços de Pesquisa, mas não aceder-lhes. |
| [Administrador de segurança](#security-admin) | No Centro de segurança apenas: pode ver as políticas de segurança, ver Estados de segurança, editar as políticas de segurança, ver alertas e as recomendações, dispensar alertas e recomendações |
| [Gestor de segurança (legados)](#security-manager-legacy) | Esta é uma função de legado. Em vez disso, utilize o administrador de segurança |
| [Leitor de segurança](#security-reader) | No Centro de segurança apenas: pode ver as recomendações e alertas, vista de políticas de segurança, ver Estados de segurança, mas não é possível efetuar alterações |
| [Contribuinte de recuperação de site](#site-recovery-contributor) | Permite-lhe gerir o serviço do Site Recovery exceto a criação do cofre e atribuição de funções |
| [Operador de recuperação de site](#site-recovery-operator) | Permite-lhe efetuar a ativação pós-falha mas não efetuar outras operações de gestão do Site Recovery |
| [Leitor de recuperação de site](#site-recovery-reader) | Permite-lhe ver o estado do Site Recovery mas não efetuar outras operações de gestão |
| [Contribuinte da BD SQL](#sql-db-contributor) | Permite-lhe gerir bases de dados SQL, mas não aceder aos mesmos. Além disso, não é possível gerir as respetivas políticas relacionadas com segurança ou os servidores do SQL Server principal. |
| [Gestor de segurança do SQL Server](#sql-security-manager) | Permite gerir as políticas de servidores e bases de dados SQL relacionadas com a segurança, mas não aceder às mesmas. |
| [SQL Server contribuinte](#sql-server-contributor) | Permite gerir servidores e bases de dados SQL, mas não aceder aos mesmos nem às respetivas políticas relacionadas com a segurança. |
| [Contribuidor de Conta de Armazenamento](#storage-account-contributor) | Permite-lhe gerir contas de armazenamento, mas não aceder às mesmas. |
| [Função de serviço de operador de chave de conta de armazenamento](#storage-account-key-operator-service-role) | Os Operadores de Chave da Conta de Armazenamento têm permissão para listar e regenerar chaves em Contas de Armazenamento |
| [Contribuinte de dados de BLOBs de armazenamento (pré-visualização)](#storage-blob-data-contributor-preview) | Permite acesso de leitura, escrita e eliminação a dados e contentores de blobs de Armazenamento do Azure |
| [Leitor de dados de BLOBs de armazenamento (pré-visualização)](#storage-blob-data-reader-preview) | Permite o acesso de leitura de dados e contentores de blobs de Armazenamento do Azure. |
| [Contribuinte de dados de fila de armazenamento (pré-visualização)](#storage-queue-data-contributor-preview) | Permite acesso de leitura, escrita e eliminação a filas e mensagens de filas de Armazenamento do Azure |
| [Leitor de dados de fila de armazenamento (pré-visualização)](#storage-queue-data-reader-preview) | Permite acesso de leitura às filas e mensagens de fila de Armazenamento do Azure |
| [Contribuinte de pedido de suporte](#support-request-contributor) | Permite-lhe criar e gerir os pedidos de Suporte |
| [Gestor de tráfego contribuinte](#traffic-manager-contributor) | Permite-lhe gerir perfis do Gestor de Tráfego, mas não lhe permite controlar quem tem acesso aos mesmos. |
| [Administrador de acesso de utilizador](#user-access-administrator) | Permite-lhe gerir o acesso de utilizador aos recursos do Azure. |
| [Início de sessão de administrador de máquinas virtuais](#virtual-machine-administrator-login) | -  Os utilizadores com esta função têm a capacidade de iniciar sessão numa máquina virtual com privilégios de administrador do Windows ou utilizador de raiz do Linux. |
| [Contribuidor de Máquina Virtual](#virtual-machine-contributor) | Permite-lhe gerir máquinas virtuais, mas não aceder aos mesmos e não a rede virtual ou ligados a conta do storage. |
| [Início de sessão de utilizador de máquina virtual](#virtual-machine-user-login) | Os utilizadores com esta função têm a capacidade de iniciar sessão numa máquina virtual como utilizador normal. |
| [Plano de contribuinte da Web](#web-plan-contributor) | Permite-lhe gerir planos Web para sites, mas não aceder-lhes. |
| [Contribuinte de Web site](#website-contributor) | Permite-lhe gerir sites (não planos Web), mas não aceder-lhes. |


## <a name="owner"></a>Proprietário
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir tudo, incluindo o acesso aos recursos. |
> | **Id** | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | **Ações** |  |
> | * | Criar e gerir recursos de todos os tipos |

## <a name="contributor"></a>Contribuinte
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir tudo, exceto o acesso aos recursos. |
> | **Id** | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | **Ações** |  |
> | * | Criar e gerir recursos de todos os tipos |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Não é possível eliminar funções e atribuições de função |
> | Microsoft.Authorization/*/Write | Não é possível criar funções e atribuições de função |
> | Microsoft.Authorization/elevateAccess/Action | Garante ao chamador Administrador de Acesso dos Utilizadores acesso ao âmbito do inquilino |

## <a name="reader"></a>Leitor
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe ver tudo, mas não efetuar alterações. |
> | **Id** | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | **Ações** |  |
> | * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |

## <a name="acrimagesigner"></a>AcrImageSigner
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | signatário de imagem acr |
> | **Id** | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | **Ações** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |

## <a name="acrquarantinereader"></a>AcrQuarantineReader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | leitor de dados de quarentena do acr |
> | **Id** | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | **Ações** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="acrquarantinewriter"></a>AcrQuarantineWriter
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | escritor de dados de quarentena do acr |
> | **Id** | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | **Ações** |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="api-management-service-contributor"></a>Contribuinte de Serviços de Gestão de API
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode gerir o serviço e as APIs |
> | **Id** | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | **Ações** |  |
> | Microsoft.ApiManagement/service/* | Criar e gerir o serviço de API Management |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="api-management-service-operator-role"></a>Função de Operador de Serviço de Gestão de API
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode gerir o serviço, mas não as APIs |
> | **Id** | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | **Ações** |  |
> | Microsoft.ApiManagement/service/*/read | Instâncias de serviço de gestão de API de leitura |
> | Microsoft.ApiManagement/service/backup/action | Serviço de gestão de API a cópia de segurança para o contentor especificado um utilizador forneceu a conta de armazenamento |
> | Microsoft.ApiManagement/service/delete | Eliminar a instância de serviço de gestão de API |
> | Microsoft.ApiManagement/service/managedeployments/action | Alterar o SKU/unidades, adicionar/remover implementações regional do serviço de gestão de API |
> | Microsoft.ApiManagement/service/read | Ler os metadados para uma instância de serviço de gestão de API |
> | Microsoft.ApiManagement/service/restore/action | Restaurar o serviço de gestão de API a partir do contentor especificado um utilizador fornecida a conta de armazenamento |
> | Microsoft.ApiManagement/service/updatecertificate/action | Carregar o certificado SSL para um serviço de gestão de API |
> | Microsoft.ApiManagement/service/updatehostname/action | O programa de configuração, atualizar ou remover nomes de domínio personalizado para um serviço de gestão de API |
> | Microsoft.ApiManagement/service/write | Criar uma nova instância do serviço de gestão de API |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Obter a lista de chaves de utilizador |

## <a name="api-management-service-reader-role"></a>Função do Leitor do Serviço de Gestão de API do Serviço de Gestão de API
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Acesso só de leitura para o serviço e APIs |
> | **Id** | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | **Ações** |  |
> | Microsoft.ApiManagement/service/*/read | Instâncias de serviço de gestão de API de leitura |
> | Microsoft.ApiManagement/service/read | Ler os metadados para uma instância de serviço de gestão de API |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Obter a lista de chaves de utilizador |

## <a name="application-insights-component-contributor"></a>Contribuinte de Componentes do Application Insights
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite gerir componentes do Application Insights |
> | **Id** | ae349356-3a1b-4a5e-921d-050484c6347e |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.Insights/components/* | Criar e gerir Insights componentes |
> | Microsoft.Insights/webtests/* | Criar e gerir os testes web |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="application-insights-snapshot-debugger"></a>Snapshot Debugger do Application Insights
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Dá permissão ao utilizador para utilizar as funcionalidades do Snapshot Debugger do Application Insights |
> | **Id** | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="automation-job-operator"></a>Operador de Tarefas de Automatização
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Criar e Gerir Tarefas através de Runbooks de Automatização. |
> | **Id** | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Automation/automationAccounts/jobs/read | Obtém uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Retoma uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Para uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lê recursos de trabalho de Runbook híbrida |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Obtém um fluxo de trabalho da automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspende uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/write | Cria uma tarefa de automatização do Azure |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="automation-operator"></a>Operador de Automatização
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Os Operadores de Automatização podem iniciar, parar, suspender e retomar tarefas |
> | **Id** | d3881f73-407a-4167-8283-e981cbba0404 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lê recursos de trabalho de Runbook híbrida |
> | Microsoft.Automation/automationAccounts/jobs/read | Obtém uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Retoma uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Para uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Obtém um fluxo de trabalho da automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspende uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobs/write | Cria uma tarefa de automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Obtém uma tarefa da automatização do Azure |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Cria uma tarefa da automatização do Azure |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Obtém a área de trabalho associada à conta de automatização |
> | Microsoft.Automation/automationAccounts/read | Obtém uma conta de automatização do Azure |
> | Microsoft.Automation/automationAccounts/runbooks/read | Obtém um runbook de automatização do Azure |
> | Microsoft.Automation/automationAccounts/schedules/read | Obtém um recurso de agenda da automatização do Azure |
> | Microsoft.Automation/automationAccounts/schedules/write | Cria ou atualiza um recurso de agenda da automatização do Azure |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Obtém o resultado de uma tarefa |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="automation-runbook-operator"></a>Operador de Runbook de Automatização
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Ler as propriedades do Runbook para que possa criar Tarefas do runbook. |
> | **Id** | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Automation/automationAccounts/runbooks/read | Obtém um runbook de automatização do Azure |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="azure-stack-registration-owner"></a>Proprietário do Registo do Azure Stack
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir os registos do Azure Stack. |
> | **Id** | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | **Ações** |  |
> | Microsoft.AzureStack/registrations/products/listDetails/action | Obtém expandido detalhes de um produto de pilha de Azure Marketplace |
> | Microsoft.AzureStack/registrations/products/read | Obtém as propriedades de um produto de pilha de Azure Marketplace |
> | Microsoft.AzureStack/registrations/read | Obtém as propriedades de um registo de pilha do Azure |

## <a name="backup-contributor"></a>Contribuidor de Cópia de Segurança
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir o serviço de cópia de segurança mas não pode criar cofres e conceder o acesso a outros |
> | **Id** | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Gerir os resultados da operação na gestão de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Criar e gerir contentores de cópia de segurança no interior de recursos de infraestrutura de cópia de segurança do Cofre de serviços de recuperação |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Criar e gerir tarefas de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Tarefas de exportação |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Criar e gerir os dados de metadados relacionados com a gestão de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Criar e gerir resultados de operações de gestão de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Criar e gerir políticas de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Criar e gerir itens que podem ser uma cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Criar e gerir itens de cópias de segurança |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Criar e gerir contentores que contém os itens de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Criar e gerir certificados relacionados com a cópia de segurança no Cofre de serviços de recuperação |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Criar e gerir informações expandidas relacionados com o Cofre |
> | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/* | Gerir a operação de deteção para obtenção recém-criado contentores |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Criar e gerir identidades registadas |
> | Microsoft.RecoveryServices/Vaults/usages/* | Criar e gerir a utilização do Cofre de serviços de recuperação |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="backup-operator"></a>Operador de Cópia de Segurança
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir serviços de cópia de segurança, exceto de remoção de cópia de segurança, criação de cofre e concessão de acesso a outros |
> | **Id** | 00c29273-979b-4161-815C-10b084fb9324 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Devolve o estado da operação |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Obtém o resultado da Operação efetuada no Contentor de Proteção. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Executa a Cópia de Segurança do Item Protegido. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Obtém o Resultado da Operação Efetuada nos Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Devolve o estado da Operação efetuada nos Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devolve detalhes do objecto do Item protegido |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Obter Pontos de Recuperação de Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Restaurar Pontos de Recuperação de Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Criar um Item protegido cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Devolve todas as registado contentores |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Criar e gerir tarefas de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Cancelar a tarefa |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Devolve o Resultado da Operação de Tarefa. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Devolve todos os objetos da tarefa |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Tarefas de exportação |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Devolve os Metadados da Gestão da Cópia de Segurança do Cofre de Serviços de Recuperação. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Criar e gerir resultados de operações de gestão de cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Obter Resultados da Operação de Política. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devolve todas as políticas de proteção |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Criar e gerir itens que podem ser uma cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Devolve a lista de todos os Itens Susceptíveis de Proteção. |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Devolve a lista de todos os Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Devolve todos os contentores que pertence à subscrição |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/* | Gerir a operação de deteção para obtenção recém-criado contentores |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | A operação de registar o contentor de serviço pode ser utilizada para registar um contentor com o serviço de recuperação. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Aprovisionar Item instantâneas recuperação para o Item protegido |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Revogar a recuperação do Item instantâneas para Item protegido |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
> | Microsoft.RecoveryServices/Vaults/certificates/write | A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="backup-reader"></a>Leitor de Cópia de Segurança
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode ver os serviços de cópia de segurança mas não pode efetuar alterações |
> | **Id** | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Devolve o estado da operação |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Obtém o resultado da Operação efetuada no Contentor de Proteção. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Obtém o Resultado da Operação Efetuada nos Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Devolve o estado da Operação efetuada nos Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devolve detalhes do objecto do Item protegido |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Devolve todas as registado contentores |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Devolve o Resultado da Operação de Tarefa. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Devolve todos os objetos da tarefa |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Tarefas de exportação |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Devolve os Metadados da Gestão da Cópia de Segurança do Cofre de Serviços de Recuperação. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Devolve o Resultado da Operação da Cópia de Segurança do Cofre de Serviços de Recuperação. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Obter Resultados da Operação de Política. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devolve todas as políticas de proteção |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Devolve a lista de todos os Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Devolve todos os contentores que pertence à subscrição |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Obter Pontos de Recuperação de Itens Protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |

## <a name="billing-reader"></a>Leitor de Faturação
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite o acesso de leitura para dados de faturação |
> | **Id** | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Billing/*/read | Ler as informações de faturação |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Management/managementGroups/read | Liste os grupos de gestão para o utilizador autenticado. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="biztalk-contributor"></a>Contribuinte do BizTalk
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir serviços do BizTalk, mas não aceder-lhes. |
> | **Id** | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.BizTalkServices/BizTalk/* | Criar e gerir serviços BizTalk |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-endpoint-contributor"></a>Contribuidor de Ponto Final de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode gerir pontos finais de CDN, as não pode conceder o acesso a outros utilizadores. |
> | **Id** | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-endpoint-reader"></a>Leitor do Ponto Final de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode visualizar os pontos finais de CDN, mas não pode efetuar alterações. |
> | **Id** | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-profile-contributor"></a>Contribuidor de Perfil de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode gerir perfis de CDN e os respetivos pontos finais, mas não pode conceder o acesso a outros utilizadores. |
> | **Id** | ec156ff8-a8d1-4d15-830C-5b80698ca432 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cdn-profile-reader"></a>Leitor de Perfil de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode visualizar perfis de CDN e os respetivos pontos finais, mas não pode efetuar alterações. |
> | **Id** | 8f96442b-4075-438f-813d-ad51ab4019af |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-network-contributor"></a>Contribuinte de Rede Clássica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir redes virtuais, mas não aceder-lhes. |
> | **Id** | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.ClassicNetwork/* | Criar e gerir redes clássicos |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-storage-account-contributor"></a>Contribuinte de Conta de Armazenamento Clássica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir contas de armazenamento clássico, mas não aceder às mesmas. |
> | **Id** | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.ClassicStorage/storageAccounts/* | Criar e gerir contas de armazenamento |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="classic-storage-account-key-operator-service-role"></a>Função do Serviço de Operador de Chave da Conta de Armazenamento Clássica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Os Operadores de Chave da Conta de Armazenamento Clássica têm permissão para listar e regenerar chaves em Contas de Armazenamento Clássicas |
> | **Id** | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | **Ações** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Lista as chaves de acesso para as contas de armazenamento. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Volta a gerar as chaves de acesso existentes para a conta de armazenamento. |

## <a name="classic-virtual-machine-contributor"></a>Contribuinte de Máquina Virtual Clássica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir máquinas virtuais clássicas, mas não aceder-lhes, além de que não pode gerir a rede virtual ou conta de armazenamento às quais estão ligadas. |
> | **Id** | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.ClassicCompute/domainNames/* | Criar e gerir nomes de domínio de computação clássicos |
> | Microsoft.ClassicCompute/virtualMachines/* | Criar e gerir máquinas virtuais |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Ligar um IP reservado |
> | Microsoft.ClassicNetwork/reservedIps/read | Obtém os IPs reservados |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Associa à rede virtual. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Obtém a rede virtual. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Devolve o disco de conta de armazenamento. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Devolve a imagem de conta de armazenamento. |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista as chaves de acesso para as contas de armazenamento. |
> | Microsoft.ClassicStorage/storageAccounts/read | Devolve a conta de armazenamento com a conta fornecida. |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="cleardb-mysql-db-contributor"></a>Contribuinte de Base de Dados MySQL ClearDB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir bases de dados MySQL ClearDB, mas não aceder-lhes. |
> | **Id** | 9106cda0-8a86-4E81-b686-29a22c54effe |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | successbricks.cleardb/databases/* | Criar e gerir bases de dados ClearDB MySQL |

## <a name="cosmos-db-account-reader-role"></a>Função do Leitor da Conta do Cosmos DB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode ler os dados de conta de base de dados do Azure Cosmos. Consulte [contribuinte de conta do DocumentDB](#documentdb-account-contributor) para gerir contas de base de dados do Azure Cosmos. |
> | **Id** | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de funções, pode permissões de leitura para cada utilizador |
> | Microsoft.DocumentDB/*/read | Uma coleção de leitura |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Lê a base de dados de chaves de conta de só de leitura. |
> | Microsoft.Insights/MetricDefinitions/read | Ler definições de métricas |
> | Microsoft.Insights/Metrics/read | Ler métricas |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="data-factory-contributor"></a>Contribuinte do Data Factory
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Criar e gerir fábricas de dados, assim como os recursos subordinados dentro dos mesmos. |
> | **Id** | 673868aa-7521-48A0-acc6-0f60742d39f5 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.DataFactory/dataFactories/* | Criar e gerir as fábricas de dados e recursos subordinados dentro delas. |
> | Microsoft.DataFactory/factories/* | Criar e gerir as fábricas de dados e recursos subordinados dentro delas. |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="data-lake-analytics-developer"></a>Programador do Data Lake Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe submeter, monitorizar e gerir as suas tarefas, mas não criar ou eliminar as contas do Data Lake Analytics. |
> | **Id** | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | Elimine uma conta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Conceder permissões para cancelar as tarefas submetidas por outros utilizadores. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Criar ou atualizar uma conta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Criar ou atualizar uma conta de DataLakeStore ligada de uma conta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Desassociar uma conta de DataLakeStore de uma conta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Criar ou atualizar uma conta de armazenamento ligada de uma conta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Desassociar uma conta de armazenamento de uma conta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Criar ou atualizar uma regra de firewall. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Elimine uma regra de firewall. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Criar ou atualizar uma política de computação. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Elimine uma política de computação. |

## <a name="data-purger"></a>Purger de dados
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode remover os dados de análise |
> | **Id** | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | **Ações** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action |  |
> | Microsoft.OperationalInsights/workspaces/*/read |  |
> | Microsoft.OperationalInsights/workspaces/purge/action | Eliminar dados especificado a partir da área de trabalho |

## <a name="devtest-labs-user"></a>Utilizador do DevTest Labs
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe ligar, iniciar, reiniciar e encerrar as máquinas virtuais no Azure DevTest Labs. |
> | **Id** | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Compute/availabilitySets/read | Obter as propriedades de um conjunto de disponibilidade |
> | Microsoft.Compute/virtualMachines/*/read | Ler as propriedades de uma máquina virtual (tamanhos de VM, estado de runtime, extensões VM, etc.) |
> | Microsoft.Compute/virtualMachines/deallocate/action | Desliga a máquina virtual e liberta os recursos de computação |
> | Microsoft.Compute/virtualMachines/read | Obter as propriedades de uma máquina virtual |
> | Microsoft.Compute/virtualMachines/restart/action | Reinicia a máquina virtual |
> | Microsoft.Compute/virtualMachines/start/action | Inicia a máquina virtual |
> | Microsoft.DevTestLab/*/read | Ler as propriedades de um laboratório |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Crie máquinas virtuais no laboratório. |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Uma máquina virtual claimable aleatória no laboratório de afirmações. |
> | Microsoft.DevTestLab/labs/formulas/delete | Elimine as fórmulas. |
> | Microsoft.DevTestLab/labs/formulas/read | Ler as fórmulas. |
> | Microsoft.DevTestLab/labs/formulas/write | Adiciona ou modifica as fórmulas. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Avalia a política de laboratório. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Assumir a propriedade de uma máquina virtual existente |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de Balanceador de carga |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Associa uma regra de nat de entrada do Balanceador de carga |
> | Microsoft.Network/networkInterfaces/*/read | Ler as propriedades de uma interface de rede (por exemplo, todos os balanceadores de carga que a interface de rede faz parte de) |
> | Microsoft.Network/networkInterfaces/join/action | Associa uma Máquina Virtual a uma interface de rede |
> | Microsoft.Network/networkInterfaces/read | Obtém uma definição de interface de rede.  |
> | Microsoft.Network/networkInterfaces/write | Cria uma interface de rede ou atualiza uma interface de rede existente.  |
> | Microsoft.Network/publicIPAddresses/*/read | Ler as propriedades de um endereço IP público |
> | Microsoft.Network/publicIPAddresses/join/action | Associa um endereço ip público |
> | Microsoft.Network/publicIPAddresses/read | Obtém uma definição de endereço ip público. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Associa uma rede virtual |
> | Microsoft.Resources/deployments/operations/read | Obtém ou lista as operações de implementação. |
> | Microsoft.Resources/deployments/read | Obtém ou lista implementações. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Lista os tamanhos disponíveis para os quais a máquina virtual pode ser atualizada |

## <a name="dns-zone-contributor"></a>Contribuidor da Zona DNS
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir zonas DNS e conjuntos de registos no DNS do Azure, mas não lhe permite controlar quem tem acesso aos mesmos. |
> | **Id** | befefa01-2a29-4197-83a8-272ff33ce314 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.Network/dnsZones/* | Criar e gerir registos e zonas DNS |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="documentdb-account-contributor"></a>Contribuidor de Conta do DocumentDB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode gerir contas de base de dados do Azure Cosmos. BD do Azure do Cosmos anteriormente é conhecido como DocumentDB. |
> | **Id** | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.DocumentDb/databaseAccounts/* | Criar e gerir contas de base de dados do Azure Cosmos |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="intelligent-systems-account-contributor"></a>Contribuinte de Conta de Sistemas Inteligentes
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir contas de Sistemas Inteligentes, mas não aceder-lhes. |
> | **Id** | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.IntelligentSystems/accounts/* | Criar e gerir contas de sistemas inteligentes |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="key-vault-contributor"></a>Contribuidor do Key Vault
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir cofres de chaves, mas não o acesso aos mesmos. |
> | **Id** | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Remover um cofre de chaves eliminado de forma recuperável |
> | Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>Criador do Laboratório
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe criar, gerir e eliminar os seus laboratórios geridos nas suas Contas do Azure Lab. |
> | **Id** | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Crie um laboratório numa conta de laboratório. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="log-analytics-contributor"></a>Contribuidor do Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Contribuinte de análise do registo pode ler todos os dados de monitorização e editar as definições de monitorização. Editar definições de monitorização inclui a adicionar a extensão VM para VMs; ler as chaves de conta de armazenamento para poder configurar a recolha de registos do armazenamento do Azure; criar e configurar as contas de automatização; Adicionar soluções; e configurar o diagnóstico do Azure em todos os recursos do Azure. |
> | **Id** | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | **Ações** |  |
> | * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista as chaves de acesso para as contas de armazenamento. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Insights/diagnosticSettings/* | Cria, atualiza ou lê a definição de diagnóstico para o servidor de Analysis Services |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="log-analytics-reader"></a>Leitor do Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | O Leitor do Log Analytics pode visualizar e procurar todos os dados de monitorização assim como visualizar as definições de monitorização, incluindo a visualização da configuração de diagnósticos Azure em todos os recursos do Azure. |
> | **Id** | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Ações** |  |
> | * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | A procura utilizando o motor de novo. |
> | Microsoft.OperationalInsights/workspaces/search/action | Executa uma consulta de pesquisa |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |

## <a name="logic-app-contributor"></a>Contribuidor da Aplicação Lógica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir aplicações lógicas, mas não aceder às mesmas. |
> | **Id** | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista as chaves de acesso para as contas de armazenamento. |
> | Microsoft.ClassicStorage/storageAccounts/read | Devolve a conta de armazenamento com a conta fornecida. |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Insights/diagnosticSettings/* | Cria, atualiza ou lê a definição de diagnóstico para o servidor de Analysis Services |
> | Microsoft.Insights/logdefinitions/* | Esta permissão é necessária para os utilizadores que necessitam de aceder a registos de atividade através do portal. Lista categorias de registo no registo de atividade. |
> | Microsoft.Insights/metricDefinitions/* | Ler as definições de métrica (lista de tipos de métricas disponíveis para um recurso). |
> | Microsoft.Logic/* | Gere os recursos de Logic Apps. |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/operationresults/read | Obter os resultados da operação de subscrição. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
> | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | Microsoft.Web/connectionGateways/* | Criar e gere um Gateway de ligação. |
> | Microsoft.Web/connections/* | Criar e gere uma ligação. |
> | Microsoft.Web/customApis/* | Cria e gere uma API personalizada. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Obter as propriedades num plano do App Service |
> | Microsoft.Web/sites/functions/listSecrets/action | Lista os segredos Web Apps funções. |

## <a name="logic-app-operator"></a>Operador de Aplicação Lógica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe ler, ativar e desativar a aplicação lógica. |
> | **Id** | 515c2055-d9d4-4321-B1B9-bd0c9a0f79fe |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/*/read | Regras de alertas das informações de leitura |
> | Microsoft.Insights/diagnosticSettings/*/read | Obtém as definições de diagnóstico para Logic Apps |
> | Microsoft.Insights/metricDefinitions/*/read | Obtem as métricas disponíveis para aplicações lógicas. |
> | Microsoft.Logic/*/read | Lê recursos Logic Apps. |
> | Microsoft.Logic/workflows/disable/action | Desativa o fluxo de trabalho. |
> | Microsoft.Logic/workflows/enable/action | Ativa o fluxo de trabalho. |
> | Microsoft.Logic/workflows/validate/action | Valida o fluxo de trabalho. |
> | Microsoft.Resources/deployments/operations/read | Obtém ou lista as operações de implementação. |
> | Microsoft.Resources/subscriptions/operationresults/read | Obter os resultados da operação de subscrição. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | Microsoft.Web/connectionGateways/*/read | Ler ligação Gateways. |
> | Microsoft.Web/connections/*/read | Ligações de leitura. |
> | Microsoft.Web/customApis/*/read | API personalizada de leitura. |
> | Microsoft.Web/serverFarms/read | Obter as propriedades num plano do App Service |

## <a name="managed-identity-contributor"></a>Contribuidor de Identidade Gerido
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Criar, Ler, Atualizar e Eliminar a Identidade Atribuída ao Utilizador |
> | **Id** | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | **Ações** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="managed-identity-operator"></a>Operador de Identidade Gerido
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Ler e Atribuir a Identidade Atribuída ao Utilizador |
> | **Id** | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Ações** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="monitoring-contributor"></a>Contribuidor de Monitorização
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode ler todos os dados de monitorização e editar as definições de monitorização. Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | **Ações** |  |
> | * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/AlertRules/* | Regras de alertas de leitura/escrita/eliminar. |
> | Microsoft.Insights/components/* | Componentes do Application Insights de leitura/escrita/eliminar. |
> | Microsoft.Insights/DiagnosticSettings/* | Definições de diagnóstico de leitura/escrita/eliminar. |
> | Microsoft.Insights/eventtypes/* | Lista de eventos de registo de atividade (eventos de gestão) numa subscrição. Esta permissão é aplicável ao acesso programático e portal de registo de atividade. |
> | Microsoft.Insights/LogDefinitions/* | Esta permissão é necessária para os utilizadores que necessitam de aceder a registos de atividade através do portal. Lista categorias de registo no registo de atividade. |
> | Microsoft.Insights/MetricDefinitions/* | Ler as definições de métrica (lista de tipos de métricas disponíveis para um recurso). |
> | Microsoft.Insights/Metrics/* | Ler métricas para um recurso. |
> | Microsoft.Insights/Register/Action | Registar o fornecedor de microsoft insights |
> | Microsoft.Insights/webtests/* | Testes web do Application Insights de leitura/escrita/eliminar. |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Pacotes de solução de análise de registos de leitura/escrita/eliminar. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Análise de registos de leitura/escrita/eliminar pesquisas guardadas. |
> | Microsoft.OperationalInsights/workspaces/search/action | Executa uma consulta de pesquisa |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Configurações de conhecimentos aprofundados de armazenamento de análise de registos de leitura/escrita/eliminar. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>Leitor de Monitorização
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Pode ler todos os dados de monitorização (métricas, os registos, etc.). Consulte também [introdução à segurança com a monitorização do Azure, funções e permissões](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | **Ações** |  |
> | * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
> | Microsoft.OperationalInsights/workspaces/search/action | Executa uma consulta de pesquisa |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="network-contributor"></a>Contribuinte de Rede
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir redes, mas não aceder-lhes. |
> | **Id** | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.Network/* | Criar e gerir redes |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="new-relic-apm-account-contributor"></a>Contribuidor de Conta APM do New Relic
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir contas e aplicações do New Relic Application Performance Management, mas não aceder-lhes. |
> | **Id** | 5d28c62d-5b37-4476-8438-e587778df237 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | NewRelic.APM/accounts/* |  |

## <a name="reader-and-data-access"></a>Acesso de Dados e Leitor
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite ver tudo mas não permitirá eliminar ou criar uma conta de armazenamento ou recurso contido. Também permitirá o acesso de leitura/escrita para todos os dados contidos numa conta do storage através de acesso às chaves de conta de armazenamento. |
> | **Id** | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | **Ações** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
> | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |

## <a name="redis-cache-contributor"></a>Contribuinte de Cache de Redis
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir caches de Redis, mas não aceder-lhes. |
> | **Id** | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Cache/redis/* | Criar e gerir caches de Redis |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="resource-policy-contributor-preview"></a>Contribuinte da Política de Recursos (pré-visualização)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | (Pré-visualização) Os utilizadores substituídos de EA, com direitos para criar/modificar a política de recurso, criam pedidos de suporte e leem recursos/hierarquia. |
> | **Id** | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | **Ações** |  |
> | * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
> | Microsoft.Authorization/policyassignments/* | Criar e gerir atribuições de políticas |
> | Microsoft.Authorization/policydefinitions/* | Criar e gerir definições de política |
> | Microsoft.Authorization/policysetdefinitions/* | Criar e gerir conjuntos de política |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="scheduler-job-collections-contributor"></a>Contribuinte de Coleções de Tarefas do Agendador
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir coleções de tarefas do Scheduler, mas não aceder-lhes. |
> | **Id** | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Scheduler/jobcollections/* | Criar e gerir coleções de tarefas |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="search-service-contributor"></a>Contribuinte de Serviços de Pesquisa
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir serviços de Pesquisa, mas não aceder-lhes. |
> | **Id** | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Search/searchServices/* | Criar e gerir serviços de pesquisa |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="security-admin"></a>Administrador de Segurança
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | No Centro de segurança apenas: pode ver as políticas de segurança, ver Estados de segurança, editar as políticas de segurança, ver alertas e as recomendações, dispensar alertas e recomendações |
> | **Id** | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Authorization/policyAssignments/* | Criar e gerir atribuições de políticas |
> | Microsoft.Authorization/policyDefinitions/* | Criar e gerir definições de política |
> | Microsoft.Authorization/policySetDefinitions/* | Criar e gerir conjuntos de política |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.operationalInsights/workspaces/*/read | Ver os dados de análise de registos |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Security/*/read | Componentes de segurança de leitura e políticas |
> | Microsoft.Security/locations/alerts/dismiss/action | Dispensar um alerta de segurança |
> | Microsoft.Security/locations/alerts/activate/action | Ativar um alerta de segurança |
> | Microsoft.Security/locations/tasks/dismiss/action | Ignorar uma recomendação de segurança |
> | Microsoft.Security/locations/tasks/activate/action | Ativar uma recomendação de segurança |
> | Microsoft.Security/policies/write | Atualiza a política de segurança |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="security-manager-legacy"></a>Gestor de Segurança (Legado)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Esta é uma função de legado. Em vez disso, utilize o administrador de segurança |
> | **Id** | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.ClassicCompute/*/read | Ler as informações de configuração as máquinas virtuais clássicas |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Escrita de configuração para as máquinas virtuais clássicas |
> | Microsoft.ClassicNetwork/*/read | Ler as informações de configuração sobre a rede clássico |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Security/* | Criar e gerir os componentes de segurança e políticas |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="security-reader"></a>Leitor de Segurança
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | No Centro de segurança apenas: pode ver as recomendações e alertas, vista de políticas de segurança, ver Estados de segurança, mas não é possível efetuar alterações |
> | **Id** | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **Ações** |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.operationalInsights/workspaces/*/read | Ver os dados de análise de registos |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Security/*/read | Componentes de segurança de leitura e políticas |

## <a name="site-recovery-contributor"></a>Contribuidor do Site Recovery
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir o serviço do Site Recovery exceto a criação do cofre e atribuição de funções |
> | **Id** | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/Vaults/certificates/write | A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Criar e gerir informações expandidas relacionados com o Cofre |
> | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Criar e gerir identidades registadas |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Criar ou atualizar definições de alerta de replicação |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler quaisquer eventos |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Criar e gerir recursos de infraestrutura de replicação |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Criar e gerir tarefas de replicação |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Criar e gerir políticas de replicação |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Criar e gerir planos de recuperação |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Criar e gerir a configuração de armazenamento do Cofre de serviços de recuperação |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Devolve informações sobre o token para o Cofre de serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Alertas de leitura para o Cofre de serviços de recuperação |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="site-recovery-operator"></a>Operador do Site Recovery
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe efetuar a ativação pós-falha mas não efetuar outras operações de gestão do Site Recovery |
> | **Id** | 494ae006-db33-4328-bf46-533a6560a3ca |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Ler as definições de alertas |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler quaisquer eventos |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Verifica a Consistência dos Recursos de infraestrutura |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Ler todos os recursos de infraestrutura |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Reassociar Gateway |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Renovar o certificado para recursos de infraestrutura |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Ler a quaisquer redes |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Leia os mapeamentos de rede |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Leu os contentores de proteção |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Ler quaisquer itens passíveis de proteção |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Aplicar o ponto de recuperação |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Consolidação de ativação pós-falha |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Ativação pós-falha planeada |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Ler os itens protegidos |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Pontos de recuperação de replicação de leitura |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Reparar a replicação |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Proteja o Item protegido |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Ativação Pós-falha de Teste |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Limpeza da ativação pós-falha de teste |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Ativação pós-falha |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Serviço de mobilidade de atualização |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Ler quaisquer mapeamentos de contentor de proteção |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Ler quaisquer fornecedores de serviços de recuperação |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Atualize o fornecedor |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Ler as classificações de armazenamento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Ler quaisquer mapeamentos de classificação de armazenamento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Ler todas as tarefas |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Criar e gerir tarefas de replicação |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Ler todas as políticas |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Plano de recuperação de consolidação de ativação pós-falha |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Plano de recuperação de ativação pós-falha planeada |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Leia os planos de recuperação |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Proteja o plano de recuperação |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Plano de recuperação de ativação pós-falha de teste |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Plano de recuperação de limpeza da ativação pós-falha de teste |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Plano de recuperação de ativação pós-falha |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Alertas de leitura para o Cofre de serviços de recuperação |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Devolve informações sobre o token para o Cofre de serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="site-recovery-reader"></a>Leitor do Site Recovery
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe ver o estado do Site Recovery mas não efetuar outras operações de gestão |
> | **Id** | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Ler as definições de alertas |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler quaisquer eventos |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Ler todos os recursos de infraestrutura |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Ler a quaisquer redes |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Leia os mapeamentos de rede |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Leu os contentores de proteção |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Ler quaisquer itens passíveis de proteção |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Ler os itens protegidos |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Pontos de recuperação de replicação de leitura |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Ler quaisquer mapeamentos de contentor de proteção |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Ler quaisquer fornecedores de serviços de recuperação |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Ler as classificações de armazenamento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Ler quaisquer mapeamentos de classificação de armazenamento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Ler todas as tarefas |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Ler todas as tarefas |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Ler todas as políticas |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Leia os planos de recuperação |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Devolve informações sobre o token para o Cofre de serviços de recuperação. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="sql-db-contributor"></a>Contribuinte de Base de Dados SQL
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir bases de dados SQL, mas não aceder aos mesmos. Além disso, não é possível gerir as respetivas políticas relacionadas com segurança ou os servidores do SQL Server principal. |
> | **Id** | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | Criar e gerir bases de dados SQL |
> | Microsoft.Sql/servers/read | Devolva a lista de servidores ou obtém as propriedades para o servidor especificado. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | **NotActions** |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Não é possível editar as políticas de auditoria |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Não é possível editar as definições de auditoria |
> | Microsoft.Sql/servers/databases/auditRecords/read | Obter os registos de auditoria de blob de base de dados |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Não é possível editar as políticas de ligação |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Não é possível editar as políticas de máscara de dados |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Não é possível editar as políticas de alerta de segurança |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Não é possível editar as métricas de segurança |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>Gestor de Segurança SQL
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite gerir as políticas de servidores e bases de dados SQL relacionadas com a segurança, mas não aceder às mesmas. |
> | **Id** | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura Microsoft |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Sql/servers/auditingPolicies/* | Criar e gerir políticas de auditoria do SQL server |
> | Microsoft.Sql/servers/auditingSettings/* | Criar e gerir a definição de auditoria do servidor SQL |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Criar e gerir políticas de auditoria de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Criar e gerir definições de auditoria de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/auditRecords/read | Registos de auditoria de leitura |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Criar e gerir políticas de ligação de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Criar e gerir políticas de máscara de dados do SQL server da base de dados |
> | Microsoft.Sql/servers/databases/read | Devolva a lista de bases de dados ou obtém as propriedades da base de dados especificado. |
> | Microsoft.Sql/servers/databases/schemas/read | Obter a lista de esquemas de uma base de dados |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Obter a lista de colunas de uma tabela |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Obter a lista de tabelas de uma base de dados |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Criar e gerir políticas alertas de segurança da base de dados de servidor SQL |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Criar e gerir as métricas de segurança de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Devolva a lista de servidores ou obtém as propriedades para o servidor especificado. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Criar e gerir políticas de alerta de segurança do SQL server |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="sql-server-contributor"></a>Contribuinte do SQL Server
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite gerir servidores e bases de dados SQL, mas não aceder aos mesmos nem às respetivas políticas relacionadas com a segurança. |
> | **Id** | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Criar e gerir servidores SQL |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | **NotActions** |  |
> | Microsoft.Sql/servers/auditingPolicies/* | Não é possível editar as políticas de auditoria do SQL server |
> | Microsoft.Sql/servers/auditingSettings/* | Não é possível editar definições de auditoria do SQL server |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Não é possível editar as políticas de auditoria de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Não é possível editar definições de auditoria de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/auditRecords/read | Não é possível ler os registos de auditoria |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Não é possível editar as políticas de ligação de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Não é possível editar as políticas de máscara de dados do SQL server da base de dados |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Não é possível editar o SQL server da base de dados alerta as políticas de segurança |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Não é possível editar as métricas de segurança de base de dados do SQL server |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Não é possível editar as políticas de alerta de segurança do SQL server |

## <a name="storage-account-contributor"></a>Contribuinte de Conta de Armazenamento
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir contas de armazenamento, mas não aceder às mesmas. |
> | **Id** | 17d1049b-9a84-46FB-8f53-869881c3d3ab |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Ler todas as autorização |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Insights/diagnosticSettings/* | Gerir definições de diagnóstico |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/* | Criar e gerir contas de armazenamento |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="storage-account-key-operator-service-role"></a>Função do Serviço de Operador de Chave da Conta de Armazenamento
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Os Operadores de Chave da Conta de Armazenamento têm permissão para listar e regenerar chaves em Contas de Armazenamento |
> | **Id** | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | **Ações** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Regenera as chaves de acesso da conta de armazenamento especificada. |

## <a name="storage-blob-data-contributor-preview"></a>Contribuinte de Dados do Armazenamento de Blobs (Pré-visualização)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite acesso de leitura, escrita e eliminação a dados e contentores de blobs de Armazenamento do Azure |
> | **Id** | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | **Ações** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Devolve o resultado da eliminação de um contentor |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Devolve um contentor ou uma lista de contentores |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Devolve o resultado do contentor de blobs colocados ou de concessão |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Devolve o resultado da eliminação de um blob |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Devolve um blob ou uma lista de blobs |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Devolve o resultado de escrever um blob |

## <a name="storage-blob-data-reader-preview"></a>Leitor de Dados do Armazenamento de Blobs (Pré-visualização)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite o acesso de leitura de dados e contentores de blobs de Armazenamento do Azure. |
> | **Id** | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | **Ações** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Devolve um contentor ou uma lista de contentores |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Devolve um blob ou uma lista de blobs |

## <a name="storage-queue-data-contributor-preview"></a>Contribuinte de Dados do Armazenamento de Filas (Pré-visualização)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite acesso de leitura, escrita e eliminação a filas e mensagens de filas de Armazenamento do Azure |
> | **Id** | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | **Ações** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Devolve o resultado da eliminação de uma fila |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Devolve uma fila ou uma lista de filas. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | Devolve o resultado da escrita de uma fila |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Devolve o resultado da eliminação de uma mensagem |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Devolve uma mensagem |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Devolve o resultado de escrever uma mensagem |

## <a name="storage-queue-data-reader-preview"></a>Leitor de Dados do Armazenamento de Filas (Pré-visualização)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite acesso de leitura às filas e mensagens de fila de Armazenamento do Azure |
> | **Id** | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Ações** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Devolve uma fila ou uma lista de filas. |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Devolve uma mensagem |

## <a name="support-request-contributor"></a>Contribuidor de Pedido de Suporte
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe criar e gerir os pedidos de Suporte |
> | **Id** | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="traffic-manager-contributor"></a>Contribuidor do Gestor de Tráfego
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir perfis do Gestor de Tráfego, mas não lhe permite controlar quem tem acesso aos mesmos. |
> | **Id** | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Funções de leitura e atribuições de função |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="user-access-administrator"></a>Administrador de Acesso de Utilizador
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir o acesso de utilizador aos recursos do Azure. |
> | **Id** | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Ações** |  |
> | * / leitura | Ler os recursos de todos os tipos, exceto os segredos. |
> | Microsoft.Authorization/* | Gerir autorização |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="virtual-machine-administrator-login"></a>Início de Sessão de Administrador na Máquina Virtual
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | -  Os utilizadores com esta função têm a capacidade de iniciar sessão numa máquina virtual com privilégios de administrador do Windows ou utilizador de raiz do Linux. |
> | **Id** | 1c0163c0-47E6-4577-8991-ea5c82e286e4 |
> | **Ações** |  |
> | Microsoft.Network/publicIPAddresses/read | Obtém uma definição de endereço ip público. |
> | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Microsoft.Network/loadBalancers/read | Obtém uma definição de Balanceador de carga |
> | Microsoft.Network/networkInterfaces/read | Obtém uma definição de interface de rede.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Iniciar sessão numa máquina virtual como um utilizador normal |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Inicie sessão para uma máquina virtual com o administrador do Windows ou privilégios de utilizador de raiz do Linux |

## <a name="virtual-machine-contributor"></a>Contribuinte de Máquina Virtual
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir máquinas virtuais, mas não aceder aos mesmos e não a rede virtual ou ligados a conta do storage. |
> | **Id** | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.Compute/availabilitySets/* | Criar e gerir conjuntos de disponibilidade de computação |
> | Microsoft.Compute/locations/* | Criar e gerir as localizações de computação |
> | Microsoft.Compute/virtualMachines/* | Criar e gerir máquinas virtuais |
> | Microsoft.Compute/virtualMachineScaleSets/* | Criar e gerir conjuntos de dimensionamento de máquina virtual |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de gateway de aplicação |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de Balanceador de carga |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Associa um balanceador de carga conjunto nat de entrada |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Associa uma regra de nat de entrada do Balanceador de carga |
> | Microsoft.Network/loadBalancers/probes/join/action | Permite utilizar as pesquisas de um balanceador de carga. Por exemplo, com esta propriedade de healthProbe de permissão de dimensionamento VM conjunto pode referenciar a pesquisa. |
> | Microsoft.Network/loadBalancers/read | Obtém uma definição de Balanceador de carga |
> | Microsoft.Network/locations/* | Criar e gerir as localizações de rede |
> | Microsoft.Network/networkInterfaces/* | Criar e gerir as interfaces de rede |
> | Microsoft.Network/networkSecurityGroups/join/action | Associa um grupo de segurança de rede |
> | Microsoft.Network/networkSecurityGroups/read | Obtém uma definição de grupo de segurança de rede |
> | Microsoft.Network/publicIPAddresses/join/action | Associa um endereço ip público |
> | Microsoft.Network/publicIPAddresses/read | Obtém uma definição de endereço ip público. |
> | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Associa uma rede virtual |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devolve detalhes do objecto do Item protegido |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Criar um Item protegido cópia de segurança |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Criar uma cópia de segurança intenção de proteção |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devolve todas as políticas de proteção |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Cria a política de proteção |
> | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
> | Microsoft.RecoveryServices/Vaults/write | A operação Criar Cofre cria um recurso do tipo "cofre" do Azure |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
> | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |

## <a name="virtual-machine-user-login"></a>Início de Sessão de Utilizador de Máquina Virtual
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Os utilizadores com esta função têm a capacidade de iniciar sessão numa máquina virtual como utilizador normal. |
> | **Id** | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Ações** |  |
> | Microsoft.Network/publicIPAddresses/read | Obtém uma definição de endereço ip público. |
> | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Microsoft.Network/loadBalancers/read | Obtém uma definição de Balanceador de carga |
> | Microsoft.Network/networkInterfaces/read | Obtém uma definição de interface de rede.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Iniciar sessão numa máquina virtual como um utilizador normal |

## <a name="web-plan-contributor"></a>Contribuinte de Plano Web
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir planos Web para sites, mas não aceder-lhes. |
> | **Id** | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | Microsoft.Web/serverFarms/* | Criar e gerir farms de servidores |

## <a name="website-contributor"></a>Contribuinte de Web site
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descrição** | Permite-lhe gerir sites (não planos Web), mas não aceder-lhes. |
> | **Id** | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Ações** |  |
> | Microsoft.Authorization/*/read | Autorização de leitura |
> | Microsoft.Insights/alertRules/* | Criar e gerir regras de alerta Insights |
> | Microsoft.Insights/components/* | Criar e gerir Insights componentes |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Microsoft.Resources/deployments/* | Criar e gerir implementações do grupo de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Microsoft.Support/* | Criar e gerir pedidos de suporte |
> | Microsoft.Web/certificates/* | Criar e gerir certificados de Web site |
> | Microsoft.Web/listSitesAssignedToHostName/read | Obter os nomes dos sites atribuídos ao nome do anfitrião. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Obter as propriedades num plano do App Service |
> | Microsoft.Web/sites/* | Criar e gerir sites (criação de site também necessita de permissões de escrita para o plano de serviço de aplicação associado) |

## <a name="next-steps"></a>Passos Seguintes

- [Funções personalizadas](custom-roles.md)
- [Gerir as atribuições de funções no portal do Azure](role-assignments-portal.md)
- [Permissões no Centro de segurança do Azure](../security-center/security-center-permissions.md)
