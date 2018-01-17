---
title: Registo do Azure para a pilha do Azure integrado sistemas | Microsoft Docs
description: "Descreve o processo de registo do Azure implementações de vários nós ligado do Azure de pilha do Azure."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 03a6ce77eed16cbc5d2fe46094b0b6ac7fbc022e
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="register-azure-stack-with-azure"></a>Registar a pilha do Azure com o Azure
Pode registar pilha do Azure com o Azure para transferir os itens do marketplace a partir do Azure e configurar dados de comércio relatórios de volta à Microsoft. Depois de registar pilha do Azure, a utilização é comunicada para comércio do Azure. Pode vê-lo sob a subscrição utilizada para o registo.

> [!IMPORTANT]
> O registo é obrigatório se escolher o modelo de faturação pay-como-utiliza. Caso contrário, será violar os termos de licenciamento da implementação da pilha de Azure conforme utilização caso contrário, não será reportada.

## <a name="before-you-register-azure-stack-with-azure"></a>Antes de registar a pilha do Azure com o Azure
Antes de registar a pilha do Azure com o Azure, tem de ter:

- O ID de subscrição de uma subscrição do Azure. Para obter o ID, inicie sessão no Azure, clique em **mais serviços** > **subscrições**, clique na subscrição que pretende utilizar, e, em **Essentials** pode encontrar o ID da subscrição. 

  > [!NOTE]
  > China, Datacenters e subscrições de nuvem do Governo dos EUA não são atualmente suportados. 

- O nome de utilizador e palavra-passe para uma conta que é proprietário da subscrição (são suportadas contas MSA/2FA)
- *Não é necessário a partir da versão de atualização de 1712 de pilha do Azure (180106.1)*: O Azure AD para a subscrição do Azure. Pode encontrar este diretório no Azure ao passar por cima do seu avatar no canto superior direito do portal do Azure. 
- Registar o fornecedor de recursos de pilha do Azure (consulte a secção de registar o fornecedor de recursos do Azure pilha abaixo para obter detalhes)

Se não tiver uma subscrição do Azure que cumpra estes requisitos, pode [criar uma conta do Azure gratuita aqui](https://azure.microsoft.com/free/?b=17.06). Registar o Azure pilha incorreu sem qualquer custo na sua subscrição do Azure.

### <a name="bkmk_powershell"></a>Instale o PowerShell para a pilha do Azure
Tem de utilizar o PowerShell mais recente do Azure pilha para registar o sistema com o Azure.

Se ainda não estiver instalado, [instale o PowerShell para Azure pilha](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install). 

### <a name="bkmk_tools"></a>Transferir as ferramentas de pilha do Azure
O repositório do GitHub ferramentas Azure pilha contém módulos do PowerShell que suportem a funcionalidade de pilha do Azure; incluindo a funcionalidade de registo. Durante o registo do processo, terá de importar e utilizar o módulo do RegisterWithAzure.psm1 PowerShell encontrados no repositório de ferramentas de pilha do Azure, para registar a instância de pilha do Azure com o Azure. 

```powershell
# Change directory to the root directory. 
cd \

# Download the tools archive.
  invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
  expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
  cd AzureStack-Tools-master
```

## <a name="register-azure-stack-in-connected-environments"></a>Registar o Azure pilha em ambientes ligados
Podem aceder a ambientes ligados à internet e ao Azure. Para estes ambientes, terá de registar o fornecedor de recursos de pilha do Azure com o Azure e, em seguida, configurar o modelo de faturação.

### <a name="register-the-azure-stack-resource-provider"></a>Registar o fornecedor de recursos de pilha do Azure
Para registar o fornecedor de recursos de pilha do Azure com o Azure, inicie o ISE do Powershell como administrador e utilize os seguintes comandos do PowerShell. Estes comandos serão:
- Solicitar-lhe para iniciar sessão como um proprietário da subscrição do Azure a ser utilizado e definir o `EnvironmentName` parâmetro **AzureCloud**.
- Registar o fornecedor de recursos do Azure **Microsoft.AzureStack**.

PowerShell para executar:

```powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack 
```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Registar a pilha do Azure com o Azure utilizando o modelo de faturação pay-como-utiliza
Utilize a estes passos para registar a pilha do Azure com o Azure utilizando o modelo de faturação pay-como-utiliza.

Inicie o ISE do PowerShell como administrador e navegue para o **registo** pasta no **mestre de ferramentas AzureStack** directory criados quando [transferido as ferramentas de pilha do Azure](#bkmk_tools). Importar o **RegisterWithAzure.psm1** módulo com o PowerShell: 

PowerShell para executar:

```powershell
Import-Module .\RegisterWithAzure.psm1
```
Em seguida, na mesma sessão do PowerShell, execute o **conjunto AzsRegistration** cmdlet. Quando lhe forem pedidas as credenciais, especifique o proprietário da subscrição do Azure.  

PowerShell para executar:

```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Azure subscription owner>  -Message "Enter the cloud domain credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -BillingModel PayAsYouUse
```

|Parâmetro|Descrição|
|-----|-----|
|CloudAdminCredential|Objeto do PowerShell que contém informações de credenciais (nome de utilizador e palavra-passe) para o proprietário da subscrição do Azure.|
|PrivilegedEndpoint|Uma previamente configurada PowerShell consola remota que fornece capacidades, como a recolha de registos e outra post tarefas de implementação. Para obter mais informações, consulte o [utilizando o ponto final com privilégios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint#access-the-privileged-endpoint) artigo.|
|BillingModel|O modelo de faturação que utiliza a sua subscrição. Os valores para este parâmetro permitidos são: capacidade, PayAsYouUse e desenvolvimento.|

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Registar a pilha do Azure com o Azure utilizando o modelo de faturação de capacidade
Siga as mesmas instruções utilizadas para registar utilizando o modelo de faturação pay-como-utiliza, mas a adicionar o número do contrato com a qual tenha sido comprada capacidade e a alterar o `BillingModel` parâmetro **capacidade**. Todos os outros parâmetros são iguais.

PowerShell para executar:
```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Azure subscription owner>  -Message "Enter the cloud domain credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Registar o Azure pilha em ambientes desligados 
*As informações nesta secção aplicam-se de que começa com a versão de atualização de 1712 de pilha do Azure (180106.1) e não são suportadas com versões anteriores.*

Se estiver a registar pilha do Azure num ambiente desligado (com sem conectividade internet), tem de obter um registo de token do ambiente de pilha do Azure e, em seguida, utilizar esse token num computador que pode ligar ao Azure e tem [PowerShell para a pilha de Azure instalado](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Obter um registo de token do ambiente de pilha do Azure
  1. Para obter o token de registo, execute os seguintes comandos do PowerShell:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > O token de registo é guardado no ficheiro especificado para *$env:SystemDrive\RegistrationToken.txt*.

  2. Guarde este token de registo para utilização do Azure ligado máquina.


### <a name="connect-to-azure-and-register"></a>Ligar ao Azure e registar
Inicie o ISE do PowerShell como administrador e navegue para o **registo** pasta no **mestre de ferramentas AzureStack** directory criados quando [transferido as ferramentas de pilha do Azure](#bkmk_tools). Importar o **RegisterWithAzure.psm1** módulo: 

PowerShell para executar:
```powershell
Import-Module .\RegisterWithAzure.psm1
```
Em seguida, na mesma sessão do PowerShell, especifique o token de registo para registar com o Azure:

```Powershell  
$registrationToken = "<Your Registration Token>"
Register-AzsEnvironment -RegistrationToken $registrationToken  
```
Opcionalmente, pode utilizar o cmdlet Get-Content para apontar para um ficheiro que contém o token de registo:

 ```Powershell  
 $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
 Register-AzsEnvironment -RegistrationToken $registrationToken  
 ```
> [!NOTE]  
> Guarde o nome do registo de recursos ou o token de registo para consulta futura.

## <a name="verify-azure-stack-registration"></a>Verifique o registo de pilha do Azure
Utilize estes passos para verificar que a pilha de Azure registou com êxito com o Azure.
1. A iniciar sessão com a pilha do Azure [portal de administrador](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https &#58; / / adminportal. *&lt;região >. &lt;fqdn >*.
2. Clique em **mais serviços** > **Marketplace gestão** > **adicionar a partir do Azure**.

Se vir uma lista de itens disponíveis a partir do Azure (por exemplo, WordPress), a ativação foi efetuada com êxito.

> [!NOTE]
> Após a conclusão do registo, o aviso de Active Directory para registar não deixará de ser apresentada.

## <a name="renew-or-change-registration"></a>Renovar ou alterar o registo
Terá de atualizar ou renovar o seu registo nas seguintes circunstâncias:
- Depois de renovar a sua subscrição anual com base na capacidade.
- Quando altera o modelo de faturação.
- Quando dimensionar as alterações (Adicionar/remover nós) para faturação baseada na capacidade.

### <a name="change-the-subscription-you-use"></a>Alterar a subscrição que utilizar
Se gostaria de alterar a subscrição utilizar, tem de executar primeiro o **remover AzsRegistration** cmdlet, certifique-se de que tem sessão iniciada contexto do Azure PowerShell correto e, por fim, execute **AzsRegistration conjunto**  com qualquer alterado parâmetros:

```powershell
Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
Set-AzureRmContext -SubscriptionId $NewSubscriptionId
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```

### <a name="change-the-billing-model-or-syndication-features"></a>Alterar as funcionalidades de modelo ou de sindicação faturação
Se gostaria de alterar o modelo de faturação ou funcionalidades de sindicação para a instalação, pode chamar a função de registo para definir os novos valores. Não terá primeiro de remover o registo atual: 

```powershell
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```

## <a name="remove-a-registered-resource"></a>Remover um recurso registado
Se quiser remover um registo, em seguida, tem de utilizar **UnRegister-AzsEnvironment** cmdlet e passar o nome do registo de recursos ou o registo do token é utilizado para **Register-AzsEnvironment**.

Para remover um registo com um nome de recurso:

```Powershell    
UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
```
Para remover um registo com um token de registo:

```Powershell
$registrationToken = "*Your copied registration token*"
UnRegister-AzsEnvironment -RegistrationToken $registrationToken
```

