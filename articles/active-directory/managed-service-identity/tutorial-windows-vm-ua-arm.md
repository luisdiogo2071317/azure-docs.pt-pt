---
title: Utilizar uma MSI atribuída pelo utilizador de VM do Windows para aceder ao Azure Resource Manager
description: Um tutorial que explica o processo de utilização de uma Identidade de Serviço Gerida (MSI) Atribuída pelo Utilizador numa VM do Windows, para aceder ao Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: daveba
ms.openlocfilehash: 67bb45f7bd27a142b978bedb48925cc41e8d1287
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37904378"
---
# <a name="tutorial-use-a-user-assigned-managed-service-identity-msi-on-a-windows-vm-to-access-azure-resource-manager"></a>Tutorial: Utilizar uma Identidade de Serviço Gerida (MSI) Atribuída pelo Utilizador numa VM do Windows, para aceder ao Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial explica como criar uma identidade atribuída pelo utilizador, atribuí-la a uma Máquina Virtual (VM) do Windows e, em seguida, utilizar essa identidade para aceder à API do Azure Resource Manager. As Identidades de Serviço Geridas são geridas automaticamente pelo Azure. Permitem a autenticação em serviços que suportam a autenticação do Azure AD, sem ter de incorporar as credenciais no código. 

Saiba como:

> [!div class="checklist"]
> * Criar uma VM do Windows 
> * Criar uma identidade atribuída pelo utilizador
> * Atribuir a identidade atribuída pelo utilizador à sua VM do Windows
> * Conceder o acesso de identidade atribuída pelo utilizador a um Grupo de Recursos no Azure Resource Manager 
> * Obter um token de acesso com a identidade atribuída pelo utilizador e utilizá-lo para chamar o Azure Resource Manager 
> * Ler as propriedades de um Grupo de Recursos

## <a name="prerequisites"></a>Pré-requisitos

- Se está a familiarizado com a Identidade de Serviço Gerida, consulte a secção [Descrição Geral](overview.md). **Certifique-se de que revê as [diferenças entre as identidades atribuídas pelo sistema e pelo utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para executar os passos necessários de criação de recursos e gestão de funções neste tutorial, a sua conta precisa de permissões de "Proprietário" no âmbito adequado (a sua subscrição ou grupo de recursos). Se precisar de assistência com a atribuição de funções, veja [Utilizar o Controlo de Acesso Baseado em Funções para gerir o acesso aos recursos de subscrição do Azure](/azure/role-based-access-control/role-assignments-portal).

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 5.7 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-resource-group"></a>Criar grupo de recursos

No exemplo seguinte, é criado um grupo de recursos designado *myResourceGroupVM* na região *EastUS*.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

## <a name="create-virtual-machine"></a>Criar a máquina virtual

Depois de criar o grupo de recursos, crie uma VM do Windows.

Defina o nome de utilizador e a palavra-passe necessários para a conta de administrador na máquina virtual com [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```
Criar a máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="create-a-user-assigned-identity"></a>Criar uma identidade atribuída pelo utilizador

A identidades atribuídas pelo utilizador são criadas como um recurso do Azure autónomo. Através de [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity), o Azure cria uma identidade no seu inquilino do Azure AD que pode ser atribuído a uma ou mais instâncias do serviço do Azure.

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

A resposta contém detalhes para a identidade atribuída pelo utilizador criada, semelhante ao seguinte exemplo. Anote o valor `Id` da identidade atribuída pelo utilizador, uma vez que vai ser utilizada no próximo passo:

```azurepowershell
{
Id: /subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1
ResourceGroupName : myResourceGroupVM
Name: ID1
Location: westus
TenantId: 733a8f0e-ec41-4e69-8ad8-971fc4b533f8
PrincipalId: e591178e-b785-43c8-95d2-1397559b2fb9
ClientId: af825a31-b0e0-471f-baea-96de555632f9
ClientSecretUrl: https://control-westus.identity.azure.net/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1/credentials?tid=733a8f0e-ec41-4e69-8ad8-971fc4b533f8&oid=e591178e-b785-43c8-95d2-1397559b2fb9&aid=af825a31-b0e0-471f-baea-96de555632f9
Type: Microsoft.ManagedIdentity/userAssignedIdentities
}
```

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Atribuir a identidade atribuída pelo utilizador a uma VM do Windows

Uma identidade atribuída pelo utilizador pode ser utilizada pelos clientes em vários recursos do Azure. Utilize os seguintes comandos para atribuir a identidade atribuída pelo utilizador a uma única VM. Utilize a propriedade `Id` devolvida no passo anterior para o parâmetro `-IdentityID`.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Conceder o acesso da MSI atribuída pelo utilizador a um Grupo de Recursos no Azure Resource Manager 

A Identidade de Serviço Gerida (MSI) fornece identidades que podem ser utilizadas pelo seu código para pedir os tokens de acesso para autenticar para APIs de recurso que suportam a autenticação do Azure AD. Neste tutorial, o seu código irá aceder à API do Azure Resource Manager. 

Antes de o seu código pode aceder à API, tem de conceder o acesso de identidade a um recurso no Azure Resource Manager. Neste caso, o Grupo de Recursos no qual a VM está contida. Atualize o valor de `<SUBSCRIPTION ID>`, conforme adequado para o seu ambiente.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

A resposta contém detalhes da atribuição de função criada, semelhante ao seguinte exemplo:

```azurepowershell
RoleAssignmentId: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM/providers/Microsoft.Authorization/roleAssignments/f9cc753d-265e-4434-ae19-0c3e2ead62ac
Scope: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM
DisplayName: ID1
SignInName:
RoleDefinitionName: Reader
RoleDefinitionId: acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId: e591178e-b785-43c8-95d2-1397559b2fb9
ObjectType: ServicePrincipal
CanDelegate: False
```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Obter um token de acesso com a identidade da VM e utilizá-lo para chamar o Resource Manager 

No resto do tutorial, vai trabalhar a partir da VM que criámos anteriormente.

1. Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com)

2. No portal, navegue para **Máquinas Virtuais**, aceda à sua máquina virtual do Windows e, em **Descrição Geral**, clique em **Ligar**.

3. Introduza o **Nome de Utilizador** e a **Palavra-passe** que utilizou quando criou a VM do Windows.

4. Agora que já criou uma **Ligação ao Ambiente de Trabalho Remoto** com a máquina virtual, abra o **PowerShell** na sessão remota.

5. Através de `Invoke-WebRequest` do PowerShell, faça um pedido ao ponto final de MSI local para obter um token de acesso para o Azure Resource Manager.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=73444643-8088-4d70-9532-c3a0fdc190fz&resource=https://management.azure.com' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Ler as propriedades de um Grupo de Recursos

Utilize o token de acesso obtido no passo anterior para aceder ao Azure Resource Manager e ler as propriedades do Grupo de Recursos ao qual concedeu o acesso de identidade atribuída pelo utilizador. Substitua <SUBSCRIPTION ID> pelo ID da subscrição do seu ambiente.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
A resposta contém as informações específicas do Grupo de Recursos, semelhantes ao seguinte exemplo:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/TestRG","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar uma identidade atribuída pelo utilizador e a anexá-la a uma Máquina Virtual do Azure para aceder à API do Azure Resource Manager.  Para saber mais sobre o Azure Resource Manager, veja:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)