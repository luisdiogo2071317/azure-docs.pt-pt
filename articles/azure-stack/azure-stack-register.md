---
title: Registar a pilha do Azure | Microsoft Docs
description: "Registe pilha do Azure com a sua subscrição do Azure."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/8/2018
ms.author: jeffgilb
ms.openlocfilehash: b58b3fc538d2237c12a860d268d550c4223155ba
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Registar a pilha do Azure com a sua subscrição do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode registar [Azure pilha](azure-stack-poc.md) com o Azure para transferir os itens do marketplace a partir do Azure e configurar dados de comércio relatórios de volta à Microsoft.

> [!NOTE]
>Registo é recomendado porque permite-lhe testar importante funcionalidade da pilha do Azure, como os relatórios de utilização e de sindicação do marketplace. Depois de registar pilha do Azure, a utilização é comunicada para comércio do Azure. Pode vê-lo sob a subscrição utilizada para o registo. Utilizadores do Azure do Kit de desenvolvimento de pilha não são-lhe cobrados de qualquer utilização que possam comunicam.


## <a name="get-azure-subscription"></a>Obter a subscrição do Azure

Antes de registar a pilha do Azure com o Azure, tem de ter:

- O ID de subscrição de uma subscrição do Azure. Para obter o ID, inicie sessão no Azure, clique em **mais serviços** > **subscrições**, clique na subscrição que pretende utilizar, e, em **Essentials** pode encontrar o **ID de subscrição**. China, Datacenters e subscrições de nuvem do Governo dos EUA não são atualmente suportados.
- O nome de utilizador e palavra-passe para uma conta que é proprietário da subscrição (são suportadas contas MSA/2FA).
- *Não é necessário a partir da versão de atualização de 1712 de pilha do Azure (180106.1):* o Azure Active Directory para a subscrição do Azure. Pode encontrar este diretório no Azure ao passar por cima do seu avatar no canto superior direito do portal do Azure.

Se não tiver uma subscrição do Azure que cumpra estes requisitos, pode [criar uma conta do Azure gratuita aqui](https://azure.microsoft.com/en-us/free/?b=17.06). Registar o Azure pilha incorreu sem qualquer custo na sua subscrição do Azure.

## <a name="register-azure-stack-with-azure"></a>Registar a pilha do Azure com o Azure  
> [!NOTE]
> Todos estes passos tem de ser executados a partir de uma máquina que tenha acesso ao ponto final com privilégios. Para o Kit de desenvolvimento de pilha do Azure, seria o computador anfitrião. Se estiver a utilizar um sistema integrado, contacte o operador de pilha do Azure.
>

1. Abra uma consola do PowerShell como administrador e [instale o PowerShell para Azure pilha](azure-stack-powershell-install.md).  

2. Adicione a conta do Azure que utiliza para registar a pilha do Azure. Para adicionar a conta, execute o `Add-AzureRmAccount` cmdlet com o parâmetro de EnvironmentName definido como **AzureCloud**. É-lhe pedido que introduza as credenciais da conta do Azure e poderá ter de utilizar a autenticação de fator 2, com base na configuração da sua conta.

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureCloud"
   ```

3. Se tiver várias subscrições, execute o seguinte comando para selecionar aquela que pretende utilizar:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Execute o seguinte comando para registar o fornecedor de recursos de pilha do Azure na sua subscrição do Azure:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Eliminar quaisquer versões dos módulos do PowerShell que correspondem ao registo existentes e [transferir a versão mais recente do mesmo a partir do GitHub](azure-stack-powershell-download.md).  

6. O diretório de "AzureStack-ferramentas-master" que é criado no passo anterior, navegue para a pasta "Registo" e importe o módulo ".\RegisterWithAzure.psm1":  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

7. Na mesma sessão do PowerShell, execute o seguinte script: quando lhe forem pedidas as credenciais, especifique `azurestack\cloudadmin` como o utilizador e a palavra-passe é igual à que utilizou para o administrador local durante a implementação.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Set-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development
   ```

   | Parâmetro | Descrição |  
   |--------|-------------|
   | CloudAdminCredential | As credenciais de domínio de nuvem que são utilizadas para [aceder ao ponto final com privilégios](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). O nome de utilizador está no formato  **\<domínio Azure pilha\>\cloudadmin**. No kit de desenvolvimento, o nome de utilizador é definida **azurestack\cloudadmin**. Se estiver a utilizar um sistema integrado, contacte o operador de pilha do Azure para obter este valor.|  
   | PrivilegedEndpoint | Uma previamente configurada PowerShell consola remota que fornece capacidades, como a recolha de registos e outra post tarefas de implementação. Para o kit de desenvolvimento, o ponto final com privilégios está alojado na máquina virtual "AzS ERCS01". Se estiver a utilizar um sistema integrado, contacte o operador de pilha do Azure para obter este valor. Para obter mais informações, consulte o [utilizando o ponto final com privilégios](azure-stack-privileged-endpoint.md) artigo.|  
   | BillingModel | O modelo de faturação que utiliza a sua subscrição. Valores para este parâmetro tem - permitidos **capacidade**, **PayAsYouUse**, e **desenvolvimento**. Para o kit de desenvolvimento, este valor é definido como **desenvolvimento**. Se estiver a utilizar um sistema integrado, contacte o operador de pilha do Azure para obter este valor. |

8. O script estiver concluído, verá uma mensagem "em ativação Azure (este passo pode demorar até 10 minutos a concluir) de pilha."




## <a name="verify-the-registration"></a>Verifique o registo  

1. Inicie sessão no portal do administrador (https://adminportal.local.azurestack.external).

2. Clique em **mais serviços** > **Marketplace gestão** > **adicionar a partir do Azure**.

3. Se vir uma lista de itens disponíveis a partir do Azure (por exemplo, WordPress), a ativação foi efetuada com êxito.

> [!NOTE]
> Após a conclusão do registo, o aviso de Active Directory para registar não deixará de ser apresentada.


## <a name="modify-the-registration"></a>Modificar o registo

### <a name="change-the-subscription-you-use"></a>Alterar a subscrição que utilizar
Se gostaria de alterar a subscrição que utilizar, tem primeiro execute Remove-AzsRegistration, certifique-se de que tem sessão iniciada contexto do Azure PowerShell correto e em seguida, execute Set-AzsRegistration com quaisquer parâmetros foi alterados.

  ```Powershell   
  Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```
### <a name="change-the-billing-model-or-syndication-features"></a>Alterar as funcionalidades de modelo ou de sindicação faturação
Se gostaria de alterar o modelo de faturação ou funcionalidades de sindicação para a instalação, pode chamar a função de registo para definir os novos valores. Não terá primeiro de remover o registo atual.  

  ```Powershell
     Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```


## <a name="disconnected-registration"></a>Registo desligado
*As informações nesta secção aplicam-se de que começa com a versão de atualização de 1712 de pilha do Azure (180106.1) e não são suportadas com versões anteriores.*

Se estiver a registar o Azure pilha num ambiente desligado, terá de obter um registo de token do ambiente de pilha do Azure e, em seguida, utilizar esse token numa máquina que pode ligar ao Azure para o registo.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Obter um registo de token do ambiente de pilha do Azure
  1. Para obter o token de registo, execute o seguinte:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > O token de registo é guardado no ficheiro especificado para *$env:SystemDrive\RegistrationToken.txt*.

  2. Guarde este token de registo para utilização do Azure ligado máquina.


### <a name="connect-to-azure-and-register"></a>Ligar ao Azure e registar
Execute os passos para este procedimento num computador que pode ligar ao Azure.

  1. [Transferir os mais recentes módulos do PowerShell que correspondem ao registo a partir do GitHub](azure-stack-powershell-download.md).  

  2. O diretório de "AzureStack-ferramentas-master" que é criado no passo anterior, navegue para a pasta "Registo" e importe o módulo ".\RegisterWithAzure.psm1":  

     ```powershell
     Import-Module .\RegisterWithAzure.psm1
     ```

  3. Cópia [RegisterWithAzure.psm1](https://go.microsoft.com/fwlink/?linkid=842959) para uma pasta, como *C:\Temp*.

  4. Inicie o ISE do PowerShell como administrador e, em seguida, importe o módulo de RegisterWithAzure.  

  5. Certifique-se de que tem sessão iniciada para o contexto do Azure PowerShell correto que pretende utilizar para registar o seu ambiente de pilha do Azure:  

     ```Powershell
        Set-AzureRmContext -SubscriptionId $YourAzureSubscriptionId   
     ```
  6. Especifique o token de registo para registar com o Azure:

     ```Powershell  
       $registrationToken = "*Your Registration Token*"
       Register-AzsEnvironment -RegistrationToken $registrationToken  
     ```
    Opcionalmente, pode utilizar o cmdlet Get-Content para apontar para um ficheiro que contém o token de registo:

    ```Powershell  
       $registrationToken = Get-Content -Path 'C:\Temp\<Registration Token File>'
       Register-AzsEnvironment -RegistrationToken $registrationToken  
    ```
> [!NOTE]  
> Guarde o nome do registo de recursos ou o token de registo para consulta futura.

### <a name="remove-a-registered-resource"></a>Remover um recurso registado
Se pretender remover o registo de recurso, tem de utilizar UnRegister-AzsEnvironment e passar o nome do registo de recursos ou o token de registo utilizado para registar AzsEnvironment.
- **Nome do registo de recursos:**

  ```Powershell    
     UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
  ```
- **Token de registo:**    

  ```Powershell
     $registrationToken = "*Your copied registration token*"
     UnRegister-AzsEnvironment -RegistrationToken $registrationToken
   ```


## <a name="next-steps"></a>Passos Seguintes
[Ligar ao Azure Stack](azure-stack-connect-azure-stack.md)
