---
title: Proteger o seu Cofre de chave do Azure | Documentos da Microsoft
description: Gerir permissões de acesso para o key vault para gerir o Azure Key Vault, chaves e segredos. Modelo de autenticação e autorização para o Cofre de chaves e como proteger o seu Cofre de chaves.
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
ms.openlocfilehash: 58307b25c03202fea63be25136c5a9c5a2d32473
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079043"
---
# <a name="secure-your-key-vault"></a>Proteger o seu cofre de chaves
O Azure Key Vault é um serviço cloud que salvaguarda as chaves de encriptação e os segredos (tal como certificados, cadeias de ligação, palavras-passe). Uma vez que estes dados são confidenciais e críticos de negócios, acesso aos seus cofres de chaves tem de estar protegido, permitindo que apenas autorizados aplicações e os utilizadores para obter acesso. 

Este artigo fornece uma descrição geral do modelo de acesso do Cofre de chaves. Ele explica a autenticação e autorização e descreve como proteger o acesso ao Cofre de chaves.

## <a name="overview"></a>Descrição geral
O controlo aos cofres de chaves é controlado através de duas interfaces separadas - o plano de gestão e o plano de dados. Para ambos os planos, autenticação e autorização adequadas são necessárias antes de um chamador (um utilizador ou uma aplicação) pode obter acesso ao Cofre de chaves. A autenticação estabelece a identidade do chamador, ao passo que autorização determina as operações que o chamador tem permissão para realizar.

Tanto o plano de gestão, como o de dados, utilizam o Azure Active Directory na autenticação. Relativamente à autorização, o plano de gestão utiliza o controlo de acesso baseado em funções (RBAC) e o plano de dados utiliza a política de acesso ao cofre de chaves.

Eis uma breve descrição geral dos tópicos abrangidos:

[Autenticação com o Azure Active Directory](#authentication-using-azure-active-directory) - esta secção explica como um chamador se autentica com o Azure Active Directory para aceder a um cofre de chaves através dos planos de gestão e de dados. 

[Plano de gestão e plano de dados](#management-plane-and-data-plane) - o plano de gestão e o plano de dados são dois planos de acesso utilizados para aceder aos seus cofres de chaves. Cada plano de acesso suporta operações específicas. Esta secção descreve os pontos finais de acesso, as operações suportadas e o método de controlo de acesso que cada plano utiliza. 

[Controlo de acesso do plano de gestão](#management-plane-access-control) - nesta secção, vamos ver como permitir acesso a operações do plano de gestão com o controlo de acesso baseado em funções.

[Controlo de acesso do plano de dados](#data-plane-access-control) - esta secção descreve como utilizar a política de acesso ao cofre de chaves para controlar o acesso ao plano de dados.

[Exemplo](#example) -este exemplo descreve como configurar o controlo de acesso para o seu Cofre de chaves permitir que três equipas diferentes (equipa de segurança, programadores/operadores e auditores) para executar tarefas específicas para desenvolver, gerir e monitorizar uma aplicação no Azure.

## <a name="authentication-using-azure-active-directory"></a>Autenticação com o Azure Active Directory
Quando cria um cofre de chaves numa subscrição do Azure, é automaticamente associado com o inquilino do Azure Active Directory da subscrição. Todos os chamadores (utilizadores e aplicações) têm de estar registados neste inquilino e tem de ser autenticado para aceder ao Cofre de chave. Este requisito aplica-se para ambos os acesso do plano de dados e de plano de gestão. Em ambos os casos, as aplicações podem aceder a um cofre de chaves de duas formas:

* **acesso à aplicação de utilizador +** - utilizado com aplicativos que aceder ao Cofre de chaves em nome de um utilizador com sessão iniciada. O Azure PowerShell e o portal do Azure são exemplos deste tipo de acesso. Existem duas formas de conceder acesso aos utilizadores: 
- Conceder acesso aos utilizadores para que possam aceder Cofre de chaves a partir de qualquer aplicação.
- Conceda acesso ao Cofre de chaves apenas quando utilizam uma aplicação específica (conhecida como identidade composta).

* **acesso só de aplicação** - utilizado com aplicativos executados como serviços de daemon ou tarefas em segundo plano. É concedido acesso ao cofre de chaves à identidade da aplicação.

Em ambos os tipos de aplicações, a aplicação autentica-se com o Azure Active Directory através de um dos [métodos de autenticação suportados](../active-directory/develop/authentication-scenarios.md) e recebe um token. O método de autenticação utilizado depende do tipo de aplicação. Depois, a aplicação utiliza esse token e envia um pedido à API REST para o cofre de chaves. Pedidos de plano de gestão são encaminhados através de um ponto de final do Azure Resource Manager. Ao aceder ao plano de gestão, a aplicação fala diretamente com um ponto final do cofre de chaves. Veja mais detalhes sobre o [fluxo de autenticação completo](../active-directory/develop/v1-protocols-oauth-code.md). 

O nome do recurso relativamente ao qual a aplicação pede um token é diferente, consoante a aplicação esteja a aceder ao plano de gestão ou de dados. Por conseguinte, o nome do recurso é o ponto final do plano de gestão ou do plano de dados, descrito na tabela numa secção anterior, dependendo do ambiente do Azure.

Ter um mecanismo único para autenticação nos dois planos tem as suas vantagens:

* As organizações podem controlar centralmente o acesso a todos os cofres de chaves das mesmas.
* Se um utilizador sair da organização, perde imediatamente o acesso a todos os cofres da organização.
* As organizações podem personalizar a autenticação através das opções disponíveis no Azure Active Directory (por exemplo, podem ativar a autenticação multifator para aumentar a segurança).

## <a name="management-plane-and-data-plane"></a>Plano de gestão e plano de dados
O Azure Key Vault é um serviço do Azure disponível por meio do modelo de implementação Azure Resource Manager. Quando cria um cofre de chaves, recebe um contentor virtual para armazenar objetos confidenciais, tais como chaves, segredos e certificados. Operações específicas são executadas num cofre de chaves com interfaces de plano de dados e de plano de gestão. O plano de gestão é utilizado para gerir o Cofre de chaves propriamente dito. Isto inclui operações como gerir atributos e definir políticas de acesso do plano de dados. A interface de plano de dados é utilizada para adicionar, eliminar, modificar e usar as chaves, segredos e certificados armazenados no Cofre de chaves.

As interfaces de plano de dados e de plano de gestão são acedidas através de pontos finais diferentes listados abaixo. A segunda coluna descreve os nomes DNS destes pontos finais em diferentes ambientes do Azure. A terceira coluna descreve as operações que pode fazer a partir de cada plano de acesso. Cada plano de acesso também tem seu próprio mecanismo de controlo de acesso. Controlo de acesso do plano de gestão é definido com o controlo de acesso baseado (RBAC). Controlo de acesso do plano de dados é definido com política de acesso do Cofre de chaves.

| Plano de acesso | Pontos finais de acesso | Operações | Mecanismo de controlo de acesso |
| --- | --- | --- | --- |
| Plano de gestão |**Global:**<br> management.azure.com:443<br><br> **O Azure na China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Alemanha:**<br> management.microsoftazure.de:443 |Criar/Ler/Atualizar/Eliminar cofre de chaves <br> Definir políticas de acesso ao cofre de chaves<br>Definir etiquetas para o cofre de chaves |Controlo de Acesso Baseado em Funções (RBAC) do Azure Resource Manager |
| Plano de dados |**Global:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **O Azure na China 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Alemanha:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |Em chaves: desencriptar, encriptar, unwrapKey, wrapKey, verificar, assinar, obter, listar, atualizar, criar, importar, eliminar, fazer cópia de segurança, restaurar<br><br> Em segredos: obter, listar, definir, eliminar |Política de acesso do cofre de chaves |

Plano de dados e o plano acesso controlos de gestão de trabalham de forma independente. Por exemplo, se quiser conceder um acesso de aplicação para utilizar as chaves num cofre de chaves, só tem de conceder acesso do plano de dados. O acesso é concedido através de políticas de acesso do Cofre de chaves. Por outro lado, um utilizador que precisa para ler as propriedades do cofre e etiquetas, mas não aceder a dados (chaves, segredos ou certificados), só precisa controlar o acesso do plano. O acesso é concedido ao atribuir o "acesso de leitura' para o utilizador, utilizando o RBAC.

## <a name="management-plane-access-control"></a>Controlo de acesso do plano de gestão
O plano de gestão consiste em operações que afetam o Cofre de chaves propriamente dito, tais como:

- Criar ou eliminar um cofre de chaves.
- Obter uma lista dos cofres numa subscrição.
- A obter propriedades do Cofre de chaves (por exemplo, o SKU e as etiquetas).
- Definir políticas de acesso do Cofre de chaves que o utilizador de controle e da aplicação acedem a chaves e segredos.

O controlo de acesso do plano de gestão utiliza o RBAC. Ver a lista completa das operações do Cofre de chaves que podem ser efetuadas por plano de gestão, na tabela na secção anterior. 

### <a name="role-based-access-control-rbac"></a>Controlo de Acesso Baseado em Funções (RBAC)
Cada subscrição do Azure tem um Azure Active Directory. Os utilizadores, grupos e aplicações deste diretório podem receber acesso para gerir recursos na subscrição do Azure que utilizem o modelo de implementação Azure Resource Manager. Este tipo de controlo de acesso tem o nome de Controlo de Acesso Baseado em Funções (Role-Based Access Control, RBAC). Para gerir este acesso, pode utilizar o [portal do Azure](https://portal.azure.com/), as [ferramentas da CLI do Azure](../cli-install-nodejs.md), o [PowerShell](/powershell/azureps-cmdlets-docs) ou as [APIs REST do Azure Resource Manager](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Criar um cofre de chaves num recurso grupo e controlar o acesso ao plano de gestão com o Azure Active Directory. Por exemplo, pode conceder a utilizadores ou um grupo a capacidade de gerir cofres de chaves num grupo de recursos.

Pode conceder acesso a utilizadores, grupos e aplicações com um âmbito específico através da atribuição de funções RBAC adequadas. Por exemplo, para conceder acesso a um utilizador para gerir cofres de chaves, atribuir uma função de 'Contribuinte do Cofre de chave' predefinida para este utilizador, com um âmbito específico. O âmbito nesse caso seria uma subscrição, um grupo de recursos ou um cofre de chaves específico. As funções atribuídas ao nível da subscrição aplicam-se a todos os grupos de recursos e recursos nessa subscrição. As funções atribuídas ao nível do grupo de recursos aplicam-se a todos os recursos nesse grupo de recursos. As funções atribuídas a um recurso específico aplica-se a esse recurso. Existem várias funções predefinidas (veja [RBAC: funções incorporadas](../role-based-access-control/built-in-roles.md)). Se uma função predefinida não atender a suas necessidades, pode definir sua própria função.

> [!IMPORTANT]
> Tenha em conta que, se um utilizador tiver as permissões de Contribuinte (RBAC) para um plano de gestão do cofre de chaves, esse utilizador pode conceder acesso a ele próprio ao plano de dados mediante a definição da política de acesso do cofre de chaves, que controla o acesso ao plano de dados. Por este motivo, é recomendado controlar rigorosamente quem tem acesso de “Contribuinte” aos seus cofres de chaves, para garantir que só pessoas autorizadas podem aceder e gerir os cofres de chaves, as chaves, os segredos e os certificados.
> 
> 

## <a name="data-plane-access-control"></a>Controlo de acesso do plano de dados
Operações de plano de dados do Cofre de chaves se aplica a objetos armazenados como chaves, segredos e certificados. Operações-chave incluem criar, importar, atualizar, listar, fazer cópias de segurança e restaurar chaves. Operações criptográficas incluem o início de sessão, verificarem, encriptarem, desencriptam, moldagem, anular a moldagem, definir etiquetas e outros atributos de chaves. Da mesma forma, as operações no segredos incluem get, o conjunto, listar, eliminar.

O acesso do plano de dados é concedido mediante a definição de políticas de acesso a cofres de chaves. De modo a poder definir políticas de acesso a um cofre de chaves, um utilizador, grupo ou aplicação tem de ter as permissões de Contribuinte (RBAC) para o plano de gestão desse cofre. Pode ser concedido acesso a um utilizador, grupo ou aplicação para fazer operações específicas de chaves ou segredos num cofre de chaves. Os cofres de chaves suportam até 1024 entradas de políticas de acesso. Crie um grupo de segurança do Azure Active Directory e adicione utilizadores ao mesmo para conceder acesso do plano de dados a vários utilizadores a um cofre de chaves.

### <a name="key-vault-access-policies"></a>Políticas de Acesso dos cofres de chaves
Políticas de acesso do Cofre de chaves concedem permissões para chaves, segredos e certificados em separado. Por exemplo, pode permitir que um utilizador acesso só a chaves e não permissões para segredos. Permissões para aceder a chaves, segredos ou certificados são ao nível do cofre. Política de acesso do Cofre de chaves não suporta permissões ao nível do objeto granular, tais como uma chave/segredo/certificado específico. Pode utilizar o [portal do Azure](https://portal.azure.com/), as [ferramentas da CLI do Azure](../cli-install-nodejs.md), o [PowerShell](/powershell/azureps-cmdlets-docs) ou as [APIs REST de Gestão dos cofres de chaves](https://msdn.microsoft.com/library/azure/mt620024.aspx) para definir políticas de acesso para um cofre de chave.

> [!IMPORTANT]
> Tenha em atenção que as políticas de acesso do cofre de chaves são aplicadas ao nível do cofre. Por exemplo, quando é concedida permissão a um utilizador para criar e eliminar chaves, esse utilizador pode fazer essas operações em todas as chaves no cofre de chaves.

Além das políticas de acesso, o acesso ao plano de serviço de dados também pode ser restringido através de [Pontos Finais do Serviço de Rede Virtual para o Azure Key Vault](key-vault-overview-vnet-service-endpoints.md) configurando [Firewalls e regras de rede virtual](key-vault-network-security.md) para um camada adicional de segurança.

## <a name="example"></a>Exemplo
Digamos que estiver desenvolvendo um aplicativo que utiliza um certificado para SSL, o armazenamento do Azure para armazenar dados e uma chave RSA de 2048 bits para operações de início de sessão. Digamos que esta aplicação está em execução na máquina Virtual do Azure (ou um conjunto de dimensionamento de Máquina Virtual). Pode utilizar um cofre de chaves para armazenar todos os segredos da aplicação e armazenar o certificado de arranque de configuração utilizado pela aplicação para autenticar com o Azure Active Directory.

Aqui está um resumo dos tipos de chaves e segredos armazenados num cofre de chaves:

* **Certificado SSL** - utilizado para SSL
* **Chave de armazenamento** - utilizado para obter acesso à conta de armazenamento
* **Chave RSA de 2048 bits** - utilizado para operações de início de sessão
* **Certificado de arranque de configuração** - utilizado para autenticar com o Azure Active Directory. Assim que o acesso é concedido, pode obter a chave de armazenamento e utilizar a chave RSA para a assinatura.

Agora vamos ver quem é gerir, implementar e auditar esta aplicação. Vamos utilizar três funções neste exemplo.

* **Equipe de segurança** -equipe de TI, normalmente, do office cso (Chief Security Officer) ou equivalente. Esta equipe é responsável por proteger adequadamente dos segredos. Por exemplo, certificados SSL, chaves RSA utilizadas para assinaturas, cadeias de ligação e as chaves de conta de armazenamento.
* **Programadores/operadores** -pessoas que desenvolveram a aplicação e, em seguida, implementação-la no Azure. Normalmente, eles não são parte da equipa de segurança, e, portanto, eles não devem ter acesso a dados confidenciais, tais como certificados SSL e chaves RSA. Apenas a aplicação que implementam deve ter acesso a esses objetos.
* **Auditores** -normalmente, um conjunto diferente de pessoas, isoladas de desenvolvedores e equipe de TI geral. Suas responsabilidades é rever a utilização e manutenção dos certificados, chaves e segredos para garantir a conformidade com normas de segurança. 

Há uma outra função que está fora do escopo desta aplicação, mas relevante aqui para ser mencionado. Essa função é a subscrição (ou grupo de recursos) administrador. O administrador da subscrição configura as permissões de acesso inicial para a equipe de segurança. O administrador da subscrição concede acesso à equipa de segurança, utilizar um grupo de recursos que contém os recursos exigidos por esta aplicação.

Agora, vamos ver que ações são realizadas por cada função no contexto da aplicação.

* **Equipa de segurança**
  * Criar cofres de chaves
  * Ativar o registo de cofres de chaves
  * Adicionar chaves/segredos
  * Criar cópias de segurança de chaves para recuperação após desastres
  * Definir a política de acesso do cofre de chaves para conceder permissões a utilizadores e aplicações para fazerem operações específicas
  * Lançar periodicamente chaves/segredos
* **Programadores/operadores**
  * Obter referências bootstrap e SSL (thumbprints) de certificados, chave de armazenamento (URI do segredo) e a chave de assinatura (URI da chave) da equipa de segurança
  * Desenvolver e implementar aplicações que acedam a chaves e segredos programaticamente
* **Auditores**
  * Rever os registos de utilização para confirmar a utilização de chaves/segredos adequada e a conformidade com as normas de segurança de dados

Agora vamos ver quais permissões de acesso são necessárias para cada função e da aplicação, para realizar suas tarefas atribuídas. 

| Função de Utilizador | Permissões do plano de gestão | Permissões do plano de dados |
| --- | --- | --- |
| Equipa de Segurança |Contribuinte do cofre de chaves |Chaves: criar cópia de segurança, criar, eliminar, obter, importar, listar, restaurar <br> Segredos: todas |
| Programadores/operadores |Permissão de implementação de cofre de chaves, para que as VMs por eles implementadas possam obter segredos a partir do cofre de chaves |Nenhuma |
| Auditores |Nenhuma |Chaves: listar<br>Segredos: listar |
| Aplicação |Nenhuma |Chaves: assinar<br>Segredos: obter |

> [!NOTE]
> Os auditores precisam da permissão de listar para chaves e segredos, para que possam inspecionar atributos de chaves e segredos que não são emitidos nos registos, como etiquetas e datas de ativação e expiração.
> 
> 

Além de permissões do Cofre de chaves, todas as três funções também precisam de acesso a outros recursos. Por exemplo, para poderem implementar VMs (ou Aplicações Web, etc.) Os programadores/operadores também precisam do acesso de “Contribuinte” a esses tipos de recursos. Auditores precisam 'acesso de leitura"à conta de armazenamento onde são armazenados os registos do Cofre de chaves.

Uma vez que o foco deste artigo está a proteger o acesso ao seu Cofre de chaves, ilustraremos apenas conceitos âmbito deste tópico. Obter detalhes sobre a implementação de certificados, aceder a chaves e segredos programaticamente e outros, são abordados em outro lugar. Por exemplo:

- Implementação de certificados armazenados no Cofre de chaves para VMs é abordado num [mensagem de blogue: implementar certificados para VMs de Cofre de chaves geridas pelo cliente](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)
- O [baixe exemplos de cliente do Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45343) ilustra como usar um certificado de arranque de configuração, para autenticar para o Azure AD para aceder a um cofre de chaves.

A maioria das permissões de acesso pode ser concedida através do portal do Azure. Para conceder permissões granulares, terá de utilizar o Azure PowerShell ou a CLI para alcançar o resultado desejado. 

Os fragmentos do PowerShell seguintes presumem que:

* O administrador do Azure Active Directory criou grupos de segurança que representam as três funções (os auditores de aplicação equipa de segurança da Contoso, Devops de aplicação da Contoso, do Contoso). O administrador também adicionou utilizadores aos grupos a que pertence.
* **ContosoAppRG** é o grupo de recursos onde residem todos os recursos. **contosologstorage** é onde são armazenados os registos. 
* O cofre de chaves **ContosoKeyVault** e a conta de armazenamento utilizada para os registos do cofre de chaves, **contosologstorage**, têm de estar na mesma localização do Azure

Primeiro, o administrador da subscrição atribui as funções “Contribuinte do cofre de chaves” e “Administrador de acesso de utilizadores” à equipa de segurança. Estas funções permitem que a equipe de segurança gerir o acesso a outros recursos e gerir cofres de chaves num grupo de recursos ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

O script seguinte mostra como a equipe de segurança pode criar um cofre de chaves e configurar permissões de acesso e de registo. Ver [sobre o Azure Key Vault chaves, segredos e certificados](about-keys-secrets-and-certificates.md) para obter detalhes sobre o Key Vault permissões de política de acesso.

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
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

A função personalizada definida só é atribuível à subscrição na qual foi criado o grupo de recursos ContosoAppRG. Se as mesmas funções personalizadas serão utilizadas para outros projetos de outras subscrições, seu escopo poderia ter mais subscrições adicionadas.

A atribuição de função personalizada para a permissão de programadores/operadores "implementar/ação" tem um âmbito para o grupo de recursos. Isso permite que apenas as VMs criadas no grupo de recursos "ContosoAppRG" para ter acesso a segredos (certificado SSL e certificado de arranque do sistema). As VMs criadas no outro grupo de recursos por um membro da equipa de programadores/operadores não terá acesso a estes segredos, mesmo que tenham os respetivos URIs.

Este exemplo mostra um cenário simples. Cenários da vida real poderão ser mais complexos, e poderá ter de ajustar as permissões para o seu Cofre de chaves com base nas suas necessidades. No nosso exemplo, partimos do princípio que a equipe de segurança irá fornecer a chave e referências segredas (URIs e thumbprints), o que os programadores/operadores tem de referenciar nas respetivas aplicações. Programadores/operadores não requerem qualquer acesso do plano de dados. Este exemplo se foca em proteger o seu Cofre de chaves. Considerações semelhantes devem ser dado ao proteger [as suas VMs](https://azure.microsoft.com/services/virtual-machines/security/), [contas de armazenamento](../storage/common/storage-security-guide.md)e outros recursos do Azure.

> [!NOTE]
> Nota: este exemplo mostra como o acesso do cofre de chaves estará bloqueado na produção. Os programadores devem ter a sua própria subscrição ou grupo de recursos nos quais tenham permissões completas para gerir os cofres, as VMs e a conta de armazenamento em que desenvolveram a aplicação.

É altamente recomendável para acesso seguro ao seu Cofre de chaves ainda mais pelo [configurar o Cofre de chaves firewalls e redes virtuais](key-vault-network-security.md).

## <a name="resources"></a>Recursos
* [Controlo de Acesso Baseado em Funções do Azure Active Directory](../role-based-access-control/role-assignments-portal.md)
  
  Este artigo explica o Controlo de Acesso Baseado em Funções do Azure Active Directory e como funciona.
* [RBAC: Funções Incorporadas](../role-based-access-control/built-in-roles.md)
  
  Este artigo mostra todas as funções incorporadas disponíveis no RBAC.
* [Compreender a implementação do Resource Manager e a implementação clássica](../azure-resource-manager/resource-manager-deployment-model.md)
  
  Este artigo explica os modelos de implementação Resource Manager e de implementação clássica e explica as vantagens de utilizar o Resource Manager e os grupos de recursos.
* [Gerir o Controlo de Acesso Baseado em Funções com o Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)
  
  Este artigo explica como gerir o controlo de acesso baseado em funções com o Azure PowerShell
* [Gerir o Controlo de Acesso Baseado em Funções com a API REST](../role-based-access-control/role-assignments-rest.md)
  
  Este artigo mostra como utilizar a API REST para gerir o RBAC.
* [Controlo de Acesso Baseado em Funções para o Microsoft Azure no Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Este 2015 Microsoft Ignite aborda de vídeo de conferência gestão de acesso e capacidades de relatórios no Azure. Também explora as práticas recomendadas para proteger o acesso a subscrições do Azure com o Azure Active Directory.
* [Autorizar o acesso a aplicações Web com OAuth 2.0 e o Azure Active Directory](../active-directory/develop/v1-protocols-oauth-code.md)
  
  Este artigo descreve o fluxo completo de OAuth 2.0 para autenticar com o Azure Active Directory.
* [APIs REST de Gestão do cofre de chaves](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Este documento é a referência para as APIs REST para gerir o seu cofre de chaves programaticamente, incluindo para definir a política de acesso do cofre de chaves.
* [APIs REST do cofre de chaves](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Ligação para a documentação de referência da API REST do Cofre de Chaves
* [Controlo de acesso a chaves](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Ligação para a documentação de referência do Controlo de acesso a segredos.
* [Controlo de acesso a segredos](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Ligação para a documentação de referência do Controlo de acesso a chaves
* [Definir](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) e [Remover](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) a política de acesso do cofre de chaves com o PowerShell
  
  Ligações para documentação de referência sobre cmdlets do PowerShell para gerir a política de acesso do cofre de chaves.

## <a name="next-steps"></a>Próximos Passos
[Configurar firewalls e redes virtuais do Key Vault](key-vault-network-security.md)

Para obter um tutorial introdutório para administradores, veja [Introdução ao Cofre de Chaves do Azure](key-vault-get-started.md).

Para obter mais informações sobre o registo de utilização do cofre de chave, veja [Registo do Cofre de Chaves do Azure](key-vault-logging.md).

Para obter mais informações sobre a utilização de chaves e segredos com o cofre de chave do Azure, veja [About Keys and Secrets (Sobre Chaves e Segredos)](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Se tiver perguntas sobre o Cofre de Chaves, visite os [Fóruns do Cofre de Chaves do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

