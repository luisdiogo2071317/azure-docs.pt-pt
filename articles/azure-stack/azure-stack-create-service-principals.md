---
title: Criar um Principal de serviço para o Azure Stack | Documentos da Microsoft
description: Descreve como criar um novo principal de serviço que pode ser utilizado com o controlo de acesso baseado em funções no Azure Resource Manager para gerir o acesso aos recursos.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2018
ms.author: mabrigg
ms.openlocfilehash: 0db3f19c99b786d7f32f126ad7bd70efc999a751
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444278"
---
# <a name="provide-applications-access-to-azure-stack"></a>Fornecimento de acesso de aplicações ao Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Quando uma aplicação precisa de aceder para implementar ou configurar recursos através do Azure Resource Manager no Azure Stack, criar um serviço principal, que é uma credencial para a sua aplicação.  Em seguida, pode delegar as permissões necessárias para esse principal de serviço.  

Por exemplo, pode ter uma ferramenta de gerenciamento de configuração que utiliza o Azure Resource Manager para Inventariar os recursos do Azure.  Neste cenário, pode criar um principal de serviço, conceder a função de leitor a esse principal de serviço e limitar a ferramenta de gestão de configuração para acesso só de leitura. 

Principais de serviço são preferíveis a executar o aplicativo em suas próprias credenciais porque:

* Pode atribuir permissões ao principal de que são diferentes de suas próprias permissões de conta de serviço. Normalmente, estas permissões estão restritas a exatamente aquilo que a aplicação precisa de fazer.
* Não é necessário que alterar as credenciais da aplicação se alterar as suas responsabilidades.
* Pode utilizar um certificado para automatizar a autenticação ao executar um script automático.  

## <a name="getting-started"></a>Introdução

Dependendo de como tiver implementado o Azure Stack, comece por criar um serviço principal.  Este documento orienta-o ao longo da criação de um principal de serviço para os dois [do Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) e [Active Directory Federation Services(AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).  Depois de criar o principal de serviço, um conjunto de passos comuns para AD FS e o Azure Active Directory está habituado [delegar permissões](azure-stack-create-service-principals.md#assign-role-to-service-principal) à função.     

## <a name="create-service-principal-for-azure-ad"></a>Criar principal de serviço para o Azure AD

Se implementar o Azure Stack com o Azure AD como o repositório de identidades, pode criar principais de serviço, tal como fazer para o Azure.  Esta secção mostra como efetuar os passos no portal.  Verifique se tem o [do Azure AD permissões obrigatórias](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) antes de começar.

### <a name="create-service-principal"></a>Criar um principal de serviço
Nesta secção, vai criar uma aplicação (principal de serviço) no Azure AD que representa a sua aplicação.

1. Inicie sessão na sua conta do Azure através da [portal do Azure](https://portal.azure.com).
2. Selecione **do Azure Active Directory** > **registos das aplicações** > **adicionar**   
3. Indique um nome e um URL para a aplicação. Selecione **aplicação Web / API** ou **nativo** para o tipo de aplicação que pretende criar. Depois de definir os valores, selecione **criar**.

Criou um principal de serviço para a sua aplicação.

### <a name="get-credentials"></a>Obter credenciais
Quando iniciar sessão programaticamente, utilize o ID para a sua aplicação e para uma aplicação Web / API, uma chave de autenticação. Para obter esses valores, utilize os seguintes passos:

1. Partir **registos das aplicações** no Active Directory, selecione a aplicação.

2. Copie o **ID da Aplicação** e armazene-o no código da aplicação. As aplicações na [aplicações de exemplo](#sample-applications) secção Consulte este valor como o ID de cliente.

     ![id de cliente](./media/azure-stack-create-service-principal/image12.png)
3. Para gerar uma chave de autenticação para uma aplicação Web / API, selecione **configurações** > **chaves**. 

4. Indique uma descrição e uma duração para a chave. Quando terminar, selecione **Guardar**.

Depois de guardar a chave, o valor da mesma é apresentado. Copie este valor, porque não vai conseguir obter a chave mais tarde. Forneça o valor da chave com o ID da aplicação para iniciar sessão como o aplicativo. Armazene o valor da chave num local onde a aplicação o possa obter.

![chave guardada](./media/azure-stack-create-service-principal/image15.png)


Depois de concluído, avance para [atribuir uma função de seu aplicativo](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Criar principal de serviço para o AD FS
Se tiver implementado o Azure Stack com o AD FS, pode utilizar o PowerShell para criar um principal de serviço, atribuir uma função de acesso e iniciar sessão a partir do PowerShell usando essa identidade.

O script é executado a partir do ponto final com privilégios numa máquina virtual ERCS.


Requisitos:
- É necessário um certificado.

**Parâmetros**

As seguintes informações são necessárias como entrada para os parâmetros de automatização:


|Parâmetro|Descrição|Exemplo|
|---------|---------|---------|
|Nome|Nome da conta SPN|MyAPP|
|ClientCertificates|Matriz de objetos de certificado|X509 certificado|
|ClientRedirectUris<br>(Opcional)|URI de redirecionamento da aplicação|         |

**Exemplo**

1. Abra uma sessão elevada do Windows PowerShell e execute os seguintes comandos:

   > [!NOTE]
   > Este exemplo cria um certificado autoassinado. Ao executar estes comandos numa implementação de produção, utilize Get-certificado para recuperar o objeto de certificado para o certificado que pretende utilizar.

   ```PowerShell  
    # Credential for accessing the ERCS PrivilegedEndpoint typically domain\cloudadmin
    $creds = Get-Credential

    # Creating a PSSession to the ERCS PrivilegedEndpoint
    $session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $creds

    # This produces a self signed cert for testing purposes.  It is prefered to use a managed certificate for this.
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<yourappname>" -KeySpec KeyExchange

    $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name '<yourappname>' -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }
    $session|remove-pssession

    # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

    # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

    # TenantID for the stamp. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $TenantID = $AzureStackInfo.AADTenantID

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

    # Set the GraphEndpointResourceId value
    Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

    Add-AzureRmAccount -EnvironmentName "azurestackuser" `
    -ServicePrincipal `
    -CertificateThumbprint $ServicePrincipal.Thumbprint `
    -ApplicationId $ServicePrincipal.ClientId `
    -TenantId $TenantID
   ```

2. Depois de concluída a automação, ele exibe os detalhes necessários para utilizar o SPN. 

   Por exemplo:

   ```
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

### <a name="assign-a-role"></a>Atribuir uma função
Depois de criar o Principal de serviço, tem [atribuí-lo a uma função](azure-stack-create-service-principals.md#assign-role-to-service-principal)

### <a name="sign-in-through-powershell"></a>Inicie sessão através do PowerShell
Assim que tiver atribuído uma função, pode iniciar sessão no Azure Stack com o principal de serviço com o seguinte comando:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ClientId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Atribuir a função ao principal de serviço
Para acessar recursos na sua subscrição, tem de atribuir a aplicação a uma função. Decida que função representa as permissões corretas para a aplicação. Para saber mais sobre as funções disponíveis, veja [RBAC: funções incorporadas](../role-based-access-control/built-in-roles.md).

Pode definir o âmbito no nível da subscrição, no grupo de recursos ou ao recurso. As permissões são herdadas para níveis inferiores de âmbito. Por exemplo, adicionar uma aplicação à função do leitor para um grupo de recursos significa pode ler o grupo de recursos e todos os recursos que nele contidos.

1. No portal do Azure Stack, navegue para o nível de âmbito que pretende atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito da subscrição, selecione **subscrições**. Em vez disso, pode selecionar um grupo de recursos ou recurso.

2. Selecione a subscrição específica (grupo de recursos ou recurso) para atribuir a aplicação.

     ![Selecione a subscrição para atribuição](./media/azure-stack-create-service-principal/image16.png)

3. Selecione **controlo de acesso (IAM)**.

     ![Selecione o acesso](./media/azure-stack-create-service-principal/image17.png)

4. Selecione **Adicionar**.

5. Selecione a função que pretende atribuir à aplicação.

6. Procure a sua aplicação e selecioná-lo.

7. Selecione **OK** para concluir a atribuir a função. Pode ver a sua aplicação na lista de utilizadores atribuídos a uma função para esse âmbito.

Agora que já criou um principal de serviço e uma função atribuída, poderá começar a usar isso na sua aplicação para aceder aos recursos do Azure Stack.  

## <a name="next-steps"></a>Passos Seguintes

[Adicionar utilizadores ao AD FS](azure-stack-add-users-adfs.md)
[gerir permissões de utilizador](azure-stack-manage-permissions.md)
