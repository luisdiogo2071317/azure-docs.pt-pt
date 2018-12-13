---
title: Proteger o seu Cofre de chave do Azure | Documentos da Microsoft
description: Gerir permissões de acesso para o Azure Key Vault, chaves e segredos. Abrange o modelo de autenticação e autorização para o Cofre de chaves e como proteger o seu Cofre de chaves.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: ambapat
ms.openlocfilehash: 67f24bbccdd2dcf5cca09e09557d7ebebd0a5c2d
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52891083"
---
# <a name="secure-your-key-vault"></a>Proteger o seu cofre de chaves
O Azure Key Vault é um serviço cloud que salvaguarda as chaves de encriptação e os segredos (tal como certificados, cadeias de ligação e palavras-passe). Uma vez que estes dados são confidenciais e críticos de negócios, deve proteger o acesso aos seus cofres de chaves, permitindo que apenas autorizado aplicativos e usuários. Este artigo fornece uma descrição geral do modelo de acesso do Cofre de chaves. Ele explica a autenticação e autorização e descreve como proteger o acesso.

## <a name="overview"></a>Descrição geral
O controlo aos cofres de chaves é controlado através de duas interfaces separadas - o plano de gestão e o plano de dados. 
**Plano de gestão** lida com a gerir o cofre, por exemplo, - criar um cofre, a atualizar um cofre, eliminar um cofre. 
**Plano de dados** negociações com segredos dentro de um cofre, que está a criar, atualizar, eliminar e ler um segredo no interior do cofre. Para ambos os planos, autenticação e autorização adequadas são necessárias antes de um chamador (um utilizador ou uma aplicação) pode obter acesso ao Cofre de chaves. A autenticação estabelece a identidade do chamador, ao passo que autorização determina as operações que o chamador tem permissão para realizar.

Tanto o plano de gestão, como o de dados, utilizam o Azure Active Directory na autenticação. Relativamente à autorização, o plano de gestão utiliza o controlo de acesso baseado em funções (RBAC) e o plano de dados utiliza a política de acesso ao cofre de chaves.

Eis uma breve descrição geral dos tópicos abrangidos:

[Autenticação com o Azure Active Directory](#authentication-using-azure-active-directory) - esta secção explica como um chamador se autentica com o Azure Active Directory para aceder a um cofre de chaves através dos planos de gestão e de dados. 

Para a autenticação, ambos os planos utilizam o Azure Active Directory (Azure AD). Para autorização, o plano de gestão utiliza o controlo de acesso baseado em funções (RBAC), enquanto o plano de dados utiliza a política de acesso do Cofre de chaves.

## <a name="authenticate-by-using-azure-active-directory"></a>Autenticar com o Azure Active Directory
Quando cria um cofre de chaves numa subscrição do Azure, é automaticamente associado com o inquilino do Azure AD da subscrição. Todos os chamadores têm de estar registados neste inquilino e tem de ser autenticado para aceder ao Cofre de chave. Este requisito aplica-se para ambos os acesso do plano de dados e de plano de gestão. Em ambos os casos, um aplicativo pode acessar o Cofre de chaves de duas formas:

* **acesso à aplicação de utilizador +**: usada com aplicativos que o Cofre de chaves de acesso em nome de um utilizador com sessão iniciada. O Azure PowerShell e o portal do Azure são exemplos deste tipo de acesso. Existem duas formas de conceder acesso aos utilizadores: 
  - Cofre de chaves de acesso de qualquer aplicativo.
  - Aceder ao Cofre de chaves apenas quando utilizam uma aplicação específica (conhecida como identidade composta).

* **acesso só de aplicação**: usadas com os aplicativos que são executados como serviços de daemon ou tarefas em segundo plano. É concedido acesso ao cofre de chaves à identidade da aplicação.

Em ambos os tipos de aplicativos, a aplicação autentica-se com o Azure AD utilizando qualquer um da [métodos de autenticação suportados](../active-directory/develop/authentication-scenarios.md)e recebe um token. O método de autenticação utilizado depende do tipo de aplicação. Em seguida, o aplicativo utiliza esse token e envia um pedido de REST API para o Key Vault. Pedidos de plano de gestão são encaminhados através de um ponto de final do Azure Resource Manager. Ao aceder ao plano de dados, o aplicativo se conectará diretamente para um ponto de final do Cofre de chaves. Para obter mais informações, consulte a [fluxo de autenticação completo](../active-directory/develop/v1-protocols-oauth-code.md). 

O nome do recurso para o qual a aplicação pede que um token depende de qual plano a aplicação está a aceder. O nome do recurso é um ponto de final do plano de gestão ou um plano de dados ponto final, dependendo do ambiente do Azure. Para obter mais detalhes, consulte a tabela neste artigo.

Ter um mecanismo único para a autenticação para ambos os planos tem alguns benefícios:

* As organizações podem controlar centralmente o acesso a todos os cofres de chaves na sua organização.
* Se um utilizador sai, ele instantaneamente perde o acesso a todos os cofres da organização.
* As organizações podem personalizar a autenticação através das opções disponíveis no Azure AD (por exemplo, ativar autenticação multifator para aumentar a segurança).

## <a name="the-management-plane-and-the-data-plane"></a>O plano de gestão e o plano de dados
Utilize o plano de gestão para gerir o Cofre de chaves propriamente dito. Isto inclui operações como gerir atributos e definir políticas de acesso do plano de dados. Utilize o plano de dados para adicionar, eliminar, modificar e usar as chaves, segredos e certificados armazenados no Cofre de chaves.

Acessa as interfaces de plano de dados e de plano de gerenciamento por meio dos pontos de extremidade diferentes listados na tabela seguinte. A segunda coluna da tabela descreve os nomes DNS destes pontos finais em diferentes ambientes do Azure. A terceira coluna descreve as operações que pode fazer a partir de cada plano de acesso. Cada plano de acesso também tem seu próprio mecanismo de controlo de acesso. Controlo de acesso do plano de gestão é definido utilizando o controlo de acesso baseado em funções do Azure Resource Manager (RBAC). Controlo de acesso do plano de dados está definido com a política de acesso do Cofre de chaves.

| Plano de acesso | Pontos finais de acesso | Operações | Mecanismo de controlo de acesso |
| --- | --- | --- | --- |
| Plano de gestão |**Global:**<br> management.azure.com:443<br><br> **O Azure na China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Alemanha:**<br> management.microsoftazure.de:443 |Criar/ler/atualizar/eliminar Cofre de chaves <br> Definir políticas de acesso para o Key Vault<br>Definir etiquetas para o Cofre de chaves |RBAC de Gestor de recursos do Azure |
| Plano de dados |**Global:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **O Azure na China 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Alemanha:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |Para as chaves: desencriptar, encriptar, UnwrapKey, WrapKey, verifique se, início de sessão, Get, lista, atualizar, criar, importar, Delete, cópia de segurança, restauro<br><br> Em segredos: obter, listar, definir, eliminar |Política de acesso do Cofre de chaves |

Plano de dados e o plano acesso controlos de gestão de trabalham de forma independente. Por exemplo, se quiser conceder um acesso de aplicação para utilizar as chaves num cofre de chaves, só tem de conceder acesso do plano de dados. Conceder acesso através de políticas de acesso do Cofre de chaves. Por outro lado, um utilizador que precisa para ler as propriedades do Cofre de chaves e etiquetas, mas não aceder a dados (chaves, segredos ou certificados), só precisa de acesso do plano de gestão. Conceder acesso ao atribuir o acesso de leitura para o utilizador com o RBAC.

## <a name="management-plane-access-control"></a>Controlo de acesso do plano de gestão
O plano de gestão consiste em operações que afetam o Cofre de chaves propriamente dito, tais como:

- Criar ou eliminar um cofre de chaves.
- Obter uma lista dos cofres numa subscrição.
- A obter propriedades do Cofre de chaves (por exemplo, SKU e etiquetas).
- Definir políticas de acesso do Cofre de chaves que o utilizador de controle e da aplicação acedem a chaves e segredos.

O controlo de acesso do plano de gestão utiliza o RBAC.  

### <a name="role-based-access-control-rbac"></a>Controlo de acesso baseado em funções (RBAC)
Cada subscrição do Azure tem uma instância do Azure AD. Conceder acesso a utilizadores, grupos e aplicações deste diretório para gerir os recursos na subscrição do Azure que utilizam o modelo de implementação Azure Resource Manager. Este tipo de controlo de acesso é referido como o RBAC. Para gerir este acesso, pode utilizar o [portal do Azure](https://portal.azure.com/), as [ferramentas da CLI do Azure](../cli-install-nodejs.md), o [PowerShell](/powershell/azureps-cmdlets-docs) ou as [APIs REST do Azure Resource Manager](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Criar um cofre de chaves num grupo de recursos e controlar o acesso ao plano de gestão com o Azure AD. Por exemplo, pode conceder a utilizadores ou um grupo a capacidade de gerir cofres de chaves num grupo de recursos.

Pode conceder acesso a utilizadores, grupos e aplicações com um âmbito específico através da atribuição de funções RBAC adequadas. Por exemplo, para conceder acesso a um utilizador para gerir cofres de chaves, atribuir uma função de contribuinte do Cofre de chave predefinida para este utilizador, com um âmbito específico. O âmbito nesse caso seria uma subscrição, um grupo de recursos ou um cofre de chaves específico. As funções atribuídas ao nível da subscrição se aplica a todos os grupos de recursos e recursos nessa subscrição. As funções atribuídas ao nível do grupo de recursos se aplica a todos os recursos nesse grupo de recursos. As funções atribuídas a um recurso específico aplica-se a esse recurso. Existem várias funções predefinidas (veja [RBAC: funções incorporadas](../role-based-access-control/built-in-roles.md)). Se uma função predefinida não atender a suas necessidades, pode definir sua própria função.

> [!IMPORTANT]
> Tenha em atenção que se um utilizador tiver contribuinte permissões para um plano de gestão do Cofre de chaves, she pode concedem si próprio acesso ao plano de dados através da definição de política de acesso do Cofre de chaves. Portanto, deve controlar rigidamente quem tem acesso de contribuinte aos seus cofres de chaves. Certifique-se de que só pessoas autorizadas podem aceder e gerir cofres de chaves, chaves, segredos e certificados.
> 
> 

## <a name="data-plane-access-control"></a>Controlo de acesso do plano de dados
Operações de plano de dados do Cofre de chaves se aplica a objetos armazenados como chaves, segredos e certificados. Operações-chave incluem criar, importar, atualizar, listar, fazer cópias de segurança e restaurar chaves. Operações criptográficas incluem o início de sessão, verificarem, encriptarem, desencriptam, encapsulam, anular a moldagem, defina etiquetas e outros atributos de chaves. Da mesma forma, as operações no segredos incluem get, o conjunto, listar, eliminar.

Conceder acesso do plano de dados através da definição de políticas de acesso para um cofre de chaves. Um utilizador, grupo ou aplicação tem de ter permissões de Contribuidor para o plano de gestão para um cofre de chaves poder definir políticas de acesso para o Cofre de chaves. Pode conceder a um utilizador, grupo ou acesso a aplicações para fazerem operações específicas de chaves ou segredos num cofre de chaves. Key Vault suporta até 1024 entradas de política de acesso para um cofre de chaves. Para conceder acesso do plano de dados a vários utilizadores, crie um grupo de segurança do Azure AD e adicionar utilizadores a esse grupo.

### <a name="key-vault-access-policies"></a>Políticas de acesso do Cofre de chaves
Políticas de acesso do Cofre de chaves concedem permissões para chaves, segredos e certificados em separado. Por exemplo, pode permitir que um utilizador acesso só a chaves e não permissões para segredos. Permissões para aceder a chaves, segredos ou certificados são ao nível do cofre. Política de acesso do Cofre de chaves não suporta permissões granulares, ao nível do objeto, como uma chave específica, o segredo ou o certificado. Pode utilizar o [portal do Azure](https://portal.azure.com/), o [ferramentas de CLI do Azure](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), ou o [APIs de REST de gestão de Cofre de chave](https://msdn.microsoft.com/library/azure/mt620024.aspx) para definir políticas de acesso para um cofre de chaves.

> [!IMPORTANT]
> Políticas de acesso do Key Vault aplicam-se ao nível do cofre. Por exemplo, quando é concedida permissão a um utilizador para criar e eliminar chaves, esse utilizador pode fazer essas operações em todas as chaves no cofre de chaves.

Além de utilizar as políticas de acesso, também pode restringir o acesso do plano de dados usando [pontos finais de serviço de rede Virtual para o Azure Key Vault](key-vault-overview-vnet-service-endpoints.md). Configura [Firewalls e regras de rede virtual](key-vault-network-security.md) para uma camada adicional de segurança.

## <a name="example"></a>Exemplo
Digamos que estiver desenvolvendo um aplicativo que utiliza um certificado para SSL, o armazenamento do Azure para armazenar dados e uma chave RSA de 2048 bits para operações de início de sessão. Digamos que esta aplicação está em execução na máquina virtual do Azure (ou um conjunto de dimensionamento de máquinas virtuais). Pode utilizar um cofre de chaves para armazenar todos os segredos da aplicação e armazenar o certificado de arranque de configuração utilizado pela aplicação para autenticar com o Azure AD.

Aqui está um resumo dos tipos de chaves e segredos armazenados:

* **Certificado SSL**: utilizado para SSL.
* **Chave de armazenamento**: utilizado para obter acesso à conta de armazenamento.
* **Chave RSA de 2048 bits**: utilizado para operações de início de sessão.
* **Certificado de arranque de configuração**: utilizado para autenticar com o Azure AD. Depois do acesso é concedido, pode obter a chave de armazenamento e utilizar a chave RSA para a assinatura.

Agora vamos ver quem é gerir, implementar e auditar esta aplicação. Vamos utilizar três funções neste exemplo.

* **Equipe de segurança**: equipe de TI normalmente no escritório de CSO (Chief Security Officer), ou equivalente. Esta equipe é responsável por proteger adequadamente dos segredos. Os exemplos incluem certificados SSL, chaves RSA utilizadas para assinaturas, cadeias de ligação e as chaves de conta de armazenamento.
* **Programadores/operadores**: pessoas que desenvolveram a aplicação e, em seguida, implementação-la no Azure. Normalmente, eles não são parte da equipa de segurança, e, portanto, eles não devem ter acesso a dados confidenciais, tais como certificados SSL e chaves RSA. Apenas a aplicação que implementam deve ter acesso a esses objetos.
* **Auditores**: geralmente, um conjunto diferente de pessoas, isoladas de desenvolvedores e equipe de TI geral. Suas responsabilidades é rever a utilização e manutenção dos certificados, chaves e segredos para garantir a conformidade com normas de segurança. 

Há uma outra função que está fora do escopo desta aplicação, mas relevante aqui para ser mencionado. Essa função é a subscrição (ou grupo de recursos) administrador. O administrador da subscrição configura as permissões de acesso inicial para a equipe de segurança. O administrador da subscrição concede acesso à equipa de segurança, utilizar um grupo de recursos que contém os recursos exigidos por esta aplicação.

Agora, vamos ver que ações são realizadas por cada função no contexto da aplicação.

* **Equipa de segurança**
  * Cria cofres de chaves.
  * Ativa o registo do Cofre de chaves.
  * Adiciona chaves/segredos.
  * Cria uma cópia de segurança de chaves para recuperação após desastre.
  * Define a política de acesso do Key Vault para conceder permissões a utilizadores e aplicações para fazerem operações específicas.
  * Agrega periodicamente chaves/segredos.
* **Programadores/operadores**
  * Obter referências para o arranque e certificados SSL (thumbprints), a chave de armazenamento (URI do segredo) e a chave de assinatura (URI da chave) da equipa de segurança.
  * Desenvolver e implementar aplicações que acedam a chaves e segredos programaticamente.
* **Auditores**
  * Reveja os registos de utilização para confirmar a utilização de chaves/segredos adequada e conformidade com as normas de segurança de dados.

Agora vamos ver quais permissões de acesso são necessárias para cada função e da aplicação, para realizar suas tarefas atribuídas. 

| Função de utilizador | Permissões do plano de gestão | Permissões do plano de dados |
| --- | --- | --- |
| Equipa de Segurança |Contribuidor do Key Vault |Chaves: criar cópia de segurança, criar, eliminar, obter, importar, listar, restaurar <br> Segredos: todas |
| Programadores/operadores |A implementação de Cofre de chave de permissão, para que as máquinas virtuais que implementam possam obter segredos a partir do Cofre de chave. |Nenhuma |
| Auditores |Nenhuma |Chaves: listar<br>Segredos: listar |
| Aplicação |Nenhuma |Chaves: assinar<br>Segredos: obter |

> [!NOTE]
> Auditores precisam de permissão de lista de chaves e segredos, para que possam inspecionar atributos de chaves e segredos que não são emitidos nos registos. Esses atributos incluem marcas, ativação e datas de expiração.
> 
> 

Além de permissões do Cofre de chaves, todas as três funções também precisam de acesso a outros recursos. Por exemplo, para poder implementar VMs (ou a funcionalidade de aplicações Web do serviço de aplicações do Azure), os desenvolvedores e operadores também tem acesso de contribuinte a esses tipos de recursos. Os auditores requerem acesso de leitura à conta de armazenamento onde são armazenados os registos do Key Vault.

Uma vez que o foco deste artigo está a proteger o acesso ao seu Cofre de chaves, ilustraremos apenas conceitos âmbito deste tópico. Obter detalhes sobre a implementação de certificados e aceder a chaves e segredos programaticamente são abordados em outro lugar. Por exemplo:

- Implementação de certificados armazenados no Cofre de chaves para VMs é abordada [implementar certificados para VMs de Cofre de chaves geridas pelo cliente](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) (mensagem de blogue).
- O [baixe exemplos de cliente do Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45343) ilustra como usar um certificado de arranque de configuração para autenticar para o Azure AD para aceder a um cofre de chaves.

Pode conceder a maioria das permissões de acesso com o portal do Azure. Para conceder permissões granulares, poderá ter de utilizar o Azure PowerShell ou a CLI para alcançar o resultado desejado. 

Os fragmentos do PowerShell seguintes presumem que:

* O administrador do Azure AD criar grupos de segurança que representam as três funções (a equipe de segurança da Contoso, Devops de aplicação da Contoso e auditores de aplicações da Contoso). O administrador também adicionou utilizadores aos grupos a que pertencem.
* **ContosoAppRG** é o grupo de recursos onde residem todos os recursos. **contosologstorage** é onde são armazenados os registos. 
* O Cofre de chaves **ContosoKeyVault**e a conta de armazenamento utilizada para os registos do Key Vault **contosologstorage**, tem de estar na mesma localização do Azure.

Primeiro o administrador da subscrição atribui `key vault Contributor` e `User Access Administrator` funções para a equipe de segurança. Estas funções permitem que a equipe de segurança gerir o acesso a outros recursos e gerir cofres de chaves, no grupo de recursos ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

O script seguinte mostra como a equipe de segurança pode criar um cofre de chaves e configurar permissões de acesso e de registo. Para obter detalhes sobre as permissões de política de acesso do Cofre de chaves, consulte [sobre o Azure Key Vault chaves, segredos e certificados](about-keys-secrets-and-certificates.md).

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

A função personalizada definida só é atribuível à subscrição na qual o `ContosoAppRG` é criado o grupo de recursos. Se as mesmas funções personalizadas serão utilizadas para outros projetos de outras subscrições, o seu âmbito pode ter mais subscrições adicionadas.

A atribuição de função personalizada para a permissão "implementar/ação" para programadores e operadores tem um âmbito para o grupo de recursos. Isso permite que apenas as VMs criadas no grupo de recursos `ContosoAppRG` para ter acesso a segredos (certificado SSL e certificado de arranque do sistema). As VMs criadas no outro grupo de recursos por um desenvolvedores e membro da equipe de operadores não terá acesso a estes segredos, mesmo que tenham os respetivos URIs.

Este exemplo mostra um cenário simples. Cenários da vida real podem ser mais complexos e pode ajustar as permissões para o seu Cofre de chaves com base nas suas necessidades. No nosso exemplo, partimos do princípio que a equipe de segurança fornece a chave e segredas referências (URIs e thumbprints) que os desenvolvedores e os operadores têm de referenciar nas respetivas aplicações. Os desenvolvedores e operadores não necessitam de qualquer acesso do plano de dados. Este exemplo se foca em proteger o seu Cofre de chaves. Deve dar considerações semelhantes para proteger [as suas VMs](https://azure.microsoft.com/services/virtual-machines/security/), [contas de armazenamento](../storage/common/storage-security-guide.md)e outros recursos do Azure.

> [!NOTE]
> Este exemplo mostra como o acesso do Cofre de chaves estará bloqueado na produção. Os desenvolvedores devem ter sua própria subscrição ou grupo de recursos em que têm permissões completas para gerir os seus cofres, VMs e conta de armazenamento em que desenvolveram o aplicativo.

É altamente recomendável que ainda mais acesso seguro ao seu Cofre de chaves por [configurar o Cofre de chaves firewalls e redes virtuais](key-vault-network-security.md).

## <a name="resources"></a>Recursos
* [Controlo de acesso baseado em funções do Azure Active Directory](../role-based-access-control/role-assignments-portal.md)
  
* [RBAC: Funções Incorporadas](../role-based-access-control/built-in-roles.md)
  
* [Compreender a implementação do Resource Manager e a implementação clássica](../azure-resource-manager/resource-manager-deployment-model.md)
  
* [Gerir controlo de acesso baseado em funções com o Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)
  
* [Gerir o controlo de acesso baseado em funções com a API REST](../role-based-access-control/role-assignments-rest.md)
  
* [Controlo de acesso baseado em funções para o Microsoft Azure do Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Este 2015 Microsoft Ignite aborda de vídeo de conferência gestão de acesso e capacidades de relatórios no Azure. Também explora as práticas recomendadas para proteger o acesso a subscrições do Azure ao utilizar o Azure AD.
* [Autorizar o acesso a aplicações web com OAuth 2.0 e o Azure AD](../active-directory/develop/v1-protocols-oauth-code.md)
  
* [REST APIs de gestão do Cofre de chaves](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Este documento é a referência para as APIs REST para gerir o seu Cofre de chaves programaticamente, incluindo a definição de política de acesso do Cofre de chaves.
* [APIs REST do Cofre de chaves](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
* [Controlo de acesso a chaves](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
* [Controlo de acesso a segredos](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
* [Definir](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) e [remover](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) política de acesso do Key Vault com o PowerShell
  
## <a name="next-steps"></a>Passos Seguintes
[Configurar firewalls e redes virtuais do Key Vault](key-vault-network-security.md)

Para uma introdução tutorial para um administrador, consulte [Introdução ao cofre de chave do Azure](key-vault-get-started.md).

Para obter mais informações sobre o registo de utilização do Key Vault, veja [Registo do Azure Key Vault](key-vault-logging.md).

Para obter mais informações sobre a utilização de chaves e segredos com o Azure Key Vault, consulte [sobre chaves e segredos](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Se tiver dúvidas sobre o Key Vault, visite o [fóruns](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

