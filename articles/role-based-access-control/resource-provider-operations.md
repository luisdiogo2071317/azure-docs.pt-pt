---
title: Operações de fornecedor de recursos do Azure Resource Manager | Microsoft Docs
description: Lista as operações disponíveis para os fornecedores de recursos do Microsoft Azure Resource Manager
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/28/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: df999ce08785ce2c3954d18f3b05eb82bbbc5c97
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116416"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Operações de fornecedor de recursos do Azure Resource Manager

Este artigo apresenta as operações disponíveis para cada fornecedor de recursos do Azure Resource Manager. Estas operações podem ser utilizadas em [funções personalizadas](custom-roles.md) para fornecer granulares [controlo de acesso baseado em funções (RBAC)](overview.md) a recursos no Azure. Cadeias de operação de ter o seguinte formato: `{Company}.{ProviderName}/{resourceType}/{action}`

As operações de fornecedor de recursos são sempre evolução. Para obter as operações mais recentes, utilize [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) ou [a lista de operações de fornecedor de az](/cli/azure/provider/operation#az-provider-operation-list).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.AAD/domainServices/delete | Elimina os serviços de domínio. |
> | Ação | Microsoft.AAD/domainServices/read | Lê os serviços de domínio. |
> | Ação | Microsoft.AAD/domainServices/write | Escrever os serviços de domínio |
> | Ação | Microsoft.AAD/locations/operationresults/read |  |
> | Ação | Microsoft.AAD/Operations/read |  |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.aadiam/diagnosticsettings/DELETE | Eliminar uma definição de diagnóstico |
> | Ação | Microsoft.aadiam/diagnosticsettings/Read | Ler a definição de diagnóstico |
> | Ação | Microsoft.aadiam/diagnosticsettings/Write | Escrever uma definição de diagnóstico |
> | Ação | Microsoft.aadiam/diagnosticsettingscategories/Read | Ler um categorias de definição de diagnóstico |
> | Ação | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | microsoft.aadiam/tenants/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para os inquilinos |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Addons/operations/read | Obtém suportadas operações RP. |
> | Ação | Microsoft.Addons/register/action | Registar a subscrição especificada Microsoft.Addons |
> | Ação | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Lista as informações de plano de suporte para a subscrição especificada. |
> | Ação | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Remove o plano de suporte canónico especificado |
> | Ação | Microsoft.Addons/supportProviders/supportPlanTypes/read | Obter o estado do plano de suporte canónico especificado. |
> | Ação | Microsoft.Addons/supportProviders/supportPlanTypes/write | Adiciona o tipo do plano de suporte canónico especificado. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ADHybridHealthService/addsservices/action | Crie uma nova floresta para o inquilino. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Obtém todos os servidores para o nome do serviço especificado. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/alerts/read | Obtém os detalhes de alertas para a data de floresta como alertid, alertas gerada, última detetada, alerta descrição do alerta, mais recente atualizado, alerta nível, alerta Estado, alerta de resolução de problemas de ligações, etc. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/configuration/read | Obtém a configuração do serviço para a floresta. Exemplo - nome de floresta, Functionla nível, função FSMO de mestre de nomenclatura domínio, a função FSMO do mestre de esquema etc. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/delete | Elimina um serviço e -lo de servidores, juntamente com dados de estado de funcionamento. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Obtém os detalhes de domínios e de sites para a floresta. Estado de funcionamento de exemplo, alertas ativos, resolver alertas, propriedades, como o nível funcional do domínio, a floresta, a mestre de infraestrutura, PDC, etc mestre de RID.  |
> | Ação | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Obtém a definição de preferência de utilizador para a floresta.<br>Exemplo - MetricCounterName ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes, ldapsearches.<br>Definições para o etc de gráficos de IU. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Obtém floresta resumo da floresta especificado como o nome de floresta, número de domínios nesta floresta, o número de sites e detalhes de sites etc. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Obtém a lista de métricas suportadas para um determinado serviço.<br>Para o exemplo Extranet bloqueios de conta, Total de pedidos falhados, pedidos de Token pendentes (Proxy), por segundo de pedidos de Token etc para o serviço ADFS.<br>NTLM autenticações/seg, com êxito LDAP vincula/seg, tempo de enlace de LDAP, Threads ativos de LDAP, autenticações de Kerberos/seg, ATQ etc Total de Threads para ADDomainService.<br>Execute a latência de perfil, estabelecer ligações de TCP, Bytes privados do agente de Insights, exportar estatísticas para o Azure AD para o serviço ADSync. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Tendo em conta um serviço, esta API obtém as informações de métricas.<br>Por exemplo, esta API pode ser utilizada para obter informações relacionadas com: Extranet bloqueios de conta, Total de pedidos falhados, pedidos de Token pendentes (Proxy), por segundo de pedidos de Token etc ADFederation serviço.<br>NTLM autenticações/seg, LDAP bem-sucedida vincula/seg, tempo de enlace de LDAP, Threads ativos de LDAP, as autenticações de Kerberos/seg, ATQ etc Total de Threads do serviço de ADDomain.<br>Execute a latência de perfil, ligações de TCP estabelecidas, o Insights privada do agente Bytes, exporte estatísticas para o Azure AD para o serviço de sincronização. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Esta API obtém a lista de todos os integrado ADDomainServices para um inquilino premium. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/read | Obtém os detalhes do serviço para o nome do serviço especificado. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Obtém os detalhes de replicação para todos os servidores para o nome do serviço especificado. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Obtém o número de controladores de domínio e os erros de replicação se aplicável. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Obtém concluir a lista de controlador de domínio, juntamente com os detalhes de replicação para a floresta especificada. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Adicione uma instância de servidor para o serviço. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Durante o registo do servidor de ADDomainService, esta api denomina-se para obter as credenciais para a integração novos servidores. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Elimina um servidor para um determinado serviço e inquilino. |
> | Ação | Microsoft.ADHybridHealthService/addsservices/write | Cria ou atualiza a instância de ADDomainService para o inquilino. |
> | Ação | Microsoft.ADHybridHealthService/configuration/action | Configuração de inquilinos de atualizações. |
> | Ação | Microsoft.ADHybridHealthService/configuration/read | Lê a configuração de inquilino. |
> | Ação | Microsoft.ADHybridHealthService/configuration/write | Cria uma configuração de inquilino. |
> | Ação | Microsoft.ADHybridHealthService/logs/contents/read | Obtém o conteúdo de instalação do agente e os registos de registo armazenados no blob. |
> | Ação | Microsoft.ADHybridHealthService/logs/read | Obtém a instalação do agente e ficheiros de registo para o inquilino. |
> | Ação | Microsoft.ADHybridHealthService/operations/read | Obtém a lista de operações suportada pelo sistema. |
> | Ação | Microsoft.ADHybridHealthService/register/action | Regista o fornecedor de recursos do serviço de estado de funcionamento ADHybrid e permite a criação do recurso de serviço de integridade ADHybrid. |
> | Ação | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Obtém a lista de regiões disponíveis, utilizado por DevOps para suportar a incidentes de cliente. |
> | Ação | Microsoft.ADHybridHealthService/reports/badpassword/read | Obtém a lista de tentativas de palavra-passe incorreta para todos os utilizadores no serviço de Federação do Active Directory. |
> | Ação | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Obtém o URI de SAS do Blob que contém a Estado e eventual resultado recentemente tarefa de relatório colocados em fila para a frequência de nome de utilizador/palavra-passe incorreta tenta por UserId por IPAddress por dia para um determinado inquilino. |
> | Ação | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Obtém a lista de DevOps autorizado aos inquilinos. Normalmente utilizado para o suporte ao cliente. |
> | Ação | Microsoft.ADHybridHealthService/reports/isdevops/read | Obtém um valor que indica wheather o teannt é DevOps autorizado ou não. |
> | Ação | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Atualizações userid(objectid) para o inquilino de ops dev selecionado. |
> | Ação | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Obtém a implementação selecionada para o inquilino especificado. |
> | Ação | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Recebe um id de inquilino obtém a localização de armazenamento de inquilino. |
> | Ação | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Obtém a geolocalização partir do qual serão possível aceder aos dados. |
> | Ação | Microsoft.ADHybridHealthService/services/action | As atualizações de uma instância de serviço no inquilino. |
> | Ação | Microsoft.ADHybridHealthService/services/alerts/read | Lê os alertas para um serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/alerts/read | Lê os alertas para um serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | Fornecida uma funcionalidade nome verifica se um serviço tem tudo necessários para utilizar essa funcionalidade. |
> | Ação | Microsoft.ADHybridHealthService/services/delete | Elimina uma instância de serviço no inquilino. |
> | Ação | Microsoft.ADHybridHealthService/services/exporterrors/read | Obtém os erros de exportação de um determinado serviço de sincronização. |
> | Ação | Microsoft.ADHybridHealthService/services/exportstatus/read | Obtém o estado de exportação para um determinado serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Obtém a comentários de alertas para um determinado serviço e o servidor. |
> | Ação | Microsoft.ADHybridHealthService/services/metricmetadata/read | Obtém a lista de métricas suportadas para um determinado serviço.<br>Para o exemplo Extranet bloqueios de conta, Total de pedidos falhados, pedidos de Token pendentes (Proxy), por segundo de pedidos de Token etc para o serviço ADFS.<br>NTLM autenticações/seg, com êxito LDAP vincula/seg, tempo de enlace de LDAP, Threads ativos de LDAP, autenticações de Kerberos/seg, ATQ etc Total de Threads para ADDomainService.<br>Execute a latência de perfil, estabelecer ligações de TCP, Bytes privados do agente de Insights, exportar estatísticas para o Azure AD para o serviço ADSync. |
> | Ação | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Tendo em conta um serviço, esta API obtém a média de métricas para um determinado serviço.<br>Por exemplo, esta API pode ser utilizada para obter informações relacionadas com: Extranet bloqueios de conta, Total de pedidos falhados, pedidos de Token pendentes (Proxy), por segundo de pedidos de Token etc ADFederation serviço.<br>NTLM autenticações/seg, LDAP bem-sucedida vincula/seg, tempo de enlace de LDAP, Threads ativos de LDAP, as autenticações de Kerberos/seg, ATQ etc Total de Threads do serviço de ADDomain.<br>Execute a latência de perfil, ligações de TCP estabelecidas, o Insights privada do agente Bytes, exporte estatísticas para o Azure AD para o serviço de sincronização. |
> | Ação | Microsoft.ADHybridHealthService/services/metrics/groups/read | Tendo em conta um serviço, esta API obtém as informações de métricas.<br>Por exemplo, esta API pode ser utilizada para obter informações relacionadas com: Extranet bloqueios de conta, Total de pedidos falhados, pedidos de Token pendentes (Proxy), por segundo de pedidos de Token etc ADFederation serviço.<br>NTLM autenticações/seg, LDAP bem-sucedida vincula/seg, tempo de enlace de LDAP, Threads ativos de LDAP, as autenticações de Kerberos/seg, ATQ etc Total de Threads do serviço de ADDomain.<br>Execute a latência de perfil, ligações de TCP estabelecidas, o Insights privada do agente Bytes, exporte estatísticas para o Azure AD para o serviço de sincronização. |
> | Ação | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Tendo em conta um serviço, esta API obtém a vista de agregados de métricas para um determinado serviço.<br>Por exemplo, esta API pode ser utilizada para obter informações relacionadas com: Extranet bloqueios de conta, Total de pedidos falhados, pedidos de Token pendentes (Proxy), por segundo de pedidos de Token etc ADFederation serviço.<br>NTLM autenticações/seg, LDAP bem-sucedida vincula/seg, tempo de enlace de LDAP, Threads ativos de LDAP, as autenticações de Kerberos/seg, ATQ etc Total de Threads do serviço de ADDomain.<br>Execute a latência de perfil, ligações de TCP estabelecidas, o Insights privada do agente Bytes, exporte estatísticas para o Azure AD para o serviço de sincronização. |
> | Ação | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Adicionar ou atualiza a configuração de monitorização para um serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Obtém as configurações de monitorização para um determinado serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Adicionar ou configurações de monitorização para um serviço de atualizações. |
> | Ação | Microsoft.ADHybridHealthService/services/premiumcheck/read | Esta API obtém a lista de todos os serviços de integrado para um inquilino premium. |
> | Ação | Microsoft.ADHybridHealthService/services/read | Lê as instâncias do serviço no inquilino. |
> | Ação | Microsoft.ADHybridHealthService/services/reports/details/read | Obtém relatório dos principais 50 os utilizadores com erros de palavra-passe incorreta dos últimos 7 dias |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/action | Cria uma instância de servidor no serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Lê os alertas para um servidor. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Durante o registo de servidor, esta api denomina-se para obter as credenciais para a integração novos servidores. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Para um determinado servidor, esta API obtém uma lista dos tipos de dados que estão a ser carregados por servidores e a hora mais recente para cada carregamento. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/delete | Elimina uma instância de servidor no serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Obtém os detalhes do erro de exportação de sincronização para um determinado serviço de sincronização. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Tendo em conta um serviço, esta API obtém as informações de métricas.<br>Por exemplo, esta API pode ser utilizada para obter informações relacionadas com: Extranet bloqueios de conta, Total de pedidos falhados, pedidos de Token pendentes (Proxy), por segundo de pedidos de Token etc ADFederation serviço.<br>NTLM autenticações/seg, LDAP bem-sucedida vincula/seg, tempo de enlace de LDAP, Threads ativos de LDAP, as autenticações de Kerberos/seg, ATQ etc Total de Threads do serviço de ADDomain.<br>Execute a latência de perfil, ligações de TCP estabelecidas, o Insights privada do agente Bytes, exporte estatísticas para o Azure AD para o serviço de sincronização. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/read | Lê a instância de servidor no serviço. |
> | Ação | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Obtém a configuração do serviço para um determinado inquilino. |
> | Ação | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Obtém o estado de listas brancas de funcionalidade de um determinado inquilino. |
> | Ação | Microsoft.ADHybridHealthService/services/write | Cria uma instância de serviço no inquilino. |
> | Ação | Microsoft.ADHybridHealthService/unregister/action | Anula o registo da subscrição para o fornecedor de recursos de serviço de estado de funcionamento de ADHybrid. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Advisor/configurations/read | Obter configurações |
> | Ação | Microsoft.Advisor/configurations/write | Configuração de cria/atualizações |
> | Ação | Microsoft.Advisor/generateRecommendations/action | Gera recomendações |
> | Ação | Microsoft.Advisor/generateRecommendations/read | Obtém gerar Estado recomendações |
> | Ação | Microsoft.Advisor/operations/read | Obtém as operações do Microsoft Advisor |
> | Ação | Microsoft.Advisor/recommendations/read | Recomendações de leituras |
> | Ação | Microsoft.Advisor/recommendations/suppressions/delete | Elimina a supressão |
> | Ação | Microsoft.Advisor/recommendations/suppressions/read | Obtém suppressions |
> | Ação | Microsoft.Advisor/recommendations/suppressions/write | Cria/atualizações suppressions |
> | Ação | Microsoft.Advisor/register/action | Regista a subscrição para a Microsoft Advisor |
> | Ação | Microsoft.Advisor/suppressions/delete | Elimina a supressão |
> | Ação | Microsoft.Advisor/suppressions/read | Obtém suppressions |
> | Ação | Microsoft.Advisor/suppressions/write | Cria/atualizações suppressions |
> | Ação | Microsoft.Advisor/unregister/action | Anula o registo da subscrição para a Microsoft Advisor |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.AlertsManagement/alerts/changestate/action | Altere o estado do alerta. |
> | Ação | Microsoft.AlertsManagement/alerts/read | Obter todos os alertas para os filtros de entrada. |
> | Ação | Microsoft.AlertsManagement/alertsSummary/read | Obter o resumo dos alertas |
> | Ação | Microsoft.AlertsManagement/Operations/read | Lê as operações fornecidas |
> | Ação | Microsoft.AlertsManagement/smartGroups/changestate/action | Alterar o estado do grupo inteligente |
> | Ação | Microsoft.AlertsManagement/smartGroups/read | Obter todos os grupos de inteligentes para os filtros de entrada |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Verificações de que nome de servidor Analysis Services é válido e não em utilização. |
> | Ação | Microsoft.AnalysisServices/locations/operationresults/read | Obtém as informações do resultado da operação especificada. |
> | Ação | Microsoft.AnalysisServices/locations/operationstatuses/read | Obtém as informações do Estado da operação especificada. |
> | Ação | Microsoft.AnalysisServices/operations/read | Obtém as informações de operações |
> | Ação | Microsoft.AnalysisServices/register/action | Regista o fornecedor de recursos do Analysis Services. |
> | Ação | Microsoft.AnalysisServices/servers/delete | Elimina o servidor de Analysis Services. |
> | Ação | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Lista o estado do gateway associado ao servidor. |
> | Ação | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico para o servidor de Analysis Services |
> | Ação | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o servidor de Analysis Services |
> | Ação | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para servidores |
> | Ação | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o servidor de Analysis Services |
> | Ação | Microsoft.AnalysisServices/servers/read | Obtém as informações do servidor de Analysis Services especificada. |
> | Ação | Microsoft.AnalysisServices/servers/resume/action | Retoma o Analysis Server. |
> | Ação | Microsoft.AnalysisServices/servers/skus/read | Obter informações de SKU disponíveis para o servidor |
> | Ação | Microsoft.AnalysisServices/servers/suspend/action | Suspende o Analysis Server. |
> | Ação | Microsoft.AnalysisServices/servers/write | Cria ou atualiza o servidor de Analysis Services especificada. |
> | Ação | Microsoft.AnalysisServices/skus/read | Obtém as informações de Skus |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ApiManagement/checkNameAvailability/read | Verifica se o nome de serviço está disponível |
> | Ação | Microsoft.ApiManagement/operations/read | Ler todas as operações API disponíveis para o recurso de Microsoft.ApiManagement |
> | Ação | Microsoft.ApiManagement/register/action | Registar a subscrição para o fornecedor de recursos de Microsoft.ApiManagement |
> | Ação | Microsoft.ApiManagement/reports/read | Obter relatórios agregados pelo períodos de tempo, região geográfica, os programadores, produtos, APIs, operações, subscrição e byRequest. |
> | Ação | Microsoft.ApiManagement/service/apis/delete | Remover API existente |
> | Ação | Microsoft.ApiManagement/service/apis/diagnostics/delete | Remova o diagnóstico existente |
> | Ação | Microsoft.ApiManagement/service/apis/diagnostics/read | Obter a lista de diagnóstico ou Obtenha detalhes do diagnóstico |
> | Ação | Microsoft.ApiManagement/service/apis/diagnostics/write | Adicionar novo diagnóstico ou atualizar detalhes do diagnóstico existentes |
> | Ação | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Remove existente anexo |
> | Ação | Microsoft.ApiManagement/service/apis/issues/attachments/read | Detalhes do obter problema anexos ou problema de API Management obtém anexo |
> | Ação | Microsoft.ApiManagement/service/apis/issues/attachments/write | Adicionar anexos de problema de api |
> | Ação | Microsoft.ApiManagement/service/apis/issues/comments/delete | Remove existente comentário |
> | Ação | Microsoft.ApiManagement/service/apis/issues/comments/read | Obtém detalhes do problema comentários ou problema de API Management obtém comentário |
> | Ação | Microsoft.ApiManagement/service/apis/issues/comments/write | Adicione um comentário de problema de api |
> | Ação | Microsoft.ApiManagement/service/apis/issues/delete | Remove existente problema |
> | Ação | Microsoft.ApiManagement/service/apis/issues/read | Obter problemas associados ao API ou obtém a API Management detalhes do problema |
> | Ação | Microsoft.ApiManagement/service/apis/issues/write | Problema de api de adicionar ou atualizar o problema de api |
> | Ação | Microsoft.ApiManagement/service/apis/operations/delete | A operação de API existente de remoção |
> | Ação | Microsoft.ApiManagement/service/apis/operations/policies/delete | Remover configuração da política de políticas de operação de API |
> | Ação | Microsoft.ApiManagement/service/apis/operations/policies/read | Obter as políticas para a operação de API ou Obtenha detalhes de configuração de política para a operação de API |
> | Ação | Microsoft.ApiManagement/service/apis/operations/policies/write | Definir os detalhes de configuração de política para a operação de API |
> | Ação | Microsoft.ApiManagement/service/apis/operations/policy/delete | Remover configuração da política de operação |
> | Ação | Microsoft.ApiManagement/service/apis/operations/policy/read | Obter os detalhes de configuração de política para a operação |
> | Ação | Microsoft.ApiManagement/service/apis/operations/policy/write | Definir os detalhes da configuração de política para a operação |
> | Ação | Microsoft.ApiManagement/service/apis/operations/read | Obter a lista de operações de API existentes ou obter os detalhes da operação de API |
> | Ação | Microsoft.ApiManagement/service/apis/operations/tags/delete | Eliminar a associação da Tag existente com a operação existente |
> | Ação | Microsoft.ApiManagement/service/apis/operations/tags/read | Obter etiquetas associadas aos detalhes de operação ou obter etiqueta |
> | Ação | Microsoft.ApiManagement/service/apis/operations/tags/write | Associar etiqueta existente com a operação existente |
> | Ação | Microsoft.ApiManagement/service/apis/operations/write | Criar nova operação da API ou atualizar a operação de API existente |
> | Ação | Microsoft.ApiManagement/service/apis/operationsByTags/read | Obter a lista de associações de Tag/operação |
> | Ação | Microsoft.ApiManagement/service/apis/policies/delete | Remover configuração da política de políticas de API |
> | Ação | Microsoft.ApiManagement/service/apis/policies/read | Obter as políticas de API ou Get detalhes de configuração de políticas de API |
> | Ação | Microsoft.ApiManagement/service/apis/policies/write | Definir os detalhes de configuração de política de API |
> | Ação | Microsoft.ApiManagement/service/apis/policy/delete | Remover configuração da política da API |
> | Ação | Microsoft.ApiManagement/service/apis/policy/read | Obter os detalhes de configuração de políticas de API |
> | Ação | Microsoft.ApiManagement/service/apis/policy/write | Definir os detalhes de configuração de política de API |
> | Ação | Microsoft.ApiManagement/service/apis/products/read | Obter todos os produtos que a API faz parte |
> | Ação | Microsoft.ApiManagement/service/apis/read | Obter a lista de todos os registado APIs ou Get detalhes da API |
> | Ação | Microsoft.ApiManagement/service/apis/releases/delete | Remove todas as versões da versão de API ou remover API |
> | Ação | Microsoft.ApiManagement/service/apis/releases/read | Obter versões de uma API ou Get. Detalhes da API reelase |
> | Ação | Microsoft.ApiManagement/service/apis/releases/write | Criar nova versão de API ou atualizar a versão de API existente |
> | Ação | Microsoft.ApiManagement/service/apis/revisions/delete | Remove todas as revisões de uma API |
> | Ação | Microsoft.ApiManagement/service/apis/revisions/read | Obter revisões que pertencem a uma API |
> | Ação | Microsoft.ApiManagement/service/apis/schemas/delete | Remove existente esquema |
> | Ação | Microsoft.ApiManagement/service/apis/schemas/document/read | Obter o documento que descreve o esquema |
> | Ação | Microsoft.ApiManagement/service/apis/schemas/document/write | Atualizar o documento que descreve o esquema |
> | Ação | Microsoft.ApiManagement/service/apis/schemas/read | Obtém todos os esquemas de uma determinada API ou obtém os esquemas utilizados pela API |
> | Ação | Microsoft.ApiManagement/service/apis/schemas/write | Define os esquemas utilizados pela API |
> | Ação | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Remover a descrição da etiqueta da API |
> | Ação | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Obter as descrições de etiquetas no âmbito da API ou da Tag de obter descrição no âmbito da API |
> | Ação | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Descrição da etiqueta de criar/alterar no âmbito da API |
> | Ação | Microsoft.ApiManagement/service/apis/tags/delete | Remova a associação de API/etiqueta existente |
> | Ação | Microsoft.ApiManagement/service/apis/tags/read | Obter todas as API/Tag associação para os detalhes de API ou Get da associação de API/etiqueta |
> | Ação | Microsoft.ApiManagement/service/apis/tags/write | Adicionar nova associação de API/etiqueta |
> | Ação | Microsoft.ApiManagement/service/apis/write | Criar nova API ou atualizar os detalhes de API existentes |
> | Ação | Microsoft.ApiManagement/service/apisByTags/read | Obter a lista de associações/etiqueta de API |
> | Ação | Microsoft.ApiManagement/service/api-version-sets/delete | Remover VersionSet existente |
> | Ação | Microsoft.ApiManagement/service/api-version-sets/read | Obter a lista de entidades do grupo de versão ou obtém detalhes de um VersionSet |
> | Ação | Microsoft.ApiManagement/service/api-version-sets/versions/read | Obter a lista de entidades de versão |
> | Ação | Microsoft.ApiManagement/service/api-version-sets/write | Criar novo VersionSet ou atualizar detalhes do VersionSet existentes |
> | Ação | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Atualiza os recursos de Microsoft.ApiManagement em execução na rede Virtual para escolher as definições de rede atualizado. |
> | Ação | Microsoft.ApiManagement/service/authorizationServers/delete | Remover o servidor de autorização existente |
> | Ação | Microsoft.ApiManagement/service/authorizationServers/read | Obter a lista de servidores de autorização ou obter os detalhes do servidor de autorização |
> | Ação | Microsoft.ApiManagement/service/authorizationServers/write | Criar um novo servidor de autorização ou detalhes da atualização de um servidor de autorização existente |
> | Ação | Microsoft.ApiManagement/service/backends/delete | Remova o back-end existente |
> | Ação | Microsoft.ApiManagement/service/backends/read | Obter a lista de back-ends ou obter os detalhes de back-end |
> | Ação | Microsoft.ApiManagement/service/backends/reconnect/action | Criar um pedido de restabelecimento de ligação |
> | Ação | Microsoft.ApiManagement/service/backends/write | Adicionar um novo back-end ou atualizar detalhes do back-end existente |
> | Ação | Microsoft.ApiManagement/service/backup/action | Serviço de gestão de API a cópia de segurança para o contentor especificado um utilizador forneceu a conta de armazenamento |
> | Ação | Microsoft.ApiManagement/service/certificates/delete | Remover o certificado existente |
> | Ação | Microsoft.ApiManagement/service/certificates/read | Obter a lista de certificados ou obter os detalhes do certificado |
> | Ação | Microsoft.ApiManagement/service/certificates/write | Adicionar novo certificado |
> | Ação | Microsoft.ApiManagement/service/delete | Eliminar a instância de serviço de gestão de API |
> | Ação | Microsoft.ApiManagement/service/diagnostics/delete | Remova o diagnóstico existente |
> | Ação | Microsoft.ApiManagement/service/diagnostics/read | Obter a lista de diagnóstico ou Obtenha detalhes do diagnóstico |
> | Ação | Microsoft.ApiManagement/service/diagnostics/write | Adicionar novo diagnóstico ou atualizar detalhes do diagnóstico existentes |
> | Ação | Microsoft.ApiManagement/service/getssotoken/action | Token SSO obtém que pode ser utilizada para iniciar sessão no portal de legado de serviço de gestão de API como administrador |
> | Ação | Microsoft.ApiManagement/service/groups/delete | Remover grupo existente |
> | Ação | Microsoft.ApiManagement/service/groups/read | Obter a lista de grupos ou obtém detalhes de um grupo |
> | Ação | Microsoft.ApiManagement/service/groups/users/delete | Remover utilizador existente do grupo existente |
> | Ação | Microsoft.ApiManagement/service/groups/users/read | Obter a lista de utilizadores do grupo |
> | Ação | Microsoft.ApiManagement/service/groups/users/write | Adicionar utilizador existente ao grupo existente |
> | Ação | Microsoft.ApiManagement/service/groups/write | Criar novo grupo ou atualizar detalhes do grupo existente |
> | Ação | Microsoft.ApiManagement/service/identityProviders/delete | Remover o fornecedor de identidade existentes |
> | Ação | Microsoft.ApiManagement/service/identityProviders/read | Obter a lista de fornecedores de identidade ou Obtenha detalhes do fornecedor de identidade |
> | Ação | Microsoft.ApiManagement/service/identityProviders/write | Criar um novo fornecedor de identidade ou atualizar os detalhes de um fornecedor de identidade existentes |
> | Ação | Microsoft.ApiManagement/service/locations/networkstatus/read | Obtém o estado de acesso de rede de recursos no qual o serviço depende de na localização. |
> | Ação | Microsoft.ApiManagement/service/loggers/delete | Remover o registo existente |
> | Ação | Microsoft.ApiManagement/service/loggers/read | Obter a lista de registadores ou obter os detalhes de registo |
> | Ação | Microsoft.ApiManagement/service/loggers/write | Adicionar novo registador ou atualizar detalhes do registo existente |
> | Ação | Microsoft.ApiManagement/service/managedeployments/action | Alterar o SKU/unidades, adicionar/remover implementações regional do serviço de gestão de API |
> | Ação | Microsoft.ApiManagement/service/networkstatus/read | Obtém o estado de acesso de rede de recursos no qual o serviço depende. |
> | Ação | Microsoft.ApiManagement/service/notifications/action | Envia a notificação para um utilizador especificado |
> | Ação | Microsoft.ApiManagement/service/notifications/read | Obtém todas as notificações de publicador da API Management ou Editor de gestão de API de obter os detalhes de notificação |
> | Ação | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Remove existente associado uma notificação de E-Mail |
> | Ação | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Obter os destinatários de E-mail associado à notificação de editor de gestão de API |
> | Ação | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Criar novo E-Mail destinatário da notificação |
> | Ação | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Remove o utilizador associado para os destinatários de notificação |
> | Ação | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Obter os utilizadores de destinatário associados a notificação |
> | Ação | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Adicionar o utilizador para os destinatários de notificação |
> | Ação | Microsoft.ApiManagement/service/notifications/write | Criar ou notificação de editor de gestão de API de atualização |
> | Ação | Microsoft.ApiManagement/service/openidConnectProviders/delete | Remover existente OpenID Connect fornecedor |
> | Ação | Microsoft.ApiManagement/service/openidConnectProviders/read | Obter a lista de fornecedores de OpenID Connect ou Obtenha detalhes de OpenID Connect fornecedor |
> | Ação | Microsoft.ApiManagement/service/openidConnectProviders/write | Criar um novo OpenID Connect fornecedor ou atualizar os detalhes de um fornecedor de ligação de OpenID existente |
> | Ação | Microsoft.ApiManagement/service/operationresults/read | Obtém o estado atual da operação de execução longa |
> | Ação | Microsoft.ApiManagement/service/policies/delete | Remover configuração da política de políticas de inquilino |
> | Ação | Microsoft.ApiManagement/service/policies/read | Obter as políticas para detalhes de configuração de política do inquilino ou Get para o Tenant |
> | Ação | Microsoft.ApiManagement/service/policies/write | Definir os detalhes de configuração de política para o Tenant |
> | Ação | Microsoft.ApiManagement/service/policySnippets/read | Obter todos os fragmentos de política |
> | Ação | Microsoft.ApiManagement/service/portalsettings/read | Obter as definições de inscrição para o Portal ou Get iniciar sessão nas definições para o Portal ou obter definições de delegação para o Portal |
> | Ação | Microsoft.ApiManagement/service/portalsettings/write | Atualizar as definições de segurança de início de sessão ou as definições de atualização inscrever-se ou as definições de atualização de sessão ou as definições de atualização de sessão ou definições de delegação de atualização ou definições de delegação de atualização |
> | Ação | Microsoft.ApiManagement/service/products/apis/delete | Remover API existente de produto existente |
> | Ação | Microsoft.ApiManagement/service/products/apis/read | Obter a lista de APIs adicionadas ao produto existente |
> | Ação | Microsoft.ApiManagement/service/products/apis/write | Adicionar API existente ao produto existente |
> | Ação | Microsoft.ApiManagement/service/products/delete | Remova o produto existente |
> | Ação | Microsoft.ApiManagement/service/products/groups/delete | Eliminar a associação de grupo de programador existente com o produto existente |
> | Ação | Microsoft.ApiManagement/service/products/groups/read | Obter lista de grupos de programador associados ao produto |
> | Ação | Microsoft.ApiManagement/service/products/groups/write | Associar grupo existente do Programador de produto existente |
> | Ação | Microsoft.ApiManagement/service/products/policies/delete | Remover configuração da política de políticas de produto |
> | Ação | Microsoft.ApiManagement/service/products/policies/read | Obter as políticas para detalhes de configuração de política do produto ou Get para o produto |
> | Ação | Microsoft.ApiManagement/service/products/policies/write | Definir os detalhes de configuração de política para o produto |
> | Ação | Microsoft.ApiManagement/service/products/policy/delete | Remover configuração da política de produto existente |
> | Ação | Microsoft.ApiManagement/service/products/policy/read | Obter a configuração da política de produto existente |
> | Ação | Microsoft.ApiManagement/service/products/policy/write | Definir a configuração de política para o produto existente |
> | Ação | Microsoft.ApiManagement/service/products/read | Obter a lista de produtos ou obter os detalhes do produto |
> | Ação | Microsoft.ApiManagement/service/products/subscriptions/read | Obter a lista de subscrições do produto |
> | Ação | Microsoft.ApiManagement/service/products/tags/delete | Eliminar a associação de uma etiqueta existente com o produto existente |
> | Ação | Microsoft.ApiManagement/service/products/tags/read | Obter etiquetas associadas aos detalhes do produto ou obter etiqueta |
> | Ação | Microsoft.ApiManagement/service/products/tags/write | Associar etiqueta existente de produto existente |
> | Ação | Microsoft.ApiManagement/service/products/write | Criar novo produto ou atualizar detalhes do produto existente |
> | Ação | Microsoft.ApiManagement/service/productsByTags/read | Obter a lista de associações/etiqueta de produto |
> | Ação | Microsoft.ApiManagement/service/properties/delete | Remove existente propriedade |
> | Ação | Microsoft.ApiManagement/service/properties/read | Obtém a lista de todas as propriedades ou obtém os detalhes de propriedade especificado |
> | Ação | Microsoft.ApiManagement/service/properties/write | Cria uma nova propriedade ou atualiza o valor para a propriedade especificada |
> | Ação | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico para o serviço de API Management |
> | Ação | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o serviço de API Management |
> | Ação | Microsoft.ApiManagement/service/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para o serviço de API Management |
> | Ação | Microsoft.ApiManagement/service/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o serviço de API Management |
> | Ação | Microsoft.ApiManagement/service/quotas/periods/read | Obter o valor do contador de quota para período |
> | Ação | Microsoft.ApiManagement/service/quotas/periods/write | Defina o valor atual do contador de quota |
> | Ação | Microsoft.ApiManagement/service/quotas/read | Obter os valores de quota |
> | Ação | Microsoft.ApiManagement/service/quotas/write | Defina o valor atual do contador de quota |
> | Ação | Microsoft.ApiManagement/service/read | Ler os metadados para uma instância de serviço de gestão de API |
> | Ação | Microsoft.ApiManagement/service/reports/read | Obter o relatório agregado pelo períodos de tempo ou relatório Get agregados por região geográfica ou relatório Get agregado pelos programadores.<br>ou obter relatório agregado pelo produtos.<br>ou obter relatório agregado pelo APIs ou Get relatório agregado pelo operações ou relatório Get agregados por subscrição.<br>ou obter pedidos de dados de relatórios |
> | Ação | Microsoft.ApiManagement/service/restore/action | Restaurar o serviço de gestão de API a partir do contentor especificado um utilizador fornecida a conta de armazenamento |
> | Ação | Microsoft.ApiManagement/service/subscriptions/delete | Elimine a subscrição. Esta operação pode ser utilizada para eliminar a subscrição |
> | Ação | Microsoft.ApiManagement/service/subscriptions/read | Obter uma lista de subscrições do produto ou obter os detalhes da subscrição do produto |
> | Ação | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Regenerar a chave primária da subscrição |
> | Ação | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Regenerar a chave secundária da subscrição |
> | Ação | Microsoft.ApiManagement/service/subscriptions/write | Subscrever um utilizador existente para um produto existente ou atualizar os detalhes da subscrição existente. Esta operação pode ser utilizada para renovar a subscrição |
> | Ação | Microsoft.ApiManagement/service/tagResources/read | Obter a lista de etiquetas aos recursos associados |
> | Ação | Microsoft.ApiManagement/service/tags/delete | Remover etiqueta existente |
> | Ação | Microsoft.ApiManagement/service/tags/read | Obter a lista de etiquetas ou obter detalhes da etiqueta |
> | Ação | Microsoft.ApiManagement/service/tags/write | Adicionar nova etiqueta ou atualizar detalhes de etiqueta existentes |
> | Ação | Microsoft.ApiManagement/service/templates/delete | Repor o modelo de correio eletrónico de API de gestão predefinido |
> | Ação | Microsoft.ApiManagement/service/templates/read | Obtém todos os modelos de e-mail ou o e-mail de API Management obtém os detalhes de modelo |
> | Ação | Microsoft.ApiManagement/service/templates/write | Criar ou atualizar o modelo de correio eletrónico de API Management ou modelo de correio eletrónico de atualizações de API Management |
> | Ação | Microsoft.ApiManagement/service/tenant/delete | Remover configuração da política para o inquilino |
> | Ação | Microsoft.ApiManagement/service/tenant/deploy/action | Executa uma tarefa de implementação para aplicar as alterações a partir do ramo de git especificado para a configuração na base de dados. |
> | Ação | Microsoft.ApiManagement/service/tenant/operationResults/read | Obter a lista de resultados da operação ou obter Resultado de uma operação específica |
> | Ação | Microsoft.ApiManagement/service/tenant/read | Obter a configuração de política para o inquilino ou Get inquilino detalhes de informações de acesso |
> | Ação | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Regenerar a chave de acesso primária |
> | Ação | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Regenerar a chave de acesso secundária |
> | Ação | Microsoft.ApiManagement/service/tenant/save/action | Cria a consolidação com instantâneos de configuração para o ramo especificado no repositório |
> | Ação | Microsoft.ApiManagement/service/tenant/syncState/read | Obter o estado da última sincronização do git |
> | Ação | Microsoft.ApiManagement/service/tenant/validate/action | Valida as alterações do ramo do git especificado |
> | Ação | Microsoft.ApiManagement/service/tenant/write | Definir a configuração de política para o inquilino ou para detalhes de informação de acesso de inquilino de atualização |
> | Ação | Microsoft.ApiManagement/service/updatecertificate/action | Carregar o certificado SSL para um serviço de gestão de API |
> | Ação | Microsoft.ApiManagement/service/updatehostname/action | O programa de configuração, atualizar ou remover nomes de domínio personalizado para um serviço de gestão de API |
> | Ação | Microsoft.ApiManagement/service/users/action | Registar um novo utilizador |
> | Ação | Microsoft.ApiManagement/service/users/applications/attachments/delete | Remove um anexo |
> | Ação | Microsoft.ApiManagement/service/users/applications/attachments/read | Obtém os anexos de aplicação ou obtém anexo |
> | Ação | Microsoft.ApiManagement/service/users/applications/attachments/write | Adicionar anexo à aplicação |
> | Ação | Microsoft.ApiManagement/service/users/applications/delete | Remove existentes da aplicação |
> | Ação | Microsoft.ApiManagement/service/users/applications/read | Obter a lista de todas as aplicações de utilizador ou detalhes da aplicação obtém API Management |
> | Ação | Microsoft.ApiManagement/service/users/applications/write | Regista detalhes da aplicação API de gestão ou atualizações de uma aplicação |
> | Ação | Microsoft.ApiManagement/service/users/delete | Remover conta de utilizador |
> | Ação | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Gere o URL do SSO. O URL pode ser utilizado para aceder ao portal de administração |
> | Ação | Microsoft.ApiManagement/service/users/groups/read | Obter a lista de grupos de utilizadores |
> | Ação | Microsoft.ApiManagement/service/users/keys/read | Obter a lista de chaves de utilizador |
> | Ação | Microsoft.ApiManagement/service/users/read | Obter uma lista de utilizadores registados ou obter os detalhes da conta de utilizador |
> | Ação | Microsoft.ApiManagement/service/users/subscriptions/read | Obter a lista de subscrições do utilizador |
> | Ação | Microsoft.ApiManagement/service/users/token/action | Obter o token acesso token para um utilizador |
> | Ação | Microsoft.ApiManagement/service/users/write | Registar um novo utilizador ou detalhes da conta de atualização de um utilizador existente |
> | Ação | Microsoft.ApiManagement/service/write | Criar uma nova instância do serviço de gestão de API |
> | Ação | Microsoft.ApiManagement/unregister/action | Subscrição de anular o registo do fornecedor de recursos de Microsoft.ApiManagement |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Authorization/checkAccess/action | Verifica se o autor da chamada está autorizado a executar uma ação específica |
> | Ação | Microsoft.Authorization/classicAdministrators/delete | Remove o administrador da subscrição. |
> | Ação | Microsoft.Authorization/classicAdministrators/read | Lê os administradores da subscrição. |
> | Ação | Microsoft.Authorization/classicAdministrators/write | Adiciona ou modifica o administrador de uma subscrição. |
> | Ação | Microsoft.Authorization/elevateAccess/action | Garante ao chamador Administrador de Acesso dos Utilizadores acesso ao âmbito do inquilino |
> | Ação | Microsoft.Authorization/locks/delete | Elimina bloqueios no âmbito especificado. |
> | Ação | Microsoft.Authorization/locks/read | Obtém bloqueios no âmbito especificado. |
> | Ação | Microsoft.Authorization/locks/write | Adiciona bloqueios ao âmbito especificado. |
> | Ação | Microsoft.Authorization/permissions/read | Lista todas as permissões que o chamador tem num determinado âmbito. |
> | Ação | Microsoft.Authorization/policyAssignments/delete | Elimine uma atribuição de política no âmbito especificado. |
> | Ação | Microsoft.Authorization/policyAssignments/read | Obtenha informação sobre uma atribuição de política. |
> | Ação | Microsoft.Authorization/policyAssignments/write | Crie uma atribuição de política no âmbito especificado. |
> | Ação | Microsoft.Authorization/policyDefinitions/delete | Elimine uma definição de política. |
> | Ação | Microsoft.Authorization/policyDefinitions/read | Obtenha informação sobre uma definição de política. |
> | Ação | Microsoft.Authorization/policyDefinitions/write | Crie uma definição de política personalizada. |
> | Ação | Microsoft.Authorization/policySetDefinitions/delete | Elimine uma definição do conjunto de políticas. |
> | Ação | Microsoft.Authorization/policySetDefinitions/read | Obter informações sobre uma definição do conjunto de políticas. |
> | Ação | Microsoft.Authorization/policySetDefinitions/write | Crie uma definição do conjunto de políticas personalizada. |
> | Ação | Microsoft.Authorization/providerOperations/read | Obter operações para todos os fornecedores de recursos que possam ser utilizadas em definições de funções. |
> | Ação | Microsoft.Authorization/roleAssignments/delete | Elimina uma atribuição de função no âmbito especificado. |
> | Ação | Microsoft.Authorization/roleAssignments/read | Obtém informações sobre uma atribuição de função. |
> | Ação | Microsoft.Authorization/roleAssignments/write | Cria uma atribuição de função no âmbito especificado. |
> | Ação | Microsoft.Authorization/roleDefinitions/delete | Eliminar a definição de função personalizada especificada. |
> | Ação | Microsoft.Authorization/roleDefinitions/read | Obtém informação sobre uma definição de função. |
> | Ação | Microsoft.Authorization/roleDefinitions/write | Criar ou atualizar uma definição de função personalizada com permissões especificadas e âmbitos atribuíveis. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Ler as informações de registo do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Escreve um pedido para voltar a gerar chaves de DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/certificates/delete | Elimina um recurso de certificado da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/certificates/getCount/action | Lê a contagem de certificados |
> | Ação | Microsoft.Automation/automationAccounts/certificates/read | Obtém um recurso de certificado da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/certificates/write | Cria ou atualiza um recurso de certificado da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/compilationjobs/read | Lê a compilação do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/compilationjobs/read | Lê a compilação do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/compilationjobs/write | Escreve compilação do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/compilationjobs/write | Escreve compilação do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/configurations/content/read | Lê o conteúdo do suporte de dados de configuração |
> | Ação | Microsoft.Automation/automationAccounts/configurations/delete | Elimina o conteúdo do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/configurations/getCount/action | Lê a contagem de conteúdo do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/configurations/read | Obtém o conteúdo do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/configurations/write | Escreve o conteúdo do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/connections/delete | Elimina um recurso de ligação da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/connections/getCount/action | Lê o número de ligações |
> | Ação | Microsoft.Automation/automationAccounts/connections/read | Obtém um recurso de ligação da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/connections/write | Cria ou atualiza um recurso de ligação da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/connectionTypes/delete | Elimina um recurso de tipo de ligação de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/connectionTypes/read | Obtém um recurso de tipo de ligação de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/connectionTypes/write | Cria um recurso de tipo de ligação de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/credentials/delete | Elimina um recurso de credencial de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/credentials/getCount/action | Lê a contagem de credenciais |
> | Ação | Microsoft.Automation/automationAccounts/credentials/read | Obtém um recurso de credencial de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/credentials/write | Cria ou atualiza um recurso de credencial de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/delete | Elimina uma conta de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Automation/automationAccounts/diagnosticSettings/write | Define a definição de diagnóstico para o recurso |
> | Ação | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Elimina recursos de trabalho de Runbook híbrida |
> | Ação | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lê recursos de trabalho de Runbook híbrida |
> | Ação | Microsoft.Automation/automationAccounts/jobs/output/read | Obtém o resultado de uma tarefa |
> | Ação | Microsoft.Automation/automationAccounts/jobs/read | Obtém uma tarefa de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobs/resume/action | Retoma uma tarefa de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | Obtém o conteúdo do runbook da automatização do Azure no momento da execução da tarefa |
> | Ação | Microsoft.Automation/automationAccounts/jobs/stop/action | Para uma tarefa de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobs/streams/read | Obtém um fluxo de trabalho da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobs/streams/read | Obtém um fluxo de trabalho da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspende uma tarefa de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobs/write | Cria uma tarefa de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobSchedules/delete | Elimina uma tarefa da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobSchedules/read | Obtém uma tarefa da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/jobSchedules/write | Cria uma tarefa da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Obtém a área de trabalho associada à conta de automatização |
> | Ação | Microsoft.Automation/automationAccounts/listKeys/action | Lê as chaves da conta de automatização |
> | Ação | Microsoft.Automation/automationAccounts/logDefinitions/read | Obtém os registos disponíveis para a conta de automatização |
> | Ação | Microsoft.Automation/automationAccounts/modules/activities/read | Obtém as actividades de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/modules/delete | Elimina um módulo de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/modules/getCount/action | Obtém a contagem de módulos na conta de automatização |
> | Ação | Microsoft.Automation/automationAccounts/modules/read | Obtém um módulo de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/modules/write | Cria ou atualiza um módulo de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Elimina a configuração do nó do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Lê o conteúdo de configuração do nó do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Lê a configuração do nó do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Escreve a configuração do nó do DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodecounts/read | Lê o resumo de contagem de nós para o tipo especificado |
> | Ação | Microsoft.Automation/automationAccounts/nodes/delete | Elimina nós de DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodes/read | Lê nós de DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Lê os conteúdos do relatório de DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodes/reports/read | Lê os relatórios do Automation DSC do Azure |
> | Ação | Microsoft.Automation/automationAccounts/nodes/write | Cria ou atualiza nós de DSC de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Obtém TypeFields de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições de métrica de automatização |
> | Ação | Microsoft.Automation/automationAccounts/read | Obtém uma conta de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/content/read | Obtém o conteúdo de um runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/delete | Elimina um runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Cria o conteúdo de um rascunho de runbook da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Obtém os resultados da operação de rascunho de runbook da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/read | Obtém um rascunho de runbook da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Obtém uma tarefa de teste de rascunho de runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Retoma uma tarefa de teste de rascunho de runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Parar uma tarefa de teste de rascunho de runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Suspende uma tarefa de teste de rascunho de runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Cria uma tarefa de teste de rascunho de runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Anular edições de um rascunho de runbook da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Obtém a contagem de runbooks de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/publish/action | Publica um rascunho de runbook da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/read | Obtém um runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/runbooks/write | Cria ou atualiza um runbook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/schedules/delete | Elimina um recurso de agenda da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/schedules/getCount/action | Obtém a contagem de agendas da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/schedules/read | Obtém um recurso de agenda da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/schedules/write | Cria ou atualiza um recurso de agenda da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Elimina uma configuração de atualização de Software da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Obtém uma configuração de atualização de Software da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Cria ou atualiza a configuração de atualização de Software de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/statistics/read | Obtém estatísticas de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Obter uma máquina de implementação de atualização de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Obtém uma tarefa de patch de gestão de atualização de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/usages/read | Obtém a utilização da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/variables/delete | Elimina um recurso de variável da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/variables/read | Lê um recurso de variável da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/variables/write | Cria ou atualiza um recurso de variável da automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/watchers/delete | Eliminar uma tarefa de observador de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/watchers/read | Obtém uma tarefa de observador de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/watchers/start/action | Iniciar uma tarefa de observador de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/watchers/stop/action | Parar uma tarefa de observador de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/watchers/streams/read | Obtém um fluxo de tarefas do observador de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Eliminar um ações de tarefas do observador de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Obtém um observador da automatização do Azure ações de tarefas |
> | Ação | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Criar um observador da automatização do Azure ações de tarefas |
> | Ação | Microsoft.Automation/automationAccounts/watchers/write | Cria uma tarefa de observador de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/webhooks/action | Gera um URI para um webhook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/webhooks/delete | Elimina um webhook de automatização do Azure  |
> | Ação | Microsoft.Automation/automationAccounts/webhooks/read | Lê um webhook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/webhooks/write | Cria ou atualiza um webhook de automatização do Azure |
> | Ação | Microsoft.Automation/automationAccounts/write | Cria ou atualiza uma conta de automatização do Azure |
> | Ação | Microsoft.Automation/operations/read | Obtém as operações disponíveis para os recursos de automatização do Azure |
> | Ação | Microsoft.Automation/register/action | Regista a subscrição da automatização do Azure |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.AzureActiveDirectory/b2cDirectories/delete | Eliminar o recurso do Diretório B2C |
> | Ação | Microsoft.AzureActiveDirectory/b2cDirectories/read | Visualizar o recurso do Diretório B2C |
> | Ação | Microsoft.AzureActiveDirectory/b2cDirectories/write | Criar ou atualizar o recursos do Diretório B2C |
> | Ação | Microsoft.AzureActiveDirectory/operations/read | Ler todas as operações de API disponíveis para o fornecedor de recursos de Microsoft.AzureActiveDirectory |
> | Ação | Microsoft.AzureActiveDirectory/register/action | Registar subscrição para o fornecedor de recursos de Microsoft.AzureActiveDirectory |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.AzureStack/Operations/read | Obtém as propriedades de uma operação do fornecedor de recursos |
> | Ação | Microsoft.AzureStack/register/action | Regista a subscrição no fornecedor de recursos de Microsoft.AzureStack |
> | Ação | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Elimina uma subscrição do cliente de pilha do Azure |
> | Ação | Microsoft.AzureStack/registrations/customerSubscriptions/read | Obtém as propriedades de uma subscrição de cliente de pilha do Azure |
> | Ação | Microsoft.AzureStack/registrations/customerSubscriptions/write | Cria ou atualiza uma subscrição de cliente de pilha do Azure |
> | Ação | Microsoft.AzureStack/registrations/delete | Elimina um registo de pilha do Azure |
> | Ação | Microsoft.AzureStack/registrations/getActivationKey/action | Obtém a chave de ativação de pilha do Azure mais recente |
> | Ação | Microsoft.AzureStack/registrations/products/listDetails/action | Obtém expandido detalhes de um produto de pilha de Azure Marketplace |
> | Ação | Microsoft.AzureStack/registrations/products/read | Obtém as propriedades de um produto de pilha de Azure Marketplace |
> | Ação | Microsoft.AzureStack/registrations/read | Obtém as propriedades de um registo de pilha do Azure |
> | Ação | Microsoft.AzureStack/registrations/write | Cria ou atualiza um registo de pilha do Azure |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Batch/batchAccounts/applications/delete | Elimina uma aplicação |
> | Ação | Microsoft.Batch/batchAccounts/applications/read | Apresenta uma lista de aplicações ou obtém as propriedades de uma aplicação |
> | Ação | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Ativa um pacote de aplicação |
> | Ação | Microsoft.Batch/batchAccounts/applications/versions/delete | Elimina um pacote de aplicação |
> | Ação | Microsoft.Batch/batchAccounts/applications/versions/read | Obtém as propriedades de um pacote de aplicação |
> | Ação | Microsoft.Batch/batchAccounts/applications/versions/write | Cria um novo pacote de aplicação ou atualiza um pacote de aplicação existente |
> | Ação | Microsoft.Batch/batchAccounts/applications/write | Cria uma nova aplicação ou atualiza uma aplicação existente |
> | Ação | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Obtém os resultados de uma operação de certificado longa numa conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Cancela a falha na eliminação de um certificado numa conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/certificates/delete | Elimina um certificado de uma conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/certificates/read | Apresenta uma lista de certificados numa conta do Batch ou obtém as propriedades de um certificado |
> | Ação | Microsoft.Batch/batchAccounts/certificates/write | Cria um novo certificado numa conta do Batch ou atualiza um certificado existente |
> | Ação | Microsoft.Batch/batchAccounts/delete | Elimina uma conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/listkeys/action | Apresenta uma lista de aceder às chaves para uma conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/operationResults/read | Obtém os resultados de uma operação de conta de Batch longa |
> | Ação | Microsoft.Batch/batchAccounts/poolOperationResults/read | Obtém os resultados de uma operação de agrupamento de execução longa numa conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/pools/delete | Elimina um agrupamento de uma conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Desativa o dimensionamento automático para um conjunto de conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/pools/read | Apresenta uma lista de agrupamentos de uma conta do Batch ou obtém as propriedades de um agrupamento |
> | Ação | Microsoft.Batch/batchAccounts/pools/stopResize/action | Pára em curso uma redimensiona a operação de um conjunto de conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/pools/upgradeOs/action | Atualiza o sistema operativo de um conjunto de conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/pools/write | Cria um novo conjunto numa conta do Batch ou atualiza um conjunto existente |
> | Ação | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para o serviço Batch |
> | Ação | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o serviço Batch |
> | Ação | Microsoft.Batch/batchAccounts/read | Apresenta uma lista de contas do Batch ou obtém as propriedades de uma conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/regeneratekeys/action | Gera de novo a aceder às chaves para uma conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Sincroniza as chaves de acesso da conta do storage automaticamente configurado para uma conta do Batch |
> | Ação | Microsoft.Batch/batchAccounts/write | Cria uma nova conta do Batch ou atualiza uma conta de Batch existente |
> | Ação | Microsoft.Batch/locations/checkNameAvailability/action | Verifica se o nome da conta é válido e não em utilização. |
> | Ação | Microsoft.Batch/locations/quotas/read | Obtém as quotas de lote da subscrição especificada na região do Azure especificada |
> | Ação | Microsoft.Batch/register/action | Regista a subscrição para o fornecedor de recursos do Batch e permite a criação de contas do Batch |
> | Ação | Microsoft.Batch/unregister/action | Anula o registo da subscrição para o fornecedor de recursos do Batch que impede a criação de contas do Batch |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.BatchAI/clusters/read | Apresenta uma lista de clusters de AI do Batch ou obtém as propriedades de um cluster de AI do Batch |
> | Ação | Microsoft.BatchAI/fileservers/read | Apresenta uma lista de AI do Batch fileservers ou obtém as propriedades de um servidor de ficheiros Batch AI |
> | Ação | Microsoft.BatchAI/locations/operationresults/read | Obtém o resultado assíncrono AI do Batch na região do Azure especificada |
> | Ação | Microsoft.BatchAI/locations/operationstatuses/read | Obtém o estado da operação assíncrona AI do Batch na região do Azure especificada |
> | Ação | Microsoft.BatchAI/locations/usages/read | Obtém as utilizações de AI do lote da subscrição especificada na região do Azure especificada |
> | Ação | Microsoft.BatchAI/register/action | Regista a subscrição para o fornecedor de recursos do Batch AI e permite a criação dos recursos do Batch AI |
> | Ação | Microsoft.BatchAI/unregister/action | Anula o registo da subscrição para o fornecedor de recursos do Batch AI impedir a criação de recursos do Batch AI |
> | Ação | Microsoft.BatchAI/workspaces/clusters/delete | Elimina um cluster de AI do Batch |
> | Ação | Microsoft.BatchAI/workspaces/clusters/read | Apresenta uma lista de clusters de AI do Batch ou obtém as propriedades de um cluster de AI do Batch |
> | Ação | Microsoft.BatchAI/workspaces/clusters/remoteLoginInformation/action | Lista as informações de início de sessão remoto para um cluster de AI do Batch |
> | Ação | Microsoft.BatchAI/workspaces/clusters/write | Cria um novo cluster de AI do Batch ou atualiza um cluster existente de AI do Batch |
> | Ação | Microsoft.BatchAI/workspaces/delete | Elimina uma área de trabalho do Batch AI |
> | Ação | Microsoft.BatchAI/workspaces/experiments/delete | Elimina uma experimentação de AI do Batch |
> | Ação | Microsoft.BatchAI/workspaces/experiments/jobs/delete | Elimina uma tarefa de lote AI |
> | Ação | Microsoft.BatchAI/workspaces/experiments/jobs/listoutputfiles/action | Apresenta uma lista de ficheiros para uma tarefa de lote AI de saída |
> | Ação | Microsoft.BatchAI/workspaces/experiments/jobs/read | Lista as tarefas de lote AI ou obtém as propriedades de uma tarefa de lote AI |
> | Ação | Microsoft.BatchAI/workspaces/experiments/jobs/remoteLoginInformation/action | Lista as informações de início de sessão remoto para uma tarefa de lote AI |
> | Ação | Microsoft.BatchAI/workspaces/experiments/jobs/terminate/action | Termina uma tarefa de lote AI |
> | Ação | Microsoft.BatchAI/workspaces/experiments/jobs/write | Cria uma nova tarefa de lote AI ou atualiza uma tarefa de lote AI existente |
> | Ação | Microsoft.BatchAI/workspaces/experiments/read | Apresenta uma lista de Batch AI experimentações ou obtém as propriedades de uma experimentação de AI do Batch |
> | Ação | Microsoft.BatchAI/workspaces/experiments/write | Cria uma nova experimentação de AI do Batch ou atualiza uma experimentação de AI do Batch existente |
> | Ação | Microsoft.BatchAI/workspaces/fileservers/delete | Elimina um servidor de ficheiros Batch AI |
> | Ação | Microsoft.BatchAI/workspaces/fileservers/read | Apresenta uma lista de AI do Batch fileservers ou obtém as propriedades de um servidor de ficheiros Batch AI |
> | Ação | Microsoft.BatchAI/workspaces/fileservers/write | Cria um novo servidor de ficheiros de AI do Batch ou atualiza um servidor de ficheiros existente do Batch AI |
> | Ação | Microsoft.BatchAI/workspaces/read | Apresenta uma lista de áreas de trabalho do Batch AI ou obtém as propriedades de uma área de trabalho do Batch AI |
> | Ação | Microsoft.BatchAI/workspaces/write | Cria uma nova área de trabalho do Batch AI ou atualiza uma área de trabalho do Batch AI existente |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Billing/billingPeriods/read | Apresenta uma lista de períodos de faturação disponíveis |
> | Ação | Microsoft.Billing/invoices/read | Apresenta uma lista de faturas disponíveis |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.BingMaps/mapApis/Delete | Operação de Eliminação |
> | Ação | Microsoft.BingMaps/mapApis/listSecrets/action | Listar os Segredos |
> | Ação | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Ler Token de Autorização de Início de Sessão Único para o Recurso |
> | Ação | Microsoft.BingMaps/mapApis/Read | Operação de Leitura |
> | Ação | Microsoft.BingMaps/mapApis/regenerateKey/action | Regenera a Chave |
> | Ação | Microsoft.BingMaps/mapApis/Write | Operação de Escrita |
> | Ação | Microsoft.BingMaps/Operations/read | Descrição da operação. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Cache/checknameavailability/action | Verifica se um nome está disponível para utilização com uma nova Cache de Redis |
> | Ação | Microsoft.Cache/locations/operationresults/read | Obtém o resultado de uma operação de longa duração da qual o cabeçalho "Localização" foi devolvido anteriormente ao cliente |
> | Ação | Microsoft.Cache/operations/read | Lista as operações que o fornecedor "Microsoft.Cache" suporta. |
> | Ação | Microsoft.Cache/redis/delete | Eliminar a Cache de Redis completa |
> | Ação | Microsoft.Cache/redis/export/action | Exportar dados Redis para os blobs de armazenamento com prefixo no formato especificado |
> | Ação | Microsoft.Cache/redis/firewallRules/delete | Eliminar as regras de firewall do IP da Cache de Redis |
> | Ação | Microsoft.Cache/redis/firewallRules/read | Obter as regras de firewall do IP da Cache de Redis |
> | Ação | Microsoft.Cache/redis/firewallRules/write | Editar regras de firewall do IP de uma Cache de Redis |
> | Ação | Microsoft.Cache/redis/forceReboot/action | Forçar reinício de uma instância da cache, potencialmente com perda de dados. |
> | Ação | Microsoft.Cache/redis/import/action | Importar dados de um formato especificado a partir de vários blobs para Redis |
> | Ação | Microsoft.Cache/redis/linkedservers/delete | Eliminar Servidor Ligado de uma Cache de Redis |
> | Ação | Microsoft.Cache/redis/linkedservers/read | Obter Servidores Ligados associados a uma cache de redis. |
> | Ação | Microsoft.Cache/redis/linkedservers/write | Adicionar Servidor Ligado a uma Cache de Redis |
> | Ação | Microsoft.Cache/redis/listKeys/action | Ver o valor das chaves de acesso da Cache de Redis no portal de gestão |
> | Ação | Microsoft.Cache/redis/listUpgradeNotifications/read | Listar as Notificações de Atualização mais recentes para o inquilino da cache. |
> | Ação | Microsoft.Cache/redis/metricDefinitions/read | Obtem as métricas disponíveis para uma Cache de Redis |
> | Ação | Microsoft.Cache/redis/patchSchedules/delete | Eliminar a agenda de correção da Cache de Redis |
> | Ação | Microsoft.Cache/redis/patchSchedules/read | Obtém a agenda de correções de uma Cache de Redis |
> | Ação | Microsoft.Cache/redis/patchSchedules/write | Modificar a agenda de correções de uma Cache de Redis |
> | Ação | Microsoft.Cache/redis/read | Ver as definições e a configuração da Cache de Redis no portal de gestão |
> | Ação | Microsoft.Cache/redis/regenerateKey/action | Alterar o valor das chaves de acesso da Cache de Redis no portal de gestão |
> | Ação | Microsoft.Cache/redis/start/action | Iniciar uma instância de cache. |
> | Ação | Microsoft.Cache/redis/stop/action | Parar uma instância de cache. |
> | Ação | Microsoft.Cache/redis/write | Modificar as definições e a configuração da Cache de Redis no portal de gestão |
> | Ação | Microsoft.Cache/register/action | Regista o fornecedor de recursos "Microsoft.Cache" com uma subscrição |
> | Ação | Microsoft.Cache/unregister/action | Anula o registo do fornecedor de recursos "Microsoft.Cache" com uma subscrição |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Capacity/register/action | Regista o fornecedor de recursos de capacidade e permite a criação dos recursos de capacidade. |
> | Ação | Microsoft.Capacity/reservationorders/action | Atualizar qualquer reserva |
> | Ação | Microsoft.Capacity/reservationorders/delete | Eliminar reserva de qualquer |
> | Ação | Microsoft.Capacity/reservationorders/read | Ler todas as reservas |
> | Ação | Microsoft.Capacity/reservationorders/reservations/action | Atualizar qualquer reserva |
> | Ação | Microsoft.Capacity/reservationorders/reservations/delete | Eliminar reserva de qualquer |
> | Ação | Microsoft.Capacity/reservationorders/reservations/read | Ler todas as reservas |
> | Ação | Microsoft.Capacity/reservationorders/reservations/revisions/read | Ler todas as reservas |
> | Ação | Microsoft.Capacity/reservationorders/reservations/write | Criar reserva de qualquer |
> | Ação | Microsoft.Capacity/reservationorders/write | Criar reserva de qualquer |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Ação | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Ação | Microsoft.Cdn/edgenodes/delete |  |
> | Ação | Microsoft.Cdn/edgenodes/read |  |
> | Ação | Microsoft.Cdn/edgenodes/write |  |
> | Ação | Microsoft.Cdn/operationresults/delete |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Ação | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Ação | Microsoft.Cdn/operationresults/read |  |
> | Ação | Microsoft.Cdn/operationresults/write |  |
> | Ação | Microsoft.Cdn/operations/read |  |
> | Ação | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Ação | Microsoft.Cdn/profiles/delete |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico para o recurso |
> | Ação | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico para o recurso |
> | Ação | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para Microsoft.Cdn |
> | Ação | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/read |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Ação | Microsoft.Cdn/profiles/endpoints/write |  |
> | Ação | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Ação | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Ação | Microsoft.Cdn/profiles/read |  |
> | Ação | Microsoft.Cdn/profiles/write |  |
> | Ação | Microsoft.Cdn/register/action | Regista a subscrição para o fornecedor de recursos CDN e ativa a criação de perfis CDN. |
> | Ação | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Eliminar um certificado existente |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | Obter a lista de certificados |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Adicionar um certificado novo ou atualizar um já existente |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/Delete | Eliminar um AppServiceCertificate existente |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/Read | Obter a lista de CertificateOrders |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Volte a emitir um certificateorder existente |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Renovar um certificateorder existente |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Certificado de reenviar correio eletrónico |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Volte a enviar e-mails de pedido para outro endereço de e-mail |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Obter selar de site para um certificado de serviço de aplicações emitido |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Obter a lista de ações de certificado |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Obter o histórico de correio eletrónico de certificado |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Verificar a propriedade do domínio |
> | Ação | Microsoft.CertificateRegistration/certificateOrders/Write | Adicionar um novo certificateOrder ou atualizar um já existente |
> | Ação | Microsoft.CertificateRegistration/operations/Read | Lista todas as operações de registo de certificados do serviço de aplicações |
> | Ação | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Principal de serviço de aprovisionamento para o principal da aplicação de serviço |
> | Ação | Microsoft.CertificateRegistration/register/action | Registar o fornecedor de recursos Microsoft Certificates para a subscrição |
> | Ação | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Validar o objeto de compra do certificado sem submetê-la |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ClassicCompute/capabilities/read | Mostra as capacidades |
> | Ação | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Verifica a disponibilidade de um nome de domínio fornecido. |
> | Ação | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Verifica a disponibilidade de um nome de domínio específico. |
> | Ação | Microsoft.ClassicCompute/domainNames/active/write | Define o nome do domínio ativo. |
> | Ação | Microsoft.ClassicCompute/domainNames/availabilitySets/read | Mostra o conjunto de disponibilidade do recurso. |
> | Ação | Microsoft.ClassicCompute/domainNames/capabilities/read | Mostra as funções de nome de domínio |
> | Ação | Microsoft.ClassicCompute/domainNames/delete | Remove os nomes de domínio dos recursos. |
> | Ação | Microsoft.ClassicCompute/domainNames/deploymentslots/read | Mostra os blocos de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/deploymentslots/state/read | Obter o estado do bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/deploymentslots/state/write | Adicionar o estado do bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/deploymentslots/write | Cria ou atualiza a implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/extensions/delete | Remova as extensões do nome de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Lê o estado da operação das extensões de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/extensions/read | Devolve as extensões do nome de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/extensions/write | Adicione as extensões do nome de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Remove um novo balanceador de carga interno. |
> | Ação | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Lê o estado da operação dos balanceadores de carga internos de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | Obtém os balanceadores de carga internos. |
> | Ação | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Cria um novo balanceador de carga interno. |
> | Ação | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Lê o estado da operação dos conjuntos de pontos finais com balanceamento de carga de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Obter os conjuntos de pontos finais com balanceamento de carga. |
> | Ação | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | Adicionar o conjunto de pontos finais com balanceamento de carga. |
> | Ação | Microsoft.ClassicCompute/domainNames/operationstatuses/read | Obter o estado da operação do nome de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Lê o estado da operação das extensões de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/read | Devolve os nomes de domínio dos recursos. |
> | Ação | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | Elimina os certificados de serviço utilizados. |
> | Ação | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Lê o estado da operação dos certificados de serviço de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Devolve os certificados de serviço utilizados. |
> | Ação | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Adiciona ou modifica os certificados de serviço utilizados. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/abortMigration/action | Aborta a migração de um bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/commitMigration/action | Consolida a migração de uma ranhura de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/delete | Elimina um bloco de implementação fornecido. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Lê o estado da operação das ranhuras de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/prepareMigration/action | Prepara a migração de uma ranhura de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/read | Mostra os blocos de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Remova a referência da extensão para a função de bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Lê o estado da operação das referências de extensão de funções de ranhuras de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Devolve a referência da extensão para a função de bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Adicionar ou modificar a referência da extensão para a função de bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | Obter a definição de métrica de função para o nome de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | Obter métrica de função para o nome de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/operationstatuses/read | Obter o estado de operação para a função de bloco de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Adiciona ou modifica as definições de diagnóstico. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições das métricas. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/read | Obtém a função do bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Transfere o ficheiro de ligação de ambiente de trabalho remoto para a instância de função na função de bloco de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Obtém o estado de operação para a instância de função na função de bloco de nomes de domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Obtém a instância de função. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Recria a instância de função. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Recria a imagem da instância de função. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Reinicia instâncias de função. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/skus/read | Obter o SKU de função para o bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/roles/write | Adicionar função para bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/start/action | Inicia um bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Altera o estado do bloco de implementação para parado. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Altera o estado do bloco de implementação para iniciado. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/stop/action | Suspende o bloco de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Percorre a atualização do domínio. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/validateMigration/action | Valida a migração de uma ranhura de implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/slots/write | Cria ou atualiza a implementação. |
> | Ação | Microsoft.ClassicCompute/domainNames/swap/action | Troca o bloco de teste pelo bloco de produção. |
> | Ação | Microsoft.ClassicCompute/domainNames/write | Adiciona ou modifica os nomes de domínio dos recursos. |
> | Ação | Microsoft.ClassicCompute/moveSubscriptionResources/action | Mover todos os recursos clássicos para uma subscrição diferente. |
> | Ação | Microsoft.ClassicCompute/operatingSystemFamilies/read | Lista as famílias de sistemas operativos convidadas disponíveis no Microsoft Azure, e lista as versões de sistema operativo disponíveis para cada família. |
> | Ação | Microsoft.ClassicCompute/operatingSystems/read | Lista as versões dos sistemas operativos convidados que estão, de momento, disponíveis no Microsoft Azure. |
> | Ação | Microsoft.ClassicCompute/operations/read | Obtém a lista de operações. |
> | Ação | Microsoft.ClassicCompute/operationStatuses/read | Lê o estado da operação do recurso. |
> | Ação | Microsoft.ClassicCompute/quotas/read | Obtém a quota da subscrição. |
> | Ação | Microsoft.ClassicCompute/register/action | Registar na Computação Clássica |
> | Ação | Microsoft.ClassicCompute/resourceTypes/skus/read | Obtém a lista de SKUs para tipos de recurso suportados. |
> | Ação | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Valide a disponibilidade da subscrição para operação de movimentação clássica. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Elimina o grupo de segurança de rede associado à máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Lê o estado da operação dos grupos de segurança de rede associados a máquinas virtuais. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Obtém o grupo de segurança de rede associado à máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Adiciona um grupo de segurança de rede associado à máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Obtém as operações assíncronas possíveis |
> | Ação | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Anexa um disco de dados a uma máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/capture/action | Capturar uma máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/delete | Remove máquinas virtuais. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Desanexa um disco de dados da máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Obter as definições de diagnóstico de máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/disks/read | Obtém a lista de discos de dados |
> | Ação | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Transfere o ficheiro RDP da máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Lê o estado da operação das extensões de máquinas virtuais. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/extensions/read | Obtém a extensão da máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/extensions/write | Coloca a extensão da máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | Obter a definição de métrica da máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/metrics/read | Obtém as métricas. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Elimina o grupo de segurança de rede associado à interface de rede. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Lê o estado da operação dos grupos de segurança de rede associados a máquinas virtuais. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Obtém o grupo de segurança de rede associado à interface de rede. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Adiciona um grupo de segurança de rede associado à interface de rede. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Lê o estado da operação das máquinas virtuais. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Executa a manutenção na máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Adiciona ou modifica as definições de diagnóstico. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições das métricas. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/read | Obtém a lista de máquinas virtuais. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Reimplementa a máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/restart/action | Reinicia máquinas virtuais. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Encerra a máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/start/action | Inicia a máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/stop/action | Para a máquina virtual. |
> | Ação | Microsoft.ClassicCompute/virtualMachines/write | Adiciona ou modifica máquinas virtuais. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Obtém a lista de dispositivos suportados. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Elimina o grupo de segurança de rede. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Lê o estado da operação do grupo de segurança de rede. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/read | Obtém o grupo de segurança de rede. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Elimina a regra de segurança. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Lê o estado da operação das regras de segurança do grupo de segurança de rede. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Obtém a regra de segurança. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Adiciona ou atualiza uma regra de segurança. |
> | Ação | Microsoft.ClassicNetwork/networkSecurityGroups/write | Adiciona um novo grupo de segurança de rede. |
> | Ação | Microsoft.ClassicNetwork/quotas/read | Obtém a quota da subscrição. |
> | Ação | Microsoft.ClassicNetwork/register/action | Registar na Rede Clássica |
> | Ação | Microsoft.ClassicNetwork/reservedIps/delete | Elimina um IP reservado. |
> | Ação | Microsoft.ClassicNetwork/reservedIps/join/action | Associar um IP reservado |
> | Ação | Microsoft.ClassicNetwork/reservedIps/link/action | Ligar um IP reservado |
> | Ação | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Lê o estado da operação dos IPs reservados. |
> | Ação | Microsoft.ClassicNetwork/reservedIps/read | Obtém os IPs reservados |
> | Ação | Microsoft.ClassicNetwork/reservedIps/write | Adiciona um novo IP reservado |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Mostra as capacidades |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Verifica a disponibilidade de um determinado endereço IP numa rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/delete | Elimina a rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Anula a revogação de um certificado de cliente. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | Lê os certificados de cliente revogados. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Revoga um certificado de cliente. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Elimina o certificado de cliente do gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Transfere o certificado por thumbprint. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Lista o pacote de certificado do gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | Localiza os certificados de raiz de cliente. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Carrega um novo certificado de raiz de cliente. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Estabelece uma ligação de gateway site para site. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Desliga uma ligação de gateway site para site. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | Obtém a lista de ligações. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Testa uma ligação de gateway site para site. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | Elimina o gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Transfere o script de configuração do dispositivo. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | Transfere o diagnóstico do gateway. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Obtém a chave do serviço de circuito. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Lista o pacote do gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Lê o estado da operação dos gateways de redes virtuais. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Obtém o pacote do gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | Obtém os gateways da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Inicia o diagnóstico do gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Para o diagnóstico do gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Adiciona um gateway da rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/join/action | Associa à rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Lê o estado da operação das redes virtuais. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Elementos de uma rede virtual com outra rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/read | Obtém a rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Elimina o grupo de segurança de rede associado à sub-rede. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Lê o estado da operação do grupo de segurança de rede associado à sub-rede de rede virtual. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Obtém o grupo de segurança de rede associado à sub-rede. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Adiciona um grupo de segurança de rede associado à sub-rede. |
> | Ação | Microsoft.ClassicNetwork/virtualNetworks/write | Adiciona uma nova rede virtual. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ClassicStorage/capabilities/read | Mostra as capacidades |
> | Ação | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Verifica a disponibilidade de uma conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/disks/read | Devolve o disco de conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/images/read | Devolve a imagem. |
> | Ação | Microsoft.ClassicStorage/osImages/read | Devolve a imagem de sistema operativo. |
> | Ação | Microsoft.ClassicStorage/publicImages/read | Obtém a imagem pública da máquina virtual. |
> | Ação | Microsoft.ClassicStorage/quotas/read | Obtém a quota da subscrição. |
> | Ação | Microsoft.ClassicStorage/register/action | Registar para Armazenamento Clássico |
> | Ação | Microsoft.ClassicStorage/storageAccounts/delete | Elimina a conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/disks/delete | Elimina um determinado disco de conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Lê o estado da operação do recurso. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/disks/read | Devolve o disco de conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/disks/write | Adiciona um disco de conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/images/delete | Elimina uma imagem de conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/images/read | Devolve a imagem de conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista as chaves de acesso para as contas de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Lê o estado da operação do recurso. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Elimina uma imagem do sistema operativo de uma conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/osImages/read | Devolve a imagem do sistema operativo da conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/read | Devolve a conta de armazenamento com a conta fornecida. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Volta a gerar as chaves de acesso existentes para a conta de armazenamento. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Obtém as definições de diagnóstico. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Adiciona ou modifica as definições de diagnóstico. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Obtém as definições das métricas. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Obtém as métricas. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/services/read | Obtém os serviços disponíveis. |
> | Ação | Microsoft.ClassicStorage/storageAccounts/write | Adiciona uma nova conta de armazenamento. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.CognitiveServices/accounts/delete | Elimina contas da API |
> | Ação | Microsoft.CognitiveServices/accounts/listKeys/action | Listar Chaves |
> | Ação | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o recurso. |
> | Ação | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso. |
> | Ação | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para a conta dos Serviços Cognitivos |
> | Ação | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | Obtém as métricas disponíveis para os Serviços Cognitivos. |
> | Ação | Microsoft.CognitiveServices/accounts/read | Lê contas da API. |
> | Ação | Microsoft.CognitiveServices/accounts/regenerateKey/action | Regenerar Chave |
> | Ação | Microsoft.CognitiveServices/accounts/skus/read | Lê os SKUs disponíveis para um recurso existente. |
> | Ação | Microsoft.CognitiveServices/accounts/usages/read | Obter a utilização de quota para um recurso existente. |
> | Ação | Microsoft.CognitiveServices/accounts/write | Escreve Contas da API. |
> | Ação | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Lê SKUs disponíveis para uma subscrição. |
> | Ação | Microsoft.CognitiveServices/Operations/read | Listar todas as operações disponíveis |
> | Ação | Microsoft.CognitiveServices/register/action | Regista a Subscrição nos Serviços Cognitivos |
> | Ação | Microsoft.CognitiveServices/skus/read | Lê as SKUs disponíveis para os Serviços Cognitivos. |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Commerce/RateCard/read | Devolve oferece dados, os metadados do recurso/medir e as taxas para a subscrição fornecida. |
> | Ação | Microsoft.Commerce/UsageAggregates/read | Obtém o-consumo Microsoft Azure por uma subscrição. O resultado contém agrega dados de utilização, subscrição e dos recursos relacionados com informações, um intervalo de tempo específico. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Compute/availabilitySets/delete | Elimina o conjunto de disponibilidade |
> | Ação | Microsoft.Compute/availabilitySets/read | Obter as propriedades de um conjunto de disponibilidade |
> | Ação | Microsoft.Compute/availabilitySets/vmSizes/read | Listar tamanhos disponíveis para criar ou atualizar uma máquina virtual no conjunto de disponibilidade |
> | Ação | Microsoft.Compute/availabilitySets/write | Cria um novo conjunto de disponibilidade ou atualiza um já existente |
> | Ação | Microsoft.Compute/disks/beginGetAccess/action | Obter o URI de SAS do Disco para acesso do blob |
> | Ação | Microsoft.Compute/disks/delete | Elimina o Disco |
> | Ação | Microsoft.Compute/disks/endGetAccess/action | Revogar o URI de SAS do Disco |
> | Ação | Microsoft.Compute/disks/read | Obter as propriedades de um Disco |
> | Ação | Microsoft.Compute/disks/write | Cria um novo Disco ou atualiza um já existente |
> | Ação | Microsoft.Compute/images/delete | Elimina a imagem |
> | Ação | Microsoft.Compute/images/read | Obter as propriedades da Imagem |
> | Ação | Microsoft.Compute/images/write | Cria uma nova Imagem ou atualiza uma existente |
> | Ação | Microsoft.Compute/locations/capsOperations/read | Obtém o estado de uma operação de Limite assíncrona |
> | Ação | Microsoft.Compute/locations/diskOperations/read | Obtém o estado de uma operação assíncrona do disco |
> | Ação | Microsoft.Compute/locations/operations/read | Obtém o estado de uma operação assíncrona |
> | Ação | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | Obter as propriedades de uma Oferta de Imagem da Plataforma |
> | Ação | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | Obter as propriedades de uma SKU da Imagem de Plataforma |
> | Ação | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | Obter as propriedades de uma Versão da Imagem de Plataforma |
> | Ação | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Obter as propriedades de um Tipo de VMExtension |
> | Ação | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Obter as propriedades de uma Versão do VMExtension |
> | Ação | Microsoft.Compute/locations/publishers/read | Obter as propriedades de um Publicador |
> | Ação | Microsoft.Compute/locations/runCommands/read | Lista os comandos de execução disponíveis na localização |
> | Ação | Microsoft.Compute/locations/usages/read | Obtém os limites de serviço e as quantidades de utilização atuais dos recursos de computação da subscrição numa localização |
> | Ação | Microsoft.Compute/locations/vmSizes/read | Lista os tamanhos de máquina virtual disponíveis numa localização |
> | Ação | Microsoft.Compute/operations/read | Lista as operações disponíveis no fornecedor de recursos Microsoft.Compute |
> | Ação | Microsoft.Compute/register/action | Regista a Subscrição no fornecedor de recursos Microsoft.Compute |
> | Ação | Microsoft.Compute/restorePointCollections/delete | Elimina a coleção de ponto de restauro e pontos de restauro contidos |
> | Ação | Microsoft.Compute/restorePointCollections/read | Obter as propriedades de uma coleção de ponto de restauro |
> | Ação | Microsoft.Compute/restorePointCollections/restorePoints/delete | Elimina o ponto de restauro |
> | Ação | Microsoft.Compute/restorePointCollections/restorePoints/read | Obter as propriedades de um ponto de restauro |
> | Ação | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Obter as propriedades de um ponto de restauro juntamente com os URIs de SAS do blob |
> | Ação | Microsoft.Compute/restorePointCollections/restorePoints/write | Cria um novo ponto de restauro |
> | Ação | Microsoft.Compute/restorePointCollections/write | Cria uma nova coleção de ponto de restauro ou atualiza uma existente |
> | Ação | Microsoft.Compute/sharedVMImages/delete | Elimina o SharedVMImage |
> | Ação | Microsoft.Compute/sharedVMImages/read | Obter as propriedades de um SharedVMImage |
> | Ação | Microsoft.Compute/sharedVMImages/versions/delete | Eliminar um SharedVMImageVersion |
> | Ação | Microsoft.Compute/sharedVMImages/versions/read | Obter as propriedades de um SharedVMImageVersion |
> | Ação | Microsoft.Compute/sharedVMImages/versions/replicate/action | Replicar uma SharedVMImageVersion para regiões de destino |
> | Ação | Microsoft.Compute/sharedVMImages/versions/write | Criar um novo SharedVMImageVersion ou atualizar um já existente |
> | Ação | Microsoft.Compute/sharedVMImages/write | Cria uma nova SharedVMImage ou atualiza uma existente |
> | Ação | Microsoft.Compute/skus/read | Obtém a lista das SKUs do Microsoft.Compute disponíveis para a sua Subscrição |
> | Ação | Microsoft.Compute/snapshots/beginGetAccess/action | Obter o URI de SAS do instantâneo para acesso do blob |
> | Ação | Microsoft.Compute/snapshots/delete | Eliminar um Instantâneo |
> | Ação | Microsoft.Compute/snapshots/endGetAccess/action | Revogar o URI de SAS do instantâneo |
> | Ação | Microsoft.Compute/snapshots/read | Obter as propriedades de um Instantâneo |
> | Ação | Microsoft.Compute/snapshots/write | Criar um novo Instantâneo ou atualizar um já existente |
> | Ação | Microsoft.Compute/virtualMachines/capture/action | Recolhe a máquina virtual, copiando os discos rígidos virtuais, e gera um modelo que pode ser utilizado para criar máquinas virtuais semelhantes |
> | Ação | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Converte os discos baseados em blob da máquina virtual em discos geridos |
> | Ação | Microsoft.Compute/virtualMachines/deallocate/action | Desliga a máquina virtual e liberta os recursos de computação |
> | Ação | Microsoft.Compute/virtualMachines/delete | Elimina a máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/extensions/delete | Elimina a extensão da máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/extensions/read | Obter as propriedades de uma extensão da máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/extensions/write | Cria uma nova extensão da máquina virtual ou atualiza uma já existente |
> | Ação | Microsoft.Compute/virtualMachines/generalize/action | Define o estado da máquina virtual como Generalizado e prepara-a para a captura |
> | Ação | Microsoft.Compute/virtualMachines/instanceView/read | Obtém o estado de runtime detalhado da máquina virtual e dos respetivos recursos |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Iniciar sessão numa máquina virtual como um utilizador normal |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Inicie sessão para uma máquina virtual com o administrador do Windows ou privilégios de utilizador de raiz do Linux |
> | Ação | Microsoft.Compute/virtualMachines/performMaintenance/action | Realiza a Operação de Manutenção na VM. |
> | Ação | Microsoft.Compute/virtualMachines/powerOff/action | Desliga a máquina virtual. Tenha em atenção que a máquina virtual irá continuar a ser faturada. |
> | Ação | Microsoft.Compute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Lê as Definições de Métricas da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachines/read | Obter as propriedades de uma máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/redeploy/action | Reimplementa a máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/reimage/action | Recria a imagem da máquina virtual que está a utilizar o disco de diferenciação. |
> | Ação | Microsoft.Compute/virtualMachines/restart/action | Reinicia a máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/runCommand/action | Executa um script predefinido na máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/start/action | Inicia a máquina virtual |
> | Ação | Microsoft.Compute/virtualMachines/vmSizes/read | Lista os tamanhos disponíveis para os quais a máquina virtual pode ser atualizada |
> | Ação | Microsoft.Compute/virtualMachines/write | Cria uma nova máquina virtual ou atualiza uma máquina virtual existente |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Desliga e liberta os recursos de computação para as instâncias do Conjunto de Dimensionamento da Máquina Virtual  |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/delete | Elimina o Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/delete/action | Elimina as instâncias do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Elimina a Extensão do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Obtém as propriedades de uma Extensão do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Cria uma nova Extensão do Conjunto de Dimensionamento da Máquina Virtual ou atualiza uma existente |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Percorrer manualmente os domínios de atualização da plataforma de um Conjunto de Dimensionamento de Máquinas Virtuais do Service Fabric para concluir uma atualização pendente que está parada |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Obtém a vista de instância do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Atualiza manualmente as instâncias para o modelo mais recente do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Obter as propriedades de todas as interfaces de rede de um Conjunto de Dimensionamento de Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Obtém o histórico de atualizações do SO de um Conjunto de Dimensionamento de Máquinas Virtuais |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Executa a manutenção planeada nas instâncias do Conjunto de Dimensionamento de Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Desliga as instâncias do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/read | Lê as Definições de Métricas do Conjunto de Dimensionamento de Máquinas Virtuais |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Obter as propriedade de todos os endereços IP públicos de um Conjunto de Dimensionamento de Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/read | Obter as propriedades de um Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | Implementar novamente as instâncias do Conjunto de Dimensionamento de Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Recria imagens de instâncias do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/restart/action | Reinicia as instâncias do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Cancela a atualização gradual de um Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Obtenha o estado mais recente da Atualização Gradual para um Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/scale/action | Verificar se um Conjunto de Dimensionamento da Máquina Virtual existente pode ser Reduzido/Aumentado Horizontalmente para a contagem de instâncias especificada |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/skus/read | Lista os SKUs válidos de um Conjunto de Dimensionamento da Máquina Virtual existente |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/start/action | Inicia as instâncias do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Desativa e liberta os recursos de computação de uma Máquina Virtual num Conjunto de Dimensionamento da VM. |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Eliminar uma Máquina Virtual específica num Conjunto de Dimensionamento da VM. |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Obtém a vista de instância de uma Máquina Virtual num Conjunto de Dimensionamento da VM. |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Obter propriedades de endereço IP público criado utilizando o conjunto de dimensionamento da Máquina Virtual. O conjunto de dimensionamento de máquina virtual, pode criar no máximo, um IP público por ipconfiguration (IP privado) |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Obter propriedades de uma ou todas as configurações de IP de uma interface de rede criada utilizando o conjunto de dimensionamento da Máquina Virtual. Configurações de IP representam IPs privados |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Obter as propriedades de uma ou todas as interfaces de rede de uma máquina virtual criada através do Conjunto de Dimensionamento da Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Executa a manutenção planeada numa instância de Máquina Virtual num Conjunto de Dimensionamento de Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Desativa uma instância de Máquina Virtual num Conjunto de Dimensionamento da VM. |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Lê a Máquina Virtual nas Definições de Métricas de Conjunto de Dimensionamento |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | Obtém as propriedades de uma Máquina Virtual num Conjunto de Dimensionamento da VM |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Implementa novamente uma instância de Máquina Virtual num Conjunto de Dimensionamento de Máquina Virtual |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Recria imagens de uma instância de Máquina Virtual num Conjunto de Dimensionamento de Máquina Virtual. |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Reinicia uma instância de Máquina Virtual num Conjunto de Dimensionamento da VM. |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Inicia uma instância de Máquina Virtual num Conjunto de Dimensionamento da VM. |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Atualiza as propriedades de uma Máquina Virtual num Conjunto de Dimensionamento de VM |
> | Ação | Microsoft.Compute/virtualMachineScaleSets/write | Cria um novo Conjunto de Dimensionamento da Máquina Virtual ou atualiza um já existente |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Consumption/balances/read | Liste a utilização de resumo para um período de faturação para um grupo de gestão. |
> | Ação | Microsoft.Consumption/budgets/read | Liste os orçamentos por uma subscrição ou um grupo de gestão. |
> | Ação | Microsoft.Consumption/budgets/write | Criar, atualizar e eliminar os orçamentos por uma subscrição ou um grupo de gestão. |
> | Ação | Microsoft.Consumption/marketplaces/read | Lista os detalhes de utilização de recursos de mercado para um âmbito para EA e WebDirect subscrições. |
> | Ação | Microsoft.Consumption/operations/read | Liste operações todos suportadas pelo fornecedor de recursos de Microsoft.Consumption. |
> | Ação | Microsoft.Consumption/pricesheets/read | Liste os dados de Pricesheets para uma subscrição ou um grupo de gestão. |
> | Ação | Microsoft.Consumption/reservationDetails/read | Lista os detalhes de utilização para instâncias reservados por grupos de gestão ou de ordem de reserva. Os dados de detalhes são por instância por nível de dia. |
> | Ação | Microsoft.Consumption/reservationRecommendations/read | Lista único ou partilhadas recomendações para instâncias reservadas para uma subscrição. |
> | Ação | Microsoft.Consumption/reservationSummaries/read | Liste a utilização de resumo para instâncias reservadas por grupos de gestão ou de ordem de reserva. Os dados de resumos são ao nível diário ou mensal. |
> | Ação | Microsoft.Consumption/reservationTransactions/read | Liste o histórico de transação para instâncias reservados por grupos de gestão. |
> | Ação | Microsoft.Consumption/tenants/register/action | Registe ação de âmbito do Microsoft.Consumption por um inquilino. |
> | Ação | Microsoft.Consumption/terms/read | Liste os termos de licenciamento para uma subscrição ou um grupo de gestão. |
> | Ação | Microsoft.Consumption/usageDetails/read | Lista os detalhes de utilização para um âmbito para EA e WebDirect subscrições. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Obtenha registos para um contentor específico. |
> | Ação | Microsoft.ContainerInstance/containerGroups/delete | Elimine o grupo de contentores específico. |
> | Ação | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o grupo de contentores. |
> | Ação | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o grupo de recursos. |
> | Ação | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Obtém as métricas disponíveis para o grupo de contentores. |
> | Ação | Microsoft.ContainerInstance/containerGroups/read | Obtenha todos os grupos de contentores. |
> | Ação | Microsoft.ContainerInstance/containerGroups/write | Crie ou atualize um grupo de contentores específico. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ContainerRegistry/checkNameAvailability/read | Verifica se o nome de registo do contentor está disponível para utilização. |
> | Ação | Microsoft.ContainerRegistry/locations/operationResults/read | Obtém um resultado de operação assíncrona |
> | Ação | Microsoft.ContainerRegistry/operations/read | Apresenta uma lista de todas as operações de API de REST de registo de contentor do Azure disponíveis |
> | Ação | Microsoft.ContainerRegistry/register/action | Regista a subscrição para o fornecedor de recursos de registo do contentor e permite a criação de registos do contentor. |
> | Ação | Microsoft.ContainerRegistry/registries/delete | Elimina um registo de contentor. |
> | Ação | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Elimina um filtro de grelha de eventos de um registo de contentor. |
> | Ação | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Obtém as propriedades do filtro de grelha de evento especificado ou apresenta uma lista de todos os filtros de grelha de eventos para o registo de contentor especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Cria ou atualiza um filtro de grelha de eventos para um registo de contentor com os parâmetros especificados. |
> | Ação | Microsoft.ContainerRegistry/registries/importImage/action | Importe imagem para o registo de contentor com os parâmetros especificados. |
> | Ação | Microsoft.ContainerRegistry/registries/listCredentials/action | Lista as credenciais de início de sessão para o registo de contentor especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/listUsages/read | Lista as utilizações de quota para o registo de contentor especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/operationStatuses/read | Obtém o estado da operação assíncrona um registo |
> | Ação | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para a Microsoft ContainerRegistry |
> | Ação | Microsoft.ContainerRegistry/registries/read | Obtém as propriedades de registo do contentor especificado ou apresenta uma lista de todos os registos de contentor sob a subscrição ou grupo de recursos especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Gera de novo uma das credenciais de início de sessão para o registo de contentor especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/replications/delete | Elimina uma replicação de um registo de contentor. |
> | Ação | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Obtém o estado da operação assíncrona uma replicação |
> | Ação | Microsoft.ContainerRegistry/registries/replications/read | Obtém as propriedades da replicação especificada ou apresenta uma lista de todas as replicações para o registo de contentor especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/replications/write | Cria ou atualiza uma replicação para um registo de contentor com os parâmetros especificados. |
> | Ação | Microsoft.ContainerRegistry/registries/webhooks/delete | Elimina um webhook de um registo de contentor. |
> | Ação | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Obtém a configuração de URI do serviço e cabeçalhos personalizados para o webhook. |
> | Ação | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Apresenta uma lista de eventos recentes para o webhook especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Obtém o estado da operação assíncrona um webhook |
> | Ação | Microsoft.ContainerRegistry/registries/webhooks/ping/action | É acionado um evento de ping para serem enviados para o webhook. |
> | Ação | Microsoft.ContainerRegistry/registries/webhooks/read | Obtém as propriedades do webhook especificado ou apresenta uma lista de todos os webhooks para o registo de contentor especificado. |
> | Ação | Microsoft.ContainerRegistry/registries/webhooks/write | Cria ou atualiza um webhook para um registo de contentor com os parâmetros especificados. |
> | Ação | Microsoft.ContainerRegistry/registries/write | Cria ou atualiza um registo de contentor com os parâmetros especificados. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ContainerService/containerServices/delete | Elimina um serviço de contentor |
> | Ação | Microsoft.ContainerService/containerServices/read | Obter um serviço de contentor |
> | Ação | Microsoft.ContainerService/containerServices/write | Cria um novo serviço de contentor ou atualiza um já existente |
> | Ação | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Obter um perfil de acesso de cluster geridos por nome de função utilizando credenciais de lista |
> | Ação | Microsoft.ContainerService/managedClusters/accessProfiles/read | Obter um perfil de acesso de cluster geridos por nome de função |
> | Ação | Microsoft.ContainerService/managedClusters/delete | Elimina um cluster gerido |
> | Ação | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/read | Obter a definição de diagnóstico para um recurso de cluster geridos |
> | Ação | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para um recurso de cluster geridos |
> | Ação | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para o Cluster gerido |
> | Ação | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o Cluster gerido |
> | Ação | Microsoft.ContainerService/managedClusters/read | Obter um cluster gerido |
> | Ação | Microsoft.ContainerService/managedClusters/write | Cria um novo cluster gerido ou atualiza um já existente |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ContentModerator/applications/delete | Operação de Eliminação |
> | Ação | Microsoft.ContentModerator/applications/listSecrets/action | Listar Segredos |
> | Ação | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Início de sessão único Tokens de leitura |
> | Ação | Microsoft.ContentModerator/applications/read | Operação de Leitura |
> | Ação | Microsoft.ContentModerator/applications/write | Operação de Escrita |
> | Ação | Microsoft.ContentModerator/applications/write | Operação de Escrita |
> | Ação | Microsoft.ContentModerator/listCommunicationPreference/action | Preferência de comunicação de lista |
> | Ação | Microsoft.ContentModerator/operations/read | operações de leitura |
> | Ação | Microsoft.ContentModerator/updateCommunicationPreference/action | Preferência de comunicação de atualização |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.CustomerInsights/hubs/adobemetadata/action | Criar ou atualizar os metadados do cliente do Azure Insights Adobe |
> | Ação | Microsoft.CustomerInsights/hubs/adobemetadata/read | Ler os metadados do cliente do Azure Insights Adobe |
> | Ação | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Eliminar qualquer política de assinatura de acesso partilhado de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Leia a que política de assinatura de acesso de partilhadas quaisquer informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Regenerar a chave primária da política de assinaturas de acesso de partilhado das informações do cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Regenerar a chave secundária da política de assinaturas de acesso de partilhado das informações do cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Criar ou atualizar qualquer política de assinatura de acesso partilhado de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/activate/action | Ativar os conectores de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/activate/action | Ativar os conectores de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/delete | Eliminar qualquer conector das informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | Obter o estado de runtime qualquer conector de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Ativar qualquer mapeamento de conector de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Eliminar qualquer mapeamento de conector de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Ler os resultados da operação mapeamento do conector Azure cliente Insights |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Ler qualquer mapeamento de conector de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Criar ou atualizar qualquer mapeamento de conector de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/operations/read | Ler os resultados da operação de conector de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/read | Leia os conectores de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Criar ou atualizar todas as informações conector cliente do Azure Insights autenticação |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/update/action | Atualizar qualquer conector das informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/connectors/write | Criar ou atualizar qualquer conector das informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/crmmetadata/action | Criar ou atualizar os metadados do cliente do Azure Insights Crm |
> | Ação | Microsoft.CustomerInsights/hubs/crmmetadata/read | Ler os metadados do cliente do Azure Insights Crm |
> | Ação | Microsoft.CustomerInsights/hubs/delete | Eliminar o Hub de Insights qualquer cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/gdpr/delete | Eliminar qualquer Gdpr de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/gdpr/read | Ler qualquer Gdpr de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/gdpr/write | Criar ou atualizar qualquer Gdpr de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Obter créditos de faturação de Hub de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Obter o histórico de faturação de Hub de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/images/delete | Elimine qualquer imagem de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/images/read | Ler qualquer imagem de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/images/write | Criar ou atualizar qualquer imagem de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/interactions/delete | Eliminar qualquer interações de informações de cliente do azure |
> | Ação | Microsoft.CustomerInsights/hubs/interactions/operations/read | Ler os resultados da operação interação de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/interactions/read | Ler qualquer interação de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | Sugerimos relação ligações para qualquer interações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/interactions/write | Criar ou atualizar qualquer interação de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/kpi/delete | Eliminar qualquer indicador de chave de desempenho de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/kpi/operations/read | Ler os resultados da operação indicadores de desempenho de chave de informações ao cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/kpi/read | Ler qualquer indicador de chave de desempenho de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Reprocessar qualquer indicadores de chave de desempenho de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/kpi/write | Criar ou atualizar qualquer indicador de chave de desempenho de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/links/delete | Eliminar todas as ligações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/links/operations/read | Ler os resultados da operação de ligações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/links/read | Ler todas as ligações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/links/write | Criar ou atualizar todas as ligações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/msemetadata/action | Criar ou atualizar os metadados do cliente do Azure Insights Mse |
> | Ação | Microsoft.CustomerInsights/hubs/msemetadata/read | Ler os metadados do cliente do Azure Insights Mse |
> | Ação | Microsoft.CustomerInsights/hubs/operationresults/read | Obter resultados de operação de Hub de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/predictions/delete | Eliminar qualquer Predições de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/predictions/operations/read | Ler os resultados da operação Predições de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/predictions/read | Ler qualquer Predições de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/predictions/write | Criar ou atualizar qualquer Predições do cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Eliminar quaisquer políticas de correspondência preditiva de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Ler os resultados da operação Azure cliente Insights preditiva correspondência políticas |
> | Ação | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Ler todas as políticas de cliente do Azure Insights correspondência preditiva |
> | Ação | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Criar ou atualizar quaisquer políticas de correspondência preditiva de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/profiles/delete | Eliminar qualquer perfil de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/profiles/operations/read | Ler os resultados da operação de perfil de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/profiles/read | Ler nenhum perfil de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/profiles/write | Escrever qualquer perfil de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para o recurso |
> | Ação | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o recurso |
> | Ação | Microsoft.CustomerInsights/hubs/read | Ler qualquer Hub de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Eliminar todas as ligações do cliente do Azure Insights relação |
> | Ação | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Ler os resultados da operação de ligações de relação de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Ler todas as ligações do cliente do Azure Insights relação |
> | Ação | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Criar ou atualizar todas as ligações do cliente do Azure Insights relação |
> | Ação | Microsoft.CustomerInsights/hubs/relationships/delete | Eliminar quaisquer relações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/relationships/operations/read | Ler os resultados da operação de relações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/relationships/read | Ler quaisquer relações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/relationships/write | Criar ou atualizar quaisquer relações de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Eliminar qualquer atribuição do cliente do Azure Insights Rbac |
> | Ação | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Ler os resultados da operação de atribuição de Rbac do Azure ao cliente Insights |
> | Ação | Microsoft.CustomerInsights/hubs/roleAssignments/read | Ler qualquer atribuição do cliente do Azure Insights Rbac |
> | Ação | Microsoft.CustomerInsights/hubs/roleAssignments/write | Criar ou atualizar qualquer atribuição do cliente do Azure Insights Rbac |
> | Ação | Microsoft.CustomerInsights/hubs/roles/read | Ler quaisquer funções de Rbac de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Criar ou atualizar os metadados do cliente do Azure Insights SalesForce |
> | Ação | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Ler os metadados do cliente do Azure Insights SalesForce |
> | Ação | Microsoft.CustomerInsights/hubs/segments/delete | Elimine os segmentos de Insights qualquer cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Gestão Segmenta de qualquer dinâmica de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/segments/read | Ler quaisquer segmentos de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/segments/static/action | Gestão Segmenta de qualquer estática de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/segments/write | Criar ou atualizar quaisquer segmentos de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Eliminar qualquer SqlConnectionStrings de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Ler qualquer SqlConnectionStrings de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Criar ou atualizar qualquer SqlConnectionStrings de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Gerar sugerir esquema de tipo de dados de exemplo |
> | Ação | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Gerir as definições de hub de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/views/delete | Eliminar qualquer vista de aplicação de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/views/read | Ler a qualquer vista de aplicação de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/views/write | Criar ou atualizar qualquer vista de aplicação de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/hubs/widgettypes/read | Ler todos os tipos de cliente do Azure Insights Widget de aplicação |
> | Ação | Microsoft.CustomerInsights/hubs/write | Criar ou atualizar qualquer Hub de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/operations/read | Ler Metadatas de Api de informações de cliente do Azure |
> | Ação | Microsoft.CustomerInsights/register/action | Regista a subscrição para o fornecedor de recursos de informações de cliente e permite a criação dos recursos de informações de cliente |
> | Ação | Microsoft.CustomerInsights/unregister/action | Anula o registo da subscrição para o fornecedor de recursos de informações de cliente |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Databricks/workspaces/delete | Remove uma área de trabalho. |
> | Ação | Microsoft.Databricks/workspaces/read | Obtém uma lista de áreas de trabalho. |
> | Ação | Microsoft.Databricks/workspaces/write | Cria uma área de trabalho. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DataCatalog/catalogs/delete | Elimina o catálogo. |
> | Ação | Microsoft.DataCatalog/catalogs/read | Obter propriedades para o catálogo ou catálogos sob a subscrição ou grupo de recursos. |
> | Ação | Microsoft.DataCatalog/catalogs/write | Cria o catálogo ou as etiquetas e propriedades para o catálogo de atualizações. |
> | Ação | Microsoft.DataCatalog/checkNameAvailability/action | Verifica a disponibilidade do nome de catálogo para o inquilino. |
> | Ação | Microsoft.DataCatalog/operations/read | Lista as operações disponíveis no fornecedor de recursos de Microsoft.DataCatalog. |
> | Ação | Microsoft.DataCatalog/register/action | Regista a subscrição no fornecedor de recursos de Microsoft.DataCatalog. |
> | Ação | Microsoft.DataCatalog/unregister/action | Anula o registo da subscrição do fornecedor de recursos de Microsoft.DataCatalog. |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DataFactory/datafactories/activitywindows/read | Lê o Windows de atividade no Factory de dados com os parâmetros especificados. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Lê o Windows de atividade para a atividade do Pipeline com os parâmetros especificados. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Lê o Windows de atividade para o Pipeline com os parâmetros especificados. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/delete | Elimina quaisquer Pipeline. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Interrompe a qualquer Pipeline. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/read | Lê qualquer Pipeline. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Retoma qualquer Pipeline. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/update/action | Atualiza qualquer Pipeline. |
> | Ação | Microsoft.DataFactory/datafactories/datapipelines/write | Cria ou atualiza qualquer Pipeline. |
> | Ação | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Lê o Windows de atividade para o conjunto de dados com os parâmetros especificados. |
> | Ação | Microsoft.DataFactory/datafactories/datasets/delete | Elimina a qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/datafactories/datasets/read | Lê qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Lê a executar a segmentação de dados para o conjunto de dados fornecido com a hora de início especificada. |
> | Ação | Microsoft.DataFactory/datafactories/datasets/slices/read | Obtém os setores de dados num determinado período. |
> | Ação | Microsoft.DataFactory/datafactories/datasets/slices/write | Atualize o estado do setor de dados. |
> | Ação | Microsoft.DataFactory/datafactories/datasets/write | Cria ou atualiza qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/datafactories/delete | Elimina a fábrica de dados. |
> | Ação | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Lê as informações de ligação para qualquer Gateway. |
> | Ação | Microsoft.DataFactory/datafactories/gateways/delete | Elimina a qualquer Gateway. |
> | Ação | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Lista as chaves de autenticação para qualquer Gateway. |
> | Ação | Microsoft.DataFactory/datafactories/gateways/read | Lê qualquer Gateway. |
> | Ação | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Gera de novo as chaves de autenticação para qualquer Gateway. |
> | Ação | Microsoft.DataFactory/datafactories/gateways/write | Cria ou atualiza qualquer Gateway. |
> | Ação | Microsoft.DataFactory/datafactories/linkedServices/delete | Elimina a qualquer serviço ligado. |
> | Ação | Microsoft.DataFactory/datafactories/linkedServices/read | Lê a qualquer serviço ligado. |
> | Ação | Microsoft.DataFactory/datafactories/linkedServices/write | Cria ou atualiza a qualquer serviço ligado. |
> | Ação | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para datafactories |
> | Ação | Microsoft.DataFactory/datafactories/read | Lê a fábrica de dados. |
> | Ação | Microsoft.DataFactory/datafactories/runs/loginfo/read | Lê um URI de SAS para um contentor do blob que contém os registos. |
> | Ação | Microsoft.DataFactory/datafactories/tables/delete | Elimina a qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/datafactories/tables/read | Lê qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/datafactories/tables/write | Cria ou atualiza qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/datafactories/write | Cria ou atualiza a fábrica de dados. |
> | Ação | Microsoft.DataFactory/factories/cancelpipelinerun/action | Cancela o pipeline executar especificado pelo ID de execução. |
> | Ação | Microsoft.DataFactory/factories/datasets/delete | Elimina a qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/factories/datasets/read | Lê qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/factories/datasets/write | Cria ou atualiza qualquer conjunto de dados. |
> | Ação | Microsoft.DataFactory/factories/delete | Elimina a fábrica de dados. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/delete | Elimina a qualquer tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Lê as informações de ligação de tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Lê o estado de Runtime de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Lista as chaves de autenticação para qualquer tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Obtém os dados de monitorização para qualquer tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Elimina o nó para o tempo de execução de integração especificado. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Devolve o endereço IP para o nó especificado do tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | As atualizações de um nó de tempo de execução de integração personalizada alojada. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/read | Lê qualquer tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Gera de novo as chaves de autenticação para o tempo de execução de integração especificado. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/start/action | Inicia a qualquer tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Interrompe a qualquer tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Sincroniza-se as credenciais para o tempo de execução de integração especificado. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Atualiza o tempo de execução de integração especificado. |
> | Ação | Microsoft.DataFactory/factories/integrationruntimes/write | Cria ou atualiza qualquer tempo de execução de integração. |
> | Ação | Microsoft.DataFactory/factories/linkedServices/delete | Elimina o serviço ligado. |
> | Ação | Microsoft.DataFactory/factories/linkedServices/read | Leituras de serviço ligado. |
> | Ação | Microsoft.DataFactory/factories/linkedServices/write | Criar ou atualizar o serviço ligado |
> | Ação | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Lê que a atividade é executada para o pipeline especificado execute ID. |
> | Ação | Microsoft.DataFactory/factories/pipelineruns/read | Lê a execução de Pipeline. |
> | Ação | Microsoft.DataFactory/factories/pipelines/createrun/action | Cria uma execução para o Pipeline. |
> | Ação | Microsoft.DataFactory/factories/pipelines/delete | Elimina o Pipeline. |
> | Ação | Microsoft.DataFactory/factories/pipelines/read | Lê o Pipeline. |
> | Ação | Microsoft.DataFactory/factories/pipelines/write | Criar ou atualizar o Pipeline |
> | Ação | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.DataFactory/factories/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para as fábricas de |
> | Ação | Microsoft.DataFactory/factories/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para as fábricas de |
> | Ação | Microsoft.DataFactory/factories/read | Lê a fábrica de dados. |
> | Ação | Microsoft.DataFactory/factories/triggers/delete | Elimina quaisquer acionador. |
> | Ação | Microsoft.DataFactory/factories/triggers/read | Lê qualquer acionador. |
> | Ação | Microsoft.DataFactory/factories/triggers/start/action | Inicia a qualquer acionador. |
> | Ação | Microsoft.DataFactory/factories/triggers/stop/action | Interrompe a qualquer acionador. |
> | Ação | Microsoft.DataFactory/factories/triggers/triggerruns/read | Lê a execução do acionador. |
> | Ação | Microsoft.DataFactory/factories/triggers/write | Cria ou atualiza qualquer acionador. |
> | Ação | Microsoft.DataFactory/factories/write | Criar ou atualizar a fábrica de dados |
> | Ação | Microsoft.DataFactory/locations/configureFactoryRepo/action | Configura o repositório do Factory. |
> | Ação | Microsoft.DataFactory/register/action | Regista a subscrição para o fornecedor de recursos de fábrica de dados. |
> | Ação | Microsoft.DataFactory/unregister/action | Anula o registo da subscrição para o fornecedor de recursos de fábrica de dados. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Elimine uma política de computação. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Obter informações sobre uma política de computação. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Criar ou atualizar uma política de computação. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Desassociar uma conta de DataLakeStore de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Obter informações sobre uma conta de DataLakeStore ligada de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Criar ou atualizar uma conta de DataLakeStore ligada de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/delete | Elimine uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Elimine uma regra de firewall. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Obter informações sobre uma regra de firewall. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Criar ou atualizar uma regra de firewall. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Obter o resultado de uma operação de conta DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Obter as definições de diagnóstico para a conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Criar ou atualizar as definições de diagnóstico para a conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/logDefinitions/read | Obter os registos disponíveis para a conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/metricDefinitions/read | Obter as métricas disponíveis para a conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/read | Obter informações sobre uma conta de DataLakeAnalytics existente. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Lista os tokens SAS para contentores de armazenamento de uma conta de armazenamento ligada de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Obter contentores de uma conta de armazenamento ligada de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Desassociar uma conta de armazenamento de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Obter informações sobre a conta de armazenamento ligada de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Criar ou atualizar uma conta de armazenamento ligada de uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Conceder permissões para cancelar as tarefas submetidas por outros utilizadores. |
> | Ação | Microsoft.DataLakeAnalytics/accounts/write | Criar ou atualizar uma conta de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/locations/capability/read | Obter informações de capacidade de uma subscrição relativos à utilização DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | Verifique a disponibilidade de um nome de conta DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/locations/operationResults/read | Obter o resultado de uma operação de conta DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/operations/read | Obter operações disponíveis de DataLakeAnalytics. |
> | Ação | Microsoft.DataLakeAnalytics/register/action | Registe a subscrição para DataLakeAnalytics. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DataLakeStore/accounts/delete | Elimine uma conta de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/accounts/enableKeyVault/action | Ative KeyVault para uma conta de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | Elimine um filtro de EventGrid. |
> | Ação | Microsoft.DataLakeStore/accounts/eventGridFilters/read | Obter um filtro de EventGrid. |
> | Ação | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Criar ou atualizar um filtro de EventGrid. |
> | Ação | Microsoft.DataLakeStore/accounts/firewallRules/delete | Elimine uma regra de firewall. |
> | Ação | Microsoft.DataLakeStore/accounts/firewallRules/read | Obter informações sobre uma regra de firewall. |
> | Ação | Microsoft.DataLakeStore/accounts/firewallRules/write | Criar ou atualizar uma regra de firewall. |
> | Ação | Microsoft.DataLakeStore/accounts/operationResults/read | Obter o resultado de uma operação de conta DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Obter as definições de diagnóstico para a conta de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Criar ou atualizar as definições de diagnóstico para a conta de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/logDefinitions/read | Obter os registos disponíveis para a conta de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/metricDefinitions/read | Obter as métricas disponíveis para a conta de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/accounts/read | Obter informações sobre uma conta de DataLakeStore existente. |
> | Ação | Microsoft.DataLakeStore/accounts/Superuser/action | Conceder Superutilizador no Data Lake Store quando concedido com Microsoft.Authorization/roleAssignments/write. |
> | Ação | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Elimine um fornecedor de identidade fidedigna. |
> | Ação | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Obter informações sobre um fornecedor de identidade fidedigna. |
> | Ação | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Criar ou atualizar um fornecedor de identidade fidedigna. |
> | Ação | Microsoft.DataLakeStore/accounts/write | Criar ou atualizar uma conta de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/locations/capability/read | Obter informações de capacidade de uma subscrição relativos à utilização DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Verifique a disponibilidade de um nome de conta DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/locations/operationResults/read | Obter o resultado de uma operação de conta DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/operations/read | Obter operações disponíveis de DataLakeStore. |
> | Ação | Microsoft.DataLakeStore/register/action | Registe a subscrição para DataLakeStore. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DataMigration/locations/operationResults/read | Obter o estado de uma operação prolongada relacionada com uma resposta Aceite 202 |
> | Ação | Microsoft.DataMigration/locations/operationStatuses/read | Obter o estado de uma operação prolongada relacionada com uma resposta Aceite 202 |
> | Ação | Microsoft.DataMigration/register/action | Regista a subscrição com o Azure Database Migration Service |
> | Ação | Microsoft.DataMigration/services/checkStatus/action | Verificar se o serviço está implementado e em execução |
> | Ação | Microsoft.DataMigration/services/delete | Elimina um recurso e respetivos subordinados |
> | Ação | Microsoft.DataMigration/services/projects/accessArtifacts/action | Gerar um URL que possa ser utilizado para OBTER ou COLOCAR artefactos de projeto |
> | Ação | Microsoft.DataMigration/services/projects/delete | Elimina um recurso e respetivos subordinados |
> | Ação | Microsoft.DataMigration/services/projects/read | Ler informações sobre recursos |
> | Ação | Microsoft.DataMigration/services/projects/tasks/cancel/action | Cancelar a tarefa se estiver atualmente em execução |
> | Ação | Microsoft.DataMigration/services/projects/tasks/delete | Elimina um recurso e respetivos subordinados |
> | Ação | Microsoft.DataMigration/services/projects/tasks/read | Ler informações sobre recursos |
> | Ação | Microsoft.DataMigration/services/projects/tasks/write | Executar tarefas das tarefas do Azure Database Migration Service |
> | Ação | Microsoft.DataMigration/services/projects/write | Executar tarefas das tarefas do Azure Database Migration Service |
> | Ação | Microsoft.DataMigration/services/read | Ler informações sobre recursos |
> | Ação | Microsoft.DataMigration/services/slots/delete | Elimina um recurso e respetivos subordinados |
> | Ação | Microsoft.DataMigration/services/slots/read | Ler informações sobre recursos |
> | Ação | Microsoft.DataMigration/services/slots/write | Criar ou atualizar recursos e respetivas propriedades |
> | Ação | Microsoft.DataMigration/services/start/action | Inicie o serviço DMS para permitir que o mesmo processe migrações novamente |
> | Ação | Microsoft.DataMigration/services/stop/action | Pare o serviço DMS para minimizar os respetivos custos |
> | Ação | Microsoft.DataMigration/services/write | Criar ou atualizar recursos e respetivas propriedades |
> | Ação | Microsoft.DataMigration/skus/read | Obtenha uma lista de SKUs suportados por recursos DMS. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DBforMySQL/locations/performanceTiers/read | Devolve a lista de escalões de desempenho disponíveis. |
> | Ação | Microsoft.DBforMySQL/performanceTiers/read | Devolve a lista de escalões de desempenho disponíveis. |
> | Ação | Microsoft.DBforMySQL/servers/delete | Elimina um servidor existente. |
> | Ação | Microsoft.DBforMySQL/servers/firewallRules/delete | Elimina uma regra de firewall existente. |
> | Ação | Microsoft.DBforMySQL/servers/firewallRules/read | Devolva a lista de firewall regras para um servidor ou obtém as propriedades para a regra de firewall especificada. |
> | Ação | Microsoft.DBforMySQL/servers/firewallRules/write | Cria uma regra de firewall com os parâmetros especificados ou atualiza uma regra existente. |
> | Ação | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do recurso disagnostic |
> | Ação | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Tipos de retorno de métricas que estão disponíveis para bases de dados |
> | Ação | Microsoft.DBforMySQL/servers/read | Devolva a lista de servidores ou obtém as propriedades para o servidor especificado. |
> | Ação | Microsoft.DBforMySQL/servers/recoverableServers/read | Devolver as informações de servidor MySQL recuperáveis |
> | Ação | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Elimina uma regra de rede Virtual existente |
> | Ação | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Devolva a lista de rede virtual regras ou obtém as propriedades para a regra de rede virtual especificado. |
> | Ação | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Cria uma regra de rede virtual com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a regra de rede virtual especificado. |
> | Ação | Microsoft.DBforMySQL/servers/write | Cria um servidor com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o servidor especificado. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Devolve a lista de escalões de desempenho disponíveis. |
> | Ação | Microsoft.DBforPostgreSQL/performanceTiers/read | Devolve a lista de escalões de desempenho disponíveis. |
> | Ação | Microsoft.DBforPostgreSQL/servers/delete | Elimina um servidor existente. |
> | Ação | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Elimina uma regra de firewall existente. |
> | Ação | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Devolva a lista de firewall regras para um servidor ou obtém as propriedades para a regra de firewall especificada. |
> | Ação | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Cria uma regra de firewall com os parâmetros especificados ou atualiza uma regra existente. |
> | Ação | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do recurso disagnostic |
> | Ação | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para os servidores de Postgres |
> | Ação | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Tipos de retorno de métricas que estão disponíveis para bases de dados |
> | Ação | Microsoft.DBforPostgreSQL/servers/read | Devolva a lista de servidores ou obtém as propriedades para o servidor especificado. |
> | Ação | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Devolver as informações de servidor PostgreSQL recuperáveis |
> | Ação | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Obter os detalhes da política de deteção de ameaças de servidor configurada num determinado servidor |
> | Ação | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Alterar a política de deteção de ameaças de servidor para um determinado servidor |
> | Ação | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Elimina uma regra de rede Virtual existente |
> | Ação | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Devolva a lista de rede virtual regras ou obtém as propriedades para a regra de rede virtual especificado. |
> | Ação | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Cria uma regra de rede virtual com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a regra de rede virtual especificado. |
> | Ação | Microsoft.DBforPostgreSQL/servers/write | Cria um servidor com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o servidor especificado. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Devices/canaryoperationresults/Read | Obter o resultado da operação para Canary |
> | Ação | Microsoft.Devices/checkNameAvailability/Action | Verifique o nome do IotHub se está disponível |
> | Ação | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Verifique o nome do IotHub se está disponível |
> | Ação | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Elimina o certificado |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Gerar código de verificação |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Obtém o certificado |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Verifique o recurso de certificado |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Criar ou atualizar certificados |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Eliminar o recurso de inquilino IotHub |
> | Ação | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | Eliminar grupo de consumidores de EventHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | Obter os grupos de consumidores de EventHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | Criar grupo de consumidores de EventHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Dispositivos de exportação |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Obtém o inquilino IotHub recursos estatísticas |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Importar dispositivos |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Obtém a chave de inquilino IotHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Obter tarefas detalhes submetidos numa fornecido IotHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Obtém as chaves de inquilino do IotHub |
> | Ação | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Obtém as definições de registo disponível para o serviço de IotHub |
> | Ação | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Obtem as métricas disponíveis para o serviço de IotHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Obter métricas de Quota |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/Read | Obtém o recurso de inquilino IotHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Testar uma mensagem de todas as rotas existentes |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Uma mensagem de um teste fornecido rota de teste |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Obtém o estado de funcionamento de todos os pontos finais de encaminhamento para um IotHub |
> | Ação | Microsoft.Devices/elasticPools/iotHubTenants/Write | Criar ou atualizar o recurso de inquilino IotHub |
> | Ação | Microsoft.Devices/ElasticPools/metricDefinitions/read | Obtem as métricas disponíveis para o serviço de IotHub |
> | Ação | Microsoft.Devices/iotHubs/canaryoperationresults/Read | Obter o resultado da operação para Canary (API obsoleta) |
> | Ação | Microsoft.Devices/iotHubs/certificates/Delete | Elimina o certificado |
> | Ação | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Gerar código de verificação |
> | Ação | Microsoft.Devices/iotHubs/certificates/Read | Obtém o certificado |
> | Ação | Microsoft.Devices/iotHubs/certificates/verify/Action | Verifique o recurso de certificado |
> | Ação | Microsoft.Devices/iotHubs/certificates/Write | Criar ou atualizar certificados |
> | Ação | Microsoft.Devices/iotHubs/Delete | Eliminar recurso IotHub |
> | Ação | Microsoft.Devices/IotHubs/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Devices/IotHubs/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Elimina o filtro de eventos de grelha |
> | Ação | Microsoft.Devices/iotHubs/eventGridFilters/Read | Obtém o filtro de eventos de grelha |
> | Ação | Microsoft.Devices/iotHubs/eventGridFilters/Write | Criar um novo ou atualizar o filtro de grelha de evento existente |
> | Ação | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | Eliminar grupo de consumidores de EventHub |
> | Ação | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | Obter os grupos de consumidores de EventHub |
> | Ação | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | Criar grupo de consumidores de EventHub |
> | Ação | Microsoft.Devices/iotHubs/exportDevices/Action | Dispositivos de exportação |
> | Ação | Microsoft.Devices/iotHubs/importDevices/Action | Importar dispositivos |
> | Ação | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Obter a chave de IotHub para o nome fornecido |
> | Ação | Microsoft.Devices/iotHubs/iotHubStats/Read | Obter estatísticas IotHub |
> | Ação | Microsoft.Devices/iotHubs/jobs/Read | Obter tarefas detalhes submetidos numa fornecido IotHub |
> | Ação | Microsoft.Devices/iotHubs/listkeys/Action | Obter todas as chaves de IotHub |
> | Ação | Microsoft.Devices/IotHubs/logDefinitions/read | Obtém as definições de registo disponível para o serviço de IotHub |
> | Ação | Microsoft.Devices/IotHubs/metricDefinitions/read | Obtem as métricas disponíveis para o serviço de IotHub |
> | Ação | Microsoft.Devices/iotHubs/operationresults/Read | Obter o resultado da operação (API obsoleto) |
> | Ação | Microsoft.Devices/iotHubs/quotaMetrics/Read | Obter métricas de Quota |
> | Ação | Microsoft.Devices/iotHubs/Read | Obtém os recursos de IotHub |
> | Ação | Microsoft.Devices/iotHubs/routing/$testall/Action | Testar uma mensagem de todas as rotas existentes |
> | Ação | Microsoft.Devices/iotHubs/routing/$testnew/Action | Uma mensagem de um teste fornecido rota de teste |
> | Ação | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Obtém o estado de funcionamento de todos os pontos finais de encaminhamento para um IotHub |
> | Ação | Microsoft.Devices/iotHubs/skus/Read | Obter Skus válidos de IotHub |
> | Ação | Microsoft.Devices/iotHubs/Write | Criar ou atualizar o recurso de IotHub |
> | Ação | Microsoft.Devices/operationresults/Read | Obter Resultado da Operação |
> | Ação | Microsoft.Devices/operations/Read | Obter todas as operações de ResourceProvider |
> | Ação | Microsoft.Devices/provisioningServices/certificates/Delete | Elimina o certificado |
> | Ação | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Gerar código de verificação |
> | Ação | Microsoft.Devices/provisioningServices/certificates/Read | Obtém o certificado |
> | Ação | Microsoft.Devices/provisioningServices/certificates/verify/Action | Verifique o recurso de certificado |
> | Ação | Microsoft.Devices/provisioningServices/certificates/Write | Criar ou atualizar certificados |
> | Ação | Microsoft.Devices/provisioningServices/Delete | Eliminar recurso IotDps |
> | Ação | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Devices/provisioningServices/keys/listkeys/Action | Obter chaves IotDps para o nome da chave |
> | Ação | Microsoft.Devices/provisioningServices/listkeys/Action | Obter todas as chaves de IotDps |
> | Ação | Microsoft.Devices/provisioningServices/logDefinitions/read | Obtém as definições de registo disponível para o serviço de aprovisionamento |
> | Ação | Microsoft.Devices/provisioningServices/metricDefinitions/read | Obtem as métricas disponíveis para o serviço de aprovisionamento |
> | Ação | Microsoft.Devices/provisioningServices/operationresults/Read | Obter o resultado da operação DPS |
> | Ação | Microsoft.Devices/provisioningServices/Read | Obter o recurso de IotDps |
> | Ação | Microsoft.Devices/provisioningServices/skus/Read | Obter IotDps Skus válidos |
> | Ação | Microsoft.Devices/provisioningServices/Write | Criar recurso IotDps |
> | Ação | Microsoft.Devices/register/action | Registar a subscrição para o fornecedor de recursos do IotHub e permite a criação dos recursos de IotHub |
> | Ação | Microsoft.Devices/register/action | Registar a subscrição para o fornecedor de recursos do IotHub e permite a criação dos recursos de IotHub |
> | Ação | Microsoft.Devices/usages/Read | Obter detalhes de utilização de subscrição para este fornecedor. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DevTestLab/labCenters/delete | Elimine centros de laboratório. |
> | Ação | Microsoft.DevTestLab/labCenters/read | Ler centros de laboratório. |
> | Ação | Microsoft.DevTestLab/labCenters/write | Adiciona ou modifica os centros de laboratório. |
> | Ação | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Ler modelos do Gestor de recursos do azure. |
> | Ação | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Gera um modelo ARM para o artefacto indicado, carrega os ficheiros necessários para uma conta de armazenamento e valida o artefacto gerado. |
> | Ação | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Ler os artefactos. |
> | Ação | Microsoft.DevTestLab/labs/artifactSources/delete | Elimine as origens de artefactos. |
> | Ação | Microsoft.DevTestLab/labs/artifactSources/read | Ler as origens de artefactos. |
> | Ação | Microsoft.DevTestLab/labs/artifactSources/write | Adiciona ou modifica as origens de artefactos. |
> | Ação | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Uma máquina virtual claimable aleatória no laboratório de afirmações. |
> | Ação | Microsoft.DevTestLab/labs/costs/read | Ler os custos. |
> | Ação | Microsoft.DevTestLab/labs/costs/write | Adiciona ou modifica os custos. |
> | Ação | Microsoft.DevTestLab/labs/CreateEnvironment/action | Crie máquinas virtuais no laboratório. |
> | Ação | Microsoft.DevTestLab/labs/customImages/delete | Elimine imagens personalizadas. |
> | Ação | Microsoft.DevTestLab/labs/customImages/read | Ler imagens personalizadas. |
> | Ação | Microsoft.DevTestLab/labs/customImages/write | Adiciona ou modifica imagens personalizadas. |
> | Ação | Microsoft.DevTestLab/labs/delete | Elimine laboratórios. |
> | Ação | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Exporta a utilização de recursos de laboratório para uma conta de armazenamento |
> | Ação | Microsoft.DevTestLab/labs/formulas/delete | Elimine as fórmulas. |
> | Ação | Microsoft.DevTestLab/labs/formulas/read | Ler as fórmulas. |
> | Ação | Microsoft.DevTestLab/labs/formulas/write | Adiciona ou modifica as fórmulas. |
> | Ação | Microsoft.DevTestLab/labs/galleryImages/read | Ler imagens gallery. |
> | Ação | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Gere um URI de carregamento de imagens do disco personalizado para um laboratório. |
> | Ação | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Importe uma máquina virtual para um laboratório diferentes. |
> | Ação | Microsoft.DevTestLab/labs/ListVhds/action | Imagens de disco de lista disponível para criação de imagens personalizadas. |
> | Ação | Microsoft.DevTestLab/labs/notificationChannels/delete | Elimine notificationchannels. |
> | Ação | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Envie notificações para canal fornecido. |
> | Ação | Microsoft.DevTestLab/labs/notificationChannels/read | Ler notificationchannels. |
> | Ação | Microsoft.DevTestLab/labs/notificationChannels/write | Adiciona ou modifica notificationchannels. |
> | Ação | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Avalia a política de laboratório. |
> | Ação | Microsoft.DevTestLab/labs/policySets/policies/delete | Elimine políticas. |
> | Ação | Microsoft.DevTestLab/labs/policySets/policies/read | Políticas de leitura. |
> | Ação | Microsoft.DevTestLab/labs/policySets/policies/write | Adicionar ou modificar as políticas. |
> | Ação | Microsoft.DevTestLab/labs/read | Laboratórios de leitura. |
> | Ação | Microsoft.DevTestLab/labs/schedules/delete | Elimine agendas. |
> | Ação | Microsoft.DevTestLab/labs/schedules/Execute/action | Execute uma agenda. |
> | Ação | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Apresenta uma lista de todos os agendamentos aplicáveis |
> | Ação | Microsoft.DevTestLab/labs/schedules/read | As agendas de leitura. |
> | Ação | Microsoft.DevTestLab/labs/schedules/write | Adiciona ou modifica as agendas. |
> | Ação | Microsoft.DevTestLab/labs/serviceRunners/delete | Elimine runners de serviço. |
> | Ação | Microsoft.DevTestLab/labs/serviceRunners/read | Ler runners de serviço. |
> | Ação | Microsoft.DevTestLab/labs/serviceRunners/write | Adiciona ou modifica runners de serviço. |
> | Ação | Microsoft.DevTestLab/labs/users/delete | Elimine perfis de utilizador. |
> | Ação | Microsoft.DevTestLab/labs/users/disks/Attach/action | Ligar e criar a concessão do disco para a máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/users/disks/delete | Elimine os discos. |
> | Ação | Microsoft.DevTestLab/labs/users/disks/Detach/action | Desligue e quebra a concessão dos discos ligados à máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/users/disks/read | Leitura de discos. |
> | Ação | Microsoft.DevTestLab/labs/users/disks/write | Adiciona ou modifica os discos. |
> | Ação | Microsoft.DevTestLab/labs/users/environments/delete | Elimine ambientes. |
> | Ação | Microsoft.DevTestLab/labs/users/environments/read | Ambientes de leitura. |
> | Ação | Microsoft.DevTestLab/labs/users/environments/write | Adiciona ou modifica os ambientes. |
> | Ação | Microsoft.DevTestLab/labs/users/read | Perfis de utilizador de leitura. |
> | Ação | Microsoft.DevTestLab/labs/users/secrets/delete | Elimine segredos. |
> | Ação | Microsoft.DevTestLab/labs/users/secrets/read | Ler os segredos. |
> | Ação | Microsoft.DevTestLab/labs/users/secrets/write | Adiciona ou modifica os segredos. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Elimine recursos de infraestrutura de serviço. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Apresenta uma lista de todos os agendamentos aplicáveis |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Ler os recursos de infraestrutura de serviço. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Elimine agendas. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Execute uma agenda. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | As agendas de leitura. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Adiciona ou modifica as agendas. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Inicie uma infraestrutura de serviço. |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Parar uma infraestrutura de serviço |
> | Ação | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Adiciona ou modifica os recursos de infraestrutura de serviço. |
> | Ação | Microsoft.DevTestLab/labs/users/write | Adiciona ou modifica os perfis de utilizador. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Anexe um disco de dados nova ou existente à máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Aplicam-se de artefactos a máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Assumir a propriedade de uma máquina virtual existente |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/delete | Elimine as máquinas virtuais. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Desligar o disco da máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Apresenta uma lista de todos os agendamentos aplicáveis |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/read | Máquinas virtuais de leitura. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Volte a implementar uma máquina virtual |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Redimensione a Máquina Virtual. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Reinicie uma máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Elimine agendas. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Execute uma agenda. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | As agendas de leitura. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Adiciona ou modifica as agendas. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Inicie uma máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Parar uma máquina virtual |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Transferir a propriedade dos discos de dados de máquina virtual a próprio |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Propriedade de versão de uma máquina virtual existente |
> | Ação | Microsoft.DevTestLab/labs/virtualMachines/write | Adiciona ou modifica máquinas virtuais. |
> | Ação | Microsoft.DevTestLab/labs/virtualNetworks/delete | Elimine redes virtuais. |
> | Ação | Microsoft.DevTestLab/labs/virtualNetworks/read | Ler as redes virtuais. |
> | Ação | Microsoft.DevTestLab/labs/virtualNetworks/write | Adiciona ou modifica as redes virtuais. |
> | Ação | Microsoft.DevTestLab/labs/vmPools/delete | Elimine conjuntos de máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/vmPools/read | Ler conjuntos de máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/vmPools/write | Adicionar ou modificar conjuntos de máquina virtual. |
> | Ação | Microsoft.DevTestLab/labs/write | Adiciona ou modifica laboratórios. |
> | Ação | Microsoft.DevTestLab/locations/operations/read | Operações de leitura. |
> | Ação | Microsoft.DevTestLab/register/action | Regista a subscrição |
> | Ação | Microsoft.DevTestLab/schedules/delete | Elimine agendas. |
> | Ação | Microsoft.DevTestLab/schedules/Execute/action | Execute uma agenda. |
> | Ação | Microsoft.DevTestLab/schedules/read | As agendas de leitura. |
> | Ação | Microsoft.DevTestLab/schedules/Retarget/action | ID de recurso de destino de uma agenda de atualizações |
> | Ação | Microsoft.DevTestLab/schedules/write | Adiciona ou modifica as agendas. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DocumentDB/databaseAccountNames/read | Verifica a disponibilidade do nome. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Alterar o grupo de recursos de uma conta de base de dados |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Lê a coleção de definições de métrica. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Lê as métricas de coleção. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Base de dados conta partição chave as métricas de nível de leitura |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Ler métricas de nível da base de dados conta partição |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | As partições de conta de base de dados de uma coleção de leitura |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Ler a conta de base de dados utilizações de nível de partição |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Lê as utilizações de coleção. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Lê as definições de métrica de base de dados |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Lê as métricas de base de dados. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Lê as utilizações de base de dados. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/delete | Elimina as contas de base de dados. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Alterar as prioridades de ativação pós-falha de regiões de uma conta de base de dados. Isto é utilizado para efetuar a operação de ativação pós-falha manual |
> | Ação | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Obter as cadeias de ligação para uma conta de base de dados |
> | Ação | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Lista de chaves de uma conta de base de dados |
> | Ação | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Lê a base de dados definições de métrica de conta. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/metrics/read | Lê as métricas de conta de base de dados. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Offline uma região de uma conta de base de dados.  |
> | Ação | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Online uma região de uma conta de base de dados. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Ler o estado da operação assíncrona |
> | Ação | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Métricas de latência de leitura |
> | Ação | Microsoft.DocumentDB/databaseAccounts/percentile/read | Ler percentiles de latências de replicação |
> | Ação | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Ler métricas de latência para uma região de origem e de destino específica |
> | Ação | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Métricas de latência de leitura para uma região de destino específico |
> | Ação | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read | Obtém o catageries de registo disponível para a conta de base de dados |
> | Ação | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para a conta de base de dados |
> | Ação | Microsoft.DocumentDB/databaseAccounts/read | Lê uma conta de base de dados. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Lê a base de dados de chaves de conta de só de leitura. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Lê a base de dados de chaves de conta de só de leitura. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Rodar chaves de uma conta de base de dados |
> | Ação | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Lê as métricas de coleção regional. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Base de dados regionais conta partição chave as métricas de nível de leitura |
> | Ação | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Ler métricas de nível da base de dados regionais conta partição |
> | Ação | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | As partições de conta de base de dados regionais numa coleção de leitura |
> | Ação | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Lê as métricas de conta de base de dados e a região. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/usages/read | Lê a base de dados utilizações de conta. |
> | Ação | Microsoft.DocumentDB/databaseAccounts/write | Atualize contas de base de dados. |
> | Ação | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Notifica Microsoft.DocumentDB que está a ser eliminado VirtualNetwork ou sub-rede |
> | Ação | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Ler o estado de deleteVirtualNetworkOrSubnets operação assíncrona |
> | Ação | Microsoft.DocumentDB/operationResults/read | Ler o estado da operação assíncrona |
> | Ação | Microsoft.DocumentDB/operations/read | Operações de leitura disponíveis para a Microsoft DocumentDB  |
> | Ação | Microsoft.DocumentDB/register/action |  Registar o fornecedor de recursos do Microsoft DocumentDB para a subscrição |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DomainRegistration/checkDomainAvailability/Action | Verifique se um domínio está disponível para a compra |
> | Ação | Microsoft.DomainRegistration/domains/Delete | Elimine um domínio existente. |
> | Ação | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Eliminar o identificador de propriedade |
> | Ação | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Identificadores de propriedade de lista |
> | Ação | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Obter o identificador de propriedade |
> | Ação | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Criar ou atualizar o identificador |
> | Ação | Microsoft.DomainRegistration/domains/operationresults/Read | Obter uma operação de domínio |
> | Ação | Microsoft.DomainRegistration/domains/Read | Obter a lista de domínios |
> | Ação | Microsoft.DomainRegistration/domains/Read | Obter o domínio |
> | Ação | Microsoft.DomainRegistration/domains/renew/Action | Renove um domínio existente. |
> | Ação | Microsoft.DomainRegistration/domains/Write | Adicionar um novo domínio ou atualizar um já existente |
> | Ação | Microsoft.DomainRegistration/generateSsoRequest/Action | Gere um pedido de assinatura no Centro de controlo de domínio. |
> | Ação | Microsoft.DomainRegistration/listDomainRecommendations/Action | Obter as recomendações de domínio de lista com base em palavras-chave |
> | Ação | Microsoft.DomainRegistration/operations/Read | Lista todas as operações de registo de domínios do serviço de aplicações |
> | Ação | Microsoft.DomainRegistration/register/action | Registar o fornecedor de recursos Microsoft Domains para a subscrição |
> | Ação | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Ação de contrato de lista |
> | Ação | Microsoft.DomainRegistration/topLevelDomains/Read | Obter toplevel domínios |
> | Ação | Microsoft.DomainRegistration/topLevelDomains/Read | Obter toplevel domínio |
> | Ação | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Validar o objeto de compra de domínio sem submetê-la |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/read | Apresentar as implementações de avaliação do Microsoft Dynamics AX 2012 R3 num projeto dos serviços de ciclo de vida do Microsoft Dynamics que pertençam a um utilizador |
> | Ação | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/write | Crie implementação de avaliação do Microsoft Dynamics AX 2012 R3 num projeto dos serviços de ciclo de vida do Microsoft Dynamics que pertençam a um utilizador. Implementações podem ser geridas a partir do portal de gestão do Azure |
> | Ação | Microsoft.DynamicsLcs/lcsprojects/connectors/read | Conectores de leitura que pertençam a um projeto de serviços de ciclo de vida do Microsoft Dynamics |
> | Ação | Microsoft.DynamicsLcs/lcsprojects/connectors/write | Criar e atualizar os conectores que pertençam a um projeto de serviços de ciclo de vida do Microsoft Dynamics |
> | Ação | Microsoft.DynamicsLcs/lcsprojects/delete | Eliminar projetos de serviços de ciclo de vida do Microsoft Dynamics que pertencem ao utilizador |
> | Ação | Microsoft.DynamicsLcs/lcsprojects/read | Apresentar os projetos de serviços de ciclo de vida do Microsoft Dynamics que pertençam a um utilizador |
> | Ação | Microsoft.DynamicsLcs/lcsprojects/write | Criar e atualizar os projetos de serviços de ciclo de vida do Microsoft Dynamics que pertencem ao utilizador. Os nome e descrição propriedades só podem ser atualizadas. A subscrição e localização associadas ao projeto não podem ser atualizadas após a criação |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.EventGrid/eventSubscriptions/delete | Eliminar um eventSubscription |
> | Ação | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Obter o url completo para a subscrição de evento |
> | Ação | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico para subscrições de eventos |
> | Ação | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para subscrições de eventos |
> | Ação | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para eventSubscriptions |
> | Ação | Microsoft.EventGrid/eventSubscriptions/read | Ler um eventSubscription |
> | Ação | Microsoft.EventGrid/eventSubscriptions/write | Criar ou atualizar um eventSubscription |
> | Ação | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico para tópicos |
> | Ação | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para tópicos |
> | Ação | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para tópicos |
> | Ação | Microsoft.EventGrid/register/action | Regista o eventSubscription para o fornecedor de recursos EventGrid e permite a criação de subscrições de grelha de eventos. |
> | Ação | Microsoft.EventGrid/topics/delete | Eliminar um tópico |
> | Ação | Microsoft.EventGrid/topics/listKeys/action | Lista de chaves para o tópico |
> | Ação | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico para tópicos |
> | Ação | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para tópicos |
> | Ação | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para tópicos |
> | Ação | Microsoft.EventGrid/topics/read | Ler um tópico |
> | Ação | Microsoft.EventGrid/topics/regenerateKey/action | Voltar a gerar chave para o tópico |
> | Ação | Microsoft.EventGrid/topics/write | Criar ou atualizar um tópico |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.EventHub/checkNameAvailability/action | Verifica a disponibilidade do espaço de nomes em determinada subscrição. |
> | Ação | Microsoft.EventHub/checkNamespaceAvailability/action | Verifica a disponibilidade do espaço de nomes em determinada subscrição. Esta API foi preterida utilize CheckNameAvailabiltiy em vez disso. |
> | Ação | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de descrições de recurso as métricas de Cluster |
> | Ação | Microsoft.EventHub/clusters/read | Obtém a Descrição de Recurso de Cluster |
> | Ação | Microsoft.EventHub/clusters/write | Obtém a Descrição de Recurso de Cluster |
> | Ação | Microsoft.EventHub/namespaces/authorizationRules/action | Regra de autorização de espaço de nomes de atualizações. Esta API está depricated. Para atualizar a regra de autorização de espaço de nomes em vez disso, utilize uma chamada PUT... Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.EventHub/namespaces/authorizationRules/delete | Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido.  |
> | Ação | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Obter Cadeia de Ligação para o Espaço de nomes |
> | Ação | Microsoft.EventHub/namespaces/authorizationRules/read | Obter a lista de descrição das Regras de Autorização de Espaços de nomes. |
> | Ação | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.EventHub/namespaces/authorizationRules/write | Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas. |
> | Ação | Microsoft.EventHub/namespaces/Delete | Eliminar Recurso de Espaço de nomes |
> | Ação | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Obtém as chaves das regras de autorização do espaço de nomes principal da Recuperação Após Desastre |
> | Ação | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Obter as Regras de Autorização do Espaço de Nomes Principal da Recuperação Após Desastre |
> | Ação | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Desativa a Recuperação Após Desastre e para a replicação de alterações de espaço de nomes principais para secundários. |
> | Ação | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Verifica a disponibilidade do alias do espaço de nomes numa determinada subscrição. |
> | Ação | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Elimina a configuração de recuperação após desastre associada com o espaço de nomes. Esta operação só pode ser invocada através do espaço de nomes primário. |
> | Ação | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Invoca uma ativação pós-falha GEO DR e reconfigura o alias de espaço de nomes para apontar para o espaço de nomes secundário. |
> | Ação | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Obtém a configuração da Recuperação Após Desastre associada ao espaço de nomes. |
> | Ação | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Cria ou Atualiza a configuração da Recuperação Após Desastre associada ao espaço de nomes. |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Operação atualizar EventHub. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT. |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Operação para eliminar as regras de autorização de EventHub |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | Obter a cadeia de ligação a EventHub |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  Obter a lista de regras de autorização de EventHub |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Criar regras de autorização de EventHub e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados. |
> | Ação | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Operação para eliminar recurso ConsumerGroup |
> | Ação | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Obter a lista de descrições de recurso ConsumerGroup |
> | Ação | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Criar ou atualizar ConsumerGroup propriedades. |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/Delete | Operação para eliminar o recurso de EventHub |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/read | Obter a lista de descrições de recursos de EventHub |
> | Ação | Microsoft.EventHub/namespaces/eventhubs/write | Criar ou propriedades de EventHub de atualização. |
> | Ação | Microsoft.EventHub/namespaces/messagingPlan/read | Obtém o plano de serviço de mensagens para um espaço de nomes.<br>Esta API foi preterida.<br>As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)...<br>Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.EventHub/namespaces/messagingPlan/write | Atualiza o serviço de mensagens planear um espaço de nomes.<br>Esta API foi preterida.<br>As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)...<br>Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.EventHub/namespaces/operationresults/read | Obter o estado da operação de Espaço de nomes |
> | Ação | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes |
> | Ação | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes |
> | Ação | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Obter a lista de registos de espaço de nomes descrições de recursos |
> | Ação | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de descrições de recursos de métricas de espaço de nomes |
> | Ação | Microsoft.EventHub/namespaces/read | Obter a lista de Descrição de Recursos de Espaço de nomes |
> | Ação | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | Remova o espaço de nomes do ACS |
> | Ação | Microsoft.EventHub/namespaces/write | Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas. |
> | Ação | Microsoft.EventHub/operations/read | Obter operações |
> | Ação | Microsoft.EventHub/register/action | Regista a subscrição do fornecedor de recursos do EventHub e ativa a criação de recursos do EventHub |
> | Ação | Microsoft.EventHub/sku/read | Obter a lista de descrições de recursos de Sku |
> | Ação | Microsoft.EventHub/sku/regions/read | Obter a lista de descrições de recurso SkuRegions |
> | Ação | Microsoft.EventHub/unregister/action | Regista o Fornecedor de Recursos do EventHub |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Features/features/read | Obtém as funcionalidades de uma subscrição. |
> | Ação | Microsoft.Features/operations/read | Obtém a lista de operações. |
> | Ação | Microsoft.Features/providers/features/read | Obtém a funcionalidade de uma subscrição de um fornecedor de recursos específico. |
> | Ação | Microsoft.Features/providers/features/register/action | Regista a funcionalidade para uma subscrição de um fornecedor de recursos específico. |
> | Ação | Microsoft.Features/providers/features/unregister/action | Anula o registo da funcionalidade para uma subscrição de um fornecedor de recursos específico. |
> | Ação | Microsoft.Features/register/action | Regista a funcionalidade de uma subscrição. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.HDInsight/clusters/changerdpsetting/action | Alteração da definição de RDP de Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/configurations/action | Atualizar a configuração de Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/configurations/read | Obter configurações de Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/delete | Eliminar um Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico para o recurso de Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso de Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/read | Obter detalhes sobre o Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/roles/resize/action | Redimensionar um Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/clusters/write | Criar ou atualizar o Cluster do HDInsight |
> | Ação | Microsoft.HDInsight/locations/capabilities/read | Obter as funções da subscrição |
> | Ação | Microsoft.HDInsight/locations/checkNameAvailability/read | Verificar Disponibilidade do Nome |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ImportExport/jobs/delete | Elimina uma tarefa existente. |
> | Ação | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Obtém as chaves do BitLocker para a tarefa especificada. |
> | Ação | Microsoft.ImportExport/jobs/read | Obtém as propriedades para a tarefa especificada ou devolve a lista de tarefas. |
> | Ação | Microsoft.ImportExport/jobs/write | Cria uma tarefa com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a tarefa especificada. |
> | Ação | Microsoft.ImportExport/locations/read | Obtém as propriedades para a localização especificada ou devolve a lista de localizações. |
> | Ação | Microsoft.ImportExport/register/action | Regista a subscrição para o fornecedor de recursos de importação/exportação e permite a criação de tarefas de importação/exportação. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Insights/ActionGroups/Delete | A eliminar um grupo de ação |
> | Ação | Microsoft.Insights/ActionGroups/Read | A ler um grupo de ação |
> | Ação | Microsoft.Insights/ActionGroups/Write | A escrever um grupo de ação |
> | Ação | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Foi acionado o Alerta do Registo de Atividade |
> | Ação | Microsoft.Insights/ActivityLogAlerts/Delete | A eliminar um alerta do registo de atividade |
> | Ação | Microsoft.Insights/ActivityLogAlerts/Read | A ler um alerta do registo de atividade |
> | Ação | Microsoft.Insights/ActivityLogAlerts/Write | A ler um alerta do registo de atividade |
> | Ação | Microsoft.Insights/AlertRules/Activated/Action | Regra de Alerta ativada |
> | Ação | Microsoft.Insights/AlertRules/Delete | Eliminar uma configuração de regra de alerta |
> | Ação | Microsoft.Insights/AlertRules/Incidents/Read | Ler uma configuração de incidente da regra de alerta |
> | Ação | Microsoft.Insights/AlertRules/Read | Ler uma configuração de regra de alerta |
> | Ação | Microsoft.Insights/AlertRules/Resolved/Action | Regra de Alerta resolvida |
> | Ação | Microsoft.Insights/AlertRules/Throttled/Action | A regra de alerta está otimizada |
> | Ação | Microsoft.Insights/AlertRules/Write | Escrever numa configuração de regra de alerta |
> | Ação | Microsoft.Insights/AutoscaleSettings/Delete | Eliminar a configuração de uma definição de dimensionamento automático |
> | Ação | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Ler definições de métricas |
> | Ação | Microsoft.Insights/AutoscaleSettings/Read | Ler a configuração de uma definição de dimensionamento automático |
> | Ação | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Operação de redução do Dimensionamento Automático |
> | Ação | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Resultado da operação de redução do dimensionamento automático |
> | Ação | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Operação de aumento do Dimensionamento Automático |
> | Ação | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Resultado da operação de aumento do dimensionamento automático |
> | Ação | Microsoft.Insights/AutoscaleSettings/Write | Escrever numa configuração de definição de dimensionamento automático |
> | Ação | Microsoft.Insights/Components/AnalyticsItems/Delete | Eliminar um item de análise do Application Insights |
> | Ação | Microsoft.Insights/Components/AnalyticsItems/Read | A ler um item de análise do Application Insights |
> | Ação | Microsoft.Insights/Components/AnalyticsItems/Write | Escrever um item de análise do Application Insights |
> | Ação | Microsoft.Insights/Components/AnalyticsTables/Action | Ação de tabela de análise do Application Insights |
> | Ação | Microsoft.Insights/Components/AnalyticsTables/Delete | Eliminar um esquema de tabela de análise do Application Insights |
> | Ação | Microsoft.Insights/Components/AnalyticsTables/Read | A ler um esquema de tabela de análise do Application Insights |
> | Ação | Microsoft.Insights/Components/AnalyticsTables/Write | A escrever um esquema de tabela de análise do Application Insights |
> | Ação | Microsoft.Insights/Components/Annotations/Delete | Eliminar uma anotação do Application Insights |
> | Ação | Microsoft.Insights/Components/Annotations/Read | A ler uma anotação do Application Insights |
> | Ação | Microsoft.Insights/Components/Annotations/Write | A escrever uma anotação do Application Insights |
> | Ação | Microsoft.Insights/Components/Api/Read | A ler a API de dados de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/ApiKeys/Action | Gerar uma chave de API do Application Insights |
> | Ação | Microsoft.Insights/Components/ApiKeys/Delete | Eliminar uma chave de API do Application Insights |
> | Ação | Microsoft.Insights/Components/ApiKeys/Read | A ler uma chave de API do Application Insights |
> | Ação | Microsoft.Insights/Components/BillingPlanForComponent/Read | A ler um plano de faturação para o componente do Application Insights |
> | Ação | Microsoft.Insights/Components/CurrentBillingFeatures/Read | A ler as atuais funcionalidades de faturação para componente do Application Insights |
> | Ação | Microsoft.Insights/Components/CurrentBillingFeatures/Write | A escrever as atuais funcionalidades de faturação para componente do Application Insights |
> | Ação | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | A ler uma configuração de integração de ALM predefinida do Application Insights |
> | Ação | Microsoft.Insights/Components/Delete | A eliminar uma configuração de componentes de informações da aplicação |
> | Ação | Microsoft.Insights/Components/Events/Read | Obter registos do Application Insights com o formato de consulta do OData |
> | Ação | Microsoft.Insights/Components/ExportConfiguration/Action | Ação de definições de exportação do Application Insights |
> | Ação | Microsoft.Insights/Components/ExportConfiguration/Delete | A eliminar definições de exportação do Application Insights |
> | Ação | Microsoft.Insights/Components/ExportConfiguration/Read | A ler configurações de exportação do Application Insights |
> | Ação | Microsoft.Insights/Components/ExportConfiguration/Write | A escrever definições de exportação do Application Insights |
> | Ação | Microsoft.Insights/Components/ExtendQueries/Read | A ler os resultados de consulta estendida de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/Favorites/Delete | Eliminar um favorito do Application Insights |
> | Ação | Microsoft.Insights/Components/Favorites/Read | A ler um favorito do Application Insights |
> | Ação | Microsoft.Insights/Components/Favorites/Write | A escrever um favorito do Application Insights |
> | Ação | Microsoft.Insights/Components/FeatureCapabilities/Read | A ler as funcionalidades de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | A ler as funcionalidades de faturaçao disponíveis de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/GetToken/Read | A ler um token de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/MetricDefinitions/Read | A ler definições métricas de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/Metrics/Read | A ler métricas de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/Move/Action | Mover um Componente do Application Insights para outro grupo de recursos ou subscrição |
> | Ação | Microsoft.Insights/Components/MyAnalyticsItems/Delete | A eliminar um item de análise pessoal do Application Insights |
> | Ação | Microsoft.Insights/Components/MyAnalyticsItems/Read | A ler um item de análise pessoal do Application Insights |
> | Ação | Microsoft.Insights/Components/MyAnalyticsItems/Write | Escrever um item de análise pessoal do Application Insights |
> | Ação | Microsoft.Insights/Components/MyFavorites/Read | A ler um favorito pessoal do Application Insights |
> | Ação | Microsoft.Insights/Components/Operations/Read | Obter o estado de operações de execução longa no Application Insights |
> | Ação | Microsoft.Insights/Components/PricingPlans/Read | A ler um plano de preços de componentes do Application Insights |
> | Ação | Microsoft.Insights/Components/PricingPlans/Write | A escrever um plano de preços de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | A ler configuração de deteção proativa do Application Insights |
> | Ação | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | A escrever configuração de deteção proativa do Application Insights |
> | Ação | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Ler definições de métricas |
> | Ação | Microsoft.Insights/Components/Purge/Action | A remover os dados do Application Insights |
> | Ação | Microsoft.Insights/Components/Query/Read | Executar consultas aos registos do Application Insights |
> | Ação | Microsoft.Insights/Components/QuotaStatus/Read | A ler estado de quota de componente do Application Insights |
> | Ação | Microsoft.Insights/Components/Read | A ler uma configuração de componentes de informações da aplicação |
> | Ação | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | A ler as localizações de webtest do Application Insights |
> | Ação | Microsoft.Insights/Components/Webtests/Read | A ler uma configuração de webtest |
> | Ação | Microsoft.Insights/Components/WorkItemConfigs/Delete | Eliminar uma configuração de integração de ALM do Application Insights |
> | Ação | Microsoft.Insights/Components/WorkItemConfigs/Read | A ler uma configuração de integração de ALM do Application Insights |
> | Ação | Microsoft.Insights/Components/WorkItemConfigs/Write | A escrever uma configuração de integração de ALM predefinida do Application Insights |
> | Ação | Microsoft.Insights/Components/Write | A escrever para uma configuração de componentes de informações da aplicação |
> | Ação | Microsoft.Insights/DiagnosticSettings/Delete | A eliminar a configuração de definições de diagnóstico |
> | Ação | Microsoft.Insights/DiagnosticSettings/Read | A ler uma configuração de definições de diagnóstico |
> | Ação | Microsoft.Insights/DiagnosticSettings/Write | A escrever para a configuração de definições de diagnóstico |
> | Ação | Microsoft.Insights/EventCategories/Read | A ler uma categoria de eventos |
> | Ação | Microsoft.Insights/eventtypes/digestevents/Read | Ler resumo do tipo de eventos de gestão |
> | Ação | Microsoft.Insights/eventtypes/values/Read | Ler valores do tipo de eventos de gestão |
> | Ação | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | A eliminar a configuração de definições de diagnóstico avançadas |
> | Ação | Microsoft.Insights/ExtendedDiagnosticSettings/Read | A ler uma configuração das definições de diagnóstico avançadas |
> | Ação | Microsoft.Insights/ExtendedDiagnosticSettings/Write | A escrever para a configuração das definições de diagnóstico avançadas |
> | Ação | Microsoft.Insights/ListMigrationDate/Action | Voltar a obter a data de migração da Subscrição |
> | Ação | Microsoft.Insights/ListMigrationDate/Read | Voltar a obter a data de migração da subscrição |
> | Ação | Microsoft.Insights/LogDefinitions/Read | Ler definições de registo |
> | Ação | Microsoft.Insights/LogProfiles/Delete | Eliminar configuração dos perfis de registo |
> | Ação | Microsoft.Insights/LogProfiles/Read | Ler perfis de registo |
> | Ação | Microsoft.Insights/LogProfiles/Write | A escrever para uma configuração de perfil de registo |
> | Ação | Microsoft.Insights/MetricAlerts/Delete | A eliminar um alerta de métrica |
> | Ação | Microsoft.Insights/MetricAlerts/Read | A ler um alerta de métrica |
> | Ação | Microsoft.Insights/MetricAlerts/Status/Read | A ler o estado de alerta de métrica |
> | Ação | Microsoft.Insights/MetricAlerts/Write | A escrever um alerta de métrica |
> | Ação | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Ler definições de métricas |
> | Ação | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Ler definições de métricas |
> | Ação | Microsoft.Insights/MetricDefinitions/Read | Ler definições de métricas |
> | Ação | Microsoft.Insights/Metrics/providers/Metrics/Read | Ler métricas |
> | Ação | Microsoft.Insights/Metrics/Read | Ler métricas |
> | Ação | Microsoft.Insights/Metrics/Write | Escrever métricas |
> | Ação | Microsoft.Insights/MigrateToNewpricingModel/Action | Migrar subscrição para o novo modelo de preços |
> | Ação | Microsoft.Insights/Operations/Read | A ler operações |
> | Ação | Microsoft.Insights/Register/Action | Registar o fornecedor de microsoft insights |
> | Ação | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Anular subscrição para o modelo de preços legado |
> | Ação | Microsoft.Insights/ScheduledQueryRules/Delete | A eliminar uma regra de consulta agendada |
> | Ação | Microsoft.Insights/ScheduledQueryRules/Read | A ler uma regra de consulta agendada |
> | Ação | Microsoft.Insights/ScheduledQueryRules/Write | A escrever uma regra de consulta agendada |
> | Ação | Microsoft.Insights/Tenants/Register/Action | Iniciar o fornecedor de informações da Microsoft |
> | Ação | Microsoft.Insights/Unregister/Action | Registar o fornecedor de microsoft insights |
> | Ação | Microsoft.Insights/Webtests/Delete | A eliminar uma configuração de webtest |
> | Ação | Microsoft.Insights/Webtests/GetToken/Read | A ler um token de webtest |
> | Ação | Microsoft.Insights/Webtests/MetricDefinitions/Read | A ler definições de métrica de webtest |
> | Ação | Microsoft.Insights/Webtests/Metrics/Read | A ler métricas de webtest |
> | Ação | Microsoft.Insights/Webtests/Read | A ler uma configuração de webtest |
> | Ação | Microsoft.Insights/Webtests/Write | A escrever para uma configuração de webtest |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.KeyVault/checkNameAvailability/read | Verifica se um nome de cofre de chaves é válido e se não está em utilização |
> | Ação | Microsoft.KeyVault/deletedVaults/read | Visualizar as propriedades dos cofres de chaves eliminados de forma recuperável |
> | Ação | Microsoft.KeyVault/hsmPools/delete | Eliminar um conjunto de HSM |
> | Ação | Microsoft.KeyVault/hsmPools/joinVault/action | Associar um cofre de chaves a um conjunto de HSM |
> | Ação | Microsoft.KeyVault/hsmPools/read | Visualizar as propriedades de um conjunto HSM |
> | Ação | Microsoft.KeyVault/hsmPools/write | Criar um novo conjunto de HSM de atualização das propriedades de um conjunto de HSM existente |
> | Ação | Microsoft.KeyVault/locations/deletedVaults/purge/action | Remover um cofre de chaves eliminado de forma recuperável |
> | Ação | Microsoft.KeyVault/locations/deletedVaults/read | Visualizar as propriedades de um cofre de chaves eliminado de forma recuperável |
> | Ação | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Notifica o Microsoft.KeyVault de que uma rede virtual ou sub-rede está a ser eliminada |
> | Ação | Microsoft.KeyVault/locations/operationResults/read | Verificar o resultado de uma operação de execução longa |
> | Ação | Microsoft.KeyVault/operations/read | Lista as operações disponíveis no fornecedor de recursos Microsoft.KeyVault |
> | Ação | Microsoft.KeyVault/register/action | Regista uma subscrição |
> | Ação | Microsoft.KeyVault/unregister/action | Cancela o registo de uma subscrição |
> | Ação | Microsoft.KeyVault/vaults/accessPolicies/write | Atualize uma política de acesso existente através da intercalação ou substituição, ou adicione uma nova política de acesso ao cofre. |
> | Ação | Microsoft.KeyVault/vaults/delete | Eliminar um cofre de chaves |
> | Ação | Microsoft.KeyVault/vaults/deploy/action | Permite o acesso a segredos num cofre de chaves ao implementar os recursos do Azure |
> | Ação | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para um cofre de chaves |
> | Ação | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/metricDefinitions/read | Obtém as métricas disponíveis de um cofre de chaves |
> | Ação | Microsoft.KeyVault/vaults/read | Ver as propriedades de um cofre de chaves |
> | Ação | Microsoft.KeyVault/vaults/secrets/read | Ver as propriedades de um segredo, mas não o seu valor |
> | Ação | Microsoft.KeyVault/vaults/secrets/write | Crie um novo segredo ou atualize o valor de um segredo existente |
> | Ação | Microsoft.KeyVault/vaults/write | Criar um novo cofre de chaves ou atualizar as propriedades de um cofre de chaves existente |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Kusto/Clusters/Databases/delete | Elimina um recurso de base de dados. |
> | Ação | Microsoft.Kusto/Clusters/Databases/read | Lê um recurso de base de dados. |
> | Ação | Microsoft.Kusto/Clusters/Databases/write | Escreve um recurso de base de dados. |
> | Ação | Microsoft.Kusto/Clusters/delete | Elimina um recurso de cluster. |
> | Ação | Microsoft.Kusto/Clusters/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico para o recurso |
> | Ação | Microsoft.Kusto/Clusters/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Kusto/Clusters/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições de métricas do recurso |
> | Ação | Microsoft.Kusto/Clusters/read | Lê um recurso de cluster. |
> | Ação | Microsoft.Kusto/Clusters/write | Escreve um recurso de cluster. |
> | Ação | Microsoft.Kusto/Locations/CheckNameAvailability/write | Leituras de recurso de nome de disponibilidade de verificação |
> | Ação | Microsoft.Kusto/locations/operationresults/read | Lê recursos de operações |
> | Ação | Microsoft.Kusto/Operations/read | Lê recursos de operações |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.LabServices/labAccounts/CreateLab/action | Crie um laboratório numa conta de laboratório. |
> | Ação | Microsoft.LabServices/labAccounts/delete | Elimine contas de laboratório. |
> | Ação | Microsoft.LabServices/labAccounts/galleryImages/read | Ler imagens gallery. |
> | Ação | Microsoft.LabServices/labAccounts/galleryImages/write | Adiciona ou modifica as imagens de galeria. |
> | Ação | Microsoft.LabServices/labAccounts/labs/delete | Elimine laboratórios. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Um ambiente aleatório para um utilizador nas definições do ambiente de afirmações |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Elimine a definição de ambiente. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | O ambiente de afirmações e atribui-o para o utilizador |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Elimine ambientes. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Ambientes de leitura. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Inicia um ambiente iniciando todos os recursos do ambiente. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Parar um ambiente, parando todos os recursos do ambiente |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Adiciona ou modifica os ambientes. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Aprovisiona/deprovisions necessários recursos para um definição de ambiente com base no estado atual da definição de ambiente de laboratório /. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Definição de ambiente de leitura. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Inicia um modelo ao iniciar a todos os recursos dentro do modelo. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Inicia um modelo ao iniciar a todos os recursos dentro do modelo. |
> | Ação | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Adicionar ou modificar a definição do ambiente. |
> | Ação | Microsoft.LabServices/labAccounts/labs/read | Laboratórios de leitura. |
> | Ação | Microsoft.LabServices/labAccounts/labs/Register/action | Registar-se no laboratório gerido. |
> | Ação | Microsoft.LabServices/labAccounts/labs/users/delete | Elimine utilizadores. |
> | Ação | Microsoft.LabServices/labAccounts/labs/users/read | Os utilizadores de leitura. |
> | Ação | Microsoft.LabServices/labAccounts/labs/users/write | Adiciona ou modifica os utilizadores. |
> | Ação | Microsoft.LabServices/labAccounts/labs/write | Adiciona ou modifica laboratórios. |
> | Ação | Microsoft.LabServices/labAccounts/read | Ler as contas de laboratório. |
> | Ação | Microsoft.LabServices/labAccounts/sizes/ListAvailableSkus/action | Listar SKUs disponíveis para cada tipo de tamanho numa conta do laboratório |
> | Ação | Microsoft.LabServices/labAccounts/sizes/read | Os tamanhos de leitura. |
> | Ação | Microsoft.LabServices/labAccounts/write | Adiciona ou modifica contas de laboratório. |
> | Ação | Microsoft.LabServices/locations/operations/read | Operações de leitura. |
> | Ação | Microsoft.LabServices/register/action | Regista a subscrição |
> | Ação | Microsoft.LabServices/users/GetEnvironment/action | Obtém os detalhes da máquina virtual |
> | Ação | Microsoft.LabServices/users/GetOperationStatus/action | Obtém o estado da operação de execução longa |
> | Ação | Microsoft.LabServices/users/ListEnvironments/action | Ambientes de lista para o utilizador |
> | Ação | Microsoft.LabServices/users/ListLabs/action | Laboratórios de lista para o utilizador. |
> | Ação | Microsoft.LabServices/users/Register/action | Registar um utilizador para um laboratório gerido |
> | Ação | Microsoft.LabServices/users/StartEnvironment/action | Inicia um ambiente iniciando todos os recursos do ambiente. |
> | Ação | Microsoft.LabServices/users/StopEnvironment/action | Parar um ambiente, parando todos os recursos do ambiente |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.LocationBasedServices/accounts/delete | (Preterido: Utilize /providers/Microsoft.Maps) Eliminar uma localização com base dos serviços de conta. |
> | Ação | Microsoft.LocationBasedServices/accounts/listKeys/action | (Preterido: Utilize /providers/Microsoft.Maps) Listar chaves de conta de serviços com base da localização |
> | Ação | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | (Preterido: Utilize /providers/Microsoft.Maps) Obtém a definição de diagnóstico para o recurso |
> | Ação | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | (Preterido: Utilize /providers/Microsoft.Maps) Cria ou atualiza a definição de diagnóstico para o recurso |
> | Ação | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | (Preterido: Utilize /providers/Microsoft.Maps) Obtem as métricas disponíveis para a localização com base em contas de serviço |
> | Ação | Microsoft.LocationBasedServices/accounts/read | (Preterido: Utilize /providers/Microsoft.Maps) Obter uma localização com base dos serviços de conta. |
> | Ação | Microsoft.LocationBasedServices/accounts/regenerateKey/action | (Preterido: Utilize /providers/Microsoft.Maps) Gerar a nova localização com base em serviços primário ou secundário chave da conta |
> | Ação | Microsoft.LocationBasedServices/accounts/write | (Preterido: Utilize /providers/Microsoft.Maps) Criar ou atualizar uma conta de serviços com base da localização. |
> | Ação | Microsoft.LocationBasedServices/register/action | (Preterido: Utilize /providers/Microsoft.Maps) Registar o fornecedor |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | DataAction | Microsoft.LogAnalytics/logs/ADAssessmentRecommendation/read | Ler dados a partir da tabela ADAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/ADReplicationResult/read | Ler dados a partir da tabela ADReplicationResult |
> | DataAction | Microsoft.LogAnalytics/logs/ADSecurityAssessmentRecommendation/read | Ler dados a partir da tabela ADSecurityAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/Alert/read | Ler os dados da tabela de alerta |
> | DataAction | Microsoft.LogAnalytics/logs/AlertHistory/read | Ler dados a partir da tabela AlertHistory |
> | DataAction | Microsoft.LogAnalytics/logs/ApplicationInsights/read | Ler dados a partir da tabela ApplicationInsights |
> | DataAction | Microsoft.LogAnalytics/logs/AzureActivity/read | Ler dados a partir da tabela AzureActivity |
> | DataAction | Microsoft.LogAnalytics/logs/AzureMetrics/read | Ler dados a partir da tabela AzureMetrics |
> | DataAction | Microsoft.LogAnalytics/logs/BoundPort/read | Ler dados a partir da tabela BoundPort |
> | DataAction | Microsoft.LogAnalytics/logs/CommonSecurityLog/read | Ler dados a partir da tabela CommonSecurityLog |
> | DataAction | Microsoft.LogAnalytics/logs/ComputerGroup/read | Ler dados a partir da tabela do grupo de computador |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationChange/read | Ler dados a partir da tabela ConfigurationChange |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationData/read | Ler dados a partir da tabela ConfigurationData |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerImageInventory/read | Ler dados a partir da tabela ContainerImageInventory |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerInventory/read | Ler dados a partir da tabela ContainerInventory |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerLog/read | Ler dados a partir da tabela ContainerLog |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerServiceLog/read | Ler dados a partir da tabela ContainerServiceLog |
> | DataAction | Microsoft.LogAnalytics/logs/CustomLogs/read | A leitura de dados a partir de qualquer registo personalizado |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppCrash/read | Ler dados a partir da tabela DeviceAppCrash |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppLaunch/read | Ler dados a partir da tabela DeviceAppLaunch |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCalendar/read | Ler dados a partir da tabela DeviceCalendar |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCleanup/read | Ler dados a partir da tabela DeviceCleanup |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceConnectSession/read | Ler dados a partir da tabela DeviceConnectSession |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceEtw/read | Ler dados a partir da tabela DeviceEtw |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHardwareHealth/read | Ler dados a partir da tabela DeviceHardwareHealth |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHealth/read | Ler dados a partir da tabela DeviceHealth |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHeartbeat/read | Ler dados a partir da tabela DeviceHeartbeat |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeHeartbeat/read | Ler dados a partir da tabela DeviceSkypeHeartbeat |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeSignIn/read | Ler dados a partir da tabela DeviceSkypeSignIn |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSleepState/read | Ler dados a partir da tabela DeviceSleepState |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppFailure/read | Ler dados a partir da tabela DHAppFailure |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppReliability/read | Ler dados a partir da tabela DHAppReliability |
> | DataAction | Microsoft.LogAnalytics/logs/DHDriverReliability/read | Ler dados a partir da tabela DHDriverReliability |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonFailures/read | Ler dados a partir da tabela DHLogonFailures |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonMetrics/read | Ler dados a partir da tabela DHLogonMetrics |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSCrashData/read | Ler dados a partir da tabela DHOSCrashData |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSReliability/read | Ler dados a partir da tabela DHOSReliability |
> | DataAction | Microsoft.LogAnalytics/logs/DHWipAppLearning/read | Ler dados a partir da tabela DHWipAppLearning |
> | DataAction | Microsoft.LogAnalytics/logs/DnsEvents/read | Ler dados a partir da tabela DnsEvents |
> | DataAction | Microsoft.LogAnalytics/logs/DnsInventory/read | Ler dados a partir da tabela DnsInventory |
> | DataAction | Microsoft.LogAnalytics/logs/ETWEvent/read | Ler dados a partir da tabela ETWEvent |
> | DataAction | Microsoft.LogAnalytics/logs/Event/read | Ler dados a partir da tabela de eventos |
> | DataAction | Microsoft.LogAnalytics/logs/ExchangeAssessmentRecommendation/read | Ler dados a partir da tabela ExchangeAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/Heartbeat/read | Ler dados a partir da tabela de Heartbeat |
> | DataAction | Microsoft.LogAnalytics/logs/IISAssessmentRecommendation/read | Ler dados a partir da tabela IISAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/InboundConnection/read | Ler dados a partir da tabela InboundConnection |
> | DataAction | Microsoft.LogAnalytics/logs/KubeNodeInventory/read | Ler dados a partir da tabela KubeNodeInventory |
> | DataAction | Microsoft.LogAnalytics/logs/KubePodInventory/read | Ler dados a partir da tabela KubePodInventory |
> | DataAction | Microsoft.LogAnalytics/logs/LinuxAuditLog/read | Ler dados a partir da tabela LinuxAuditLog |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplication/read | Ler dados a partir da tabela MAApplication |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealth/read | Ler dados a partir da tabela MAApplicationHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationInstance/read | Ler dados a partir da tabela MAApplicationInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationReadiness/read | Ler dados a partir da tabela MAApplicationReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MADeploymentPlan/read | Ler dados a partir da tabela MADeploymentPlan |
> | DataAction | Microsoft.LogAnalytics/logs/MADevice/read | Ler dados a partir da tabela MADevice |
> | DataAction | Microsoft.LogAnalytics/logs/MADeviceReadiness/read | Ler dados a partir da tabela MADeviceReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MADriverReadiness/read | Ler dados a partir da tabela MADriverReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddin/read | Ler dados a partir da tabela MAOfficeAddin |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinHealth/read | Ler dados a partir da tabela MAOfficeAddinHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinInstance/read | Ler dados a partir da tabela MAOfficeAddinInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinReadiness/read | Ler dados a partir da tabela MAOfficeAddinReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeApp/read | Ler dados a partir da tabela MAOfficeApp |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppHealth/read | Ler dados a partir da tabela MAOfficeAppHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppInstance/read | Ler dados a partir da tabela MAOfficeAppInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppReadiness/read | Ler dados a partir da tabela MAOfficeAppReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeBuildInfo/read | Ler dados a partir da tabela MAOfficeBuildInfo |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessment/read | Ler dados a partir da tabela MAOfficeCurrencyAssessment |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessmentDailyCounts/read | Ler dados a partir da tabela MAOfficeCurrencyAssessmentDailyCounts |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeDeploymentStatus/read | Ler dados a partir da tabela MAOfficeDeploymentStatus |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroIssueReadiness/read | Ler dados a partir da tabela MAOfficeMacroIssueReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroSummary/read | Ler dados a partir da tabela MAOfficeMacroSummary |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuite/read | Ler dados a partir da tabela MAOfficeSuite |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuiteInstance/read | Ler dados a partir da tabela MAOfficeSuiteInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAProposedPilotDevices/read | Ler dados a partir da tabela MAProposedPilotDevices |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsBuildInfo/read | Ler dados a partir da tabela MAWindowsBuildInfo |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessment/read | Ler dados a partir da tabela MAWindowsCurrencyAssessment |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessmentDailyCounts/read | Ler dados a partir da tabela MAWindowsCurrencyAssessmentDailyCounts |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsDeploymentStatus/read | Ler dados a partir da tabela MAWindowsDeploymentStatus |
> | DataAction | Microsoft.LogAnalytics/logs/NetworkMonitoring/read | Ler dados a partir da tabela NetworkMonitoring |
> | DataAction | Microsoft.LogAnalytics/logs/OfficeActivity/read | Ler dados a partir da tabela OfficeActivity |
> | DataAction | Microsoft.LogAnalytics/logs/Operation/read | Ler dados a partir da tabela de operação |
> | DataAction | Microsoft.LogAnalytics/logs/OutboundConnection/read | Ler dados a partir da tabela OutboundConnection |
> | DataAction | Microsoft.LogAnalytics/logs/Perf/read | Ler dados a partir da tabela de desempenho |
> | DataAction | Microsoft.LogAnalytics/logs/ProtectionStatus/read | Ler dados a partir da tabela ProtectionStatus |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedAzureCommonFields/read | Ler dados a partir da tabela ReservedAzureCommonFields |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedCommonFields/read | Ler dados a partir da tabela ReservedCommonFields |
> | DataAction | Microsoft.LogAnalytics/logs/SCCMAssessmentRecommendation/read | Ler dados a partir da tabela SCCMAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SCOMAssessmentRecommendation/read | Ler dados a partir da tabela SCOMAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityAlert/read | Ler dados a partir da tabela SecurityAlert |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaseline/read | Ler dados a partir da tabela SecurityBaseline |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaselineSummary/read | Ler dados a partir da tabela SecurityBaselineSummary |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityDetection/read | Ler dados a partir da tabela SecurityDetection |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityEvent/read | Ler dados a partir da tabela SecurityEvent |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricOperationalEvent/read | Ler dados a partir da tabela ServiceFabricOperationalEvent |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableActorEvent/read | Ler dados a partir da tabela ServiceFabricReliableActorEvent |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableServiceEvent/read | Ler dados a partir da tabela ServiceFabricReliableServiceEvent |
> | DataAction | Microsoft.LogAnalytics/logs/SfBAssessmentRecommendation/read | Ler dados a partir da tabela SfBAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SPAssessmentRecommendation/read | Ler dados a partir da tabela SPAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SQLAssessmentRecommendation/read | Ler dados a partir da tabela SQLAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SQLQueryPerformance/read | Ler dados a partir da tabela SQLQueryPerformance |
> | DataAction | Microsoft.LogAnalytics/logs/Syslog/read | Ler dados a partir da tabela Syslog |
> | DataAction | Microsoft.LogAnalytics/logs/SysmonEvent/read | Ler dados a partir da tabela SysmonEvent |
> | DataAction | Microsoft.LogAnalytics/logs/UAApp/read | Ler dados a partir da tabela UAApp |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputer/read | Ler dados a partir da tabela UAComputer |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputerRank/read | Ler dados a partir da tabela UAComputerRank |
> | DataAction | Microsoft.LogAnalytics/logs/UADriver/read | Ler dados a partir da tabela UADriver |
> | DataAction | Microsoft.LogAnalytics/logs/UADriverProblemCodes/read | Ler dados a partir da tabela UADriverProblemCodes |
> | DataAction | Microsoft.LogAnalytics/logs/UAFeedback/read | Ler dados a partir da tabela UAFeedback |
> | DataAction | Microsoft.LogAnalytics/logs/UAHardwareSecurity/read | Ler dados a partir da tabela UAHardwareSecurity |
> | DataAction | Microsoft.LogAnalytics/logs/UAIESiteDiscovery/read | Ler dados a partir da tabela UAIESiteDiscovery |
> | DataAction | Microsoft.LogAnalytics/logs/UAOfficeAddIn/read | Ler dados a partir da tabela UAOfficeAddIn |
> | DataAction | Microsoft.LogAnalytics/logs/UAProposedActionPlan/read | Ler dados a partir da tabela UAProposedActionPlan |
> | DataAction | Microsoft.LogAnalytics/logs/UASysReqIssue/read | Ler dados a partir da tabela UASysReqIssue |
> | DataAction | Microsoft.LogAnalytics/logs/UAUpgradedComputer/read | Ler dados a partir da tabela UAUpgradedComputer |
> | DataAction | Microsoft.LogAnalytics/logs/Update/read | Ler dados a partir da tabela de atualização |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateRunProgress/read | Ler dados a partir da tabela UpdateRunProgress |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateSummary/read | Ler dados a partir da tabela UpdateSummary |
> | DataAction | Microsoft.LogAnalytics/logs/Usage/read | Ler dados a partir da tabela de utilização |
> | DataAction | Microsoft.LogAnalytics/logs/W3CIISLog/read | Ler dados a partir da tabela W3CIISLog |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSDeploymentStatus/read | Ler dados a partir da tabela WaaSDeploymentStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSInsiderStatus/read | Ler dados a partir da tabela WaaSInsiderStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSUpdateStatus/read | Ler dados a partir da tabela WaaSUpdateStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVStatus/read | Ler dados a partir da tabela WDAVStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVThreat/read | Ler dados a partir da tabela WDAVThreat |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsClientAssessmentRecommendation/read | Ler dados a partir da tabela WindowsClientAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsFirewall/read | Ler dados a partir da tabela WindowsFirewall |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsServerAssessmentRecommendation/read | Ler dados a partir da tabela WindowsServerAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/WireData/read | Ler dados a partir da tabela WireData |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOAggregatedStatus/read | Ler dados a partir da tabela WUDOAggregatedStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOStatus/read | Ler dados a partir da tabela WUDOStatus |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Logic/integrationAccounts/agreements/delete | Elimina o acordo na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Obtém o URL de chamada de retorno para o conteúdo do acordo na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/agreements/read | Lê o acordo na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/agreements/write | Cria ou atualiza o acordo na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/assemblies/delete | Elimina a assemblagem na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Obtém o URL de chamada de retorno para o conteúdo da assemblagem na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/assemblies/read | Lê a assemblagem na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/assemblies/write | Cria ou atualiza a assemblagem na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Elimina a configuração do lote na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Lê a configuração do lote na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Cria ou atualiza a configuração de lote na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/certificates/delete | Elimina o certificado na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/certificates/read | Lê o certificado na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/certificates/write | Cria ou atualiza o certificado na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/delete | Elimina a conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/join/action | Adere à Conta de Integração. |
> | Ação | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Obtém o URL de chamada de retorno para a conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Obtém as chaves no cofre de chaves. |
> | Ação | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Regista os eventos de rastreio na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/maps/delete | Elimina o mapa na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Obtém o URL de chamada de retorno para o conteúdo do mapa na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/maps/read | Lê o mapa na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/maps/write | Cria ou atualiza o mapa na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/partners/delete | Elimina o parceiro na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Obtém o URL de chamada de retorno para o conteúdo de parceiro na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/partners/read | Lê o parceiro na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/partners/write | Cria ou atualiza o parceiro na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Lê as definições de registo da Conta de Integração. |
> | Ação | Microsoft.Logic/integrationAccounts/read | Lê a conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Regenera os segredos da chave de acesso. |
> | Ação | Microsoft.Logic/integrationAccounts/schemas/delete | Elimina o esquema na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Obtém o URL de chamada de retorno para o conteúdo do esquema na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/schemas/read | Lê o esquema na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/schemas/write | Cria ou atualiza o esquema na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/sessions/delete | Elimina a sessão na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/sessions/read | Lê a configuração do lote na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/sessions/write | Cria ou atualiza a sessão na conta de integração. |
> | Ação | Microsoft.Logic/integrationAccounts/write | Cria ou atualiza a conta de integração. |
> | Ação | Microsoft.Logic/isolatedEnvironments/delete | Elimina o ambiente isolado. |
> | Ação | Microsoft.Logic/isolatedEnvironments/join/action | Associa o Ambiente Isolado. |
> | Ação | Microsoft.Logic/isolatedEnvironments/read | Lê o ambiente isolado. |
> | Ação | Microsoft.Logic/isolatedEnvironments/write | Cria ou atualiza o ambiente isolado. |
> | Ação | Microsoft.Logic/locations/workflows/validate/action | Valida o fluxo de trabalho. |
> | Ação | Microsoft.Logic/operations/read | Obtém a operação. |
> | Ação | Microsoft.Logic/register/action | Regista o fornecedor de recursos de Microsoft.Logic para uma determinada subscrição. |
> | Ação | Microsoft.Logic/workflows/accessKeys/delete | Elimina a chave de acesso. |
> | Ação | Microsoft.Logic/workflows/accessKeys/list/action | Lista os segredos da chave de acesso. |
> | Ação | Microsoft.Logic/workflows/accessKeys/read | Lê a chave de acesso. |
> | Ação | Microsoft.Logic/workflows/accessKeys/regenerate/action | Regenera os segredos da chave de acesso. |
> | Ação | Microsoft.Logic/workflows/accessKeys/write | Cria ou atualiza a chave de acesso. |
> | Ação | Microsoft.Logic/workflows/delete | Elimina o fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/disable/action | Desativa o fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/enable/action | Ativa o fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/listCallbackUrl/action | Obtém o URL de chamada de retorno do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/listSwagger/action | Obtém as definições do swagger do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/move/action | Move o Fluxo de trabalho do ID de subscrição, grupo de recursos e/ou nome existentes para um ID de subscrição, grupo de recursos e/ou nome diferentes. |
> | Ação | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Lê as definições de diagnóstico do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Lê as definições de registo do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Lê as definições de métrica do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/read | Lê o fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/regenerateAccessKey/action | Regenera os segredos da chave de acesso. |
> | Ação | Microsoft.Logic/workflows/run/action | Inicia a execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Obtém os rastreios de expressão da ação de execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/actions/read | Lê a ação de execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Obtém os rastreios de expressão da repetição da ação de execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/actions/repetitions/read | Lê a repetição da ação de execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | Lê o histórico de pedidos de ação de repetição da execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/actions/requestHistories/read | Lê o histórico de pedidos de ação de execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Lê a repetição do âmbito da ação de execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/cancel/action | Cancela a execução de um fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/operations/read | Lê o estado da operação de execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/runs/read | Lê a execução do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/suspend/action | Suspende o fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/triggers/histories/read | Lê o histórico do acionador. |
> | Ação | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Submete novamente o acionador do fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Obtém o URL de chamada de retorno para o acionador. |
> | Ação | Microsoft.Logic/workflows/triggers/read | Lê o acionador. |
> | Ação | Microsoft.Logic/workflows/triggers/reset/action | Repõe o acionador. |
> | Ação | Microsoft.Logic/workflows/triggers/run/action | Executa o acionador. |
> | Ação | Microsoft.Logic/workflows/triggers/setState/action | Define o estado do acionador. |
> | Ação | Microsoft.Logic/workflows/validate/action | Valida o fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/versions/read | Lê a versão de fluxo de trabalho. |
> | Ação | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Obtém o URL de chamada de retorno para o acionador. |
> | Ação | Microsoft.Logic/workflows/write | Cria ou atualiza o fluxo de trabalho. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Mover quaisquer do Machine Learning compromisso plano de associação |
> | Ação | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Qualquer do Machine Learning compromisso plano de associação de leitura |
> | Ação | Microsoft.MachineLearning/commitmentPlans/delete | Eliminar qualquer compromisso plano de aprendizagem |
> | Ação | Microsoft.MachineLearning/commitmentPlans/join/action | Aderir a qualquer máquina compromisso plano de aprendizagem |
> | Ação | Microsoft.MachineLearning/commitmentPlans/read | Ler qualquer compromisso plano de aprendizagem |
> | Ação | Microsoft.MachineLearning/commitmentPlans/write | Criar ou atualizar qualquer plano de compromisso do Machine Learning |
> | Ação | Microsoft.MachineLearning/locations/operationresults/read | Obter Resultado de uma operação do Machine Learning |
> | Ação | Microsoft.MachineLearning/locations/operationsstatus/read | Obter o estado de uma operação de aprendizagem máquina em curso |
> | Ação | Microsoft.MachineLearning/operations/read | Obter operações de aprendizagem |
> | Ação | Microsoft.MachineLearning/register/action | Regista a subscrição para a fornecedor de recursos do serviço web de aprendizagem e permite a criação de serviços web. |
> | Ação | Microsoft.MachineLearning/skus/read | Obter do Machine Learning SKUs do plano de compromisso |
> | Ação | Microsoft.MachineLearning/webServices/action | Criar as propriedades do serviço Web regional para as regiões suportadas |
> | Ação | Microsoft.MachineLearning/webServices/delete | Eliminar qualquer serviço Web do Machine Learning |
> | Ação | Microsoft.MachineLearning/webServices/listkeys/read | Obter chaves para um serviço Web do Machine Learning |
> | Ação | Microsoft.MachineLearning/webServices/read | Ler a qualquer serviço Web do Machine Learning |
> | Ação | Microsoft.MachineLearning/webServices/write | Criar ou atualizar qualquer serviço Web do Machine Learning |
> | Ação | Microsoft.MachineLearning/Workspaces/delete | Eliminar qualquer área de trabalho de aprendizagem |
> | Ação | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Lista de chaves para uma área de trabalho do Machine Learning |
> | Ação | Microsoft.MachineLearning/Workspaces/read | Ler qualquer área de trabalho de aprendizagem |
> | Ação | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Ressincronizar chaves de conta de armazenamento configurada para uma área de trabalho do Machine Learning |
> | Ação | Microsoft.MachineLearning/Workspaces/write | Criar ou atualizar qualquer área de trabalho de aprendizagem |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.MachineLearningCompute/operationalizationClusters/checkUpdate/action | Verifique se as atualizações estão disponíveis para os serviços de sistema para o cluster operationalization |
> | Ação | Microsoft.MachineLearningCompute/operationalizationClusters/delete | Eliminar qualquer conta de alojamento |
> | Ação | Microsoft.MachineLearningCompute/operationalizationClusters/listKeys/action | Listar as chaves associadas ao operationalization cluster |
> | Ação | Microsoft.MachineLearningCompute/operationalizationClusters/read | Qualquer conta de alojamento de leitura |
> | Ação | Microsoft.MachineLearningCompute/operationalizationClusters/updateSystem/action | Atualizar os serviços do sistema de um cluster de operationalization |
> | Ação | Microsoft.MachineLearningCompute/operationalizationClusters/write | Criar ou atualizar qualquer conta de alojamento |
> | Ação | Microsoft.MachineLearningCompute/register/action | Regista o ID de subscrição para o fornecedor de recursos e permite a criação de um de machine learning recursos de computação |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.MachineLearningModelManagement/accounts/delete | Eliminar qualquer conta de alojamento |
> | Ação | Microsoft.MachineLearningModelManagement/accounts/read | Qualquer conta de alojamento de leitura |
> | Ação | Microsoft.MachineLearningModelManagement/accounts/write | Criar ou atualizar qualquer conta de alojamento |
> | Ação | Microsoft.MachineLearningModelManagement/register/action | Regista o ID de subscrição para o fornecedor de recursos e permite a criação de uma conta de alojamento |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.MachineLearningServices/register/action | Regista a subscrição para o fornecedor de recursos de serviços do Machine Learning |
> | Ação | Microsoft.MachineLearningServices/workspaces/computes/delete | Elimina os recursos de computação no Machine Learning serviços Workspace(s) |
> | Ação | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Listar segredos recursos de computação na área de trabalho do Machine Learning serviços |
> | Ação | Microsoft.MachineLearningServices/workspaces/computes/read | Obtém os recursos de computação no Machine Learning serviços Workspace(s) |
> | Ação | Microsoft.MachineLearningServices/workspaces/computes/write | Cria ou atualiza os recursos de computação no Machine Learning serviços Workspace(s) |
> | Ação | Microsoft.MachineLearningServices/workspaces/delete | Elimina a aprendizagem automática Workspace(s) de serviços |
> | Ação | Microsoft.MachineLearningServices/workspaces/listKeys/action | Lista os segredos para uma área de trabalho de serviços do Machine Learning |
> | Ação | Microsoft.MachineLearningServices/workspaces/read | Obtém a aprendizagem automática Workspace(s) de serviços |
> | Ação | Microsoft.MachineLearningServices/workspaces/write | Cria ou atualiza um Workspace(s) de serviços do Machine Learning |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | Ação de RBAC para a atribuição de um utilizador existente atribuídas identidade a um recurso |
> | Ação | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Elimina um utilizador existente atribuído identidade |
> | Ação | Microsoft.ManagedIdentity/userAssignedIdentities/read | Obtém um atribuído a identidade de utilizador existente |
> | Ação | Microsoft.ManagedIdentity/userAssignedIdentities/write | Cria um novo utilizador atribuído a identidade ou atualiza etiquetas associadas um utilizador existente atribuído identidade |

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ManagedLab/labAccounts/CreateLab/action | Crie um laboratório numa conta de laboratório. |
> | Ação | Microsoft.ManagedLab/labAccounts/delete | Elimine contas de laboratório. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/delete | Elimine laboratórios. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/delete | Elimine a definição de ambiente. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/delete | Elimine ambientes. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/read | Ambientes de leitura. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/write | Adiciona ou modifica os ambientes. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/read | Definição de ambiente de leitura. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/write | Adicionar ou modificar a definição do ambiente. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/labVms/delete | Elimine as máquinas virtuais do laboratório. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/labVms/read | Máquinas virtuais do laboratório de leitura. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/labVms/write | Adiciona ou modifica máquinas virtuais do laboratório. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/read | Laboratórios de leitura. |
> | Ação | Microsoft.ManagedLab/labAccounts/labs/write | Adiciona ou modifica laboratórios. |
> | Ação | Microsoft.ManagedLab/labAccounts/read | Ler as contas de laboratório. |
> | Ação | Microsoft.ManagedLab/labAccounts/write | Adiciona ou modifica contas de laboratório. |
> | Ação | Microsoft.ManagedLab/locations/operations/read | Operações de leitura. |
> | Ação | Microsoft.ManagedLab/register/action | Regista a subscrição |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Management/checkNameAvailability/action | Verifica se o nome do grupo de gestão especificado é válido e exclusivo. |
> | Ação | Microsoft.Management/getEntities/action | Liste todas as entidades (grupos de gestão, subscrições, etc.) para o utilizador autenticado. |
> | Ação | Microsoft.Management/managementGroups/delete | Elimine grupo de gestão. |
> | Ação | Microsoft.Management/managementGroups/read | Liste os grupos de gestão para o utilizador autenticado. |
> | Ação | Microsoft.Management/managementGroups/subscriptions/delete | Associa anular a subscrição do grupo de gestão. |
> | Ação | Microsoft.Management/managementGroups/subscriptions/write | Associates existente subscrição com o grupo de gestão. |
> | Ação | Microsoft.Management/managementGroups/write | Criar ou atualizar um grupo de gestão. |
> | Ação | Microsoft.Management/register/action | Registar a subscrição especificada Microsoft.Management |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Maps/accounts/delete | Elimine uma conta do Maps. |
> | Ação | Microsoft.Maps/accounts/listKeys/action | Chaves de conta de mapas de lista |
> | Ação | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Maps/accounts/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para contas de Maps |
> | Ação | Microsoft.Maps/accounts/read | Obter uma conta de mapas. |
> | Ação | Microsoft.Maps/accounts/regenerateKey/action | Gerar novo Maps primário ou secundário chave da conta |
> | Ação | Microsoft.Maps/accounts/write | Criar ou atualizar uma conta de mapas. |
> | Ação | Microsoft.Maps/register/action | Registar o fornecedor |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Devolve um contrato. |
> | Ação | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Aceita um contrato assinado. |
> | Ação | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Importa uma imagem para ACR o utilizador final. |
> | Ação | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Devolve uma configuração. |
> | Ação | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Guarda uma configuração. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.MarketplaceApps/ClassicDevServices/delete | Efetua uma operação de eliminação num recurso de serviço do programador clássico. |
> | Ação | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Obtém um dev clássico chaves de gestão de recursos do serviço. |
> | Ação | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Obtém o início de sessão único num URL para um serviço dev clássico. |
> | Ação | Microsoft.MarketplaceApps/ClassicDevServices/read | Executa uma operação de GET num serviço dev clássico. |
> | Ação | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Gera um chaves de gestão de recursos de serviço dev clássico. |
> | Ação | Microsoft.MarketplaceApps/Operations/read | Ler as operações para todos os tipos de recursos. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Cancelar um contrato de um item do marketplace indicado |
> | Ação | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Devolver um contrato de um item do marketplace indicado |
> | Ação | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Assinar um contrato de um item do marketplace indicado |
> | Ação | Microsoft.MarketplaceOrdering/agreements/read | Devolver todos os contratos em dada subscrição |
> | Ação | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Obter um contrato de um item de máquina virtual fornecido marketplace |
> | Ação | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Inicie sessão ou em Cancelar um contrato de um item de máquina virtual fornecido marketplace |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Media/checknameavailability/action | Verifica se está disponível um nome de conta de Media Services |
> | Ação | Microsoft.Media/locations/checkNameAvailability/action | Verifica se está disponível um nome de conta de Media Services |
> | Ação | Microsoft.Media/mediaservices/assets/delete | Eliminar qualquer recurso |
> | Ação | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Obter a chave de encriptação de recurso |
> | Ação | Microsoft.Media/mediaservices/assets/listContainerSas/action | Lista os URLs SAS do recurso contentor |
> | Ação | Microsoft.Media/mediaservices/assets/read | Qualquer recurso de leitura |
> | Ação | Microsoft.Media/mediaservices/assets/write | Criar ou atualizar qualquer recurso |
> | Ação | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Eliminar qualquer política de chave de conteúdo |
> | Ação | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Obter propriedades de política de segredos |
> | Ação | Microsoft.Media/mediaservices/contentKeyPolicies/read | Ler a qualquer política de chave de conteúdo |
> | Ação | Microsoft.Media/mediaservices/contentKeyPolicies/write | Criar ou atualizar qualquer política de chave de conteúdo |
> | Ação | Microsoft.Media/mediaservices/delete | Eliminar qualquer conta de Media Services |
> | Ação | Microsoft.Media/mediaservices/eventGridFilters/delete | Eliminar qualquer filtro da grelha de eventos |
> | Ação | Microsoft.Media/mediaservices/eventGridFilters/read | Qualquer filtro da grelha de evento de leitura |
> | Ação | Microsoft.Media/mediaservices/eventGridFilters/write | Criar ou atualizar qualquer filtro da grelha de eventos |
> | Ação | Microsoft.Media/mediaservices/liveEventOperations/read | Ler todas as operações de evento em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/delete | Eliminar qualquer evento em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Eliminar quaisquer dados em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Ler os resultados em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Criar ou atualizar quaisquer dados em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/read | Qualquer evento em direto de leitura |
> | Ação | Microsoft.Media/mediaservices/liveEvents/reset/action | Repor todas as operações de evento em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/start/action | Iniciar todas as operações de evento em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/stop/action | Parar todas as operações de evento em direto |
> | Ação | Microsoft.Media/mediaservices/liveEvents/write | Criar ou atualizar qualquer evento em direto |
> | Ação | Microsoft.Media/mediaservices/liveOutputOperations/read | Ler todas as operações de saída em direto |
> | Ação | Microsoft.Media/mediaservices/read | Ler a qualquer conta de Media Services |
> | Ação | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Qualquer operação de ponto final de transmissão em fluxo de leitura |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/delete | Elimine qualquer ponto final de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o recurso. |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso. |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de definições de métrica de ponto final de transmissão em fluxo do suporte de dados dos serviços. |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/read | Qualquer ponto final de transmissão em fluxo de leitura |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Dimensionar qualquer operação de ponto final de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Iniciar a qualquer operação de ponto final de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Parar qualquer operação de ponto final de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingEndpoints/write | Criar ou atualizar qualquer ponto final de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingLocators/delete | Eliminar qualquer localizador de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Lista de chaves conteúdo |
> | Ação | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Caminhos de lista |
> | Ação | Microsoft.Media/mediaservices/streamingLocators/read | Ler qualquer localizador de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingLocators/write | Criar ou atualizar qualquer localizador de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingPolicies/delete | Eliminar qualquer política de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingPolicies/read | Ler a qualquer política de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/streamingPolicies/write | Criar ou atualizar qualquer política de transmissão em fluxo |
> | Ação | Microsoft.Media/mediaservices/syncStorageKeys/action | Sincronizar as chaves de armazenamento para uma conta de armazenamento do Azure anexada |
> | Ação | Microsoft.Media/mediaservices/transforms/delete | Eliminar qualquer transformação |
> | Ação | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Cancelar a tarefa |
> | Ação | Microsoft.Media/mediaservices/transforms/jobs/delete | Eliminar todas as tarefas |
> | Ação | Microsoft.Media/mediaservices/transforms/jobs/read | Ler todas as tarefas |
> | Ação | Microsoft.Media/mediaservices/transforms/jobs/write | Criar ou atualizar qualquer tarefa |
> | Ação | Microsoft.Media/mediaservices/transforms/read | Ler qualquer transformação |
> | Ação | Microsoft.Media/mediaservices/transforms/write | Criar ou atualizar qualquer transformação |
> | Ação | Microsoft.Media/mediaservices/write | Criar ou atualizar qualquer conta de Media Services |
> | Ação | Microsoft.Media/operations/read | Obter operações disponíveis |
> | Ação | Microsoft.Media/register/action | Regista a subscrição para o fornecedor de recursos de Media Services e permite a criação de contas dos Media Services |
> | Ação | Microsoft.Media/unregister/action | Anula o registo da subscrição para o fornecedor de recursos de Media Services |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Migrate/locations/assessmentOptions/read | Obtém as opções de avaliação que estão disponíveis na localização especificada |
> | Ação | Microsoft.Migrate/locations/checknameavailability/action | Verifica a disponibilidade do nome do recurso para a subscrição fornecida na localização determinada |
> | Ação | Microsoft.Migrate/Operations/read | Lista as operações disponíveis no fornecedor de recursos Microsoft.Migrate |
> | Ação | Microsoft.Migrate/projects/delete | Elimina o projeto |
> | Ação | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Obter as propriedades de uma máquina avaliada |
> | Ação | Microsoft.Migrate/projects/groups/assessments/delete | Elimina uma avaliação |
> | Ação | Microsoft.Migrate/projects/groups/assessments/read | Obter as propriedades de uma avaliação |
> | Ação | Microsoft.Migrate/projects/groups/assessments/write | Cria uma nova avaliação ou atualiza uma existente |
> | Ação | Microsoft.Migrate/projects/groups/delete | Elimina um grupo |
> | Ação | Microsoft.Migrate/projects/groups/read | Obter as propriedades de um grupo |
> | Ação | Microsoft.Migrate/projects/groups/write | Cria um novo grupo ou atualiza um grupo existente |
> | Ação | Microsoft.Migrate/projects/keys/action | Obtém as chaves partilhadas para o projeto |
> | Ação | Microsoft.Migrate/projects/machines/read | Obter as propriedades de um computador |
> | Ação | Microsoft.Migrate/projects/read | Obtém as propriedades de um projeto |
> | Ação | Microsoft.Migrate/projects/write | Cria um novo projeto ou atualiza um existente |
> | Ação | Microsoft.Migrate/register/action | Regista a Subscrição no fornecedor de recursos Microsoft.Migrate |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.NetApp/locations/operationresults/read | Lê um recurso de resultado da operação. |
> | Ação | Microsoft.NetApp/locations/read | Leituras uma disponibilidade verifique recursos. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Elimina um recurso de agrupamento. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/providers/Microsoft.Insights/diagnosticSettings/read | Elimina um recurso de agrupamento. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/providers/Microsoft.Insights/diagnosticSettings/write | Elimina um recurso de agrupamento. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/providers/Microsoft.Insights/metricDefinitions/read | Elimina um recurso de agrupamento. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/read | Lê um recurso de agrupamento. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/delete | Elimina um recurso de volume. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/MountTargets/delete | Elimina um recurso de destino de montagem. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/MountTargets/read | Lê um recurso de destino de montagem. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/MountTargets/write | Escreve um recurso de destino de montagem. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/providers/Microsoft.Insights/diagnosticSettings/read | Elimina um recurso de volume. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/providers/Microsoft.Insights/diagnosticSettings/write | Elimina um recurso de volume. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/providers/Microsoft.Insights/metricDefinitions/read | Elimina um recurso de volume. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/read | Lê um recurso de volume. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots/delete | Elimina um recurso de instantâneo. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots/read | Lê um recurso de instantâneo. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots/write | Escreve um recurso de instantâneo. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/write | Escreve um recurso de volume. |
> | Ação | Microsoft.NetApp/netAppAccounts/capacityPools/write | Escreve um recurso de agrupamento. |
> | Ação | Microsoft.NetApp/netAppAccounts/delete | Elimina um recurso de conta. |
> | Ação | Microsoft.NetApp/netAppAccounts/read | Lê um recurso de conta. |
> | Ação | Microsoft.NetApp/netAppAccounts/write | Escreve um recurso de conta. |
> | Ação | Microsoft.NetApp/Operations/read | Lê recursos de uma operação. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Ssl do Gateway de aplicação predefinidas política |
> | Ação | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Opções de Ssl de disponíveis de Gateway de aplicação |
> | Ação | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Obtém o Gateway de aplicação define a regra de Waf disponíveis |
> | Ação | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/backendhealth/action | Obtém um funcionamento de back-end do gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/delete | Elimina um gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/effectiveNetworkSecurityGroups/action | Obter tabela de rotas configurado no Gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/effectiveRouteTable/action | Obter tabela de rotas configurado no Gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/logDefinitions/read | Obtém os eventos do Gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o Gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/read | Obtém um gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/setSecurityCenterConfiguration/action | Configuração de centro de segurança de Gateway de aplicação de conjuntos |
> | Ação | Microsoft.Network/applicationGateways/start/action | Inicia um gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/stop/action | Parar um gateway de aplicação |
> | Ação | Microsoft.Network/applicationGateways/write | Cria um gateway de aplicação ou atualiza um gateway de aplicação |
> | Ação | Microsoft.Network/applicationSecurityGroups/delete | Elimina um grupo de segurança da aplicação |
> | Ação | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Associa uma configuração de IP para grupos de segurança de aplicações. |
> | Ação | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Associa uma regra de segurança para grupos de segurança de aplicações. |
> | Ação | Microsoft.Network/applicationSecurityGroups/read | Obtém um ID de grupo de segurança de aplicação. |
> | Ação | Microsoft.Network/applicationSecurityGroups/write | Cria um grupo de segurança da aplicação ou atualiza um grupo de segurança de aplicação existente. |
> | Ação | Microsoft.Network/bgpServiceCommunities/read | Obter Comunidades de Bgp de serviço |
> | Ação | Microsoft.Network/checkTrafficManagerNameAvailability/action | Verifica a disponibilidade de um nome de DNS de caminho relativo do Gestor de tráfego. |
> | Ação | Microsoft.Network/connections/delete | Elimina o VirtualNetworkGatewayConnection |
> | Ação | Microsoft.Network/connections/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico de ligações |
> | Ação | Microsoft.Network/connections/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico de ligações |
> | Ação | Microsoft.Network/connections/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições de métricas de ligações |
> | Ação | Microsoft.Network/connections/read | Obtém o VirtualNetworkGatewayConnection |
> | Ação | Microsoft.Network/connections/sharedkey/action | Obter o VirtualNetworkGatewayConnection SharedKey |
> | Ação | Microsoft.Network/connections/sharedKey/read | Obtém o VirtualNetworkGatewayConnection SharedKey |
> | Ação | Microsoft.Network/connections/sharedKey/write | Cria ou atualiza um VirtualNetworkGatewayConnection SharedKey existente |
> | Ação | Microsoft.Network/connections/vpndeviceconfigurationscript/action | Obtém a configuração do dispositivo de Vpn de VirtualNetworkGatewayConnection |
> | Ação | Microsoft.Network/connections/write | Cria ou atualiza um VirtualNetworkGatewayConnection existente |
> | Ação | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/delete | Elimina um Proxy de plano de proteção DDoS |
> | Ação | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/read | Obtém uma definição de Proxy de planear a proteção de DDoS |
> | Ação | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/write | Cria um Proxy de plano de proteção para DDoS ou atualizações e Proxy de plano de proteção de DDoS existente |
> | Ação | Microsoft.Network/ddosProtectionPlans/delete | Elimina um plano de proteção DDoS |
> | Ação | Microsoft.Network/ddosProtectionPlans/join/action | Associa um plano de proteção DDoS |
> | Ação | Microsoft.Network/ddosProtectionPlans/read | Obtém um plano de proteção DDoS |
> | Ação | Microsoft.Network/ddosProtectionPlans/write | Cria um plano de proteção DDoS ou atualiza um plano de proteção DDoS  |
> | Ação | Microsoft.Network/dnsoperationresults/read | Obtém os resultados de uma operação de DNS |
> | Ação | Microsoft.Network/dnsoperationstatuses/read | Obtém o estado de uma operação de DNS  |
> | Ação | Microsoft.Network/dnszones/A/delete | Remova o conjunto de registos de um determinado nome e tipo "A" de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/A/read | Obtenha o conjunto de registos do tipo "A", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/A/write | Crie ou Atualize um conjunto de registos do tipo 'A' dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos. |
> | Ação | Microsoft.Network/dnszones/AAAA/delete | Remova o conjunto de registos de um determinado nome e tipo "AAAA" de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/AAAA/read | Obtenha o conjunto de registos do tipo "AAAA", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/AAAA/write | Crie ou Atualize um conjunto de registos do tipo "AAAA" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos. |
> | Ação | Microsoft.Network/dnszones/all/read | Obtém os conjuntos de registos de DNS em tipos |
> | Ação | Microsoft.Network/dnszones/CAA/delete | Remova o conjunto de registos de um determinado nome e o tipo 'CAA' de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/CAA/read | Obtenha o conjunto de registos do tipo 'CAA', no formato JSON. O conjunto de registos contém o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/CAA/write | Crie ou Atualize um conjunto de registos do tipo 'CAA' dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos. |
> | Ação | Microsoft.Network/dnszones/CNAME/delete | Remova o conjunto de registos de um determinado nome e tipo "CNAME" de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/CNAME/read | Obtenha o conjunto de registos do tipo "CNAME", no formato JSON. O conjunto de registos contém o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/CNAME/write | Crie ou Atualize um conjunto de registos do tipo "CNAME" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos. |
> | Ação | Microsoft.Network/dnszones/delete | Elimine a zona DNS, no formato JSON. As propriedades da zona incluem as etiquetas, etag, numberOfRecordSets e maxNumberOfRecordSets. |
> | Ação | Microsoft.Network/dnszones/MX/delete | Remova o conjunto de registos de um determinado nome e tipo "MX" de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/MX/read | Obtenha o conjunto de registos do tipo "MX", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/MX/write | Crie ou Atualize um conjunto de registos do tipo "MX" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos. |
> | Ação | Microsoft.Network/dnszones/NS/delete | Elimina o conjunto de registos DNS do tipo NS |
> | Ação | Microsoft.Network/dnszones/NS/read | Obtém o conjunto de registos de DNS do tipo NS |
> | Ação | Microsoft.Network/dnszones/NS/write | Cria ou atualiza o conjunto de registos de DNS do tipo NS |
> | Ação | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a zona DNS, definições de diagnóstico |
> | Ação | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico de zona DNS |
> | Ação | Microsoft.Network/dnszones/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições de métrica de zona DNS |
> | Ação | Microsoft.Network/dnszones/PTR/delete | Remova o conjunto de registos de um determinado nome e tipo "PTR" de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/PTR/read | Obtenha o conjunto de registos do tipo "PTR", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/PTR/write | Crie ou Atualize um conjunto de registos do tipo "PTR" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos. |
> | Ação | Microsoft.Network/dnszones/read | Obtenha a zona DNS, no formato JSON. As propriedades da zona incluem as etiquetas, etag, numberOfRecordSets e maxNumberOfRecordSets. Tenha em atenção que este comando não obtém os conjuntos de registos contidos na zona. |
> | Ação | Microsoft.Network/dnszones/recordsets/read | Obtém os conjuntos de registos de DNS em tipos |
> | Ação | Microsoft.Network/dnszones/SOA/read | Obtém o conjunto de registos DNS do tipo SOA |
> | Ação | Microsoft.Network/dnszones/SOA/write | Cria ou atualiza o conjunto de registos DNS do tipo SOA |
> | Ação | Microsoft.Network/dnszones/SRV/delete | Remova o conjunto de registos de um determinado nome e tipo "SRV" de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/SRV/read | Obtenha o conjunto de registos do tipo "SRV", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/SRV/write | Criar ou atualizar conjunto de registos do tipo SRV |
> | Ação | Microsoft.Network/dnszones/TXT/delete | Remova o conjunto de registos de um determinado nome e tipo "TXT" de uma zona DNS. |
> | Ação | Microsoft.Network/dnszones/TXT/read | Obtenha o conjunto de registos do tipo "TXT", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag. |
> | Ação | Microsoft.Network/dnszones/TXT/write | Crie ou Atualize um conjunto de registos do tipo "TXT" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos. |
> | Ação | Microsoft.Network/dnszones/write | Crie ou Atualize uma zona DNS dentro de um grupo de recursos.  Utilizado para atualizar as etiquetas num recurso de zona DNS. Tenha em atenção que este comando não pode ser utilizado para criar ou atualizar conjuntos de registos na zona. |
> | Ação | Microsoft.Network/expressRouteCircuits/authorizations/delete | Elimina uma ExpressRouteCircuit autorização |
> | Ação | Microsoft.Network/expressRouteCircuits/authorizations/read | Obtém uma ExpressRouteCircuit autorização |
> | Ação | Microsoft.Network/expressRouteCircuits/authorizations/write | Cria ou atualiza uma autorização ExpressRouteCircuit existente |
> | Ação | Microsoft.Network/expressRouteCircuits/delete | Elimina um ExpressRouteCircuit |
> | Ação | Microsoft.Network/expressRouteCircuits/join/action | Associa um circuito do Expressroute |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Obtém um ArpTable ExpressRouteCircuit de Peering |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Elimina uma ligação de ExpressRouteCircuit |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Obtém uma ligação de ExpressRouteCircuit |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Cria ou atualiza um recurso de ligação ExpressRouteCircuit existente |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/delete | Elimina um ExpressRouteCircuit Peering |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico para Peerings de circuito do ExpressRoute |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico para Peerings de circuito do ExpressRoute |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições de métricas para Peerings de circuito do ExpressRoute |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/read | Obtém um ExpressRouteCircuit Peering |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Obtém um RouteTable ExpressRouteCircuit de Peering |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Obtém um resumo de RouteTable Peering ExpressRouteCircuit |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Obtém um ExpressRouteCircuit Peering Stat |
> | Ação | Microsoft.Network/expressRouteCircuits/peerings/write | Cria ou atualiza um ExpressRouteCircuit Peering existente |
> | Ação | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico para circuitos do ExpressRoute |
> | Ação | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico para circuitos do ExpressRoute |
> | Ação | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read | Obter os eventos para circuitos do ExpressRoute |
> | Ação | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read | Obtém as definições de métricas para circuitos do ExpressRoute |
> | Ação | Microsoft.Network/expressRouteCircuits/read | Obter um ExpressRouteCircuit |
> | Ação | Microsoft.Network/expressRouteCircuits/stats/read | Obtém um ExpressRouteCircuit Stat |
> | Ação | Microsoft.Network/expressRouteCircuits/write | Cria ou atualiza um ExpressRouteCircuit existente |
> | Ação | Microsoft.Network/expressRouteCrossConnections/delete | Eliminar Expressroute entre a ligação |
> | Ação | Microsoft.Network/expressRouteCrossConnections/join/action | Associa um Expressroute cruzada ligação |
> | Ação | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Obtém um Expressroute entre a tabela de Arp Peering da ligação |
> | Ação | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Elimina uma Expressroute cruzada ligação Peering |
> | Ação | Microsoft.Network/expressRouteCrossConnections/peerings/read | Obtém um Expressroute cruzada ligação Peering |
> | Ação | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Obtém um Expressroute entre a tabela de rotas de Peering de ligação |
> | Ação | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Obtém um Expressroute entre a ligação de resumo de tabela de rota Peering |
> | Ação | Microsoft.Network/expressRouteCrossConnections/peerings/write | Cria um cruzada ligação Peering de Expressroute ou atualiza um existente cruzada ligação Peering de Expressroute |
> | Ação | Microsoft.Network/expressRouteCrossConnections/read | Obter Expressroute entre a ligação |
> | Ação | Microsoft.Network/expressRouteCrossConnections/write | Criar ou atualizar Expressroute cruzada ligação |
> | Ação | Microsoft.Network/expressRoutePorts/delete | Elimina ExpressRoutePorts |
> | Ação | Microsoft.Network/expressRoutePorts/join/action | Associa ExpressRoutePorts |
> | Ação | Microsoft.Network/expressRoutePorts/ports/read | Obtém ExpressRoutePortsPort |
> | Ação | Microsoft.Network/expressRoutePorts/ports/write | Cria ou atualiza ExpressRoutePortsPort |
> | Ação | Microsoft.Network/expressRoutePorts/read | Obtém ExpressRoutePorts |
> | Ação | Microsoft.Network/expressRoutePorts/write | Cria ou atualiza ExpressRoutePorts |
> | Ação | Microsoft.Network/expressRouteServiceProviders/read | Obtém os fornecedores de serviços de Expressroute |
> | Ação | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Associa um conjunto de endereços de back-end de Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/backendAddressPools/read | Obtém uma definição de conjunto de endereços de back-end de Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/delete | Elimina um balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Obtém uma definição de configuração de IP de front-end de Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Associa um balanceador de carga conjunto nat de entrada |
> | Ação | Microsoft.Network/loadBalancers/inboundNatPools/read | Obtém um balanceador de carga definição do conjunto nat de entrada |
> | Ação | Microsoft.Network/loadBalancers/inboundNatRules/delete | Elimina uma regra de nat de entrada do Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Associa uma regra de nat de entrada do Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/inboundNatRules/read | Obtém um balanceador de carga definição da regra nat de entrada |
> | Ação | Microsoft.Network/loadBalancers/inboundNatRules/write | Cria uma regra de nat de entrada do Balanceador de carga ou atualiza uma regra nat de entrada do Balanceador de carga existente |
> | Ação | Microsoft.Network/loadBalancers/loadBalancingRules/read | Obtém uma definição de regra balanceamento de carga Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/networkInterfaces/read | Obtém as referências a todas as interfaces de rede num Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/outboundRules/read | Obtém uma definição de regra de saída do Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/probes/join/action | Permite utilizar as pesquisas de um balanceador de carga. Por exemplo, com esta propriedade de healthProbe de permissão de dimensionamento VM conjunto pode referenciar a pesquisa. |
> | Ação | Microsoft.Network/loadBalancers/probes/read | Obtém uma sonda do Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico de Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico de Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/logDefinitions/read | Obtém os eventos do Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/read | Obtém uma definição de Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/virtualMachines/read | Obtém as referências a todas as máquinas virtuais num Balanceador de carga |
> | Ação | Microsoft.Network/loadBalancers/write | Cria um balanceador de carga ou atualiza um balanceador de carga existente |
> | Ação | Microsoft.Network/localnetworkgateways/delete | Elimina LocalNetworkGateway |
> | Ação | Microsoft.Network/localnetworkgateways/read | Obtém LocalNetworkGateway |
> | Ação | Microsoft.Network/localnetworkgateways/write | Cria ou atualiza um LocalNetworkGateway existente |
> | Ação | Microsoft.Network/locations/availableDelegations/read | Obtém as delegações disponíveis |
> | Ação | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Suporte de rede acelerados verificações |
> | Ação | Microsoft.Network/locations/checkDnsNameAvailability/read | Verifica se a etiqueta de dns está disponível na localização especificada |
> | Ação | Microsoft.Network/locations/effectiveResourceOwnership/action | Obtém a propriedade de recurso Efetivo |
> | Ação | Microsoft.Network/locations/operationResults/read | Obtém o resultado de um async POST ou operação de eliminação |
> | Ação | Microsoft.Network/locations/operations/read | Obtém o recurso da operação que representa o estado de uma operação assíncrona |
> | Ação | Microsoft.Network/locations/setResourceOwnership/action | Define a propriedade de recurso |
> | Ação | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Obtém suportado tamanhos de máquinas virtuais |
> | Ação | Microsoft.Network/locations/usages/read | Obtém os recursos a métrica de utilização |
> | Ação | Microsoft.Network/locations/validateResourceOwnership/action | Valida a propriedade de recurso |
> | Ação | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Obtém uma lista dos serviços de ponto final de rede Virtual disponíveis |
> | Ação | Microsoft.Network/networkIntentPolicies/delete | Elimina uma política de intenção de rede |
> | Ação | Microsoft.Network/networkIntentPolicies/read | Obtém uma descrição de intenção de política de rede |
> | Ação | Microsoft.Network/networkIntentPolicies/write | Cria uma política de intenção de rede ou atualiza uma política de intenção de rede existente |
> | Ação | Microsoft.Network/networkInterfaces/delete | Elimina uma interface de rede |
> | Ação | Microsoft.Network/networkInterfaces/diagnosticIdentity/read | Obtém a identidade de diagnóstico do recurso |
> | Ação | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Obter grupos de segurança de rede configurado na Interface da Vm de rede |
> | Ação | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Obter tabela de rotas configurado na Interface de rede da Vm |
> | Ação | Microsoft.Network/networkInterfaces/ipconfigurations/read | Obtém uma definição de configuração de ip de interface de rede.  |
> | Ação | Microsoft.Network/networkInterfaces/join/action | Associa uma Máquina Virtual a uma interface de rede |
> | Ação | Microsoft.Network/networkInterfaces/joinViaPrivateIp/action | Associa um recurso a uma interface de rede através de uma associação de serviço |
> | Ação | Microsoft.Network/networkInterfaces/loadBalancers/read | Obtém todos os balanceadores de carga que a interface de rede faz parte |
> | Ação | Microsoft.Network/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read | Obtém as métricas disponíveis para a Interface de rede |
> | Ação | Microsoft.Network/networkInterfaces/read | Obtém uma definição de interface de rede.  |
> | Ação | Microsoft.Network/networkInterfaces/serviceAssociations/delete | Elimina uma associação de serviço |
> | Ação | Microsoft.Network/networkInterfaces/serviceAssociations/read | Obtém uma definição de associação de serviço |
> | Ação | Microsoft.Network/networkInterfaces/serviceAssociations/validate/action | Valida uma associação de serviço |
> | Ação | Microsoft.Network/networkInterfaces/serviceAssociations/write | Cria uma nova associação de serviço ou modifica uma associação de serviço existente |
> | Ação | Microsoft.Network/networkInterfaces/write | Cria uma interface de rede ou atualiza uma interface de rede existente.  |
> | Ação | Microsoft.Network/networkProfiles/delete | Elimina um perfil de rede |
> | Ação | Microsoft.Network/networkProfiles/read | Obtém um perfil de rede |
> | Ação | Microsoft.Network/networkProfiles/removeContainers/action | Remove os contentores |
> | Ação | Microsoft.Network/networkProfiles/setContainers/action | Define contentores |
> | Ação | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Define as Interfaces de rede do contentor |
> | Ação | Microsoft.Network/networkProfiles/write | Cria ou atualiza um perfil de rede |
> | Ação | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Obtém uma definição de regra de segurança predefinida |
> | Ação | Microsoft.Network/networkSecurityGroups/delete | Elimina um grupo de segurança de rede |
> | Ação | Microsoft.Network/networkSecurityGroups/join/action | Associa um grupo de segurança de rede |
> | Ação | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as Definições de Diagnóstico dos Grupos de Segurança de Rede |
> | Ação | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico dos Grupos de Segurança de Rede. Esta operação é complementada pelo fornecedor de recursos de informações. |
> | Ação | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Obtém os eventos do grupo de segurança de rede |
> | Ação | Microsoft.Network/networkSecurityGroups/read | Obtém uma definição de grupo de segurança de rede |
> | Ação | Microsoft.Network/networkSecurityGroups/securityRules/delete | Elimina uma regra de segurança |
> | Ação | Microsoft.Network/networkSecurityGroups/securityRules/read | Obtém a definição de uma regra de segurança |
> | Ação | Microsoft.Network/networkSecurityGroups/securityRules/write | Cria uma regra de segurança ou atualiza uma regra de segurança existente |
> | Ação | Microsoft.Network/networkSecurityGroups/write | Cria um grupo de segurança de rede ou atualiza um grupo de segurança de rede existente |
> | Ação | Microsoft.Network/networkWatchers/availableProvidersList/action | Devolve a internet disponível todos os fornecedores de serviços para uma região do Azure especificada. |
> | Ação | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Devolve a classificação de latência relativo para a internet fornecedores de serviços de uma localização especificada para regiões do Azure. |
> | Ação | Microsoft.Network/networkWatchers/configureFlowLog/action | Configura o registo de fluxo para um recurso de destino. |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/delete | Elimina um Monitor de ligação |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/read | Obter as definições de diagnóstico do Monitor de ligação |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico de Monitor de ligação |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o Monitor de ligação |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Consulta monitorização conectividade entre pontos finais especificados |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/read | Obter os detalhes de Monitor de ligação |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Iniciar a monitorização de conectividade entre pontos finais especificados |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Parar/pausa monitorização conectividade entre pontos finais especificados |
> | Ação | Microsoft.Network/networkWatchers/connectionMonitors/write | Cria um Monitor de ligação |
> | Ação | Microsoft.Network/networkWatchers/connectivityCheck/action | Verifica a possibilidade de estabelecer uma ligação de TCP direta de uma máquina virtual para um ponto de final fornecido, incluindo outra VM ou um servidor remoto arbitrário. |
> | Ação | Microsoft.Network/networkWatchers/delete | Elimina um observador de rede |
> | Ação | Microsoft.Network/networkWatchers/ipFlowVerify/action | Devolve se o pacote é permitido ou negado de ou para um destino específico. |
> | Ação | Microsoft.Network/networkWatchers/lenses/delete | Elimina uma lente |
> | Ação | Microsoft.Network/networkWatchers/lenses/query/action | Consulta de monitorização do tráfego de rede de um ponto final especificado |
> | Ação | Microsoft.Network/networkWatchers/lenses/read | Obter os detalhes da lente |
> | Ação | Microsoft.Network/networkWatchers/lenses/start/action | Iniciar a monitorização de tráfego de rede de um ponto final especificado |
> | Ação | Microsoft.Network/networkWatchers/lenses/stop/action | Parar/pausa monitorização do tráfego de rede de um ponto final especificado |
> | Ação | Microsoft.Network/networkWatchers/lenses/write | Cria uma lente |
> | Ação | Microsoft.Network/networkWatchers/nextHop/action | Para um destino especificado e o endereço IP de destino, devolver o tipo de salto seguinte e hope seguinte endereço IP. |
> | Ação | Microsoft.Network/networkWatchers/packetCaptures/delete | Elimina uma captura de pacotes |
> | Ação | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Obtém informações sobre as propriedades e estado de um recurso de captura de pacotes. |
> | Ação | Microsoft.Network/networkWatchers/packetCaptures/read | Obter a definição de captura de pacotes |
> | Ação | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Pare a sessão de captura de pacote em execução. |
> | Ação | Microsoft.Network/networkWatchers/packetCaptures/write | Cria uma captura de pacotes |
> | Ação | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Obtém o estado do fluxo de registo num recurso. |
> | Ação | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Obtém o resultado de resolução de problemas de anteriormente executados ou atualmente em execução a operação de resolução de problemas. |
> | Ação | Microsoft.Network/networkWatchers/read | Obter a definição de observador de rede |
> | Ação | Microsoft.Network/networkWatchers/securityGroupView/action | Ver as regras de grupo de segurança de rede configurados e eficaz aplicadas numa VM. |
> | Ação | Microsoft.Network/networkWatchers/topology/action | Obtém uma vista de nível de rede de recursos e as respetivas relações num grupo de recursos. |
> | Ação | Microsoft.Network/networkWatchers/troubleshoot/action | Inicia a resolução de problemas num recurso de rede no Azure. |
> | Ação | Microsoft.Network/networkWatchers/write | Cria um observador de rede ou atualiza um observador de rede existente |
> | Ação | Microsoft.Network/operations/read | Obter operações disponíveis |
> | Ação | Microsoft.Network/publicIPAddresses/delete | Elimina um endereço Ip público. |
> | Ação | Microsoft.Network/publicIPAddresses/join/action | Associa um endereço ip público |
> | Ação | Microsoft.Network/publicIPAddresses/loadBalancerPools/delete | Elimina um conjunto de back-end do Balanceador de carga de endereço IP público |
> | Ação | Microsoft.Network/publicIPAddresses/loadBalancerPools/join/action | Associa um conjunto de back-end do Balanceador de carga de endereço IP público |
> | Ação | Microsoft.Network/publicIPAddresses/loadBalancerPools/read | Obtém uma pública IP endereço definição Balanceador de carga back-end conjunto |
> | Ação | Microsoft.Network/publicIPAddresses/loadBalancerPools/write | Cria um conjunto de back-end do Balanceador de carga de endereço IP público ou atualiza um existente público endereço IP conjunto Balanceador de carga back-end |
> | Ação | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read | Obter as definições de diagnóstico do endereço IP público |
> | Ação | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write | Criar ou atualizar as definições de diagnóstico do endereço IP público |
> | Ação | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read | Obter as definições de registo do endereço IP público |
> | Ação | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read | Obter as definições de métrica de endereço IP público |
> | Ação | Microsoft.Network/publicIPAddresses/read | Obtém uma definição de endereço ip público. |
> | Ação | Microsoft.Network/publicIPAddresses/write | Cria um endereço Ip público ou atualiza um endereço Ip público existente.  |
> | Ação | Microsoft.Network/register/action | Regista a subscrição |
> | Ação | Microsoft.Network/routeFilters/delete | Elimina uma definição de filtro de rota |
> | Ação | Microsoft.Network/routeFilters/join/action | Associa um filtro de rota |
> | Ação | Microsoft.Network/routeFilters/read | Obtém uma definição de filtro de rota |
> | Ação | Microsoft.Network/routeFilters/routeFilterRules/delete | Elimina a definição de uma regra de filtro de rota |
> | Ação | Microsoft.Network/routeFilters/routeFilterRules/read | Obtém a definição de uma regra de filtro de rota |
> | Ação | Microsoft.Network/routeFilters/routeFilterRules/write | Cria uma regra de filtro de rota ou atualiza uma regra de filtro de rota existente |
> | Ação | Microsoft.Network/routeFilters/write | Cria um filtro de rota ou atualiza um filtro já existente |
> | Ação | Microsoft.Network/routeTables/delete | Elimina uma definição de tabela de rota |
> | Ação | Microsoft.Network/routeTables/join/action | Junta a tabela de rotas |
> | Ação | Microsoft.Network/routeTables/read | Obtém uma definição de tabela de rota |
> | Ação | Microsoft.Network/routeTables/routes/delete | Elimina uma definição de rota |
> | Ação | Microsoft.Network/routeTables/routes/read | Obtém uma definição de rota |
> | Ação | Microsoft.Network/routeTables/routes/write | Cria uma rota ou atualiza uma já existente |
> | Ação | Microsoft.Network/routeTables/write | Cria uma tabela de rotas ou atualiza uma tabela já existente |
> | Ação | Microsoft.Network/securegateways/applicationRuleCollections/delete | Elimina uma coleção de regras de aplicação para um Gateway seguro |
> | Ação | Microsoft.Network/securegateways/applicationRuleCollections/read | Obter uma coleção de regras de aplicação para um determinado Gateway seguro |
> | Ação | Microsoft.Network/securegateways/applicationRuleCollections/write | Cria ou atualiza uma coleção de regras de aplicação para um Gateway seguro |
> | Ação | Microsoft.Network/securegateways/delete | Eliminar Gateway segura |
> | Ação | Microsoft.Network/securegateways/networkRuleCollections/delete | Elimina uma coleção de regras de rede para um Gateway seguro |
> | Ação | Microsoft.Network/securegateways/networkRuleCollections/read | Obter uma coleção de regras de rede para um determinado Gateway seguro |
> | Ação | Microsoft.Network/securegateways/networkRuleCollections/write | Cria ou atualiza uma coleção de regras de rede para um Gateway seguro |
> | Ação | Microsoft.Network/securegateways/providers/Microsoft.Insights/logDefinitions/read | Obtém os eventos de Firewall do Azure |
> | Ação | Microsoft.Network/securegateways/read | Obter o Gateway segura |
> | Ação | Microsoft.Network/securegateways/write | Cria ou atualiza um Gateway seguro |
> | Ação | Microsoft.Network/serviceEndpointPolicies/delete | Elimina uma política de ponto final de serviço |
> | Ação | Microsoft.Network/serviceEndpointPolicies/join/action | Associa uma política de ponto final de serviço |
> | Ação | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Associa uma sub-rede para políticas de ponto final de serviço |
> | Ação | Microsoft.Network/serviceEndpointPolicies/read | Obtém uma descrição de política do ponto final de serviço |
> | Ação | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Elimina uma definição de política do ponto final de serviço |
> | Ação | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Obtém um Decription de definição de política de ponto final de serviço |
> | Ação | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Cria uma definição de política do ponto final de serviço ou atualiza uma definição de política de ponto final de serviço existente |
> | Ação | Microsoft.Network/serviceEndpointPolicies/write | Cria uma política de ponto final de serviço ou atualiza uma política de ponto final de serviço existente |
> | Ação | Microsoft.Network/trafficManagerGeographicHierarchies/read | Obtém a hierarquia de Geographic do Gestor de tráfego que contém regiões que podem ser utilizadas com o método de encaminhamento de tráfego geográfica |
> | Ação | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Elimina um ponto final do Azure a partir de um perfil do Traffic Manager existente. Gestor de tráfego deixará de encaminhamento de tráfego para o ponto final do Azure eliminado. |
> | Ação | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Obtém um ponto final do Azure, que pertence a um perfil de Gestor de tráfego, incluindo todas as propriedades desse ponto final do Azure. |
> | Ação | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Adicionar um novo ponto de final do Azure num perfil do Traffic Manager existente ou atualizar as propriedades de um ponto final do Azure existente nesse perfil do Traffic Manager. |
> | Ação | Microsoft.Network/trafficManagerProfiles/delete | Elimine o perfil do Traffic Manager. Todas as definições associadas ao perfil do Gestor de tráfego serão perdidas e o perfil já não pode ser utilizado para encaminhar o tráfego. |
> | Ação | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Elimina um ponto final externo de um perfil do Traffic Manager existente. Gestor de tráfego deixará de encaminhamento de tráfego para o ponto final externo eliminado. |
> | Ação | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Obtém um ponto final externo que pertence a um perfil de Gestor de tráfego, incluindo todas as propriedades desse ponto final externo. |
> | Ação | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Adicionar um novo ponto final externo num perfil do Traffic Manager existente ou atualizar as propriedades de um ponto final externo existente nesse perfil do Traffic Manager. |
> | Ação | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Obtém o mapa térmico do Gestor de tráfego para o perfil do Gestor de tráfego fornecido que contém dados de contagens e a latência de consulta por IP de origem e de localização. |
> | Ação | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Elimina um ponto final aninhada de um perfil do Traffic Manager existente. Gestor de tráfego deixará de encaminhamento de tráfego para o ponto de final aninhados eliminados. |
> | Ação | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Obtém um ponto final aninhada que pertence a um perfil de Gestor de tráfego, incluindo todas as propriedades desse ponto final aninhado. |
> | Ação | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Adicionar um novo ponto de final aninhado num perfil do Traffic Manager existente ou atualizar as propriedades de um aninhada ponto final existente nesse perfil do Traffic Manager. |
> | Ação | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico do Gestor de tráfego |
> | Ação | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico do Traffic Manager, esta operação é suplementada pelo fornecedor de recursos de informações. |
> | Ação | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read | Obtém os eventos para o Gestor de tráfego |
> | Ação | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o Gestor de tráfego. |
> | Ação | Microsoft.Network/trafficManagerProfiles/read | Obter a configuração do perfil do Traffic Manager. Isto inclui as definições DNS, as definições de encaminhamento de tráfego, as definições de monitorização do ponto final e a lista de pontos finais encaminhados por este perfil do Traffic Manager. |
> | Ação | Microsoft.Network/trafficManagerProfiles/write | Criar um perfil de Gestor de tráfego ou modificar a configuração de um perfil do Traffic Manager existente.<br>Isto inclui ativar ou desativar um perfil e modificar as definições de DNS, as definições de encaminhamento de tráfego ou as definições de monitorização do ponto final.<br>Os pontos finais encaminhados pelo perfil do Gestor de tráfego podem ser adicionados, removidos, ativados ou desativados. |
> | Ação | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Elimina a chave de nível de subscrição utilizada para a coleção de métricas de utilizador em tempo real. |
> | Ação | Microsoft.Network/trafficManagerUserMetricsKeys/read | Obtém a chave de nível de subscrição utilizada para a coleção de métricas de utilizador em tempo real. |
> | Ação | Microsoft.Network/trafficManagerUserMetricsKeys/write | Cria uma nova chave de nível de subscrição a ser utilizado para a coleção de métricas de utilizador em tempo real. |
> | Ação | Microsoft.Network/unregister/action | Anula o registo da subscrição |
> | Ação | Microsoft.Network/virtualHubs/delete | Elimina um concentrador Virtual |
> | Ação | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Elimina um HubVirtualNetworkConnection |
> | Ação | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Obter um HubVirtualNetworkConnection |
> | Ação | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Criar ou atualizar um HubVirtualNetworkConnection |
> | Ação | Microsoft.Network/virtualHubs/read | Obter um concentrador Virtual |
> | Ação | Microsoft.Network/virtualHubs/write | Criar ou atualizar um concentrador Virtual |
> | Ação | Microsoft.Network/virtualnetworkgateways/Connections/Read | Obter o VirtualNetworkGatewayConnection |
> | Ação | Microsoft.Network/virtualNetworkGateways/delete | Elimina um virtualNetworkGateway |
> | Ação | Microsoft.Network/virtualnetworkgateways/generatevpnclientpackage/Action | Gerar o pacote de VpnClient para virtualNetworkGateway |
> | Ação | Microsoft.Network/virtualnetworkgateways/generatevpnprofile/Action | Gerar o pacote de VpnProfile para VirtualNetworkGateway |
> | Ação | Microsoft.Network/virtualnetworkgateways/getadvertisedroutes/Action | Obtém virtualNetworkGateway anunciados rotas |
> | Ação | Microsoft.Network/virtualnetworkgateways/getbgppeerstatus/Action | Obtém o estado de ponto a ponto do bgp virtualNetworkGateway |
> | Ação | Microsoft.Network/virtualnetworkgateways/getlearnedroutes/Action | Obtém as rotas aprendida de virtualnetworkgateway |
> | Ação | Microsoft.Network/virtualnetworkgateways/getvpnclientipsecparameters/Action | Obter parâmetros Vpnclient Ipsec para VirtualNetworkGateway P2S cliente. |
> | Ação | Microsoft.Network/virtualnetworkgateways/getvpnprofilepackageurl/Action | Obtém o URL de um pacote de perfil de cliente vpn pré-geradas |
> | Ação | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read | Obtém as definições de diagnóstico de Gateway de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza as definições de diagnóstico do Gateway de rede Virtual, esta operação é suplementada pelo fornecedor de recursos de informações. |
> | Ação | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read | Obtém os eventos do Gateway de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para o Gateway de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworkGateways/read | Obtém um VirtualNetworkGateway |
> | Ação | Microsoft.Network/virtualnetworkgateways/Reset/Action | Repõe um virtualNetworkGateway |
> | Ação | Microsoft.Network/virtualnetworkgateways/setvpnclientipsecparameters/Action | Defina Vpnclient Ipsec parâmetros para VirtualNetworkGateway P2S cliente. |
> | Ação | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Apresenta uma lista de suportados dispositivos Vpn |
> | Ação | Microsoft.Network/virtualNetworkGateways/write | Cria ou atualiza um VirtualNetworkGateway |
> | Ação | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Verifique se o endereço Ip está disponível na rede virtual especificada |
> | Ação | Microsoft.Network/virtualNetworks/customViews/get/action | Obter um conteúdo de vista personalizada de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworks/customViews/read | Obter a definição de uma vista personalizada da rede Virtual |
> | Ação | Microsoft.Network/virtualNetworks/delete | Elimina uma rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/peer/action | Peers uma rede virtual com a outra rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read | Obter as definições de diagnóstico de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write | Criar ou atualizar as definições de diagnóstico da rede Virtual |
> | Ação | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read | Obter as definições de registo de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworks/read | Obter a definição de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Elimina um proxy de peering de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Obtém uma definição de proxy de peering de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Cria um proxy de peering de rede virtual ou atualiza um proxy de peering de rede virtual existente |
> | Ação | Microsoft.Network/virtualNetworks/subnets/delete | Elimina uma sub-rede de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/subnets/join/action | Associa uma rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede. |
> | Ação | Microsoft.Network/virtualNetworks/subnets/read | Obtém uma definição de sub-rede de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/delete | Elimina uma ligação de navegação de recursos |
> | Ação | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/read | Obter a definição de ligação de navegação de recursos |
> | Ação | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/write | Cria uma ligação de navegação de recursos ou atualiza uma hiperligação de navegação de recursos existente |
> | Ação | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/delete | Elimina uma ligação de associação de serviço |
> | Ação | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/details/read | Obtém uma definição de detalhes de ligação de associação de serviço |
> | Ação | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/read | Obtém uma definição de ligação de associação de serviço |
> | Ação | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/validate/action | Valida uma ligação de associação de serviço |
> | Ação | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/write | Cria uma ligação de associação de serviço ou atualiza uma hiperligação de associação de serviço existente |
> | Ação | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Obtém as referências a todas as máquinas virtuais numa sub-rede de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/subnets/write | Cria uma sub-rede de rede virtual ou atualiza uma sub-rede de rede virtual existente |
> | Ação | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/delete | Elimina um consumidor de tráfego marcado |
> | Ação | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/read | Obter a definição de consumidor etiquetados de tráfego |
> | Ação | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/validate/action | Valida um consumidor de tráfego marcado |
> | Ação | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/write | Cria um consumidor de tráfego etiquetados ou atualiza um consumidor de tráfego etiquetados existente |
> | Ação | Microsoft.Network/virtualNetworks/usages/read | Obter as utilizações de IP para cada sub-rede da rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/virtualMachines/read | Obtém as referências a todas as máquinas virtuais numa rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Elimina um peering de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Obtém uma definição de peering de rede virtual |
> | Ação | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Cria um peering de rede virtual ou atualiza um peering de rede virtual existente |
> | Ação | Microsoft.Network/virtualNetworks/write | Cria uma rede virtual ou atualiza uma rede virtual existente |
> | Ação | Microsoft.Network/virtualNetworkTaps/delete | Eliminar toque de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworkTaps/join/action | Associa um toque de rede virtual |
> | Ação | Microsoft.Network/virtualNetworkTaps/read | Obter o toque de rede Virtual |
> | Ação | Microsoft.Network/virtualNetworkTaps/write | Criar ou atualizar o toque de rede Virtual |
> | Ação | Microsoft.Network/virtualWans/delete | Elimina um Virtual Wan |
> | Ação | Microsoft.Network/virtualWans/read | Obter um Virtual Wan |
> | Ação | Microsoft.Network/virtualWans/virtualHubProxies/delete | Elimina um proxy de Virtual Hub |
> | Ação | Microsoft.Network/virtualWans/virtualHubProxies/read | Obtém uma definição de proxy do Virtual Hub |
> | Ação | Microsoft.Network/virtualWans/virtualHubProxies/write | Cria um proxy de Virtual Hub ou atualiza um proxy de Virtual Hub |
> | Ação | Microsoft.Network/virtualWans/virtualHubs/read | Obtém todos os Hubs Virtual que façam referência a uma Wan Virtual. |
> | Ação | Microsoft.Network/virtualwans/vpnconfiguration/action | Obtém uma configuração de Vpn |
> | Ação | Microsoft.Network/virtualWans/vpnSiteProxies/delete | Elimina um proxy de Vpn de Site |
> | Ação | Microsoft.Network/virtualWans/vpnSiteProxies/read | Obtém uma definição de proxy de Vpn de Site |
> | Ação | Microsoft.Network/virtualWans/vpnSiteProxies/write | Cria um proxy de Vpn de Site ou atualiza um proxy de Vpn de Site |
> | Ação | Microsoft.Network/virtualWans/vpnSites/read | Obtém todos os Sites de VPN que façam referência a uma Wan Virtual. |
> | Ação | Microsoft.Network/virtualWans/write | Criar ou atualizar um Virtual Wan |
> | Ação | Microsoft.Network/vpnGateways/delete | Elimina um VpnGateway. |
> | Ação | Microsoft.Network/vpnGateways/read | Obtém um VpnGateway. |
> | Ação | microsoft.network/vpnGateways/vpnConnections/read | Obtém um VpnConnection. |
> | Ação | microsoft.network/vpnGateways/vpnConnections/write | Coloca um VpnConnection. |
> | Ação | Microsoft.Network/vpnGateways/write | Coloca um VpnGateway. |
> | Ação | Microsoft.Network/vpnsites/delete | Elimina um recurso de Vpn de Site. |
> | Ação | Microsoft.Network/vpnsites/read | Obtém um recurso de Vpn de Site. |
> | Ação | Microsoft.Network/vpnsites/write | Cria ou atualiza um recurso de Vpn de Site. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Verifica se determinado nome de recurso do Espaço de nomes está ou não disponível dentro do serviço NotificationHub. |
> | Ação | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Obter a lista de descrição das Regras de Autorização de Espaços de nomes. |
> | Ação | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido.  |
> | Ação | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Obter Cadeia de Ligação para o Espaço de nomes |
> | Ação | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Obter a lista de descrição das Regras de Autorização de Espaços de nomes. |
> | Ação | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Chave Primária/Secundária de Regeneração de Regra de Autorização de Espaço de nomes, especifica a Chave que tem de ser regenerada |
> | Ação | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas. |
> | Ação | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Verifica se determinado nome do NotificationHub está disponível dentro do Espaço de nomes. |
> | Ação | Microsoft.NotificationHubs/Namespaces/Delete | Eliminar Recurso de Espaço de nomes |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Obter lista de Regras de Autorização do Hub de Notificações |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Eliminar Regras de Autorização do Hub de Notificações |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Obter Cadeia de Ligação para o Hub de Notificações |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Obter lista de Regras de Autorização do Hub de Notificações |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Regra de Autorização do Hub de Notificações Regenerar Chave Primária/Secundária, especifica a Chave que tem de ser regenerada |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Criar regras de autorização de Hub de notificação e atualizar as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas. |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Enviar notificação push de teste. |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Eliminar Recurso do Hub de Notificações |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Obter a lista de descrições de recursos de métricas de espaço de nomes |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Obter todas as credenciais PNS do Hub de notificação. Isto inclui as credenciais do WNS, o MPNS, o APNS, o GCM e o Baidu |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Obter lista de Descrições de Recursos do Hub de Notificações |
> | Ação | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Criar um Hub de notificação e atualizar as respetivas propriedades. As respetivas propriedades incluem principalmente PNS credenciais. Regras de autorização e TTL |
> | Ação | Microsoft.NotificationHubs/Namespaces/read | Obter a lista de Descrição de Recursos de Espaço de nomes |
> | Ação | Microsoft.NotificationHubs/Namespaces/write | Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas. |
> | Ação | Microsoft.NotificationHubs/operationResults/read | Devolve os resultados da operação para o fornecedor de Hubs de Notificação |
> | Ação | Microsoft.NotificationHubs/operations/read | Devolve uma lista de operações suportadas para o fornecedor de Hubs de Notificação |
> | Ação | Microsoft.NotificationHubs/register/action | Regista a subscrição do fornecedor de recursos de NotificationHubs e ativa a criação de Espaços de nomes e NotificationHubs |
> | Ação | Microsoft.NotificationHubs/unregister/action | Anula o registo da subscrição para o fornecedor de recursos NotifciationHubs e permite a criação de Espaços de nomes e NotificationHubs |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.OperationalInsights/linkTargets/read | Apresenta uma lista de contas existentes que não estão associadas uma subscrição do Azure. Para ligar esta subscrição do Azure a uma área de trabalho, utilize um id de cliente devolvido por esta operação na propriedade de id de cliente da operação de área de trabalho de criar. |
> | Ação | Microsoft.OperationalInsights/register/action | Registe uma subscrição para um fornecedor de recursos. |
> | Ação | Microsoft.OperationalInsights/workspaces/analytics/query/action | A procura utilizando o motor de novo. |
> | Ação | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Obter o esquema de pesquisa V2. |
> | Ação | Microsoft.OperationalInsights/workspaces/api/query/action | A procura utilizando o motor de novo. |
> | Ação | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Obter o esquema de pesquisa V2. |
> | Ação | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Eliminar âmbito de configuração |
> | Ação | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Obter o âmbito de configuração |
> | Ação | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Definir âmbito de configuração |
> | Ação | Microsoft.OperationalInsights/workspaces/datasources/delete | Elimine as origens de dados na área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/datasources/read | Obter as origens de dados na área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/datasources/write | Criar/atualizar as origens de dados na área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/delete | Elimina uma área de trabalho. Se a área de trabalho foi associada a uma área de trabalho existente no momento da criação a área de trabalho que foi ligada não é eliminada. |
> | Ação | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Gera o certificado de registo para a área de trabalho. Este certificado é utilizado para ligar o Microsoft System Center Operation Manager à área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Desativa um pacote de análise para uma área de trabalho especificado. |
> | Ação | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Permite que um pacote de análise para uma área de trabalho especificado. |
> | Ação | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Apresenta uma lista de todos os pacotes de intelligence que estão visíveis para um determinado worksapce e também indica se o pacote está ativado ou desativado para essa área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Serviços de eliminação ligado em indicado área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/linkedServices/read | Obter serviços ligados em fornecido a área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/linkedServices/write | Criar/atualizar ligado serviços em atribuídos a área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/listKeys/action | Obtém as chaves de lista para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/listKeys/read | Obtém as chaves de lista para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/managementGroups/read | Obtém os nomes e os metadados para grupos de gestão do System Center Operations Manager ligados a esta área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Obter definições de métrica na área de trabalho |
> | Ação | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Elimine as definições de notificação do utilizador para a área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Obter definições de notificação do utilizador para a área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Defina as definições de notificação do utilizador para a área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/purge/action | Eliminar dados especificado a partir da área de trabalho |
> | Ação | Microsoft.OperationalInsights/workspaces/read | Obtém uma área de trabalho existente |
> | Ação | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Elimina uma consulta de pesquisa guardada |
> | Ação | Microsoft.OperationalInsights/workspaces/savedSearches/read | Obtém uma consulta de pesquisa guardada |
> | Ação | Microsoft.OperationalInsights/workspaces/savedSearches/write | Cria uma consulta de pesquisa guardada |
> | Ação | Microsoft.OperationalInsights/workspaces/schema/read | Obtém o esquema de pesquisa para a área de trabalho.  Esquema de pesquisa inclui os campos expostos e os respetivos tipos. |
> | Ação | Microsoft.OperationalInsights/workspaces/search/action | Executa uma consulta de pesquisa |
> | Ação | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Elimina uma configuração de armazenamento. Isto irá parar informações operacionais do Microsoft ao ler os dados da conta do storage. |
> | Ação | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Obtém uma configuração de armazenamento. |
> | Ação | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Cria uma nova configuração de armazenamento. Estas configurações são utilizadas para solicitar dados a partir de uma localização de uma conta de armazenamento existente. |
> | Ação | Microsoft.OperationalInsights/workspaces/usages/read | Obtém dados de utilização de uma área de trabalho, incluindo a quantidade de dados lidos pela área de trabalho. |
> | Ação | Microsoft.OperationalInsights/workspaces/write | Cria uma nova área de trabalho ou ligações para uma área de trabalho existente ao fornecer o id de cliente da área de trabalho existente. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.OperationsManagement/managementAssociations/delete | Eliminar a associação de gestão existente |
> | Ação | Microsoft.OperationsManagement/managementAssociations/read | Obtenha a associação de gestão existente |
> | Ação | Microsoft.OperationsManagement/managementAssociations/write | Criar uma nova associação de gestão |
> | Ação | Microsoft.OperationsManagement/managementConfigurations/delete | Eliminar Configuratin de gestão existente |
> | Ação | Microsoft.OperationsManagement/managementConfigurations/read | Obter a configuração de gestão existente |
> | Ação | Microsoft.OperationsManagement/managementConfigurations/write | Criar uma nova configuração de gestão |
> | Ação | Microsoft.OperationsManagement/register/action | Registe uma subscrição para um fornecedor de recursos. |
> | Ação | Microsoft.OperationsManagement/solutions/delete | Eliminar a solução existente do OMS |
> | Ação | Microsoft.OperationsManagement/solutions/read | Obter a sair com a solução do OMS |
> | Ação | Microsoft.OperationsManagement/solutions/write | Criar nova solução do OMS |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.PolicyInsights/policyEvents/queryResults/action | Informações de consulta sobre eventos de política. |
> | Ação | Microsoft.PolicyInsights/policyStates/queryResults/action | Informações de consulta sobre estados de política. |
> | Ação | Microsoft.PolicyInsights/policyStates/summarize/action | Informações de resumo de consulta sobre os estados de política mais recentes. |
> | Ação | Microsoft.PolicyInsights/register/action | Regista o fornecedor de recursos de informações de política e permite ações no mesmo. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Portal/dashboards/delete | Remove o dashboard da subscrição. |
> | Ação | Microsoft.Portal/dashboards/read | Lê os dashboards da subscrição. |
> | Ação | Microsoft.Portal/dashboards/write | Adicionar ou modificar o dashboard para uma subscrição. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.PowerBIDedicated/capacities/checkNameAvailability/action | Verificações de que nome do Power BI dedicado capacidade é válido e não em utilização. |
> | Ação | Microsoft.PowerBIDedicated/capacities/delete | Elimina o Power BI dedicado capacidade. |
> | Ação | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para as de capacidades do Power BI dedicado |
> | Ação | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para a capacidade de dedicado do Power BI. |
> | Ação | Microsoft.PowerBIDedicated/capacities/read | Obtém as informações de que o Power BI dedicado capacidade especificada. |
> | Ação | Microsoft.PowerBIDedicated/capacities/resume/action | Retoma a capacidade. |
> | Ação | Microsoft.PowerBIDedicated/capacities/skus/read | Obter informações de SKU disponíveis para a capacidade |
> | Ação | Microsoft.PowerBIDedicated/capacities/suspend/action | Suspende a capacidade. |
> | Ação | Microsoft.PowerBIDedicated/capacities/write | Cria ou atualiza o Power BI dedicado capacidade especificada. |
> | Ação | Microsoft.PowerBIDedicated/locations/operationresults/read | Obtém as informações do resultado da operação especificada. |
> | Ação | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Obtém as informações do Estado da operação especificada. |
> | Ação | Microsoft.PowerBIDedicated/operations/read | Obtém as informações de operações |
> | Ação | Microsoft.PowerBIDedicated/register/action | Regista o fornecedor de recursos dedicado do Power BI. |
> | Ação | Microsoft.PowerBIDedicated/skus/read | Obtém as informações de Skus |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp é uma operação interna utilizada pelo serviço |
> | Ação | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp é uma operação interna utilizada pelo serviço |
> | Ação | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Ação | Microsoft.RecoveryServices/locations/backupStatus/action | Verificar o estado de cópia de segurança para os cofres dos serviços de recuperação |
> | Ação | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Validar funcionalidades |
> | Ação | Microsoft.RecoveryServices/locations/operationStatus/read | Obtém o estado da operação para uma operação especificada |
> | Ação | Microsoft.RecoveryServices/operations/read | Operação devolve a lista de operações para um fornecedor de recursos |
> | Ação | Microsoft.RecoveryServices/register/action | Regista a subscrição de dado o fornecedor de recursos |
> | Ação | Microsoft.RecoveryServices/Vaults/backupconfig/read | Cofre dos serviços de configuração de devolve para recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupconfig/write | Cofre dos serviços de configuração de atualizações para a recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupEngines/read | Devolve todos os servidores de gestão de cópia de segurança registados no cofre. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Criar uma cópia de segurança intenção de proteção |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Devolve o estado da operação |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Obter todos os contentores passíveis de proteção |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Elimina o contentor registado |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Fazer a consulta para cargas de trabalho num contentor |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Obter todos os itens num contentor |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Obtém o resultado da Operação efetuada no Contentor de Proteção. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Executa a Cópia de Segurança do Item Protegido. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Elimina protegidos Item |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Obtém o Resultado da Operação Efetuada nos Itens Protegidos. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Devolve o estado da Operação efetuada nos Itens Protegidos. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devolve detalhes do objecto do Item protegido |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Aprovisionar Item instantâneas recuperação para o Item protegido |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Obter Pontos de Recuperação de Itens Protegidos. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Restaurar Pontos de Recuperação de Itens Protegidos. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Revogar a recuperação do Item instantâneas para Item protegido |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Criar um Item protegido cópia de segurança |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Devolve todas as registado contentores |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Cria um contentor registado |
> | Ação | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Atualiza a lista de contentor |
> | Ação | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Cancelar a tarefa |
> | Ação | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Devolve o Resultado da Operação de Tarefa. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupJobs/read | Devolve todos os objetos da tarefa |
> | Ação | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Tarefas de exportação |
> | Ação | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Devolve o resultado da operação de tarefa de exportação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Devolve os Metadados da Gestão da Cópia de Segurança do Cofre de Serviços de Recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Devolve o Resultado da Operação da Cópia de Segurança do Cofre de Serviços de Recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupOperations/read | Devolve a operação de cópia de segurança cofre dos serviços de estado de recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Eliminar uma política de proteção |
> | Ação | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Obter Resultados da Operação de Política. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Obter o estado da operação de política. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devolve todas as políticas de proteção |
> | Ação | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Cria a política de proteção |
> | Ação | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Devolve a lista de todos os Itens Susceptíveis de Proteção. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Devolve a lista de todos os Itens Protegidos. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Devolve todos os contentores que pertence à subscrição |
> | Ação | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Devolve a segurança PIN Cofre de serviços de informação para recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Devolve a configuração de armazenamento para a recuperação cofre dos serviços. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | Cofre dos serviços de configuração de armazenamento de atualizações para a recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/certificates/write | A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre. |
> | Ação | Microsoft.RecoveryServices/Vaults/delete | A operação eliminar cofre elimina os recursos do Azure especificado do tipo 'cofre' |
> | Ação | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Ação | Microsoft.RecoveryServices/Vaults/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Ação | Microsoft.RecoveryServices/Vaults/extendedInformation/write | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Ação | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtém os alertas para o Cofre de serviços de recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Resolve o alerta. |
> | Ação | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Obtém a configuração de notificação de Cofre de serviços de recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Configura as notificações por correio eletrónico para o Cofre de serviços de recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/read | Diagnóstico de cópia de segurança do Azure |
> | Ação | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/write | Diagnóstico de cópia de segurança do Azure |
> | Ação | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/logDefinitions/read | Registos de cópia de segurança do Azure |
> | Ação | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/metricDefinitions/read | Métricas de cópia de segurança do Azure |
> | Ação | Microsoft.RecoveryServices/Vaults/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Ação | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | A operação de anular o registo do contentor pode ser utilizada para anular o registo de um contentor. |
> | Ação | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido |
> | Ação | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Os contentores obter operação pode ser utilizada obter contentores registados para um recurso. |
> | Ação | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | A operação de registar o contentor de serviço pode ser utilizada para registar um contentor com o serviço de recuperação. |
> | Ação | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Ler as definições de alertas |
> | Ação | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Criar ou atualizar as definições de alertas |
> | Ação | Microsoft.RecoveryServices/vaults/replicationEvents/read | Ler quaisquer eventos |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Verifica a Consistência dos Recursos de infraestrutura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Elimine quaisquer recursos de infraestrutura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Implementar a imagem de servidor de processo |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Migrar os recursos de infraestrutura para o AAD |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Ler todos os recursos de infraestrutura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Reassociar Gateway |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Remover recursos de infraestrutura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Renovar o certificado para recursos de infraestrutura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Ler as redes lógicas |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Ler a quaisquer redes |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Eliminar quaisquer mapeamentos de rede |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Leia os mapeamentos de rede |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Criar ou atualizar quaisquer mapeamentos de rede |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Detetar o Item Protegível |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Leu os contentores de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Remover o contentor de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Ler quaisquer itens passíveis de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Aplicar o ponto de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Eliminar os itens protegidos |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Consolidação de ativação pós-falha |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Ativação pós-falha planeada |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Ler os itens protegidos |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Pontos de recuperação de replicação de leitura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Remover o Item protegido |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Reparar a replicação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Proteja o Item protegido |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Submeter Comentários |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Leia os tamanhos de computação de destino |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Ativação Pós-falha de Teste |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Limpeza da ativação pós-falha de teste |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Ativação pós-falha |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Serviço de mobilidade de atualização |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Criar ou atualizar quaisquer itens protegidos |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Eliminar quaisquer mapeamentos de contentor de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Ler quaisquer mapeamentos de contentor de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Remova o mapeamento de contentor de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Criar ou atualizar quaisquer mapeamentos de contentor de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Contentor de proteção do comutador |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Criar ou atualizar quaisquer contentores de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Eliminar quaisquer fornecedores de serviços de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Ler quaisquer fornecedores de serviços de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Atualize o fornecedor |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Remover o fornecedor de serviços de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Criar ou atualizar quaisquer fornecedores de serviços de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Ler as classificações de armazenamento |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Eliminar quaisquer mapeamentos de classificação de armazenamento |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Ler quaisquer mapeamentos de classificação de armazenamento |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Criar ou atualizar quaisquer mapeamentos de classificação de armazenamento |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Eliminar qualquer vCenters |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Qualquer vCenters de leitura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Criar ou atualizar qualquer vCenters |
> | Ação | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Criar ou atualizar quaisquer recursos de infraestrutura |
> | Ação | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Cancelar a tarefa |
> | Ação | Microsoft.RecoveryServices/vaults/replicationJobs/read | Ler todas as tarefas |
> | Ação | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Reinicie a tarefa |
> | Ação | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Retomar a tarefa |
> | Ação | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Leia os mapeamentos de rede |
> | Ação | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Ler a quaisquer redes |
> | Ação | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Eliminar todas as políticas |
> | Ação | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Ler todas as políticas |
> | Ação | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Criar ou atualizar as políticas |
> | Ação | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Ler os itens protegidos |
> | Ação | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Ler quaisquer mapeamentos de contentor de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Leu os contentores de proteção |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Eliminar quaisquer planos de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Plano de recuperação de consolidação de ativação pós-falha |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Plano de recuperação de ativação pós-falha planeada |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Leia os planos de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Proteja o plano de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Plano de recuperação de ativação pós-falha de teste |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Plano de recuperação de limpeza da ativação pós-falha de teste |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Plano de recuperação de ativação pós-falha |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Criar ou atualizar quaisquer planos de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Ler quaisquer fornecedores de serviços de recuperação |
> | Ação | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Ler quaisquer mapeamentos de classificação de armazenamento |
> | Ação | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Ler as classificações de armazenamento |
> | Ação | Microsoft.RecoveryServices/vaults/replicationUsages/read | Ler a quaisquer utilizações de replicação do Cofre |
> | Ação | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Ler qualquer Estado de funcionamento de replicação do Cofre |
> | Ação | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Atualize o estado de funcionamento do Cofre |
> | Ação | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Qualquer vCenters de leitura |
> | Ação | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Devolve informações sobre o token para o Cofre de serviços de recuperação. |
> | Ação | Microsoft.RecoveryServices/vaults/usages/read | Ler as utilizações do Cofre |
> | Ação | Microsoft.RecoveryServices/Vaults/usages/read | Devolve detalhes de utilização de um Cofre de Serviços de Recuperação. |
> | Ação | Microsoft.RecoveryServices/Vaults/validateOperation/action | Validar a operação no Item protegido |
> | Ação | Microsoft.RecoveryServices/Vaults/vaultTokens/read | A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre. |
> | Ação | Microsoft.RecoveryServices/Vaults/write | A operação Criar Cofre cria um recurso do tipo "cofre" do Azure |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Relay/checkNameAvailability/action | Verifica a disponibilidade do espaço de nomes em determinada subscrição. |
> | Ação | Microsoft.Relay/checkNamespaceAvailability/action | Verifica a disponibilidade do espaço de nomes em determinada subscrição. Esta API foi preterida utilize CheckNameAvailabiltiy em vez disso. |
> | Ação | Microsoft.Relay/namespaces/authorizationRules/action | Regra de autorização de espaço de nomes de atualizações. Esta API está depricated. Para atualizar a regra de autorização de espaço de nomes em vez disso, utilize uma chamada PUT... Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.Relay/namespaces/authorizationRules/delete | Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido.  |
> | Ação | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Obter Cadeia de Ligação para o Espaço de nomes |
> | Ação | Microsoft.Relay/namespaces/authorizationRules/read | Obter a lista de descrição das Regras de Autorização de Espaços de nomes. |
> | Ação | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.Relay/namespaces/authorizationRules/write | Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas. |
> | Ação | Microsoft.Relay/namespaces/Delete | Eliminar Recurso de Espaço de nomes |
> | Ação | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Obtém as chaves das regras de autorização do espaço de nomes principal da Recuperação Após Desastre |
> | Ação | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Obter as Regras de Autorização do Espaço de Nomes Principal da Recuperação Após Desastre |
> | Ação | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Desativa a Recuperação Após Desastre e para a replicação de alterações de espaço de nomes principais para secundários. |
> | Ação | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Verifica a disponibilidade do alias do espaço de nomes numa determinada subscrição. |
> | Ação | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Elimina a configuração de recuperação após desastre associada com o espaço de nomes. Esta operação só pode ser invocada através do espaço de nomes primário. |
> | Ação | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Invoca uma ativação pós-falha GEO DR e reconfigura o alias de espaço de nomes para apontar para o espaço de nomes secundário. |
> | Ação | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Obtém a configuração da Recuperação Após Desastre associada ao espaço de nomes. |
> | Ação | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Cria ou Atualiza a configuração da Recuperação Após Desastre associada ao espaço de nomes. |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | Operação atualizar HybridConnection. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT. |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Operação para eliminar as regras de autorização de HybridConnection |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | Obter a cadeia de ligação para HybridConnection |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  Obter a lista de regras de autorização de HybridConnection |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Criar regras de autorização de HybridConnection e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados. |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/Delete | Operação para eliminar recurso HybridConnection |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/read | Obter a lista de descrições de recurso HybridConnection |
> | Ação | Microsoft.Relay/namespaces/HybridConnections/write | Criar ou atualizar HybridConnection propriedades. |
> | Ação | Microsoft.Relay/namespaces/messagingPlan/read | Obtém o plano de serviço de mensagens para um espaço de nomes.<br>Esta API foi preterida.<br>As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)...<br>Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.Relay/namespaces/messagingPlan/write | Atualiza o serviço de mensagens planear um espaço de nomes.<br>Esta API foi preterida.<br>As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)...<br>Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.Relay/namespaces/operationresults/read | Obter o estado da operação de Espaço de nomes |
> | Ação | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de descrições de recursos de métricas de espaço de nomes |
> | Ação | Microsoft.Relay/namespaces/read | Obter a lista de Descrição de Recursos de Espaço de nomes |
> | Ação | Microsoft.Relay/namespaces/removeAcsNamepsace/action | Remova o espaço de nomes do ACS |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | Operação atualizar WcfRelay. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT. |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Operação para eliminar as regras de autorização de WcfRelay |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | Obter a cadeia de ligação para WcfRelay |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  Obter a lista de regras de autorização de WcfRelay |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Criar regras de autorização de WcfRelay e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados. |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/Delete | Operação para eliminar recurso WcfRelay |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/read | Obter a lista de descrições de recurso WcfRelay |
> | Ação | Microsoft.Relay/namespaces/WcfRelays/write | Criar ou atualizar WcfRelay propriedades. |
> | Ação | Microsoft.Relay/namespaces/write | Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas. |
> | Ação | Microsoft.Relay/operations/read | Obter operações |
> | Ação | Microsoft.Relay/register/action | Regista a subscrição do fornecedor de recursos de Reencaminhamento e ativa a criação de recursos de Reencaminhamento |
> | Ação | Microsoft.Relay/unregister/action | Regista a subscrição do fornecedor de recursos de Reencaminhamento e ativa a criação de recursos de Reencaminhamento |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Obtém o estado de disponibilidade para o recurso especificado. |
> | Ação | Microsoft.ResourceHealth/AvailabilityStatuses/read | Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado |
> | Ação | Microsoft.ResourceHealth/events/read | Obter Eventos de Estado de Funcionamento do Serviço da subscrição indicada |
> | Ação | Microsoft.Resourcehealth/healthevent/action | Indica a alteração no estado de funcionamento para o recurso especificado |
> | Ação | Microsoft.Resourcehealth/healthevent/Activated/action | Indica a alteração no estado de funcionamento para o recurso especificado |
> | Ação | Microsoft.Resourcehealth/healthevent/InProgress/action | Indica a alteração no estado de funcionamento para o recurso especificado |
> | Ação | Microsoft.Resourcehealth/healthevent/Pending/action | Indica a alteração no estado de funcionamento para o recurso especificado |
> | Ação | Microsoft.Resourcehealth/healthevent/Resolved/action | Indica a alteração no estado de funcionamento para o recurso especificado |
> | Ação | Microsoft.Resourcehealth/healthevent/Updated/action | Indica a alteração no estado de funcionamento para o recurso especificado |
> | Ação | Microsoft.ResourceHealth/impactedResources/read | Obter Recursos Afetados da subscrição indicada |
> | Ação | Microsoft.ResourceHealth/register/action | Regista a subscrição para o Microsoft ResourceHealth |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Resources/checkPolicyCompliance/action | Verifique o estado de conformidade de um dado recurso em relação a políticas de recursos. |
> | Ação | Microsoft.Resources/checkResourceName/action | Verificar a validade do nome do recurso. |
> | Ação | Microsoft.Resources/deployments/cancel/action | Cancela uma implementação. |
> | Ação | Microsoft.Resources/deployments/delete | Elimina uma implementação. |
> | Ação | Microsoft.Resources/deployments/operations/read | Obtém ou lista as operações de implementação. |
> | Ação | Microsoft.Resources/deployments/read | Obtém ou lista implementações. |
> | Ação | Microsoft.Resources/deployments/validate/action | Valida uma implementação. |
> | Ação | Microsoft.Resources/deployments/write | Cria ou atualiza uma implementação. |
> | Ação | Microsoft.Resources/links/delete | Elimina uma ligação de recursos. |
> | Ação | Microsoft.Resources/links/read | Obtém ou lista ligações de recursos. |
> | Ação | Microsoft.Resources/links/write | Cria ou atualiza uma ligação de recursos. |
> | Ação | Microsoft.Resources/marketplace/purchase/action | Adquire um recurso do mercado. |
> | Ação | Microsoft.Resources/providers/read | Obter a lista de fornecedores. |
> | Ação | Microsoft.Resources/resources/read | Obter a lista de recursos com base em filtros. |
> | Ação | Microsoft.Resources/subscriptions/locations/read | Obtém a lista de localizações suportadas. |
> | Ação | Microsoft.Resources/subscriptions/operationresults/read | Obter os resultados da operação de subscrição. |
> | Ação | Microsoft.Resources/subscriptions/providers/read | Obtém ou lista fornecedores de recursos. |
> | Ação | Microsoft.Resources/subscriptions/read | Obtém a lista de subscrições. |
> | Ação | Microsoft.Resources/subscriptions/resourceGroups/delete | Elimina um grupo de recursos e respetivos recursos. |
> | Ação | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Obtém ou lista as operações de implementação. |
> | Ação | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Obtém ou lista os estados da operação de implementação. |
> | Ação | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Obtém ou lista implementações. |
> | Ação | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Cria ou atualiza uma implementação. |
> | Ação | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Move os recursos de um grupo de recursos para outro. |
> | Ação | Microsoft.Resources/subscriptions/resourceGroups/read | Obtém ou lista os grupos de recursos. |
> | Ação | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Obtém os recursos do grupo de recursos. |
> | Ação | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Validar a movimentação de recursos de um grupo de recursos para outro. |
> | Ação | Microsoft.Resources/subscriptions/resourceGroups/write | Cria ou atualiza um grupo de recursos. |
> | Ação | Microsoft.Resources/subscriptions/resources/read | Obtém os recursos de uma subscrição. |
> | Ação | Microsoft.Resources/subscriptions/tagNames/delete | Elimina uma etiqueta de subscrição. |
> | Ação | Microsoft.Resources/subscriptions/tagNames/read | Obtém ou lista etiquetas de subscrição. |
> | Ação | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Elimina um valor de etiqueta de subscrição. |
> | Ação | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Obtém ou lista valores de etiqueta de subscrição. |
> | Ação | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Adiciona um valor de etiqueta de subscrição. |
> | Ação | Microsoft.Resources/subscriptions/tagNames/write | Adiciona uma etiqueta de subscrição. |
> | Ação | Microsoft.Resources/tenants/read | Obtém a lista de inquilinos. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Scheduler/jobcollections/delete | Elimina a coleção de tarefas. |
> | Ação | Microsoft.Scheduler/jobcollections/disable/action | Desativa a coleção de tarefas. |
> | Ação | Microsoft.Scheduler/jobcollections/enable/action | Ativa a coleção de tarefas. |
> | Ação | Microsoft.Scheduler/jobcollections/jobs/delete | Elimina a tarefa. |
> | Ação | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Gera a definição da Aplicação Lógica com base numa Tarefa do Scheduler. |
> | Ação | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Obtém o histórico de tarefas. |
> | Ação | Microsoft.Scheduler/jobcollections/jobs/read | Obtém a tarefa. |
> | Ação | Microsoft.Scheduler/jobcollections/jobs/run/action | Executa a tarefa. |
> | Ação | Microsoft.Scheduler/jobcollections/jobs/write | Cria ou atualiza a tarefa. |
> | Ação | Microsoft.Scheduler/jobcollections/read | Obter Coleção de Tarefas |
> | Ação | Microsoft.Scheduler/jobcollections/write | Cria ou atualiza a coleção de tarefas. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Search/checkNameAvailability/action | Verifica a disponibilidade do nome do serviço. |
> | Ação | Microsoft.Search/operations/read | Apresenta uma lista de todas as operações disponíveis do fornecedor Microsoft.Search. |
> | Ação | Microsoft.Search/register/action | Regista a subscrição para o fornecedor de recursos de pesquisa e permite a criação de serviços de pesquisa. |
> | Ação | Microsoft.Search/searchServices/createQueryKey/action | Cria a chave de consulta. |
> | Ação | Microsoft.Search/searchServices/delete | Elimina o serviço de pesquisa. |
> | Ação | Microsoft.Search/searchServices/deleteQueryKey/delete | Elimina a chave de consulta. |
> | Ação | Microsoft.Search/searchServices/diagnosticSettings/read | Obtém o diganostic definição de leitura para o recurso |
> | Ação | Microsoft.Search/searchServices/diagnosticSettings/write | Cria ou atualiza a definição de diganostic para o recurso |
> | Ação | Microsoft.Search/searchServices/listAdminKeys/action | Lê as chaves de administração. |
> | Ação | Microsoft.Search/searchServices/listQueryKeys/read | Devolve a lista de chaves de consulta API para o serviço da Azure Search indicado. |
> | Ação | Microsoft.Search/searchServices/logDefinitions/read | Obtém os registos disponíveis para o serviço de pesquisa |
> | Ação | Microsoft.Search/searchServices/metricDefinitions/read | Obtem as métricas disponíveis para o serviço de pesquisa |
> | Ação | Microsoft.Search/searchServices/read | Lê o serviço de pesquisa. |
> | Ação | Microsoft.Search/searchServices/regenerateAdminKey/action | A chave de administrador de gera de novo. |
> | Ação | Microsoft.Search/searchServices/start/action | Inicia o serviço de pesquisa. |
> | Ação | Microsoft.Search/searchServices/stop/action | Para o serviço de pesquisa. |
> | Ação | Microsoft.Search/searchServices/write | Cria ou atualiza o serviço de pesquisa. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Security/alerts/read | Obtém todos os alertas de segurança disponíveis |
> | Ação | Microsoft.Security/applicationWhitelistings/read | Obtém o whitelistings de aplicação |
> | Ação | Microsoft.Security/applicationWhitelistings/write | Cria uma nova aplicação a listas brancas ou atualiza um já existente |
> | Ação | Microsoft.Security/complianceResults/read | Obtém os resultados de compatibilidade para o recurso |
> | Ação | Microsoft.Security/locations/alerts/activate/action | Ativar um alerta de segurança |
> | Ação | Microsoft.Security/locations/alerts/dismiss/action | Dispensar um alerta de segurança |
> | Ação | Microsoft.Security/locations/alerts/read | Obtém todos os alertas de segurança disponíveis |
> | Ação | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Inicia uma política de acesso de rede de just-in-time |
> | Ação | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Obtém as políticas de acesso de rede de just-in-time |
> | Ação | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Cria uma nova política de acesso de rede de just-in-time ou atualiza um já existente |
> | Ação | Microsoft.Security/locations/read | Obtém a localização de dados de segurança |
> | Ação | Microsoft.Security/locations/tasks/activate/action | Ativar uma recomendação de segurança |
> | Ação | Microsoft.Security/locations/tasks/dismiss/action | Ignorar uma recomendação de segurança |
> | Ação | Microsoft.Security/locations/tasks/read | Obtém todas as recomendações de segurança disponíveis |
> | Ação | Microsoft.Security/locations/tasks/resolve/action | Resolver uma recomendação de segurança |
> | Ação | Microsoft.Security/locations/tasks/start/action | Iniciar uma recomendação de segurança |
> | Ação | Microsoft.Security/policies/read | Obtém a política de segurança |
> | Ação | Microsoft.Security/policies/write | Atualiza a política de segurança |
> | Ação | Microsoft.Security/pricings/delete | Elimina as definições de preços para o âmbito |
> | Ação | Microsoft.Security/pricings/read | Obtém as definições de preços para o âmbito |
> | Ação | Microsoft.Security/pricings/write | Atualiza as definições de preços para o âmbito |
> | Ação | Microsoft.Security/register/action | Regista a subscrição para o Centro de segurança do Azure |
> | Ação | Microsoft.Security/securityContacts/delete | Elimina o contacto de segurança |
> | Ação | Microsoft.Security/securityContacts/read | Obtém o contacto de segurança |
> | Ação | Microsoft.Security/securityContacts/write | O contacto de segurança de atualizações |
> | Ação | Microsoft.Security/securitySolutions/delete | Elimina uma solução de segurança |
> | Ação | Microsoft.Security/securitySolutions/read | Obtém as soluções de segurança |
> | Ação | Microsoft.Security/securitySolutions/write | Cria uma nova solução de segurança ou atualiza um já existente |
> | Ação | Microsoft.Security/securitySolutionsReferenceData/read | Obtém as soluções de segurança de dados de referência |
> | Ação | Microsoft.Security/securityStatuses/read | Obtém a segurança Estados de funcionamento de recursos do Azure |
> | Ação | Microsoft.Security/securityStatusesSummaries/read | Obtém a segurança resumos de Estados para o âmbito |
> | Ação | Microsoft.Security/tasks/read | Obtém todas as recomendações de segurança disponíveis |
> | Ação | Microsoft.Security/webApplicationFirewalls/delete | Elimina uma firewall de aplicação web |
> | Ação | Microsoft.Security/webApplicationFirewalls/read | Obtém a web firewalls de aplicação |
> | Ação | Microsoft.Security/webApplicationFirewalls/write | Cria uma nova firewall de aplicação web ou atualiza um já existente |
> | Ação | Microsoft.Security/workspaceSettings/connect/action | Alterar as definições de restabelecimento de ligação de definições de área de trabalho |
> | Ação | Microsoft.Security/workspaceSettings/delete | Elimina as definições da área de trabalho |
> | Ação | Microsoft.Security/workspaceSettings/read | Obtém as definições da área de trabalho |
> | Ação | Microsoft.Security/workspaceSettings/write | Atualiza as definições da área de trabalho |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.SecurityGraph/diagnosticsettings/delete | Eliminar uma definição de diagnóstico |
> | Ação | Microsoft.SecurityGraph/diagnosticsettings/read | Ler a definição de diagnóstico |
> | Ação | Microsoft.SecurityGraph/diagnosticsettings/write | Escrever uma definição de diagnóstico |
> | Ação | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Ler um categorias de definição de diagnóstico |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ServiceBus/checkNameAvailability/action | Verifica a disponibilidade do espaço de nomes em determinada subscrição. |
> | Ação | Microsoft.ServiceBus/checkNamespaceAvailability/action | Verifica a disponibilidade do espaço de nomes em determinada subscrição. Esta API foi preterida utilize CheckNameAvailabiltiy em vez disso. |
> | Ação | Microsoft.ServiceBus/namespaces/authorizationRules/action | Regra de autorização de espaço de nomes de atualizações. Esta API está depricated. Para atualizar a regra de autorização de espaço de nomes em vez disso, utilize uma chamada PUT... Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido.  |
> | Ação | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Obter Cadeia de Ligação para o Espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/authorizationRules/read | Obter a lista de descrição das Regras de Autorização de Espaços de nomes. |
> | Ação | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.ServiceBus/namespaces/authorizationRules/write | Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas. |
> | Ação | Microsoft.ServiceBus/namespaces/Delete | Eliminar Recurso de Espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Obtém as chaves das regras de autorização do espaço de nomes principal da Recuperação Após Desastre |
> | Ação | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Obter as Regras de Autorização do Espaço de Nomes Principal da Recuperação Após Desastre |
> | Ação | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Desativa a Recuperação Após Desastre e para a replicação de alterações de espaço de nomes principais para secundários. |
> | Ação | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Verifica a disponibilidade do alias do espaço de nomes numa determinada subscrição. |
> | Ação | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Elimina a configuração de recuperação após desastre associada com o espaço de nomes. Esta operação só pode ser invocada através do espaço de nomes primário. |
> | Ação | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Invoca uma ativação pós-falha GEO DR e reconfigura o alias de espaço de nomes para apontar para o espaço de nomes secundário. |
> | Ação | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Obtém a configuração da Recuperação Após Desastre associada ao espaço de nomes. |
> | Ação | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Cria ou Atualiza a configuração da Recuperação Após Desastre associada ao espaço de nomes. |
> | Ação | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Elimina o filtro do Event Grid associado ao espaço de nomes. |
> | Ação | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Obtém o filtro do Event Grid associado ao espaço de nomes. |
> | Ação | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Cria ou Atualiza o filtro do Event Grid associado ao espaço de nomes. |
> | Ação | Microsoft.ServiceBus/namespaces/eventhubs/read | Obter a lista de descrições de recursos de EventHub |
> | Ação | Microsoft.ServiceBus/namespaces/messagingPlan/read | Obtém o plano de serviço de mensagens para um espaço de nomes.<br>Esta API foi preterida.<br>As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)...<br>Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.ServiceBus/namespaces/messagingPlan/write | Atualiza o serviço de mensagens planear um espaço de nomes.<br>Esta API foi preterida.<br>As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)...<br>Esta operação não é suportada na API versão 2017-04-01. |
> | Ação | Microsoft.ServiceBus/namespaces/migrate/action | Migrar operação de espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Elimina a configuração da Migração. |
> | Ação | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Obtém a configuração da Migração, o que indica o estado da migração e as operações de replicação pendentes |
> | Ação | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Reverte a migração do espaço de nomes padrão para o premium |
> | Ação | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Atribui o DNS que está associado ao espaço de nomes padrão ao espaço de nomes premium, o que conclui a migração e para a sincronização de recursos do espaço de nomes padrão para o premium |
> | Ação | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Cria ou a configuração de migração de atualizações. Isto iniciará a sincronização de recursos do padrão para o espaço de nomes premium |
> | Ação | Microsoft.ServiceBus/namespaces/operationresults/read | Obter o estado da operação de Espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Obter a lista de registos de espaço de nomes descrições de recursos |
> | Ação | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de descrições de recursos de métricas de espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Operação para atualizar a fila. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT. |
> | Ação | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Operação para eliminar as regras de autorização de fila |
> | Ação | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Obter a cadeia de ligação para a fila |
> | Ação | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Obter a lista de regras de autorização de fila |
> | Ação | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Criar regras de autorização de fila e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados. |
> | Ação | Microsoft.ServiceBus/namespaces/queues/Delete | Operação para eliminar os recursos da fila |
> | Ação | Microsoft.ServiceBus/namespaces/queues/read | Obter a lista de descrições de recursos da fila |
> | Ação | Microsoft.ServiceBus/namespaces/queues/write | Criar ou as propriedades da fila de atualização. |
> | Ação | Microsoft.ServiceBus/namespaces/read | Obter a lista de Descrição de Recursos de Espaço de nomes |
> | Ação | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | Remova o espaço de nomes do ACS |
> | Ação | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Operação para atualizar o tópico. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT. |
> | Ação | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Operação para eliminar as regras de autorização de tópico |
> | Ação | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Obter a cadeia de ligação de tópico |
> | Ação | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Obter a lista de regras de autorização de tópico |
> | Ação | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Regenerar a chave Primária ou Secundária do Recurso |
> | Ação | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Criar regras de autorização de tópico e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados. |
> | Ação | Microsoft.ServiceBus/namespaces/topics/Delete | Operação para eliminar o recurso de tópico |
> | Ação | Microsoft.ServiceBus/namespaces/topics/read | Obter a lista de descrições de recursos de tópico |
> | Ação | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Operação para eliminar recurso TopicSubscription |
> | Ação | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Obter a lista de descrições de recurso TopicSubscription |
> | Ação | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Operação para eliminar o recurso de regra |
> | Ação | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Obter a lista de descrições de recurso de regra |
> | Ação | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Criar ou propriedades da regra de atualização. |
> | Ação | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Criar ou atualizar TopicSubscription propriedades. |
> | Ação | Microsoft.ServiceBus/namespaces/topics/write | Criar ou propriedades de tópico de atualização. |
> | Ação | Microsoft.ServiceBus/namespaces/write | Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas. |
> | Ação | Microsoft.ServiceBus/operations/read | Obter operações |
> | Ação | Microsoft.ServiceBus/register/action | Regista a subscrição do fornecedor de recursos do ServiceBus e ativa a criação de recursos do ServiceBus |
> | Ação | Microsoft.ServiceBus/sku/read | Obter a lista de descrições de recursos de Sku |
> | Ação | Microsoft.ServiceBus/sku/regions/read | Obter a lista de descrições de recurso SkuRegions |
> | Ação | Microsoft.ServiceBus/unregister/action | Regista a subscrição do fornecedor de recursos do ServiceBus e ativa a criação de recursos do ServiceBus |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.ServiceFabric/clusters/applications/delete | Eliminar qualquer Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applications/read | Ler qualquer Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applications/services/delete | Eliminar qualquer Serviço |
> | Ação | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Ler qualquer Partição |
> | Ação | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Ler qualquer Réplica |
> | Ação | Microsoft.ServiceFabric/clusters/applications/services/read | Ler qualquer Serviço |
> | Ação | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Ler qualquer Estado do Serviço |
> | Ação | Microsoft.ServiceFabric/clusters/applications/services/write | Criar ou Atualizar qualquer Serviço |
> | Ação | Microsoft.ServiceFabric/clusters/applications/write | Criar ou Atualizar qualquer Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Eliminar qualquer Tipo de Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applicationTypes/read | Ler qualquer Tipo de Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Eliminar qualquer Versão do Tipo de Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Ler qualquer Versão do Tipo de Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Criar ou Atualizar qualquer Versão do Tipo de Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/applicationTypes/write | Criar ou Atualizar qualquer Tipo de Aplicação |
> | Ação | Microsoft.ServiceFabric/clusters/delete | Eliminar qualquer Cluster |
> | Ação | Microsoft.ServiceFabric/clusters/nodes/read | Ler qualquer Nó |
> | Ação | Microsoft.ServiceFabric/clusters/read | Ler qualquer Cluster |
> | Ação | Microsoft.ServiceFabric/clusters/statuses/read | Ler qualquer Estado do Cluster |
> | Ação | Microsoft.ServiceFabric/clusters/write | Criar ou Atualizar qualquer Cluster |
> | Ação | Microsoft.ServiceFabric/locations/clusterVersions/read | Ler qualquer Versão de Cluster |
> | Ação | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Ler qualquer Versão de Cluster de um ambiente específico |
> | Ação | Microsoft.ServiceFabric/locations/operationresults/read | Ler quaisquer Resultados da Operação |
> | Ação | Microsoft.ServiceFabric/locations/operations/read | Ler quaisquer Operações por localização |
> | Ação | Microsoft.ServiceFabric/operations/read | Ler quaisquer Operações Disponíveis |
> | Ação | Microsoft.ServiceFabric/register/action | Registar qualquer Ação |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.SignalRService/checknameavailability/action | Verifica se um nome fica disponível para utilização com um novo serviço de SignalR |
> | Ação | Microsoft.SignalRService/register/action | Regista o fornecedor de recursos 'Microsoft.SignalRService' com uma subscrição |
> | Ação | Microsoft.SignalRService/SignalR/delete | Eliminar o SignalR todo |
> | Ação | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para SignalR |
> | Ação | Microsoft.SignalRService/SignalR/read | Ver definições e configurações de SignalR no portal de gestão ou através da API |
> | Ação | Microsoft.SignalRService/SignalR/write | Modificar as definições e configurações no portal de gestão ou através da API do SignalR |
> | Ação | Microsoft.SignalRService/unregister/action | Anula o registo do fornecedor de recursos 'Microsoft.SignalRService' com uma subscrição |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Solutions/applicationDefinitions/delete | Remove uma definição de aplicação. |
> | Ação | Microsoft.Solutions/applicationDefinitions/read | Obtém uma lista de definições de aplicação. |
> | Ação | Microsoft.Solutions/applicationDefinitions/write | Adicionar ou modificar uma definição de aplicação. |
> | Ação | Microsoft.Solutions/applications/delete | Remove uma aplicação. |
> | Ação | Microsoft.Solutions/applications/read | Obtém uma lista de aplicações. |
> | Ação | Microsoft.Solutions/applications/write | Cria uma aplicação. |
> | Ação | Microsoft.Solutions/locations/operationStatuses/read | Lê o estado da operação do recurso. |
> | Ação | Microsoft.Solutions/register/action | Registe-se nas Soluções. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Sql/checkNameAvailability/action | Certifique-se de que o se nome do servidor é disponível para o aprovisionamento em todo o mundo, para uma determinada subscrição. |
> | Ação | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Obter o resultado do blob servidor expandida operação de definição de política de auditoria |
> | Ação | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Obter o resultado do blob servidor operação de definição de política de auditoria |
> | Ação | Microsoft.Sql/locations/capabilities/read | Obtém as capacidades para esta subscrição numa localização especificada |
> | Ação | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Obtém o estado de uma operação de base de dados. |
> | Ação | Microsoft.Sql/locations/databaseOperationResults/read | Obtém o estado de uma operação de base de dados. |
> | Ação | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Obtém em curso operações no servidor eliminado |
> | Ação | Microsoft.Sql/locations/deletedServerOperationResults/read | Obtém em curso operações no servidor eliminado |
> | Ação | Microsoft.Sql/locations/deletedServers/read | Devolva a lista de servidores de eliminado ou obtém as propriedades para o servidor especificado foi eliminado. |
> | Ação | Microsoft.Sql/locations/deletedServers/recover/action | Recuperar um servidor eliminado |
> | Ação | Microsoft.Sql/locations/deleteVirtualNetworkOrSubnets/action | Elimina as regras de rede Virtual associadas a uma rede virtual ou a sub-rede |
> | Ação | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Obtém a operação assíncrona do azure para uma operação assíncrona do conjunto elástico |
> | Ação | Microsoft.Sql/locations/elasticPoolOperationResults/read | Obtém o resultado de uma operação de agrupamento elástico. |
> | Ação | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Obter o resultado do blob servidor expandida operação de definição de política de auditoria |
> | Ação | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Obter o resultado do blob servidor expandida operação de definição de política de auditoria |
> | Ação | Microsoft.Sql/locations/instanceFailoverGroups/delete | Elimina um grupo de ativação pós-falha de instância existente. |
> | Ação | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Executa a ativação pós-falha planeada num grupo de ativação pós-falha de instância existente. |
> | Ação | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Executa a ativação pós-falha forçada um grupo de ativação pós-falha de instância existente. |
> | Ação | Microsoft.Sql/locations/instanceFailoverGroups/read | Devolve a lista de ativação pós-falha de instância de grupos ou obtém as propriedades para o grupo de ativação pós-falha de instância especificada. |
> | Ação | Microsoft.Sql/locations/instanceFailoverGroups/write | Cria um grupo de ativação pós-falha de instância com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o grupo de ativação pós-falha de instância especificada. |
> | Ação | Microsoft.Sql/locations/longTermRetentionBackups/read | Lista as cópias de segurança de retenção de longo prazo para cada base de dados em cada servidor numa localização |
> | Ação | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Lista as cópias de segurança de retenção de longo prazo para cada base de dados num servidor |
> | Ação | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Elimina uma cópia de segurança de retenção de longo prazo |
> | Ação | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Lista as cópias de segurança de retenção de longo prazo para uma base de dados |
> | Ação | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Conclusão da operação de restauro de base de dados gerida |
> | Ação | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Obtém em curso operações de encriptação transparente de dados de base de dados gerida |
> | Ação | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Obtém em curso operações de encriptação transparente de dados de base de dados gerida |
> | Ação | Microsoft.Sql/locations/networkInterfaceAzureAsyncOperation/read | Devolve os detalhes de uma interface de rede específicas operação assíncrona do Azure |
> | Ação | Microsoft.Sql/locations/networkInterfaceOperationResults/read | Devolve os detalhes da operação de interface de rede especificado |
> | Ação | Microsoft.Sql/locations/read | Obtém as localizações disponíveis para uma determinada subscrição |
> | Ação | Microsoft.Sql/locations/syncAgentOperationResults/read | Obter o resultado da operação de recursos do agente de sincronização |
> | Ação | Microsoft.Sql/locations/syncDatabaseIds/read | Obter os ids de base de dados de sincronização para uma subscrição e região específica |
> | Ação | Microsoft.Sql/locations/syncGroupOperationResults/read | Obter o resultado da operação de recursos do grupo de sincronização |
> | Ação | Microsoft.Sql/locations/syncMemberOperationResults/read | Obter o resultado da operação de recursos do membro de sincronização |
> | Ação | Microsoft.Sql/locations/usages/read | Obtém uma coleção de métrica de utilização para esta subscrição numa localização |
> | Ação | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Devolve os detalhes das regras de rede virtual especificado operação assíncrona do azure  |
> | Ação | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Devolve os detalhes da operação de regras de rede virtual especificado  |
> | Ação | Microsoft.Sql/managedInstances/administrators/delete | Elimina um administrador existente da instância gerido. |
> | Ação | Microsoft.Sql/managedInstances/administrators/read | Obtém uma lista de administradores de instância gerido. |
> | Ação | Microsoft.Sql/managedInstances/administrators/write | Cria ou atualiza o administrador de instância geridos com os parâmetros especificados. |
> | Ação | Microsoft.Sql/managedInstances/databases/delete | Elimina uma base de dados gerido |
> | Ação | Microsoft.Sql/managedInstances/databases/read | Obtém existente gerido da base de dados |
> | Ação | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Obter os detalhes da política de deteção de ameaças da base de dados configurados numa determinada base de dados gerido |
> | Ação | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Alterar a política de deteção de ameaças de base de dados para uma determinada base de dados gerido |
> | Ação | Microsoft.Sql/managedInstances/databases/securityEvents/read | Obtém os eventos de segurança de base de dados gerida |
> | Ação | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Obter os detalhes da base de dados a encriptação transparente de dados numa determinada base de dados gerido |
> | Ação | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | Alterar a base de dados a encriptação transparente de dados para uma determinada base de dados gerido |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | Remova a avaliação da vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Obter detalhes sobre a avaliação da vulnerabilidade configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | Remover a linha de base da regra de avaliação vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | Obter a linha de base de regra de avaliação de vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | Altere a linha de base da regra de avaliação vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Converta um resultado de análise existente para um formato legível humano. Se já existe nada acontece |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Execute uma análise de avaliação da vulnerabilidade da base de dados. |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | Devolver a lista de vulnerabilidade de base de dados de registos de análise de avaliação ou obter o registo de análise para o ID especificado de análise. |
> | Ação | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | Alterar a avaliação da vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/managedInstances/databases/write | Cria uma nova base de dados ou atualiza uma base de dados existente. |
> | Ação | Microsoft.Sql/managedInstances/delete | Elimina uma instância existente gerida. |
> | Ação | Microsoft.Sql/managedInstances/metricDefinitions/read | Obter as definições de métrica de instância gerido |
> | Ação | Microsoft.Sql/managedInstances/metrics/read | Obter métricas de instância gerido |
> | Ação | Microsoft.Sql/managedInstances/read | Devolva a lista de instâncias geridas ou obtém as propriedades para a instância de gerido especificada. |
> | Ação | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Obter os detalhes da política de deteção de ameaças de servidor gerido configurado num determinado servidor gerido |
> | Ação | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Alterar a política de deteção de ameaças de servidor gerido para um determinado servidor gerido |
> | Ação | Microsoft.Sql/managedInstances/tdeCertificates/action | Certificado TDE criar/atualizar |
> | Ação | Microsoft.Sql/managedInstances/write | Cria uma instância gerida com os parâmetros especificados ou atualizar as propriedades ou etiquetas para a instância de gerido especificada. |
> | Ação | Microsoft.Sql/operations/read | Obtém as operações REST disponíveis |
> | Ação | Microsoft.Sql/register/action | Regista a subscrição para o fornecedor de recursos de base de dados do Microsoft SQL Server e permite a criação de bases de dados do Microsoft SQL Server. |
> | Ação | Microsoft.Sql/servers/administratorOperationResults/read | Operações em curso obtém os administradores de servidores |
> | Ação | Microsoft.Sql/servers/administrators/delete | Eliminar o administrador do servidor |
> | Ação | Microsoft.Sql/servers/administrators/read | Obter os detalhes de administrador do servidor |
> | Ação | Microsoft.Sql/servers/administrators/write | Criar ou atualizar o administrador do servidor |
> | Ação | Microsoft.Sql/servers/advisors/read | Devolve a lista de consultores disponíveis para o servidor |
> | Ação | Microsoft.Sql/servers/advisors/recommendedActions/read | Devolve a lista de ações recomendadas do advisor especificado para o servidor |
> | Ação | Microsoft.Sql/servers/advisors/recommendedActions/write | Aplicar a ação recomendada no servidor |
> | Ação | Microsoft.Sql/servers/advisors/write | As atualizações de autom-executar o estado de um advisor no nível do servidor. |
> | Ação | Microsoft.Sql/servers/auditingPolicies/read | Obter os detalhes da tabela de servidor predefinida configurada num determinado servidor de política de auditoria |
> | Ação | Microsoft.Sql/servers/auditingPolicies/write | Alterar a predefinição servidor tabela auditoria para um determinado servidor |
> | Ação | Microsoft.Sql/servers/auditingSettings/operationResults/read | Obter o resultado do blob servidor operação de definição de política de auditoria |
> | Ação | Microsoft.Sql/servers/auditingSettings/read | Obter detalhes sobre a servidor blob política de auditoria configurado num determinado servidor |
> | Ação | Microsoft.Sql/servers/auditingSettings/write | Alterar a auditoria de blob de servidor para um determinado servidor |
> | Ação | Microsoft.Sql/servers/automaticTuning/read | Devolve as definições de otimização automáticas para o servidor |
> | Ação | Microsoft.Sql/servers/automaticTuning/write | Atualizações de definições de otimização automáticas para o servidor e devolve as definições atualizadas |
> | Ação | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Elimina um cofre de arquivo de cópia de segurança existente. |
> | Ação | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | Esta operação é utilizada para obter um cofre de retenção de cópias de segurança de longo prazo. Devolve informações sobre o Cofre registada para este servidor |
> | Ação | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | Esta operação é utilizada para registar uma retenção de cópias de segurança de longa duração cofre para um servidor |
> | Ação | Microsoft.Sql/servers/communicationLinks/delete | Elimina uma ligação de comunicação de servidor existente. |
> | Ação | Microsoft.Sql/servers/communicationLinks/read | Devolva a lista de ligações de comunicação de um servidor especificado. |
> | Ação | Microsoft.Sql/servers/communicationLinks/write | Criar ou atualizar uma ligação de comunicação do servidor. |
> | Ação | Microsoft.Sql/servers/connectionPolicies/read | Devolva a lista de políticas de ligação do servidor de um servidor especificado. |
> | Ação | Microsoft.Sql/servers/connectionPolicies/write | Criar ou atualizar uma política de ligação do servidor. |
> | Ação | Microsoft.Sql/servers/databases/advisors/read | Devolve a lista de consultores disponíveis para a base de dados |
> | Ação | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Devolve a lista de ações recomendadas do advisor especificado para a base de dados |
> | Ação | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Aplicar a ação recomendada na base de dados |
> | Ação | Microsoft.Sql/servers/databases/advisors/write | Atualização automática-executar o estado de um advisor no nível de base de dados. |
> | Ação | Microsoft.Sql/servers/databases/auditingPolicies/read | Obter os detalhes da política de auditoria de tabela configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/servers/databases/auditingPolicies/write | Alterar a política de auditoria de tabela para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/auditingSettings/read | Obter os detalhes da política de auditoria de blob configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/servers/databases/auditingSettings/write | Alterar a política de auditoria de blob para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/auditRecords/read | Obter os registos de auditoria de blob de base de dados |
> | Ação | Microsoft.Sql/servers/databases/automaticTuning/read | Devolve as definições de otimização automáticas para uma base de dados |
> | Ação | Microsoft.Sql/servers/databases/automaticTuning/write | Atualizações de definições de otimização automáticas para uma base de dados e devolve as definições atualizadas |
> | Ação | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Obtém o estado de uma operação de base de dados. |
> | Ação | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Devolva a lista de políticas de arquivo de cópia de segurança de uma base de dados especificado. |
> | Ação | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Criar ou atualizar uma política de arquivo de cópia de segurança da base de dados. |
> | Ação | Microsoft.Sql/servers/databases/connectionPolicies/read | Obter os detalhes da política de ligação configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/servers/databases/connectionPolicies/write | Alterar a política de ligação para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Devolva a lista de políticas de máscara de dados de base de dados. |
> | Ação | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Eliminar regra de política para uma determinada base de dados de máscara de dados |
> | Ação | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Obter detalhes sobre os regra de política configurada numa base de dados indicado de máscara de dados |
> | Ação | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Alterar a regra de política para uma determinada base de dados de máscara de dados |
> | Ação | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Alterar a política para uma determinada base de dados de máscara de dados |
> | Ação | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Devolve as informações de passo de consulta distribuída da consulta de armazém de dados para o ID do passo selecionado |
> | Ação | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Devolve as informações da consulta de distribuição de armazém de dados para o ID de consulta selecionada |
> | Ação | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Obtém as atividades de utilizador de uma instância do SQL Data Warehouse que inclui as consultas em execução e suspensos |
> | Ação | Microsoft.Sql/servers/databases/delete | Elimina uma base de dados existente. |
> | Ação | Microsoft.Sql/servers/databases/export/action | Exportar a base de dados SQL do Azure |
> | Ação | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Obter os detalhes da política de auditoria de blob expandido configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Alterar a política de auditoria de blob expandido para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/extensions/read | Obtém uma coleção de extensões para a base de dados. |
> | Ação | Microsoft.Sql/servers/databases/extensions/write | Altere a extensão para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Obter as políticas de cópia de segurança de georreplicação para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Criar ou atualizar uma política de geobackup de base de dados |
> | Ação | Microsoft.Sql/servers/databases/importExportOperationResults/read | Obtém em curso operações de importação/exportação |
> | Ação | Microsoft.Sql/servers/databases/metricDefinitions/read | Tipos de retorno de métricas que estão disponíveis para bases de dados |
> | Ação | Microsoft.Sql/servers/databases/metrics/read | Métricas de retorno de bases de dados |
> | Ação | Microsoft.Sql/servers/databases/move/action | Mudar o nome da base de dados SQL do Azure |
> | Ação | Microsoft.Sql/servers/databases/operationResults/read | Obtém o estado de uma operação de base de dados. |
> | Ação | Microsoft.Sql/servers/databases/operations/cancel/action | Cancela a SQL Database do Azure operação assíncrona que não foi concluída ainda pendente. |
> | Ação | Microsoft.Sql/servers/databases/operations/read | Devolver a lista de operações executadas na base de dados |
> | Ação | Microsoft.Sql/servers/databases/pause/action | Base de dados de armazém de dados SQL do Azure de pausa |
> | Ação | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para bases de dados |
> | Ação | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Tipos de retorno de métricas que estão disponíveis para bases de dados |
> | Ação | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Devolve a coleção de textos de consulta que correspondem aos parâmetros especificados. |
> | Ação | Microsoft.Sql/servers/databases/queryStore/read | Devolve os valores atuais das definições de arquivo de consultas da base de dados. |
> | Ação | Microsoft.Sql/servers/databases/queryStore/write | Atualizações de definição de arquivo de consultas da base de dados |
> | Ação | Microsoft.Sql/servers/databases/read | Devolva a lista de bases de dados ou obtém as propriedades da base de dados especificado. |
> | Ação | Microsoft.Sql/servers/databases/replicationLinks/delete | Terminar a relação de replicação forçadamente e com uma potencial perda de dados |
> | Ação | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Ativação pós-falha após a sincronização de todos os é alterado de principal, tornar esta base de dados para a replicação relationship\u0027s primário e efetuar remoto primário para uma secundária |
> | Ação | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Ativação pós-falha imediatamente com potencial perda de dados, tornar esta base de dados para a replicação relationship\u0027s primário e efetuar remoto primário para uma secundária |
> | Ação | Microsoft.Sql/servers/databases/replicationLinks/read | Retorno detalhes sobre ligações de replicação estabelecidas para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Terminar a relação de replicação forçadamente ou após a sincronização com o parceiro |
> | Ação | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Atualizar o modo de replicação para a ligação para o modo síncrono ou assíncrono |
> | Ação | Microsoft.Sql/servers/databases/restorePoints/action | Cria um novo ponto de restauro |
> | Ação | Microsoft.Sql/servers/databases/restorePoints/delete | Elimina um ponto de restauro da base de dados. |
> | Ação | Microsoft.Sql/servers/databases/restorePoints/read | Devolve restaurar pontos para a base de dados. |
> | Ação | Microsoft.Sql/servers/databases/resume/action | Base de dados de armazém de dados SQL do Azure de retomar |
> | Ação | Microsoft.Sql/servers/databases/schemas/read | Obter a lista de esquemas de uma base de dados |
> | Ação | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Obter a lista de colunas de uma tabela |
> | Ação | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Eliminar a etiqueta de sensibilidade de uma dada coluna |
> | Ação | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Obter a etiqueta de sensibilidade de uma dada coluna |
> | Ação | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Criar ou atualizar a etiqueta de sensibilidade de uma dada coluna |
> | Ação | Microsoft.Sql/servers/databases/schemas/tables/read | Obter a lista de tabelas de uma base de dados |
> | Ação | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Obter a lista de recomendações do índice numa base de dados |
> | Ação | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Aplicar a recomendação do índice |
> | Ação | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Obter os detalhes da política de deteção de ameaças configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Alterar a política de deteção de ameaças para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/securityMetrics/read | Obtém uma coleção de métricas de segurança da base de dados |
> | Ação | Microsoft.Sql/servers/databases/sensitivityLabels/read | Etiquetas de sensibilidade da lista de uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Devolver a sugestão sobre dimensionar a base de dados ou reduzir verticalmente baseadas em estatísticas de execução de consulta para melhorar o desempenho ou reduzir o custo |
> | Ação | Microsoft.Sql/servers/databases/skus/read | Obtém uma coleção de skus disponíveis para uma base de dados |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Cancelar Sincronização de grupo de sincronização |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/delete | Elimina um grupo de sincronização existente. |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Devolver a lista de sincronização de esquemas de base de dados do hub |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/logs/read | Devolver a lista de registos do grupo de sincronização |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/read | Devolva a lista de sincronização de grupos ou obtém as propriedades para o grupo de sincronização especificada. |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Atualizar o esquema de base de dados do hub de sincronização |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Obter o resultado da operação de atualização de esquema de hub de sincronização |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Elimina um membro de sincronização existente. |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Devolva a lista de membros de sincronização ou obtém as propriedades para o membro de sincronização especificada. |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Atualizar o esquema de membro de sincronização |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Obter o resultado da operação de atualização de esquema de membro de sincronização |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Devolver a lista de sincronização de esquemas de base de dados do membro |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Cria um membro de sincronização com os parâmetros especificados ou atualiza as propriedades para o membro de sincronização especificada. |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Sincronização de grupo de sincronização do acionador |
> | Ação | Microsoft.Sql/servers/databases/syncGroups/write | Cria um grupo de sincronização com os parâmetros especificados ou atualiza as propriedades para o grupo de sincronização especificada. |
> | Ação | Microsoft.Sql/servers/databases/topQueries/queryText/action | Devolve o texto de Transact-SQL para o ID de consulta selecionada |
> | Ação | Microsoft.Sql/servers/databases/topQueries/read | Devolve agregar estatísticas de tempo de execução para a consulta selecionada no período de tempo selecionado |
> | Ação | Microsoft.Sql/servers/databases/topQueries/statistics/read | Devolve agregar estatísticas de tempo de execução para a consulta selecionada no período de tempo selecionado |
> | Ação | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Obtém em curso operações de encriptação transparente de dados |
> | Ação | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Obter o estado e os detalhes da funcionalidade de segurança de encriptação transparente de dados para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Alterar o estado de encriptação transparente de dados |
> | Ação | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Atualizar a base de dados do armazém de dados SQL do Azure |
> | Ação | Microsoft.Sql/servers/databases/usages/read | Obtém as informações de utilizações de SQL Database do Azure |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Remova a avaliação da vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Obter detalhes sobre a avaliação da vulnerabilidade configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Remover a linha de base da regra de avaliação vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Obter a linha de base de regra de avaliação de vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Altere a linha de base da regra de avaliação vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Converta um resultado de análise existente para um formato legível humano. Se já existe nada acontece |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Execute uma análise de avaliação da vulnerabilidade da base de dados. |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Devolver a lista de vulnerabilidade de base de dados de registos de análise de avaliação ou obter o registo de análise para o ID especificado de análise. |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Alterar a avaliação da vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Execute uma análise de avaliação da vulnerabilidade da base de dados. |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Obter o resultado da análise de avaliação da vulnerabilidade da base de dados a operação de execução |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Obter detalhes sobre a avaliação da vulnerabilidade configurado numa base de dados indicado |
> | Ação | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Alterar a avaliação da vulnerabilidade para uma determinada base de dados |
> | Ação | Microsoft.Sql/servers/databases/write | Cria uma base de dados com os parâmetros especificados ou atualiza as propriedades ou etiquetas da base de dados especificado. |
> | Ação | Microsoft.Sql/servers/delete | Elimina um servidor existente. |
> | Ação | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Elimina um configurações de recuperação após desastre existente para um determinado servidor |
> | Ação | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Ativação pós-falha um DisasterRecoveryConfiguration |
> | Ação | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Forçar a ativação pós-falha um DisasterRecoveryConfiguration |
> | Ação | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Obtém uma coleção de desastre configurações de recuperação que incluem este servidor |
> | Ação | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Alterar a configuração de recuperação de desastre servidor |
> | Ação | Microsoft.Sql/servers/elasticPoolEstimates/read | Devolve a lista de estimativas de conjunto elástico já criada para este servidor |
> | Ação | Microsoft.Sql/servers/elasticPoolEstimates/write | Cria nova estimativa de conjunto elástico para obter a lista de bases de dados fornecido |
> | Ação | Microsoft.Sql/servers/elasticPools/advisors/read | Devolve a lista de consultores disponíveis para o conjunto elástico |
> | Ação | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Devolve a lista de ações recomendadas do advisor especificado para o conjunto elástico |
> | Ação | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Aplicar a ação recomendada do conjunto elástico |
> | Ação | Microsoft.Sql/servers/elasticPools/advisors/write | Atualização automática-executar o estado de um advisor no nível do conjunto elástico. |
> | Ação | Microsoft.Sql/servers/elasticPools/databases/read | Obtém uma lista de bases de dados de um conjunto elástico |
> | Ação | Microsoft.Sql/servers/elasticPools/delete | Eliminar o conjunto elástico existente |
> | Ação | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Obter as atividades e detalhes sobre um conjunto de bases de dados elásticas indicado |
> | Ação | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Obter as atividades e os detalhes numa determinada base de dados que faz parte do agrupamento de base de dados elástica |
> | Ação | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Tipos de retorno de métricas que estão disponíveis para conjuntos de bases de dados elásticas |
> | Ação | Microsoft.Sql/servers/elasticPools/metrics/read | Métricas de retorno de conjuntos de bases de dados elásticas |
> | Ação | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Cancela o agrupamento elástico de SQL do Azure operação assíncrona que não foi concluída ainda pendente. |
> | Ação | Microsoft.Sql/servers/elasticPools/operations/read | Devolver a lista de operações executadas no conjunto elástico |
> | Ação | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Tipos de retorno de métricas que estão disponíveis para conjuntos de bases de dados elásticas |
> | Ação | Microsoft.Sql/servers/elasticPools/read | Obter os detalhes do conjunto elástico num determinado servidor |
> | Ação | Microsoft.Sql/servers/elasticPools/skus/read | Obtém uma coleção de skus disponíveis de um conjunto elástico |
> | Ação | Microsoft.Sql/servers/elasticPools/write | Crie um novo ou alterar as propriedades do conjunto elástico existente |
> | Ação | Microsoft.Sql/servers/encryptionProtector/read | Devolve uma lista de protetores de encriptação do servidor ou obtém as propriedades para o servidor especificado protetor de encriptação. |
> | Ação | Microsoft.Sql/servers/encryptionProtector/write | Atualize as propriedades para o Protetor de encriptação de servidor especificado. |
> | Ação | Microsoft.Sql/servers/extendedAuditingSettings/read | Obter os detalhes do blob expandida de servidor configurada num determinado servidor de política de auditoria |
> | Ação | Microsoft.Sql/servers/extendedAuditingSettings/write | Alterar a auditoria de blob de expandida de servidor para um determinado servidor |
> | Ação | Microsoft.Sql/servers/failoverGroups/delete | Elimina um grupo de ativação pós-falha existente. |
> | Ação | Microsoft.Sql/servers/failoverGroups/failover/action | Executa a ativação pós-falha planeada num grupo de ativação pós-falha existente. |
> | Ação | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Executa a ativação pós-falha forçada num grupo de ativação pós-falha existente. |
> | Ação | Microsoft.Sql/servers/failoverGroups/read | Devolve a lista de ativação pós-falha de grupos ou obtém as propriedades para o grupo de ativação pós-falha especificado. |
> | Ação | Microsoft.Sql/servers/failoverGroups/write | Cria um grupo de ativação pós-falha com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o grupo de ativação pós-falha especificado. |
> | Ação | Microsoft.Sql/servers/firewallRules/delete | Elimina uma regra de firewall do servidor existente. |
> | Ação | Microsoft.Sql/servers/firewallRules/read | Devolva a lista de firewall do servidor regras ou obtém as propriedades para o servidor especificado regra de firewall. |
> | Ação | Microsoft.Sql/servers/firewallRules/write | Cria uma regra de firewall do servidor com os parâmetros especificados, atualizar as propriedades para a regra especificada ou substituir todas as regras existentes com novas regras de firewall do servidor. |
> | Ação | Microsoft.Sql/servers/import/action | Criar uma nova base de dados no servidor e implementar o esquema e dados a partir de um pacote de DacPac |
> | Ação | Microsoft.Sql/servers/importExportOperationResults/read | Obtém em curso operações de importação/exportação |
> | Ação | Microsoft.Sql/servers/keys/delete | Elimina uma chave de servidor existente. |
> | Ação | Microsoft.Sql/servers/keys/read | Devolva a lista de servidor de chaves ou obtém as propriedades para a chave de servidor especificado. |
> | Ação | Microsoft.Sql/servers/keys/write | Cria uma chave com os parâmetros especificados ou atualiza as propriedades ou etiquetas da chave de servidor especificado. |
> | Ação | Microsoft.Sql/servers/networkInterfaces/read | Devolve as propriedades para a interface de rede especificado |
> | Ação | Microsoft.Sql/servers/networkInterfaces/write | Cria uma interface de rede com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a interface de rede especificado |
> | Ação | Microsoft.Sql/servers/operationResults/read | Obtém em curso operações de servidor |
> | Ação | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Tipos de retorno de métricas que estão disponíveis para servidores |
> | Ação | Microsoft.Sql/servers/read | Devolva a lista de servidores ou obtém as propriedades para o servidor especificado. |
> | Ação | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Obter métricas para os conjuntos de bases de dados elásticas recomendado para um determinado servidor |
> | Ação | Microsoft.Sql/servers/recommendedElasticPools/read | Obter a recomendação para conjuntos de bases de dados elásticas reduzir o custo ou melhorar o desempenho com base na utilização de recursos de historica |
> | Ação | Microsoft.Sql/servers/recoverableDatabases/read | Esta operação é utilizada para recuperação após desastre de base de dados dinâmicos para restaurar a base de dados ao conhecido do último ponto de cópia de segurança boa. Devolve informações sobre a última cópia de segurança boa mas doesn\u0027t, na verdade, restaure a base de dados. |
> | Ação | Microsoft.Sql/servers/restorableDroppedDatabases/read | Obter uma lista de bases de dados que foram ignorados num determinado servidor que estão ainda na política de retenção. |
> | Ação | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Obter os resultados da operação de escrita de política de deteção de ameaças de servidor |
> | Ação | Microsoft.Sql/servers/securityAlertPolicies/read | Obter os detalhes da política de deteção de ameaças de servidor configurada num determinado servidor |
> | Ação | Microsoft.Sql/servers/securityAlertPolicies/write | Alterar a política de deteção de ameaças de servidor para um determinado servidor |
> | Ação | Microsoft.Sql/servers/serviceObjectives/read | Obter a lista de serviço objetivos do nível (também conhecido como escalões de desempenho) disponíveis num determinado servidor |
> | Ação | Microsoft.Sql/servers/syncAgents/delete | Elimina um agente de sincronização existentes. |
> | Ação | Microsoft.Sql/servers/syncAgents/generateKey/action | Gerar a chave de registo do agente de sincronização |
> | Ação | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Devolver a lista de sincronizar bases de dados do agente ligado |
> | Ação | Microsoft.Sql/servers/syncAgents/read | Devolva a lista de agentes de sincronização ou obtém as propriedades para o agente de sincronização especificada. |
> | Ação | Microsoft.Sql/servers/syncAgents/write | Cria um agente de sincronização com os parâmetros especificados ou atualiza as propriedades para o agente de sincronização especificada. |
> | Ação | Microsoft.Sql/servers/tdeCertificates/action | Certificado TDE criar/atualizar |
> | Ação | Microsoft.Sql/servers/usages/read | Devolver a quota DTU do servidor e consuption DTU atual por todas as bases de dados no servidor |
> | Ação | Microsoft.Sql/servers/virtualNetworkRules/delete | Elimina uma regra de rede Virtual existente |
> | Ação | Microsoft.Sql/servers/virtualNetworkRules/read | Devolva a lista de rede virtual regras ou obtém as propriedades para a regra de rede virtual especificado. |
> | Ação | Microsoft.Sql/servers/virtualNetworkRules/write | Cria uma regra de rede virtual com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a regra de rede virtual especificado. |
> | Ação | Microsoft.Sql/servers/write | Cria um servidor com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o servidor especificado. |
> | Ação | Microsoft.Sql/unregister/action | Anula o registo da subscrição para o fornecedor de recursos de base de dados do Microsoft SQL Server e permite a criação de bases de dados do Microsoft SQL Server. |
> | Ação | Microsoft.Sql/virtualClusters/read | Devolva a lista de clusters virtuais ou obtém as propriedades para o cluster virtual especificado. |
> | Ação | Microsoft.Sql/virtualClusters/write | As atualizações de etiquetas de virtual cluster. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Storage/checknameavailability/read | Verifica se o nome da conta é válido e se não está já a ser utilizado. |
> | Ação | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Notifica o Microsoft.Storage que a rede ou sub-rede virtual está a ser eliminada |
> | Ação | Microsoft.Storage/locations/usages/read | Devolve o limite e a contagem de utilização atual de recursos na subscrição especificada |
> | Ação | Microsoft.Storage/operations/read | Consulta o estado de uma operação assíncrona. |
> | Ação | Microsoft.Storage/register/action | Regista a subscrição do fornecedor de recursos de armazenamento e ativa a criação de contas de armazenamento. |
> | Ação | Microsoft.Storage/skus/read | Lista os Skus suportados pelo Microsoft.Storage. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Devolve o resultado da adição de conteúdo de blob |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Devolve o resultado da eliminação de um blob |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Devolve um blob ou uma lista de blobs |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/runAsSuperUser/action | Devolve o resultado do comando de blobs |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Devolve o resultado de escrever um blob |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Limpar o arquivo legal do contentor de blobs |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Devolve o resultado da eliminação de um contentor |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Eliminar a política de imutabilidade do contentor de blobs |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Expandir a política de imutabilidade do contentor de blobs |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Bloquear a política de imutabilidade do contentor de blobs |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Obter a política de imutabilidade do contentor de blobs |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Colocar a política de imutabilidade do contentor de blobs |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/read | Devolve um contentor ou uma lista de contentores |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Definir o arquivo legal do contentor de blobs |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/containers/write | Devolve o resultado do contentor de blobs colocados ou de concessão |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Devolve uma chave de delegação de utilizador para o serviço blob |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/logDefinitions/read | Obtém a definição de registo para o Blob |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de definições das Métricas de Armazenamento Microsoft. |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/read | Devolve propriedades ou estatísticas do serviço Blob |
> | Ação | Microsoft.Storage/storageAccounts/blobServices/write | Devolve o resultado das propriedades do serviço de blobs colocado |
> | Ação | Microsoft.Storage/storageAccounts/delete | Elimina uma conta de armazenamento existente. |
> | Ação | Microsoft.Storage/storageAccounts/fileServices/fileShare/delete | Permite ao utilizador eliminar a partilha de ficheiros |
> | Ação | Microsoft.Storage/storageAccounts/fileServices/fileShare/read | Permite ao utilizador ler a partilha de ficheiros |
> | Ação | Microsoft.Storage/storageAccounts/fileServices/fileShare/write | Permite ao utilizador escrever numa partilha de ficheiros |
> | Ação | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/logDefinitions/read | Obtém a definição de registo para o Ficheiro |
> | Ação | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de definições das Métricas de Armazenamento Microsoft. |
> | Ação | Microsoft.Storage/storageAccounts/listAccountSas/action | Devolve o token de SAS de Conta para a conta de armazenamento especificada. |
> | Ação | Microsoft.Storage/storageAccounts/listkeys/action | Devolve as chaves de acesso da conta de armazenamento especificada. |
> | Ação | Microsoft.Storage/storageAccounts/listServiceSas/action | Devolve o token de SAS de Serviço para a conta de armazenamento especificada. |
> | Ação | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de definições das Métricas de Armazenamento Microsoft. |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/logDefinitions/read | Obtém a definição de registo para a Fila |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de definições das Métricas de Armazenamento Microsoft. |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Devolve o resultado da eliminação de uma fila |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Devolve o resultado da adição de uma mensagem |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Devolve o resultado da eliminação de uma mensagem |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Devolve o resultado do processamento de uma mensagem |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Devolve uma mensagem |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Devolve o resultado de escrever uma mensagem |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/queues/read | Devolve uma fila ou uma lista de filas. |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/queues/write | Devolve o resultado da escrita de uma fila |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/read | Propriedades ou estatistas do serviço Fila de devolução. |
> | Ação | Microsoft.Storage/storageAccounts/queueServices/write | Devolve o resultado de definir as propriedades do serviço Fila |
> | Ação | Microsoft.Storage/storageAccounts/read | Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada. |
> | Ação | Microsoft.Storage/storageAccounts/regeneratekey/action | Regenera as chaves de acesso da conta de armazenamento especificada. |
> | Ação | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Criar/atualizar definições de diagnóstico da conta de armazenamento. |
> | Ação | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição do diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição de diagnóstico para o recurso. |
> | Ação | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/logDefinitions/read | Obtém a definição de registo para a Tabela |
> | Ação | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read | Obter a lista de definições das Métricas de Armazenamento Microsoft. |
> | Ação | Microsoft.Storage/storageAccounts/write | Cria uma conta de armazenamento com os parâmetros especificados ou atualiza as propriedades ou etiquetas, ou adiciona um domínio personalizado à conta de armazenamento especificada. |
> | Ação | Microsoft.Storage/usages/read | Devolve o limite e a contagem de utilização atual de recursos na subscrição especificada |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | microsoft.storagesync/storageSyncServices/delete | Eliminar quaisquer serviços de sincronização de armazenamento |
> | Ação | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para os serviços de sincronização de armazenamento |
> | Ação | microsoft.storagesync/storageSyncServices/read | Ler quaisquer serviços de sincronização de armazenamento |
> | Ação | microsoft.storagesync/storageSyncServices/registeredServers/delete | Eliminar qualquer servidor registado |
> | Ação | microsoft.storagesync/storageSyncServices/registeredServers/read | Ler a qualquer servidor registado |
> | Ação | microsoft.storagesync/storageSyncServices/registeredServers/write | Criar ou atualizar qualquer servidor registado |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Eliminar quaisquer pontos finais de nuvem |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Api de localização para chamadas de cópia de segurança assíncrona |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Chamar esta ação depois de cópia de segurança |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Chamar esta ação depois de restauro |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Chamar esta ação antes de cópia de segurança |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Chamar esta ação antes de restauro |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Ler quaisquer pontos finais de nuvem |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Restaurar heartbeat |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Criar ou atualizar quaisquer pontos finais de nuvem |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/delete | Eliminar todos os grupos de sincronização |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/read | Ler todos os grupos de sincronização |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Eliminar quaisquer pontos finais de servidor |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Ler todos os pontos finais do servidor |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Chamar esta ação para recuperar um servidor de ficheiros |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Criar ou atualizar quaisquer pontos finais de servidor |
> | Ação | microsoft.storagesync/storageSyncServices/syncGroups/write | Criar ou atualizar quaisquer grupos de sincronização |
> | Ação | microsoft.storagesync/storageSyncServices/write | Criar ou atualizar os serviços de sincronização de armazenamento |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.StorSimple/managers/accessControlRecords/delete | Elimina os registos de controlo de acesso |
> | Ação | Microsoft.StorSimple/managers/accessControlRecords/read | Apresenta uma lista ou obtém os registos de controlo de acesso |
> | Ação | Microsoft.StorSimple/managers/accessControlRecords/write | Criar ou atualizar os registos de controlo de acesso |
> | Ação | Microsoft.StorSimple/managers/alerts/read | Apresenta uma lista ou obtém os alertas |
> | Ação | Microsoft.StorSimple/managers/bandwidthSettings/delete | Elimina as definições de largura de banda de um existente (8000 série apenas) |
> | Ação | Microsoft.StorSimple/managers/bandwidthSettings/read | Lista as definições de largura de banda (apenas 8000 série) |
> | Ação | Microsoft.StorSimple/managers/bandwidthSettings/write | Cria um novo ou atualiza as definições de largura de banda (8000 série apenas) |
> | Ação | Microsoft.StorSimple/Managers/certificates/write | A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre. |
> | Ação | Microsoft.StorSimple/managers/clearAlerts/action | Limpe todos os alertas associados com o Gestor de dispositivos. |
> | Ação | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | A aplicação de nuvem de lista as configurações suportadas |
> | Ação | Microsoft.StorSimple/managers/configureDevice/action | Configura um dispositivo |
> | Ação | Microsoft.StorSimple/managers/delete | Elimina os gestores de dispositivos |
> | Ação | Microsoft.StorSimple/Managers/delete | A operação eliminar cofre elimina os recursos do Azure especificado do tipo 'cofre' |
> | Ação | Microsoft.StorSimple/managers/devices/alertSettings/read | Apresenta uma lista ou obtém as definições de alerta |
> | Ação | Microsoft.StorSimple/managers/devices/alertSettings/write | Criar ou atualizar as definições de alerta |
> | Ação | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Efetuar uma cópia de segurança manual para criar um pedido cópia de segurança de todos os volumes protegidos pela política. |
> | Ação | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Elimina um políticas de cópia de segurança existente (8000 série apenas) |
> | Ação | Microsoft.StorSimple/managers/devices/backupPolicies/read | Políticas de lista a cópia de segurança (8000 série apenas) |
> | Ação | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Elimina um agendas existentes |
> | Ação | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Lista as agendas |
> | Ação | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Cria um novo ou agendas de atualizações |
> | Ação | Microsoft.StorSimple/managers/devices/backupPolicies/write | Cria um novo ou atualizações de políticas de cópia de segurança (8000 série apenas) |
> | Ação | Microsoft.StorSimple/managers/devices/backups/delete | Elimina o conjunto de cópia de segurança |
> | Ação | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Clone de uma partilha ou volume com um elemento de cópia de segurança. |
> | Ação | Microsoft.StorSimple/managers/devices/backups/read | Apresenta uma lista ou obtém o conjunto de cópia de segurança |
> | Ação | Microsoft.StorSimple/managers/devices/backups/restore/action | Restaure todos os volumes a partir de um conjunto de cópia de segurança. |
> | Ação | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Elimina os grupos de agenda de cópia de segurança |
> | Ação | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Apresenta uma lista ou obtém os grupos de agenda de cópia de segurança |
> | Ação | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Criar ou atualizar os grupos de agenda de cópia de segurança |
> | Ação | Microsoft.StorSimple/managers/devices/chapSettings/delete | Elimina as definições do Chap |
> | Ação | Microsoft.StorSimple/managers/devices/chapSettings/read | Apresenta uma lista ou obtém as definições Chap |
> | Ação | Microsoft.StorSimple/managers/devices/chapSettings/write | Criar ou atualizar as definições Chap |
> | Ação | Microsoft.StorSimple/managers/devices/deactivate/action | Desativa um dispositivo. |
> | Ação | Microsoft.StorSimple/managers/devices/delete | Elimina os dispositivos |
> | Ação | Microsoft.StorSimple/managers/devices/download/action | Atualizações de transferências para um dispositivo. |
> | Ação | Microsoft.StorSimple/managers/devices/failover/action | Ativação pós-falha do dispositivo. |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Efetuar cópia de segurança de um servidor de ficheiros. |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/delete | Elimina os servidores de ficheiros |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Apresenta uma lista ou obtém as métricas |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Apresenta uma lista ou obtém as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/read | Apresenta uma lista ou obtém os servidores de ficheiros |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Elimina as partilhas |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Apresenta uma lista ou obtém as métricas |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Apresenta uma lista ou obtém as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Apresenta uma lista ou obtém as partilhas |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Criar ou atualizar as partilhas |
> | Ação | Microsoft.StorSimple/managers/devices/fileservers/write | Criar ou atualizar os servidores de ficheiros |
> | Ação | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Alterar o estado de energia do controlador de grupos de componentes de hardware |
> | Ação | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Lista os grupos de componentes de Hardware |
> | Ação | Microsoft.StorSimple/managers/devices/install/action | Instale as atualizações num dispositivo. |
> | Ação | Microsoft.StorSimple/managers/devices/installUpdates/action | Instala as atualizações nos dispositivos |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Efetuar cópia de segurança de um servidor de iSCSI. |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Elimina o iSCSI de servidores |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Elimina os discos |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Apresenta uma lista ou obtém as métricas |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Apresenta uma lista ou obtém as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Apresenta uma lista ou obtém os discos |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Criar ou atualizar os discos |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Apresenta uma lista ou obtém as métricas |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Apresenta uma lista ou obtém as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/read | Apresenta uma lista ou obtém o iSCSI de servidores |
> | Ação | Microsoft.StorSimple/managers/devices/iscsiservers/write | Criar ou atualizar o iSCSI de servidores |
> | Ação | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Cancelar uma tarefa em execução |
> | Ação | Microsoft.StorSimple/managers/devices/jobs/read | Apresenta uma lista ou obtém as tarefas |
> | Ação | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Liste os conjuntos de ativação pós-falha de um dispositivo existente. |
> | Ação | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Destinos de ativação pós-falha de lista dos dispositivos |
> | Ação | Microsoft.StorSimple/managers/devices/metrics/read | Apresenta uma lista ou obtém as métricas |
> | Ação | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Apresenta uma lista ou obtém as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Confirma que uma migração com êxito e a consolidação da mesma. |
> | Ação | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Obter o estado de confirmar de migração. |
> | Ação | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Obter o estado da tarefa de migração estimativa. |
> | Ação | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Obter o estado para a migração. |
> | Ação | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Importar configurações de origem para migração |
> | Ação | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Começar a utilizar configurações de origem de migração |
> | Ação | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Inicie uma tarefa para estimar a duração do processo de migração. |
> | Ação | Microsoft.StorSimple/managers/devices/networkSettings/read | Apresenta uma lista ou obtém as definições de rede |
> | Ação | Microsoft.StorSimple/managers/devices/networkSettings/write | Cria um novo ou atualiza as definições de rede |
> | Ação | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Chave de encriptação pública da lista do Gestor de dispositivos |
> | Ação | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Publica o pacote de suporte de um dispositivo para a resolução de problemas de Support da Microsoft. |
> | Ação | Microsoft.StorSimple/managers/devices/read | Apresenta uma lista ou obtém os dispositivos |
> | Ação | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Análise de atualizações de um dispositivo. |
> | Ação | Microsoft.StorSimple/managers/devices/securitySettings/read | Lista as definições de segurança |
> | Ação | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Sincronize o certificado de gestão remota para um dispositivo. |
> | Ação | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Atualize as definições de segurança. |
> | Ação | Microsoft.StorSimple/managers/devices/securitySettings/write | Cria um novo ou atualiza as definições de segurança |
> | Ação | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Envie e-mail de teste de alerta para destinatários de correio eletrónico configurado. |
> | Ação | Microsoft.StorSimple/managers/devices/timeSettings/read | Apresenta uma lista ou obtém as definições de tempo |
> | Ação | Microsoft.StorSimple/managers/devices/timeSettings/write | Cria um novo ou atualiza as definições de hora |
> | Ação | Microsoft.StorSimple/managers/devices/updateSummary/read | Apresenta uma lista ou obtém o resumo de atualização |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Elimina um contentores de Volume existente (8000 série apenas) |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/listEncryptionKeys/action | Lista de chaves de encriptação de contentores de Volume |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Lista as métricas |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Lista as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/read | Lista os contentores de Volume (8000 série apenas) |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/rolloverEncryptionKey/action | Chaves de encriptação de rollover de contentores de Volume |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Elimina um Volumes existentes |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Lista as métricas |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Lista as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Lista de Volumes |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Cria um novo ou Volumes de atualizações |
> | Ação | Microsoft.StorSimple/managers/devices/volumeContainers/write | Cria um novo ou atualiza os contentores de Volume (8000 série apenas) |
> | Ação | Microsoft.StorSimple/managers/devices/write | Criar ou atualizar os dispositivos |
> | Ação | Microsoft.StorSimple/managers/encryptionSettings/read | Apresenta uma lista ou obtém as definições de encriptação |
> | Ação | Microsoft.StorSimple/Managers/extendedInformation/delete | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Ação | Microsoft.StorSimple/Managers/extendedInformation/read | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Ação | Microsoft.StorSimple/Managers/extendedInformation/write | A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure |
> | Ação | Microsoft.StorSimple/managers/getActivationKey/action | Obter a chave de ativação para o Gestor de dispositivos. |
> | Ação | Microsoft.StorSimple/managers/getEncryptionKey/action | Obter a chave de encriptação para o Gestor de dispositivos. |
> | Ação | Microsoft.StorSimple/managers/listActivationKey/action | Obtém a chave de ativação do Gestor de dispositivos StorSimple. |
> | Ação | Microsoft.StorSimple/managers/listPrivateEncryptionKey/action | Obtém a chave privada de encriptação para um Gestor de dispositivos do StorSimple. |
> | Ação | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Listar chaves de encriptação pública de um Gestor de dispositivos do StorSimple. |
> | Ação | Microsoft.StorSimple/managers/metrics/read | Apresenta uma lista ou obtém as métricas |
> | Ação | Microsoft.StorSimple/managers/metricsDefinitions/read | Apresenta uma lista ou obtém as definições de métricas |
> | Ação | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Crie uma nova aplicação de nuvem. |
> | Ação | Microsoft.StorSimple/managers/read | Apresenta uma lista ou obtém os gestores de dispositivos |
> | Ação | Microsoft.StorSimple/Managers/read | A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre' |
> | Ação | Microsoft.StorSimple/managers/regenarateRegistationCertificate/action | Regenere o certificado de registo para os gestores de dispositivos. |
> | Ação | Microsoft.StorSimple/managers/regenerateActivationKey/action | Regenere a chave de ativação para o Gestor de dispositivos. |
> | Ação | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Elimina as credenciais da conta de armazenamento |
> | Ação | Microsoft.StorSimple/managers/storageAccountCredentials/listAccessKey/action | Lista de chaves de acesso das credenciais de conta de armazenamento |
> | Ação | Microsoft.StorSimple/managers/storageAccountCredentials/read | Apresenta uma lista ou obtém as credenciais da conta de armazenamento |
> | Ação | Microsoft.StorSimple/managers/storageAccountCredentials/write | Criar ou atualizar as credenciais da conta de armazenamento |
> | Ação | Microsoft.StorSimple/managers/storageDomains/delete | Elimina os domínios de armazenamento |
> | Ação | Microsoft.StorSimple/managers/storageDomains/read | Apresenta uma lista ou obtém os domínios de armazenamento |
> | Ação | Microsoft.StorSimple/managers/storageDomains/write | Criar ou atualizar os domínios de armazenamento |
> | Ação | Microsoft.StorSimple/managers/write | Criar ou atualizar os gestores de dispositivos |
> | Ação | Microsoft.StorSimple/Managers/write | A operação Criar Cofre cria um recurso do tipo "cofre" do Azure |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.StreamAnalytics/locations/quotas/Read | Quota de subscrição de análise de fluxo de leitura |
> | Ação | Microsoft.StreamAnalytics/operations/Read | Operações de leitura Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/Register/action | Registar a subscrição com o fornecedor de recursos do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/Delete | Eliminar a tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Eliminar a função de tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Resultados da operação de leitura para a função de tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Função de tarefa de análise de fluxo de leitura |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Obter definição predefinida de uma função de tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Função de tarefa de análise de fluxo de teste |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Escrever a função de tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Eliminar a entrada de fluxo de trabalho de análise |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Resultados da operação de leitura para a entrada de fluxo de trabalho de análise |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Entrada de tarefa de análise de fluxo de leitura |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Entrada de tarefa de análise de sequência de exemplo |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Entrada de tarefa de análise de fluxo de teste |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Escrever a entrada de fluxo de trabalho de análise |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Definições de métrica de leitura |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Resultados da operação de leitura para a tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Eliminar o resultado da tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Resultados da operação de leitura do resultado de tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Resultado da tarefa leitura Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Saída de tarefa de análise de fluxo de teste |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Escrever a saída da tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Definição de diagnóstico de leitura. |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Escreva a definição de diagnóstico. |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para streamingjobs |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para streamingjobs |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/Read | Leitura tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/Start/action | Iniciar a tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Parar a tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Eliminar a transformação de tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Transformação de tarefa de análise de fluxo de leitura |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Escrever transformação de tarefa do Stream Analytics |
> | Ação | Microsoft.StreamAnalytics/streamingjobs/Write | Escrever a tarefa do Stream Analytics |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Subscription/CreateSubscription/action | Criar uma subscrição do Azure |
> | Ação | Microsoft.Subscription/SubscriptionDefinitions/read | Obter uma definição de subscrição do Azure dentro de um grupo de gestão. |
> | Ação | Microsoft.Subscription/SubscriptionDefinitions/write | Criar uma definição de subscrição do Azure |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Support/register/action | Regista para o Fornecedor de Recursos de Suporte |
> | Ação | Microsoft.Support/supportTickets/read | Obtém os detalhes do Pedido de Suporte (incluindo o estado, gravidade, detalhes de contacto e comunicações) ou obtém a lista dos Pedidos de Suporte nas subscrições. |
> | Ação | Microsoft.Support/supportTickets/write | Cria ou atualiza um pedido de suporte. Pode criar um pedido de suporte para técnica, Quotas ou gestão de subscrição de faturação problemas relacionados com. Pode atualizar a gravidade, detalhes de contacto e comunicações para pedidos de suporte existentes. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Elimina a política de acesso. |
> | Ação | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Obter as propriedades de uma política de acesso. |
> | Ação | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Cria uma nova política de acesso para um ambiente ou atualiza uma política de acesso existentes. |
> | Ação | Microsoft.TimeSeriesInsights/environments/delete | Elimina o ambiente. |
> | Ação | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Elimina a origem do evento. |
> | Ação | Microsoft.TimeSeriesInsights/environments/eventsources/eventsources/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para eventsources |
> | Ação | Microsoft.TimeSeriesInsights/environments/eventsources/read | Obter as propriedades de uma origem de evento. |
> | Ação | Microsoft.TimeSeriesInsights/environments/eventsources/write | Cria uma nova origem de evento para um ambiente ou atualiza uma origem de evento existente. |
> | Ação | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/metricDefinitions/read | Obtem as métricas disponíveis para ambientes |
> | Ação | Microsoft.TimeSeriesInsights/environments/read | Obter as propriedades de um ambiente. |
> | Ação | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Elimina o conjunto de dados de referência. |
> | Ação | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Obter as propriedades de um conjunto de dados de referência. |
> | Ação | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Cria um novo conjunto de dados de referência para um ambiente ou atualiza um conjunto de dados de referência existente. |
> | Ação | Microsoft.TimeSeriesInsights/environments/status/read | Obter o estado do ambiente, estado do respetivos operações associados como entrada. |
> | Ação | Microsoft.TimeSeriesInsights/environments/write | Cria um novo ambiente ou atualiza um ambiente existente. |
> | Ação | Microsoft.TimeSeriesInsights/register/action | Regista a subscrição para o fornecedor de recursos de informações de séries de tempo e permite a criação dos ambientes das informações de séries de tempo. |

## <a name="microsoftvisualstudio"></a>Microsoft.visualstudio

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.VisualStudio/Account/Delete | Eliminar Conta |
> | Ação | Microsoft.VisualStudio/Account/Read | Conta de leitura |
> | Ação | Microsoft.VisualStudio/Account/Write | Conta do conjunto |
> | Ação | Microsoft.VisualStudio/Extension/Delete | Eliminar a extensão |
> | Ação | Microsoft.VisualStudio/Extension/Read | Extensão de leitura |
> | Ação | Microsoft.VisualStudio/Extension/Write | Definir extensão |
> | Ação | Microsoft.VisualStudio/Project/Delete | Eliminar Projeto |
> | Ação | Microsoft.VisualStudio/Project/Read | Projeto de leitura |
> | Ação | Microsoft.VisualStudio/Project/Write | Defina o projeto |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.Web/apimanagementaccounts/apiacls/Read | Obter Apiacls de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/apiacls/DELETE | Elimine Apiacls de APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/apiacls/Read | Obter Apiacls de APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/apiacls/Write | Atualize Apiacls de APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/connectionacls/Read | Obter Connectionacls de APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/confirmconsentcode/Action | Certifique-se ligações de APIs de contas de gestão Api código consentimento. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/connectionacls/DELETE | Elimine a Api Management contas APIs ligações Connectionacls. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/connectionacls/Read | Obter a Api Management contas APIs ligações Connectionacls. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/connectionacls/Write | Atualize a Api Management contas APIs ligações Connectionacls. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/DELETE | Elimine ligações de APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/getconsentlinks/Action | Obter consentimento ligações para ligações de APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/listconnectionkeys/Action | Lista ligação chaves Api contas APIs ligações de gestão. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/listsecrets/Action | Lista os segredos Api contas APIs ligações de gestão. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/Read | Obter ligações de APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Connections/Write | Ligações de APIs de contas de gestão de Api de atualização. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/DELETE | Elimine APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/localizeddefinitions/DELETE | Eliminar a Api Management contas APIs localizado definições. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/localizeddefinitions/Read | Obter gestão de Api contas APIs localizado definições. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/localizeddefinitions/Write | APIs de contas de gestão de Api de atualização localizado definições. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Read | Obter APIs de contas de gestão de Api. |
> | Ação | Microsoft.Web/apimanagementaccounts/APIs/Write | APIs de contas de gestão de Api de atualização. |
> | Ação | Microsoft.Web/apimanagementaccounts/connectionacls/Read | Obter Connectionacls de contas de gestão de Api. |
> | Ação | Microsoft.Web/availablestacks/Read | Obter pilhas disponíveis. |
> | Ação | Microsoft.Web/billingmeters/Read | Obter a lista de faturação medidores. |
> | Ação | Microsoft.Web/certificates/Delete | Elimine um certificado existente. |
> | Ação | Microsoft.Web/certificates/Read | Obter a lista de certificados. |
> | Ação | Microsoft.Web/certificates/Write | Adicionar um certificado novo ou atualizar um já existente. |
> | Ação | Microsoft.Web/checknameavailability/Read | Verifique se o nome do recurso está disponível. |
> | Ação | Microsoft.Web/classicmobileservices/Read | Obter clássicos Mobile Services. |
> | Ação | Microsoft.Web/connectionGateways/Delete | Elimina um Gateway de ligação. |
> | Ação | Microsoft.Web/connectionGateways/Join/Action | Associa um Gateway de ligação. |
> | Ação | Microsoft.Web/connectionGateways/ListStatus/Action | Apresenta uma lista de estado de um Gateway de ligação. |
> | Ação | Microsoft.Web/connectionGateways/Move/Action | Move um Gateway de ligação. |
> | Ação | Microsoft.Web/connectionGateways/Read | Obter a lista de Gateways de ligação. |
> | Ação | Microsoft.Web/connectionGateways/Write | Cria ou atualiza um Gateway de ligação. |
> | Ação | Microsoft.Web/Connections/confirmconsentcode/Action | Confirme o código de autorização de ligações. |
> | Ação | Microsoft.Web/connections/Delete | Elimina uma ligação. |
> | Ação | Microsoft.Web/connections/Join/Action | Associa uma ligação. |
> | Ação | Microsoft.Web/Connections/listconsentlinks/Action | Ligações de consentimento de lista de ligações. |
> | Ação | Microsoft.Web/connections/Move/Action | Move uma ligação. |
> | Ação | Microsoft.Web/connections/Read | Obter a lista de ligações. |
> | Ação | Microsoft.Web/connections/Write | Cria ou atualiza uma ligação. |
> | Ação | Microsoft.Web/customApis/Delete | Elimina uma API personalizada. |
> | Ação | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Extrai a definição da API de um WSDL. |
> | Ação | Microsoft.Web/customApis/Join/Action | Associa uma API personalizada. |
> | Ação | Microsoft.Web/customApis/listWsdlInterfaces/Action | Apresenta uma lista de interfaces WSDL para uma API personalizada. |
> | Ação | Microsoft.Web/customApis/Move/Action | Move uma API personalizada. |
> | Ação | Microsoft.Web/customApis/Read | Obter a lista de API personalizada. |
> | Ação | Microsoft.Web/customApis/Write | Cria ou atualiza uma API personalizada. |
> | Ação | Microsoft.Web/deletedSites/Read | Obter as propriedades de uma aplicação de Web eliminado |
> | Ação | Microsoft.Web/deploymentlocations/Read | Obter as localizações de implementação. |
> | Ação | Microsoft.Web/geoRegions/Read | Obter a lista de regiões de Georreplicação. |
> | Ação | Microsoft.Web/hostingenvironments/capacities/Read | Obter as capacidades de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingEnvironments/Delete | Eliminar um ambiente de serviço de aplicações |
> | Ação | Microsoft.Web/hostingenvironments/detectors/Read | Obter detetores de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/Diagnostics/Read | Obter o diagnóstico de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/inboundnetworkdependenciesendpoints/Read | Obter os pontos finais de rede de todas as dependências de entrada. |
> | Ação | Microsoft.Web/hostingenvironments/metricdefinitions/Read | Obter as definições de métrica de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/multirolepools/metricdefinitions/Read | Obter as definições de métrica MultiRole conjuntos de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/multirolepools/Metrics/Read | Obter métricas de MultiRole conjuntos de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/metricDefinitions/Read | Obtem as métricas disponíveis para MultiRole de ambiente de serviço de aplicações |
> | Ação | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Obter as propriedades de um conjunto de front-end num ambiente de serviço de aplicações |
> | Ação | Microsoft.Web/hostingenvironments/multirolepools/SKUs/Read | Obter SKUs de MultiRole conjuntos de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/multirolepools/usages/Read | Obter utilizações MultiRole conjuntos de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Criar um novo conjunto de front-end num ambiente de serviço de aplicações ou atualizar um já existente |
> | Ação | Microsoft.Web/hostingenvironments/Operations/Read | Obter operações de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/outboundnetworkdependenciesendpoints/Read | Obter os pontos finais de rede de todas as dependências de saída. |
> | Ação | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Web/hostingEnvironments/Read | Obter as propriedades de um ambiente de serviço de aplicações |
> | Ação | Microsoft.Web/hostingEnvironments/reboot/Action | Reinicie todas as máquinas no ambiente de serviço de aplicações |
> | Ação | Microsoft.Web/hostingenvironments/resume/Action | Retomar a ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/serverfarms/Read | Obter planos de serviço de aplicações de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/sites/Read | Obter aplicações Web de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/suspend/Action | Suspenda a ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/usages/Read | Obter utilizações de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/workerpools/metricdefinitions/Read | Obter definições de métrica de Workerpools de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingenvironments/workerpools/Metrics/Read | Obter métricas de Workerpools de ambientes de alojamento. |
> | Ação | Microsoft.Web/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read | Obtem as métricas disponíveis para WorkerPool de ambiente de serviço de aplicações |
> | Ação | Microsoft.Web/hostingEnvironments/workerPools/Read | Obter as propriedades de um conjunto de trabalho num ambiente de serviço de aplicações |
> | Ação | Microsoft.Web/hostingenvironments/workerpools/SKUs/Read | Obter o alojamento de ambientes Workerpools SKUs. |
> | Ação | Microsoft.Web/hostingenvironments/workerpools/usages/Read | Obter o alojamento de ambientes Workerpools utilizações. |
> | Ação | Microsoft.Web/hostingEnvironments/workerPools/Write | Criar um novo conjunto de trabalho num ambiente de serviço de aplicações ou atualizar um já existente |
> | Ação | Microsoft.Web/hostingEnvironments/Write | Criar um novo ambiente de serviço de aplicações ou Atualize uma existente |
> | Ação | Microsoft.Web/ishostingenvironmentnameavailable/Read | Get se o nome do ambiente de alojamento está disponível. |
> | Ação | Microsoft.Web/ishostnameavailable/Read | Verifique se o nome do anfitrião está disponível. |
> | Ação | Microsoft.Web/isusernameavailable/Read | Verifique se o nome de utilizador está disponível. |
> | Ação | Microsoft.Web/listSitesAssignedToHostName/Read | Obter os nomes dos sites atribuídos ao nome do anfitrião. |
> | Ação | Microsoft.Web/Locations/apioperations/Read | Obter operações de API de localizações. |
> | Ação | Microsoft.Web/Locations/connectiongatewayinstallations/Read | Obter instalações de Gateway de ligação de localizações. |
> | Ação | Microsoft.Web/Locations/extractapidefinitionfromwsdl/Action | Extrair definição da Api de WSDL para localizações. |
> | Ação | Microsoft.Web/Locations/listwsdlinterfaces/Action | Interfaces WSDL de lista de localizações. |
> | Ação | Microsoft.Web/Locations/managedapis/apioperations/Read | Obter operações de API gerido de localizações. |
> | Ação | Microsoft.Web/locations/managedapis/Join/Action | Associa uma API gerida. |
> | Ação | Microsoft.Web/Locations/managedapis/Read | Obter as localizações de APIs geridas. |
> | Ação | Microsoft.Web/Operations/Read | Obter operações. |
> | Ação | Microsoft.Web/publishingusers/Read | Obter a publicação os utilizadores. |
> | Ação | Microsoft.Web/publishingusers/Write | Atualize a publicação de utilizadores. |
> | Ação | Microsoft.Web/recommendations/Read | Obter a lista de recomendações para subscrições. |
> | Ação | Microsoft.Web/register/Action | Registe o fornecedor de recursos Microsoft. Web para a subscrição. |
> | Ação | Microsoft.Web/resourcehealthmetadata/Read | Obter os metadados do Estado de funcionamento de recursos. |
> | Ação | Microsoft.Web/serverfarms/Capabilities/Read | Obter capacidades de planos do App Service. |
> | Ação | Microsoft.Web/serverfarms/Delete | Eliminar uma existente plano do App Service |
> | Ação | Microsoft.Web/serverfarms/firstpartyapps/Settings/DELETE | Elimine o serviço de aplicações planos primeiro intervenientes as definições de aplicações. |
> | Ação | Microsoft.Web/serverfarms/firstpartyapps/Settings/Read | Obter definições de aplicações intervenientes primeiro planos do serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/firstpartyapps/Settings/Write | Atualize definições de aplicações da primeira planos do serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/DELETE | Elimine reencaminhamentos de espaços de nomes de ligação do serviço de aplicações planos híbrida. |
> | Ação | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/Read | Obter reencaminhamentos de espaços de nomes de ligação do serviço de aplicações planos híbrida. |
> | Ação | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/sites/Read | Obter planos do serviço de aplicações híbridas ligação espaços de nomes reencaminhamentos da aplicações Web. |
> | Ação | Microsoft.Web/serverfarms/hybridconnectionplanlimits/Read | Obter os limites de plano de ligação do serviço de aplicações planos híbrida. |
> | Ação | Microsoft.Web/serverfarms/hybridconnectionrelays/Read | Obter planos de serviço de aplicações híbridas reencaminhamentos de ligação. |
> | Ação | Microsoft.Web/serverfarms/metricdefinitions/Read | Obter as definições de métrica de planos do serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/Metrics/Read | Obter métricas de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/operationresults/Read | Obter os resultados da operação planos do serviço de aplicações. |
> | Ação | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | Microsoft.Web/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read | Obtem as métricas disponíveis para o plano do App Service |
> | Ação | Microsoft.Web/serverfarms/Read | Obter as propriedades num plano do App Service |
> | Ação | Microsoft.Web/serverfarms/restartSites/Action | Reiniciar todas as aplicações Web dentro de um plano do App Service |
> | Ação | Microsoft.Web/serverfarms/sites/Read | Obter aplicações de Web de planos do App Service. |
> | Ação | Microsoft.Web/serverfarms/SKUs/Read | Obter SKUs de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/usages/Read | Obter utilizações de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/virtualnetworkconnections/gateways/Write | Atualize os Gateways de ligações de rede Virtual de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/virtualnetworkconnections/Read | Obter as ligações de rede Virtual de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/virtualnetworkconnections/routes/DELETE | Elimine rotas de ligações de rede Virtual de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/virtualnetworkconnections/routes/Read | Obter rotas de ligações de rede Virtual de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/virtualnetworkconnections/routes/Write | Atualize rotas de ligações de rede Virtual de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/Workers/reboot/Action | Reinicie trabalhadores de planos de serviço de aplicações. |
> | Ação | Microsoft.Web/serverfarms/Write | Criar um novo plano do App Service ou atualizar um já existente |
> | Ação | Microsoft.Web/sites/analyzecustomhostname/Read | Analise o nome do anfitrião personalizado. |
> | Ação | Microsoft.Web/sites/applySlotConfig/Action | Aplicar a configuração de ranhura de aplicação do web a partir da ranhura de destino para a aplicação web atual |
> | Ação | Microsoft.Web/sites/backup/Action | Criar uma nova cópia de segurança de aplicação de web |
> | Ação | Microsoft.Web/sites/backup/Read | Obter a cópia de segurança do Web Apps. |
> | Ação | Microsoft.Web/sites/backup/Write | Atualize a cópia de segurança do Web Apps. |
> | Ação | Microsoft.Web/sites/backups/Action | Deteta uma cópia de segurança de aplicação existente que pode ser restaurada a partir de um blob no storage do Azure. |
> | Ação | Microsoft.Web/sites/backups/DELETE | Elimine as cópias de segurança do Web Apps. |
> | Ação | Microsoft.Web/sites/backups/List/Action | Lista Web Apps as cópias de segurança. |
> | Ação | Microsoft.Web/sites/backups/Read | Obter as propriedades de cópia de segurança de uma aplicação web |
> | Ação | Microsoft.Web/sites/backups/Restore/Action | Restaure cópias de segurança do Web Apps. |
> | Ação | Microsoft.Web/sites/backups/Write | Atualize a cópias de segurança de aplicações Web. |
> | Ação | Microsoft.Web/sites/config/DELETE | Elimine a configuração de aplicações Web. |
> | Ação | Microsoft.Web/sites/config/list/Action | Lista confidenciais definições de segurança da aplicação Web, tais como publicar as credenciais, as definições de aplicação e as cadeias de ligação |
> | Ação | Microsoft.Web/sites/config/Read | Obter definições de configuração de aplicação Web |
> | Ação | Microsoft.Web/sites/config/Write | Atualizar as definições de configuração da aplicação Web |
> | Ação | Microsoft.Web/sites/containerlogs/Action | Obter zipado registos do contentor para a aplicação Web. |
> | Ação | Microsoft.Web/sites/continuouswebjobs/DELETE | Elimine tarefas de Web contínua de aplicações Web. |
> | Ação | Microsoft.Web/sites/continuouswebjobs/Read | Obter as tarefas de Web contínua de aplicações Web. |
> | Ação | Microsoft.Web/sites/continuouswebjobs/Start/Action | Inicie tarefas de Web contínua de aplicações Web. |
> | Ação | Microsoft.Web/sites/continuouswebjobs/Stop/Action | Pare tarefas de Web contínua de aplicações Web. |
> | Ação | Microsoft.Web/sites/Delete | Eliminar uma aplicação Web existente |
> | Ação | Microsoft.Web/sites/Deployments/DELETE | Elimine as implementações de aplicações Web. |
> | Ação | Microsoft.Web/sites/Deployments/log/Read | Obter o registo de implementações de aplicações Web. |
> | Ação | Microsoft.Web/sites/Deployments/Read | Obter implementações de aplicações Web. |
> | Ação | Microsoft.Web/sites/Deployments/Write | Atualize as implementações de aplicações Web. |
> | Ação | Microsoft.Web/sites/detectors/Read | Obter detetores de aplicações Web. |
> | Ação | microsoft.web/sites/diagnostics/analyses/execute/Action | Execute a análise de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/analyses/Read | Obter a análise de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/aspnetcore/Read | Obter o diagnóstico de aplicações Web da aplicação ASP.NET Core. |
> | Ação | Microsoft.Web/sites/Diagnostics/autoheal/Read | Obter Autoheal de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/Deployment/Read | Obter a implementação de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/Deployments/Read | Obter implementações de diagnóstico de aplicações Web. |
> | Ação | microsoft.web/sites/diagnostics/detectors/execute/Action | Execute Detector de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/detectors/Read | Obter Detector de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/failedrequestsperuri/Read | Obter os pedidos falhados de diagnóstico de aplicações Web por Uri. |
> | Ação | Microsoft.Web/sites/Diagnostics/frebanalysis/Read | Obter a análise FREB de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/loganalyzer/Read | Obter o analisador de registo de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/Read | Obter as categorias de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/runtimeavailability/Read | Obter a disponibilidade de tempo de execução de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/servicehealth/Read | Obter o estado de funcionamento de serviço Web diagnóstico de aplicações. |
> | Ação | Microsoft.Web/sites/Diagnostics/sitecpuanalysis/Read | Obter a análise de CPU de Site de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/sitecrashes/Read | Obter falhas de Site de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/sitelatency/Read | Obter a latência de Site de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/sitememoryanalysis/Read | Obter a análise de memória de Site de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/siterestartsettingupdate/Read | Obter a atualização de definição de reinício de Site diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/siterestartuserinitiated/Read | Obter Web diagnóstico de aplicações Site reinício utilizador iniciado. |
> | Ação | Microsoft.Web/sites/Diagnostics/siteswap/Read | Obter a troca de Site de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/ThreadCount/Read | Obter a contagem de threads de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/workeravailability/Read | Obter Workeravailability de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/Diagnostics/workerprocessrecycle/Read | Obter a Reciclagem de processo de trabalho de diagnóstico de aplicações Web. |
> | Ação | Microsoft.Web/sites/domainownershipidentifiers/Read | Obter os identificadores de propriedade de domínio de aplicações Web. |
> | Ação | Microsoft.Web/sites/domainownershipidentifiers/Write | Atualize identificadores de propriedade de domínio de aplicações Web. |
> | Ação | Microsoft.Web/sites/Functions/Action | Aplicações Web de funções. |
> | Ação | Microsoft.Web/sites/Functions/DELETE | Elimine funções de aplicações Web. |
> | Ação | Microsoft.Web/sites/Functions/listsecrets/Action | Lista os segredos Web Apps funções. |
> | Ação | Microsoft.Web/sites/Functions/masterkey/Read | Obter Masterkey de funções de aplicações Web. |
> | Ação | Microsoft.Web/sites/Functions/Read | Obter as funções de aplicações Web. |
> | Ação | Microsoft.Web/sites/Functions/token/Read | Token de funções do Get Web Apps. |
> | Ação | Microsoft.Web/sites/Functions/Write | Atualize funções de aplicações Web. |
> | Ação | Microsoft.Web/sites/hostnamebindings/DELETE | Elimine enlaces de nome de anfitrião de aplicações Web. |
> | Ação | Microsoft.Web/sites/hostnamebindings/Read | Obter os enlaces de nome de anfitrião de aplicações Web. |
> | Ação | Microsoft.Web/sites/hostnamebindings/Write | Atualize os enlaces de nome de anfitrião de aplicações Web. |
> | Ação | Microsoft.Web/sites/hybridconnection/DELETE | Elimine a ligação de híbrida de aplicações Web. |
> | Ação | Microsoft.Web/sites/hybridconnection/Read | Obter da ligação híbrida de aplicações Web. |
> | Ação | Microsoft.Web/sites/hybridconnection/Write | Atualize da ligação híbrida de aplicações Web. |
> | Ação | Microsoft.Web/sites/hybridconnectionnamespaces/relays/DELETE | Elimine reencaminhamentos de espaços de nomes de ligação de híbrida de aplicações Web. |
> | Ação | Microsoft.Web/sites/hybridconnectionnamespaces/relays/listkeys/Action | Lista as chaves Web aplicações híbridas ligação reencaminhamentos espaços de nomes. |
> | Ação | Microsoft.Web/sites/hybridconnectionnamespaces/relays/Read | Obter reencaminhamentos de espaços de nomes de ligação de híbrida de aplicações Web. |
> | Ação | Microsoft.Web/sites/hybridconnectionnamespaces/relays/Write | Atualize reencaminhamentos de espaços de nomes de ligação de híbrida de aplicações Web. |
> | Ação | Microsoft.Web/sites/hybridconnectionrelays/Read | Obter reencaminhamentos de ligação de híbrida de aplicações Web. |
> | Ação | Microsoft.Web/sites/instances/Deployments/DELETE | Elimine as implementações de instâncias de aplicações Web. |
> | Ação | Microsoft.Web/sites/instances/Deployments/Read | Obter implementações de instâncias de aplicações Web. |
> | Ação | Microsoft.Web/sites/instances/Extensions/log/Read | Obter o registo de extensões de instâncias de aplicações Web. |
> | Ação | Microsoft.Web/sites/instances/Extensions/Read | Obter as extensões de instâncias de aplicações Web. |
> | Ação | Microsoft.Web/sites/instances/processes/DELETE | Elimine processos de instâncias de aplicações Web. |
> | Ação | Microsoft.Web/sites/instances/processes/Read | Obter os processos de instâncias de aplicações Web. |
> | Ação | Microsoft.Web/sites/instances/Read | Obter instâncias de aplicações Web. |
> | Ação | Microsoft.Web/sites/listsyncfunctiontriggerstatus/Action | Lista sincronização função acionar Estado as Web Apps. |
> | Ação | Microsoft.Web/sites/metricdefinitions/Read | Obter definições de métrica de aplicações Web. |
> | Ação | Microsoft.Web/sites/Metrics/Read | Obter métricas de aplicações Web. |
> | Ação | Microsoft.Web/sites/metricsdefinitions/Read | Obter definições de métrica de aplicações Web. |
> | Ação | Microsoft.Web/sites/migratemysql/Action | Migre as aplicações MySql Web. |
> | Ação | Microsoft.Web/sites/migratemysql/Read | Introdução a Web Apps migrar MySql. |
> | Ação | Microsoft.Web/sites/networktrace/Action | Aplicações de Web de rastreio de rede. |
> | Ação | Microsoft.Web/sites/newpassword/Action | Aplicações newpassword Web. |
> | Ação | Microsoft.Web/sites/operationresults/Read | Obter os resultados da operação aplicações Web. |
> | Ação | Microsoft.Web/sites/Operations/Read | Obter operações de aplicações Web. |
> | Ação | Microsoft.Web/sites/perfcounters/Read | Obter os contadores de desempenho de aplicações Web. |
> | Ação | Microsoft.Web/sites/premieraddons/DELETE | Elimine Addons de Premier de aplicações Web. |
> | Ação | Microsoft.Web/sites/premieraddons/Read | Obter Addons de Premier de aplicações Web. |
> | Ação | Microsoft.Web/sites/premieraddons/Write | Atualize Addons de Premier de aplicações Web. |
> | Ação | Microsoft.Web/sites/processes/Read | Obter os processos de aplicações Web. |
> | Ação | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | microsoft.web/sites/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para a aplicação Web |
> | Ação | Microsoft.Web/sites/providers/Microsoft.Insights/metricDefinitions/Read | Obtem as métricas disponíveis para a aplicação Web |
> | Ação | Microsoft.Web/sites/publiccertificates/DELETE | Elimine certificados de público de aplicações Web. |
> | Ação | Microsoft.Web/sites/publiccertificates/Read | Obter certificados de público de aplicações Web. |
> | Ação | Microsoft.Web/sites/publiccertificates/Write | Atualize certificados de público de aplicações Web. |
> | Ação | Microsoft.Web/sites/publish/Action | Publicar uma aplicação Web |
> | Ação | Microsoft.Web/sites/publishxml/Action | Obtenha o xml do perfil de publicação para uma aplicação Web |
> | Ação | Microsoft.Web/sites/publishxml/Read | Obterem aplicações Web publicar XML. |
> | Ação | Microsoft.Web/sites/Read | Obter as propriedades de uma aplicação Web |
> | Ação | Microsoft.Web/sites/recommendationhistory/Read | Obter o histórico de recomendação de aplicações Web. |
> | Ação | Microsoft.Web/sites/recommendations/disable/Action | Desative as recomendações de aplicações Web. |
> | Ação | Microsoft.Web/sites/recommendations/Read | Obter a lista de recomendações para a aplicação web. |
> | Ação | Microsoft.Web/sites/RECOVER/Action | Recupere as Web Apps. |
> | Ação | Microsoft.Web/sites/resetSlotConfig/Action | Repor a configuração de aplicação web |
> | Ação | Microsoft.Web/sites/resourcehealthmetadata/Read | Obter os metadados do Estado de funcionamento de recursos de aplicações Web. |
> | Ação | Microsoft.Web/sites/restart/Action | Reiniciar uma aplicação Web |
> | Ação | Microsoft.Web/sites/Restore/Read | Obter o restauro de aplicações Web. |
> | Ação | Microsoft.Web/sites/Restore/Write | Restaure aplicações Web. |
> | Ação | Microsoft.Web/sites/restorefromdeletedwebapp/Action | Restaure aplicações Web da aplicação foi eliminada. |
> | Ação | Microsoft.Web/sites/restoresnapshot/Action | Restaure instantâneos de aplicações Web. |
> | Ação | Microsoft.Web/sites/siteextensions/DELETE | Elimine extensões de Site de aplicações Web. |
> | Ação | Microsoft.Web/sites/siteextensions/Read | Obter as extensões de Site de aplicações Web. |
> | Ação | Microsoft.Web/sites/siteextensions/Write | Atualize extensões de Site de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/analyzecustomhostname/Read | Introdução a Web Apps ranhuras analisam o nome do anfitrião personalizado. |
> | Ação | Microsoft.Web/sites/slots/applySlotConfig/Action | Aplica a configuração de ranhura de aplicação do web a partir da ranhura de destino para a ranhura atual. |
> | Ação | Microsoft.Web/sites/slots/backup/Action | Crie nova cópia de segurança da ranhura de aplicação Web. |
> | Ação | Microsoft.Web/sites/slots/backup/Read | Obter a cópia de segurança de ranhuras de aplicações do Web. |
> | Ação | Microsoft.Web/sites/slots/backup/Write | Atualize a cópia de segurança de ranhuras de aplicações de Web. |
> | Ação | Microsoft.Web/sites/slots/backups/DELETE | Elimine as cópias de segurança do Web Apps ranhuras. |
> | Ação | Microsoft.Web/sites/slots/backups/List/Action | Cópias de segurança da ranhuras de aplicações do Web de lista. |
> | Ação | Microsoft.Web/sites/slots/backups/Read | Obter as propriedades de cópia de segurança dos ranhuras de aplicação um web |
> | Ação | Microsoft.Web/sites/slots/backups/Restore/Action | Restaure cópias de segurança de ranhuras de aplicações de Web. |
> | Ação | Microsoft.Web/sites/slots/config/DELETE | Elimine a configuração de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/config/list/Action | Lista confidenciais definições de segurança da ranhura de aplicação Web, tais como publicar as credenciais, as definições de aplicação e as cadeias de ligação |
> | Ação | Microsoft.Web/sites/slots/config/Read | Obter as definições de configuração da ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/config/Write | Atualizar as definições de configuração da ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/continuouswebjobs/DELETE | Elimine tarefas de Web contínua de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/continuouswebjobs/Read | Obter tarefas de Web contínua de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/continuouswebjobs/Start/Action | Inicie tarefas de Web contínua de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/continuouswebjobs/Stop/Action | Pare tarefas de Web contínua de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Delete | Eliminar uma ranhura de aplicação Web existente |
> | Ação | Microsoft.Web/sites/slots/Deployments/DELETE | Elimine as implementações de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Deployments/log/Read | Obter o registo de implementações de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Deployments/Read | Obter implementações de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Deployments/Write | Atualize as implementações de ranhuras de aplicações Web. |
> | Ação | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Execute a análise de diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/analyses/Read | Obter a análise de diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/aspnetcore/Read | Obter o diagnóstico de ranhuras de aplicações Web para a aplicação ASP.NET Core. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/autoheal/Read | Obter Autoheal de diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/Deployment/Read | Obter a implementação de diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/Deployments/Read | Obter implementações de diagnóstico de ranhuras de aplicações Web. |
> | Ação | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Execute Detector de diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/detectors/Read | Obter Detector de diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/frebanalysis/Read | Obter Web Apps ranhuras diagnóstico FREB Analysis Services. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/loganalyzer/Read | Obter o analisador de registo de diagnóstico de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/Read | Obter o diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/runtimeavailability/Read | Obter a disponibilidade de tempo de execução de diagnóstico de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/servicehealth/Read | Obter o estado de funcionamento do Web Apps ranhuras diagnóstico serviço. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/sitecpuanalysis/Read | Obter Web Apps ranhuras diagnóstico Site CPU Analysis Services. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/sitecrashes/Read | Obter Web Apps ranhuras diagnóstico Site falhas. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/sitelatency/Read | Obter a latência de Site de diagnóstico de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/sitememoryanalysis/Read | Obter a análise de memória de Site de diagnóstico ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/siterestartsettingupdate/Read | Obter as ranhuras de aplicações de Web diagnóstico atualização de definição de reinício de Site. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/siterestartuserinitiated/Read | Obter Web Apps ranhuras diagnóstico Site reinício iniciada pelo utilizador. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/siteswap/Read | Obter a troca de Site de diagnóstico de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/ThreadCount/Read | Obter a contagem de threads de diagnóstico de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/workeravailability/Read | Obter Workeravailability de diagnóstico de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Diagnostics/workerprocessrecycle/Read | Obter os Reciclagem do processo de trabalho do diagnóstico de ranhuras do Web Apps. |
> | Ação | Microsoft.Web/sites/slots/domainownershipidentifiers/Read | Obter os identificadores de propriedade de domínio de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/hostnamebindings/DELETE | Elimine enlaces de nome de anfitrião de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/hostnamebindings/Read | Obter os enlaces de nome de anfitrião de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/hostnamebindings/Write | Atualize os enlaces de nome de anfitrião de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/hybridconnection/DELETE | Elimine a ligação de híbrida de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/hybridconnection/Read | Obter aplicações da Web ranhuras ligação híbrida. |
> | Ação | Microsoft.Web/sites/slots/hybridconnection/Write | Atualize ligação de híbrida de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/hybridconnectionnamespaces/relays/DELETE | Elimine reencaminhamentos de espaços de nomes de ligação ranhuras híbrida Web Apps. |
> | Ação | Microsoft.Web/sites/slots/hybridconnectionnamespaces/relays/Write | Atualize reencaminhamentos de espaços de nomes de ligação ranhuras híbrida Web Apps. |
> | Ação | Microsoft.Web/sites/slots/hybridconnectionrelays/Read | Obter Web Apps ranhuras híbrida ligação reencaminhamentos. |
> | Ação | Microsoft.Web/sites/slots/instances/Deployments/Read | Obter implementações de instâncias de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/instances/processes/DELETE | Elimine processos de instâncias de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/instances/processes/Read | Obter os processos de instâncias de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/instances/Read | Obter instâncias de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/metricdefinitions/Read | Obter definições de métrica de ranhuras de aplicações de Web. |
> | Ação | Microsoft.Web/sites/slots/Metrics/Read | Obter métricas de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/migratemysql/Read | Introdução a Web Apps ranhuras migrar MySql. |
> | Ação | Microsoft.Web/sites/slots/networktrace/Action | Ranhuras de aplicações de Web de rastreio de rede. |
> | Ação | Microsoft.Web/sites/slots/newpassword/Action | Ranhuras de aplicações newpassword Web. |
> | Ação | Microsoft.Web/sites/slots/operationresults/Read | Obter resultados de operação de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Operations/Read | Obter operações de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/perfcounters/Read | Obter os contadores de desempenho de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/phplogging/Read | Obter Phplogging de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/premieraddons/DELETE | Elimine Addons de Premier de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/premieraddons/Read | Obter Addons de Premier de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/premieraddons/Write | Atualize Addons de Premier de ranhuras de aplicações Web. |
> | Ação | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/read | Obtém a definição de diagnóstico do recurso |
> | Ação | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/write | Cria ou atualiza a definição do diagnóstico para o recurso |
> | Ação | microsoft.web/sites/slots/providers/Microsoft.Insights/logDefinitions/read | Obtém os registos disponíveis para as ranhuras de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read | Obtem as métricas disponíveis para a ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/publiccertificates/DELETE | Elimine certificados público de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/publiccertificates/Read | Obter certificados de Web Apps ranhuras públicos. |
> | Ação | Microsoft.Web/sites/slots/publiccertificates/Write | Criar ou atualizar certificados de Web Apps ranhuras públicos. |
> | Ação | Microsoft.Web/sites/slots/publish/Action | Publicar uma ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/publishxml/Action | Obtenha o xml do perfil de publicação para a ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/Read | Obter as propriedades de uma ranhura de implementação de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/RECOVER/Action | Recupere ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/resetSlotConfig/Action | Repor a configuração de ranhura de aplicação web |
> | Ação | Microsoft.Web/sites/slots/resourcehealthmetadata/Read | Obter metadados de estado de funcionamento de recursos de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/restart/Action | Reiniciar uma ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/Restore/Read | Obter o restauro de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Restore/Write | Restaure ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/restorefromdeletedwebapp/Action | Restaure ranhuras de aplicação Web da aplicação foi eliminada. |
> | Ação | Microsoft.Web/sites/slots/restoresnapshot/Action | Restaure instantâneos de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/siteextensions/DELETE | Elimine extensões de Site de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/siteextensions/Read | Obter as extensões de Site de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/siteextensions/Write | Atualize extensões de Site de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/slotsdiffs/Action | Obter diferenças na configuração de aplicação web e de ranhuras |
> | Ação | Microsoft.Web/sites/slots/slotsswap/Action | Trocar ranhuras de implementação de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/snapshots/Read | Obter instantâneos de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/sourcecontrols/Delete | Eliminar definições de configuração de controlo de origem da ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/sourcecontrols/Read | Obter as definições de configuração de controlo de origem da ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/sourcecontrols/Write | Atualizar definições de configuração de controlo de origem da ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/start/Action | Iniciar uma ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/stop/Action | Parar uma ranhura de aplicação Web |
> | Ação | Microsoft.Web/sites/slots/Sync/Action | Ranhuras de aplicações Web de sincronização. |
> | Ação | Microsoft.Web/sites/slots/triggeredwebjobs/DELETE | Elimine as Web Apps ranhuras accionadas WebJobs. |
> | Ação | Microsoft.Web/sites/slots/triggeredwebjobs/Read | Obter Web Apps ranhuras accionadas WebJobs. |
> | Ação | Microsoft.Web/sites/slots/triggeredwebjobs/Run/Action | Execute Web Apps ranhuras accionadas WebJobs. |
> | Ação | Microsoft.Web/sites/slots/usages/Read | Obter as utilizações de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/virtualnetworkconnections/DELETE | Elimine ligações de rede Virtual de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/virtualnetworkconnections/gateways/Write | Atualize os Gateways de ligações de rede Virtual de ranhuras aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/virtualnetworkconnections/Read | Obter ligações de rede Virtual de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/virtualnetworkconnections/Write | Atualize ligações de rede Virtual de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/webjobs/Read | Obter WebJobs de ranhuras de aplicações Web. |
> | Ação | Microsoft.Web/sites/slots/Write | Criar uma nova ranhura de aplicação Web ou atualizar um já existente |
> | Ação | Microsoft.Web/sites/slotsdiffs/Action | Obter diferenças na configuração de aplicação web e de ranhuras |
> | Ação | Microsoft.Web/sites/slotsswap/Action | Trocar ranhuras de implementação de aplicação Web |
> | Ação | Microsoft.Web/sites/snapshots/Read | Obter instantâneos de aplicações Web. |
> | Ação | Microsoft.Web/sites/sourcecontrols/Delete | Eliminar definições de configuração de controlo de origem da aplicação Web |
> | Ação | Microsoft.Web/sites/sourcecontrols/Read | Obter as definições de configuração de controlo de origem da aplicação Web |
> | Ação | Microsoft.Web/sites/sourcecontrols/Write | Atualizar definições de configuração de controlo de origem da aplicação Web |
> | Ação | Microsoft.Web/sites/start/Action | Iniciar uma aplicação Web |
> | Ação | Microsoft.Web/sites/stop/Action | Parar uma aplicação Web |
> | Ação | Microsoft.Web/sites/Sync/Action | Aplicações Web de sincronização. |
> | Ação | Microsoft.Web/sites/syncfunctiontriggers/Action | Acionadores de função de sincronização para aplicações Web. |
> | Ação | Microsoft.Web/sites/triggeredwebjobs/DELETE | Elimine WebJobs accionadas de aplicações Web. |
> | Ação | Microsoft.Web/sites/triggeredwebjobs/History/Read | Obter o histórico de WebJobs accionadas aplicações Web. |
> | Ação | Microsoft.Web/sites/triggeredwebjobs/Read | Obter WebJobs accionadas de aplicações Web. |
> | Ação | Microsoft.Web/sites/triggeredwebjobs/Run/Action | Execute Web Apps accionadas WebJobs. |
> | Ação | Microsoft.Web/sites/usages/Read | Obter as utilizações de aplicações Web. |
> | Ação | Microsoft.Web/sites/virtualnetworkconnections/DELETE | Elimine ligações de rede Virtual de aplicações Web. |
> | Ação | Microsoft.Web/sites/virtualnetworkconnections/gateways/Read | Obter os Gateways de ligações de rede Virtual de aplicações Web. |
> | Ação | Microsoft.Web/sites/virtualnetworkconnections/gateways/Write | Atualize os Gateways de ligações de rede Virtual de aplicações Web. |
> | Ação | Microsoft.Web/sites/virtualnetworkconnections/Read | Obter ligações de rede Virtual de aplicações Web. |
> | Ação | Microsoft.Web/sites/virtualnetworkconnections/Write | Atualize ligações de rede Virtual de aplicações Web. |
> | Ação | Microsoft.Web/sites/webjobs/Read | Obter WebJobs de aplicações Web. |
> | Ação | Microsoft.Web/sites/Write | Criar uma nova aplicação Web ou atualizar um já existente |
> | Ação | Microsoft.Web/SKUs/Read | Obter SKUs. |
> | Ação | Microsoft.Web/sourcecontrols/Read | Obter controlos de origem. |
> | Ação | Microsoft.Web/sourcecontrols/Write | Controlos de origem de atualização. |
> | Ação | Microsoft.Web/unregister/Action | Anular o registo do fornecedor de recursos Microsoft. Web para a subscrição. |
> | Ação | Microsoft.Web/Validate/Action | Valide. |
> | Ação | Microsoft.Web/verifyhostingenvironmentvnet/Action | Certifique-se de Vnet do ambiente de alojamento. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Tipo de Acção | Operação | Descrição |
> | --- | --- | --- |
> | Ação | Microsoft.WorkloadMonitor/components/read | Recursos de operações de leitura |
> | Ação | Microsoft.WorkloadMonitor/healthInstances/read | Recursos de operações de leitura |
> | Ação | Microsoft.WorkloadMonitor/Operations/read | Recursos de operações de leitura |
> | Ação | Microsoft.WorkloadMonitor/workloads/delete | Elimina um recurso de carga de trabalho |
> | Ação | Microsoft.WorkloadMonitor/workloads/read | Lê um recurso de carga de trabalho |
> | Ação | Microsoft.WorkloadMonitor/workloads/write | Escreve um recurso de carga de trabalho |

## <a name="next-steps"></a>Passos Seguintes

- [Funções personalizadas](custom-roles.md)
- [Funções incorporadas](built-in-roles.md)
