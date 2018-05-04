---
title: Registo do Azure para a pilha do Azure integrado sistemas | Microsoft Docs
description: Descreve o processo de registo do Azure implementações de vários nós ligado do Azure de pilha do Azure.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: jeffgilb
ms.reviewer: avishwan
ms.openlocfilehash: 0d8c1ebe9688f32c460ef689119313b0682a0a68
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="register-azure-stack-with-azure"></a>Registar a pilha do Azure com o Azure
Registar [Azure pilha](azure-stack-poc.md) com o Azure permite-lhe para transferir os itens do marketplace a partir do Azure e configurar dados de comércio relatórios de volta à Microsoft. Depois de registar pilha do Azure, utilização é considerada comércio do Azure e pode vê-lo sob a subscrição utilizada para o registo. 

> [!IMPORTANT]
> O registo é obrigatório se escolher o modelo de faturação pay-como-utiliza. Caso contrário, será violar os termos de licenciamento da implementação da pilha de Azure conforme utilização não será reportada.

## <a name="prerequisites"></a>Pré-requisitos
Antes de registar a pilha do Azure com o Azure, tem de ter:

- O ID de subscrição de uma subscrição do Azure. Para obter o ID, inicie sessão no Azure, clique em **mais serviços** > **subscrições**, clique na subscrição que pretende utilizar, e, em **Essentials** pode encontrar o ID da subscrição. 

  > [!NOTE]
  > Alemanha e subscrições de nuvem do US Government não são atualmente suportadas.

- O nome de utilizador e palavra-passe para uma conta que é proprietário da subscrição (são suportadas contas MSA/2FA).
- Registar o fornecedor de recursos de pilha do Azure (consulte a secção de registar o fornecedor de recursos do Azure pilha abaixo para obter detalhes).

Se não tiver uma subscrição do Azure que cumpra estes requisitos, pode [criar uma conta do Azure gratuita aqui](https://azure.microsoft.com/free/?b=17.06). Registar o Azure pilha incorreu sem qualquer custo na sua subscrição do Azure.

### <a name="bkmk_powershell"></a>Instale o PowerShell para a pilha do Azure
Tem de utilizar o PowerShell mais recente do Azure pilha para registar com o Azure.

Se ainda não estiver instalado, [instale o PowerShell para Azure pilha](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install). 

### <a name="bkmk_tools"></a>Transferir as ferramentas de pilha do Azure
O repositório do GitHub ferramentas Azure pilha contém módulos do PowerShell que suportem a funcionalidade de pilha do Azure; incluindo a funcionalidade de registo. Durante o processo de registo, tem de importar e utilizar o módulo RegisterWithAzure.psm1 PowerShell, foi encontrada no repositório de ferramentas pilha do Azure, para registar a instância de pilha do Azure com o Azure. 

Para garantir que está a utilizar a versão mais recente, deve eliminar quaisquer versões das ferramentas de pilha do Azure existentes e [transferir a versão mais recente a partir do GitHub](azure-stack-powershell-download.md) antes de registar com o Azure.

## <a name="register-azure-stack-in-connected-environments"></a>Registar o Azure pilha em ambientes ligados
Podem aceder a ambientes ligados à internet e ao Azure. Para estes ambientes, terá de registar o fornecedor de recursos de pilha do Azure com o Azure e, em seguida, configurar o modelo de faturação.

> [!NOTE]
> Todos estes passos tem de ser executados a partir de um computador que tenha acesso ao ponto final com privilégios. 

### <a name="register-the-azure-stack-resource-provider"></a>Registar o fornecedor de recursos de pilha do Azure
Para registar o fornecedor de recursos de pilha do Azure com o Azure, inicie o ISE do PowerShell como administrador e utilize os seguintes comandos do PowerShell com o **EnvironmentName** parâmetro definido para o tipo de subscrição do Azure adequada (consulte parâmetros abaixo). 

1. Adicione a conta do Azure que utiliza para registar a pilha do Azure. Para adicionar a conta, execute o **Add-AzureRmAccount** cmdlet. É-lhe pedido que introduza as suas credenciais de conta de administrador global do Azure e poderá ter de utilizar a autenticação de fator 2, com base na configuração da sua conta.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parâmetro | Descrição |  
   |-----|-----|
   | EnvironmentName | O nome de ambiente de subscrição de nuvem do Azure. Os nomes de ambiente suportados são **AzureCloud** ou, se utilizar uma subscrição do Azure China, **AzureChinaCloud**.  |
   |  |  |

2. Se tiver várias subscrições, execute o seguinte comando para selecionar aquela que pretende utilizar:  

   ```PowerShell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Execute o seguinte comando para registar o fornecedor de recursos de pilha do Azure na sua subscrição do Azure:

   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Registar a pilha do Azure com o Azure utilizando o modelo de faturação pay-como-utiliza
Utilize estes passos para registar a pilha do Azure com o Azure utilizando o modelo de faturação pay-como-utiliza.

1. Inicie o ISE do PowerShell como administrador e navegue para o **registo** pasta no **mestre de ferramentas AzureStack** directory criados quando [transferido as ferramentas de pilha do Azure](#bkmk_tools). Importar o **RegisterWithAzure.psm1** módulo com o PowerShell: 

  ```powershell
  Import-Module .\RegisterWithAzure.psm1
  ```

2. Em seguida, na mesma sessão do PowerShell, certifique-se de que tem sessão iniciada contexto do PowerShell correto do Azure. Esta é a conta do azure que foi utilizada para registar o fornecedor de recursos do Azure pilha acima. PowerShell para executar: 

  ```powershell 
  Add-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>" 
  ``` 

3. Na mesma sessão do PowerShell, execute o **conjunto AzsRegistration** cmdlet. PowerShell para executar:  

  ```powershell
  $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse
  ```

  |Parâmetro|Descrição|
  |-----|-----|
  |PrivilegedEndpointCredential|As credenciais utilizadas para [aceder ao ponto final com privilégios](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). O nome de utilizador está no formato **AzureStackDomain\CloudAdmin**.|
  |PrivilegedEndpoint|Uma previamente configurada PowerShell consola remota que fornece capacidades, como a recolha de registos e outra post tarefas de implementação. Para obter mais informações, consulte o [utilizando o ponto final com privilégios](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artigo.|
  |BillingModel|O modelo de faturação que utiliza a sua subscrição. Os valores para este parâmetro permitidos são: capacidade, PayAsYouUse e desenvolvimento.|
  |  |  |

  O processo irá demorar entre 10 e 15 minutos. Quando o comando for concluído, verá a mensagem **"ambiente está agora registado e ativado utilizando os parâmetros fornecidos."**

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Registar a pilha do Azure com o Azure utilizando o modelo de faturação de capacidade
Siga as mesmas instruções utilizadas para registar utilizando o modelo de faturação pay-como-utiliza, mas a adicionar o número do contrato com a qual tenha sido comprada capacidade e a alterar o **BillingModel** parâmetro **capacidade**. Todos os outros parâmetros são iguais.

PowerShell para executar:

```powershell
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Registar o Azure pilha em ambientes desligados 
*As informações nesta secção aplicam-se de que começa com a versão de atualização de 1712 de pilha do Azure (180106.1) e não são suportadas com versões anteriores.*

Se estiver a registar pilha do Azure num ambiente desligado (com sem conectividade internet), tem de obter um registo de token do ambiente de pilha do Azure e, em seguida, utilizar esse token num computador que pode ligar ao Azure e tem [PowerShell para a pilha de Azure instalado](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Obter um registo de token do ambiente de pilha do Azure

1. Inicie o ISE do PowerShell como administrador e navegue para o **registo** pasta no **mestre de ferramentas AzureStack** directory criados quando [transferido as ferramentas de pilha do Azure](#bkmk_tools). Importar o **RegisterWithAzure.psm1** módulo:  

  ```powershell 
  Import-Module .\RegisterWithAzure.psm1 
  ``` 

2. Para obter o token de registo, execute os seguintes comandos do PowerShell:  

  ```Powershell
  $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
  $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
  ```
  
  > [!TIP]  
  > O token de registo é guardado no ficheiro especificado para *$FilePathForRegistrationToken*. Pode alterar filepath ou nome de ficheiro no seu critério. 

3. Guarde este token de registo para utilização do Azure ligado máquina. Pode copiar o ficheiro ou o texto de $FilePathForRegistrationToken.


### <a name="connect-to-azure-and-register"></a>Ligar ao Azure e registar
No computador que é ligadas à internet, execute os mesmos passos para importar o módulo de RegisterWithAzure.psm1 e início de sessão para o contexto do Azure Powershell correto. Em seguida, chame Register-AzsEnvironment e especifique o token de registo para registar com o Azure:

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
  > Guarde o nome do registo de recursos e o token de registo para consulta futura.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Obter uma chave de ativação de recursos de registo do Azure 
Em seguida, terá de obter uma chave de ativação a partir de recursos de registo criado no Azure durante o registo AzsEnvironment. 
 
Para obter a chave de ativação, execute os seguintes comandos do PowerShell:  

  ```Powershell 
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>" 
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt" 
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath 
  ``` 
  > [!TIP]   
  > A chave de ativação é guardada no ficheiro especificado para *$KeyOutputFilePath*. Pode alterar filepath ou nome de ficheiro no seu critério. 

### <a name="create-an-activation-resource-in-azure-stack"></a>Criar um recurso de ativação na pilha do Azure 
Devolver para o ambiente de pilha do Azure com o ficheiro ou o texto a partir de criada a partir de Get-AzsActivationKey a chave de ativação. Em seguida, irá criar um recurso de ativação na pilha do Azure utilizando essa chave de ativação. Para criar um recurso de ativação, execute os seguintes comandos do PowerShell:  

  ```Powershell 
  $ActivationKey = "<activation key>" 
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey 
  ``` 

Opcionalmente, pode utilizar o cmdlet Get-Content para apontar para um ficheiro que contém o token de registo: 

  ```Powershell   
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>' 
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey 
  ``` 

## <a name="verify-azure-stack-registration"></a>Verifique o registo de pilha do Azure
Utilize estes passos para verificar que a pilha de Azure registou com êxito com o Azure.
1. A iniciar sessão com a pilha do Azure [portal de administrador](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;região >. &lt;fqdn >*.
2. Clique em **mais serviços** > **Marketplace gestão** > **adicionar a partir do Azure**.

Se vir uma lista de itens disponíveis a partir do Azure (por exemplo, WordPress), a ativação foi efetuada com êxito. No entanto, em ambientes desligados, não verá itens do marketplace do Azure no marketplace pilha do Azure.

> [!NOTE]
> Após a conclusão do registo, o aviso de Active Directory para registar não deixará de ser apresentada.

## <a name="renew-or-change-registration"></a>Renovar ou alterar o registo
### <a name="renew-or-change-registration-in-connected-environments"></a>Renovar ou alterar o registo em ambientes ligados
Terá de atualizar ou renovar o seu registo nas seguintes circunstâncias:
- Depois de renovar a sua subscrição anual com base na capacidade.
- Quando altera o modelo de faturação.
- Quando dimensionar as alterações (Adicionar/remover nós) para faturação baseada na capacidade.

#### <a name="change-the-subscription-you-use"></a>Alterar a subscrição que utilizar
Se gostaria de alterar a subscrição utilizar, tem de executar primeiro o **remover AzsRegistration** cmdlet, certifique-se de que tem sessão iniciada contexto do Azure PowerShell correto e, por fim, execute **AzsRegistration conjunto**  com qualquer alterado parâmetros:

  ```powershell
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Alterar as funcionalidades de modelo ou de sindicação faturação
Se gostaria de alterar o modelo de faturação ou funcionalidades de sindicação para a instalação, pode chamar a função de registo para definir os novos valores. Não terá primeiro de remover o registo atual: 

  ```powershell
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Renovar ou alterar o registo em ambientes desligados 
Terá de atualizar ou renovar o seu registo nas seguintes circunstâncias: 
- Depois de renovar a sua subscrição anual com base na capacidade. 
- Quando altera o modelo de faturação. 
- Quando dimensionar as alterações (Adicionar/remover nós) para faturação baseada na capacidade. 

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Remova o recurso de ativação de pilha do Azure 
Primeiro terá de remover o recurso de ativação da pilha do Azure e, em seguida, o recurso de registo no Azure.  

Para remover o recurso de ativação na pilha do Azure, execute os seguintes comandos do PowerShell no seu ambiente de pilha do Azure:  

  ```Powershell 
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint 
  ``` 

Em seguida, para remover o recurso de registo no Azure, certifique-se num Azure ligado computador, inicie sessão no contexto correto do Azure PowerShell e execute os comandos do PowerShell apropriados, conforme descrito abaixo.

Pode utilizar o token de registo utilizado para criar o recurso:  

  ```Powershell 
  $registrationToken = "<registration token>" 
  Unregister-AzsEnvironment -RegistrationToken $registrationToken 
  ``` 
  
Ou pode utilizar o nome do registo: 

  ```Powershell 
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>" 
  Unregister-AzsEnvironment -RegistrationName $registrationName 
  ``` 

### <a name="re-register-using-disconnected-steps"></a>Volte a registar utilizando passos desligados 
Tem agora totalmente registo anulado com êxito num cenário desligado e deve repetir os passos para registar um ambiente de pilha do Azure num cenário desligado. 

## <a name="next-steps"></a>Passos Seguintes

[Transferir itens do marketplace a partir do Azure](azure-stack-download-azure-marketplace-item.md)
