---
title: Operações de fornecedor do Gestor de recursos do Azure | Microsoft Docs
description: Fornece detalhes sobre as operações disponíveis nos fornecedores de recursos do Microsoft Azure Resource Manager
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.openlocfilehash: 0b8c8823c6d21df96dcfd926db1855169f1570e4
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/11/2018
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Operações de fornecedor de recursos do Gestor de recursos do Azure

Este documento, lista as operações disponíveis para cada fornecedor de recursos do Microsoft Azure Resource Manager. Estes podem ser utilizados em funções personalizadas para fornecer permissões de controlo de acesso baseado em funções (RBAC) granulares a recursos no Azure. Tenha em atenção de que não se trata de uma lista completa e operações podem ser adicionadas ou removidas como cada fornecedor está atualizado. Cadeias de operação seguem o formato do `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. 

> [!NOTE]
> Para obter uma lista abrangente e atual utilize `Get-AzureRmProviderOperation` (no PowerShell) ou `az provider operation list` (na CLI do Azure v2) para operações de lista de fornecedores de recursos do Azure.

## <a name="microsoftaad"></a>Microsoft.AAD

| Operação | Descrição |
|---|---|
|/domainServices/delete|Elimina os serviços de domínio.|
|/domainServices/read|Lê os serviços de domínio.|
|/domainServices/write|Escrever os serviços de domínio|
|/locations/operationresults/read|Ler o estado de uma operação assíncrona.|
|/Operations/read|A descrição amigável localizada para a operação, dado que deve ser apresentada ao utilizador.|

## <a name="microsoftaadiam"></a>microsoft.aadiam

| Operação | Descrição |
|---|---|
|/diagnosticsettings/delete|Eliminar uma definição de diagnóstico|
|/diagnosticsettings/read|Ler a definição de diagnóstico|
|/diagnosticsettings/write|Escrever uma definição de diagnóstico|
|/diagnosticsettingscategories/read|Ler um categorias de definição de diagnóstico|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/tenants/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para os inquilinos|

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Operação | Descrição |
|---|---|
|/configuration/action|Configuração de inquilinos de atualizações.|
|/configuration/read|Lê a configuração de inquilino.|
|/configuration/write|Cria uma configuração de inquilino.|
|/services/action|As atualizações de uma instância de serviço no inquilino.|
|/services/alerts/read|Lê os alertas para um serviço.|
|/services/alerts/read|Lê os alertas para um serviço.|
|/services/delete|Elimina uma instância de serviço no inquilino.|
|/services/read|Lê as instâncias do serviço no inquilino.|
|/services/servicemembers/action|Cria uma instância de membro de serviço no serviço.|
|/services/servicemembers/alerts/read|Lê os alertas para um membro de serviço.|
|/services/servicemembers/delete|Elimina uma instância de membro de serviço no serviço.|
|/services/servicemembers/read|Lê a instância de membro de serviço no serviço.|
|/services/write|Cria uma instância de serviço no inquilino.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Operação | Descrição |
|---|---|
|/configurations/read|Obter configurações|
|/configurations/write|Configuração de cria/atualizações|
|/generateRecommendations/action|Gera recomendações|
|/generateRecommendations/read|Obtém gerar Estado recomendações|
|/operations/read|Obtém as operações do Microsoft Advisor|
|/recommendations/read|Recomendações de leituras|
|/recommendations/suppressions/delete|Elimina a supressão|
|/recommendations/suppressions/read|Obtém suppressions|
|/recommendations/suppressions/write|Cria/atualizações suppressions|
|/register/action|Regista a subscrição para a Microsoft Advisor|
|/suppressions/delete|Elimina a supressão|
|/suppressions/read|Obtém suppressions|
|/suppressions/write|Cria/atualizações suppressions|
|/unregister/action|Anula o registo da subscrição para a Microsoft Advisor|

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

| Operação | Descrição |
|---|---|
|/alerts/read|Obter todos os alertas para os filtros de entrada.|
|/alerts/resolve/action|Altere o estado do alerta para 'Resolva'|
|/alertsSummary/read|Obter o resumo dos alertas|
|/Operations/read|Lê as operações fornecidas|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Operação | Descrição |
|---|---|
|/locations/checkNameAvailability/action|Verificações de que nome de servidor Analysis Services é válido e não em utilização.|
|/locations/operationresults/read|Obtém as informações do resultado da operação especificada.|
|/locations/operationstatuses/read|Obtém as informações do Estado da operação especificada.|
|/operations/read|Obtém as informações de operações|
|/register/action|Regista o fornecedor de recursos do Analysis Services.|
|/servers/delete|Elimina o servidor de Analysis Services.|
|/servers/listGatewayStatus/action|Lista o estado do gateway associado ao servidor.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico para o servidor de Analysis Services|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o servidor de Analysis Services|
|/servers/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para servidores|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o servidor de Analysis Services|
|/servers/read|Obtém as informações do servidor de Analysis Services especificada.|
|/servers/resume/action|Retoma o Analysis Server.|
|/servers/skus/read|Obter informações de SKU disponíveis para o servidor|
|/servers/suspend/action|Suspende o Analysis Server.|
|/servers/write|Cria ou atualiza o servidor de Analysis Services especificada.|
|/skus/read|Obtém as informações de Skus|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Operação | Descrição |
|---|---|
|/checkNameAvailability/read|Verifica se o nome de serviço está disponível|
|/operations/read|Ler todas as operações API disponíveis para o recurso de Microsoft.ApiManagement|
|/register/action|Registar a subscrição para o fornecedor de recursos de Microsoft.ApiManagement|
|/reports/read|Obter relatórios agregados pelo períodos de tempo, região geográfica, os programadores, produtos, APIs, operações, subscrição e byRequest.|
|/service/apis/delete|Remover API existente|
|/service/apis/diagnostics/delete|Remova o diagnóstico existente|
|/service/apis/diagnostics/loggers/delete|Remova o mapeamento de um registo com uma definição de diagnóstico|
|/service/apis/diagnostics/loggers/read|Obter a lista de registadores diagnóstico existentes|
|/service/apis/diagnostics/loggers/write|Mapear registador a uma definição de diagnóstico|
|/service/apis/diagnostics/read|Obter a lista de diagnóstico ou Obtenha detalhes do diagnóstico|
|/service/apis/diagnostics/write|Adicionar novo diagnóstico ou atualizar detalhes do diagnóstico existentes|
|/service/apis/operations/delete|A operação de API existente de remoção|
|/service/apis/operations/policies/delete|Remover configuração da política de políticas de operação de API|
|/service/apis/operations/policies/read|Obter as políticas para a operação de API ou Obtenha detalhes de configuração de política para a operação de API|
|/service/apis/operations/policies/write|Definir os detalhes de configuração de política para a operação de API|
|/service/apis/operations/policy/delete|Remover configuração da política de operação|
|/service/apis/operations/policy/read|Obter os detalhes de configuração de política para a operação|
|/service/apis/operations/policy/write|Definir os detalhes da configuração de política para a operação|
|/service/apis/operations/read|Obter a lista de operações de API existentes ou obter os detalhes da operação de API|
|/service/apis/operations/tags/delete|Eliminar a associação da Tag existente com a operação existente|
|/service/apis/operations/tags/read|Obter etiquetas associadas aos detalhes de operação ou obter etiqueta|
|/service/apis/operations/tags/write|Associar etiqueta existente com a operação existente|
|/service/apis/operations/write|Criar nova operação da API ou atualizar a operação de API existente|
|/service/apis/operationsByTags/read|Obter a lista de associações de Tag/operação|
|/service/apis/policies/delete|Remover configuração da política de políticas de API|
|/service/apis/policies/read|Obter as políticas de API ou Get detalhes de configuração de políticas de API|
|/service/apis/policies/write|Definir os detalhes de configuração de política de API|
|/service/apis/policy/delete|Remover configuração da política da API|
|/service/apis/policy/read|Obter os detalhes de configuração de políticas de API|
|/service/apis/policy/write|Definir os detalhes de configuração de política de API|
|/service/apis/products/read|Obter todos os produtos que a API faz parte|
|/service/apis/read|Obter a lista de todos os registado APIs ou Get detalhes da API|
|/service/apis/releases/delete|Remove todas as versões da versão de API ou remover API|
|/service/apis/releases/read|Obter versões de uma API ou Get. Detalhes da API reelase|
|/service/apis/releases/write|Criar nova versão de API ou atualizar a versão de API existente|
|/service/apis/revisions/delete|Remove todas as revisões de uma API|
|/service/apis/revisions/read|Obter revisões que pertencem a uma API|
|/service/apis/schemas/delete|Remove existente esquema|
|/service/apis/schemas/document/read|Obter o documento que descreve o esquema|
|/service/apis/schemas/document/write|Atualizar o documento que descreve o esquema|
|/service/apis/schemas/read|Obtém todos os esquemas de uma determinada API ou obtém os esquemas utilizados pela API|
|/service/apis/schemas/write|Define os esquemas utilizados pela API|
|/service/apis/tagDescriptions/delete|Remover a descrição da etiqueta da API|
|/service/apis/tagDescriptions/read|Obter as descrições de etiquetas no âmbito da API ou da Tag de obter descrição no âmbito da API|
|/service/apis/tagDescriptions/write|Descrição da etiqueta de criar/alterar no âmbito da API|
|/service/apis/tags/delete|Remova a associação de API/etiqueta existente|
|/service/apis/tags/read|Obter todas as API/Tag associação para os detalhes de API ou Get da associação de API/etiqueta|
|/service/apis/tags/write|Adicionar nova associação de API/etiqueta|
|/service/apis/write|Criar nova API ou atualizar os detalhes de API existentes|
|/service/apisByTags/read|Obter a lista de associações/etiqueta de API|
|/service/api-version-sets/delete|Remover VersionSet existente|
|/service/api-version-sets/read|Obter a lista de entidades do grupo de versão ou obtém detalhes de um VersionSet|
|/service/api-version-sets/versions/read|Obter a lista de entidades de versão|
|/service/api-version-sets/write|Criar novo VersionSet ou atualizar detalhes do VersionSet existentes|
|/service/applynetworkconfigurationupdates/action|Atualiza os recursos de Microsoft.ApiManagement em execução na rede Virtual para escolher as definições de rede atualizado.|
|/service/authorizationServers/delete|Remover o servidor de autorização existente|
|/service/authorizationServers/read|Obter a lista de servidores de autorização ou obter os detalhes do servidor de autorização|
|/service/authorizationServers/write|Criar um novo servidor de autorização ou detalhes da atualização de um servidor de autorização existente|
|/service/backends/delete|Remova o back-end existente|
|/service/backends/read|Obter a lista de back-ends ou obter os detalhes de back-end|
|/service/backends/reconnect/action|Criar um pedido de restabelecimento de ligação|
|/service/backends/write|Adicionar um novo back-end ou atualizar detalhes do back-end existente|
|/Service/backup/Action|Serviço de gestão de API a cópia de segurança para o contentor especificado um utilizador forneceu a conta de armazenamento|
|/service/certificates/delete|Remover o certificado existente|
|/service/certificates/read|Obter a lista de certificados ou obter os detalhes do certificado|
|/service/certificates/write|Adicionar novo certificado|
|/service/delete|Eliminar a instância de serviço de gestão de API|
|/service/diagnostics/delete|Remova o diagnóstico existente|
|/service/diagnostics/loggers/delete|Remova o mapeamento de um registo com uma definição de diagnóstico|
|/service/diagnostics/loggers/read|Obter a lista de registadores diagnóstico existentes|
|/service/diagnostics/loggers/write|Mapear registador a uma definição de diagnóstico|
|/service/diagnostics/read|Obter a lista de diagnóstico ou Obtenha detalhes do diagnóstico|
|/service/diagnostics/write|Adicionar novo diagnóstico ou atualizar detalhes do diagnóstico existentes|
|/service/getssotoken/action|Token SSO obtém que pode ser utilizada para iniciar sessão no portal de legado de serviço de gestão de API como administrador|
|/service/groups/delete|Remover grupo existente|
|/service/groups/read|Obter a lista de grupos ou obtém detalhes de um grupo|
|/service/groups/users/delete|Remover utilizador existente do grupo existente|
|/service/groups/users/read|Obter a lista de utilizadores do grupo|
|/service/groups/users/write|Adicionar utilizador existente ao grupo existente|
|/service/groups/write|Criar novo grupo ou atualizar detalhes do grupo existente|
|/service/identityProviders/delete|Remover o fornecedor de identidade existentes|
|/service/identityProviders/read|Obter a lista de fornecedores de identidade ou Obtenha detalhes do fornecedor de identidade|
|/service/identityProviders/write|Criar um novo fornecedor de identidade ou atualizar os detalhes de um fornecedor de identidade existentes|
|/service/locations/networkstatus/read|Obtém o estado de acesso de rede de recursos no qual o serviço depende de na localização.|
|/service/loggers/delete|Remover o registo existente|
|/service/loggers/read|Obter a lista de registadores ou obter os detalhes de registo|
|/service/loggers/write|Adicionar novo registador ou atualizar detalhes do registo existente|
|/service/managedeployments/action|Alterar o SKU/unidades, adicionar/remover implementações regional do serviço de gestão de API|
|/service/networkstatus/read|Obtém o estado de acesso de rede de recursos no qual o serviço depende.|
|/service/notifications/action|Envia a notificação para um utilizador especificado|
|/service/notifications/read|Obtém todas as notificações de publicador da API Management ou Editor de gestão de API de obter os detalhes de notificação|
|/service/notifications/recipientEmails/delete|Remove existente associado uma notificação de E-Mail|
|/service/notifications/recipientEmails/read|Obter os destinatários de E-mail associado à notificação de editor de gestão de API|
|/service/notifications/recipientEmails/write|Criar novo E-Mail destinatário da notificação|
|/service/notifications/recipientUsers/delete|Remove o utilizador associado para os destinatários de notificação|
|/service/notifications/recipientUsers/read|Obter os utilizadores de destinatário associados a notificação|
|/service/notifications/recipientUsers/write|Adicionar o utilizador para os destinatários de notificação|
|/service/notifications/write|Criar ou notificação de editor de gestão de API de atualização|
|/service/openidConnectProviders/delete|Remover existente OpenID Connect fornecedor|
|/service/openidConnectProviders/read|Obter a lista de fornecedores de OpenID Connect ou Obtenha detalhes de OpenID Connect fornecedor|
|/service/openidConnectProviders/write|Criar um novo OpenID Connect fornecedor ou atualizar os detalhes de um fornecedor de ligação de OpenID existente|
|/service/operationresults/read|Obtém o estado atual da operação de execução longa|
|/service/policies/delete|Remover configuração da política de políticas de inquilino|
|/service/policies/read|Obter as políticas para detalhes de configuração de política do inquilino ou Get para o Tenant|
|/service/policies/write|Definir os detalhes de configuração de política para o Tenant|
|/service/policySnippets/read|Obter todos os fragmentos de política|
|/service/portalsettings/read|Obter as definições de inscrição para o Portal ou Get iniciar sessão nas definições para o Portal ou obter definições de delegação para o Portal|
|/service/portalsettings/write|Atualizar as definições de segurança de início de sessão ou as definições de atualização inscrever-se ou as definições de atualização de sessão ou as definições de atualização de sessão ou definições de delegação de atualização ou definições de delegação de atualização|
|/service/products/apis/delete|Remover API existente de produto existente|
|/service/products/apis/read|Obter a lista de APIs adicionadas ao produto existente|
|/service/products/apis/write|Adicionar API existente ao produto existente|
|/service/products/delete|Remova o produto existente|
|/service/products/groups/delete|Eliminar a associação de grupo de programador existente com o produto existente|
|/service/products/groups/read|Obter lista de grupos de programador associados ao produto|
|/service/products/groups/write|Associar grupo existente do Programador de produto existente|
|/service/products/policies/delete|Remover configuração da política de políticas de produto|
|/service/products/policies/read|Obter as políticas para detalhes de configuração de política do produto ou Get para o produto|
|/service/products/policies/write|Definir os detalhes de configuração de política para o produto|
|/service/products/policy/delete|Remover configuração da política de produto existente|
|/service/products/policy/read|Obter a configuração da política de produto existente|
|/service/products/policy/write|Definir a configuração de política para o produto existente|
|/service/products/read|Obter a lista de produtos ou obter os detalhes do produto|
|/service/products/subscriptions/read|Obter a lista de subscrições do produto|
|/service/products/tags/delete|Eliminar a associação de uma etiqueta existente com o produto existente|
|/service/products/tags/read|Obter etiquetas associadas aos detalhes do produto ou obter etiqueta|
|/service/products/tags/write|Associar etiqueta existente de produto existente|
|/service/products/write|Criar novo produto ou atualizar detalhes do produto existente|
|/service/properties/delete|Remove existente propriedade|
|/service/properties/read|Obtém a lista de todas as propriedades ou obtém os detalhes de propriedade especificado|
|/service/properties/write|Cria uma nova propriedade ou atualiza o valor para a propriedade especificada|
|/service/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico para o serviço de API Management|
|/service/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o serviço de API Management|
|/service/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para o serviço de API Management|
|/service/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o serviço de API Management|
|/service/quotas/periods/read|Obter o valor do contador de quota para período|
|/service/quotas/periods/write|Defina o valor atual do contador de quota|
|/service/quotas/read|Obter os valores de quota|
|/service/quotas/write|Defina o valor atual do contador de quota|
|/service/read|Ler os metadados para uma instância de serviço de gestão de API|
|/service/reports/read|Obter o relatório agregado pelo períodos de tempo ou relatório Get agregados por região geográfica ou relatório Get agregado pelos programadores. ou obter relatório agregado pelo produtos. ou obter relatório agregado pelo APIs ou Get relatório agregado pelo operações ou relatório Get agregados por subscrição. ou obter pedidos de dados de relatórios|
|/service/restore/action|Restaurar o serviço de gestão de API a partir do contentor especificado um utilizador fornecida a conta de armazenamento|
|/service/subscriptions/delete|Elimine a subscrição. Esta operação pode ser utilizada para eliminar a subscrição|
|/service/subscriptions/read|Obter uma lista de subscrições do produto ou obter os detalhes da subscrição do produto|
|/service/subscriptions/regeneratePrimaryKey/action|Regenerar a chave primária da subscrição|
|/service/subscriptions/regenerateSecondaryKey/action|Regenerar a chave secundária da subscrição|
|/service/subscriptions/write|Subscrever um utilizador existente para um produto existente ou atualizar os detalhes da subscrição existente. Esta operação pode ser utilizada para renovar a subscrição|
|/service/tagResources/read|Obter a lista de etiquetas aos recursos associados|
|/service/tags/delete|Remover etiqueta existente|
|/service/tags/read|Obter a lista de etiquetas ou obter detalhes da etiqueta|
|/service/tags/write|Adicionar nova etiqueta ou atualizar detalhes de etiqueta existentes|
|/service/templates/delete|Repor o modelo de correio eletrónico de API de gestão predefinido|
|/service/templates/read|Obtém todos os modelos de e-mail ou o e-mail de API Management obtém os detalhes de modelo|
|/service/templates/write|Criar ou atualizar o modelo de correio eletrónico de API Management ou modelo de correio eletrónico de atualizações de API Management|
|/service/tenant/delete|Remover configuração da política para o inquilino|
|/service/tenant/deploy/action|Executa uma tarefa de implementação para aplicar as alterações a partir do ramo de git especificado para a configuração na base de dados.|
|/service/tenant/operationResults/read|Obter a lista de resultados da operação ou obter Resultado de uma operação específica|
|/service/tenant/read|Obter a configuração de política para o inquilino ou Get inquilino detalhes de informações de acesso|
|/service/tenant/regeneratePrimaryKey/action|Regenerar a chave de acesso primária|
|/service/tenant/regenerateSecondaryKey/action|Regenerar a chave de acesso secundária|
|/service/tenant/save/action|Cria a consolidação com instantâneos de configuração para o ramo especificado no repositório|
|/service/tenant/syncState/read|Obter o estado da última sincronização do git|
|/service/tenant/validate/action|Valida as alterações do ramo do git especificado|
|/service/tenant/write|Definir a configuração de política para o inquilino ou para detalhes de informação de acesso de inquilino de atualização|
|/service/updatecertificate/action|Carregar o certificado SSL para um serviço de gestão de API|
|/service/updatehostname/action|O programa de configuração, atualizar ou remover nomes de domínio personalizado para um serviço de gestão de API|
|/service/users/action|Registar um novo utilizador|
|/service/users/applications/attachments/delete|Remove um anexo|
|/service/users/applications/attachments/read|Obtém os anexos de aplicação ou obtém anexo|
|/service/users/applications/attachments/write|Adicionar anexo à aplicação|
|/service/users/applications/delete|Remove existentes da aplicação|
|/service/users/applications/read|Obter a lista de todas as aplicações de utilizador ou detalhes da aplicação obtém API Management|
|/service/users/applications/write|Regista detalhes da aplicação API de gestão ou atualizações de uma aplicação|
|/service/users/delete|Remover conta de utilizador|
|/service/users/generateSsoUrl/action|Gere o URL do SSO. O URL pode ser utilizado para aceder ao portal de administração|
|/service/users/groups/read|Obter a lista de grupos de utilizadores|
|/service/users/keys/read|Obter a lista de chaves de utilizador|
|/service/users/read|Obter uma lista de utilizadores registados ou obter os detalhes da conta de utilizador|
|/service/users/subscriptions/read|Obter a lista de subscrições do utilizador|
|/service/users/token/action|Obter o token acesso token para um utilizador|
|/service/users/write|Registar um novo utilizador ou detalhes da conta de atualização de um utilizador existente|
|/service/write|Criar uma nova instância do serviço de gestão de API|
|/unregister/action|Subscrição de anular o registo do fornecedor de recursos de Microsoft.ApiManagement|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Operação | Descrição |
|---|---|
|/checkAccess/action|Verifica se o autor da chamada está autorizado a executar uma ação específica|
|/classicAdministrators/delete|Remove o administrador da subscrição.|
|/classicAdministrators/read|Lê os administradores da subscrição.|
|/classicAdministrators/write|Adiciona ou modifica o administrador de uma subscrição.|
|/elevateAccess/action|Garante ao chamador Administrador de Acesso dos Utilizadores acesso ao âmbito do inquilino|
|/locks/delete|Elimina bloqueios no âmbito especificado.|
|/locks/read|Obtém bloqueios no âmbito especificado.|
|/locks/write|Adiciona bloqueios ao âmbito especificado.|
|/permissions/read|Lista todas as permissões que o chamador tem num determinado âmbito.|
|/policyAssignments/delete|Elimine uma atribuição de política no âmbito especificado.|
|/policyAssignments/read|Obtenha informação sobre uma atribuição de política.|
|/policyAssignments/write|Crie uma atribuição de política no âmbito especificado.|
|/policyDefinitions/delete|Elimine uma definição de política.|
|/policyDefinitions/read|Obtenha informação sobre uma definição de política.|
|/policyDefinitions/write|Crie uma definição de política personalizada.|
|/policySetDefinitions/delete|Elimine uma definição do conjunto de políticas.|
|/policySetDefinitions/read|Obter informações sobre uma definição do conjunto de políticas.|
|/policySetDefinitions/write|Crie uma definição do conjunto de políticas personalizada.|
|/providerOperations/read|Obter operações para todos os fornecedores de recursos que possam ser utilizadas em definições de funções.|
|/roleAssignments/delete|Elimina uma atribuição de função no âmbito especificado.|
|/roleAssignments/read|Obtém informações sobre uma atribuição de função.|
|/roleAssignments/write|Cria uma atribuição de função no âmbito especificado.|
|/roleDefinitions/delete|Eliminar a definição de função personalizada especificada.|
|/roleDefinitions/read|Obtém informação sobre uma definição de função.|
|/roleDefinitions/write|Criar ou atualizar uma definição de função personalizada com permissões especificadas e âmbitos atribuíveis.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Operação | Descrição |
|---|---|
|/automationAccounts/agentRegistrationInformation/read|Ler as informações de registo do DSC de automatização do Azure|
|/automationAccounts/agentRegistrationInformation/regenerateKey/action|Escreve um pedido para voltar a gerar chaves de DSC de automatização do Azure|
|/automationAccounts/certificates/delete|Elimina um recurso de certificado da automatização do Azure|
|/automationAccounts/certificates/read|Obtém um recurso de certificado da automatização do Azure|
|/automationAccounts/certificates/write|Cria ou atualiza um recurso de certificado da automatização do Azure|
|/automationAccounts/compilationjobs/read|Lê a compilação do DSC de automatização do Azure|
|/automationAccounts/compilationjobs/write|Escreve compilação do DSC de automatização do Azure|
|/automationAccounts/configurations/delete|Elimina o conteúdo do DSC de automatização do Azure|
|/automationAccounts/configurations/getCount/action|Lê a contagem de conteúdo do DSC de automatização do Azure|
|/automationAccounts/configurations/read|Obtém o conteúdo do DSC de automatização do Azure|
|/automationAccounts/configurations/write|Escreve o conteúdo do DSC de automatização do Azure|
|/automationAccounts/connections/delete|Elimina um recurso de ligação da automatização do Azure|
|/automationAccounts/connections/read|Obtém um recurso de ligação da automatização do Azure|
|/automationAccounts/connections/write|Cria ou atualiza um recurso de ligação da automatização do Azure|
|/automationAccounts/connectionTypes/delete|Elimina um recurso de tipo de ligação de automatização do Azure|
|/automationAccounts/connectionTypes/read|Obtém um recurso de tipo de ligação de automatização do Azure|
|/automationAccounts/connectionTypes/write|Cria um recurso de tipo de ligação de automatização do Azure|
|/automationAccounts/credentials/delete|Elimina um recurso de credencial de automatização do Azure|
|/automationAccounts/credentials/read|Obtém um recurso de credencial de automatização do Azure|
|/automationAccounts/credentials/write|Cria ou atualiza um recurso de credencial de automatização do Azure|
|/automationAccounts/delete|Elimina uma conta de automatização do Azure|
|/automationAccounts/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/automationAccounts/diagnosticSettings/write|Define a definição de diagnóstico para o recurso|
|/automationAccounts/hybridRunbookWorkerGroups/delete|Elimina recursos de trabalho de Runbook híbrida|
|/automationAccounts/hybridRunbookWorkerGroups/read|Lê recursos de trabalho de Runbook híbrida|
|/automationAccounts/jobs/output/action|Obtém o resultado de uma tarefa|
|/automationAccounts/jobs/output/action|Obtém o resultado de uma tarefa|
|/automationAccounts/jobs/read|Obtém uma tarefa de automatização do Azure|
|/automationAccounts/jobs/read|Obtém uma tarefa de automatização do Azure|
|/automationAccounts/jobs/resume/action|Retoma uma tarefa de automatização do Azure|
|/automationAccounts/jobs/resume/action|Retoma uma tarefa de automatização do Azure|
|/automationAccounts/jobs/runbookContent/action|Obtém o conteúdo do runbook da automatização do Azure no momento da execução da tarefa|
|/automationAccounts/jobs/runbookContent/action|Obtém o conteúdo do runbook da automatização do Azure no momento da execução da tarefa|
|/automationAccounts/jobs/stop/action|Para uma tarefa de automatização do Azure|
|/automationAccounts/jobs/stop/action|Para uma tarefa de automatização do Azure|
|/automationAccounts/jobs/streams/read|Obtém um fluxo de trabalho da automatização do Azure|
|/automationAccounts/jobs/streams/read|Obtém um fluxo de trabalho da automatização do Azure|
|/automationAccounts/jobs/suspend/action|Suspende uma tarefa de automatização do Azure|
|/automationAccounts/jobs/suspend/action|Suspende uma tarefa de automatização do Azure|
|/automationAccounts/jobs/write|Cria uma tarefa de automatização do Azure|
|/automationAccounts/jobs/write|Cria uma tarefa de automatização do Azure|
|/automationAccounts/jobSchedules/delete|Elimina uma tarefa da automatização do Azure|
|/automationAccounts/jobSchedules/read|Obtém uma tarefa da automatização do Azure|
|/automationAccounts/jobSchedules/write|Cria uma tarefa da automatização do Azure|
|/automationAccounts/linkedWorkspace/read|Obtém a área de trabalho associada à conta de automatização|
|/automationAccounts/logDefinitions/read|Obtém os registos disponíveis para a conta de automatização|
|/automationAccounts/modules/activities/read|Obtém as actividades de automatização do Azure|
|/automationAccounts/modules/delete|Elimina um módulo de automatização do Azure|
|/automationAccounts/modules/read|Obtém um módulo de automatização do Azure|
|/automationAccounts/modules/write|Cria ou atualiza um módulo de automatização do Azure|
|/automationAccounts/nodeConfigurations/delete|Elimina a configuração do nó do DSC de automatização do Azure|
|/automationAccounts/nodeConfigurations/read|Lê a configuração do nó do DSC de automatização do Azure|
|/automationAccounts/nodeConfigurations/readContent/action|Lê o conteúdo de configuração do nó do DSC de automatização do Azure|
|/automationAccounts/nodeConfigurations/write|Escreve a configuração do nó do DSC de automatização do Azure|
|/automationAccounts/nodes/delete|Elimina nós de DSC de automatização do Azure|
|/automationAccounts/nodes/read|Lê nós de DSC de automatização do Azure|
|/automationAccounts/nodes/reports/read|Lê contentss de relatório de DSC de automatização do Azure|
|/automationAccounts/nodes/reports/read|Lê os relatórios do Automation DSC do Azure|
|/automationAccounts/objectDataTypes/fields/read|Obtém TypeFields de automatização do Azure|
|/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read|Obtém as definições de métrica de automatização|
|/automationAccounts/read|Obtém uma conta de automatização do Azure|
|/automationAccounts/runbooks/delete|Elimina um runbook de automatização do Azure|
|/automationAccounts/runbooks/draft/publish/action|Publica um rascunho de runbook da automatização do Azure|
|/automationAccounts/runbooks/draft/read|Obtém um rascunho de runbook da automatização do Azure|
|/automationAccounts/runbooks/draft/readContent/action|Obtém o conteúdo de um rascunho de runbook da automatização do Azure|
|/automationAccounts/runbooks/draft/testJob/read|Obtém uma tarefa de teste de rascunho de runbook de automatização do Azure|
|/automationAccounts/runbooks/draft/testJob/resume/action|Retoma uma tarefa de teste de rascunho de runbook de automatização do Azure|
|/automationAccounts/runbooks/draft/testJob/stop/action|Parar uma tarefa de teste de rascunho de runbook de automatização do Azure|
|/automationAccounts/runbooks/draft/testJob/suspend/action|Suspende uma tarefa de teste de rascunho de runbook de automatização do Azure|
|/automationAccounts/runbooks/draft/testJob/write|Cria uma tarefa de teste de rascunho de runbook de automatização do Azure|
|/automationAccounts/runbooks/draft/undoEdit/action|Anular edições de um rascunho de runbook da automatização do Azure|
|/automationAccounts/runbooks/draft/writeContent/action|Cria o conteúdo de um rascunho de runbook da automatização do Azure|
|/automationAccounts/runbooks/read|Obtém um runbook de automatização do Azure|
|/automationAccounts/runbooks/readContent/action|Obtém o conteúdo de um runbook de automatização do Azure|
|/automationAccounts/runbooks/write|Cria ou atualiza um runbook de automatização do Azure|
|/automationAccounts/schedules/delete|Elimina um recurso de agenda da automatização do Azure|
|/automationAccounts/schedules/read|Obtém um recurso de agenda da automatização do Azure|
|/automationAccounts/schedules/write|Cria ou atualiza um recurso de agenda da automatização do Azure|
|/automationAccounts/statistics/read|Obtém estatísticas de automatização do Azure|
|/automationAccounts/usages/read|Obtém a utilização da automatização do Azure|
|/automationAccounts/variables/delete|Elimina um recurso de variável da automatização do Azure|
|/automationAccounts/variables/read|Lê um recurso de variável da automatização do Azure|
|/automationAccounts/variables/write|Cria ou atualiza um recurso de variável da automatização do Azure|
|/automationAccounts/watchers/streams/read|Obtém um fluxo de tarefas do observador de automatização do Azure|
|/automationAccounts/webhooks/delete|Elimina um webhook de automatização do Azure |
|/automationAccounts/webhooks/generateUri/action|Gera um URI para um webhook de automatização do Azure|
|/automationAccounts/webhooks/read|Lê um webhook de automatização do Azure|
|/automationAccounts/webhooks/write|Cria ou atualiza um webhook de automatização do Azure|
|/automationAccounts/write|Cria ou atualiza uma conta de automatização do Azure|
|/automationAccounts/write|Cria ou atualiza uma conta de automatização do Azure|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

| Operação | Descrição |
|---|---|
|/b2cDirectories/delete|Eliminar o recurso do Diretório B2C|
|/b2cDirectories/read|Visualizar o recurso do Diretório B2C|
|/b2cDirectories/write|Criar ou atualizar o recursos do Diretório B2C|
|/operations/read|Ler todas as operações de API disponíveis para o fornecedor de recursos de Microsoft.AzureActiveDirectory|
|/register/action|Registar subscrição para o fornecedor de recursos de Microsoft.AzureActiveDirectory|

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

| Operação | Descrição |
|---|---|
|/Operations/read|Obtém as propriedades de uma operação do fornecedor de recursos|
|/register/action|Regista a subscrição no fornecedor de recursos de Microsoft.AzureStack|
|/registrations/customerSubscriptions/delete|Elimina uma subscrição do cliente de pilha do Azure|
|/registrations/customerSubscriptions/read|Obtém as propriedades de uma subscrição de cliente de pilha do Azure|
|/registrations/customerSubscriptions/write|Cria ou atualiza uma subscrição de cliente de pilha do Azure|
|/registrations/delete|Elimina um registo de pilha do Azure|
|/registrations/getActivationKey/action|Obtém a chave de ativação de pilha do Azure mais recente|
|/registrations/products/listDetails/action|Obtém expandido detalhes de um produto de pilha de Azure Marketplace|
|/registrations/products/read|Obtém as propriedades de um produto de pilha de Azure Marketplace|
|/registrations/read|Obtém as propriedades de um registo de pilha do Azure|
|/registrations/write|Cria ou atualiza um registo de pilha do Azure|

## <a name="microsoftbatch"></a>Microsoft.Batch

| Operação | Descrição |
|---|---|
|/batchAccounts/applications/delete|Elimina uma aplicação|
|/batchAccounts/applications/read|Apresenta uma lista de aplicações ou obtém as propriedades de uma aplicação|
|/batchAccounts/applications/versions/activate/action|Ativa um pacote de aplicação|
|/batchAccounts/applications/versions/delete|Elimina um pacote de aplicação|
|/batchAccounts/applications/versions/read|Obtém as propriedades de um pacote de aplicação|
|/batchAccounts/applications/versions/write|Cria um novo pacote de aplicação ou atualiza um pacote de aplicação existente|
|/batchAccounts/applications/write|Cria uma nova aplicação ou atualiza uma aplicação existente|
|/batchAccounts/certificateOperationResults/read|Obtém os resultados de uma operação de certificado longa numa conta do Batch|
|/batchAccounts/certificates/cancelDelete/action|Cancela a falha na eliminação de um certificado numa conta do Batch|
|/batchAccounts/certificates/delete|Elimina um certificado de uma conta do Batch|
|/batchAccounts/certificates/read|Apresenta uma lista de certificados numa conta do Batch ou obtém as propriedades de um certificado|
|/batchAccounts/certificates/write|Cria um novo certificado numa conta do Batch ou atualiza um certificado existente|
|/batchAccounts/delete|Elimina uma conta do Batch|
|/batchAccounts/listkeys/action|Apresenta uma lista de aceder às chaves para uma conta do Batch|
|/batchAccounts/operationResults/read|Obtém os resultados de uma operação de conta de Batch longa|
|/batchAccounts/poolOperationResults/read|Obtém os resultados de uma operação de agrupamento de execução longa numa conta do Batch|
|/batchAccounts/pools/delete|Elimina um agrupamento de uma conta do Batch|
|/batchAccounts/pools/disableAutoscale/action|Desativa o dimensionamento automático para um conjunto de conta do Batch|
|/batchAccounts/pools/read|Apresenta uma lista de agrupamentos de uma conta do Batch ou obtém as propriedades de um agrupamento|
|/batchAccounts/pools/stopResize/action|Pára em curso uma redimensiona a operação de um conjunto de conta do Batch|
|/batchAccounts/pools/upgradeOs/action|Atualiza o sistema operativo de um conjunto de conta do Batch|
|/batchAccounts/pools/write|Cria um novo conjunto numa conta do Batch ou atualiza um conjunto existente|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/batchAccounts/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para o serviço Batch|
|/batchAccounts/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o serviço Batch|
|/batchAccounts/read|Apresenta uma lista de contas do Batch ou obtém as propriedades de uma conta do Batch|
|/batchAccounts/regeneratekeys/action|Gera de novo a aceder às chaves para uma conta do Batch|
|/batchAccounts/syncAutoStorageKeys/action|Sincroniza as chaves de acesso da conta do storage automaticamente configurado para uma conta do Batch|
|/batchAccounts/write|Cria uma nova conta do Batch ou atualiza uma conta de Batch existente|
|/locations/checkNameAvailability/action|Verifica se o nome da conta é válido e não em utilização.|
|/locations/quotas/read|Obtém as quotas de lote da subscrição especificada na região do Azure especificada|
|/register/action|Regista a subscrição para o fornecedor de recursos do Batch e permite a criação de contas do Batch|
|/unregister/action|Anula o registo da subscrição para o fornecedor de recursos do Batch que impede a criação de contas do Batch|

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

| Operação | Descrição |
|---|---|
|/clusters/delete|Elimina um cluster de AI do Batch|
|/clusters/read|Apresenta uma lista de clusters de AI do Batch ou obtém as propriedades de um cluster de AI do Batch|
|/clusters/remoteLoginInformation/action|Lista as informações de início de sessão remoto para um cluster de AI do Batch|
|/clusters/write|Cria um novo cluster de AI do Batch ou atualiza um cluster existente de AI do Batch|
|/fileservers/delete|Elimina um servidor de ficheiros Batch AI|
|/fileservers/read|Apresenta uma lista de AI do Batch fileservers ou obtém as propriedades de um servidor de ficheiros Batch AI|
|/fileservers/resume/action|Retoma um servidor de ficheiros Batch AI|
|/fileservers/suspend/action|Suspende a um servidor de ficheiros Batch AI|
|/fileservers/write|Cria um novo servidor de ficheiros de AI do Batch ou atualiza um servidor de ficheiros existente do Batch AI|
|/jobs/delete|Elimina uma tarefa de lote AI|
|/jobs/read|Lista as tarefas de lote AI ou obtém as propriedades de uma tarefa de lote AI|
|/jobs/remoteLoginInformation/action|Lista as informações de início de sessão remoto para uma tarefa de lote AI|
|/jobs/terminate/action|Termina uma tarefa de lote AI|
|/jobs/write|Cria uma nova tarefa de lote AI ou atualiza uma tarefa de lote AI existente|
|/register/action|Regista a subscrição para o fornecedor de recursos do Batch AI e permite a criação dos recursos do Batch AI|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Operação | Descrição |
|---|---|
|/billingPeriods/read|Apresenta uma lista de períodos de faturação disponíveis|
|/invoices/read|Apresenta uma lista de faturas disponíveis|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Operação | Descrição |
|---|---|
|/mapApis/Delete|Operação de Eliminação|
|/mapApis/listSecrets/action|Listar os Segredos|
|/mapApis/listSingleSignOnToken/action|Ler Token de Autorização de Início de Sessão Único para o Recurso|
|/mapApis/Read|Operação de Leitura|
|/mapApis/regenerateKey/action|Regenera a Chave|
|/mapApis/Write|Operação de Escrita|
|/Operations/read|Descrição da operação.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Operação | Descrição |
|---|---|
|/checknameavailability/action|Verifica se um nome está disponível para utilização com uma nova Cache de Redis|
|/operations/read|Lista as operações que suporta o fornecedor 'Microsoft.Cache'.|
|/redis/delete|Eliminar a Cache de Redis completa|
|/redis/export/action|Exportar dados Redis para os blobs de armazenamento com prefixo no formato especificado|
|/redis/firewallRules/delete|Eliminar as regras de firewall do IP da Cache de Redis|
|/redis/firewallRules/read|Obter as regras de firewall do IP da Cache de Redis|
|/redis/firewallRules/write|Editar regras de firewall do IP de uma Cache de Redis|
|/redis/forceReboot/action|Forçar reinício de uma instância da cache, potencialmente com perda de dados.|
|/redis/import/action|Importar dados de um formato especificado a partir de vários blobs para Redis|
|/redis/linkedservers/delete|Eliminar Servidor Ligado de uma Cache de Redis|
|/redis/linkedservers/read|Obter Servidores Ligados associados a uma cache de redis.|
|/redis/linkedservers/write|Adicionar Servidor Ligado a uma Cache de Redis|
|/redis/listKeys/action|Ver o valor das chaves de acesso da Cache de Redis no portal de gestão|
|/redis/listUpgradeNotifications/read|Listar as Notificações de Atualização mais recentes para o inquilino da cache.|
|/redis/locations/operationresults/read|Obtém o resultado de uma longa executar a operação para o qual o cabeçalho 'Location' anteriormente foi devolvido ao cliente|
|/redis/metricDefinitions/read|Obtem as métricas disponíveis para uma Cache de Redis|
|/redis/patchSchedules/delete|Eliminar a agenda de correção da Cache de Redis|
|/redis/patchSchedules/read|Obtém a agenda de correções de uma Cache de Redis|
|/redis/patchSchedules/write|Modificar a agenda de correções de uma Cache de Redis|
|/redis/read|Ver as definições e a configuração da Cache de Redis no portal de gestão|
|/redis/regenerateKey/action|Alterar o valor das chaves de acesso da Cache de Redis no portal de gestão|
|/redis/start/action|Iniciar uma instância de cache.|
|/redis/stop/action|Parar uma instância de cache.|
|/redis/write|Modificar as definições e a configuração da Cache de Redis no portal de gestão|
|/register/action|Regista o fornecedor de recursos "Microsoft.Cache" com uma subscrição|
|/unregister/action|Anula o registo do fornecedor de recursos "Microsoft.Cache" com uma subscrição|

## <a name="microsoftcapacity"></a>Microsoft.Capacity

| Operação | Descrição |
|---|---|
|/register/action|Regista o fornecedor de recursos de capacidade e permite a criação dos recursos de capacidade.|
|/reservationorders/action|Atualizar qualquer reserva|
|/reservationorders/delete|Eliminar reserva de qualquer|
|/reservationorders/read|Ler todas as reservas|
|/reservationorders/reservations/action|Atualizar qualquer reserva|
|/reservationorders/reservations/delete|Eliminar reserva de qualquer|
|/reservationorders/reservations/read|Ler todas as reservas|
|/reservationorders/reservations/revisions/read|Ler todas as reservas|
|/reservationorders/reservations/write|Criar reserva de qualquer|
|/reservationorders/write|Criar reserva de qualquer|

## <a name="microsoftcdn"></a>Microsoft.Cdn

| Operação | Descrição |
|---|---|
|/CheckNameAvailability/action||
|/CheckResourceUsage/action||
|/edgenodes/delete||
|/edgenodes/read||
|/edgenodes/write||
|/operationresults/delete||
|/operationresults/profileresults/CheckResourceUsage/action||
|/operationresults/profileresults/delete||
|/operationresults/profileresults/endpointresults/CheckResourceUsage/action||
|/operationresults/profileresults/endpointresults/customdomainresults/delete||
|/operationresults/profileresults/endpointresults/customdomainresults/ DisableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/ EnableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/read||
|/operationresults/profileresults/endpointresults/customdomainresults/write||
|/operationresults/profileresults/endpointresults/delete||
|/operationresults/profileresults/endpointresults/Load/action||
|/operationresults/profileresults/endpointresults/originresults/delete||
|/operationresults/profileresults/endpointresults/originresults/read||
|/operationresults/profileresults/endpointresults/originresults/write||
|/operationresults/profileresults/endpointresults/Purge/action||
|/operationresults/profileresults/endpointresults/read||
|/operationresults/profileresults/endpointresults/Start/action||
|/operationresults/profileresults/endpointresults/Stop/action||
|/operationresults/profileresults/endpointresults/ValidateCustomDomain/action||
|/operationresults/profileresults/endpointresults/write||
|/operationresults/profileresults/GenerateSsoUri/action||
|/operationresults/profileresults/GetSupportedOptimizationTypes/action||
|/operationresults/profileresults/read||
|/operationresults/profileresults/write||
|/operationresults/read||
|/operationresults/write||
|/operations/read||
|/profiles/CheckResourceUsage/action||
|/profiles/delete||
|/profiles/endpoints/CheckResourceUsage/action||
|/profiles/endpoints/customdomains/delete||
|/profiles/endpoints/customdomains/DisableCustomHttps/action||
|/profiles/endpoints/customdomains/EnableCustomHttps/action||
|/profiles/endpoints/customdomains/read||
|/profiles/endpoints/customdomains/write||
|/profiles/endpoints/delete||
|/profiles/endpoints/Load/action||
|/profiles/endpoints/origins/delete||
|/profiles/endpoints/origins/read||
|/profiles/endpoints/origins/write||
|/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/read|Obtém as definições de diagnóstico para o recurso|
|/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico para o recurso|
|/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para Microsoft.Cdn|
|/profiles/endpoints/Purge/action||
|/profiles/endpoints/read||
|/Profiles/Endpoints/Start/Action||
|/profiles/endpoints/Stop/action||
|/profiles/endpoints/ValidateCustomDomain/action||
|/profiles/endpoints/write||
|/profiles/GenerateSsoUri/action||
|/profiles/GetSupportedOptimizationTypes/action||
|/profiles/read||
|/profiles/write||
|/register/action|Regista a subscrição para o fornecedor de recursos CDN e ativa a criação de perfis CDN.|
|/ValidateProbe/action||

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Operação | Descrição |
|---|---|
|/certificateOrders/certificates/Delete|Eliminar um certificado existente|
|/certificateOrders/certificates/Read|Obter a lista de certificados|
|/certificateOrders/certificates/Write|Adicionar um certificado novo ou atualizar um já existente|
|/certificateOrders/Delete|Eliminar um AppServiceCertificate existente|
|/certificateOrders/operations/Read|Lista todas as operações de registo de certificados do serviço de aplicações|
|/certificateOrders/Read|Obter a lista de CertificateOrders|
|/certificateOrders/reissue/Action|Volte a emitir um certificateorder existente|
|/certificateOrders/renew/Action|Renovar um certificateorder existente|
|/certificateOrders/resendEmail/Action|Certificado de reenviar correio eletrónico|
|/certificateOrders/resendRequestEmails/Action|Volte a enviar e-mails de pedido para outro endereço de e-mail|
|/certificateOrders/resendRequestEmails/Action|Obter selar de site para um certificado de serviço de aplicações emitido|
|/certificateOrders/retrieveCertificateActions/Action|Obter a lista de ações de certificado|
|/certificateOrders/retrieveEmailHistory/Action|Obter o histórico de correio eletrónico de certificado|
|/certificateOrders/verifyDomainOwnership/Action|Verificar a propriedade do domínio|
|/certificateOrders/Write|Adicionar um novo certificateOrder ou atualizar um já existente|
|/provisionGlobalAppServicePrincipalInUserTenant/Action|Principal de serviço de aprovisionamento para o principal da aplicação de serviço|
|/register/action|Registar o fornecedor de recursos Microsoft Certificates para a subscrição|
|/validateCertificateRegistrationInformation/Action|Validar o objeto de compra do certificado sem submetê-la|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| Operação | Descrição |
|---|---|
|/capabilities/read|Mostra as capacidades|
|/checkDomainNameAvailability/action|Verifica a disponibilidade de um nome de domínio fornecido.|
|/domainNames/active/write|Define o nome do domínio ativo.|
|/domainNames/availabilitySets/read|Mostra o conjunto de disponibilidade do recurso.|
|/domainNames/capabilities/read|Mostra as funções de nome de domínio|
|/domainNames/delete|Remove os nomes de domínio dos recursos.|
|/domainNames/extensions/delete|Remova as extensões do nome de domínio.|
|/domainNames/extensions/operationStatuses/read|Lê o estado da operação das extensões de nomes de domínio.|
|/domainNames/extensions/read|Devolve as extensões do nome de domínio.|
|/domainNames/extensions/write|Adicione as extensões do nome de domínio.|
|/domainNames/internalLoadBalancers/delete|Remove um novo balanceador de carga interno.|
|/domainNames/internalLoadBalancers/operationStatuses/read|Lê o estado da operação dos balanceadores de carga internos de nomes de domínio.|
|/domainNames/internalLoadBalancers/read|Obtém os balanceadores de carga internos.|
|/domainNames/internalLoadBalancers/write|Cria um novo balanceador de carga interno.|
|/domainNames/loadBalancedEndpointSets/operationStatuses/read|Lê o estado da operação dos conjuntos de pontos finais com balanceamento de carga de nomes de domínio.|
|/domainNames/loadBalancedEndpointSets/read|Mostra os conjuntos de pontos finais com balanceamento de carga|
|/domainNames/read|Devolve os nomes de domínio dos recursos.|
|/domainNames/serviceCertificates/delete|Elimina os certificados de serviço utilizados.|
|/domainNames/serviceCertificates/operationStatuses/read|Lê o estado da operação dos certificados de serviço de nomes de domínio.|
|/domainNames/serviceCertificates/read|Devolve os certificados de serviço utilizados.|
|/domainNames/serviceCertificates/write|Adiciona ou modifica os certificados de serviço utilizados.|
|/domainNames/slots/delete|Elimina um bloco de implementação fornecido.|
|/domainNames/slots/operationStatuses/read|Lê o estado da operação das ranhuras de nomes de domínio.|
|/domainNames/slots/read|Mostra os blocos de implementação.|
|/domainNames/slots/roles/extensionReferences/delete|Remova a referência da extensão para a função de bloco de implementação.|
|/domainNames/slots/roles/extensionReferences/operationStatuses/read|Lê o estado da operação das referências de extensão de funções de ranhuras de nomes de domínio.|
|/domainNames/slots/roles/extensionReferences/read|Devolve a referência da extensão para a função de bloco de implementação.|
|/domainNames/slots/roles/extensionReferences/write|Adicionar ou modificar a referência da extensão para a função de bloco de implementação.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read|Obtém as definições de diagnóstico.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write|Adiciona ou modifica as definições de diagnóstico.|
|/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read|Obtém as definições das métricas.|
|/domainNames/slots/roles/read|Obtém a função do bloco de implementação.|
|/domainNames/slots/roles/roleInstances/operationStatuses/read|Lê o estado da operação das instâncias de função das funções de ranhuras de nomes de domínio.|
|/domainNames/slots/roles/roleInstances/read|Obtém a instância de função.|
|/domainNames/slots/roles/roleInstances/rebuild/action|Recria a instância de função.|
|/domainNames/slots/roles/roleInstances/reimage/action|Recria a imagem da instância de função.|
|/domainNames/slots/roles/roleInstances/restart/action|Reinicia instâncias de função.|
|/domainNames/slots/start/action|Inicia um bloco de implementação.|
|/domainNames/slots/state/start/write|Altera o estado do bloco de implementação para parado.|
|/domainNames/slots/state/stop/write|Altera o estado do bloco de implementação para iniciado.|
|/domainNames/slots/stop/action|Suspende o bloco de implementação.|
|/domainNames/slots/upgradeDomain/write|Percorre a atualização do domínio.|
|/domainNames/slots/write|Cria ou atualiza a implementação.|
|/domainNames/swap/action|Troca o bloco de teste pelo bloco de produção.|
|/domainNames/write|Adiciona ou modifica os nomes de domínio dos recursos.|
|/moveSubscriptionResources/action|Mover todos os recursos clássicos para uma subscrição diferente.|
|/operatingSystemFamilies/read|Lista as famílias de sistemas operativos convidadas disponíveis no Microsoft Azure, e lista as versões de sistema operativo disponíveis para cada família.|
|/operatingSystems/read|Lista as versões dos sistemas operativos convidados que estão, de momento, disponíveis no Microsoft Azure.|
|/quotas/read|Obtém a quota da subscrição.|
|/register/action|Registar na Computação Clássica|
|/resourceTypes/skus/read|Obtém a lista de SKUs para tipos de recurso suportados.|
|/validateSubscriptionMoveAvailability/action|Valide a disponibilidade da subscrição para operação de movimentação clássica.|
|/virtualMachines/associatedNetworkSecurityGroups/delete|Elimina o grupo de segurança de rede associado à máquina virtual.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read|Lê o estado da operação dos grupos de segurança de rede associados a máquinas virtuais.|
|/virtualMachines/associatedNetworkSecurityGroups/read|Obtém o grupo de segurança de rede associado à máquina virtual.|
|/virtualMachines/associatedNetworkSecurityGroups/write|Adiciona um grupo de segurança de rede associado à máquina virtual.|
|/virtualMachines/asyncOperations/read|Obtém as operações assíncronas possíveis|
|/virtualMachines/attachDisk/action|Anexa um disco de dados a uma máquina virtual.|
|/virtualMachines/delete|Remove máquinas virtuais.|
|/virtualMachines/detachDisk/action|Desanexa um disco de dados da máquina virtual.|
|/virtualMachines/disks/read|Obtém a lista de discos de dados|
|/virtualMachines/downloadRemoteDesktopConnectionFile/action|Transfere o ficheiro RDP da máquina virtual.|
|/virtualMachines/extensions/operationStatuses/read|Lê o estado da operação das extensões de máquinas virtuais.|
|/virtualMachines/extensions/read|Obtém a extensão da máquina virtual.|
|/virtualMachines/extensions/write|Coloca a extensão da máquina virtual.|
|/virtualMachines/metrics/read|Obtém as métricas.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete|Elimina o grupo de segurança de rede associado à interface de rede.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/ operationStatuses/read|Lê o estado da operação dos grupos de segurança de rede associados a máquinas virtuais.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read|Obtém o grupo de segurança de rede associado à interface de rede.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write|Adiciona um grupo de segurança de rede associado à interface de rede.|
|/virtualMachines/operationStatuses/read|Lê o estado da operação das máquinas virtuais.|
|/virtualMachines/performMaintenance/action|Executa a manutenção na máquina virtual.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read|Obtém as definições de diagnóstico.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write|Adiciona ou modifica as definições de diagnóstico.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Obtém as definições das métricas.|
|/virtualMachines/read|Obtém a lista de máquinas virtuais.|
|/virtualMachines/redeploy/action|Reimplementa a máquina virtual.|
|/virtualMachines/restart/action|Reinicia máquinas virtuais.|
|/virtualMachines/shutdown/action|Encerra a máquina virtual.|
|/virtualMachines/start/action|Inicia a máquina virtual.|
|/virtualMachines/stop/action|Para a máquina virtual.|
|/virtualMachines/write|Adiciona ou modifica máquinas virtuais.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Operação | Descrição |
|---|---|
|/gatewaySupportedDevices/read|Obtém a lista de dispositivos suportados.|
|/networkSecurityGroups/delete|Elimina o grupo de segurança de rede.|
|/networkSecurityGroups/operationStatuses/read|Lê o estado da operação do grupo de segurança de rede.|
|/networkSecurityGroups/read|Obtém o grupo de segurança de rede.|
|/networkSecurityGroups/securityRules/delete|Elimina a regra de segurança.|
|/networkSecurityGroups/securityRules/operationStatuses/read|Lê o estado da operação das regras de segurança do grupo de segurança de rede.|
|/networkSecurityGroups/securityRules/read|Obtém a regra de segurança.|
|/networkSecurityGroups/securityRules/write|Adiciona ou atualiza uma regra de segurança.|
|/networkSecurityGroups/write|Adiciona um novo grupo de segurança de rede.|
|/quotas/read|Obtém a quota da subscrição.|
|/register/action|Registar na Rede Clássica|
|/reservedIps/delete|Elimina um IP reservado.|
|/reservedIps/join/action|Associar um IP reservado|
|/reservedIps/link/action|Ligar um IP reservado|
|/reservedIps/operationStatuses/read|Lê o estado da operação dos IPs reservados.|
|/reservedIps/read|Obtém os IPs reservados|
|/reservedIps/write|Adiciona um novo IP reservado|
|/virtualNetworks/capabilities/read|Mostra as capacidades|
|/virtualNetworks/checkIPAddressAvailability/action|Verifica a disponibilidade de um determinado endereço IP numa rede virtual.|
|/virtualNetworks/delete|Elimina a rede virtual.|
|/virtualNetworks/gateways/clientRevokedCertificates/delete|Anula a revogação de um certificado de cliente.|
|/virtualNetworks/gateways/clientRevokedCertificates/read|Lê os certificados de cliente revogados.|
|/virtualNetworks/gateways/clientRevokedCertificates/write|Revoga um certificado de cliente.|
|/virtualNetworks/gateways/clientRootCertificates/delete|Elimina o certificado de cliente do gateway da rede virtual.|
|/virtualNetworks/gateways/clientRootCertificates/download/action|Transfere o certificado por thumbprint.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/action|Lista o pacote de certificado do gateway da rede virtual.|
|/virtualNetworks/gateways/clientRootCertificates/read|Localiza os certificados de raiz de cliente.|
|/virtualNetworks/gateways/clientRootCertificates/write|Carrega um novo certificado de raiz de cliente.|
|/virtualNetworks/gateways/connections/connect/action|Estabelece uma ligação de gateway site para site.|
|/virtualNetworks/gateways/connections/disconnect/action|Desliga uma ligação de gateway site para site.|
|/virtualNetworks/gateways/connections/read|Obtém a lista de ligações.|
|/virtualNetworks/gateways/connections/test/action|Testa uma ligação de gateway site para site.|
|/virtualNetworks/gateways/delete|Elimina o gateway da rede virtual.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/action|Transfere o script de configuração do dispositivo.|
|/virtualNetworks/gateways/downloadDiagnostics/action|Transfere o diagnóstico do gateway.|
|/virtualNetworks/gateways/listCircuitServiceKey/action|Obtém a chave do serviço de circuito.|
|/virtualNetworks/gateways/listPackage/action|Lista o pacote do gateway da rede virtual.|
|/virtualNetworks/gateways/operationStatuses/read|Lê o estado da operação dos gateways de redes virtuais.|
|/virtualNetworks/gateways/packages/read|Obtém o pacote do gateway da rede virtual.|
|/virtualNetworks/gateways/read|Obtém os gateways da rede virtual.|
|/virtualNetworks/gateways/startDiagnostics/action|Inicia o diagnóstico do gateway da rede virtual.|
|/virtualNetworks/gateways/stopDiagnostics/action|Para o diagnóstico do gateway da rede virtual.|
|/virtualNetworks/gateways/write|Adiciona um gateway da rede virtual.|
|/virtualNetworks/join/action|Associa à rede virtual.|
|/virtualNetworks/operationStatuses/read|Lê o estado da operação das redes virtuais.|
|/virtualNetworks/peer/action|Elementos de uma rede virtual com outra rede virtual.|
|/virtualNetworks/read|Obtém a rede virtual.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete|Elimina o grupo de segurança de rede associado à sub-rede.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read|Lê o estado da operação do grupo de segurança de rede associado à sub-rede de rede virtual.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/read|Obtém o grupo de segurança de rede associado à sub-rede.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/write|Adiciona um grupo de segurança de rede associado à sub-rede.|
|/virtualNetworks/write|Adiciona uma nova rede virtual.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Operação | Descrição |
|---|---|
|/capabilities/read|Mostra as capacidades|
|/checkStorageAccountAvailability/action|Verifica a disponibilidade de uma conta de armazenamento.|
|/disks/read|Devolve o disco de conta de armazenamento.|
|/images/read|Devolve a imagem.|
|/osImages/read|Devolve a imagem de sistema operativo.|
|/publicImages/read|Obtém a imagem pública da máquina virtual.|
|/quotas/read|Obtém a quota da subscrição.|
|/register/action|Registar para Armazenamento Clássico|
|/storageAccounts/delete|Elimina a conta de armazenamento.|
|/storageAccounts/disks/delete|Elimina um determinado disco de conta de armazenamento.|
|/storageAccounts/disks/operationStatuses/read|Lê o estado da operação do recurso.|
|/storageAccounts/disks/read|Devolve o disco de conta de armazenamento.|
|/storageAccounts/disks/write|Adiciona um disco de conta de armazenamento.|
|/storageAccounts/images/delete|Elimina uma imagem de conta de armazenamento.|
|/storageAccounts/images/read|Devolve a imagem de conta de armazenamento.|
|/storageAccounts/listKeys/action|Lista as chaves de acesso para as contas de armazenamento.|
|/storageAccounts/operationStatuses/read|Lê o estado da operação do recurso.|
|/storageAccounts/osImages/delete|Elimina uma imagem do sistema operativo de uma conta de armazenamento.|
|/storageAccounts/osImages/read|Devolve a imagem do sistema operativo da conta de armazenamento.|
|/storageAccounts/read|Devolve a conta de armazenamento com a conta fornecida.|
|/storageAccounts/regenerateKey/action|Volta a gerar as chaves de acesso existentes para a conta de armazenamento.|
|/storageAccounts/services/diagnosticSettings/read|Obtém as definições de diagnóstico.|
|/storageAccounts/services/diagnosticSettings/write|Adiciona ou modifica as definições de diagnóstico.|
|/storageAccounts/services/metricDefinitions/read|Obtém as definições das métricas.|
|/storageAccounts/services/metrics/read|Obtém as métricas.|
|/storageAccounts/services/read|Obtém os serviços disponíveis.|
|/storageAccounts/write|Adiciona uma nova conta de armazenamento.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Operação | Descrição |
|---|---|
|/accounts/delete|Elimina contas da API|
|/accounts/listKeys/action|Listar Chaves|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição do diagnóstico para o recurso.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o recurso.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Obtém as métricas disponíveis para os Serviços Cognitivos.|
|/accounts/read|Lê contas da API.|
|/accounts/regenerateKey/action|Regenerar Chave|
|/accounts/skus/read|Lê os SKUs disponíveis para um recurso existente.|
|/accounts/usages/read|Obter a utilização de quota para um recurso existente.|
|/accounts/write|Escreve Contas da API.|
|/locations/checkSkuAvailability/action|Lê SKUs disponíveis para uma subscrição.|
|/Operations/read|Lista todas as operações disponíveis|
|/register/action|Regista a subscrição para serviços cognitivos|
|/skus/read|Lê SKUs disponíveis para os serviços cognitivos.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Operação | Descrição |
|---|---|
|/RateCard/read|Devolve oferece dados, os metadados do recurso/medir e as taxas para a subscrição fornecida.|
|/UsageAggregates/read|Obtém o-consumo Microsoft Azure por uma subscrição. O resultado contém agrega dados de utilização, subscrição e dos recursos relacionados com informações, um intervalo de tempo específico.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Operação | Descrição |
|---|---|
|/availabilitySets/delete|Elimina o conjunto de disponibilidade|
|/availabilitySets/read|Obter as propriedades de um conjunto de disponibilidade|
|/availabilitySets/vmSizes/read|Listar tamanhos disponíveis para criar ou atualizar uma máquina virtual no conjunto de disponibilidade|
|/availabilitySets/write|Cria um novo conjunto de disponibilidade ou atualiza um já existente|
|/disks/beginGetAccess/action|Obter o URI de SAS do Disco para acesso do blob|
|/disks/delete|Elimina o Disco|
|/disks/endGetAccess/action|Revogar o URI de SAS do Disco|
|/disks/read|Obter as propriedades de um Disco|
|/disks/write|Cria um novo Disco ou atualiza um já existente|
|/images/delete|Elimina a imagem|
|/images/read|Obter as propriedades da Imagem|
|/images/write|Cria uma nova Imagem ou atualiza uma existente|
|/locations/capsOperations/read|Obtém o estado de uma operação assíncrona de Caps|
|/locations/diskOperations/read|Obtém o estado de uma operação assíncrona do disco|
|/locations/operations/read|Obtém o estado de uma operação assíncrona|
|/locations/publishers/artifacttypes/offers/read|Obter as propriedades de uma oferta de imagem de plataforma|
|/locations/publishers/artifacttypes/offers/skus/read|Obter as propriedades de um Sku de imagem de plataforma|
|/locations/publishers/artifacttypes/offers/skus/versions/read|Obter as propriedades de uma versão de imagem de plataforma|
|/locations/publishers/artifacttypes/types/read|Obter as propriedades de um tipo de VMExtension|
|/locations/publishers/artifacttypes/types/versions/read|Obter as propriedades de uma versão de VMExtension|
|/locations/publishers/read|Obter as propriedades de um fabricante|
|/locations/runCommands/read|Lista os comandos de execução disponíveis na localização|
|/locations/usages/read|Obtém os limites de serviço e as quantidades de utilização atuais dos recursos de computação da subscrição numa localização|
|/locations/vmSizes/read|Lista os tamanhos de máquina virtual disponíveis numa localização|
|/operations/read|Lista as operações disponíveis no fornecedor de recursos Microsoft.Compute|
|/register/action|Regista a Subscrição no fornecedor de recursos Microsoft.Compute|
|/restorePointCollections/delete|Elimina a coleção de ponto de restauro e pontos de restauro contidos|
|/restorePointCollections/read|Obter as propriedades de uma coleção de ponto de restauro|
|/restorePointCollections/restorePoints/delete|Elimina o ponto de restauro|
|/restorePointCollections/restorePoints/read|Obter as propriedades de um ponto de restauro|
|/restorePointCollections/restorePoints/retrieveSasUris/action|Obter as propriedades de um ponto de restauro juntamente com os URIs de SAS do blob|
|/restorePointCollections/restorePoints/write|Cria um novo ponto de restauro|
|/restorePointCollections/write|Cria uma nova coleção de ponto de restauro ou atualiza uma existente|
|/sharedVMImages/delete|Elimina o SharedVMImage|
|/sharedVMImages/read|Obter as propriedades de um SharedVMImage|
|/sharedVMImages/versions/delete|Eliminar um SharedVMImageVersion|
|/sharedVMImages/versions/read|Obter as propriedades de um SharedVMImageVersion|
|/sharedVMImages/versions/replicate/action|Replicar um SharedVMImageVersion para regiões de destino|
|/sharedVMImages/versions/write|Criar um novo SharedVMImageVersion ou atualizar um já existente|
|/sharedVMImages/write|Cria um novo SharedVMImage ou atualiza um já existente|
|/skus/read|Obtém a lista de Microsoft. Compute SKUs disponíveis para a sua subscrição|
|/snapshots/beginGetAccess/action|Obter o URI de SAS do instantâneo para acesso do blob|
|/snapshots/delete|Eliminar um Instantâneo|
|/snapshots/endGetAccess/action|Revogar o URI de SAS do instantâneo|
|/snapshots/read|Obter as propriedades de um Instantâneo|
|/snapshots/write|Criar um novo Instantâneo ou atualizar um já existente|
|/virtualMachines/capture/action|Captura a máquina virtual, copiando os discos rígidos virtuais, e gera um modelo que pode ser utilizado para criar máquinas virtuais semelhantes|
|/virtualMachines/convertToManagedDisks/action|Converte os discos baseados em blob da máquina virtual em discos geridos|
|/virtualMachines/deallocate/action|Desliga a máquina virtual e liberta os recursos de computação|
|/virtualMachines/delete|Elimina a máquina virtual|
|/virtualMachines/extensions/delete|Elimina a extensão da máquina virtual|
|/virtualMachines/extensions/read|Obter as propriedades de uma extensão da máquina virtual|
|/virtualMachines/extensions/write|Cria uma nova extensão da máquina virtual ou atualiza uma já existente|
|/virtualMachines/generalize/action|Define o estado da máquina virtual como Generalizado e prepara-a para a captura|
|/virtualMachines/instanceView/read|Obtém o estado de runtime detalhado da máquina virtual e dos respetivos recursos|
|/virtualMachines/performMaintenance/action|Realiza a Operação de Manutenção na VM.|
|/virtualMachines/powerOff/action|Desliga a máquina virtual. Tenha em atenção que a máquina virtual irá continuar a ser faturada.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Lê as Definições de Métricas da Máquina Virtual|
|/virtualMachines/read|Obter as propriedades de uma máquina virtual|
|/virtualMachines/redeploy/action|Reimplementa a máquina virtual|
|/virtualMachines/reimage/action|Recria a imagem da máquina virtual que está a utilizar o disco de diferenciação.|
|/virtualMachines/restart/action|Reinicia a máquina virtual|
|/virtualMachines/runCommand/action|Executa um script predefinido na máquina virtual|
|/virtualMachines/start/action|Inicia a máquina virtual|
|/virtualMachines/vmSizes/read|Lista os tamanhos disponíveis para os quais a máquina virtual pode ser atualizada|
|/virtualMachines/write|Cria uma nova máquina virtual ou atualiza uma máquina virtual existente|
|/virtualMachineScaleSets/deallocate/action|Desliga e liberta os recursos de computação para as instâncias do Conjunto de Dimensionamento da Máquina Virtual |
|/virtualMachineScaleSets/delete|Elimina o Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/delete/action|Elimina as instâncias do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/extensions/delete|Elimina a Extensão do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/extensions/read|Obtém as propriedades de uma Extensão do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/extensions/write|Cria uma nova Extensão do Conjunto de Dimensionamento da Máquina Virtual ou atualiza uma existente|
|/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action|Percurso manualmente os domínios de atualização de plataforma de um conjunto de dimensionamento da Máquina Virtual para concluir uma atualização pendentes que está a ser bloqueada dos recursos de infraestrutura do serviço|
|/virtualMachineScaleSets/instanceView/read|Obtém a vista de instância do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/manualUpgrade/action|Atualiza manualmente as instâncias para o modelo mais recente do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/networkInterfaces/read|Obter propriedades de todas as interfaces de rede de um conjunto de dimensionamento de Máquina Virtual|
|/virtualMachineScaleSets/osUpgradeHistory/read|Obtém o histórico de atualizações do SO para um conjunto de dimensionamento de Máquina Virtual|
|/virtualMachineScaleSets/performMaintenance/action|Executa a manutenção planeada nas instâncias do conjunto de dimensionamento de Máquina Virtual|
|/virtualMachineScaleSets/powerOff/action|Desliga as instâncias do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/read|Lê as Definições de Métricas do Conjunto de Dimensionamento de Máquinas Virtuais|
|/virtualMachineScaleSets/publicIPAddresses/read|Obter propriedades de todos os endereços IP públicos de um conjunto de dimensionamento de Máquina Virtual|
|/virtualMachineScaleSets/read|Obter as propriedades de um Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/redeploy/action|Volte a implementar as instâncias do conjunto de dimensionamento de Máquina Virtual|
|/virtualMachineScaleSets/reimage/action|Recria imagens de instâncias do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/restart/action|Reinicia as instâncias do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/rollingUpgrades/cancel/action|Cancela a atualização gradual de um Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/rollingUpgrades/read|Obtenha o estado mais recente da Atualização Gradual para um Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/scale/action|Verificar se um Conjunto de Dimensionamento da Máquina Virtual existente pode ser Reduzido/Aumentado Horizontalmente para a contagem de instâncias especificada|
|/virtualMachineScaleSets/skus/read|Lista os SKUs válidos de um Conjunto de Dimensionamento da Máquina Virtual existente|
|/virtualMachineScaleSets/start/action|Inicia as instâncias do Conjunto de Dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/virtualMachines/deallocate/action|Desativa e liberta os recursos de computação de uma Máquina Virtual num Conjunto de Dimensionamento da VM.|
|/virtualMachineScaleSets/virtualMachines/delete|Eliminar uma Máquina Virtual específica num Conjunto de Dimensionamento da VM.|
|/virtualMachineScaleSets/virtualMachines/instanceView/read|Obtém a vista de instância de uma Máquina Virtual num Conjunto de Dimensionamento da VM.|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ ipConfigurations/publicIPAddresses/read|Obter propriedades de endereço IP público criado utilizando o conjunto de dimensionamento da Máquina Virtual. O conjunto de dimensionamento de máquina virtual, pode criar no máximo, um IP público por ipconfiguration (IP privado)|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read|Obter propriedades de uma ou todas as configurações de IP de uma interface de rede criada utilizando o conjunto de dimensionamento da Máquina Virtual. Configurações de IP representam IPs privados|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/read|Obter propriedades de uma ou todas as interfaces de rede de uma máquina virtual criada com o conjunto de dimensionamento da Máquina Virtual|
|/virtualMachineScaleSets/virtualMachines/performMaintenance/action|Executa a manutenção planeada numa instância de Máquina Virtual num conjunto de dimensionamento de Máquina Virtual|
|/virtualMachineScaleSets/virtualMachines/powerOff/action|Desativa uma instância de Máquina Virtual num Conjunto de Dimensionamento da VM.|
|/virtualMachineScaleSets/virtualMachines/providers/ Microsoft.Insights/metricDefinitions/read|Lê a Máquina Virtual nas Definições de Métricas de Conjunto de Dimensionamento|
|/virtualMachineScaleSets/virtualMachines/read|Obtém as propriedades de uma Máquina Virtual num Conjunto de Dimensionamento da VM|
|/virtualMachineScaleSets/virtualMachines/redeploy/action|Redeploys uma instância de Máquina Virtual num conjunto de dimensionamento de Máquina Virtual|
|/virtualMachineScaleSets/virtualMachines/reimage/action|Recria imagens de uma instância de Máquina Virtual num Conjunto de Dimensionamento de Máquina Virtual.|
|/virtualMachineScaleSets/virtualMachines/restart/action|Reinicia uma instância de Máquina Virtual num Conjunto de Dimensionamento da VM.|
|/virtualMachineScaleSets/virtualMachines/start/action|Inicia uma instância de Máquina Virtual num Conjunto de Dimensionamento da VM.|
|/virtualMachineScaleSets/virtualMachines/write|Atualiza as propriedades de uma Máquina Virtual num Conjunto de Dimensionamento de VM|
|/virtualMachineScaleSets/write|Cria um novo Conjunto de Dimensionamento da Máquina Virtual ou atualiza um já existente|

## <a name="microsoftconsumption"></a>Microsoft.Consumption

| Operação | Descrição |
|---|---|
|/balances/read|Liste a utilização de resumo para um período de faturação para um grupo de gestão.|
|/budgets/read|Liste os orçamentos por uma subscrição ou um grupo de gestão.|
|/budgets/write|Criar, atualizar e eliminar os orçamentos por uma subscrição ou um grupo de gestão.|
|/marketplaces/read|Lista os detalhes de utilização de recursos de mercado para um âmbito para EA e WebDirect subscrições.|
|/operations/read|Liste operações todos suportadas pelo fornecedor de recursos de Microsoft.Consumption.|
|/pricesheets/read|Liste os dados de Pricesheets para uma subscrição ou um grupo de gestão.|
|/reservationDetails/read|Lista os detalhes de utilização para instâncias reservados por grupos de gestão ou de ordem de reserva. Os dados de detalhes são por instância por nível de dia.|
|/reservationSummaries/read|Liste a utilização de resumo para instâncias reservadas por grupos de gestão ou de ordem de reserva. Os dados de resumos são ao nível diário ou mensal.|
|/reservationTransactions/read|Liste o histórico de transação para instâncias reservados por grupos de gestão.|
|/terms/read|Liste os termos de licenciamento para uma subscrição ou um grupo de gestão.|
|/usageDetails/read|Lista os detalhes de utilização para um âmbito para EA e WebDirect subscrições.|

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

| Operação | Descrição |
|---|---|
|/containerGroups/containers/logs/read|Obtenha registos para um contentor específico.|
|/containerGroups/delete|Elimine o grupo de contentores específico.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico para o grupo de contentor.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o grupo de contentor.|
|/containerGroups/providers/Microsoft.Insights/metricDefinitions/read|Obtém as métricas disponíveis para o grupo do contentor.|
|/containerGroups/read|Obtenha todos os grupos de contentores.|
|/containerGroups/write|Crie ou atualize um grupo de contentores específico.|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Operação | Descrição |
|---|---|
|/checkNameAvailability/read|Verifica se o nome de registo do contentor está disponível para utilização.|
|/locations/operationResults/read|Obtém um resultado de operação assíncrona|
|/operations/read|Apresenta uma lista de todas as operações de API de REST de registo de contentor do Azure disponíveis|
|/register/action|Regista a subscrição para o fornecedor de recursos de registo do contentor e permite a criação de registos do contentor.|
|/registries/delete|Elimina um registo de contentor.|
|/registries/eventGridFilters/delete|Elimina um filtro de grelha de eventos de um registo de contentor.|
|/registries/eventGridFilters/read|Obtém as propriedades do filtro de grelha de evento especificado ou apresenta uma lista de todos os filtros de grelha de eventos para o registo de contentor especificado.|
|/registries/eventGridFilters/write|Cria ou atualiza um filtro de grelha de eventos para um registo de contentor com os parâmetros especificados.|
|/registries/listCredentials/action|Lista as credenciais de início de sessão para o registo de contentor especificado.|
|/registries/listUsages/read|Lista as utilizações de quota para o registo de contentor especificado.|
|/registries/operationStatuses/read|Obtém o estado da operação assíncrona um registo|
|/registries/read|Obtém as propriedades de registo do contentor especificado ou apresenta uma lista de todos os registos de contentor sob a subscrição ou grupo de recursos especificado.|
|/registries/regenerateCredential/action|Gera de novo uma das credenciais de início de sessão para o registo de contentor especificado.|
|/registries/replications/delete|Elimina uma replicação de um registo de contentor.|
|/registries/replications/operationStatuses/read|Obtém o estado da operação assíncrona uma replicação|
|/registries/replications/read|Obtém as propriedades da replicação especificada ou apresenta uma lista de todas as replicações para o registo de contentor especificado.|
|/registries/replications/write|Cria ou atualiza uma replicação para um registo de contentor com os parâmetros especificados.|
|/registries/webhooks/delete|Elimina um webhook de um registo de contentor.|
|/registries/webhooks/getCallbackConfig/action|Obtém a configuração de URI do serviço e cabeçalhos personalizados para o webhook.|
|/registries/webhooks/listEvents/action|Apresenta uma lista de eventos recentes para o webhook especificado.|
|/registries/webhooks/operationStatuses/read|Obtém o estado da operação assíncrona um webhook|
|/registries/webhooks/ping/action|É acionado um evento de ping para serem enviados para o webhook.|
|/registries/webhooks/read|Obtém as propriedades do webhook especificado ou apresenta uma lista de todos os webhooks para o registo de contentor especificado.|
|/registries/webhooks/write|Cria ou atualiza um webhook para um registo de contentor com os parâmetros especificados.|
|/registries/write|Cria ou atualiza um registo de contentor com os parâmetros especificados.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Operação | Descrição |
|---|---|
|/containerServices/delete|Elimina o serviço de contentor especificado|
|/containerServices/read|Obtém o serviço de contentor especificado|
|/containerServices/write|Coloca ou atualiza o serviço de contentor especificado|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Operação | Descrição |
|---|---|
|/applications/delete|Operação de Eliminação|
|/applications/listSecrets/action|Listar Segredos|
|/applications/listSingleSignOnToken/action|Início de sessão único Tokens de leitura|
|/applications/read|Operação de Leitura|
|/applications/write|Operação de Escrita|
|/applications/write|Operação de Escrita|
|/listCommunicationPreference/action|Preferência de comunicação de lista|
|/operations/read|operações de leitura|
|/updateCommunicationPreference/action|Preferência de comunicação de atualização|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Operação | Descrição |
|---|---|
|/hubs/adobemetadata/action|Criar ou atualizar os metadados do cliente do Azure Insights Adobe|
|/hubs/adobemetadata/read|Ler os metadados do cliente do Azure Insights Adobe|
|/hubs/authorizationPolicies/delete|Eliminar qualquer política de assinatura de acesso partilhado de informações de cliente do Azure|
|/hubs/authorizationPolicies/read|Leia a que política de assinatura de acesso de partilhadas quaisquer informações de cliente do Azure|
|/hubs/authorizationPolicies/regeneratePrimaryKey/action|Regenerar a chave primária da política de assinaturas de acesso de partilhado das informações do cliente do Azure|
|/hubs/authorizationPolicies/regenerateSecondaryKey/action|Regenerar a chave secundária da política de assinaturas de acesso de partilhado das informações do cliente do Azure|
|/hubs/authorizationPolicies/write|Criar ou atualizar qualquer política de assinatura de acesso partilhado de informações de cliente do Azure|
|/hubs/connectors/activate/action|Ativar os conectores de informações de cliente do Azure|
|/hubs/connectors/activate/action|Ativar os conectores de informações de cliente do Azure|
|/hubs/connectors/delete|Eliminar qualquer conector das informações de cliente do Azure|
|/hubs/connectors/getruntimestatus/action|Obter o estado de runtime qualquer conector de informações de cliente do Azure|
|/hubs/connectors/mappings/activate/action|Ativar qualquer mapeamento de conector de informações de cliente do Azure|
|/hubs/connectors/mappings/delete|Eliminar qualquer mapeamento de conector de informações de cliente do Azure|
|/hubs/connectors/mappings/operations/read|Ler os resultados da operação mapeamento do conector Azure cliente Insights|
|/hubs/connectors/mappings/read|Ler qualquer mapeamento de conector de informações de cliente do Azure|
|/hubs/connectors/mappings/write|Criar ou atualizar qualquer mapeamento de conector de informações de cliente do Azure|
|/hubs/connectors/operations/read|Ler os resultados da operação de conector de informações de cliente do Azure|
|/hubs/connectors/read|Leia os conectores de informações de cliente do Azure|
|/hubs/connectors/saveauthinfo/action|Criar ou atualizar todas as informações conector cliente do Azure Insights autenticação|
|/hubs/connectors/update/action|Atualizar qualquer conector das informações de cliente do Azure|
|/hubs/connectors/write|Criar ou atualizar qualquer conector das informações de cliente do Azure|
|/hubs/crmmetadata/action|Criar ou atualizar os metadados do cliente do Azure Insights Crm|
|/hubs/crmmetadata/read|Ler os metadados do cliente do Azure Insights Crm|
|/hubs/delete|Eliminar o Hub de Insights qualquer cliente do Azure|
|/hubs/gdpr/delete|Eliminar qualquer Gdpr de informações de cliente do Azure|
|/hubs/gdpr/read|Ler qualquer Gdpr de informações de cliente do Azure|
|/hubs/gdpr/write|Criar ou atualizar qualquer Gdpr de informações de cliente do Azure|
|/hubs/getbillingcredits/read|Obter créditos de faturação de Hub de informações de cliente do Azure|
|/hubs/getbillinghistory/read|Obter o histórico de faturação de Hub de informações de cliente do Azure|
|/hubs/images/delete|Elimine qualquer imagem de informações de cliente do Azure|
|/hubs/images/read|Ler qualquer imagem de informações de cliente do Azure|
|/hubs/images/write|Criar ou atualizar qualquer imagem de informações de cliente do Azure|
|/hubs/interactions/delete|Eliminar qualquer interações de informações de cliente do azure|
|/hubs/interactions/operations/read|Ler os resultados da operação interação de informações de cliente do Azure|
|/hubs/interactions/read|Ler qualquer interação de informações de cliente do Azure|
|/hubs/interactions/suggestrelationshiplinks/action|Sugerimos relação ligações para qualquer interações de informações de cliente do Azure|
|/hubs/interactions/write|Criar ou atualizar qualquer interação de informações de cliente do Azure|
|/hubs/kpi/delete|Eliminar qualquer indicador de chave de desempenho de informações de cliente do Azure|
|/hubs/kpi/operations/read|Ler os resultados da operação indicadores de desempenho de chave de informações ao cliente do Azure|
|/hubs/kpi/read|Ler qualquer indicador de chave de desempenho de informações de cliente do Azure|
|/hubs/kpi/reprocess/action|Reprocessar qualquer indicadores de chave de desempenho de informações de cliente do Azure|
|/hubs/kpi/write|Criar ou atualizar qualquer indicador de chave de desempenho de informações de cliente do Azure|
|/hubs/links/delete|Eliminar todas as ligações de informações de cliente do Azure|
|/hubs/links/operations/read|Ler os resultados da operação de ligações de informações de cliente do Azure|
|/hubs/links/read|Ler todas as ligações de informações de cliente do Azure|
|/hubs/links/write|Criar ou atualizar todas as ligações de cliente do Azure|
|/hubs/msemetadata/action|Criar ou atualizar os metadados do cliente do Azure Insights Mse|
|/hubs/msemetadata/read|Ler os metadados do cliente do Azure Insights Mse|
|/hubs/operationresults/read|Obter resultados de operação de Hub de informações de cliente do Azure|
|/hubs/predictions/delete|Eliminar qualquer Predições de informações de cliente do Azure|
|/hubs/predictions/operations/read|Ler os resultados da operação Predições de informações de cliente do Azure|
|/hubs/predictions/read|Ler qualquer Predições de informações de cliente do Azure|
|/hubs/predictions/write|Criar ou atualizar qualquer Predições do cliente do Azure|
|/hubs/predictivematchpolicies/delete|Eliminar quaisquer políticas de correspondência preditiva de informações de cliente do Azure|
|/hubs/predictivematchpolicies/operations/read|Ler os resultados da operação Azure cliente Insights preditiva correspondência políticas|
|/hubs/predictivematchpolicies/read|Ler todas as políticas de cliente do Azure Insights correspondência preditiva|
|/hubs/predictivematchpolicies/write|Criar ou atualizar quaisquer políticas de correspondência preditiva de informações de cliente do Azure|
|/hubs/profiles/delete|Eliminar qualquer perfil de informações de cliente do Azure|
|/hubs/profiles/operations/read|Ler os resultados da operação de perfil de informações de cliente do Azure|
|/hubs/profiles/read|Ler nenhum perfil de informações de cliente do Azure|
|/hubs/profiles/write|Escrever qualquer perfil de informações de cliente do Azure|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/hubs/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para o recurso|
|/hubs/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o recurso|
|/hubs/read|Ler qualquer Hub de informações de cliente do Azure|
|/hubs/relationshiplinks/delete|Eliminar todas as ligações do cliente do Azure Insights relação|
|/hubs/relationshiplinks/operations/read|Ler os resultados da operação de ligações de relação de informações de cliente do Azure|
|/hubs/relationshiplinks/read|Ler todas as ligações do cliente do Azure Insights relação|
|/hubs/relationshiplinks/write|Criar ou atualizar todas as ligações do cliente do Azure Insights relação|
|/hubs/relationships/delete|Eliminar quaisquer relações de informações de cliente do Azure|
|/hubs/relationships/operations/read|Ler os resultados da operação de relações de informações de cliente do Azure|
|/hubs/relationships/read|Ler quaisquer relações de informações de cliente do Azure|
|/hubs/relationships/write|Criar ou atualizar quaisquer relações de informações de cliente do Azure|
|/hubs/roleAssignments/delete|Eliminar qualquer atribuição do cliente do Azure Insights Rbac|
|/hubs/roleAssignments/operations/read|Ler os resultados da operação de atribuição de Rbac do Azure ao cliente Insights|
|/hubs/roleAssignments/read|Ler qualquer atribuição do cliente do Azure Insights Rbac|
|/hubs/roleAssignments/write|Criar ou atualizar qualquer atribuição do cliente do Azure Insights Rbac|
|/hubs/roles/read|Ler quaisquer funções de Rbac de informações de cliente do Azure|
|/hubs/salesforcemetadata/action|Criar ou atualizar os metadados do cliente do Azure Insights SalesForce|
|/hubs/salesforcemetadata/read|Ler os metadados do cliente do Azure Insights SalesForce|
|/hubs/segments/delete|Elimine os segmentos de Insights qualquer cliente do Azure|
|/hubs/segments/dynamic/action|Gestão Segmenta de qualquer dinâmica de informações de cliente do Azure|
|/hubs/segments/read|Ler quaisquer segmentos de informações de cliente do Azure|
|/hubs/segments/static/action|Gestão Segmenta de qualquer estática de informações de cliente do Azure|
|/hubs/segments/write|Criar ou atualizar quaisquer segmentos de informações de cliente do Azure|
|/hubs/sqlconnectionstrings/delete|Eliminar qualquer SqlConnectionStrings de informações de cliente do Azure|
|/hubs/sqlconnectionstrings/read|Ler qualquer SqlConnectionStrings de informações de cliente do Azure|
|/hubs/sqlconnectionstrings/write|Criar ou atualizar qualquer SqlConnectionStrings de informações de cliente do Azure|
|/hubs/suggesttypeschema/action|Gerar sugerir esquema de tipo de dados de exemplo|
|/hubs/tenantmanagement/read|Gerir as definições de hub de informações de cliente do Azure|
|/hubs/views/delete|Eliminar qualquer vista de aplicação de informações de cliente do Azure|
|/hubs/views/read|Ler a qualquer vista de aplicação de informações de cliente do Azure|
|/hubs/views/write|Criar ou atualizar qualquer vista de aplicação de informações de cliente do Azure|
|/hubs/widgettypes/read|Ler todos os tipos de cliente do Azure Insights Widget de aplicação|
|/hubs/write|Criar ou atualizar qualquer Hub de informações de cliente do Azure|
|/operations/read|Ler Metadatas de Api de informações de cliente do Azure|
|/register/action|Regista a subscrição para o fornecedor de recursos de informações de cliente e permite a criação dos recursos de informações de cliente|
|/unregister/action|Anula o registo da subscrição para o fornecedor de recursos de informações de cliente|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Operação | Descrição |
|---|---|
|/catalogs/delete|Elimina o catálogo.|
|/catalogs/read|Obter propriedades para o catálogo ou catálogos sob a subscrição ou grupo de recursos.|
|/catalogs/write|Cria o catálogo ou as etiquetas e propriedades para o catálogo de atualizações.|
|/checkNameAvailability/action|Verifica a disponibilidade do nome de catálogo para o inquilino.|
|/operations/read|Lista as operações disponíveis no fornecedor de recursos de Microsoft.DataCatalog.|
|/register/action|Regista a subscrição no fornecedor de recursos de Microsoft.DataCatalog.|
|/unregister/action|Anula o registo da subscrição do fornecedor de recursos de Microsoft.DataCatalog.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Operação | Descrição |
|---|---|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/datafactories/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para datafactories|
|/factories/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/factories/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/factories/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para as fábricas de|
|/factories/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para as fábricas de|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Operação | Descrição |
|---|---|
|/accounts/computePolicies/delete|Elimine uma política de computação.|
|/accounts/computePolicies/read|Obter informações sobre uma política de computação.|
|/accounts/computePolicies/write|Criar ou atualizar uma política de computação.|
|/accounts/dataLakeStoreAccounts/delete|Desassociar uma conta de DataLakeStore de uma conta de DataLakeAnalytics.|
|/accounts/dataLakeStoreAccounts/read|Obter informações sobre uma conta de DataLakeStore ligada de uma conta de DataLakeAnalytics.|
|/accounts/dataLakeStoreAccounts/write|Criar ou atualizar uma conta de DataLakeStore ligada de uma conta de DataLakeAnalytics.|
|/accounts/delete|Elimine uma conta de DataLakeAnalytics.|
|/accounts/firewallRules/delete|Elimine uma regra de firewall.|
|/accounts/firewallRules/read|Obter informações sobre uma regra de firewall.|
|/accounts/firewallRules/write|Criar ou atualizar uma regra de firewall.|
|/accounts/operationResults/read|Obter o resultado de uma operação de conta DataLakeAnalytics.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Obter as definições de diagnóstico para a conta de DataLakeAnalytics.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Criar ou atualizar as definições de diagnóstico para a conta de DataLakeAnalytics.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|Obter os registos disponíveis para a conta de DataLakeAnalytics.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Obter as métricas disponíveis para a conta de DataLakeAnalytics.|
|/accounts/read|Obter informações sobre uma conta de DataLakeAnalytics existente.|
|/accounts/storageAccounts/Containers/listSasTokens/action|Lista os tokens SAS para contentores de armazenamento de uma conta de armazenamento ligada de uma conta de DataLakeAnalytics.|
|/accounts/storageAccounts/Containers/read|Obter contentores de uma conta de armazenamento ligada de uma conta de DataLakeAnalytics.|
|/accounts/storageAccounts/delete|Desassociar uma conta de armazenamento de uma conta de DataLakeAnalytics.|
|/accounts/storageAccounts/read|Obter informações sobre a conta de armazenamento ligada de uma conta de DataLakeAnalytics.|
|/accounts/storageAccounts/write|Criar ou atualizar uma conta de armazenamento ligada de uma conta de DataLakeAnalytics.|
|/Accounts/TakeOwnerShip/Action|Conceder permissões para cancelar as tarefas submetidas por outros utilizadores.|
|/accounts/write|Criar ou atualizar uma conta de DataLakeAnalytics.|
|/locations/capability/read|Obter informações de capacidade de uma subscrição relativos à utilização DataLakeAnalytics.|
|/locations/checkNameAvailability/action|Verifique a disponibilidade de um nome de conta DataLakeAnalytics.|
|/locations/operationResults/read|Obter o resultado de uma operação de conta DataLakeAnalytics.|
|/operations/read|Obter operações disponíveis de DataLakeAnalytics.|
|/register/action|Registe a subscrição para DataLakeAnalytics.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Operação | Descrição |
|---|---|
|/accounts/delete|Elimine uma conta de DataLakeStore.|
|/accounts/enableKeyVault/action|Ative KeyVault para uma conta de DataLakeStore.|
|/accounts/firewallRules/delete|Elimine uma regra de firewall.|
|/accounts/firewallRules/read|Obter informações sobre uma regra de firewall.|
|/accounts/firewallRules/write|Criar ou atualizar uma regra de firewall.|
|/accounts/operationResults/read|Obter o resultado de uma operação de conta DataLakeStore.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Obter as definições de diagnóstico para a conta de DataLakeStore.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Criar ou atualizar as definições de diagnóstico para a conta de DataLakeStore.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|Obter os registos disponíveis para a conta de DataLakeStore.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Obter as métricas disponíveis para a conta de DataLakeStore.|
|/accounts/read|Obter informações sobre uma conta de DataLakeStore existente.|
|/Accounts/SuperUser/Action|Conceder Superutilizador no Data Lake Store quando concedido com Microsoft.Authorization/roleAssignments/write.|
|/accounts/trustedIdProviders/delete|Elimine um fornecedor de identidade fidedigna.|
|/accounts/trustedIdProviders/read|Obter informações sobre um fornecedor de identidade fidedigna.|
|/accounts/trustedIdProviders/write|Criar ou atualizar um fornecedor de identidade fidedigna.|
|/accounts/write|Criar ou atualizar uma conta de DataLakeStore.|
|/locations/capability/read|Obter informações de capacidade de uma subscrição relativos à utilização DataLakeStore.|
|/locations/checkNameAvailability/action|Verifique a disponibilidade de um nome de conta DataLakeStore.|
|/locations/operationResults/read|Obter o resultado de uma operação de conta DataLakeStore.|
|/operations/read|Obter operações disponíveis de DataLakeStore.|
|/register/action|Registe a subscrição para DataLakeStore.|

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

| Operação | Descrição |
|---|---|
|/locations/performanceTiers/read|Devolve a lista de escalões de desempenho disponíveis.|
|/performanceTiers/read|Devolve a lista de escalões de desempenho disponíveis.|
|/servers/delete|Elimina um servidor existente.|
|/servers/firewallRules/delete|Elimina uma regra de firewall existente.|
|/servers/firewallRules/read|Devolva a lista de firewall regras para um servidor ou obtém as propriedades para a regra de firewall especificada.|
|/servers/firewallRules/write|Cria uma regra de firewall com os parâmetros especificados ou atualiza uma regra existente.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição do recurso disagnostic|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Tipos de retorno de métricas que estão disponíveis para bases de dados|
|/servers/read|Devolva a lista de servidores ou obtém as propriedades para o servidor especificado.|
|/servers/recoverableServers/read|Devolver as informações de servidor MySQL recuperáveis|
|/servers/virtualNetworkRules/delete|Elimina uma regra de rede Virtual existente|
|/servers/virtualNetworkRules/read|Devolva a lista de rede virtual regras ou obtém as propriedades para a regra de rede virtual especificado.|
|/servers/virtualNetworkRules/write|Cria uma regra de rede virtual com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a regra de rede virtual especificado.|
|/servers/write|Cria um servidor com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o servidor especificado.|

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

| Operação | Descrição |
|---|---|
|/locations/performanceTiers/read|Devolve a lista de escalões de desempenho disponíveis.|
|/performanceTiers/read|Devolve a lista de escalões de desempenho disponíveis.|
|/servers/delete|Elimina um servidor existente.|
|/servers/firewallRules/delete|Elimina uma regra de firewall existente.|
|/servers/firewallRules/read|Devolva a lista de firewall regras para um servidor ou obtém as propriedades para a regra de firewall especificada.|
|/servers/firewallRules/write|Cria uma regra de firewall com os parâmetros especificados ou atualiza uma regra existente.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição do recurso disagnostic|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/servers/providers/Microsoft.Insights/logDefinitions/read|Tipos de retorno de registos que estão disponíveis para bases de dados|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Tipos de retorno de métricas que estão disponíveis para bases de dados|
|/servers/read|Devolva a lista de servidores ou obtém as propriedades para o servidor especificado.|
|/servers/recoverableServers/read|Devolver as informações de servidor PostgreSQL recuperáveis|
|/servers/virtualNetworkRules/delete|Elimina uma regra de rede Virtual existente|
|/servers/virtualNetworkRules/read|Devolva a lista de rede virtual regras ou obtém as propriedades para a regra de rede virtual especificado.|
|/servers/virtualNetworkRules/write|Cria uma regra de rede virtual com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a regra de rede virtual especificado.|
|/servers/write|Cria um servidor com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o servidor especificado.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Operação | Descrição |
|---|---|
|/checkNameAvailability/Action|Verifique o nome do IotHub se está disponível|
|/checkProvisioningServiceNameAvailability/Action|Verifique o nome do IotHub se está disponível|
|/ElasticPools/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/ElasticPools/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/elasticPools/iotHubTenants/Delete|Eliminar o recurso de inquilino IotHub|
|/ElasticPools/IotHubTenants/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/ElasticPools/IotHubTenants/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete|Eliminar grupo de consumidores de EventHub|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read|Obter os grupos de consumidores de EventHub|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write|Criar grupo de consumidores de EventHub|
|/elasticPools/iotHubTenants/exportDevices/Action|Dispositivos de exportação|
|/elasticPools/iotHubTenants/getStats/Read|Obtém o inquilino IotHub recursos estatísticas|
|/elasticPools/iotHubTenants/importDevices/Action|Importar dispositivos|
|/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action|Obtém a chave de inquilino IotHub|
|/elasticPools/iotHubTenants/jobs/Read|Obter tarefas detalhes submetidos numa fornecido IotHub|
|/elasticPools/iotHubTenants/listKeys/Action|Obtém as chaves de inquilino do IotHub|
|/ElasticPools/IotHubTenants/logDefinitions/read|Obtém as definições de registo disponível para o serviço de IotHub|
|/ElasticPools/IotHubTenants/metricDefinitions/read|Obtem as métricas disponíveis para o serviço de IotHub|
|/elasticPools/iotHubTenants/quotaMetrics/Read|Obter métricas de Quota|
|/elasticPools/iotHubTenants/Read|Obtém o recurso de inquilino IotHub|
|/elasticPools/iotHubTenants/routing/routes/$testall/Action|Testar uma mensagem de todas as rotas existentes|
|/elasticPools/iotHubTenants/routing/routes/$testnew/Action|Uma mensagem de um teste fornecido rota de teste|
|/elasticPools/iotHubTenants/routingEndpointsHealth/Read|Obtém o estado de funcionamento de todos os pontos finais de encaminhamento para um IotHub|
|/elasticPools/iotHubTenants/Write|Criar ou atualizar o recurso de inquilino IotHub|
|/ElasticPools/metricDefinitions/read|Obtem as métricas disponíveis para o serviço de IotHub|
|/iotHubs/certificates/generateVerificationCode/Action|Gerar código de verificação|
|/iotHubs/certificates/verify/Action|Verifique o recurso de certificado|
|/iotHubs/Delete|Eliminar recurso IotHub|
|/IotHubs/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/IotHubs/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/iotHubs/eventGridFilters/Delete|Elimina o filtro de eventos de grelha|
|/iotHubs/eventGridFilters/Read|Obtém o filtro de eventos de grelha|
|/iotHubs/eventGridFilters/Write|Criar um novo ou atualizar o filtro de grelha de evento existente|
|/iotHubs/eventHubEndpoints/consumerGroups/Delete|Eliminar grupo de consumidores de EventHub|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Obter os grupos de consumidores de EventHub|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Criar grupo de consumidores de EventHub|
|/iotHubs/exportDevices/Action|Dispositivos de exportação|
|/iotHubs/importDevices/Action|Importar dispositivos|
|/iotHubs/iotHubKeys/listkeys/Action|Obter a chave de IotHub para o nome fornecido|
|/iotHubs/iotHubStats/Read|Obter estatísticas IotHub|
|/iotHubs/jobs/Read|Obter tarefas detalhes submetidos numa fornecido IotHub|
|/iotHubs/listkeys/Action|Obter todas as chaves de IotHub|
|/IotHubs/logDefinitions/read|Obtém as definições de registo disponível para o serviço de IotHub|
|/IotHubs/metricDefinitions/read|Obtem as métricas disponíveis para o serviço de IotHub|
|/iotHubs/quotaMetrics/Read|Obter métricas de Quota|
|/iotHubs/Read|Obtém os recursos de IotHub|
|/iotHubs/routing/$testall/Action|Testar uma mensagem de todas as rotas existentes|
|/iotHubs/routing/$testnew/Action|Uma mensagem de um teste fornecido rota de teste|
|/iotHubs/routingEndpointsHealth/Read|Obtém o estado de funcionamento de todos os pontos finais de encaminhamento para um IotHub|
|/iotHubs/skus/Read|Obter Skus válidos de IotHub|
|/iotHubs/Write|Criar ou atualizar o recurso de IotHub|
|/operations/Read|Obter todas as operações de ResourceProvider|
|/provisioningServices/certificates/generateVerificationCode/Action|Gerar código de verificação|
|/provisioningServices/certificates/verify/Action|Verifique o recurso de certificado|
|/provisioningServices/Delete|Eliminar recurso IotDps|
|/provisioningServices/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/provisioningServices/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/provisioningServices/listkeys/Action|Obter todas as chaves de IotDps|
|/provisioningServices/logDefinitions/read|Obtém as definições de registo disponível para o serviço de aprovisionamento|
|/provisioningServices/metricDefinitions/read|Obtem as métricas disponíveis para o serviço de aprovisionamento|
|/provisioningServices/ProvisioningServiceKeys/listkeys/Action|Obter chaves IotDps para o nome da chave|
|/provisioningServices/Read|Obter o recurso de IotDps|
|/provisioningServices/skus/Read|Obter IotDps Skus válidos|
|/provisioningServices/Write|Criar recurso IotDps|
|/register/action|Registar a subscrição para o fornecedor de recursos do IotHub e permite a criação dos recursos de IotHub|
|/register/action|Registar a subscrição para o fornecedor de recursos do IotHub e permite a criação dos recursos de IotHub|
|/usages/Read|Obter detalhes de utilização de subscrição para este fornecedor.|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Operação | Descrição |
|---|---|
|/labCenters/delete|Elimine centros de laboratório.|
|/labCenters/read|Ler centros de laboratório.|
|/labCenters/write|Adiciona ou modifica os centros de laboratório.|
|/labs/artifactSources/armTemplates/read|Ler modelos do Gestor de recursos do azure.|
|/labs/artifactSources/artifacts/GenerateArmTemplate/action|Gera um modelo ARM para o artefacto indicado, carrega os ficheiros necessários para uma conta de armazenamento e valida o artefacto gerado.|
|/labs/artifactSources/artifacts/read|Ler os artefactos.|
|/labs/artifactSources/delete|Elimine as origens de artefactos.|
|/labs/artifactSources/read|Ler as origens de artefactos.|
|/labs/artifactSources/write|Adiciona ou modifica as origens de artefactos.|
|/labs/ClaimAnyVm/action|Uma máquina virtual claimable aleatória no laboratório de afirmações.|
|/labs/costs/read|Ler os custos.|
|/labs/costs/write|Adiciona ou modifica os custos.|
|/labs/CreateEnvironment/action|Crie máquinas virtuais no laboratório.|
|/labs/customImages/delete|Elimine imagens personalizadas.|
|/labs/customImages/read|Ler imagens personalizadas.|
|/labs/customImages/write|Adiciona ou modifica imagens personalizadas.|
|/labs/delete|Elimine laboratórios.|
|/labs/ExportResourceUsage/action|Exporta a utilização de recursos de laboratório para uma conta de armazenamento|
|/labs/formulas/delete|Elimine as fórmulas.|
|/labs/formulas/read|Ler as fórmulas.|
|/labs/formulas/write|Adiciona ou modifica as fórmulas.|
|/labs/galleryImages/read|Ler imagens gallery.|
|/labs/GenerateUploadUri/action|Gere um URI de carregamento de imagens do disco personalizado para um laboratório.|
|/labs/ImportVirtualMachine/action|Importe uma máquina virtual para um laboratório diferentes.|
|/labs/ListVhds/action|Imagens de disco de lista disponível para criação de imagens personalizadas.|
|/labs/notificationChannels/delete|Elimine notificationchannels.|
|/labs/notificationChannels/Notify/action|Envie notificações para canal fornecido.|
|/labs/notificationChannels/read|Ler notificationchannels.|
|/labs/notificationChannels/write|Adiciona ou modifica notificationchannels.|
|/labs/policySets/EvaluatePolicies/action|Avalia a política de laboratório.|
|/labs/policySets/policies/delete|Elimine políticas.|
|/labs/policySets/policies/read|Políticas de leitura.|
|/labs/policySets/policies/write|Adicionar ou modificar as políticas.|
|/labs/read|Laboratórios de leitura.|
|/labs/schedules/delete|Elimine agendas.|
|/labs/schedules/Execute/action|Execute uma agenda.|
|/labs/schedules/ListApplicable/action|Apresenta uma lista de todos os agendamentos aplicáveis|
|/labs/schedules/read|As agendas de leitura.|
|/labs/schedules/write|Adiciona ou modifica as agendas.|
|/labs/serviceRunners/delete|Elimine runners de serviço.|
|/labs/serviceRunners/read|Ler runners de serviço.|
|/labs/serviceRunners/write|Adiciona ou modifica runners de serviço.|
|/labs/users/delete|Elimine perfis de utilizador.|
|/labs/users/disks/Attach/action|Ligar e criar a concessão do disco para a máquina virtual.|
|/labs/users/disks/delete|Elimine os discos.|
|/labs/users/disks/Detach/action|Desligue e quebra a concessão dos discos ligados à máquina virtual.|
|/labs/users/disks/read|Leitura de discos.|
|/labs/users/disks/write|Adiciona ou modifica os discos.|
|/labs/users/environments/delete|Elimine ambientes.|
|/labs/users/environments/read|Ambientes de leitura.|
|/labs/users/environments/write|Adiciona ou modifica os ambientes.|
|/labs/users/read|Perfis de utilizador de leitura.|
|/labs/users/secrets/delete|Elimine segredos.|
|/labs/users/secrets/read|Ler os segredos.|
|/labs/users/secrets/write|Adiciona ou modifica os segredos.|
|/labs/users/serviceFabrics/delete|Elimine recursos de infraestrutura de serviço.|
|/labs/users/serviceFabrics/ListApplicableSchedules/action|Apresenta uma lista de todos os agendamentos aplicáveis|
|/labs/users/serviceFabrics/read|Ler os recursos de infraestrutura de serviço.|
|/labs/users/serviceFabrics/schedules/delete|Elimine agendas.|
|/labs/users/serviceFabrics/schedules/Execute/action|Execute uma agenda.|
|/labs/users/serviceFabrics/schedules/read|As agendas de leitura.|
|/labs/users/serviceFabrics/schedules/write|Adiciona ou modifica as agendas.|
|/labs/users/serviceFabrics/Start/action|Inicie uma infraestrutura de serviço.|
|/labs/users/serviceFabrics/Stop/action|Parar uma infraestrutura de serviço|
|/labs/users/serviceFabrics/write|Adiciona ou modifica os recursos de infraestrutura de serviço.|
|/labs/users/write|Adiciona ou modifica os perfis de utilizador.|
|/labs/virtualMachines/AddDataDisk/action|Anexe um disco de dados nova ou existente à máquina virtual.|
|/labs/virtualMachines/ApplyArtifacts/action|Aplicam-se de artefactos a máquina virtual.|
|/labs/virtualMachines/Claim/action|Assumir a propriedade de uma máquina virtual existente|
|/labs/virtualMachines/delete|Elimine as máquinas virtuais.|
|/labs/virtualMachines/DetachDataDisk/action|Desligar o disco da máquina virtual.|
|/labs/virtualMachines/ListApplicableSchedules/action|Apresenta uma lista de todos os agendamentos aplicáveis|
|/labs/virtualMachines/read|Máquinas virtuais de leitura.|
|/labs/virtualMachines/Restart/action|Reinicie uma máquina virtual.|
|/labs/virtualMachines/schedules/delete|Elimine agendas.|
|/labs/virtualMachines/schedules/Execute/action|Execute uma agenda.|
|/labs/virtualMachines/schedules/read|As agendas de leitura.|
|/labs/virtualMachines/schedules/write|Adiciona ou modifica as agendas.|
|/labs/virtualMachines/Start/action|Inicie uma máquina virtual.|
|/labs/virtualMachines/Stop/action|Parar uma máquina virtual|
|/labs/virtualMachines/TransferDisks/action|Transferir a propriedade dos discos de dados de máquina virtual a próprio|
|/labs/virtualMachines/UnClaim/action|Propriedade de versão de uma máquina virtual existente|
|/labs/virtualMachines/write|Adiciona ou modifica máquinas virtuais.|
|/labs/virtualNetworks/delete|Elimine redes virtuais.|
|/labs/virtualNetworks/read|Ler as redes virtuais.|
|/labs/virtualNetworks/write|Adiciona ou modifica as redes virtuais.|
|/labs/write|Adiciona ou modifica laboratórios.|
|/locations/operations/read|Operações de leitura.|
|/register/action|Regista a subscrição do|
|/schedules/delete|Elimine agendas.|
|/Schedules/execute/Action|Execute uma agenda.|
|/schedules/read|As agendas de leitura.|
|/schedules/Retarget/action|ID de recurso de destino de uma agenda de atualizações|
|/schedules/write|Adiciona ou modifica as agendas.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Operação | Descrição |
|---|---|
|/databaseAccountNames/read|Verifica a disponibilidade do nome.|
|/databaseAccounts/changeResourceGroup/action|Alterar o grupo de recursos de uma conta de base de dados|
|/databaseAccounts/databases/collections/metricDefinitions/read|Lê a coleção de definições de métrica.|
|/databaseAccounts/databases/collections/metrics/read|Lê as métricas de coleção.|
|/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read|Base de dados conta partição chave as métricas de nível de leitura|
|/databaseAccounts/databases/collections/partitions/metrics/read|Ler métricas de nível da base de dados conta partição|
|/databaseAccounts/databases/collections/partitions/usages/read|Ler a conta de base de dados utilizações de nível de partição|
|/databaseAccounts/databases/collections/usages/read|Lê as utilizações de coleção.|
|/databaseAccounts/databases/metricDefinitions/read|Lê as definições de métrica de base de dados|
|/databaseAccounts/databases/metrics/read|Lê as métricas de base de dados.|
|/databaseAccounts/databases/usages/read|Lê as utilizações de base de dados.|
|/databaseAccounts/delete|Elimina as contas de base de dados.|
|/databaseAccounts/failoverPriorityChange/action|Alterar as prioridades de ativação pós-falha de regiões de uma conta de base de dados. Isto é utilizado para efetuar a operação de ativação pós-falha manual|
|/databaseAccounts/listConnectionStrings/action|Obter as cadeias de ligação para uma conta de base de dados|
|/databaseAccounts/listKeys/action|Lista de chaves de uma conta de base de dados|
|/databaseAccounts/metricDefinitions/read|Lê a base de dados definições de métrica de conta.|
|/databaseAccounts/metrics/read|Lê as métricas de conta de base de dados.|
|/databaseAccounts/operationResults/read|Ler o estado da operação assíncrona|
|/databaseAccounts/percentile/metrics/read|Métricas de latência de leitura|
|/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read|Ler métricas de latência para uma região de origem e de destino específica|
|/databaseAccounts/percentile/targetRegion/metrics/read|Métricas de latência de leitura para uma região de destino específico|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read|Obtém o catageries de registo disponível para a conta de base de dados|
|/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para a conta de base de dados|
|/databaseAccounts/read|Lê uma conta de base de dados.|
|/databaseAccounts/readonlykeys/action|Lê a base de dados de chaves de conta de só de leitura.|
|/databaseAccounts/readonlykeys/read|Lê a base de dados de chaves de conta de só de leitura.|
|/databaseAccounts/regenerateKey/action|Rodar chaves de uma conta de base de dados|
|/databaseAccounts/region/databases/collections/metrics/read|Lê as métricas de coleção regional.|
|/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read|Base de dados regionais conta partição chave as métricas de nível de leitura|
|/databaseAccounts/region/databases/collections/partitions/metrics/read|Ler métricas de nível da base de dados regionais conta partição|
|/databaseAccounts/region/databases/collections/partitions/read|As partições de conta de base de dados de uma coleção de leitura|
|/databaseAccounts/region/metrics/read|Lê as métricas de conta de base de dados e a região.|
|/databaseAccounts/usages/read|Lê a base de dados utilizações de conta.|
|/databaseAccounts/write|Atualize contas de base de dados.|
|/locations/deleteVirtualNetworkOrSubnets/action|Notifica Microsoft.DocumentDB que está a ser eliminado VirtualNetwork ou sub-rede|
|/operationResults/read|Ler o estado da operação assíncrona|
|/operations/read|Operações de leitura disponíveis para a Microsoft DocumentDB |
|/register/action| Registar o fornecedor de recursos do Microsoft DocumentDB para a subscrição|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Operação | Descrição |
|---|---|
|/checkDomainAvailability/Action|Verifique se um domínio está disponível para a compra|
|/domains/Delete|Elimine um domínio existente.|
|/domains/domainownershipidentifiers/Delete|Eliminar o identificador de propriedade|
|/domains/domainownershipidentifiers/Read|Identificadores de propriedade de lista|
|/domains/domainownershipidentifiers/Read|Obter o identificador de propriedade|
|/domains/domainownershipidentifiers/Write|Criar ou atualizar o identificador|
|/domains/operationresults/Read|Obter uma operação de domínio|
|/domains/operations/Read|Lista todas as operações de registo de domínios do serviço de aplicações|
|/domains/Read|Obter a lista de domínios|
|/domains/Read|Obter o domínio|
|/domains/renew/Action|Renove um domínio existente.|
|/domains/Write|Adicionar um novo domínio ou atualizar um já existente|
|/generateSsoRequest/Action|Gere um pedido de assinatura no Centro de controlo de domínio.|
|/listDomainRecommendations/Action|Obter as recomendações de domínio de lista com base em palavras-chave|
|/register/action|Registar o fornecedor de recursos Microsoft Domains para a subscrição|
|/topLevelDomains/listAgreements/Action|Ação de contrato de lista|
|/topLevelDomains/Read|Obter toplevel domínios|
|/topLevelDomains/Read|Obter toplevel domínio|
|/validateDomainRegistrationInformation/Action|Validar o objeto de compra de domínio sem submetê-la|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Operação | Descrição |
|---|---|
|/lcsprojects/clouddeployments/read|Apresentar as implementações de avaliação do Microsoft Dynamics AX 2012 R3 num projeto dos serviços de ciclo de vida do Microsoft Dynamics que pertençam a um utilizador|
|/lcsprojects/clouddeployments/write|Crie implementação de avaliação do Microsoft Dynamics AX 2012 R3 num projeto dos serviços de ciclo de vida do Microsoft Dynamics que pertençam a um utilizador. Implementações podem ser geridas a partir do portal de gestão do Azure|
|/lcsprojects/connectors/read|Conectores de leitura que pertençam a um projeto de serviços de ciclo de vida do Microsoft Dynamics|
|/lcsprojects/connectors/write|Criar e atualizar os conectores que pertençam a um projeto de serviços de ciclo de vida do Microsoft Dynamics|
|/lcsprojects/delete|Eliminar projetos de serviços de ciclo de vida do Microsoft Dynamics que pertencem ao utilizador|
|/lcsprojects/read|Apresentar os projetos de serviços de ciclo de vida do Microsoft Dynamics que pertençam a um utilizador|
|/lcsprojects/write|Criar e atualizar os projetos de serviços de ciclo de vida do Microsoft Dynamics que pertencem ao utilizador. Os nome e descrição propriedades só podem ser atualizadas. A subscrição e localização associadas ao projeto não podem ser atualizadas após a criação|

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

| Operação | Descrição |
|---|---|
|/eventSubscriptions/delete|Eliminar um eventSubscription|
|/eventSubscriptions/getFullUrl/action|Obter o url completo para a subscrição de evento|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico para subscrições de eventos|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para subscrições de eventos|
|/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para eventSubscriptions|
|/eventSubscriptions/read|Ler um eventSubscription|
|/eventSubscriptions/write|Criar ou atualizar um eventSubscription|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico para tópicos|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para tópicos|
|/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para tópicos|
|/register/action|Regista o eventSubscription para o fornecedor de recursos EventGrid e permite a criação de subscrições de grelha de eventos.|
|/topics/delete|Eliminar um tópico|
|/topics/listKeys/action|Lista de chaves para o tópico|
|/topics/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico para tópicos|
|/topics/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para tópicos|
|/topics/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para tópicos|
|/topics/read|Ler um tópico|
|/topics/regenerateKey/action|Voltar a gerar chave para o tópico|
|/topics/write|Criar ou atualizar um tópico|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Operação | Descrição |
|---|---|
|/checkNameAvailability/action|Verifica a disponibilidade do espaço de nomes em determinada subscrição.|
|/checkNamespaceAvailability/action|Verifica a disponibilidade do espaço de nomes em determinada subscrição. Esta API foi preterida utilize CheckNameAvailabiltiy em vez disso.|
|/namespaces/authorizationRules/action|Regra de autorização de espaço de nomes de atualizações. Esta API está depricated. Para atualizar a regra de autorização de espaço de nomes em vez disso, utilize uma chamada PUT... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/authorizationRules/delete|Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido. |
|/namespaces/authorizationRules/listkeys/action|Obter Cadeia de Ligação para o Espaço de nomes|
|/namespaces/authorizationRules/read|Obter a lista de descrição das Regras de Autorização de Espaços de nomes.|
|/namespaces/authorizationRules/regenerateKeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/authorizationRules/write|Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas.|
|/namespaces/Delete|Eliminar Recurso de Espaço de nomes|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Obtém a autorização de chaves de regras para o espaço de nomes de principal de recuperação após desastre|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Obter as regras de autorização do desastre recuperação primário espaço de nomes|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Desativa a Recuperação Após Desastre e para a replicação de alterações de espaço de nomes principais para secundários.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Verificações de disponibilidade do espaço de nomes de alias em dada subscrição.|
|/namespaces/disasterRecoveryConfigs/delete|Elimina a configuração de recuperação após desastre associada com o espaço de nomes. Esta operação só pode ser invocada através do espaço de nomes primário.|
|/namespaces/disasterRecoveryConfigs/failover/action|Invoca uma ativação pós-falha GEO DR e reconfigura o alias de espaço de nomes para apontar para o espaço de nomes secundário.|
|/namespaces/disasterRecoveryConfigs/read|Obtém a configuração da Recuperação Após Desastre associada ao espaço de nomes.|
|/namespaces/disasterRecoveryConfigs/write|Cria ou Atualiza a configuração da Recuperação Após Desastre associada ao espaço de nomes.|
|/namespaces/eventhubs/authorizationRules/action|Operação atualizar EventHub. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT.|
|/namespaces/eventhubs/authorizationRules/delete|Operação para eliminar as regras de autorização de EventHub|
|/namespaces/eventhubs/authorizationRules/listkeys/action|Obter a cadeia de ligação a EventHub|
|/namespaces/eventhubs/authorizationRules/read| Obter a lista de regras de autorização de EventHub|
|/namespaces/eventhubs/authorizationRules/regenerateKeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/eventhubs/authorizationRules/write|Criar regras de autorização de EventHub e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados.|
|/namespaces/eventHubs/consumergroups/Delete|Operação para eliminar recurso ConsumerGroup|
|/namespaces/eventHubs/consumergroups/read|Obter a lista de descrições de recurso ConsumerGroup|
|/namespaces/eventHubs/consumergroups/write|Criar ou atualizar ConsumerGroup propriedades.|
|/namespaces/eventhubs/Delete|Operação para eliminar o recurso de EventHub|
|/namespaces/eventhubs/read|Obter a lista de descrições de recursos de EventHub|
|/namespaces/eventhubs/write|Criar ou propriedades de EventHub de atualização.|
|/namespaces/messagingPlan/read|Obtém o plano de serviço de mensagens para um espaço de nomes. Esta API foi preterida. As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/messagingPlan/write|Atualiza o serviço de mensagens planear um espaço de nomes. Esta API foi preterida. As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/operationresults/read|Obter o estado da operação de Espaço de nomes|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/read|Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/write|Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Obter a lista de registos de espaço de nomes descrições de recursos|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de descrições de recursos de métricas de espaço de nomes|
|/namespaces/read|Obter a lista de Descrição de Recursos de Espaço de nomes|
|/namespaces/write|Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas.|
|/operations/read|Obter operações|
|/register/action|Regista a subscrição do fornecedor de recursos do EventHub e ativa a criação de recursos do EventHub|
|/sku/read|Obter a lista de descrições de recursos de Sku|
|/sku/regions/read|Obter a lista de descrições de recurso SkuRegions|
|/unregister/action|Regista o Fornecedor de Recursos do EventHub|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Operação | Descrição |
|---|---|
|/features/read|Obtém as funcionalidades de uma subscrição.|
|/providers/features/read|Obtém a funcionalidade de uma subscrição de um fornecedor de recursos específico.|
|/providers/features/register/action|Regista a funcionalidade para uma subscrição de um fornecedor de recursos específico.|
|/providers/features/unregister/action|Anula o registo da funcionalidade para uma subscrição de um fornecedor de recursos específico.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Operação | Descrição |
|---|---|
|/clusters/changerdpsetting/action|Alteração da definição de RDP de Cluster do HDInsight|
|/clusters/configurations/action|Atualizar a configuração de Cluster do HDInsight|
|/clusters/configurations/read|Obter configurações de Cluster do HDInsight|
|/clusters/delete|Eliminar um Cluster do HDInsight|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico para o recurso de Cluster do HDInsight|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o recurso de Cluster do HDInsight|
|/clusters/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o Cluster do HDInsight|
|/clusters/read|Obter detalhes sobre o Cluster do HDInsight|
|/clusters/roles/resize/action|Redimensionar um Cluster do HDInsight|
|/clusters/write|Criar ou atualizar o Cluster do HDInsight|
|/locations/capabilities/read|Obter as funções da subscrição|
|/locations/checkNameAvailability/read|Verificar Disponibilidade do Nome|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Operação | Descrição |
|---|---|
|/jobs/delete|Elimina uma tarefa existente.|
|/jobs/listBitLockerKeys/action|Obtém as chaves do BitLocker para a tarefa especificada.|
|/jobs/read|Obtém as propriedades para a tarefa especificada ou devolve a lista de tarefas.|
|/jobs/write|Cria uma tarefa com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a tarefa especificada.|
|/locations/read|Obtém as propriedades para a localização especificada ou devolve a lista de localizações.|
|/register/action|Regista a subscrição para o fornecedor de recursos de importação/exportação e permite a criação de tarefas de importação/exportação.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Operação | Descrição |
|---|---|
|/ActionGroups/Delete|A eliminar um grupo de ação|
|/ActionGroups/Read|A ler um grupo de ação|
|/ActionGroups/Write|A escrever um grupo de ação|
|/ActivityLogAlerts/Activated/Action|Foi acionado o Alerta do Registo de Atividade|
|/ActivityLogAlerts/Delete|A eliminar um alerta do registo de atividade|
|/ActivityLogAlerts/Read|A ler um alerta do registo de atividade|
|/ActivityLogAlerts/Write|A ler um alerta do registo de atividade|
|/AlertRules/Activated/Action|Regra de Alerta ativada|
|/AlertRules/Delete|Eliminar uma configuração de regra de alerta|
|/AlertRules/Incidents/Read|Ler uma configuração de incidente da regra de alerta|
|/AlertRules/Read|Ler uma configuração de regra de alerta|
|/AlertRules/Resolved/Action|Regra de Alerta resolvida|
|/ AlertRules/limitadas/ação|A regra de alerta está otimizada|
|/AlertRules/Write|Escrever numa configuração de regra de alerta|
|/AutoscaleSettings/Delete|Eliminar a configuração de uma definição de dimensionamento automático|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Ler definições de métricas|
|/AutoscaleSettings/Read|Ler a configuração de uma definição de dimensionamento automático|
|/AutoscaleSettings/Scaledown/Action|Operação de redução do Dimensionamento Automático|
|/AutoscaleSettings/Scaleup/Action|Operação de aumento do Dimensionamento Automático|
|/AutoscaleSettings/Write|Escrever numa configuração de definição de dimensionamento automático|
|/Components/AnalyticsItems/Delete|Eliminar um item de análise do Application Insights|
|/Components/AnalyticsItems/Read|Ler um item de análise do Application Insights|
|/Components/AnalyticsItems/Write|Escrever um item de análise do Application Insights|
|/Components/AnalyticsTables/Action|Ação de tabela do Application Insights analytics|
|/Components/AnalyticsTables/Delete|Eliminar um Application Insights analytics tabela esquema|
|/Components/AnalyticsTables/Read|Ler um Application Insights analytics tabela esquema|
|/Components/AnalyticsTables/Write|Escrever um Application Insights analytics tabela esquema|
|/Components/Annotations/Delete|A eliminação de uma anotação do Application Insights|
|/Components/Annotations/Read|Ler uma anotação do Application Insights|
|/ Componentes/anotações/escrita|Escrever uma anotação do Application Insights|
|/ Componentes/Api/leitura|Leitura de dados de componentes do Application Insights API|
|/Components/ApiKeys/Action|Gerar uma chave de API do Application Insights|
|/Components/ApiKeys/Delete|Eliminar uma chave de API do Application Insights|
|/Components/ApiKeys/Read|Ler uma chave de API do Application Insights|
|/Components/BillingPlanForComponent/Read|Ler um plano de faturação para o componente do Application Insights|
|/Components/CurrentBillingFeatures/Read|Ler as funcionalidades de faturação atuais para o componente do Application Insights|
|/Components/CurrentBillingFeatures/Write|Escrever funcionalidades de faturação atuais para o componente do Application Insights|
|/Components/DefaultWorkItemConfig/Read|Ler uma configuração de integração do Application Insights predefinida ALM|
|/Components/Delete|A eliminar uma configuração de componentes de informações da aplicação|
|/Components/ExportConfiguration/Action|Ação de definições de exportação do Application Insights|
|/Components/ExportConfiguration/Delete|A eliminar Application Insights exportar definições|
|/Components/ExportConfiguration/Read|Leitura Application Insights exportar definições|
|/Components/ExportConfiguration/Write|Escrita Application Insights exportar definições|
|/Components/ExtendQueries/Read|Componente de leitura Application Insights expandido os resultados da consulta|
|/Components/Favorites/Delete|A eliminar o favorito do Application Insights|
|/ Componentes/Favoritos/leitura|Ler um favorito do Application Insights|
|/ Componentes/Favoritos/escrita|Escrever um favorito do Application Insights|
|/Components/FeatureCapabilities/Read|Capacidades de funcionalidade do componente de leitura Application Insights|
|/Components/GetAvailableBillingFeatures/Read|Funcionalidades de faturação leitura Application Insights componente disponíveis|
|/Components/GetToken/Read|Ler um token de componentes do Application Insights|
|/Components/ListMigrationDate/Action|Data de migração de subscrição back Get|
|/Components/ListMigrationDate/Read|Data de migração de subscrição de back-Get|
|/Components/MetricDefinitions/Read|Ler as definições de métrica de componentes do Application Insights|
|/ Componentes/métricas/leitura|Métricas de componente de leitura Application Insights|
|/Components/MigrateToNewpricingModel/Action|Migrar a subscrição para o novo modelo de preços|
|/ Componentes / / ação mover|Mover um componente do Application Insights para outro grupo de recursos ou subscrição|
|/Components/MyAnalyticsItems/Delete|Eliminar um item de pessoal de análise do Application Insights|
|/Components/MyAnalyticsItems/Read|Ler um item de pessoal de análise do Application Insights|
|/Components/MyAnalyticsItems/Write|Escrever um item de pessoal de análise do Application Insights|
|/Components/MyFavorites/Read|Ler um favorito do Application Insights pessoal|
|/Components/PricingPlans/Read|Ler um componente do Application Insights plano de preços|
|/Components/PricingPlans/Write|Escrever um componente do Application Insights plano de preços|
|/Components/ProactiveDetectionConfigs/Read|Configuração de deteção proativa de leitura Application Insights|
|/Components/ProactiveDetectionConfigs/Write|Escrita de configuração de deteção proativa do Application Insights|
|/Components/providers/Microsoft.Insights/MetricDefinitions/Read|Ler definições de métricas|
|/Components/QuotaStatus/Read|Ao ler o estado de quota de componentes do Application Insights|
|/ Componentes/leitura|A ler uma configuração de componentes de informações da aplicação|
|/Components/RollbackToLegacyPricingModel/Action|Subscrição de reversão para legado do modelo de preços|
|/Components/SyntheticMonitorLocations/Read|Localizações de teste Web leitura Application Insights|
|/Components/WorkItemConfigs/Delete|Eliminar uma configuração de integração do Application Insights ALM|
|/Components/WorkItemConfigs/Read|Ler uma configuração de integração do Application Insights ALM|
|/Components/WorkItemConfigs/Write|Escrever uma configuração de integração do Application Insights ALM|
|/ Componentes/escrita|A escrever para uma configuração de componentes de informações da aplicação|
|/DiagnosticSettings/Delete|A eliminar a configuração de definições de diagnóstico|
|/DiagnosticSettings/Read|A ler uma configuração de definições de diagnóstico|
|/DiagnosticSettings/Write|A escrever para a configuração de definições de diagnóstico|
|/EventCategories/Read|A ler uma categoria de eventos|
|/eventtypes/digestevents/Read|Ler resumo do tipo de eventos de gestão|
|/eventtypes/values/Read|Ler valores do tipo de eventos de gestão|
|/ExtendedDiagnosticSettings/Delete|A eliminar a configuração de definições de diagnóstico avançadas|
|/ExtendedDiagnosticSettings/Read|A ler uma configuração das definições de diagnóstico avançadas|
|/ExtendedDiagnosticSettings/Write|A escrever para a configuração das definições de diagnóstico avançadas|
|/LogDefinitions/Read|Ler definições de registo|
|/LogProfiles/Delete|Eliminar configuração dos perfis de registo|
|/LogProfiles/Read|Ler perfis de registo|
|/LogProfiles/Write|A escrever para uma configuração de perfil de registo|
|/MetricAlerts/Delete|A eliminar um alerta de métrica|
|/MetricAlerts/Read|A ler um alerta de métrica|
|/MetricAlerts/Write|A escrever um alerta de métrica|
|/MetricDefinitions/Microsoft.Insights/Read|Ler definições de métricas|
|/MetricDefinitions/providers/Microsoft.Insights/Read|Ler definições de métricas|
|/MetricDefinitions/Read|Ler definições de métricas|
|/Metrics/providers/Metrics/Read|Ler métricas|
|/ Métricas/leitura|Ler métricas|
|/ Métricas/escrita|Métricas de escrita|
|/Operations/Read|A ler operações|
|/ Register/ação|Registar o fornecedor de microsoft insights|
|/ Inquilinos/Register/ação|Iniciar o fornecedor de informações da Microsoft|
|/ Anular o registo/ação|Registar o fornecedor de microsoft insights|
|/Webtests/Delete|A eliminar uma configuração de webtest|
|/Webtests/GetToken/Read|Ler um token de teste Web|
|/Webtests/MetricDefinitions/Read|Ler as definições de métrica um teste Web|
|/Webtests/Metrics/Read|Ler uma métrica de teste Web|
|/Webtests/Read|A ler uma configuração de webtest|
|/Webtests/Write|A escrever para uma configuração de webtest|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Operação | Descrição |
|---|---|
|/checkNameAvailability/read|Verifica se um nome de cofre de chaves é válido e se não está em utilização|
|/deletedVaults/read|Visualizar as propriedades dos cofres de chaves eliminados de forma recuperável|
|/hsmPools/delete|Eliminar um conjunto de HSM|
|/hsmPools/joinVault/action|Associar um cofre de chaves a um conjunto de HSM|
|/hsmPools/read|Visualizar as propriedades de um conjunto HSM|
|/hsmPools/write|Criar um novo conjunto de HSM de atualização das propriedades de um conjunto de HSM existente|
|/locations/deletedVaults/purge/action|Remover um cofre de chaves eliminado de forma recuperável|
|/locations/deletedVaults/read|Visualizar as propriedades de um cofre de chaves eliminado de forma recuperável|
|/locations/deleteVirtualNetworkOrSubnets/action|Notifica o Microsoft.KeyVault de que uma rede virtual ou sub-rede está a ser eliminada|
|/locations/operationResults/read|Verificar o resultado de uma operação de execução longa|
|/operations/read|Lista as operações disponíveis no fornecedor de recursos Microsoft.KeyVault|
|/register/action|Regista uma subscrição|
|/unregister/action|Cancela o registo de uma subscrição|
|/vaults/accessPolicies/write|Atualize uma política de acesso existente através da intercalação ou substituição, ou adicione uma nova política de acesso ao cofre.|
|/vaults/delete|Eliminar um cofre de chaves|
|/vaults/deploy/action|Permite o acesso a segredos num cofre de chaves ao implementar os recursos do Azure|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Read|Obtém a definição de diagnóstico do recurso|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/vaults/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para um cofre de chaves|
|/vaults/providers/Microsoft.Insights/metricDefinitions/read|Obtém as métricas disponíveis de um cofre de chaves|
|/vaults/read|Ver as propriedades de um cofre de chaves|
|/vaults/secrets/read|Ver as propriedades de um segredo, mas não o seu valor|
|/vaults/secrets/write|Crie um novo segredo ou atualize o valor de um segredo existente|
|/vaults/write|Criar um novo cofre de chaves ou atualizar as propriedades de um cofre de chaves existente|

## <a name="microsoftlabservices"></a>Microsoft.LabServices

| Operação | Descrição |
|---|---|
|/labAccounts/CreateLab/action|Crie um laboratório numa conta de laboratório.|
|/labAccounts/delete|Elimine contas de laboratório.|
|/labAccounts/labs/delete|Elimine laboratórios.|
|/labAccounts/labs/environmentSettings/delete|Elimine a definição de ambiente.|
|/labAccounts/labs/environmentSettings/environments/delete|Elimine ambientes.|
|/labAccounts/labs/environmentSettings/environments/read|Ambientes de leitura.|
|/labAccounts/labs/environmentSettings/environments/write|Adiciona ou modifica os ambientes.|
|/labAccounts/labs/environmentSettings/Publish/action|Aprovisiona/deprovisions necessários recursos para um definição de ambiente com base no estado atual da definição de ambiente de laboratório /.|
|/labAccounts/labs/environmentSettings/read|Definição de ambiente de leitura.|
|/labAccounts/labs/environmentSettings/write|Adicionar ou modificar a definição do ambiente.|
|/labAccounts/labs/read|Laboratórios de leitura.|
|/labAccounts/labs/users/delete|Elimine utilizadores.|
|/labAccounts/labs/users/read|Os utilizadores de leitura.|
|/labAccounts/labs/users/write|Adiciona ou modifica os utilizadores.|
|/labAccounts/labs/write|Adiciona ou modifica laboratórios.|
|/labAccounts/read|Ler as contas de laboratório.|
|/labAccounts/write|Adiciona ou modifica contas de laboratório.|
|/locations/operations/read|Operações de leitura.|
|/register/action|Regista a subscrição do|

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

| Operação | Descrição |
|---|---|
|/accounts/delete|Eliminar uma localização com base dos serviços de conta.|
|/accounts/listKeys/action|Listar chaves de conta de serviços com base da localização|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para a localização com base em contas de serviço|
|/accounts/read|Obter uma localização com base dos serviços de conta.|
|/accounts/regenerateKey/action|Gerar a nova localização com base em serviços primário ou secundário chave da conta|
|/accounts/write|Criar ou atualizar uma conta de serviços com base da localização.|
|/register/action|Registar o fornecedor|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Operação | Descrição |
|---|---|
|/integrationAccounts/agreements/delete|Elimina o contrato na conta de integração.|
|/integrationAccounts/agreements/listContentCallbackUrl/action|Obtém o URL de chamada de retorno para o conteúdo de contrato na conta de integração.|
|/integrationAccounts/agreements/read|Lê o contrato na conta de integração.|
|/integrationAccounts/agreements/write|Cria ou atualiza o contrato na conta de integração.|
|/integrationAccounts/assemblies/delete|Elimina a assemblagem na conta de integração.|
|/integrationAccounts/assemblies/listContentCallbackUrl/action|Obtém o URL de chamada de retorno para o conteúdo de assemblagem na conta de integração.|
|/integrationAccounts/assemblies/read|Lê a assemblagem na conta de integração.|
|/integrationAccounts/assemblies/write|Cria ou atualiza a assemblagem na conta de integração.|
|/integrationAccounts/batchConfigurations/delete|Elimina a configuração de batch na conta de integração.|
|/integrationAccounts/batchConfigurations/read|Lê a configuração de batch na conta de integração.|
|/integrationAccounts/batchConfigurations/write|Cria ou atualiza a configuração de batch na conta de integração.|
|/integrationAccounts/certificates/delete|Elimina o certificado na conta de integração.|
|/integrationAccounts/certificates/read|Lê o certificado na conta de integração.|
|/integrationAccounts/certificates/write|Cria ou atualiza o certificado na conta de integração.|
|/integrationAccounts/delete|Elimina a conta de integração.|
|/integrationAccounts/listCallbackUrl/action|Obtém o URL de chamada de retorno para a conta de integração.|
|/integrationAccounts/listKeyVaultKeys/action|Obtém as chaves num cofre de chaves.|
|/integrationAccounts/logTrackingEvents/action|Regista os eventos de controlo na conta de integração.|
|/integrationAccounts/maps/delete|Elimina o mapa na conta de integração.|
|/integrationAccounts/maps/listContentCallbackUrl/action|Obtém o URL de chamada de retorno para o conteúdo do mapa na conta de integração.|
|/integrationAccounts/maps/read|Lê o mapa na conta de integração.|
|/integrationAccounts/maps/write|Cria ou atualiza o mapa na conta de integração.|
|/integrationAccounts/partners/delete|Elimina o parceiro de conta de integração.|
|/integrationAccounts/partners/listContentCallbackUrl/action|Obtém o URL de chamada de retorno de conteúdo de parceiro de conta de integração.|
|/integrationAccounts/partners/read|Lê o parter na conta de integração.|
|/integrationAccounts/partners/write|Cria ou atualiza o parceiro de conta de integração.|
|/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read|Lê as definições de registo da Conta de Integração.|
|/integrationAccounts/read|Lê a conta de integração.|
|/integrationAccounts/regenerateAccessKey/action|Regenera os segredos da chave de acesso.|
|/integrationAccounts/schemas/delete|Elimina o esquema na conta de integração.|
|/integrationAccounts/schemas/listContentCallbackUrl/action|Obtém o URL de chamada de retorno para o conteúdo de esquema na conta de integração.|
|/integrationAccounts/schemas/read|Lê o esquema na conta de integração.|
|/integrationAccounts/schemas/write|Cria ou atualiza o esquema na conta de integração.|
|/integrationAccounts/sessions/delete|Elimina a sessão na conta de integração.|
|/integrationAccounts/sessions/read|Lê a configuração de batch na conta de integração.|
|/integrationAccounts/sessions/write|Cria ou atualiza a sessão na conta de integração.|
|/integrationAccounts/write|Cria ou atualiza a conta de integração.|
|/locations/workflows/validate/action|Valida o fluxo de trabalho.|
|/operations/read|Obtém a operação.|
|/register/action|Regista o fornecedor de recursos de Microsoft.Logic para uma determinada subscrição.|
|/workflows/accessKeys/delete|Elimina a chave de acesso.|
|/workflows/accessKeys/list/action|Lista os segredos da chave de acesso.|
|/workflows/accessKeys/read|Lê a chave de acesso.|
|/workflows/accessKeys/regenerate/action|Regenera os segredos da chave de acesso.|
|/workflows/accessKeys/write|Cria ou atualiza a chave de acesso.|
|/workflows/delete|Elimina o fluxo de trabalho.|
|/workflows/disable/action|Desativa o fluxo de trabalho.|
|/workflows/enable/action|Ativa o fluxo de trabalho.|
|/workflows/listCallbackUrl/action|Obtém o URL de chamada de retorno do fluxo de trabalho.|
|/workflows/listSwagger/action|Obtém as definições do swagger do fluxo de trabalho.|
|/workflows/move/action|Move o Fluxo de trabalho do ID de subscrição, grupo de recursos e/ou nome existentes para um ID de subscrição, grupo de recursos e/ou nome diferentes.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/read|Lê as definições de diagnóstico do fluxo de trabalho.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico do fluxo de trabalho.|
|/workflows/providers/Microsoft.Insights/logDefinitions/read|Lê as definições de registo do fluxo de trabalho.|
|/workflows/providers/Microsoft.Insights/metricDefinitions/read|Lê as definições de métrica do fluxo de trabalho.|
|/workflows/read|Lê o fluxo de trabalho.|
|/workflows/regenerateAccessKey/action|Regenera os segredos da chave de acesso.|
|/workflows/run/action|Inicia a execução do fluxo de trabalho.|
|/workflows/runs/actions/listExpressionTraces/action|Obtém os rastreios de expressão da ação de execução do fluxo de trabalho.|
|/workflows/runs/actions/read|Lê a ação de execução do fluxo de trabalho.|
|/workflows/runs/actions/repetitions/listExpressionTraces/action|Obtém os rastreios de expressão da repetição da ação de execução do fluxo de trabalho.|
|/workflows/runs/actions/repetitions/read|Lê a repetição da ação de execução do fluxo de trabalho.|
|/workflows/runs/actions/scoperepetitions/read|Lê a repetição do âmbito da ação de execução do fluxo de trabalho.|
|/workflows/runs/cancel/action|Cancela a execução de um fluxo de trabalho.|
|/workflows/runs/operations/read|Lê o estado da operação de execução do fluxo de trabalho.|
|/workflows/runs/read|Lê a execução do fluxo de trabalho.|
|/workflows/suspend/action|Suspende o fluxo de trabalho.|
|/workflows/triggers/histories/read|Lê o histórico do acionador.|
|/workflows/triggers/histories/resubmit/action|Submete novamente o acionador do fluxo de trabalho.|
|/workflows/triggers/listCallbackUrl/action|Obtém o URL de chamada de retorno para o acionador.|
|/workflows/triggers/read|Lê o acionador.|
|/workflows/triggers/reset/action|Repõe o acionador.|
|/workflows/triggers/run/action|Executa o acionador.|
|/workflows/triggers/setState/action|Define o estado de Acionador.|
|/workflows/validate/action|Valida o fluxo de trabalho.|
|/workflows/versions/read|Lê a versão de fluxo de trabalho.|
|/workflows/versions/triggers/listCallbackUrl/action|Obtém o URL de chamada de retorno para o acionador.|
|/workflows/write|Cria ou atualiza o fluxo de trabalho.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Operação | Descrição |
|---|---|
|/commitmentPlans/commitmentAssociations/move/action|Mover quaisquer do Machine Learning compromisso plano de associação|
|/commitmentPlans/commitmentAssociations/read|Qualquer do Machine Learning compromisso plano de associação de leitura|
|/commitmentPlans/delete|Eliminar qualquer compromisso plano de aprendizagem|
|/commitmentPlans/join/action|Aderir a qualquer máquina compromisso plano de aprendizagem|
|/commitmentPlans/read|Ler qualquer compromisso plano de aprendizagem|
|/commitmentPlans/write|Criar ou atualizar qualquer plano de compromisso do Machine Learning|
|/locations/operationresults/read|Obter Resultado de uma operação do Machine Learning|
|/locations/operationsstatus/read|Obter o estado de uma operação de aprendizagem máquina em curso|
|/operations/read|Obter operações de aprendizagem|
|/register/action|Regista a subscrição para a fornecedor de recursos do serviço web de aprendizagem e permite a criação de serviços web.|
|/skus/read|Obter do Machine Learning SKUs do plano de compromisso|
|/webServices/action|Criar as propriedades do serviço Web regional para as regiões suportadas|
|/webServices/delete|Eliminar qualquer serviço Web do Machine Learning|
|/webServices/read|Ler a qualquer serviço Web do Machine Learning|
|/webServices/write|Criar ou atualizar qualquer serviço Web do Machine Learning|
|/Workspaces/delete|Eliminar qualquer área de trabalho de aprendizagem|
|/Workspaces/listworkspacekeys/action|Lista de chaves para uma área de trabalho do Machine Learning|
|/Workspaces/read|Ler qualquer área de trabalho de aprendizagem|
|/Workspaces/resyncstoragekeys/action|Ressincronizar chaves de conta de armazenamento configurada para uma área de trabalho do Machine Learning|
|/Workspaces/write|Criar ou atualizar qualquer área de trabalho de aprendizagem|

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

| Operação | Descrição |
|---|---|
|/operationalizationClusters/checkUpdate/action|Verifique se as atualizações estão disponíveis para os serviços de sistema para o cluster operationalization|
|/operationalizationClusters/delete|Eliminar qualquer conta de alojamento|
|/operationalizationClusters/listKeys/action|Listar as chaves associadas ao operationalization cluster|
|/operationalizationClusters/read|Qualquer conta de alojamento de leitura|
|/operationalizationClusters/updateSystem/action|Atualizar os serviços do sistema de um cluster de operationalization|
|/operationalizationClusters/write|Criar ou atualizar qualquer conta de alojamento|
|/register/action|Regista o ID de subscrição para o fornecedor de recursos e permite a criação de um de machine learning recursos de computação|

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

| Operação | Descrição |
|---|---|
|/accounts/delete|Eliminar qualquer conta de alojamento|
|/accounts/read|Qualquer conta de alojamento de leitura|
|/accounts/write|Criar ou atualizar qualquer conta de alojamento|
|/register/action|Regista o ID de subscrição para o fornecedor de recursos e permite a criação de uma conta de alojamento|

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

| Operação | Descrição |
|---|---|
|/userAssignedIdentities/assign/action|Ação de RBAC para a atribuição de um utilizador existente atribuídas identidade a um recurso|
|/userAssignedIdentities/delete|Elimina um utilizador existente atribuído identidade|
|/userAssignedIdentities/read|Obtém um atribuído a identidade de utilizador existente|
|/userAssignedIdentities/write|Cria um novo utilizador atribuído a identidade ou atualiza etiquetas associadas um utilizador existente atribuído identidade|

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

| Operação | Descrição |
|---|---|
|/labAccounts/CreateLab/action|Crie um laboratório numa conta de laboratório.|
|/labAccounts/delete|Elimine contas de laboratório.|
|/labAccounts/labs/delete|Elimine laboratórios.|
|/labAccounts/labs/environmentSettings/delete|Elimine a definição de ambiente.|
|/labAccounts/labs/environmentSettings/environments/delete|Elimine ambientes.|
|/labAccounts/labs/environmentSettings/environments/read|Ambientes de leitura.|
|/labAccounts/labs/environmentSettings/environments/write|Adiciona ou modifica os ambientes.|
|/labAccounts/labs/environmentSettings/read|Definição de ambiente de leitura.|
|/labAccounts/labs/environmentSettings/write|Adicionar ou modificar a definição do ambiente.|
|/labAccounts/labs/labVms/delete|Elimine as máquinas virtuais do laboratório.|
|/labAccounts/labs/labVms/read|Máquinas virtuais do laboratório de leitura.|
|/labAccounts/labs/labVms/write|Adiciona ou modifica máquinas virtuais do laboratório.|
|/labAccounts/labs/read|Laboratórios de leitura.|
|/labAccounts/labs/write|Adiciona ou modifica laboratórios.|
|/labAccounts/read|Ler as contas de laboratório.|
|/labAccounts/write|Adiciona ou modifica contas de laboratório.|
|/locations/operations/read|Operações de leitura.|
|/register/action|Regista a subscrição do|

## <a name="microsoftmanagement"></a>Microsoft.Management

| Operação | Descrição |
|---|---|
|/checkNameAvailability/action|Verifica se o nome do grupo de gestão especificado é válido e exclusivo.|
|/getEntities/action|Liste todas as entidades (grupos de gestão, subscrições, etc.) para o utilizador autenticado.|
|/managementGroups/delete|Elimine grupo de gestão.|
|/managementGroups/read|Liste os grupos de gestão para o utilizador autenticado.|
|/managementGroups/subscriptions/delete|Associa anular a subscrição do grupo de gestão.|
|/managementGroups/subscriptions/write|Associates existente subscrição com o grupo de gestão.|
|/managementGroups/write|Criar ou atualizar um grupo de gestão.|

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

| Operação | Descrição |
|---|---|
|/ClassicDevServices/delete|Efetua uma operação de eliminação num recurso de serviço do programador clássico.|
|/ClassicDevServices/listSecrets/action|Obtém um dev clássico chaves de gestão de recursos do serviço.|
|/ClassicDevServices/listSingleSignOnToken/action|Obtém o início de sessão único num URL para um serviço dev clássico.|
|/ClassicDevServices/read|Executa uma operação de GET num serviço dev clássico.|
|/ClassicDevServices/regenerateKey/action|Gera um chaves de gestão de recursos de serviço dev clássico.|
|/Operations/read|Ler as operações para todos os tipos de recursos.|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Operação | Descrição |
|---|---|
|/agreements/offers/plans/cancel/action|Cancelar um contrato de um item do marketplace indicado|
|/agreements/offers/plans/read|Devolver um contrato de um item do marketplace indicado|
|/agreements/offers/plans/sign/action|Assinar um contrato de um item do marketplace indicado|
|/agreements/read|Devolver todos os contratos em dada subscrição|
|/offertypes/publishers/offers/plans/agreements/read|Obter um contrato de um item de máquina virtual fornecido marketplace|
|/offertypes/publishers/offers/plans/agreements/write|Inicie sessão ou em Cancelar um contrato de um item de máquina virtual fornecido marketplace|

## <a name="microsoftmedia"></a>Microsoft.Media

| Operação | Descrição |
|---|---|
|/checknameavailability/action|Verifica se está disponível um nome de conta de Media Services|
|/mediaservices/delete|Eliminar qualquer conta de Media Services|
|/mediaservices/listKeys/action|Listar as chaves de ACS para a conta de Media Services|
|/mediaservices/read|Ler a qualquer conta de Media Services|
|/mediaservices/regenerateKey/action|Voltar a gerar uma chave de serviços de suporte de dados ACS|
|/mediaservices/syncStorageKeys/action|Sincronizar as chaves de armazenamento para uma conta de armazenamento do Azure anexada|
|/mediaservices/write|Criar ou atualizar qualquer conta de Media Services|
|/operations/read|Ler a qualquer conta de Media Services|
|/register/action|Regista a subscrição para o fornecedor de recursos de Media Services e permite a criação de contas dos Media Services|

## <a name="microsoftmigrate"></a>Microsoft.Migrate

| Operação | Descrição |
|---|---|
|/Operations/read|Lê as operações expostas|

## <a name="microsoftnetwork"></a>Microsoft.Network

| Operação | Descrição |
|---|---|
|/applicationGatewayAvailableSslOptions/predefinedPolicies/read|Ssl do Gateway de aplicação predefinidas política|
|/applicationGatewayAvailableSslOptions/read|Opções de Ssl de disponíveis de Gateway de aplicação|
|/applicationGatewayAvailableWafRuleSets/read|Obtém o Gateway de aplicação define a regra de Waf disponíveis|
|/applicationGateways/backendAddressPools/join/action|Associa um conjunto de endereços de back-end de gateway de aplicação|
|/applicationGateways/backendhealth/action|Obtém um funcionamento de back-end do gateway de aplicação|
|/applicationGateways/delete|Elimina um gateway de aplicação|
|/applicationGateways/effectiveNetworkSecurityGroups/action|Obter tabela de rotas configurado no Gateway de aplicação|
|/applicationGateways/effectiveRouteTable/action|Obter tabela de rotas configurado no Gateway de aplicação|
|/applicationGateways/providers/Microsoft.Insights/logDefinitions/read|Obtém os eventos do Gateway de aplicação|
|/applicationGateways/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o Gateway de aplicação|
|/applicationGateways/read|Obtém um gateway de aplicação|
|/applicationGateways/setSecurityCenterConfiguration/action|Configuração de centro de segurança de Gateway de aplicação de conjuntos|
|/applicationGateways/start/action|Inicia um gateway de aplicação|
|/applicationGateways/stop/action|Parar um gateway de aplicação|
|/applicationGateways/write|Cria um gateway de aplicação ou atualiza um gateway de aplicação|
|/applicationSecurityGroups/delete|Elimina um grupo de segurança da aplicação|
|/applicationSecurityGroups/joinIpConfiguration/action|Associa uma configuração de IP para grupos de segurança de aplicações.|
|/applicationSecurityGroups/joinNetworkSecurityRule/action|Associa uma regra de segurança para grupos de segurança de aplicações.|
|/applicationSecurityGroups/read|Obtém um ID de grupo de segurança de aplicação.|
|/applicationSecurityGroups/write|Cria um grupo de segurança da aplicação ou atualiza um grupo de segurança de aplicação existente.|
|/bgpServiceCommunities/read|Obter Comunidades de Bgp de serviço|
|/checkTrafficManagerNameAvailability/action|Verifica a disponibilidade de um nome de DNS de caminho relativo do Gestor de tráfego.|
|/connections/delete|Elimina o VirtualNetworkGatewayConnection|
|/connections/read|Obtém o VirtualNetworkGatewayConnection|
|/connections/sharedkey/action|Obter o VirtualNetworkGatewayConnection SharedKey|
|/connections/sharedKey/read|Obtém o VirtualNetworkGatewayConnection SharedKey|
|/connections/sharedKey/write|Cria ou atualiza um VirtualNetworkGatewayConnection SharedKey existente|
|/connections/vpndeviceconfigurationscript/read|Obtém a configuração do dispositivo de Vpn de VirtualNetworkGatewayConnection|
|/connections/write|Cria ou atualiza um VirtualNetworkGatewayConnection existente|
|/ddosProtectionPlans/ddosProtectionPlanProxies/delete|Elimina um Proxy de plano de proteção DDoS|
|/ddosProtectionPlans/ddosProtectionPlanProxies/read|Obtém uma definição de Proxy de planear a proteção de DDoS|
|/ddosProtectionPlans/ddosProtectionPlanProxies/write|Cria um Proxy de plano de proteção para DDoS ou atualizações e Proxy de plano de proteção de DDoS existente|
|/ddosProtectionPlans/delete|Elimina um plano de proteção DDoS|
|/ddosProtectionPlans/join/action|Associa um plano de proteção DDoS|
|/ddosProtectionPlans/read|Obtém um plano de proteção DDoS|
|/ddosProtectionPlans/write|Cria um plano de proteção DDoS ou atualiza um plano de proteção DDoS |
|/dnsoperationresults/read|Obtém os resultados de uma operação de DNS|
|/dnsoperationstatuses/read|Obtém o estado de uma operação de DNS |
|/dnszones/A/delete|Remova o conjunto de registos de um determinado nome e tipo "A" de uma zona DNS.|
|/dnszones/A/read|Obtenha o conjunto de registos do tipo "A", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag.|
|/dnszones/A/write|Crie ou Atualize um conjunto de registos do tipo 'A' dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos.|
|/dnszones/AAAA/delete|Remova o conjunto de registos de um determinado nome e tipo "AAAA" de uma zona DNS.|
|/dnszones/AAAA/read|Obtenha o conjunto de registos do tipo "AAAA", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag.|
|/dnszones/AAAA/write|Crie ou Atualize um conjunto de registos do tipo "AAAA" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos.|
|/dnszones/all/read|Obtém os conjuntos de registos de DNS em tipos|
|/dnszones/CAA/delete|Remova o conjunto de registos de um determinado nome e o tipo 'CAA' de uma zona DNS.|
|/dnszones/CAA/read|Obtenha o conjunto de registos do tipo 'CAA', no formato JSON. O conjunto de registos contém o TTL, as etiquetas e etag.|
|/dnszones/CAA/write|Crie ou Atualize um conjunto de registos do tipo 'CAA' dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos.|
|/dnszones/CNAME/delete|Remova o conjunto de registos de um determinado nome e tipo "CNAME" de uma zona DNS.|
|/dnszones/CNAME/read|Obtenha o conjunto de registos do tipo "CNAME", no formato JSON. O conjunto de registos contém o TTL, as etiquetas e etag.|
|/dnszones/CNAME/write|Crie ou Atualize um conjunto de registos do tipo "CNAME" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos.|
|/dnszones/delete|Elimine a zona DNS, no formato JSON. As propriedades da zona incluem as etiquetas, etag, numberOfRecordSets e maxNumberOfRecordSets.|
|/dnszones/MX/delete|Remova o conjunto de registos de um determinado nome e tipo "MX" de uma zona DNS.|
|/dnszones/MX/read|Obtenha o conjunto de registos do tipo "MX", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag.|
|/dnszones/MX/write|Crie ou Atualize um conjunto de registos do tipo "MX" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos.|
|/dnszones/NS/delete|Elimina o conjunto de registos DNS do tipo NS|
|/dnszones/NS/read|Obtém o conjunto de registos de DNS do tipo NS|
|/dnszones/NS/write|Cria ou atualiza o conjunto de registos de DNS do tipo NS|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a zona DNS, definições de diagnóstico|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico de zona DNS|
|/dnszones/providers/Microsoft.Insights/metricDefinitions/read|Obtém as definições de métrica de zona DNS|
|/dnszones/PTR/delete|Remova o conjunto de registos de um determinado nome e tipo "PTR" de uma zona DNS.|
|/dnszones/PTR/read|Obtenha o conjunto de registos do tipo "PTR", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag.|
|/dnszones/PTR/write|Crie ou Atualize um conjunto de registos do tipo "PTR" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos.|
|/dnszones/read|Obtenha a zona DNS, no formato JSON. As propriedades da zona incluem as etiquetas, etag, numberOfRecordSets e maxNumberOfRecordSets. Tenha em atenção que este comando não obtém os conjuntos de registos contidos na zona.|
|/dnszones/recordsets/read|Obtém os conjuntos de registos de DNS em tipos|
|/dnszones/SOA/read|Obtém o conjunto de registos DNS do tipo SOA|
|/dnszones/SOA/write|Cria ou atualiza o conjunto de registos DNS do tipo SOA|
|/dnszones/SRV/delete|Remova o conjunto de registos de um determinado nome e tipo "SRV" de uma zona DNS.|
|/dnszones/SRV/read|Obtenha o conjunto de registos do tipo "SRV", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag.|
|/dnszones/SRV/write|Criar ou atualizar conjunto de registos do tipo SRV|
|/dnszones/TXT/delete|Remova o conjunto de registos de um determinado nome e tipo "TXT" de uma zona DNS.|
|/dnszones/TXT/read|Obtenha o conjunto de registos do tipo "TXT", no formato JSON. O conjunto de registos contém uma lista de registos, bem como o TTL, as etiquetas e etag.|
|/dnszones/TXT/write|Crie ou Atualize um conjunto de registos do tipo "TXT" dentro de uma zona DNS. Os registos especificados irão substituir os registos atuais no conjunto de registos.|
|/dnszones/write|Crie ou Atualize uma zona DNS dentro de um grupo de recursos.  Utilizado para atualizar as etiquetas num recurso de zona DNS. Tenha em atenção que este comando não pode ser utilizado para criar ou atualizar conjuntos de registos na zona.|
|/expressRouteCircuits/authorizations/delete|Elimina uma ExpressRouteCircuit autorização|
|/expressRouteCircuits/authorizations/read|Obtém uma ExpressRouteCircuit autorização|
|/expressRouteCircuits/authorizations/write|Cria ou atualiza uma autorização ExpressRouteCircuit existente|
|/expressRouteCircuits/delete|Elimina um ExpressRouteCircuit|
|/expressRouteCircuits/peerings/arpTables/action|Obtém um ArpTable ExpressRouteCircuit de Peering|
|/expressRouteCircuits/peerings/connections/delete|Elimina uma ligação de ExpressRouteCircuit|
|/expressRouteCircuits/peerings/connections/read|Obtém uma ligação de ExpressRouteCircuit|
|/expressRouteCircuits/peerings/connections/write|Cria ou atualiza um recurso de ligação ExpressRouteCircuit existente|
|/expressRouteCircuits/peerings/delete|Elimina um ExpressRouteCircuit Peering|
|/expressRouteCircuits/peerings/read|Obtém um ExpressRouteCircuit Peering|
|/expressRouteCircuits/peerings/routeTables/action|Obtém um RouteTable ExpressRouteCircuit de Peering|
|/expressRouteCircuits/peerings/routeTablesSummary/action|Obtém um resumo de RouteTable Peering ExpressRouteCircuit|
|/expressRouteCircuits/peerings/stats/read|Obtém um ExpressRouteCircuit Peering Stat|
|/expressRouteCircuits/peerings/write|Cria ou atualiza um ExpressRouteCircuit Peering existente|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read|Obtém as definições de diagnóstico para circuitos do ExpressRoute|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico para circuitos do ExpressRoute|
|/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read|Obter os eventos para circuitos do ExpressRoute|
|/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read|Obtém as definições de métricas para circuitos do ExpressRoute|
|/expressRouteCircuits/read|Obter um ExpressRouteCircuit|
|/expressRouteCircuits/stats/read|Obtém um ExpressRouteCircuit Stat|
|/expressRouteCircuits/write|Cria ou atualiza um ExpressRouteCircuit existente|
|/expressRouteCrossConnections/delete|Eliminar Expressroute entre a ligação|
|/expressRouteCrossConnections/join/action|Associa um Expressroute cruzada ligação|
|/expressRouteCrossConnections/peerings/arpTables/action|Obtém um Expressroute entre a tabela de Arp Peering da ligação|
|/expressRouteCrossConnections/peerings/delete|Elimina uma Expressroute cruzada ligação Peering|
|/expressRouteCrossConnections/peerings/read|Obtém um Expressroute cruzada ligação Peering|
|/expressRouteCrossConnections/peerings/routeTables/action|Obtém um Expressroute entre a tabela de rotas de Peering de ligação|
|/expressRouteCrossConnections/peerings/routeTableSummary/action|Obtém um Expressroute entre a ligação de resumo de tabela de rota Peering|
|/expressRouteCrossConnections/peerings/stats/read|Obtém um Expressroute cruzada ligação Peering Stat|
|/expressRouteCrossConnections/peerings/write|Cria um cruzada ligação Peering de Expressroute ou atualiza um existente cruzada ligação Peering de Expressroute|
|/expressRouteCrossConnections/read|Obter Expressroute entre a ligação|
|/expressRouteCrossConnections/write|Criar ou atualizar Expressroute cruzada ligação|
|/expressRouteServiceProviders/read|Obtém os fornecedores de serviços de Expressroute|
|/loadBalancers/backendAddressPools/join/action|Associa um conjunto de endereços de back-end de Balanceador de carga|
|/loadBalancers/backendAddressPools/read|Obtém uma definição de conjunto de endereços de back-end de Balanceador de carga|
|/loadBalancers/delete|Elimina um balanceador de carga|
|/loadBalancers/frontendIPConfigurations/read|Obtém uma definição de configuração de IP de front-end de Balanceador de carga|
|/loadBalancers/inboundNatPools/join/action|Associa um balanceador de carga conjunto nat de entrada|
|/loadBalancers/inboundNatPools/read|Obtém um balanceador de carga definição do conjunto nat de entrada|
|/loadBalancers/inboundNatRules/delete|Elimina uma regra de nat de entrada do Balanceador de carga|
|/loadBalancers/inboundNatRules/join/action|Associa uma regra de nat de entrada do Balanceador de carga|
|/loadBalancers/inboundNatRules/read|Obtém um balanceador de carga definição da regra nat de entrada|
|/loadBalancers/inboundNatRules/write|Cria uma regra de nat de entrada do Balanceador de carga ou atualiza uma regra nat de entrada do Balanceador de carga existente|
|/loadBalancers/loadBalancingRules/read|Obtém uma definição de regra balanceamento de carga Balanceador de carga|
|/loadBalancers/networkInterfaces/read|Obtém as referências a todas as interfaces de rede num Balanceador de carga|
|/loadBalancers/outboundNatRules/read|Obtém uma definição de regra de nat de saída de Balanceador de carga|
|/loadBalancers/probes/join/action|Permite utilizar as pesquisas de um balanceador de carga. Por exemplo, com esta propriedade de healthProbe de permissão de dimensionamento VM conjunto pode referenciar a pesquisa.|
|/loadBalancers/probes/read|Obtém uma sonda do Balanceador de carga|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/read|Obtém as definições de diagnóstico de Balanceador de carga|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico de Balanceador de carga|
|/loadBalancers/providers/Microsoft.Insights/logDefinitions/read|Obtém os eventos do Balanceador de carga|
|/loadBalancers/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o Balanceador de carga|
|/loadBalancers/read|Obtém uma definição de Balanceador de carga|
|/loadBalancers/virtualMachines/read|Obtém as referências a todas as máquinas virtuais num Balanceador de carga|
|/loadBalancers/write|Cria um balanceador de carga ou atualiza um balanceador de carga existente|
|/localnetworkgateways/delete|Elimina LocalNetworkGateway|
|/localnetworkgateways/read|Obtém LocalNetworkGateway|
|/localnetworkgateways/write|Cria ou atualiza um LocalNetworkGateway existente|
|/locations/checkDnsNameAvailability/read|Verifica se a etiqueta de dns está disponível na localização especificada|
|/locations/operationResults/read|Obtém o resultado de um async POST ou operação de eliminação|
|/locations/operations/read|Obtém o recurso da operação que representa o estado de uma operação assíncrona|
|/locations/usages/read|Obtém os recursos a métrica de utilização|
|/locations/virtualNetworkAvailableEndpointServices/read|Obtém uma lista dos serviços de ponto final de rede Virtual disponíveis|
|/networkInterfaces/delete|Elimina uma interface de rede|
|/networkInterfaces/diagnosticIdentity/read|Obtém a identidade de diagnóstico do recurso|
|/networkInterfaces/effectiveNetworkSecurityGroups/action|Obter grupos de segurança de rede configurado na Interface da Vm de rede|
|/networkInterfaces/effectiveRouteTable/action|Obter tabela de rotas configurado na Interface de rede da Vm|
|/networkInterfaces/ipconfigurations/read|Obtém uma definição de configuração de ip de interface de rede. |
|/networkInterfaces/join/action|Associa uma Máquina Virtual a uma interface de rede|
|/networkInterfaces/loadBalancers/read|Obtém todos os balanceadores de carga que a interface de rede faz parte|
|/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read|Obtém as métricas disponíveis para a Interface de rede|
|/networkInterfaces/read|Obtém uma definição de interface de rede. |
|/networkInterfaces/write|Cria uma interface de rede ou atualiza uma interface de rede existente. |
|/networkSecurityGroups/defaultSecurityRules/read|Obtém uma definição de regra de segurança predefinida|
|/networkSecurityGroups/delete|Elimina um grupo de segurança de rede|
|/networkSecurityGroups/join/action|Associa um grupo de segurança de rede|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read|Obtém os grupos de segurança de rede para definições de diagnóstico|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico de grupos de segurança de rede, esta operação é suplementada pelo fornecedor de recursos de informações.|
|/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read|Obtém os eventos para o grupo de segurança de rede|
|/networkSecurityGroups/read|Obtém uma definição de grupo de segurança de rede|
|/networkSecurityGroups/securityRules/delete|Elimina uma regra de segurança|
|/networkSecurityGroups/securityRules/read|Obtém a definição de uma regra de segurança|
|/networkSecurityGroups/securityRules/write|Cria uma regra de segurança ou atualiza uma regra de segurança existente|
|/networkSecurityGroups/write|Cria um grupo de segurança de rede ou atualiza um grupo de segurança de rede existente|
|/networkWatchers/availableProvidersList/action|Devolve a internet disponível todos os fornecedores de serviços para uma região do Azure especificada.|
|/networkWatchers/azureReachabilityReport/action|Devolve a classificação de latência relativo para a internet fornecedores de serviços de uma localização especificada para regiões do Azure.|
|/networkWatchers/configureFlowLog/action|Configura o registo de fluxo para um recurso de destino.|
|/networkWatchers/connectionMonitors/delete|Elimina um Monitor de ligação|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings/read|Obter as definições de diagnóstico do Monitor de ligação|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico de Monitor de ligação|
|/networkWatchers/connectionMonitors/Providers/Microsoft.Insights/ metricDefinitions/leitura|Obtem as métricas disponíveis para o Monitor de ligação|
|/networkWatchers/connectionMonitors/query/action|Consulta monitorização conectividade entre pontos finais especificados|
|/networkWatchers/connectionMonitors/read|Obter os detalhes de Monitor de ligação|
|/networkWatchers/connectionMonitors/start/action|Iniciar a monitorização de conectividade entre pontos finais especificados|
|/networkWatchers/connectionMonitors/stop/action|Parar/pausa monitorização conectividade entre pontos finais especificados|
|/networkWatchers/connectionMonitors/write|Cria um Monitor de ligação|
|/networkWatchers/connectivityCheck/action|Verifica a possibilidade de estabelecer uma ligação de TCP direta de uma máquina virtual para um ponto de final fornecido, incluindo outra VM ou um servidor remoto arbitrário.|
|/networkWatchers/delete|Elimina um observador de rede|
|/networkWatchers/ipFlowVerify/action|Devolve se o pacote é permitido ou negado de ou para um destino específico.|
|/networkWatchers/lenses/delete|Elimina uma lente|
|/networkWatchers/lenses/query/action|Consulta de monitorização do tráfego de rede de um ponto final especificado|
|/networkWatchers/lenses/read|Obter os detalhes da lente|
|/networkWatchers/lenses/start/action|Iniciar a monitorização de tráfego de rede de um ponto final especificado|
|/networkWatchers/lenses/stop/action|Parar/pausa monitorização do tráfego de rede de um ponto final especificado|
|/networkWatchers/lenses/write|Cria uma lente|
|/networkWatchers/nextHop/action|Para um destino especificado e o endereço IP de destino, devolver o tipo de salto seguinte e hope seguinte endereço IP.|
|/networkWatchers/packetCaptures/delete|Elimina uma captura de pacotes|
|/networkWatchers/packetCaptures/queryStatus/action|Obtém informações sobre as propriedades e estado de um recurso de captura de pacotes.|
|/networkWatchers/packetCaptures/read|Obter a definição de captura de pacotes|
|/networkWatchers/packetCaptures/stop/action|Pare a sessão de captura de pacote em execução.|
|/networkWatchers/packetCaptures/write|Cria uma captura de pacotes|
|/networkWatchers/queryFlowLogStatus/action|Obtém o estado do fluxo de registo num recurso.|
|/networkWatchers/queryTroubleshootResult/action|Obtém o resultado de resolução de problemas de anteriormente executados ou atualmente em execução a operação de resolução de problemas.|
|/networkWatchers/read|Obter a definição de observador de rede|
|/networkWatchers/securityGroupView/action|Ver as regras de grupo de segurança de rede configurados e eficaz aplicadas numa VM.|
|/networkWatchers/topology/action|Obtém uma vista de nível de rede de recursos e as respetivas relações num grupo de recursos.|
|/networkWatchers/troubleshoot/action|Inicia a resolução de problemas num recurso de rede no Azure.|
|/networkWatchers/write|Cria um observador de rede ou atualiza um observador de rede existente|
|/operations/read|Obter operações disponíveis|
|/publicIPAddresses/delete|Elimina um endereço Ip público.|
|/publicIPAddresses/join/action|Associa um endereço ip público|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read|Obter as definições de diagnóstico do endereço IP público|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write|Criar ou atualizar as definições de diagnóstico do endereço IP público|
|/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read|Obter as definições de registo do endereço IP público|
|/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read|Obter as definições de métrica de endereço IP público|
|/publicIPAddresses/read|Obtém uma definição de endereço ip público.|
|/publicIPAddresses/write|Cria um endereço Ip público ou atualiza um endereço Ip público existente. |
|/register/action|Regista a subscrição do|
|/routeFilters/delete|Elimina uma definição de filtro de rota|
|/routeFilters/join/action|Associa um filtro de rota|
|/routeFilters/read|Obtém uma definição de filtro de rota|
|/routeFilters/routeFilterRules/delete|Elimina a definição de uma regra de filtro de rota|
|/routeFilters/routeFilterRules/read|Obtém a definição de uma regra de filtro de rota|
|/routeFilters/routeFilterRules/write|Cria uma regra de filtro de rota ou atualiza uma regra de filtro de rota existente|
|/routeFilters/write|Cria um filtro de rota ou atualiza um filtro já existente|
|/routeTables/delete|Elimina uma definição de tabela de rota|
|/routeTables/join/action|Junta a tabela de rotas|
|/routeTables/read|Obtém uma definição de tabela de rota|
|/routeTables/routes/delete|Elimina uma definição de rota|
|/routeTables/routes/read|Obtém uma definição de rota|
|/routeTables/routes/write|Cria uma rota ou atualiza uma já existente|
|/routeTables/write|Cria uma tabela de rotas ou atualiza uma tabela já existente|
|/securegateways/applicationRuleCollections/delete|Elimina uma coleção de regras de aplicação para um Gateway seguro|
|/securegateways/applicationRuleCollections/read|Obter uma coleção de regras de aplicação para um determinado Gateway seguro|
|/securegateways/applicationRuleCollections/write|Cria ou atualiza uma coleção de regras de aplicação para um Gateway seguro|
|/securegateways/delete|Eliminar Gateway segura|
|/securegateways/networkRuleCollections/delete|Elimina uma coleção de regras de rede para um Gateway seguro|
|/securegateways/networkRuleCollections/read|Obter uma coleção de regras de rede para um determinado Gateway seguro|
|/securegateways/networkRuleCollections/write|Cria ou atualiza uma coleção de regras de rede para um Gateway seguro|
|/securegateways/read|Obter o Gateway segura|
|/securegateways/write|Cria ou atualiza um Gateway seguro|
|/serviceEndpointPolicies/delete|Elimina uma política de ponto final de serviço|
|/serviceEndpointPolicies/join/action|Associa uma política de ponto final de serviço|
|/serviceEndpointPolicies/joinSubnet/action|Associa uma sub-rede para políticas de ponto final de serviço|
|/serviceEndpointPolicies/read|Obtém uma descrição de política do ponto final de serviço|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete|Elimina uma definição de política do ponto final de serviço|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read|Obtém um Decription de definição de política de ponto final de serviço|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write|Cria uma definição de política do ponto final de serviço ou atualiza uma definição de política de ponto final de serviço existente|
|/serviceEndpointPolicies/write|Cria uma política de ponto final de serviço ou atualiza uma política de ponto final de serviço existente|
|/trafficManagerGeographicHierarchies/read|Obtém a hierarquia de Geographic do Gestor de tráfego que contém regiões que podem ser utilizadas com o método de encaminhamento de tráfego geográfica|
|/trafficManagerProfiles/azureEndpoints/delete|Elimina um ponto final do Azure a partir de um perfil do Traffic Manager existente. Gestor de tráfego deixará de encaminhamento de tráfego para o ponto final do Azure eliminado.|
|/trafficManagerProfiles/azureEndpoints/read|Obtém um ponto final do Azure, que pertence a um perfil de Gestor de tráfego, incluindo todas as propriedades desse ponto final do Azure.|
|/trafficManagerProfiles/azureEndpoints/write|Adicionar um novo ponto de final do Azure num perfil do Traffic Manager existente ou atualizar as propriedades de um ponto final do Azure existente nesse perfil do Traffic Manager.|
|/trafficManagerProfiles/delete|Elimine o perfil do Traffic Manager. Todas as definições associadas ao perfil do Gestor de tráfego serão perdidas e o perfil já não pode ser utilizado para encaminhar o tráfego.|
|/trafficManagerProfiles/externalEndpoints/delete|Elimina um ponto final externo de um perfil do Traffic Manager existente. Gestor de tráfego deixará de encaminhamento de tráfego para o ponto final externo eliminado.|
|/trafficManagerProfiles/externalEndpoints/read|Obtém um ponto final externo que pertence a um perfil de Gestor de tráfego, incluindo todas as propriedades desse ponto final externo.|
|/trafficManagerProfiles/externalEndpoints/write|Adicionar um novo ponto final externo num perfil do Traffic Manager existente ou atualizar as propriedades de um ponto final externo existente nesse perfil do Traffic Manager.|
|/trafficManagerProfiles/heatMaps/read|Obtém o mapa térmico do Gestor de tráfego para o perfil do Gestor de tráfego fornecido que contém dados de contagens e a latência de consulta por IP de origem e de localização.|
|/trafficManagerProfiles/nestedEndpoints/delete|Elimina um ponto final aninhada de um perfil do Traffic Manager existente. Gestor de tráfego deixará de encaminhamento de tráfego para o ponto de final aninhados eliminados.|
|/trafficManagerProfiles/nestedEndpoints/read|Obtém um ponto final aninhada que pertence a um perfil de Gestor de tráfego, incluindo todas as propriedades desse ponto final aninhado.|
|/trafficManagerProfiles/nestedEndpoints/write|Adicionar um novo ponto de final aninhado num perfil do Traffic Manager existente ou atualizar as propriedades de um aninhada ponto final existente nesse perfil do Traffic Manager.|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/read|Obtém as definições de diagnóstico do Gestor de tráfego|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico do Traffic Manager, esta operação é suplementada pelo fornecedor de recursos de informações.|
|/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read|Obtém os eventos para o Gestor de tráfego|
|/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o Gestor de tráfego.|
|/trafficManagerProfiles/read|Obter a configuração do perfil do Traffic Manager. Isto inclui as definições DNS, as definições de encaminhamento de tráfego, as definições de monitorização do ponto final e a lista de pontos finais encaminhados por este perfil do Traffic Manager.|
|/trafficManagerProfiles/write|Criar um perfil de Gestor de tráfego ou modificar a configuração de um perfil do Traffic Manager existente. Isto inclui ativar ou desativar um perfil e modificar as definições de DNS, as definições de encaminhamento de tráfego ou as definições de monitorização do ponto final. Os pontos finais encaminhados pelo perfil do Gestor de tráfego podem ser adicionados, removidos, ativados ou desativados.|
|/trafficManagerUserMetricsKeys/delete|Elimina a chave de nível de subscrição utilizada para a coleção de métricas de utilizador em tempo real.|
|/trafficManagerUserMetricsKeys/read|Obtém a chave de nível de subscrição utilizada para a coleção de métricas de utilizador em tempo real.|
|/trafficManagerUserMetricsKeys/write|Cria uma nova chave de nível de subscrição a ser utilizado para a coleção de métricas de utilizador em tempo real.|
|/unregister/action|Anula o registo da subscrição|
|/virtualHubs/delete|Elimina um concentrador Virtual|
|/virtualHubs/hubVirtualNetworkConnections/delete|Elimina um HubVirtualNetworkConnection|
|/virtualHubs/hubVirtualNetworkConnections/read|Obter um HubVirtualNetworkConnection|
|/virtualHubs/hubVirtualNetworkConnections/write|Criar ou atualizar um HubVirtualNetworkConnection|
|/virtualHubs/read|Obter um concentrador Virtual|
|/virtualHubs/write|Criar ou atualizar um concentrador Virtual|
|/virtualnetworkgateways/connections/read|Obter o VirtualNetworkGatewayConnection|
|/virtualNetworkGateways/delete|Elimina um virtualNetworkGateway|
|/virtualnetworkgateways/generatevpnclientpackage/action|Gerar o pacote de VpnClient para virtualNetworkGateway|
|/virtualnetworkgateways/generatevpnprofile/action|Gerar o pacote de VpnProfile para VirtualNetworkGateway|
|/virtualnetworkgateways/getadvertisedroutes/action|Obtém virtualNetworkGateway anunciados rotas|
|/virtualnetworkgateways/getbgppeerstatus/action|Obtém o estado de ponto a ponto do bgp virtualNetworkGateway|
|/virtualnetworkgateways/getlearnedroutes/action|Obtém as rotas aprendida de virtualnetworkgateway|
|/virtualnetworkgateways/getvpnclientipsecparameters/action|Obter parâmetros Vpnclient Ipsec para VirtualNetworkGateway P2S cliente.|
|/virtualnetworkgateways/getvpnprofilepackageurl/action|Obtém o URL de um pacote de perfil de cliente vpn pré-geradas|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read|Obtém as definições de diagnóstico de Gateway de rede Virtual|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza as definições de diagnóstico do Gateway de rede Virtual, esta operação é suplementada pelo fornecedor de recursos de informações.|
|/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read|Obtém os eventos do Gateway de rede Virtual|
|/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para o Gateway de rede Virtual|
|/virtualNetworkGateways/read|Obtém um VirtualNetworkGateway|
|/virtualnetworkgateways/reset/action|Repõe um virtualNetworkGateway|
|/virtualnetworkgateways/setvpnclientipsecparameters/action|Defina Vpnclient Ipsec parâmetros para VirtualNetworkGateway P2S cliente.|
|/virtualnetworkgateways/supportedvpndevices/action|Apresenta uma lista de suportados dispositivos Vpn|
|/virtualNetworkGateways/write|Cria ou atualiza um VirtualNetworkGateway|
|/virtualNetworks/checkIpAddressAvailability/read|Verifique se o endereço Ip está disponível na rede virtual especificada|
|/virtualNetworks/customViews/get/action|Obter um conteúdo de vista personalizada de rede Virtual|
|/virtualNetworks/customViews/read|Obter a definição de uma vista personalizada da rede Virtual|
|/virtualNetworks/delete|Elimina uma rede virtual|
|/virtualNetworks/peer/action|Peers uma rede virtual com a outra rede virtual|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read|Obter as definições de diagnóstico de rede Virtual|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write|Criar ou atualizar as definições de diagnóstico da rede Virtual|
|/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read|Obter as definições de registo de rede Virtual|
|/virtualNetworks/providers/Microsoft.Insights/metricDefinitions/read|Obter as definições de métricas de rede Virtual|
|/virtualNetworks/read|Obter a definição de rede virtual|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete|Elimina um proxy de peering de rede virtual|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/read|Obtém uma definição de proxy de peering de rede virtual|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/write|Cria um proxy de peering de rede virtual ou atualiza um proxy de peering de rede virtual existente|
|/virtualNetworks/subnets/delete|Elimina uma sub-rede de rede virtual|
|/virtualNetworks/subnets/join/action|Associa uma rede virtual|
|/virtualNetworks/subnets/joinViaServiceEndpoint/action|Associa os recursos, tais como a conta de armazenamento ou base de dados SQL para uma sub-rede.|
|/virtualNetworks/subnets/read|Obtém uma definição de sub-rede de rede virtual|
|/virtualNetworks/subnets/resourceNavigationLinks/delete|Elimina uma ligação de navegação de recursos|
|/virtualNetworks/subnets/resourceNavigationLinks/read|Obter a definição de ligação de navegação de recursos|
|/virtualNetworks/subnets/resourceNavigationLinks/write|Cria uma ligação de navegação de recursos ou atualiza uma hiperligação de navegação de recursos existente|
|/virtualNetworks/subnets/virtualMachines/read|Obtém as referências a todas as máquinas virtuais numa sub-rede de rede virtual|
|/virtualNetworks/subnets/write|Cria uma sub-rede de rede virtual ou atualiza uma sub-rede de rede virtual existente|
|/virtualNetworks/taggedTrafficConsumers/delete|Elimina um consumidor de tráfego marcado|
|/virtualNetworks/taggedTrafficConsumers/read|Obter a definição de consumidor etiquetados de tráfego|
|/virtualNetworks/taggedTrafficConsumers/validate/action|Valida um consumidor de tráfego marcado|
|/virtualNetworks/taggedTrafficConsumers/write|Cria um consumidor de tráfego etiquetados ou atualiza um consumidor de tráfego etiquetados existente|
|/virtualNetworks/usages/read|Obter as utilizações de IP para cada sub-rede da rede virtual|
|/virtualNetworks/virtualMachines/read|Obtém as referências a todas as máquinas virtuais numa rede virtual|
|/virtualNetworks/virtualNetworkPeerings/delete|Elimina um peering de rede virtual|
|/virtualNetworks/virtualNetworkPeerings/read|Obtém uma definição de peering de rede virtual|
|/virtualNetworks/virtualNetworkPeerings/write|Cria um peering de rede virtual ou atualiza um peering de rede virtual existente|
|/virtualNetworks/write|Cria uma rede virtual ou atualiza uma rede virtual existente|
|/virtualNetworkTaps/delete|Eliminar toque de rede Virtual|
|/virtualNetworkTaps/join/action|Associa um toque de rede virtual|
|/virtualNetworkTaps/read|Obter o toque de rede Virtual|
|/virtualNetworkTaps/write|Criar ou atualizar o toque de rede Virtual|
|/virtualwans/delete|Elimina um Virtual Wan|
|/virtualwans/read|Obter um Virtual Wan|
|/virtualWans/virtualHubProxies/delete|Elimina um proxy de Virtual Hub|
|/virtualWans/virtualHubProxies/read|Obtém uma definição de proxy do Virtual Hub|
|/virtualWans/virtualHubProxies/write|Cria um proxy de Virtual Hub ou atualiza um proxy de Virtual Hub|
|/virtualwans/virtualHubs/read|Obtém todos os Hubs virtuais que estão associados à Virtual Wan.|
|/virtualwans/vpnconfiguration/read|Obtém uma configuração de Vpn|
|/virtualWans/vpnSiteProxies/delete|Elimina um proxy de Vpn de Site|
|/virtualWans/vpnSiteProxies/read|Obtém uma definição de proxy de Vpn de Site|
|/virtualWans/vpnSiteProxies/write|Cria um proxy de Vpn de Site ou atualiza um proxy de Vpn de Site|
|/virtualwans/vpnSites/read|Obtém todos os Sites de VPN que estão associados à Virtual Wan.|
|/virtualwans/write|Criar ou atualizar um Virtual Wan|
|/vpnGateways/read|Obtém um VpnGateway.|
|/vpnGateways/vpnConnections/read|Obtém um VpnConnection.|
|/vpnGateways/vpnConnections/write|Coloca um VpnConnection.|
|/vpnGateways/write|Coloca um VpnGateway.|
|/vpnsites/delete|Elimina um recurso de Vpn de Site.|
|/vpnsites/read|Obtém um recurso de Vpn de Site.|
|/vpnsites/write|Cria ou atualiza um recurso de Vpn de Site.|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Operação | Descrição |
|---|---|
|/CheckNamespaceAvailability/action|Verifica se determinado nome de recurso do Espaço de nomes está ou não disponível dentro do serviço NotificationHub.|
|/Namespaces/authorizationRules/action|Obter a lista de descrição das Regras de Autorização de Espaços de nomes.|
|/Namespaces/authorizationRules/delete|Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido. |
|/Namespaces/authorizationRules/listkeys/action|Obter Cadeia de Ligação para o Espaço de nomes|
|/Namespaces/authorizationRules/read|Obter a lista de descrição das Regras de Autorização de Espaços de nomes.|
|/Namespaces/authorizationRules/regenerateKeys/action|Chave Primária/Secundária de Regeneração de Regra de Autorização de Espaço de nomes, especifica a Chave que tem de ser regenerada|
|/Namespaces/authorizationRules/write|Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas.|
|/Namespaces/CheckNotificationHubAvailability/action|Verifica se determinado nome do NotificationHub está disponível dentro do Espaço de nomes.|
|/Namespaces/Delete|Eliminar Recurso de Espaço de nomes|
|/Namespaces/NotificationHubs/authorizationRules/action|Obter lista de Regras de Autorização do Hub de Notificações|
|/Namespaces/NotificationHubs/authorizationRules/delete|Eliminar Regras de Autorização do Hub de Notificações|
|/Namespaces/NotificationHubs/authorizationRules/listkeys/action|Obter Cadeia de Ligação para o Hub de Notificações|
|/Namespaces/NotificationHubs/authorizationRules/read|Obter lista de Regras de Autorização do Hub de Notificações|
|/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action|Regra de Autorização do Hub de Notificações Regenerar Chave Primária/Secundária, especifica a Chave que tem de ser regenerada|
|/Namespaces/NotificationHubs/authorizationRules/write|Criar regras de autorização de Hub de notificação e atualizar as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas.|
|/Namespaces/NotificationHubs/debugSend/action|Enviar notificação push de teste.|
|/Namespaces/NotificationHubs/Delete|Eliminar Recurso do Hub de Notificações|
|/Namespaces/NotificationHubs/metricDefinitions/read|Obter a lista de descrições de recursos de métricas de espaço de nomes|
|/Namespaces/NotificationHubs/pnsCredentials/action|Obter todas as credenciais PNS do Hub de notificação. Isto inclui as credenciais do WNS, o MPNS, o APNS, o GCM e o Baidu|
|/Namespaces/NotificationHubs/read|Obter lista de Descrições de Recursos do Hub de Notificações|
|/Namespaces/NotificationHubs/write|Criar um Hub de notificação e atualizar as respetivas propriedades. As respetivas propriedades incluem principalmente PNS credenciais. Regras de autorização e TTL|
|/Namespaces/read|Obter a lista de Descrição de Recursos de Espaço de nomes|
|/Namespaces/write|Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas.|
|/register/action|Regista a subscrição do fornecedor de recursos de NotificationHubs e ativa a criação de Espaços de nomes e NotificationHubs|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Operação | Descrição |
|---|---|
|/linkTargets/read|Apresenta uma lista de contas existentes que não estão associadas uma subscrição do Azure. Para ligar esta subscrição do Azure a uma área de trabalho, utilize um id de cliente devolvido por esta operação na propriedade de id de cliente da operação de área de trabalho de criar.|
|/register/action|Registe uma subscrição para um fornecedor de recursos.|
|/workspaces/analytics/query/action|A procura utilizando o motor de novo.|
|/workspaces/analytics/query/schema/read|Obter o esquema de pesquisa V2.|
|/workspaces/api/query/action|A procura utilizando o motor de novo.|
|/workspaces/api/query/schema/read|Obter o esquema de pesquisa V2.|
|/workspaces/configurationScopes/delete|Eliminar âmbito de configuração|
|/workspaces/configurationScopes/read|Obter o âmbito de configuração|
|/workspaces/configurationScopes/write|Definir âmbito de configuração|
|/workspaces/datasources/delete|Elimine as origens de dados na área de trabalho.|
|/workspaces/datasources/read|Obter as origens de dados na área de trabalho.|
|/workspaces/datasources/write|Criar/atualizar as origens de dados na área de trabalho.|
|/workspaces/delete|Elimina uma área de trabalho. Se a área de trabalho foi associada a uma área de trabalho existente no momento da criação a área de trabalho que foi ligada não é eliminada.|
|/workspaces/generateregistrationcertificate/action|Gera o certificado de registo para a área de trabalho. Este certificado é utilizado para ligar o Microsoft System Center Operation Manager à área de trabalho.|
|/workspaces/intelligencepacks/disable/action|Desativa um pacote de análise para uma área de trabalho especificado.|
|/workspaces/intelligencepacks/enable/action|Permite que um pacote de análise para uma área de trabalho especificado.|
|/workspaces/intelligencepacks/read|Apresenta uma lista de todos os pacotes de intelligence que estão visíveis para um determinado worksapce e também indica se o pacote está ativado ou desativado para essa área de trabalho.|
|/workspaces/linkedServices/delete|Serviços de eliminação ligado em indicado área de trabalho.|
|/workspaces/linkedServices/read|Obter serviços ligados em fornecido a área de trabalho.|
|/workspaces/linkedServices/write|Criar/atualizar ligado serviços em atribuídos a área de trabalho.|
|/workspaces/listKeys/action|Obtém as chaves de lista para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho.|
|/workspaces/listKeys/read|Obtém as chaves de lista para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho.|
|/workspaces/managementGroups/read|Obtém os nomes e os metadados para grupos de gestão do System Center Operations Manager ligados a esta área de trabalho.|
|/workspaces/metricDefinitions/read|Obter definições de métrica na área de trabalho|
|/workspaces/notificationSettings/delete|Elimine as definições de notificação do utilizador para a área de trabalho.|
|/workspaces/notificationSettings/read|Obter definições de notificação do utilizador para a área de trabalho.|
|/workspaces/notificationSettings/write|Defina as definições de notificação do utilizador para a área de trabalho.|
|/workspaces/purge/action|Eliminar dados especificado a partir da área de trabalho|
|/workspaces/read|Obtém uma área de trabalho existente|
|/workspaces/savedSearches/delete|Elimina uma consulta de pesquisa guardada|
|/workspaces/savedSearches/read|Obtém uma consulta de pesquisa guardada|
|/workspaces/savedSearches/write|Cria uma consulta de pesquisa guardada|
|/workspaces/schema/read|Obtém o esquema de pesquisa para a área de trabalho.  Esquema de pesquisa inclui os campos expostos e os respetivos tipos.|
|/workspaces/search/action|Executa uma consulta de pesquisa|
|/workspaces/sharedKeys/action|Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho.|
|/workspaces/sharedKeys/read|Obtém as chaves partilhadas para a área de trabalho. Estas chaves são utilizadas para ligar agentes de informações operacionais do Microsoft à área de trabalho.|
|/workspaces/storageinsightconfigs/delete|Elimina uma configuração de armazenamento. Isto irá parar informações operacionais do Microsoft ao ler os dados da conta do storage.|
|/workspaces/storageinsightconfigs/read|Obtém uma configuração de armazenamento.|
|/workspaces/storageinsightconfigs/write|Cria uma nova configuração de armazenamento. Estas configurações são utilizadas para solicitar dados a partir de uma localização de uma conta de armazenamento existente.|
|/workspaces/usages/read|Obtém dados de utilização de uma área de trabalho, incluindo a quantidade de dados lidos pela área de trabalho.|
|/workspaces/write|Cria uma nova área de trabalho ou ligações para uma área de trabalho existente ao fornecer o id de cliente da área de trabalho existente.|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Operação | Descrição |
|---|---|
|/managementAssociations/delete|Eliminar a associação de gestão existente|
|/managementAssociations/read|Obtenha a associação de gestão existente|
|/managementAssociations/write|Criar uma nova associação de gestão|
|/managementConfigurations/delete|Eliminar Configuratin de gestão existente|
|/managementConfigurations/read|Obter a configuração de gestão existente|
|/managementConfigurations/write|Criar uma nova configuração de gestão|
|/register/action|Registe uma subscrição para um fornecedor de recursos.|
|/solutions/delete|Eliminar a solução existente do OMS|
|/solutions/read|Obter a sair com a solução do OMS|
|/solutions/write|Criar nova solução do OMS|

## <a name="microsoftportal"></a>Microsoft.Portal

| Operação | Descrição |
|---|---|
|/dashboards/delete|Remove o dashboard da subscrição.|
|/dashboards/read|Lê os dashboards da subscrição.|
|/dashboards/write|Adicionar ou modificar o dashboard para uma subscrição.|

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

| Operação | Descrição |
|---|---|
|/capacities/checkNameAvailability/action|Verificações de que nome do Power BI dedicado capacidade é válido e não em utilização.|
|/capacities/delete|Elimina o Power BI dedicado capacidade.|
|/capacities/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para a capacidade de dedicado do Power BI.|
|/capacities/read|Obtém as informações de que o Power BI dedicado capacidade especificada.|
|/capacities/write|Cria ou atualiza o Power BI dedicado capacidade especificada.|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Operação | Descrição |
|---|---|
|/locations/allocatedStamp/read|GetAllocatedStamp é uma operação interna utilizada pelo serviço|
|/locations/allocateStamp/action|AllocateStamp é uma operação interna utilizada pelo serviço|
|/locations/backupPreValidateProtection/action||
|/locations/backupStatus/action|Verificar o estado de cópia de segurança para os cofres dos serviços de recuperação|
|/locations/backupValidateFeatures/action|Validar funcionalidades|
|/operations/read|Operação devolve a lista de operações para um fornecedor de recursos|
|/register/action|Regista a subscrição de dado o fornecedor de recursos|
|/Vaults/backupconfig/read|Cofre dos serviços de configuração de devolve para recuperação.|
|/Vaults/backupconfig/write|Cofre dos serviços de configuração de atualizações para a recuperação.|
|/Vaults/backupEngines/read|Devolve todos os servidores de gestão de cópia de segurança registados no cofre.|
|/Vaults/backupFabrics/{fabricName}/protectionContainers/{containerName}/items/read|Obter todos os itens num contentor|
|/Vaults/backupFabrics/backupProtectionIntent/write|Criar uma cópia de segurança intenção de proteção|
|/Vaults/backupFabrics/operationResults/read|Devolve o estado da operação|
|/Vaults/backupFabrics/protectableContainers/read|Obter todos os contentores passíveis de proteção|
|/Vaults/backupFabrics/protectionContainers/inquire/action|Fazer a consulta para cargas de trabalho num contentor|
|/Vaults/backupFabrics/protectionContainers/operationResults/read|Obtém o resultado da Operação efetuada no Contentor de Proteção.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action|Executa a Cópia de Segurança do Item Protegido.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/delete|Elimina protegidos Item|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read|Obtém o Resultado da Operação Efetuada nos Itens Protegidos.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read|Devolve o estado da Operação efetuada nos Itens Protegidos.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/read|Devolve detalhes do objecto do Item protegido|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/provisionInstantItemRecovery/action|Aprovisionar Item instantâneas recuperação para o Item protegido|
|/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read|Obter Pontos de Recuperação de Itens Protegidos.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/restore/action|Restaurar Pontos de Recuperação de Itens Protegidos.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/revokeInstantItemRecovery/action|Revogar a recuperação do Item instantâneas para Item protegido|
|/Vaults/backupFabrics/protectionContainers/protectedItems/write|Criar um Item protegido cópia de segurança|
|/Vaults/backupFabrics/protectionContainers/read|Devolve todas as registado contentores|
|/Vaults/backupFabrics/protectionContainers/write|Cria um contentor registado|
|/Vaults/backupFabrics/refreshContainers/action|Atualiza a lista de contentor|
|/Vaults/backupJobs/cancel/action|Cancelar a tarefa|
|/Vaults/backupJobs/operationResults/read|Devolve o Resultado da Operação de Tarefa.|
|/Vaults/backupJobs/read|Devolve todos os objetos da tarefa|
|/Vaults/backupJobsExport/action|Tarefas de exportação|
|/Vaults/backupJobsExport/operationResults/read|Devolve o resultado da operação de tarefa de exportação.|
|/Vaults/backupManagementMetaData/read|Devolve os Metadados da Gestão da Cópia de Segurança do Cofre de Serviços de Recuperação.|
|/Vaults/backupOperationResults/read|Devolve o Resultado da Operação da Cópia de Segurança do Cofre de Serviços de Recuperação.|
|/Vaults/backupOperations/read|Devolve a operação de cópia de segurança cofre dos serviços de estado de recuperação.|
|/Vaults/backupPolicies/delete|Eliminar uma política de proteção|
|/Vaults/backupPolicies/operationResults/read|Obter Resultados da Operação de Política.|
|/Vaults/backupPolicies/operations/read|Obter o estado da operação de política.|
|/Vaults/backupPolicies/read|Devolve todas as políticas de proteção|
|/Vaults/backupPolicies/write|Cria a política de proteção|
|/Vaults/backupProtectableItems/read|Devolve a lista de todos os Itens Susceptíveis de Proteção.|
|/Vaults/backupProtectedItems/read|Devolve a lista de todos os Itens Protegidos.|
|/Vaults/backupProtectionContainers/read|Devolve todos os contentores que pertence à subscrição|
|/Vaults/backupSecurityPIN/action|Devolve a segurança PIN Cofre de serviços de informação para recuperação.|
|/Vaults/backupstorageconfig/read|Devolve a configuração de armazenamento para a recuperação cofre dos serviços.|
|/Vaults/backupstorageconfig/write|Cofre dos serviços de configuração de armazenamento de atualizações para a recuperação.|
|/Vaults/backupUsageSummaries/read|Devolve resumos de itens protegidos e os servidores protegidos dos serviços de recuperação.|
|/Vaults/certificates/write|A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre.|
|/Vaults/delete|A operação eliminar cofre elimina os recursos do Azure especificado do tipo 'cofre'|
|/Vaults/extendedInformation/delete|A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure|
|/Vaults/extendedInformation/read|A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure|
|/Vaults/extendedInformation/write|A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure|
|/Vaults/monitoringAlerts/read|Obtém os alertas para o Cofre de serviços de recuperação.|
|/Vaults/monitoringAlerts/write|Resolve o alerta.|
|/Vaults/monitoringConfigurations/read|Obtém a configuração de notificação de Cofre de serviços de recuperação.|
|/Vaults/monitoringConfigurations/write|Configura as notificações por correio eletrónico para o Cofre de serviços de recuperação.|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/read|Diagnóstico de cópia de segurança do Azure|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/write|Diagnóstico de cópia de segurança do Azure|
|/Vaults/providers/Microsoft.Insights/logDefinitions/read|Registos de cópia de segurança do Azure|
|/Vaults/providers/Microsoft.Insights/metricDefinitions/read|Métricas de cópia de segurança do Azure|
|/Vaults/read|A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre'|
|/Vaults/registeredIdentities/delete|A operação de anular o registo do contentor pode ser utilizada para anular o registo de um contentor.|
|/Vaults/registeredIdentities/operationResults/read|A obter os resultados da operação operação pode ser utilizada obter o estado da operação e o resultado para a operação assíncrona foi submetido|
|/Vaults/registeredIdentities/read|Os contentores obter operação pode ser utilizada obter contentores registados para um recurso.|
|/Vaults/registeredIdentities/write|A operação de registar o contentor de serviço pode ser utilizada para registar um contentor com o serviço de recuperação.|
|/vaults/replicationAlertSettings/read|Ler as definições de alertas|
|/vaults/replicationAlertSettings/write|Criar ou atualizar as definições de alertas|
|/vaults/replicationEvents/read|Ler quaisquer eventos|
|/vaults/replicationFabrics/checkConsistency/action|Verifica a Consistência dos Recursos de infraestrutura|
|/vaults/replicationFabrics/delete|Elimine quaisquer recursos de infraestrutura|
|/vaults/replicationFabrics/deployProcessServerImage/action|Implementar a imagem de servidor de processo|
|/vaults/replicationFabrics/read|Ler todos os recursos de infraestrutura|
|/vaults/replicationFabrics/reassociateGateway/action|Reassociar Gateway|
|/vaults/replicationFabrics/remove/action|Remover recursos de infraestrutura|
|/vaults/replicationFabrics/renewcertificate/action|Renovar o certificado para recursos de infraestrutura|
|/vaults/replicationFabrics/replicationNetworks/read|Ler a quaisquer redes|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete|Eliminar quaisquer mapeamentos de rede|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read|Leia os mapeamentos de rede|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write|Criar ou atualizar quaisquer mapeamentos de rede|
|/vaults/replicationFabrics/replicationProtectionContainers/ discoverProtectableItem/action|Detetar o Item Protegível|
|/vaults/replicationFabrics/replicationProtectionContainers/read|Leu os contentores de proteção|
|/vaults/replicationFabrics/replicationProtectionContainers/remove/action|Remover o contentor de proteção|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectableItems/read|Ler quaisquer itens passíveis de proteção|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/applyRecoveryPoint/action|Aplicar o ponto de recuperação|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/delete|Eliminar os itens protegidos|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/failoverCommit/action|Consolidação de ativação pós-falha|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/plannedFailover/action|Ativação pós-falha planeada|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/read|Ler os itens protegidos|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/recoveryPoints/read|Pontos de recuperação de replicação de leitura|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/remove/action|Remover o Item protegido|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/repairReplication/action|Reparar a replicação|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/reProtect/action|Proteja o Item protegido|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/testFailover/action|Ativação pós-falha de teste|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/testFailoverCleanup/action|Limpeza da ativação pós-falha de teste|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/unplannedFailover/action|Ativação pós-falha|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/updateMobilityService/action|Serviço de mobilidade de atualização|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/write|Criar ou atualizar quaisquer itens protegidos|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/delete|Eliminar quaisquer mapeamentos de contentor de proteção|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/read|Ler quaisquer mapeamentos de contentor de proteção|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/remove/action|Remova o mapeamento de contentor de proteção|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/write|Criar ou atualizar quaisquer mapeamentos de contentor de proteção|
|/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action|Contentor de proteção do comutador|
|/vaults/replicationFabrics/replicationProtectionContainers/write|Criar ou atualizar quaisquer contentores de proteção|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete|Eliminar quaisquer fornecedores de serviços de recuperação|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/read|Ler quaisquer fornecedores de serviços de recuperação|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/ refreshProvider/action|Atualize o fornecedor|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action|Remover o fornecedor de serviços de recuperação|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/write|Criar ou atualizar quaisquer fornecedores de serviços de recuperação|
|/vaults/replicationFabrics/replicationStorageClassifications/read|Ler as classificações de armazenamento|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/delete|Eliminar quaisquer mapeamentos de classificação de armazenamento|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/read|Ler quaisquer mapeamentos de classificação de armazenamento|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/write|Criar ou atualizar quaisquer mapeamentos de classificação de armazenamento|
|/vaults/replicationFabrics/replicationvCenters/delete|Eliminar todas as tarefas|
|/vaults/replicationFabrics/replicationvCenters/read|Ler todas as tarefas|
|/vaults/replicationFabrics/replicationvCenters/write|Criar ou atualizar todas as tarefas|
|/vaults/replicationFabrics/write|Criar ou atualizar quaisquer recursos de infraestrutura|
|/vaults/replicationJobs/cancel/action|Cancelar a tarefa|
|/vaults/replicationJobs/read|Ler todas as tarefas|
|/vaults/replicationJobs/restart/action|Reinicie a tarefa|
|/vaults/replicationJobs/resume/action|Retomar a tarefa|
|/vaults/replicationPolicies/delete|Eliminar todas as políticas|
|/vaults/replicationPolicies/read|Ler todas as políticas|
|/vaults/replicationPolicies/write|Criar ou atualizar as políticas|
|/vaults/replicationRecoveryPlans/delete|Eliminar quaisquer planos de recuperação|
|/vaults/replicationRecoveryPlans/failoverCommit/action|Plano de recuperação de consolidação de ativação pós-falha|
|/vaults/replicationRecoveryPlans/plannedFailover/action|Plano de recuperação de ativação pós-falha planeada|
|/vaults/replicationRecoveryPlans/read|Leia os planos de recuperação|
|/vaults/replicationRecoveryPlans/reProtect/action|Proteja o plano de recuperação|
|/vaults/replicationRecoveryPlans/testFailover/action|Plano de recuperação de ativação pós-falha de teste|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/action|Plano de recuperação de limpeza da ativação pós-falha de teste|
|/vaults/replicationRecoveryPlans/unplannedFailover/action|Plano de recuperação de ativação pós-falha|
|/vaults/replicationRecoveryPlans/write|Criar ou atualizar quaisquer planos de recuperação|
|/Vaults/tokenInfo/read|Devolve informações sobre o token para o Cofre de serviços de recuperação.|
|/vaults/usages/read|Ler as utilizações do Cofre|
|/Vaults/usages/read|Devolve detalhes de utilização de um Cofre de Serviços de Recuperação.|
|/Vaults/vaultTokens/read|A operação de Token de cofre pode ser utilizada para obter Token do cofre para operações de back-end ao nível do cofre.|
|/Vaults/write|A operação Criar Cofre cria um recurso do tipo "cofre" do Azure|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Operação | Descrição |
|---|---|
|/checkNameAvailability/action|Verifica a disponibilidade do espaço de nomes em determinada subscrição.|
|/checkNamespaceAvailability/action|Verifica a disponibilidade do espaço de nomes em determinada subscrição. Esta API foi preterida utilize CheckNameAvailabiltiy em vez disso.|
|/namespaces/authorizationRules/action|Regra de autorização de espaço de nomes de atualizações. Esta API está depricated. Para atualizar a regra de autorização de espaço de nomes em vez disso, utilize uma chamada PUT... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/authorizationRules/delete|Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido. |
|/namespaces/authorizationRules/listkeys/action|Obter Cadeia de Ligação para o Espaço de nomes|
|/namespaces/authorizationRules/read|Obter a lista de descrição das Regras de Autorização de Espaços de nomes.|
|/namespaces/authorizationRules/regenerateKeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/authorizationRules/write|Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas.|
|/namespaces/Delete|Eliminar Recurso de Espaço de nomes|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Obtém a autorização de chaves de regras para o espaço de nomes de principal de recuperação após desastre|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Obter as regras de autorização do desastre recuperação primário espaço de nomes|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Desativa a Recuperação Após Desastre e para a replicação de alterações de espaço de nomes principais para secundários.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Verificações de disponibilidade do espaço de nomes de alias em dada subscrição.|
|/namespaces/disasterRecoveryConfigs/delete|Elimina a configuração de recuperação após desastre associada com o espaço de nomes. Esta operação só pode ser invocada através do espaço de nomes primário.|
|/namespaces/disasterRecoveryConfigs/failover/action|Invoca uma ativação pós-falha GEO DR e reconfigura o alias de espaço de nomes para apontar para o espaço de nomes secundário.|
|/namespaces/disasterRecoveryConfigs/read|Obtém a configuração da Recuperação Após Desastre associada ao espaço de nomes.|
|/namespaces/disasterRecoveryConfigs/write|Cria ou Atualiza a configuração da Recuperação Após Desastre associada ao espaço de nomes.|
|/namespaces/HybridConnections/authorizationRules/action|Operação atualizar HybridConnection. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT.|
|/namespaces/HybridConnections/authorizationRules/delete|Operação para eliminar as regras de autorização de HybridConnection|
|/namespaces/HybridConnections/authorizationRules/listkeys/action|Obter a cadeia de ligação para HybridConnection|
|/namespaces/HybridConnections/authorizationRules/read| Obter a lista de regras de autorização de HybridConnection|
|/namespaces/HybridConnections/authorizationRules/regeneratekeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/HybridConnections/authorizationRules/write|Criar regras de autorização de HybridConnection e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados.|
|/namespaces/HybridConnections/Delete|Operação para eliminar recurso HybridConnection|
|/namespaces/HybridConnections/read|Obter a lista de descrições de recurso HybridConnection|
|/namespaces/HybridConnections/write|Criar ou atualizar HybridConnection propriedades.|
|/namespaces/messagingPlan/read|Obtém o plano de serviço de mensagens para um espaço de nomes. Esta API foi preterida. As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/messagingPlan/write|Atualiza o serviço de mensagens planear um espaço de nomes. Esta API foi preterida. As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/operationresults/read|Obter o estado da operação de Espaço de nomes|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de descrições de recursos de métricas de espaço de nomes|
|/namespaces/read|Obter a lista de Descrição de Recursos de Espaço de nomes|
|/namespaces/WcfRelays/authorizationRules/action|Operação atualizar WcfRelay. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT.|
|/namespaces/WcfRelays/authorizationRules/delete|Operação para eliminar as regras de autorização de WcfRelay|
|/namespaces/WcfRelays/authorizationRules/listkeys/action|Obter a cadeia de ligação para WcfRelay|
|/namespaces/WcfRelays/authorizationRules/read| Obter a lista de regras de autorização de WcfRelay|
|/namespaces/WcfRelays/authorizationRules/regeneratekeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/WcfRelays/authorizationRules/write|Criar regras de autorização de WcfRelay e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados.|
|/namespaces/WcfRelays/Delete|Operação para eliminar recurso WcfRelay|
|/namespaces/WcfRelays/read|Obter a lista de descrições de recurso WcfRelay|
|/namespaces/WcfRelays/write|Criar ou atualizar WcfRelay propriedades.|
|/namespaces/write|Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas.|
|/operations/read|Obter operações|
|/register/action|Regista a subscrição do fornecedor de recursos de Reencaminhamento e ativa a criação de recursos de Reencaminhamento|
|/unregister/action|Regista a subscrição do fornecedor de recursos de Reencaminhamento e ativa a criação de recursos de Reencaminhamento|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Operação | Descrição |
|---|---|
|/AvailabilityStatuses/current/read|Obtém o estado de disponibilidade para o recurso especificado.|
|/AvailabilityStatuses/read|Obtenha os estados de disponibilidade para todos os recursos no âmbito especificado|
|/healthevent/action|Indica a alteração no estado de funcionamento para o recurso especificado|
|/healthevent/Activated/action|Indica a alteração no estado de funcionamento para o recurso especificado|
|/healthevent/InProgress/action|Indica a alteração no estado de funcionamento para o recurso especificado|
|/healthevent/Pending/Action|Indica a alteração no estado de funcionamento para o recurso especificado|
|/healthevent/resolved/Action|Indica a alteração no estado de funcionamento para o recurso especificado|
|/healthevent/Updated/action|Indica a alteração no estado de funcionamento para o recurso especificado|
|/register/action|Regista a subscrição para o Microsoft ResourceHealth|

## <a name="microsoftresources"></a>Microsoft.Resources

| Operação | Descrição |
|---|---|
|/checkResourceName/action|Verificar a validade do nome do recurso.|
|/deployments/cancel/action|Cancela uma implementação.|
|/deployments/delete|Elimina uma implementação.|
|/deployments/operations/read|Obtém ou lista as operações de implementação.|
|/deployments/read|Obtém ou lista implementações.|
|/deployments/validate/action|Valida uma implementação.|
|/deployments/write|Cria ou atualiza uma implementação.|
|/links/delete|Elimina uma ligação de recursos.|
|/links/read|Obtém ou lista ligações de recursos.|
|/links/write|Cria ou atualiza uma ligação de recursos.|
|/marketplace/purchase/action|Adquire um recurso do mercado.|
|/providers/read|Obter a lista de fornecedores.|
|/resources/read|Obter a lista de recursos com base em filtros.|
|/subscriptions/locations/read|Obtém a lista de localizações suportadas.|
|/subscriptions/operationresults/read|Obter os resultados da operação de subscrição.|
|/subscriptions/providers/read|Obtém ou lista fornecedores de recursos.|
|/subscriptions/read|Obtém a lista de subscrições.|
|/subscriptions/resourceGroups/delete|Elimina um grupo de recursos e respetivos recursos.|
|/subscriptions/resourcegroups/deployments/operations/read|Obtém ou lista as operações de implementação.|
|/subscriptions/resourcegroups/deployments/operationstatuses/read|Obtém ou lista os estados da operação de implementação.|
|/subscriptions/resourcegroups/deployments/read|Obtém ou lista implementações.|
|/subscriptions/resourcegroups/deployments/write|Cria ou atualiza uma implementação.|
|/subscriptions/resourceGroups/moveResources/action|Move os recursos de um grupo de recursos para outro.|
|/subscriptions/resourceGroups/read|Obtém ou lista os grupos de recursos.|
|/subscriptions/resourcegroups/resources/read|Obtém os recursos do grupo de recursos.|
|/subscriptions/resourceGroups/validateMoveResources/action|Validar a movimentação de recursos de um grupo de recursos para outro.|
|/subscriptions/resourceGroups/write|Cria ou atualiza um grupo de recursos.|
|/subscriptions/resources/read|Obtém os recursos de uma subscrição.|
|/subscriptions/tagNames/delete|Elimina uma etiqueta de subscrição.|
|/subscriptions/tagNames/read|Obtém ou lista etiquetas de subscrição.|
|/subscriptions/tagNames/tagValues/delete|Elimina um valor de etiqueta de subscrição.|
|/subscriptions/tagNames/tagValues/read|Obtém ou lista valores de etiqueta de subscrição.|
|/subscriptions/tagNames/tagValues/write|Adiciona um valor de etiqueta de subscrição.|
|/subscriptions/tagNames/write|Adiciona uma etiqueta de subscrição.|
|/tenants/read|Obtém a lista de inquilinos.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Operação | Descrição |
|---|---|
|/jobcollections/delete|Elimina a coleção de tarefas.|
|/jobcollections/disable/action|Desativa a coleção de tarefas.|
|/jobcollections/enable/action|Ativa a coleção de tarefas.|
|/jobcollections/jobs/delete|Elimina a tarefa.|
|/jobcollections/jobs/generateLogicAppDefinition/action|Gera a definição da Aplicação Lógica com base numa Tarefa do Scheduler.|
|/jobcollections/jobs/jobhistories/read|Obtém o histórico de tarefas.|
|/jobcollections/jobs/read|Obtém a tarefa.|
|/jobcollections/jobs/run/action|Executa a tarefa.|
|/jobcollections/jobs/write|Cria ou atualiza a tarefa.|
|/jobcollections/read|Obter Coleção de Tarefas|
|/jobcollections/write|Cria ou atualiza a coleção de tarefas.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Operação | Descrição |
|---|---|
|/checkNameAvailability/action|Verifica a disponibilidade do nome do serviço.|
|/register/action|Regista a subscrição para o fornecedor de recursos de pesquisa e permite a criação de serviços de pesquisa.|
|/searchServices/createQueryKey/action|Cria a chave de consulta.|
|/searchServices/delete|Elimina o serviço de pesquisa.|
|/searchServices/diagnosticSettings/read|Obtém o diganostic definição de leitura para o recurso|
|/searchServices/diagnosticSettings/write|Cria ou atualiza a definição de diganostic para o recurso|
|/searchServices/listAdminKeys/action|Lê as chaves de administração.|
|/searchServices/logDefinitions/read|Obtém os registos disponíveis para o serviço de pesquisa|
|/searchServices/metricDefinitions/read|Obtem as métricas disponíveis para o serviço de pesquisa|
|/searchServices/queryKey/delete|Elimina a chave de consulta.|
|/searchServices/queryKey/read|Lê as chaves de consulta.|
|/searchServices/read|Lê o serviço de pesquisa.|
|/searchServices/regenerateAdminKey/action|A chave de administrador de gera de novo.|
|/searchServices/start/action|Inicia o serviço de pesquisa.|
|/searchServices/stop/action|Para o serviço de pesquisa.|
|/searchServices/write|Cria ou atualiza o serviço de pesquisa.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Operação | Descrição |
|---|---|
|/alerts/read|Obtém todos os alertas de segurança disponíveis|
|/applicationWhitelistings/read|Obtém o whitelistings de aplicação|
|/applicationWhitelistings/write|Cria uma nova aplicação a listas brancas ou atualiza um já existente|
|/complianceResults/read|Obtém os resultados de compatibilidade para o recurso|
|/locations/alerts/activate/action|Ativar um alerta de segurança|
|/locations/alerts/dismiss/action|Dispensar um alerta de segurança|
|/locations/alerts/read|Obtém todos os alertas de segurança disponíveis|
|/locations/jitNetworkAccessPolicies/initiate/action|Inicia uma política de acesso de rede de just-in-time|
|/locations/jitNetworkAccessPolicies/read|Obtém as políticas de acesso de rede de just-in-time|
|/locations/jitNetworkAccessPolicies/write|Cria uma nova política de acesso de rede de just-in-time ou atualiza um já existente|
|/locations/read|Obtém a localização de dados de segurança|
|/locations/tasks/activate/action|Ativar uma recomendação de segurança|
|/locations/tasks/dismiss/action|Ignorar uma recomendação de segurança|
|/locations/tasks/read|Obtém todas as recomendações de segurança disponíveis|
|/locations/tasks/resolve/action|Resolver uma recomendação de segurança|
|/locations/tasks/start/action|Iniciar uma recomendação de segurança|
|/policies/read|Obtém a política de segurança|
|/policies/write|Atualiza a política de segurança|
|/pricings/delete|Elimina as definições de preços para o âmbito|
|/pricings/read|Obtém as definições de preços para o âmbito|
|/pricings/write|Atualiza as definições de preços para o âmbito|
|/register/action|Regista a subscrição para o Centro de segurança do Azure|
|/securityContacts/delete|Elimina o contacto de segurança|
|/securityContacts/read|Obtém o contacto de segurança|
|/securityContacts/write|O contacto de segurança de atualizações|
|/securitySolutions/delete|Elimina uma solução de segurança|
|/securitySolutions/read|Obtém as soluções de segurança|
|/securitySolutions/write|Cria uma nova solução de segurança ou atualiza um já existente|
|/securitySolutionsReferenceData/read|Obtém as soluções de segurança de dados de referência|
|/securityStatuses/read|Obtém a segurança Estados de funcionamento de recursos do Azure|
|/securityStatusesSummaries/read|Obtém a segurança resumos de Estados para o âmbito|
|/tasks/read|Obtém todas as recomendações de segurança disponíveis|
|/webApplicationFirewalls/delete|Elimina uma firewall de aplicação web|
|/webApplicationFirewalls/read|Obtém a web firewalls de aplicação|
|/webApplicationFirewalls/write|Cria uma nova firewall de aplicação web ou atualiza um já existente|
|/workspaceSettings/connect/action|Alterar as definições de restabelecimento de ligação de definições de área de trabalho|
|/workspaceSettings/delete|Elimina as definições da área de trabalho|
|/workspaceSettings/read|Obtém as definições da área de trabalho|
|/workspaceSettings/write|Atualiza as definições da área de trabalho|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Operação | Descrição |
|---|---|
|/checkNameAvailability/action|Verifica a disponibilidade do espaço de nomes em determinada subscrição.|
|/checkNamespaceAvailability/action|Verifica a disponibilidade do espaço de nomes em determinada subscrição. Esta API foi preterida utilize CheckNameAvailabiltiy em vez disso.|
|/namespaces/authorizationRules/action|Regra de autorização de espaço de nomes de atualizações. Esta API está depricated. Para atualizar a regra de autorização de espaço de nomes em vez disso, utilize uma chamada PUT... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/authorizationRules/delete|Elimine regra de autorização de espaço de nomes. Não é possível eliminar a regra de autorização de espaço de nomes predefinido. |
|/namespaces/authorizationRules/listkeys/action|Obter Cadeia de Ligação para o Espaço de nomes|
|/namespaces/authorizationRules/read|Obter a lista de descrição das Regras de Autorização de Espaços de nomes.|
|/namespaces/authorizationRules/regenerateKeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/authorizationRules/write|Criar regras de autorização ao nível de espaço de nomes e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização, o site primário e as chaves secundárias podem ser atualizadas.|
|/namespaces/Delete|Eliminar Recurso de Espaço de nomes|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Obtém a autorização de chaves de regras para o espaço de nomes de principal de recuperação após desastre|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Obter as regras de autorização do desastre recuperação primário espaço de nomes|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Desativa a Recuperação Após Desastre e para a replicação de alterações de espaço de nomes principais para secundários.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Verificações de disponibilidade do espaço de nomes de alias em dada subscrição.|
|/namespaces/disasterRecoveryConfigs/delete|Elimina a configuração de recuperação após desastre associada com o espaço de nomes. Esta operação só pode ser invocada através do espaço de nomes primário.|
|/namespaces/disasterRecoveryConfigs/failover/action|Invoca uma ativação pós-falha GEO DR e reconfigura o alias de espaço de nomes para apontar para o espaço de nomes secundário.|
|/namespaces/disasterRecoveryConfigs/read|Obtém a configuração da Recuperação Após Desastre associada ao espaço de nomes.|
|/namespaces/disasterRecoveryConfigs/write|Cria ou Atualiza a configuração da Recuperação Após Desastre associada ao espaço de nomes.|
|/namespaces/eventGridFilters/delete|Elimina o filtro de eventos grelha associado com o espaço de nomes.|
|/namespaces/eventGridFilters/read|Obtém o filtro de eventos grelha associado com o espaço de nomes.|
|/namespaces/eventGridFilters/write|Cria ou atualiza o filtro de eventos grelha associado com o espaço de nomes.|
|/namespaces/eventhubs/read|Obter a lista de descrições de recursos de EventHub|
|/namespaces/messagingPlan/read|Obtém o plano de serviço de mensagens para um espaço de nomes. Esta API foi preterida. As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/messagingPlan/write|Atualiza o serviço de mensagens planear um espaço de nomes. Esta API foi preterida. As propriedades expostas através do recurso MessagingPlan são movidas para o recurso de espaço de nomes em versões posteriores de API do (principal)... Esta operação não é suportada na API versão 2017-04-01.|
|/namespaces/migrate/action|Migrar operação de espaço de nomes|
|/namespaces/operationresults/read|Obter o estado da operação de Espaço de nomes|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/read|Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/write|Obter a lista de descrições de recursos de definições de diagnóstico de espaço de nomes|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Obter a lista de registos de espaço de nomes descrições de recursos|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de descrições de recursos de métricas de espaço de nomes|
|/namespaces/queues/authorizationRules/action|Operação para atualizar a fila. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT.|
|/namespaces/queues/authorizationRules/delete|Operação para eliminar as regras de autorização de fila|
|/namespaces/queues/authorizationRules/listkeys/action|Obter a cadeia de ligação para a fila|
|/namespaces/queues/authorizationRules/read| Obter a lista de regras de autorização de fila|
|/namespaces/queues/authorizationRules/regenerateKeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/queues/authorizationRules/write|Criar regras de autorização de fila e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados.|
|/namespaces/queues/Delete|Operação para eliminar os recursos da fila|
|/namespaces/queues/read|Obter a lista de descrições de recursos da fila|
|/namespaces/queues/write|Criar ou as propriedades da fila de atualização.|
|/namespaces/read|Obter a lista de Descrição de Recursos de Espaço de nomes|
|/namespaces/topics/authorizationRules/action|Operação para atualizar o tópico. Esta operação não é suportada na API versão 2017-04-01. Regras de autorização. Ao atualizar a regra de autorização, utilize uma chamada PUT.|
|/namespaces/topics/authorizationRules/delete|Operação para eliminar as regras de autorização de tópico|
|/namespaces/topics/authorizationRules/listkeys/action|Obter a cadeia de ligação de tópico|
|/namespaces/topics/authorizationRules/read| Obter a lista de regras de autorização de tópico|
|/namespaces/topics/authorizationRules/regenerateKeys/action|Regenerar a chave Primária ou Secundária do Recurso|
|/namespaces/topics/authorizationRules/write|Criar regras de autorização de tópico e Atualize as respetivas propriedades. Os direitos de acesso de regras de autorização pode ser atualizados.|
|/namespaces/topics/Delete|Operação para eliminar o recurso de tópico|
|/namespaces/topics/read|Obter a lista de descrições de recursos de tópico|
|/namespaces/topics/subscriptions/Delete|Operação para eliminar recurso TopicSubscription|
|/namespaces/topics/subscriptions/read|Obter a lista de descrições de recurso TopicSubscription|
|/namespaces/topics/subscriptions/rules/Delete|Operação para eliminar o recurso de regra|
|/namespaces/topics/subscriptions/rules/read|Obter a lista de descrições de recurso de regra|
|/namespaces/topics/subscriptions/rules/write|Criar ou propriedades da regra de atualização.|
|/namespaces/topics/subscriptions/write|Criar ou atualizar TopicSubscription propriedades.|
|/namespaces/topics/write|Criar ou propriedades de tópico de atualização.|
|/namespaces/write|Crie um recurso de espaço de nomes e Atualize as respetivas propriedades. As etiquetas e a capacidade do espaço de nomes são as propriedades que podem ser atualizadas.|
|/operations/read|Obter operações|
|/register/action|Regista a subscrição do fornecedor de recursos do ServiceBus e ativa a criação de recursos do ServiceBus|
|/sku/read|Obter a lista de descrições de recursos de Sku|
|/sku/regions/read|Obter a lista de descrições de recurso SkuRegions|
|/unregister/action|Regista a subscrição do fornecedor de recursos do ServiceBus e ativa a criação de recursos do ServiceBus|

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

| Operação | Descrição |
|---|---|
|/clusters/applications/delete|Eliminar qualquer Aplicação|
|/clusters/applications/read|Ler qualquer Aplicação|
|/clusters/applications/services/delete|Eliminar qualquer Serviço|
|/clusters/applications/services/partitions/read|Ler qualquer Partição|
|/clusters/applications/services/partitions/replicas/read|Ler qualquer Réplica|
|/clusters/applications/services/read|Ler qualquer Serviço|
|/clusters/applications/services/statuses/read|Ler qualquer Estado do Serviço|
|/clusters/applications/services/write|Criar ou Atualizar qualquer Serviço|
|/clusters/applications/write|Criar ou Atualizar qualquer Aplicação|
|/clusters/applicationTypes/delete|Eliminar qualquer Tipo de Aplicação|
|/clusters/applicationTypes/read|Ler qualquer Tipo de Aplicação|
|/clusters/applicationTypes/versions/delete|Eliminar qualquer Versão do Tipo de Aplicação|
|/clusters/applicationTypes/versions/read|Ler qualquer Versão do Tipo de Aplicação|
|/clusters/applicationTypes/versions/write|Criar ou Atualizar qualquer Versão do Tipo de Aplicação|
|/clusters/applicationTypes/write|Criar ou Atualizar qualquer Tipo de Aplicação|
|/clusters/delete|Eliminar qualquer Cluster|
|/clusters/nodes/read|Ler qualquer Nó|
|/clusters/read|Ler qualquer Cluster|
|/clusters/statuses/read|Ler qualquer Estado do Cluster|
|/clusters/write|Criar ou Atualizar qualquer Cluster|
|/locations/clusterVersions/read|Ler qualquer Versão de Cluster|
|/locations/environments/clusterVersions/read|Ler qualquer Versão de Cluster de um ambiente específico|
|/locations/operationresults/read|Ler quaisquer Resultados da Operação|
|/locations/operations/read|Ler quaisquer Operações por localização|
|/operations/read|Ler quaisquer Operações Disponíveis|
|/register/action|Registar qualquer Ação|

## <a name="microsoftsolutions"></a>Microsoft.Solutions

| Operação | Descrição |
|---|---|
|/applicationDefinitions/delete|Remove uma definição de aplicação.|
|/applicationDefinitions/read|Obtém uma lista de definições de aplicação.|
|/applicationDefinitions/write|Adicionar ou modificar uma definição de aplicação.|
|/applications/delete|Remove uma aplicação.|
|/applications/read|Obtém uma lista de aplicações.|
|/applications/write|Cria uma aplicação.|
|/locations/operationStatuses/read|Lê o estado da operação do recurso.|
|/register/action|Registe-se nas Soluções.|

## <a name="microsoftsql"></a>Microsoft.Sql

| Operação | Descrição |
|---|---|
|/checkNameAvailability/action|Certifique-se de que o se nome do servidor é disponível para o aprovisionamento em todo o mundo, para uma determinada subscrição.|
|/locations/auditingSettingsAzureAsyncOperation/read|Obter o resultado do blob servidor expandida operação de definição de política de auditoria|
|/locations/auditingSettingsOperationResults/read|Obter o resultado do blob servidor operação de definição de política de auditoria|
|/locations/capabilities/read|Obtém as capacidades para esta subscrição numa localização especificada|
|/locations/databaseAzureAsyncOperation/read|Obtém o estado de uma operação de base de dados.|
|/locations/databaseOperationResults/read|Obtém o estado de uma operação de base de dados.|
|/locations/deletedServerAsyncOperation/read|Obtém em curso operações no servidor eliminado|
|/locations/deletedServerOperationResults/read|Obtém em curso operações no servidor eliminado|
|/locations/deletedServers/read|Devolva a lista de servidores de eliminado ou obtém as propriedades para o servidor especificado foi eliminado.|
|/locations/deletedServers/recover/action|Recuperar um servidor eliminado|
|/locations/deleteVirtualNetworkOrSubnets/action|Elimina as regras de rede Virtual associadas a uma rede virtual ou a sub-rede|
|/locations/elasticPoolAzureAsyncOperation/read|Obtém a operação assíncrona do azure para uma operação assíncrona do conjunto elástico|
|/locations/elasticPoolOperationResults/read|Obtém o resultado de uma operação de agrupamento elástico.|
|/locations/extendedAuditingSettingsAzureAsyncOperation/read|Obter o resultado do blob servidor expandida operação de definição de política de auditoria|
|/locations/extendedAuditingSettingsOperationResults/read|Obter o resultado do blob servidor expandida operação de definição de política de auditoria|
|/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action|Conclusão da operação de restauro de base de dados gerida|
|/locations/managedTransparentDataEncryptionAzureAsyncOperation/read|Obtém em curso operações de encriptação transparente de dados de base de dados gerida|
|/locations/managedTransparentDataEncryptionOperationResults/read|Obtém em curso operações de encriptação transparente de dados de base de dados gerida|
|/locations/read|Obtém as localizações disponíveis para uma determinada subscrição|
|/locations/syncAgentOperationResults/read|Obter o resultado da operação de recursos do agente de sincronização|
|/locations/syncDatabaseIds/read|Obter os ids de base de dados de sincronização para uma subscrição e região específica|
|/locations/syncGroupOperationResults/read|Obter o resultado da operação de recursos do grupo de sincronização|
|/locations/syncMemberOperationResults/read|Obter o resultado da operação de recursos do membro de sincronização|
|/locations/usages/read|Obtém uma coleção de métrica de utilização para esta subscrição numa localização|
|/locations/virtualNetworkRulesAzureAsyncOperation/read|Devolve os detalhes das regras de rede virtual especificado operação assíncrona do azure |
|/locations/virtualNetworkRulesOperationResults/read|Devolve os detalhes da operação de regras de rede virtual especificado |
|/managedInstances/administrators/delete|Elimina um administrador existente da instância gerido.|
|/managedInstances/administrators/read|Obtém uma lista de administradores de instância gerido.|
|/managedInstances/administrators/write|Cria ou atualiza o administrador de instância geridos com os parâmetros especificados.|
|/managedInstances/databases/delete|Elimina uma base de dados gerido|
|/managedInstances/databases/read|Obtém existente gerido da base de dados|
|/managedInstances/databases/securityAlertPolicies/read|Obter os detalhes da política de deteção de ameaças da base de dados configurados numa determinada base de dados gerido|
|/managedInstances/databases/securityAlertPolicies/write|Alterar a política de deteção de ameaças de base de dados para uma determinada base de dados gerido|
|/managedInstances/databases/securityEvents/read|Obtém os eventos de segurança de base de dados gerida|
|/managedInstances/databases/transparentDataEncryption/read|Obter os detalhes da base de dados a encriptação transparente de dados numa determinada base de dados gerido|
|/managedInstances/databases/transparentDataEncryption/write|Alterar a base de dados a encriptação transparente de dados para uma determinada base de dados gerido|
|/managedInstances/databases/write|Cria uma nova base de dados ou atualiza uma base de dados existente.|
|/managedInstances/delete|Elimina uma instância existente gerida.|
|/managedInstances/metricDefinitions/read|Obter as definições de métrica de instância gerido|
|/managedInstances/metrics/read|Obter métricas de instância gerido|
|/managedInstances/read|Devolva a lista de instâncias geridas ou obtém as propriedades para a instância de gerido especificada.|
|/managedInstances/securityAlertPolicies/read|Obter os detalhes da política de deteção de ameaças de servidor gerido configurado num determinado servidor gerido|
|/managedInstances/securityAlertPolicies/write|Alterar a política de deteção de ameaças de servidor gerido para um determinado servidor gerido|
|/managedInstances/write|Cria uma instância gerida com os parâmetros especificados ou atualizar as propriedades ou etiquetas para a instância de gerido especificada.|
|/operations/read|Obtém as operações REST disponíveis|
|/register/action|Regista a subscrição para o fornecedor de recursos de base de dados do Microsoft SQL Server e permite a criação de bases de dados do Microsoft SQL Server.|
|/servers/administratorOperationResults/read|Operações em curso obtém os administradores de servidores|
|/servers/administrators/delete|Eliminar o administrador do servidor|
|/servers/administrators/read|Obter os detalhes de administrador do servidor|
|/servers/administrators/write|Criar ou atualizar o administrador do servidor|
|/servers/advisors/read|Devolve a lista de consultores disponíveis para o servidor|
|/servers/advisors/recommendedActions/read|Devolve a lista de ações recomendadas do advisor especificado para o servidor|
|/servers/advisors/recommendedActions/write|Aplicar a ação recomendada no servidor|
|/servers/advisors/write|As atualizações de autom-executar o estado de um advisor no nível do servidor.|
|/servers/auditingPolicies/read|Obter os detalhes da tabela de servidor predefinida configurada num determinado servidor de política de auditoria|
|/servers/auditingPolicies/write|Alterar a predefinição servidor tabela auditoria para um determinado servidor|
|/servers/auditingSettings/operationResults/read|Obter o resultado do blob servidor operação de definição de política de auditoria|
|/servers/auditingSettings/read|Obter detalhes sobre a servidor blob política de auditoria configurado num determinado servidor|
|/servers/auditingSettings/write|Alterar a auditoria de blob de servidor para um determinado servidor|
|/servers/automaticTuning/read|Devolve as definições de otimização automáticas para o servidor|
|/servers/automaticTuning/write|Atualizações de definições de otimização automáticas para o servidor e devolve as definições atualizadas|
|/servers/backupLongTermRetentionVaults/delete|Elimina um cofre de arquivo de cópia de segurança existente.|
|/servers/backupLongTermRetentionVaults/read|Esta operação é utilizada para obter um cofre de retenção de cópias de segurança de longo prazo. Devolve informações sobre o Cofre registada para este servidor|
|/servers/backupLongTermRetentionVaults/write|Esta operação é utilizada para registar uma retenção de cópias de segurança de longa duração cofre para um servidor|
|/servers/communicationLinks/delete|Elimina uma ligação de comunicação de servidor existente.|
|/servers/communicationLinks/read|Devolva a lista de ligações de comunicação de um servidor especificado.|
|/servers/communicationLinks/write|Criar ou atualizar uma ligação de comunicação do servidor.|
|/servers/connectionPolicies/read|Devolva a lista de políticas de ligação do servidor de um servidor especificado.|
|/servers/connectionPolicies/write|Criar ou atualizar uma política de ligação do servidor.|
|/servers/databases/advisors/read|Devolve a lista de consultores disponíveis para a base de dados|
|/servers/databases/advisors/recommendedActions/read|Devolve a lista de ações recomendadas do advisor especificado para a base de dados|
|/servers/databases/advisors/recommendedActions/write|Aplicar a ação recomendada na base de dados|
|/servers/databases/advisors/write|Atualização automática-executar o estado de um advisor no nível de base de dados.|
|/servers/databases/auditingPolicies/read|Obter os detalhes da política de auditoria de tabela configurado numa base de dados indicado|
|/servers/databases/auditingPolicies/write|Alterar a política de auditoria de tabela para uma determinada base de dados|
|/servers/databases/auditingSettings/read|Obter os detalhes da política de auditoria de blob configurado numa base de dados indicado|
|/servers/databases/auditingSettings/write|Alterar a política de auditoria de blob para uma determinada base de dados|
|/servers/databases/auditRecords/read|Obter os registos de auditoria de blob de base de dados|
|/servers/databases/automaticTuning/read|Devolve as definições de otimização automáticas para uma base de dados|
|/servers/databases/automaticTuning/write|Atualizações de definições de otimização automáticas para uma base de dados e devolve as definições atualizadas|
|/servers/databases/azureAsyncOperation/read|Obtém o estado de uma operação de base de dados.|
|/servers/databases/backupLongTermRetentionPolicies/read|Devolva a lista de políticas de arquivo de cópia de segurança de uma base de dados especificado.|
|/servers/databases/backupLongTermRetentionPolicies/write|Criar ou atualizar uma política de arquivo de cópia de segurança da base de dados.|
|/servers/databases/connectionPolicies/read|Obter os detalhes da política de ligação configurado numa base de dados indicado|
|/servers/databases/connectionPolicies/write|Alterar a política de ligação para uma determinada base de dados|
|/servers/databases/dataMaskingPolicies/read|Devolva a lista de políticas de máscara de dados de base de dados.|
|/servers/databases/dataMaskingPolicies/rules/delete|Eliminar regra de política para uma determinada base de dados de máscara de dados|
|/servers/databases/dataMaskingPolicies/rules/read|Obter detalhes sobre os regra de política configurada numa base de dados indicado de máscara de dados|
|/servers/databases/dataMaskingPolicies/rules/write|Alterar a regra de política para uma determinada base de dados de máscara de dados|
|/servers/databases/dataMaskingPolicies/write|Alterar a política para uma determinada base de dados de máscara de dados|
|/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read|Devolve as informações de passo de consulta distribuída da consulta de armazém de dados para o ID do passo selecionado|
|/servers/databases/dataWarehouseQueries/read|Devolve as informações da consulta de distribuição de armazém de dados para o ID de consulta selecionada|
|/servers/databases/dataWarehouseUserActivities/read|Obtém as atividades de utilizador de uma instância do SQL Data Warehouse que inclui as consultas em execução e suspensos|
|/servers/databases/delete|Elimina uma base de dados existente.|
|/servers/databases/export/action|Exportar a base de dados SQL do Azure|
|/servers/databases/extendedAuditingSettings/read|Obter os detalhes da política de auditoria de blob expandido configurado numa base de dados indicado|
|/servers/databases/extendedAuditingSettings/write|Alterar a política de auditoria de blob expandido para uma determinada base de dados|
|/servers/databases/extensions/read|Obtém uma coleção de extensões para a base de dados.|
|/servers/databases/extensions/write|Altere a extensão para uma determinada base de dados|
|/servers/databases/geoBackupPolicies/read|Obter as políticas de cópia de segurança de georreplicação para uma determinada base de dados|
|/servers/databases/geoBackupPolicies/write|Criar ou atualizar uma política de geobackup de base de dados|
|/servers/databases/importExportOperationResults/read|Obtém em curso operações de importação/exportação|
|/servers/databases/metricDefinitions/read|Tipos de retorno de métricas que estão disponíveis para bases de dados|
|/servers/databases/metrics/read|Métricas de retorno de bases de dados|
|/servers/databases/move/action|Mudar o nome da base de dados SQL do Azure|
|/servers/databases/operationResults/read|Obtém o estado de uma operação de base de dados.|
|/servers/databases/operations/cancel/action|Cancela a SQL Database do Azure operação assíncrona que não foi concluída ainda pendente.|
|/servers/databases/operations/read|Devolver a lista de operações executadas na base de dados|
|/servers/databases/pause/action|Base de dados de armazém de dados SQL do Azure de pausa|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/servers/databases/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para bases de dados|
|/servers/databases/providers/Microsoft.Insights/metricDefinitions/read|Tipos de retorno de métricas que estão disponíveis para bases de dados|
|/servers/databases/queryStore/queryTexts/read|Devolve a coleção de textos de consulta que correspondem aos parâmetros especificados.|
|/servers/databases/queryStore/read|Devolve os valores atuais das definições de arquivo de consultas da base de dados.|
|/servers/databases/queryStore/write|Atualizações de definição de arquivo de consultas da base de dados|
|/servers/databases/read|Devolva a lista de bases de dados ou obtém as propriedades da base de dados especificado.|
|/servers/databases/replicationLinks/delete|Terminar a relação de replicação forçadamente e com uma potencial perda de dados|
|/servers/databases/replicationLinks/failover/action|Ativação pós-falha após a sincronização de todos os é alterado de principal, tornar esta base de dados para a replicação relationship\u0027s primário e efetuar remoto primário para uma secundária|
|/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action|Ativação pós-falha imediatamente com potencial perda de dados, tornar esta base de dados para a replicação relationship\u0027s primário e efetuar remoto primário para uma secundária|
|/servers/databases/replicationLinks/read|Retorno detalhes sobre ligações de replicação estabelecidas para uma determinada base de dados|
|/servers/databases/replicationLinks/unlink/action|Terminar a relação de replicação forçadamente ou após a sincronização com o parceiro|
|/servers/databases/replicationLinks/updateReplicationMode/action|Atualizar o modo de replicação para a ligação para o modo síncrono ou assíncrono|
|/servers/databases/restorePoints/action|Cria um novo ponto de restauro|
|/servers/databases/restorePoints/read|Devolve restaurar pontos para a base de dados.|
|/servers/databases/resume/action|Base de dados de armazém de dados SQL do Azure de retomar|
|/servers/databases/schemas/read|Obter a lista de esquemas de uma base de dados|
|/servers/databases/schemas/tables/columns/read|Obter a lista de colunas de uma tabela|
|/servers/databases/schemas/tables/columns/sensitivityLabels/delete|Eliminar a etiqueta de sensibilidade de uma dada coluna|
|/servers/databases/schemas/tables/columns/sensitivityLabels/read|Obter a etiqueta de sensibilidade de uma dada coluna|
|/servers/databases/schemas/tables/columns/sensitivityLabels/write|Criar ou atualizar a etiqueta de sensibilidade de uma dada coluna|
|/servers/databases/schemas/tables/read|Obter a lista de tabelas de uma base de dados|
|/servers/databases/schemas/tables/recommendedIndexes/read|Obter a lista de recomendações do índice numa base de dados|
|/servers/databases/schemas/tables/recommendedIndexes/write|Aplicar a recomendação do índice|
|/servers/databases/securityAlertPolicies/read|Obter os detalhes da política de deteção de ameaças configurado numa base de dados indicado|
|/servers/databases/securityAlertPolicies/write|Alterar a política de deteção de ameaças para uma determinada base de dados|
|/servers/databases/securityMetrics/read|Obtém uma coleção de métricas de segurança da base de dados|
|/servers/databases/sensitivityLabels/read|Etiquetas de sensibilidade da lista de uma determinada base de dados|
|/servers/databases/serviceTierAdvisors/read|Devolver a sugestão sobre dimensionar a base de dados ou reduzir verticalmente baseadas em estatísticas de execução de consulta para melhorar o desempenho ou reduzir o custo|
|/servers/databases/syncGroups/cancelSync/action|Cancelar Sincronização de grupo de sincronização|
|/servers/databases/syncGroups/delete|Elimina um grupo de sincronização existente.|
|/servers/databases/syncGroups/hubSchemas/read|Devolver a lista de sincronização de esquemas de base de dados do hub|
|/servers/databases/syncGroups/logs/read|Devolver a lista de registos do grupo de sincronização|
|/servers/databases/syncGroups/read|Devolva a lista de sincronização de grupos ou obtém as propriedades para o grupo de sincronização especificada.|
|/servers/databases/syncGroups/refreshHubSchema/action|Atualizar o esquema de base de dados do hub de sincronização|
|/servers/databases/syncGroups/refreshHubSchemaOperationResults/read|Obter o resultado da operação de atualização de esquema de hub de sincronização|
|/servers/databases/syncGroups/syncMembers/delete|Elimina um membro de sincronização existente.|
|/servers/databases/syncGroups/syncMembers/read|Devolva a lista de membros de sincronização ou obtém as propriedades para o membro de sincronização especificada.|
|/servers/databases/syncGroups/syncMembers/refreshSchema/action|Atualizar o esquema de membro de sincronização|
|/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read|Obter o resultado da operação de atualização de esquema de membro de sincronização|
|/servers/databases/syncGroups/syncMembers/schemas/read|Devolver a lista de sincronização de esquemas de base de dados do membro|
|/servers/databases/syncGroups/syncMembers/write|Cria um membro de sincronização com os parâmetros especificados ou atualiza as propriedades para o membro de sincronização especificada.|
|/servers/databases/syncGroups/triggerSync/action|Sincronização de grupo de sincronização do acionador|
|/servers/databases/syncGroups/write|Cria um grupo de sincronização com os parâmetros especificados ou atualiza as propriedades para o grupo de sincronização especificada.|
|/servers/databases/topQueries/queryText/action|Devolve o texto de Transact-SQL para o ID de consulta selecionada|
|/servers/databases/topQueries/read|Devolve agregar estatísticas de tempo de execução para a consulta selecionada no período de tempo selecionado|
|/servers/databases/topQueries/statistics/read|Devolve agregar estatísticas de tempo de execução para a consulta selecionada no período de tempo selecionado|
|/servers/databases/transparentDataEncryption/operationResults/read|Obtém em curso operações de encriptação transparente de dados|
|/servers/databases/transparentDataEncryption/read|Obter o estado e os detalhes da funcionalidade de segurança de encriptação transparente de dados para uma determinada base de dados|
|/servers/databases/transparentDataEncryption/write|Alterar o estado de encriptação transparente de dados|
|/servers/databases/upgradeDataWarehouse/action|Atualizar a base de dados do armazém de dados SQL do Azure|
|/servers/databases/usages/read|Obtém as informações de utilizações de SQL Database do Azure|
|/servers/databases/vulnerabilityAssessments/delete|Remova a avaliação da vulnerabilidade para uma determinada base de dados|
|/servers/databases/vulnerabilityAssessments/read|Obter detalhes sobre a avaliação da vulnerabilidade configurado numa base de dados indicado|
|/servers/databases/vulnerabilityAssessments/rules/baselines/delete|Remover a linha de base da regra de avaliação vulnerabilidade para uma determinada base de dados|
|/servers/databases/vulnerabilityAssessments/rules/baselines/read|Obter a linha de base de regra de avaliação de vulnerabilidade para uma determinada base de dados|
|/servers/databases/vulnerabilityAssessments/rules/baselines/write|Altere a linha de base da regra de avaliação vulnerabilidade para uma determinada base de dados|
|/servers/databases/vulnerabilityAssessments/scans/action|Execute uma análise de avaliação da vulnerabilidade da base de dados.|
|/servers/databases/vulnerabilityAssessments/scans/export/action|Converta um resultado de análise existente para um formato legível humano. Se já existe nada acontece|
|/servers/databases/vulnerabilityAssessments/scans/read|Devolver a lista de vulnerabilidade de base de dados de registos de análise de avaliação ou obter o registo de análise para o ID especificado de análise.|
|/servers/databases/vulnerabilityAssessments/write|Alterar a avaliação da vulnerabilidade para uma determinada base de dados|
|/servers/databases/vulnerabilityAssessmentScans/action|Execute uma análise de avaliação da vulnerabilidade da base de dados.|
|/servers/databases/vulnerabilityAssessmentScans/operationResults/read|Obter o resultado da análise de avaliação da vulnerabilidade da base de dados a operação de execução|
|/servers/databases/vulnerabilityAssessmentSettings/read|Obter detalhes sobre a avaliação da vulnerabilidade configurado numa base de dados indicado|
|/servers/databases/vulnerabilityAssessmentSettings/write|Alterar a avaliação da vulnerabilidade para uma determinada base de dados|
|/servers/databases/write|Cria uma base de dados com os parâmetros especificados ou atualiza as propriedades ou etiquetas da base de dados especificado.|
|/servers/delete|Elimina um servidor existente.|
|/servers/disasterRecoveryConfiguration/delete|Elimina um configurações de recuperação após desastre existente para um determinado servidor|
|/servers/disasterRecoveryConfiguration/failover/action|Ativação pós-falha um DisasterRecoveryConfiguration|
|/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action|Forçar a ativação pós-falha um DisasterRecoveryConfiguration|
|/servers/disasterRecoveryConfiguration/read|Obtém uma coleção de desastre configurações de recuperação que incluem este servidor|
|/servers/disasterRecoveryConfiguration/write|Alterar a configuração de recuperação de desastre servidor|
|/servers/elasticPoolEstimates/read|Devolve a lista de estimativas de conjunto elástico já criada para este servidor|
|/servers/elasticPoolEstimates/write|Cria nova estimativa de conjunto elástico para obter a lista de bases de dados fornecido|
|/servers/elasticPools/advisors/read|Devolve a lista de consultores disponíveis para o conjunto elástico|
|/servers/elasticPools/advisors/recommendedActions/read|Devolve a lista de ações recomendadas do advisor especificado para o conjunto elástico|
|/servers/elasticPools/advisors/recommendedActions/write|Aplicar a ação recomendada do conjunto elástico|
|/servers/elasticPools/advisors/write|Atualização automática-executar o estado de um advisor no nível do conjunto elástico.|
|/servers/elasticPools/databases/read|Obtém uma lista de bases de dados de um conjunto elástico|
|/servers/elasticPools/delete|Eliminar o conjunto elástico existente|
|/servers/elasticPools/elasticPoolActivity/read|Obter as atividades e detalhes sobre um conjunto de bases de dados elásticas indicado|
|/servers/elasticPools/elasticPoolDatabaseActivity/read|Obter as atividades e os detalhes numa determinada base de dados que faz parte do agrupamento de base de dados elástica|
|/servers/elasticPools/metricDefinitions/read|Tipos de retorno de métricas que estão disponíveis para conjuntos de bases de dados elásticas|
|/servers/elasticPools/metrics/read|Métricas de retorno de conjuntos de bases de dados elásticas|
|/servers/elasticPools/operations/cancel/action|Cancela o agrupamento elástico de SQL do Azure operação assíncrona que não foi concluída ainda pendente.|
|/servers/elasticPools/operations/read|Devolver a lista de operações executadas no conjunto elástico|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read|Tipos de retorno de métricas que estão disponíveis para conjuntos de bases de dados elásticas|
|/servers/elasticPools/read|Obter os detalhes do conjunto elástico num determinado servidor|
|/servers/elasticPools/skus/read|Obtém uma coleção de skus disponíveis para este agrupamento elástico|
|/servers/elasticPools/write|Crie um novo ou alterar as propriedades do conjunto elástico existente|
|/servers/encryptionProtector/read|Devolve uma lista de protetores de encriptação do servidor ou obtém as propriedades para o servidor especificado protetor de encriptação.|
|/servers/encryptionProtector/write|Atualize as propriedades para o Protetor de encriptação de servidor especificado.|
|/servers/extendedAuditingSettings/read|Obter os detalhes do blob expandida de servidor configurada num determinado servidor de política de auditoria|
|/servers/extendedAuditingSettings/write|Alterar a auditoria de blob de expandida de servidor para um determinado servidor|
|/servers/failoverGroups/delete|Elimina um grupo de ativação pós-falha existente.|
|/servers/failoverGroups/failover/action|Executa a ativação pós-falha planeada num grupo de ativação pós-falha existente.|
|/servers/failoverGroups/forceFailoverAllowDataLoss/action|Executa a ativação pós-falha forçada num grupo de ativação pós-falha existente.|
|/servers/failoverGroups/read|Devolve a lista de ativação pós-falha de grupos ou obtém as propriedades para o grupo de ativação pós-falha especificado.|
|/servers/failoverGroups/write|Cria um grupo de ativação pós-falha com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o grupo de ativação pós-falha especificado.|
|/servers/firewallRules/delete|Elimina uma regra de firewall do servidor existente.|
|/servers/firewallRules/read|Devolva a lista de firewall do servidor regras ou obtém as propriedades para o servidor especificado regra de firewall.|
|/servers/firewallRules/write|Cria uma regra de firewall do servidor com os parâmetros especificados, atualizar as propriedades para a regra especificada ou substituir todas as regras existentes com novas regras de firewall do servidor.|
|/servers/import/action|Criar uma nova base de dados no servidor e implementar o esquema e dados a partir de um pacote de DacPac|
|/servers/importExportOperationResults/read|Obtém em curso operações de importação/exportação|
|/servers/keys/delete|Elimina uma chave de servidor existente.|
|/servers/keys/read|Devolva a lista de servidor de chaves ou obtém as propriedades para a chave de servidor especificado.|
|/servers/keys/write|Cria uma chave com os parâmetros especificados ou atualiza as propriedades ou etiquetas da chave de servidor especificado.|
|/servers/operationResults/read|Obtém em curso operações de servidor|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Tipos de retorno de métricas que estão disponíveis para servidores|
|/servers/read|Devolva a lista de servidores ou obtém as propriedades para o servidor especificado.|
|/servers/recommendedElasticPools/databases/read|Obter métricas para os conjuntos de bases de dados elásticas recomendado para um determinado servidor|
|/servers/recommendedElasticPools/read|Obter a recomendação para conjuntos de bases de dados elásticas reduzir o custo ou melhorar o desempenho com base na utilização de recursos de historica|
|/servers/recoverableDatabases/read|Esta operação é utilizada para recuperação após desastre de base de dados dinâmicos para restaurar a base de dados ao conhecido do último ponto de cópia de segurança boa. Devolve informações sobre a última cópia de segurança boa mas doesn\u0027t, na verdade, restaure a base de dados.|
|/servers/restorableDroppedDatabases/read|Obter uma lista de bases de dados que foram ignorados num determinado servidor que estão ainda na política de retenção.|
|/servers/securityAlertPolicies/operationResults/read|Obter os resultados da operação de escrita de política de deteção de ameaças de servidor|
|/servers/securityAlertPolicies/read|Obter os detalhes da política de deteção de ameaças de servidor configurada num determinado servidor|
|/servers/securityAlertPolicies/write|Alterar a política de deteção de ameaças de servidor para um determinado servidor|
|/servers/serviceObjectives/read|Obter a lista de serviço objetivos do nível (também conhecido como escalões de desempenho) disponíveis num determinado servidor|
|/servers/syncAgents/delete|Elimina um agente de sincronização existentes.|
|/servers/syncAgents/generateKey/action|Gerar a chave de registo do agente de sincronização|
|/servers/syncAgents/linkedDatabases/read|Devolver a lista de sincronizar bases de dados do agente ligado|
|/servers/syncAgents/read|Devolva a lista de agentes de sincronização ou obtém as propriedades para o agente de sincronização especificada.|
|/servers/syncAgents/write|Cria um agente de sincronização com os parâmetros especificados ou atualiza as propriedades para o agente de sincronização especificada.|
|/servers/usages/read|Devolver a quota DTU do servidor e consuption DTU atual por todas as bases de dados no servidor|
|/servers/virtualNetworkRules/delete|Elimina uma regra de rede Virtual existente|
|/servers/virtualNetworkRules/read|Devolva a lista de rede virtual regras ou obtém as propriedades para a regra de rede virtual especificado.|
|/servers/virtualNetworkRules/write|Cria uma regra de rede virtual com os parâmetros especificados ou atualiza as propriedades ou etiquetas para a regra de rede virtual especificado.|
|/servers/write|Cria um servidor com os parâmetros especificados ou atualiza as propriedades ou etiquetas para o servidor especificado.|
|/unregister/action|Anula o registo da subscrição para o fornecedor de recursos de base de dados do Microsoft SQL Server e permite a criação de bases de dados do Microsoft SQL Server.|
|/virtualClusters/read|Devolva a lista de clusters virtuais ou obtém as propriedades para o cluster virtual especificado.|
|/virtualClusters/write|As atualizações de etiquetas de virtual cluster.|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Operação | Descrição |
|---|---|
|/checknameavailability/read|Verifica se o nome da conta é válido e se não está já a ser utilizado.|
|/locations/deleteVirtualNetworkOrSubnets/action|Notifica o Microsoft.Storage que a rede ou sub-rede virtual está a ser eliminada|
|/operations/read|Consulta o estado de uma operação assíncrona.|
|/register/action|Regista a subscrição do fornecedor de recursos de armazenamento e ativa a criação de contas de armazenamento.|
|/skus/read|Lista os Skus suportados pelo Microsoft.Storage.|
|/storageAccounts/blobServices/containers/clearLegalHold/action|Limpar o arquivo legal do contentor de blobs|
|/storageAccounts/blobServices/containers/delete|Devolve o resultado da eliminação de um contentor|
|/storageAccounts/blobServices/containers/immutabilityPolicies/delete|Eliminar a política de imutabilidade do contentor de blobs|
|/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action|Expandir a política de imutabilidade do contentor de blobs|
|/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action|Bloquear a política de imutabilidade do contentor de blobs|
|/storageAccounts/blobServices/containers/immutabilityPolicies/read|Obter a política de imutabilidade do contentor de blobs|
|/storageAccounts/blobServices/containers/immutabilityPolicies/write|Colocar a política de imutabilidade do contentor de blobs|
|/storageAccounts/blobServices/containers/read|Devolve um contentor ou uma lista de contentores|
|/storageAccounts/blobServices/containers/setLegalHold/action|Definir o arquivo legal do contentor de blobs|
|/storageAccounts/blobServices/containers/write|Devolve o resultado do contentor de blobs colocados ou de concessão|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição do diagnóstico para o recurso.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o recurso.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de definições das Métricas de Armazenamento Microsoft.|
|/storageAccounts/blobServices/read|Devolve propriedades ou estatísticas do serviço Blob|
|/storageAccounts/blobServices/write|Devolve o resultado das propriedades do serviço de blobs colocado|
|/storageAccounts/delete|Elimina uma conta de armazenamento existente.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição do diagnóstico para o recurso.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o recurso.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de definições das Métricas de Armazenamento Microsoft.|
|/storageAccounts/listAccountSas/action|Devolve o token de SAS de Conta para a conta de armazenamento especificada.|
|/storageAccounts/listkeys/action|Devolve as chaves de acesso da conta de armazenamento especificada.|
|/storageAccounts/listServiceSas/action|Devolve o token de SAS de Serviço para a conta de armazenamento especificada.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição do diagnóstico para o recurso.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o recurso.|
|/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de definições das Métricas de Armazenamento Microsoft.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o recurso.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de definições das Métricas de Armazenamento Microsoft.|
|/storageAccounts/queueServices/queues/delete|Devolve o resultado da eliminação de uma fila|
|/storageAccounts/queueServices/queues/read|Devolve uma fila ou uma lista de filas.|
|/storageAccounts/queueServices/queues/write|Devolve o resultado da escrita de uma fila|
|/storageAccounts/queueServices/read|Propriedades ou estatistas do serviço Fila de devolução.|
|/storageAccounts/queueServices/write|Devolve o resultado de definir as propriedades do serviço Fila|
|/storageAccounts/read|Devolve a lista de contas de armazenamento ou obtém as propriedades da conta de armazenamento especificada.|
|/storageAccounts/regeneratekey/action|Regenera as chaves de acesso da conta de armazenamento especificada.|
|/storageAccounts/services/diagnosticSettings/write|Criar/atualizar definições de diagnóstico da conta de armazenamento.|
|/storageAccounts/storageAccounts/queueServices/providers/ Microsoft.Insights/diagnosticSettings/read|Obtém a definição do diagnóstico para o recurso.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição do diagnóstico para o recurso.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição de diagnóstico para o recurso.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read|Obter a lista de definições das Métricas de Armazenamento Microsoft.|
|/storageAccounts/write|Cria uma conta de armazenamento com os parâmetros especificados ou atualiza as propriedades ou etiquetas, ou adiciona um domínio personalizado à conta de armazenamento especificada.|
|/usages/read|Devolve o limite e a contagem de utilização atual de recursos na subscrição especificada|

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

| Operação | Descrição |
|---|---|
|/storageSyncServices/delete|Eliminar quaisquer serviços de sincronização de armazenamento|
|/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para os serviços de sincronização de armazenamento|
|/storageSyncServices/read|Ler quaisquer serviços de sincronização de armazenamento|
|/storageSyncServices/registeredServers/delete|Eliminar qualquer servidor registado|
|/storageSyncServices/registeredServers/read|Ler a qualquer servidor registado|
|/storageSyncServices/registeredServers/write|Criar ou atualizar qualquer servidor registado|
|/storageSyncServices/syncGroups/cloudEndpoints/delete|Eliminar quaisquer pontos finais de nuvem|
|/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read|Api de localização para chamadas de cópia de segurança assíncrona|
|/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action|Chamar esta ação depois de cópia de segurança|
|/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action|Chamar esta ação depois de restauro|
|/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action|Chamar esta ação antes de cópia de segurança|
|/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action|Chamar esta ação antes de restauro|
|/storageSyncServices/syncGroups/cloudEndpoints/read|Ler quaisquer pontos finais de nuvem|
|/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action|Restaurar heartbeat|
|/storageSyncServices/syncGroups/cloudEndpoints/write|Criar ou atualizar quaisquer pontos finais de nuvem|
|/storageSyncServices/syncGroups/delete|Eliminar todos os grupos de sincronização|
|/storageSyncServices/syncGroups/read|Ler todos os grupos de sincronização|
|/storageSyncServices/syncGroups/serverEndpoints/delete|Eliminar quaisquer pontos finais de servidor|
|/storageSyncServices/syncGroups/serverEndpoints/read|Ler todos os pontos finais do servidor|
|/storageSyncServices/syncGroups/serverEndpoints/recallAction/action|Chamar esta ação para recuperar um servidor de ficheiros|
|/storageSyncServices/syncGroups/serverEndpoints/write|Criar ou atualizar quaisquer pontos finais de servidor|
|/storageSyncServices/syncGroups/write|Criar ou atualizar quaisquer grupos de sincronização|
|/storageSyncServices/write|Criar ou atualizar os serviços de sincronização de armazenamento|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Operação | Descrição |
|---|---|
|/managers/accessControlRecords/delete|Elimina os registos de controlo de acesso|
|/managers/accessControlRecords/read|Apresenta uma lista ou obtém os registos de controlo de acesso|
|/managers/accessControlRecords/write|Criar ou atualizar os registos de controlo de acesso|
|/managers/alerts/read|Apresenta uma lista ou obtém os alertas|
|/managers/bandwidthSettings/delete|Elimina as definições de largura de banda de um existente (8000 série apenas)|
|/managers/bandwidthSettings/read|Lista as definições de largura de banda (apenas 8000 série)|
|/managers/bandwidthSettings/write|Cria um novo ou atualiza as definições de largura de banda (8000 série apenas)|
|/Managers/certificates/write|A operação de certificado de recursos de atualização atualiza o certificado da credencial de recursos/cofre.|
|/managers/clearAlerts/action|Limpe todos os alertas associados com o Gestor de dispositivos.|
|/managers/cloudApplianceConfigurations/read|A aplicação de nuvem de lista as configurações suportadas|
|/managers/configureDevice/action|Configura um dispositivo|
|/managers/delete|Elimina os gestores de dispositivos|
|/Managers/delete|A operação eliminar cofre elimina os recursos do Azure especificado do tipo 'cofre'|
|/managers/devices/alertSettings/read|Apresenta uma lista ou obtém as definições de alerta|
|/managers/devices/alertSettings/write|Criar ou atualizar as definições de alerta|
|/managers/devices/backupPolicies/backup/action|Efetuar uma cópia de segurança manual para criar um pedido cópia de segurança de todos os volumes protegidos pela política.|
|/managers/devices/backupPolicies/delete|Elimina um políticas de cópia de segurança existente (8000 série apenas)|
|/managers/devices/backupPolicies/read|Políticas de lista a cópia de segurança (8000 série apenas)|
|/managers/devices/backupPolicies/schedules/delete|Elimina um agendas existentes|
|/managers/devices/backupPolicies/schedules/read|Lista as agendas|
|/managers/devices/backupPolicies/schedules/write|Cria um novo ou agendas de atualizações|
|/managers/devices/backupPolicies/write|Cria um novo ou atualizações de políticas de cópia de segurança (8000 série apenas)|
|/managers/devices/backups/delete|Elimina o conjunto de cópia de segurança|
|/managers/devices/backups/elements/clone/action|Clone de uma partilha ou volume com um elemento de cópia de segurança.|
|/managers/devices/backups/read|Apresenta uma lista ou obtém o conjunto de cópia de segurança|
|/managers/devices/backups/restore/action|Restaure todos os volumes a partir de um conjunto de cópia de segurança.|
|/managers/devices/backupScheduleGroups/delete|Elimina os grupos de agenda de cópia de segurança|
|/managers/devices/backupScheduleGroups/read|Apresenta uma lista ou obtém os grupos de agenda de cópia de segurança|
|/managers/devices/backupScheduleGroups/write|Criar ou atualizar os grupos de agenda de cópia de segurança|
|/managers/devices/chapSettings/delete|Elimina as definições do Chap|
|/managers/devices/chapSettings/read|Apresenta uma lista ou obtém as definições Chap|
|/managers/devices/chapSettings/write|Criar ou atualizar as definições Chap|
|/managers/devices/deactivate/action|Desativa um dispositivo.|
|/managers/devices/delete|Elimina os dispositivos|
|/managers/devices/download/action|Atualizações de transferências para um dispositivo.|
|/managers/devices/failover/action|Ativação pós-falha do dispositivo.|
|/managers/devices/fileservers/backup/action|Efetuar cópia de segurança de um servidor de ficheiros.|
|/managers/devices/fileservers/delete|Elimina os servidores de ficheiros|
|/managers/devices/fileservers/metrics/read|Apresenta uma lista ou obtém as métricas|
|/managers/devices/fileservers/metricsDefinitions/read|Apresenta uma lista ou obtém as definições de métricas|
|/managers/devices/fileservers/read|Apresenta uma lista ou obtém os servidores de ficheiros|
|/managers/devices/fileservers/shares/delete|Elimina as partilhas|
|/managers/devices/fileservers/shares/metrics/read|Apresenta uma lista ou obtém as métricas|
|/managers/devices/fileservers/shares/metricsDefinitions/read|Apresenta uma lista ou obtém as definições de métricas|
|/managers/devices/fileservers/shares/read|Apresenta uma lista ou obtém as partilhas|
|/managers/devices/fileservers/shares/write|Criar ou atualizar as partilhas|
|/managers/devices/fileservers/write|Criar ou atualizar os servidores de ficheiros|
|/managers/devices/hardwareComponentGroups/changeControllerPowerState/action|Alterar o estado de energia do controlador de grupos de componentes de hardware|
|/managers/devices/hardwareComponentGroups/read|Lista os grupos de componentes de Hardware|
|/managers/devices/install/action|Instale as atualizações num dispositivo.|
|/managers/devices/installUpdates/action|Instala as atualizações nos dispositivos|
|/managers/devices/iscsiservers/backup/action|Efetuar cópia de segurança de um servidor de iSCSI.|
|/managers/devices/iscsiservers/delete|Elimina o iSCSI de servidores|
|/managers/devices/iscsiservers/disks/delete|Elimina os discos|
|/managers/devices/iscsiservers/disks/metrics/read|Apresenta uma lista ou obtém as métricas|
|/managers/devices/iscsiservers/disks/metricsDefinitions/read|Apresenta uma lista ou obtém as definições de métricas|
|/managers/devices/iscsiservers/disks/read|Apresenta uma lista ou obtém os discos|
|/managers/devices/iscsiservers/disks/write|Criar ou atualizar os discos|
|/managers/devices/iscsiservers/metrics/read|Apresenta uma lista ou obtém as métricas|
|/managers/devices/iscsiservers/metricsDefinitions/read|Apresenta uma lista ou obtém as definições de métricas|
|/managers/devices/iscsiservers/read|Apresenta uma lista ou obtém o iSCSI de servidores|
|/managers/devices/iscsiservers/write|Criar ou atualizar o iSCSI de servidores|
|/managers/devices/jobs/cancel/action|Cancelar uma tarefa em execução|
|/managers/devices/jobs/read|Apresenta uma lista ou obtém as tarefas|
|/managers/devices/listFailoverSets/action|Liste os conjuntos de ativação pós-falha de um dispositivo existente.|
|/managers/devices/listFailoverTargets/action|Destinos de ativação pós-falha de lista dos dispositivos|
|/managers/devices/metrics/read|Apresenta uma lista ou obtém as métricas|
|/managers/devices/metricsDefinitions/read|Apresenta uma lista ou obtém as definições de métricas|
|/managers/devices/migrationSourceConfigurations/confirmMigration/action|Confirma que uma migração com êxito e a consolidação da mesma.|
|/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action|Obter o estado de confirmar de migração.|
|/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action|Obter o estado da tarefa de migração estimativa.|
|/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action|Obter o estado para a migração.|
|/managers/devices/migrationSourceConfigurations/import/action|Importar configurações de origem para migração|
|/managers/devices/migrationSourceConfigurations/startMigration/action|Começar a utilizar configurações de origem de migração|
|/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action|Inicie uma tarefa para estimar a duração do processo de migração.|
|/managers/devices/networkSettings/read|Apresenta uma lista ou obtém as definições de rede|
|/managers/devices/networkSettings/write|Cria um novo ou atualiza as definições de rede|
|/managers/devices/publicEncryptionKey/action|Chave de encriptação pública da lista do Gestor de dispositivos|
|/managers/devices/publishSupportPackage/action|Publica o pacote de suporte de um dispositivo para a resolução de problemas de Support da Microsoft.|
|/managers/devices/read|Apresenta uma lista ou obtém os dispositivos|
|/managers/devices/scanForUpdates/action|Análise de atualizações de um dispositivo.|
|/managers/devices/securitySettings/read|Lista as definições de segurança|
|/managers/devices/securitySettings/syncRemoteManagementCertificate/action|Sincronize o certificado de gestão remota para um dispositivo.|
|/managers/devices/securitySettings/update/action|Atualize as definições de segurança.|
|/managers/devices/securitySettings/write|Cria um novo ou atualiza as definições de segurança|
|/managers/devices/sendTestAlertEmail/action|Envie e-mail de teste de alerta para destinatários de correio eletrónico configurado.|
|/managers/devices/timeSettings/read|Apresenta uma lista ou obtém as definições de tempo|
|/managers/devices/timeSettings/write|Cria um novo ou atualiza as definições de hora|
|/managers/devices/updateSummary/read|Apresenta uma lista ou obtém o resumo de atualização|
|/managers/devices/volumeContainers/delete|Elimina um contentores de Volume existente (8000 série apenas)|
|/managers/devices/volumeContainers/listEncryptionKeys/action|Lista de chaves de encriptação de contentores de Volume|
|/managers/devices/volumeContainers/metrics/read|Lista as métricas|
|/managers/devices/volumeContainers/metricsDefinitions/read|Lista as definições de métricas|
|/managers/devices/volumeContainers/read|Lista os contentores de Volume (8000 série apenas)|
|/managers/devices/volumeContainers/rolloverEncryptionKey/action|Chaves de encriptação de rollover de contentores de Volume|
|/managers/devices/volumeContainers/volumes/delete|Elimina um Volumes existentes|
|/managers/devices/volumeContainers/volumes/metrics/read|Lista as métricas|
|/managers/devices/volumeContainers/volumes/metricsDefinitions/read|Lista as definições de métricas|
|/managers/devices/volumeContainers/volumes/read|Lista de Volumes|
|/managers/devices/volumeContainers/volumes/write|Cria um novo ou Volumes de atualizações|
|/managers/devices/volumeContainers/write|Cria um novo ou atualiza os contentores de Volume (8000 série apenas)|
|/managers/devices/write|Criar ou atualizar os dispositivos|
|/managers/encryptionSettings/read|Apresenta uma lista ou obtém as definições de encriptação|
|/Managers/extendedInformation/delete|A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure|
|/Managers/extendedInformation/read|A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure|
|/Managers/extendedInformation/write|A operação Obter Informações Detalhadas obtém as Informações Detalhadas de um objeto que representa o tipo de recurso ?vault? do Azure|
|/managers/getActivationKey/action|Obter a chave de ativação para o Gestor de dispositivos.|
|/managers/getEncryptionKey/action|Obter a chave de encriptação para o Gestor de dispositivos.|
|/managers/listActivationKey/action|Obtém a chave de ativação do Gestor de dispositivos StorSimple.|
|/managers/listPrivateEncryptionKey/action|Obtém a chave privada de encriptação para um Gestor de dispositivos do StorSimple.|
|/managers/listPublicEncryptionKey/action|Listar chaves de encriptação pública de um Gestor de dispositivos do StorSimple.|
|/managers/metrics/read|Apresenta uma lista ou obtém as métricas|
|/managers/metricsDefinitions/read|Apresenta uma lista ou obtém as definições de métricas|
|/managers/provisionCloudAppliance/action|Crie uma nova aplicação de nuvem.|
|/managers/read|Apresenta uma lista ou obtém os gestores de dispositivos|
|/Managers/read|A operação de introdução cofre obtém um objeto que representa o recurso do Azure do tipo 'cofre'|
|/managers/regenarateRegistationCertificate/action|Regenere o certificado de registo para os gestores de dispositivos.|
|/managers/regenerateActivationKey/action|Regenere a chave de ativação para o Gestor de dispositivos.|
|/managers/storageAccountCredentials/delete|Elimina as credenciais da conta de armazenamento|
|/managers/storageAccountCredentials/listAccessKey/action|Lista de chaves de acesso das credenciais de conta de armazenamento|
|/managers/storageAccountCredentials/read|Apresenta uma lista ou obtém as credenciais da conta de armazenamento|
|/managers/storageAccountCredentials/write|Criar ou atualizar as credenciais da conta de armazenamento|
|/managers/storageDomains/delete|Elimina os domínios de armazenamento|
|/managers/storageDomains/read|Apresenta uma lista ou obtém os domínios de armazenamento|
|/managers/storageDomains/write|Criar ou atualizar os domínios de armazenamento|
|/managers/write|Criar ou atualizar os gestores de dispositivos|
|/Managers/write|A operação Criar Cofre cria um recurso do tipo "cofre" do Azure|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Operação | Descrição |
|---|---|
|/locations/quotas/Read|Quota de subscrição de análise de fluxo de leitura|
|/operations/Read|Operações de leitura Stream Analytics|
|/ Register/ação|Registar a subscrição com o fornecedor de recursos do Stream Analytics|
|/streamingjobs/Delete|Eliminar a tarefa do Stream Analytics|
|/streamingjobs/functions/Delete|Eliminar a função de tarefa do Stream Analytics|
|/streamingjobs/functions/operationresults/Read|Resultados da operação de leitura para a função de tarefa do Stream Analytics|
|/streamingjobs/functions/Read|Função de tarefa de análise de fluxo de leitura|
|/streamingjobs/functions/RetrieveDefaultDefinition/action|Obter definição predefinida de uma função de tarefa do Stream Analytics|
|/streamingjobs/functions/Test/action|Função de tarefa de análise de fluxo de teste|
|/streamingjobs/functions/Write|Escrever a função de tarefa do Stream Analytics|
|/streamingjobs/inputs/Delete|Eliminar a entrada de fluxo de trabalho de análise|
|/streamingjobs/inputs/operationresults/Read|Resultados da operação de leitura para a entrada de fluxo de trabalho de análise|
|/streamingjobs/inputs/Read|Entrada de tarefa de análise de fluxo de leitura|
|/streamingjobs/inputs/Sample/action|Entrada de tarefa de análise de sequência de exemplo|
|/streamingjobs/inputs/Test/action|Entrada de tarefa de análise de fluxo de teste|
|/streamingjobs/inputs/Write|Escrever a entrada de fluxo de trabalho de análise|
|/streamingjobs/metricdefinitions/Read|Definições de métrica de leitura|
|/streamingjobs/operationresults/Read|Resultados da operação de leitura para a tarefa do Stream Analytics|
|/streamingjobs/outputs/Delete|Eliminar o resultado da tarefa do Stream Analytics|
|/streamingjobs/outputs/operationresults/Read|Resultados da operação de leitura do resultado de tarefa do Stream Analytics|
|/streamingjobs/outputs/Read|Resultado da tarefa leitura Stream Analytics|
|/streamingjobs/outputs/Test/action|Saída de tarefa de análise de fluxo de teste|
|/streamingjobs/outputs/Write|Escrever a saída da tarefa do Stream Analytics|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read|Definição de diagnóstico de leitura.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write|Escreva a definição de diagnóstico.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/read|Obtém os registos disponíveis para streamingjobs|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para streamingjobs|
|/streamingjobs/Read|Leitura tarefa do Stream Analytics|
|/streamingjobs/Start/action|Iniciar a tarefa do Stream Analytics|
|/streamingjobs/Stop/action|Parar a tarefa do Stream Analytics|
|/streamingjobs/transformations/Delete|Eliminar a transformação de tarefa do Stream Analytics|
|/streamingjobs/transformations/Read|Transformação de tarefa de análise de fluxo de leitura|
|/streamingjobs/transformations/Write|Escrever transformação de tarefa do Stream Analytics|
|/streamingjobs/Write|Escrever a tarefa do Stream Analytics|

## <a name="microsoftsubscription"></a>Microsoft.Subscription

| Operação | Descrição |
|---|---|
|/SubscriptionDefinitions/read|Obter uma definição de subscrição do Azure dentro de um grupo de gestão.|
|/SubscriptionDefinitions/write|Criar uma definição de subscrição do Azure|

## <a name="microsoftsupport"></a>Microsoft.Support

| Operação | Descrição |
|---|---|
|/register/action|Regista para o Fornecedor de Recursos de Suporte|
|/supportTickets/read|Obtém os detalhes do Pedido de Suporte (incluindo o estado, gravidade, detalhes de contacto e comunicações) ou obtém a lista dos Pedidos de Suporte nas subscrições.|
|/supportTickets/write|Cria ou atualiza um pedido de suporte. Pode criar um pedido de suporte para técnica, Quotas ou gestão de subscrição de faturação problemas relacionados com. Pode atualizar a gravidade, detalhes de contacto e comunicações para pedidos de suporte existentes.|

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

| Operação | Descrição |
|---|---|
|/environments/accesspolicies/delete|Elimina a política de acesso.|
|/environments/accesspolicies/read|Obter as propriedades de uma política de acesso.|
|/environments/accesspolicies/write|Cria uma nova política de acesso para um ambiente ou atualiza uma política de acesso existentes.|
|/environments/delete|Elimina o ambiente.|
|/environments/eventsources/delete|Elimina a origem do evento.|
|/Environments/eventsources/eventsources/Providers/Microsoft.Insights/ diagnosticSettings/escrita|Cria ou atualiza a definição do diagnóstico para o recurso|
|/environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/environments/eventsources/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para eventsources|
|/environments/eventsources/read|Obter as propriedades de uma origem de evento.|
|/environments/eventsources/write|Cria uma nova origem de evento para um ambiente ou atualiza uma origem de evento existente.|
|/environments/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/environments/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/environments/providers/Microsoft.Insights/metricDefinitions/read|Obtem as métricas disponíveis para ambientes|
|/environments/read|Obter as propriedades de um ambiente.|
|/environments/referencedatasets/delete|Elimina o conjunto de dados de referência.|
|/environments/referencedatasets/read|Obter as propriedades de um conjunto de dados de referência.|
|/environments/referencedatasets/write|Cria um novo conjunto de dados de referência para um ambiente ou atualiza um conjunto de dados de referência existente.|
|/environments/status/read|Obter o estado do ambiente, estado do respetivos operações associados como entrada.|
|/environments/write|Cria um novo ambiente ou atualiza um ambiente existente.|
|/register/action|Regista a subscrição para o fornecedor de recursos de informações de séries de tempo e permite a criação dos ambientes das informações de séries de tempo.|

## <a name="microsoftweb"></a>microsoft.web

| Operação | Descrição |
|---|---|
|/apimanagementaccounts/apiacls/read|Obter Apiacls de contas de gestão de Api.|
|/apimanagementaccounts/apis/apiacls/delete|Elimine Apiacls de APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/apiacls/read|Obter Apiacls de APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/apiacls/write|Atualize Apiacls de APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/connectionacls/read|Obter Connectionacls de APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/connections/confirmconsentcode/action|Certifique-se ligações de APIs de contas de gestão Api código consentimento.|
|/apimanagementaccounts/apis/connections/connectionacls/delete|Elimine a Api Management contas APIs ligações Connectionacls.|
|/apimanagementaccounts/apis/connections/connectionacls/read|Obter a Api Management contas APIs ligações Connectionacls.|
|/apimanagementaccounts/apis/connections/connectionacls/write|Atualize a Api Management contas APIs ligações Connectionacls.|
|/apimanagementaccounts/apis/connections/delete|Elimine ligações de APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/connections/getconsentlinks/action|Obter consentimento ligações para ligações de APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/connections/listconnectionkeys/action|Lista ligação chaves Api contas APIs ligações de gestão.|
|/apimanagementaccounts/apis/connections/listsecrets/action|Lista os segredos Api contas APIs ligações de gestão.|
|/apimanagementaccounts/apis/connections/read|Obter ligações de APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/connections/write|Ligações de APIs de contas de gestão de Api de atualização.|
|/apimanagementaccounts/apis/delete|Elimine APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/localizeddefinitions/delete|Eliminar a Api Management contas APIs localizado definições.|
|/apimanagementaccounts/apis/localizeddefinitions/read|Obter gestão de Api contas APIs localizado definições.|
|/apimanagementaccounts/apis/localizeddefinitions/write|APIs de contas de gestão de Api de atualização localizado definições.|
|/apimanagementaccounts/apis/read|Obter APIs de contas de gestão de Api.|
|/apimanagementaccounts/apis/write|APIs de contas de gestão de Api de atualização.|
|/apimanagementaccounts/connectionacls/read|Obter Connectionacls de contas de gestão de Api.|
|/availablestacks/read|Obter pilhas disponíveis.|
|/billingmeters/read|Obter a lista de faturação medidores.|
|/certificates/Delete|Elimine um certificado existente.|
|/certificates/Read|Obter a lista de certificados.|
|/certificates/Write|Adicionar um certificado novo ou atualizar um já existente.|
|/checknameavailability/read|Verifique se o nome do recurso está disponível.|
|/classicmobileservices/read|Obter clássicos Mobile Services.|
|/connectionGateways/Delete|Elimina um Gateway de ligação.|
|/connectionGateways/Join/Action|Associa um Gateway de ligação.|
|/connectiongateways/liststatus/action|Gateways de ligação de estado de lista.|
|/connectionGateways/ListStatus/Action|Apresenta uma lista de estado de um Gateway de ligação.|
|/connectionGateways/Move/Action|Move um Gateway de ligação.|
|/connectionGateways/Read|Obter a lista de Gateways de ligação.|
|/connectionGateways/Write|Cria ou atualiza um Gateway de ligação.|
|/connections/confirmconsentcode/action|Confirme o código de autorização de ligações.|
|/connections/Delete|Elimina uma ligação.|
|/connections/Join/Action|Associa uma ligação.|
|/connections/listconsentlinks/action|Ligações de consentimento de lista de ligações.|
|/connections/Move/Action|Move uma ligação.|
|/connections/Read|Obter a lista de ligações.|
|ligações/escrita|Cria ou atualiza uma ligação.|
|/customApis/Delete|Elimina uma API personalizada.|
|/customApis/extractApiDefinitionFromWsdl/Action|Extrai a definição da API de um WSDL.|
|/customApis/Join/Action|Associa uma API personalizada.|
|/customApis/listWsdlInterfaces/Action|Apresenta uma lista de interfaces WSDL para uma API personalizada.|
|/customApis/Move/Action|Move uma API personalizada.|
|/customApis/Read|Obter a lista de API personalizada.|
|/customApis/Write|Cria ou atualiza uma API personalizada.|
|/deploymentlocations/read|Obter as localizações de implementação.|
|/geoRegions/Read|Obter a lista de regiões de Georreplicação.|
|/hostingenvironments/capacities/read|Obter as capacidades de ambientes de alojamento.|
|/hostingEnvironments/Delete|Eliminar um ambiente de serviço de aplicações|
|/hostingenvironments/diagnostics/read|Obter o diagnóstico de ambientes de alojamento.|
|/hostingenvironments/inboundnetworkdependenciesendpoints/read|Obter os pontos finais de rede de todas as dependências de entrada.|
|/hostingenvironments/metricdefinitions/read|Obter as definições de métrica de ambientes de alojamento.|
|/hostingenvironments/multirolepools/metricdefinitions/read|Obter as definições de métrica MultiRole conjuntos de ambientes de alojamento.|
|/hostingenvironments/multirolepools/metrics/read|Obter métricas de MultiRole conjuntos de ambientes de alojamento.|
|/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/ metricDefinitions/Read|Obtem as métricas disponíveis para MultiRole de ambiente de serviço de aplicações|
|/hostingEnvironments/multiRolePools/Read|Obter as propriedades de um conjunto de front-end num ambiente de serviço de aplicações|
|/hostingenvironments/multirolepools/skus/read|Obter SKUs de MultiRole conjuntos de ambientes de alojamento.|
|/hostingenvironments/multirolepools/usages/read|Obter utilizações MultiRole conjuntos de ambientes de alojamento.|
|/hostingEnvironments/multiRolePools/Write|Criar um novo conjunto de front-end num ambiente de serviço de aplicações ou atualizar um já existente|
|/hostingenvironments/operations/read|Obter operações de ambientes de alojamento.|
|/hostingenvironments/outboundnetworkdependenciesendpoints/read|Obter os pontos finais de rede de todas as dependências de saída.|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/hostingEnvironments/Read|Obter as propriedades de um ambiente de serviço de aplicações|
|/hostingEnvironments/reboot/Action|Reinicie todas as máquinas no ambiente de serviço de aplicações|
|/hostingenvironments/resume/action|Retomar a ambientes de alojamento.|
|/hostingenvironments/serverfarms/read|Obter planos de serviço de aplicações de ambientes de alojamento.|
|/hostingenvironments/sites/read|Obter aplicações Web de ambientes de alojamento.|
|/hostingenvironments/suspend/action|Suspenda a ambientes de alojamento.|
|/hostingenvironments/usages/read|Obter utilizações de ambientes de alojamento.|
|/hostingenvironments/workerpools/metricdefinitions/read|Obter definições de métrica de Workerpools de ambientes de alojamento.|
|/hostingenvironments/workerpools/metrics/read|Obter métricas de Workerpools de ambientes de alojamento.|
|/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read|Obtem as métricas disponíveis para WorkerPool de ambiente de serviço de aplicações|
|/hostingEnvironments/workerPools/Read|Obter as propriedades de um conjunto de trabalho num ambiente de serviço de aplicações|
|/hostingenvironments/workerpools/skus/read|Obter o alojamento de ambientes Workerpools SKUs.|
|/hostingenvironments/workerpools/usages/read|Obter o alojamento de ambientes Workerpools utilizações.|
|/hostingEnvironments/workerPools/Write|Criar um novo conjunto de trabalho num ambiente de serviço de aplicações ou atualizar um já existente|
|/hostingEnvironments/Write|Criar um novo ambiente de serviço de aplicações ou Atualize uma existente|
|/ishostingenvironmentnameavailable/read|Get se o nome do ambiente de alojamento está disponível.|
|/ishostnameavailable/read|Verifique se o nome do anfitrião está disponível.|
|/isusernameavailable/read|Verifique se o nome de utilizador está disponível.|
|/listSitesAssignedToHostName/Read|Obter os nomes dos sites atribuídos ao nome do anfitrião.|
|/locations/apioperations/read|Obter operações de API de localizações.|
|/locations/connectiongatewayinstallations/read|Obter instalações de Gateway de ligação de localizações.|
|/locations/extractapidefinitionfromwsdl/action|Extrair definição da Api de WSDL para localizações.|
|/locations/listwsdlinterfaces/action|Interfaces WSDL de lista de localizações.|
|/locations/managedapis/apioperations/read|Obter operações de API gerido de localizações.|
|/locations/managedapis/Join/Action|Associa uma API gerida.|
|/locations/managedapis/read|Obter as localizações de APIs geridas.|
|/operations/read|Obter operações.|
|/publishingusers/read|Obter a publicação os utilizadores.|
|/publishingusers/write|Atualize a publicação de utilizadores.|
|/recommendations/Read|Obter a lista de recomendações para subscrições.|
|/register/action|Registe o fornecedor de recursos Microsoft. Web para a subscrição.|
|/resourcehealthmetadata/Read|Obter os metadados do Estado de funcionamento de recursos.|
|/serverfarms/capabilities/read|Obter capacidades de planos do App Service.|
|/serverfarms/Delete|Eliminar uma existente plano do App Service|
|/serverfarms/firstpartyapps/settings/delete|Elimine o serviço de aplicações planos primeiro intervenientes as definições de aplicações.|
|/serverfarms/firstpartyapps/settings/read|Obter definições de aplicações intervenientes primeiro planos do serviço de aplicações.|
|/serverfarms/firstpartyapps/settings/write|Atualize definições de aplicações da primeira planos do serviço de aplicações.|
|/serverfarms/hybridconnectionnamespaces/relays/delete|Elimine reencaminhamentos de espaços de nomes de ligação do serviço de aplicações planos híbrida.|
|/serverfarms/hybridconnectionnamespaces/relays/read|Obter reencaminhamentos de espaços de nomes de ligação do serviço de aplicações planos híbrida.|
|/serverfarms/hybridconnectionnamespaces/relays/sites/read|Obter planos do serviço de aplicações híbridas ligação espaços de nomes reencaminhamentos da aplicações Web.|
|/serverfarms/hybridconnectionplanlimits/read|Obter os limites de plano de ligação do serviço de aplicações planos híbrida.|
|/serverfarms/hybridconnectionrelays/read|Obter planos de serviço de aplicações híbridas reencaminhamentos de ligação.|
|/serverfarms/metricdefinitions/read|Obter as definições de métrica de planos do serviço de aplicações.|
|/serverfarms/metrics/read|Obter métricas de planos de serviço de aplicações.|
|/serverfarms/operationresults/read|Obter os resultados da operação planos do serviço de aplicações.|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read|Obtem as métricas disponíveis para o plano do App Service|
|/serverfarms/Read|Obter as propriedades num plano do App Service|
|/serverfarms/restartSites/Action|Reiniciar todas as aplicações Web dentro de um plano do App Service|
|/serverfarms/sites/read|Obter aplicações de Web de planos do App Service.|
|/serverfarms/skus/read|Obter SKUs de planos de serviço de aplicações.|
|/serverfarms/usages/read|Obter utilizações de planos de serviço de aplicações.|
|/serverfarms/virtualnetworkconnections/gateways/write|Atualize os Gateways de ligações de rede Virtual de planos de serviço de aplicações.|
|/serverfarms/virtualnetworkconnections/read|Obter as ligações de rede Virtual de planos de serviço de aplicações.|
|/serverfarms/virtualnetworkconnections/routes/delete|Elimine rotas de ligações de rede Virtual de planos de serviço de aplicações.|
|/serverfarms/virtualnetworkconnections/routes/read|Obter rotas de ligações de rede Virtual de planos de serviço de aplicações.|
|/serverfarms/virtualnetworkconnections/routes/write|Atualize rotas de ligações de rede Virtual de planos de serviço de aplicações.|
|/serverfarms/workers/reboot/action|Reinicie trabalhadores de planos de serviço de aplicações.|
|/serverfarms/Write|Criar um novo plano do App Service ou atualizar um já existente|
|/sites/analyzecustomhostname/read|Analise o nome do anfitrião personalizado.|
|/sites/applySlotConfig/Action|Aplicar a configuração de ranhura de aplicação do web a partir da ranhura de destino para a aplicação web atual|
|/sites/backup/Action|Criar uma nova cópia de segurança de aplicação de web|
|/sites/backup/read|Obter a cópia de segurança do Web Apps.|
|/sites/backup/write|Atualize a cópia de segurança do Web Apps.|
|/sites/backups/delete|Elimine as cópias de segurança do Web Apps.|
|/sites/backups/list/action|Lista Web Apps as cópias de segurança.|
|/sites/backups/Read|Obter as propriedades de cópia de segurança de uma aplicação web|
|/sites/backups/restore/action|Restaure cópias de segurança do Web Apps.|
|/sites/config/delete|Elimine a configuração de aplicações Web.|
|/sites/config/list/Action|Lista confidenciais definições de segurança da aplicação Web, tais como publicar as credenciais, as definições de aplicação e as cadeias de ligação|
|/sites/config/Read|Obter definições de configuração de aplicação Web|
|/sites/config/Write|Atualizar as definições de configuração da aplicação Web|
|/sites/continuouswebjobs/delete|Elimine tarefas de Web contínua de aplicações Web.|
|/sites/continuouswebjobs/read|Obter as tarefas de Web contínua de aplicações Web.|
|/sites/continuouswebjobs/start/action|Inicie tarefas de Web contínua de aplicações Web.|
|/sites/continuouswebjobs/stop/action|Pare tarefas de Web contínua de aplicações Web.|
|/sites/Delete|Eliminar uma aplicação Web existente|
|/sites/deployments/delete|Elimine as implementações de aplicações Web.|
|/sites/deployments/log/read|Obter o registo de implementações de aplicações Web.|
|/sites/deployments/read|Obter implementações de aplicações Web.|
|/sites/deployments/write|Atualize as implementações de aplicações Web.|
|/sites/diagnostics/analyses/execute/Action|Execute a análise de diagnóstico de aplicações Web.|
|/sites/diagnostics/analyses/read|Obter a análise de diagnóstico de aplicações Web.|
|/sites/diagnostics/aspnetcore/read|Obter o diagnóstico de aplicações Web da aplicação ASP.NET Core.|
|/sites/diagnostics/autoheal/read|Obter Autoheal de diagnóstico de aplicações Web.|
|/sites/diagnostics/deployment/read|Obter a implementação de diagnóstico de aplicações Web.|
|/sites/diagnostics/deployments/read|Obter implementações de diagnóstico de aplicações Web.|
|/sites/diagnostics/detectors/execute/Action|Execute Detector de diagnóstico de aplicações Web.|
|/sites/diagnostics/detectors/read|Obter Detector de diagnóstico de aplicações Web.|
|/sites/diagnostics/failedrequestsperuri/read|Obter os pedidos falhados de diagnóstico de aplicações Web por Uri.|
|/sites/diagnostics/frebanalysis/read|Obter a análise FREB de diagnóstico de aplicações Web.|
|/sites/diagnostics/loganalyzer/read|Obter o analisador de registo de diagnóstico de aplicações Web.|
|/sites/diagnostics/read|Obter as categorias de diagnóstico de aplicações Web.|
|/sites/diagnostics/runtimeavailability/read|Obter a disponibilidade de tempo de execução de diagnóstico de aplicações Web.|
|/sites/diagnostics/servicehealth/read|Obter o estado de funcionamento de serviço Web diagnóstico de aplicações.|
|/sites/diagnostics/sitecpuanalysis/read|Obter a análise de CPU de Site de diagnóstico de aplicações Web.|
|/sites/diagnostics/sitecrashes/read|Obter falhas de Site de diagnóstico de aplicações Web.|
|/sites/diagnostics/sitelatency/read|Obter a latência de Site de diagnóstico de aplicações Web.|
|/sites/diagnostics/sitememoryanalysis/read|Obter a análise de memória de Site de diagnóstico de aplicações Web.|
|/sites/diagnostics/siterestartsettingupdate/read|Obter a atualização de definição de reinício de Site diagnóstico de aplicações Web.|
|/sites/diagnostics/siterestartuserinitiated/read|Obter Web diagnóstico de aplicações Site reinício utilizador iniciado.|
|/sites/diagnostics/siteswap/read|Obter a troca de Site de diagnóstico de aplicações Web.|
|/sites/diagnostics/threadcount/read|Obter a contagem de threads de diagnóstico de aplicações Web.|
|/sites/diagnostics/workeravailability/read|Obter Workeravailability de diagnóstico de aplicações Web.|
|/sites/diagnostics/workerprocessrecycle/read|Obter a Reciclagem de processo de trabalho de diagnóstico de aplicações Web.|
|/sites/domainownershipidentifiers/read|Obter os identificadores de propriedade de domínio de aplicações Web.|
|/sites/domainownershipidentifiers/write|Atualize identificadores de propriedade de domínio de aplicações Web.|
|/sites/functions/action|Aplicações Web de funções.|
|/sites/functions/delete|Elimine funções de aplicações Web.|
|/sites/functions/listsecrets/action|Lista os segredos Web Apps funções.|
|/sites/functions/masterkey/read|Obter Masterkey de funções de aplicações Web.|
|/sites/functions/read|Obter as funções de aplicações Web.|
|/sites/functions/token/read|Token de funções do Get Web Apps.|
|/sites/functions/write|Atualize funções de aplicações Web.|
|/sites/hostnamebindings/delete|Elimine enlaces de nome de anfitrião de aplicações Web.|
|/sites/hostnamebindings/read|Obter os enlaces de nome de anfitrião de aplicações Web.|
|/sites/hostnamebindings/write|Atualize os enlaces de nome de anfitrião de aplicações Web.|
|/sites/hybridconnection/delete|Elimine a ligação de híbrida de aplicações Web.|
|/sites/hybridconnection/read|Obter da ligação híbrida de aplicações Web.|
|/sites/hybridconnection/write|Atualize da ligação híbrida de aplicações Web.|
|/sites/hybridconnectionnamespaces/relays/delete|Elimine reencaminhamentos de espaços de nomes de ligação de híbrida de aplicações Web.|
|/sites/hybridconnectionnamespaces/relays/listkeys/action|Lista as chaves Web aplicações híbridas ligação reencaminhamentos espaços de nomes.|
|/sites/hybridconnectionnamespaces/relays/read|Obter reencaminhamentos de espaços de nomes de ligação de híbrida de aplicações Web.|
|/sites/hybridconnectionnamespaces/relays/write|Atualize reencaminhamentos de espaços de nomes de ligação de híbrida de aplicações Web.|
|/sites/hybridconnectionrelays/read|Obter reencaminhamentos de ligação de híbrida de aplicações Web.|
|/sites/instances/deployments/delete|Elimine as implementações de instâncias de aplicações Web.|
|/sites/instances/deployments/read|Obter implementações de instâncias de aplicações Web.|
|/sites/instances/extensions/log/read|Obter o registo de extensões de instâncias de aplicações Web.|
|/sites/instances/extensions/read|Obter as extensões de instâncias de aplicações Web.|
|/sites/instances/processes/delete|Elimine processos de instâncias de aplicações Web.|
|/sites/instances/processes/read|Obter os processos de instâncias de aplicações Web.|
|/sites/instances/read|Obter instâncias de aplicações Web.|
|/sites/listsyncfunctiontriggerstatus/action|Lista sincronização função acionar Estado as Web Apps.|
|/sites/metricdefinitions/read|Obter definições de métrica de aplicações Web.|
|/sites/metrics/read|Obter métricas de aplicações Web.|
|/sites/metricsdefinitions/read|Obter definições de métrica de aplicações Web.|
|/sites/migratemysql/action|Migre as aplicações MySql Web.|
|/sites/migratemysql/read|Introdução a Web Apps migrar MySql.|
|/sites/networktrace/action|Aplicações de Web de rastreio de rede.|
|/sites/newpassword/action|Aplicações newpassword Web.|
|/sites/operationresults/read|Obter os resultados da operação aplicações Web.|
|/sites/operations/read|Obter operações de aplicações Web.|
|/sites/perfcounters/read|Obter os contadores de desempenho de aplicações Web.|
|/sites/premieraddons/delete|Elimine Addons de Premier de aplicações Web.|
|/sites/premieraddons/read|Obter Addons de Premier de aplicações Web.|
|/sites/premieraddons/write|Atualize Addons de Premier de aplicações Web.|
|/sites/processes/read|Obter os processos de aplicações Web.|
|/sites/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/sites/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/sites/providers/Microsoft.Insights/metricDefinitions/Read|Obtem as métricas disponíveis para a aplicação Web|
|/sites/publiccertificates/delete|Elimine certificados de público de aplicações Web.|
|/sites/publiccertificates/read|Obter certificados de público de aplicações Web.|
|/sites/publiccertificates/write|Atualize certificados de público de aplicações Web.|
|/sites/publish/Action|Publicar uma aplicação Web|
|/sites/publishxml/Action|Obtenha o xml do perfil de publicação para uma aplicação Web|
|/sites/publishxml/read|Obterem aplicações Web publicar XML.|
|/sites/Read|Obter as propriedades de uma aplicação Web|
|/sites/recommendationhistory/read|Obter o histórico de recomendação de aplicações Web.|
|/sites/recommendations/disable/action|Desative as recomendações de aplicações Web.|
|/sites/recommendations/Read|Obter a lista de recomendações para a aplicação web.|
|/sites/recover/action|Recupere as Web Apps.|
|/sites/resetSlotConfig/Action|Repor a configuração de aplicação web|
|/sites/resourcehealthmetadata/read|Obter os metadados do Estado de funcionamento de recursos de aplicações Web.|
|/sites/restart/Action|Reiniciar uma aplicação Web|
|/sites/restore/read|Obter o restauro de aplicações Web.|
|/sites/restore/write|Restaure aplicações Web.|
|/sites/siteextensions/delete|Elimine extensões de Site de aplicações Web.|
|/sites/siteextensions/read|Obter as extensões de Site de aplicações Web.|
|/sites/siteextensions/write|Atualize extensões de Site de aplicações Web.|
|/sites/slots/analyzecustomhostname/read|Introdução a Web Apps ranhuras analisam o nome do anfitrião personalizado.|
|/sites/slots/applySlotConfig/Action|Aplica a configuração de ranhura de aplicação do web a partir da ranhura de destino para a ranhura atual.|
|/sites/slots/backup/Action|Crie nova cópia de segurança da ranhura de aplicação Web.|
|/sites/slots/backup/read|Obter a cópia de segurança de ranhuras de aplicações do Web.|
|/sites/slots/backup/write|Atualize a cópia de segurança de ranhuras de aplicações de Web.|
|/sites/slots/backups/delete|Elimine as cópias de segurança do Web Apps ranhuras.|
|/sites/slots/backups/list/action|Cópias de segurança da ranhuras de aplicações do Web de lista.|
|/sites/slots/backups/Read|Obter as propriedades de cópia de segurança dos ranhuras de aplicação um web|
|/sites/slots/backups/restore/action|Restaure cópias de segurança de ranhuras de aplicações de Web.|
|/sites/slots/config/delete|Elimine a configuração de ranhuras de aplicações Web.|
|/sites/slots/config/list/Action|Lista confidenciais definições de segurança da ranhura de aplicação Web, tais como publicar as credenciais, as definições de aplicação e as cadeias de ligação|
|/sites/slots/config/Read|Obter as definições de configuração da ranhura de aplicação Web|
|/sites/slots/config/Write|Atualizar as definições de configuração da ranhura de aplicação Web|
|/sites/slots/continuouswebjobs/delete|Elimine tarefas de Web contínua de ranhuras de aplicações Web.|
|/sites/slots/continuouswebjobs/read|Obter tarefas de Web contínua de ranhuras de aplicações Web.|
|/sites/slots/continuouswebjobs/start/action|Inicie tarefas de Web contínua de ranhuras de aplicações Web.|
|/sites/slots/continuouswebjobs/stop/action|Pare tarefas de Web contínua de ranhuras de aplicações Web.|
|/sites/slots/Delete|Eliminar uma ranhura de aplicação Web existente|
|/sites/slots/deployments/delete|Elimine as implementações de ranhuras de aplicações Web.|
|/sites/slots/deployments/log/read|Obter o registo de implementações de ranhuras de aplicações Web.|
|/sites/slots/deployments/read|Obter implementações de ranhuras de aplicações Web.|
|/sites/slots/deployments/write|Atualize as implementações de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/analyses/execute/Action|Execute a análise de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/analyses/read|Obter a análise de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/aspnetcore/read|Obter o diagnóstico de ranhuras de aplicações Web para a aplicação ASP.NET Core.|
|/sites/slots/diagnostics/autoheal/read|Obter Autoheal de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/deployment/read|Obter a implementação de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/deployments/read|Obter implementações de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/detectors/execute/Action|Execute Detector de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/detectors/read|Obter Detector de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/frebanalysis/read|Obter Web Apps ranhuras diagnóstico FREB Analysis Services.|
|/sites/slots/diagnostics/loganalyzer/read|Obter o analisador de registo de diagnóstico de ranhuras aplicações Web.|
|/sites/slots/diagnostics/read|Obter o diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/runtimeavailability/read|Obter a disponibilidade de tempo de execução de diagnóstico de ranhuras aplicações Web.|
|/sites/slots/diagnostics/servicehealth/read|Obter o estado de funcionamento do Web Apps ranhuras diagnóstico serviço.|
|/sites/slots/diagnostics/sitecpuanalysis/read|Obter Web Apps ranhuras diagnóstico Site CPU Analysis Services.|
|/sites/slots/diagnostics/sitecrashes/read|Obter Web Apps ranhuras diagnóstico Site falhas.|
|/sites/slots/diagnostics/sitelatency/read|Obter a latência de Site de diagnóstico de ranhuras aplicações Web.|
|/sites/slots/diagnostics/sitememoryanalysis/read|Obter a análise de memória de Site de diagnóstico ranhuras aplicações Web.|
|/sites/slots/diagnostics/siterestartsettingupdate/read|Obter as ranhuras de aplicações de Web diagnóstico atualização de definição de reinício de Site.|
|/sites/slots/diagnostics/siterestartuserinitiated/read|Obter Web Apps ranhuras diagnóstico Site reinício iniciada pelo utilizador.|
|/sites/slots/diagnostics/siteswap/read|Obter a troca de Site de diagnóstico de ranhuras aplicações Web.|
|/sites/slots/diagnostics/threadcount/read|Obter a contagem de threads de diagnóstico de ranhuras aplicações Web.|
|/sites/slots/diagnostics/workeravailability/read|Obter Workeravailability de diagnóstico de ranhuras de aplicações Web.|
|/sites/slots/diagnostics/workerprocessrecycle/read|Obter os Reciclagem do processo de trabalho do diagnóstico de ranhuras do Web Apps.|
|/sites/slots/domainownershipidentifiers/read|Obter os identificadores de propriedade de domínio de ranhuras aplicações Web.|
|/sites/slots/hostnamebindings/delete|Elimine enlaces de nome de anfitrião de ranhuras de aplicações Web.|
|/sites/slots/hostnamebindings/read|Obter os enlaces de nome de anfitrião de ranhuras de aplicações Web.|
|/sites/slots/hostnamebindings/write|Atualize os enlaces de nome de anfitrião de ranhuras de aplicações Web.|
|/sites/slots/hybridconnection/delete|Elimine a ligação de híbrida de ranhuras de aplicações Web.|
|/sites/slots/hybridconnection/read|Obter aplicações da Web ranhuras ligação híbrida.|
|/sites/slots/hybridconnection/write|Atualize ligação de híbrida de ranhuras de aplicações Web.|
|/sites/slots/hybridconnectionnamespaces/relays/delete|Elimine reencaminhamentos de espaços de nomes de ligação ranhuras híbrida Web Apps.|
|/sites/slots/hybridconnectionnamespaces/relays/write|Atualize reencaminhamentos de espaços de nomes de ligação ranhuras híbrida Web Apps.|
|/sites/slots/hybridconnectionrelays/read|Obter Web Apps ranhuras híbrida ligação reencaminhamentos.|
|/sites/slots/instances/deployments/read|Obter implementações de instâncias de ranhuras de aplicações Web.|
|/sites/slots/instances/processes/delete|Elimine processos de instâncias de ranhuras de aplicações Web.|
|/sites/slots/instances/processes/read|Obter os processos de instâncias de ranhuras de aplicações Web.|
|/sites/slots/instances/read|Obter instâncias de ranhuras de aplicações Web.|
|/sites/slots/metricdefinitions/read|Obter definições de métrica de ranhuras de aplicações de Web.|
|/sites/slots/metrics/read|Obter métricas de ranhuras de aplicações Web.|
|/sites/slots/migratemysql/read|Introdução a Web Apps ranhuras migrar MySql.|
|/sites/slots/networktrace/action|Ranhuras de aplicações de Web de rastreio de rede.|
|/sites/slots/newpassword/action|Ranhuras de aplicações newpassword Web.|
|/sites/slots/operationresults/read|Obter resultados de operação de ranhuras de aplicações Web.|
|/sites/slots/operations/read|Obter operações de ranhuras de aplicações Web.|
|/sites/slots/perfcounters/read|Obter os contadores de desempenho de ranhuras de aplicações Web.|
|/sites/slots/phplogging/read|Obter Phplogging de ranhuras de aplicações Web.|
|/sites/slots/premieraddons/delete|Elimine Addons de Premier de ranhuras de aplicações Web.|
|/sites/slots/premieraddons/read|Obter Addons de Premier de ranhuras de aplicações Web.|
|/sites/slots/premieraddons/write|Atualize Addons de Premier de ranhuras de aplicações Web.|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/read|Obtém a definição de diagnóstico do recurso|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/write|Cria ou atualiza a definição do diagnóstico para o recurso|
|/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read|Obtem as métricas disponíveis para a ranhura de aplicação Web|
|/sites/slots/publiccertificates/read|Obter certificados de Web Apps ranhuras públicos.|
|/sites/slots/publiccertificates/write|Criar ou atualizar certificados de Web Apps ranhuras públicos.|
|/sites/slots/publish/Action|Publicar uma ranhura de aplicação Web|
|/sites/slots/publishxml/Action|Obtenha o xml do perfil de publicação para a ranhura de aplicação Web|
|/sites/slots/Read|Obter as propriedades de uma ranhura de implementação de aplicação Web|
|/sites/slots/resetSlotConfig/Action|Repor a configuração de ranhura de aplicação web|
|/sites/slots/resourcehealthmetadata/read|Obter metadados de estado de funcionamento de recursos de ranhuras aplicações Web.|
|/sites/slots/restart/Action|Reiniciar uma ranhura de aplicação Web|
|/sites/slots/restore/read|Obter o restauro de ranhuras de aplicações Web.|
|/sites/slots/restore/write|Restaure ranhuras de aplicações Web.|
|/sites/slots/siteextensions/delete|Elimine extensões de Site de ranhuras de aplicações Web.|
|/sites/slots/siteextensions/read|Obter as extensões de Site de ranhuras de aplicações Web.|
|/sites/slots/siteextensions/write|Atualize extensões de Site de ranhuras de aplicações Web.|
|/sites/slots/slotsdiffs/Action|Obter diferenças na configuração de aplicação web e de ranhuras|
|/sites/slots/slotsswap/Action|Trocar ranhuras de implementação de aplicação Web|
|/sites/slots/snapshots/read|Obter instantâneos de ranhuras de aplicações Web.|
|/sites/slots/sourcecontrols/Delete|Eliminar definições de configuração de controlo de origem da ranhura de aplicação Web|
|/sites/slots/sourcecontrols/Read|Obter as definições de configuração de controlo de origem da ranhura de aplicação Web|
|/sites/slots/sourcecontrols/Write|Atualizar definições de configuração de controlo de origem da ranhura de aplicação Web|
|/sites/slots/start/Action|Iniciar uma ranhura de aplicação Web|
|/sites/slots/stop/Action|Parar uma ranhura de aplicação Web|
|/sites/slots/sync/action|Ranhuras de aplicações Web de sincronização.|
|/sites/slots/triggeredwebjobs/delete|Elimine as Web Apps ranhuras accionadas WebJobs.|
|/sites/slots/triggeredwebjobs/read|Obter Web Apps ranhuras accionadas WebJobs.|
|/sites/slots/triggeredwebjobs/run/action|Execute Web Apps ranhuras accionadas WebJobs.|
|/sites/slots/usages/read|Obter as utilizações de ranhuras de aplicações Web.|
|/sites/slots/virtualnetworkconnections/delete|Elimine ligações de rede Virtual de ranhuras de aplicações Web.|
|/sites/slots/virtualnetworkconnections/gateways/write|Atualize os Gateways de ligações de rede Virtual de ranhuras aplicações Web.|
|/sites/slots/virtualnetworkconnections/read|Obter ligações de rede Virtual de ranhuras de aplicações Web.|
|/sites/slots/virtualnetworkconnections/write|Atualize ligações de rede Virtual de ranhuras de aplicações Web.|
|/sites/slots/webjobs/read|Obter WebJobs de ranhuras de aplicações Web.|
|/sites/slots/Write|Criar uma nova ranhura de aplicação Web ou atualizar um já existente|
|/sites/slotsdiffs/Action|Obter diferenças na configuração de aplicação web e de ranhuras|
|/sites/slotsswap/Action|Trocar ranhuras de implementação de aplicação Web|
|/sites/snapshots/read|Obter instantâneos de aplicações Web.|
|/sites/sourcecontrols/Delete|Eliminar definições de configuração de controlo de origem da aplicação Web|
|/sites/sourcecontrols/Read|Obter as definições de configuração de controlo de origem da aplicação Web|
|/sites/sourcecontrols/Write|Atualizar definições de configuração de controlo de origem da aplicação Web|
|/sites/start/Action|Iniciar uma aplicação Web|
|/sites/stop/Action|Parar uma aplicação Web|
|/sites/sync/action|Aplicações Web de sincronização.|
|/sites/syncfunctiontriggers/action|Acionadores de função de sincronização para aplicações Web.|
|/sites/triggeredwebjobs/delete|Elimine WebJobs accionadas de aplicações Web.|
|/sites/triggeredwebjobs/history/read|Obter o histórico de WebJobs accionadas aplicações Web.|
|/sites/triggeredwebjobs/read|Obter WebJobs accionadas de aplicações Web.|
|/sites/triggeredwebjobs/run/action|Execute Web Apps accionadas WebJobs.|
|/sites/usages/read|Obter as utilizações de aplicações Web.|
|/sites/virtualnetworkconnections/delete|Elimine ligações de rede Virtual de aplicações Web.|
|/sites/virtualnetworkconnections/gateways/read|Obter os Gateways de ligações de rede Virtual de aplicações Web.|
|/sites/virtualnetworkconnections/gateways/write|Atualize os Gateways de ligações de rede Virtual de aplicações Web.|
|/sites/virtualnetworkconnections/read|Obter ligações de rede Virtual de aplicações Web.|
|/sites/virtualnetworkconnections/write|Atualize ligações de rede Virtual de aplicações Web.|
|/sites/webjobs/read|Obter WebJobs de aplicações Web.|
|/sites/Write|Criar uma nova aplicação Web ou atualizar um já existente|
|/skus/read|Obter SKUs.|
|/sourcecontrols/read|Obter controlos de origem.|
|/sourcecontrols/write|Controlos de origem de atualização.|
|/unregister/action|Anular o registo do fornecedor de recursos Microsoft. Web para a subscrição.|
|/validate/action|Valide.|
|/verifyhostingenvironmentvnet/action|Certifique-se de Vnet do ambiente de alojamento.|

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

| Operação | Descrição |
|---|---|
|/components/read|Recursos de operações de leitura|
|/healthInstances/read|Recursos de operações de leitura|
|/Operations/read|Recursos de operações de leitura|
|/workloads/delete|Elimina um recurso de carga de trabalho|
|/workloads/read|Lê um recurso de carga de trabalho|
|/workloads/write|Escreve um recurso de carga de trabalho|

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar uma função personalizada](role-based-access-control-custom-roles.md).
- Reveja o [RBAC funções incorporadas](role-based-access-built-in-roles.md).
- Saiba como gerir atribuições de acesso [por utilizador](role-based-access-control-manage-assignments.md) ou [por recurso](role-based-access-control-configure.md) 
- Saiba como [ver registos de atividade para auditar as ações em recursos](~/articles/azure-resource-manager/resource-group-audit.md)
