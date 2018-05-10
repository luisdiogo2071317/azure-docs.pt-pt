---
title: Utilize um utilizador de VM do Windows atribuído MSI para aceder ao Gestor de recursos do Azure
description: Um tutorial que explica o processo de utilização de um utilizador atribuído geridos serviço de identidade (MSI) na VM do Windows, para aceder ao Gestor de recursos do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: arluca
ms.openlocfilehash: b81d4b669898a7acc428fe22a070ccd76dc5e546
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-windows-vm-to-access-azure-resource-manager"></a>Utilize um utilizador atribuído geridos serviço de identidade (MSI) na VM do Windows, para aceder ao Gestor de recursos do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial explica como criar um utilizador atribuído a identidade, atribua-o para uma Máquina Virtual (VM do Windows) e, em seguida, utilizar essa identidade para aceder à API do Gestor de recursos do Azure. Identidades de serviço geridas são automaticamente geridas pelo Azure. Ativar a autenticação nos serviços que suportam a autenticação do Azure AD, sem ser necessário incorporar as credenciais para o seu código. 

Saiba como:

> [!div class="checklist"]
> * Criar uma VM do Windows 
> * Criar um utilizador atribuído identidade
> * Atribua o utilizador atribuído a identidade para a VM do Windows
> * Conceder o acesso de identidade do utilizador atribuído a um grupo de recursos no Gestor de recursos do Azure 
> * Obter um token de acesso utilizando a identidade de atribuída ao utilizador e utilizá-la para chamar o Azure Resource Manager 
> * Ler as propriedades de um grupo de recursos

## <a name="prerequisites"></a>Pré-requisitos

- Se o tem familiarizado com a identidade de serviço geridas, consulte o [descrição geral](overview.md) secção. **Certifique-se rever o [diferenças entre o sistema e de utilizador atribuída identidades](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar a criação de recursos necessários e passos de gestão da função neste tutorial, a sua conta tem permissões de "Proprietário" no âmbito adequado (sua subscrição ou grupo de recursos). Se necessitar de assistência com a atribuição de função, consulte o artigo [Use Role-Based o controlo de acesso para gerir o acesso aos recursos da sua subscrição do Azure](/azure/role-based-access-control/role-assignments-portal).

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 5.7 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-resource-group"></a>Criar grupo de recursos

No exemplo seguinte, um grupo de recursos com o nome *myResourceGroupVM* é criado no *EastUS* região.

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

## <a name="create-a-user-assigned-identity"></a>Criar um utilizador atribuído identidade

Um utilizador atribuído identidade é criado como uma autónoma recursos do Azure. Utilizar o [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity), o Azure cria uma identidade no seu inquilino do Azure AD que pode ser atribuído a uma ou mais instâncias de serviço do Azure.

> [!IMPORTANT]
> Criar identidades de utilizador atribuída só suporta alfanuméricos e hífenes (0-9 ou a-z ou A-Z ou -) carateres. Além disso, o nome deve ser limitado a 24 carateres para a atribuição a VM/VMSS funcione corretamente. Verifique novamente para as atualizações. Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

A resposta contém detalhes para a identidade criada, semelhante ao seguinte exemplo de atribuída ao utilizador. Tenha em atenção o `Id` valor para a sua identidade de utilizador atribuída, tal como será utilizado no próximo passo:

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

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Atribua o utilizador atribuído a identidade para uma VM do Windows

Um utilizador atribuído identidade pode ser utilizado por clientes em vários recursos do Azure. Utilize os seguintes comandos para atribuir a identidade do utilizador atribuído a uma única VM. Utilize o `Id` propriedade devolvida no passo anterior para o `-IdentityID` parâmetro.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Conceder que ao utilizador atribuído acesso do MSI num grupo de recursos no Gestor de recursos do Azure 

Identidade de serviço geridas (MSI) fornece identidades, que pode utilizar o código para pedir tokens de acesso para autenticar para o recurso APIs que a autenticação de suporte do Azure AD. Neste tutorial, o código irá aceder a API do Azure Resource Manager. 

Antes do seu código pode aceder à API do, tem de conceder o acesso de identidade para um recurso no Gestor de recursos do Azure. Neste caso, o grupo de recursos no qual a VM está contida. Atualize o valor para `<SUBSCRIPTION ID>` conforme adequado para o seu ambiente.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

A resposta contém detalhes para a atribuição de função criada, semelhante ao seguinte exemplo:

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

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Obter um token de acesso através da identidade da VM e utilizá-la para chamar o Gestor de recursos 

Para o resto do tutorial, irá trabalhar da VM que criou anteriormente.

1. Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com)

2. No portal, navegue para **máquinas virtuais** e vá para a máquina virtual do Windows e no **descrição geral**, clique em **Connect**.

3. Introduza o **Username** e **palavra-passe** que utilizou quando criou a VM do Windows.

4. Agora que já criou um **ligação ao ambiente de trabalho remoto** com a máquina virtual, abra **PowerShell** na sessão remota.

5. Através do PowerShell `Invoke-WebRequest`, efetue um pedido para o ponto final local de MSI para obter acesso token para o Azure Resource Manager.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=73444643-8088-4d70-9532-c3a0fdc190fz&resource=https://management.azure.com' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Ler as propriedades de um grupo de recursos

Utilize o token de acesso obtido no passo anterior para aceder ao Azure Resource Manager e ler as propriedades do recurso do grupo concedidos o acesso de identidade do utilizador atribuído. Substitua <SUBSCRIPTION ID> com o id de subscrição do seu ambiente.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
A resposta contém as informações de grupo de recursos específicas, tal como no exemplo seguinte:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/TestRG","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do MSI, consulte [descrição geral de identidade de serviço geridas](overview.md).
