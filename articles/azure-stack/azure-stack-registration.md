---
title: Sistemas integrados do registo do Azure para o Azure Stack | Documentos da Microsoft
description: Descreve o processo de registo do Azure para implementações de ligada ao Azure Stack Azure com vários nós.
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
ms.date: 07/09/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: 65525ffe33ddc100dd3066e7c2b52ef8a856fbc3
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37934676"
---
# <a name="register-azure-stack-with-azure"></a>Registar o Azure Stack com o Azure

A registar [do Azure Stack](azure-stack-poc.md) com o Azure permite-lhe para transferir itens do marketplace do Azure e configurar relatórios de volta à Microsoft de dados de comércio. Depois de registar o Azure Stack, utilização é comunicada ao Azure commerce e pode vê-lo sob a subscrição utilizada para o registo.

> [!IMPORTANT]  
> É necessário Registro para dar suporte a todas as funcionalidades do Azure Stack, incluindo a distribuição de mercado. Além disso, será estar em violação dos termos de licenciamento, se não registar ao utilizar o modelo de faturação de pagamento-como-utiliza do Azure Stack. Para saber mais sobre o Azure Stack, modelos de licenciamento, consulte a [como comprar página](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Pré-requisitos

Antes de registar o Azure Stack com o Azure, tem de ter:

- O ID de subscrição para uma subscrição do Azure. Para obter o ID, inicie sessão no Azure, clique em **mais serviços** > **subscrições**, clique na subscrição que pretende utilizar, e, em **Essentials** pode encontrar o ID da subscrição.

  > [!NOTE]
  > Alemanha e subscrições de cloud do Governo dos EUA não são atualmente suportadas.

- O nome de utilizador e palavra-passe para uma conta que seja o proprietário da subscrição (são suportadas contas MSA/2FA).
- Registado o fornecedor de recursos do Azure Stack (consulte a secção de registar o fornecedor de recursos do Azure Stack abaixo para obter detalhes).

Se não tiver uma subscrição do Azure que cumpra estes requisitos, pode [criar uma conta gratuita do Azure aqui](https://azure.microsoft.com/free/?b=17.06). Registar o Azure Stack, incorre em sem custos na sua subscrição do Azure.

### <a name="powershell-language-mode"></a>Modo de idioma do PowerShell

Para registar com êxito o Azure Stack, o modo de idioma do PowerShell deve ser definido como **FullLanguageMode**.  Para verificar se o modo de idioma atual está definido como completa, abra uma janela elevada do PowerShell e execute os seguintes comandos do PowerShell:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Certifique-se de que a saída devolve **FullLanguageMode**. Se qualquer outro modo de idioma é retornado, registo, terá de ser executado em outra máquina ou o modo de idioma tem de ser definido como **FullLanguageMode** antes de continuar.

### <a name="bkmk_powershell"></a>Instalar o PowerShell para o Azure Stack

Tem de utilizar o PowerShell mais recente para o Azure Stack para registar com o Azure.

Se ainda não estiver instalado, [instalar o PowerShell para o Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="bkmk_tools"></a>Baixe as ferramentas do Azure Stack

O repositório de GitHub de ferramentas do Azure Stack contém módulos do PowerShell que suportam a funcionalidade do Azure Stack; incluindo a funcionalidade de registo. Durante o processo de registo, terá de importar e utilizar o módulo do PowerShell de RegisterWithAzure.psm1, disponível no repositório de ferramentas do Azure Stack, para registar a sua instância do Azure Stack com o Azure.

Para garantir que está a utilizar a versão mais recente, deve excluir as versões existentes das ferramentas do Azure Stack e [transferir a versão mais recente a partir do GitHub](azure-stack-powershell-download.md) antes de registar com o Azure.

## <a name="register-azure-stack-in-connected-environments"></a>Registar o Azure Stack em ambientes ligados

Podem aceder a ambientes conectados à internet e Azure. Para estes ambientes, terá de registar o fornecedor de recursos do Azure Stack com o Azure e, em seguida, configurar o modelo de faturação.

> [!NOTE]
> Todos estes passos tem de ser executados a partir de um computador que tenha acesso ao ponto final com privilégios.

### <a name="register-the-azure-stack-resource-provider"></a>Registar o fornecedor de recursos do Azure Stack

Para registar o fornecedor de recursos do Azure Stack com o Azure, inicie o ISE do PowerShell como administrador e utilize os seguintes comandos do PowerShell com o **EnvironmentName** parâmetro definido como o tipo de subscrição do Azure adequada (consulte a parâmetros abaixo).

1. Adicione a conta do Azure que utiliza para registar o Azure Stack. Para adicionar a conta, execute o **Add-AzureRmAccount** cmdlet. Lhe for pedido que introduza as credenciais de conta de administrador global do Azure e poderá ter de utilizar a autenticação de 2 fatores com base na configuração da sua conta.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parâmetro | Descrição |  
   |-----|-----|
   | EnvironmentName | O nome de ambiente de subscrição de cloud do Azure. Os nomes de ambiente com suporte são **AzureCloud** ou, se utilizar uma subscrição do Azure China **AzureChinaCloud**.  |
   |  |  |

2. Se tiver várias subscrições, execute o seguinte comando para selecionar a que pretende utilizar:  

   ```PowerShell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Execute o seguinte comando para registar o fornecedor de recursos do Azure Stack na sua subscrição do Azure:

   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Registar o Azure Stack com o Azure com o modelo de faturação de pagamento-como-utiliza

Utilize estes passos para registar o Azure Stack com o Azure com o modelo de faturação de pagamento-como-utiliza.

1. Inicie o ISE do PowerShell como administrador e navegue para o **registo** pasta na **AzureStack-Tools-master** diretório criado quando [baixei as ferramentas do Azure Stack](#bkmk_tools). Importar os **RegisterWithAzure.psm1** módulo com o PowerShell:

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Em seguida, na mesma sessão do PowerShell, certifique-se que estiver conectado ao contexto correto de PowerShell do Azure. Esta é a conta do azure que foi utilizada para registar o fornecedor de recursos do Azure Stack, acima. PowerShell para executar:

   ```powershell
   Add-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>"
   ```

3. Na mesma sessão do PowerShell, execute o **Set-AzsRegistration** cmdlet. PowerShell para executar:  

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
  |PrivilegedEndpoint|Uma pré-configurado remota consola do PowerShell que fornece-lhe capacidades como a recolha de registos e outro post tarefas de implementação. Para obter mais informações, consulte a [utilizando o ponto final com privilégios](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artigo.|
  |BillingModel|O modelo de faturação que utiliza a sua subscrição. Valores para este parâmetro permitidos são: capacidade, PayAsYouUse e desenvolvimento.|
  |  |  |

  O processo demorará entre 10 a 15 minutos. Quando o comando for concluído, verá a mensagem **"o seu ambiente está agora registado e ativado usando os parâmetros fornecidos."**

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Registar o Azure Stack com o Azure com o modelo de faturação de capacidade

Siga as mesmas instruções utilizadas para registar com o modelo de faturação de pagamento-como-utiliza, mas adicionar o número do contrato sob a qual capacidade foi comprada e alterar os **BillingModel** parâmetro para **decapacidade**. Todos os outros parâmetros são iguais.

PowerShell para executar:

```powershell
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Registar o Azure Stack em ambientes desligados
Se está a registar Azure Stack num ambiente desligado (sem conectividade de internet), terá de obter um registo de token do ambiente do Azure Stack e, em seguida, utilizar esse token num computador que pode ligar ao Azure e tem [PowerShell para o Azure Stack instalado](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Obter um registo de token do ambiente do Azure Stack

1. Inicie o ISE do PowerShell como administrador e navegue para o **registo** pasta na **AzureStack-Tools-master** diretório criado quando [baixei as ferramentas do Azure Stack](#bkmk_tools). Importar os **RegisterWithAzure.psm1** módulo:  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Para obter o token de registo, execute os seguintes comandos do PowerShell:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!TIP]  
   > O token de registo é guardado no ficheiro especificado para *$FilePathForRegistrationToken*. Pode alterar o caminho do ficheiro ou o nome de ficheiro a seu critério.

3. Guarde este token de registo para utilização no Azure ligado máquina. Pode copiar o ficheiro ou o texto de $FilePathForRegistrationToken.

### <a name="connect-to-azure-and-register"></a>Ligar ao Azure e registe-se

No computador que está ligados à internet, execute os mesmos passos para importar o módulo de RegisterWithAzure.psm1 e início de sessão para o contexto correto do Azure Powershell. Em seguida, chamar AzsEnvironment Registre-se e especifique o token de registo para registar com o Azure:

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
  > Guarde o nome do registo de recursos e o token de registo para referência futura.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Obter uma chave de ativação do recurso de registo do Azure

Em seguida, terá de obter uma chave de ativação do recurso de registo criado no Azure durante AzsEnvironment Registre-se.

Para obter a chave de ativação, execute os seguintes comandos do PowerShell:  

  ```Powershell
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!TIP]  
  > A chave de ativação é guardada no ficheiro especificado para *$KeyOutputFilePath*. Pode alterar o caminho do ficheiro ou o nome de ficheiro a seu critério.

### <a name="create-an-activation-resource-in-azure-stack"></a>Criar um recurso de ativação no Azure Stack

Regressar ao ambiente do Azure Stack com o ficheiro ou o texto da chave de ativação criada a partir de Get-AzsActivationKey. Em seguida vai criar um recurso de ativação no Azure Stack com essa chave de ativação. Para criar um recurso de ativação, execute os seguintes comandos do PowerShell:  

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

Opcionalmente, pode utilizar o cmdlet Get-Content para apontar para um ficheiro que contém o token de registo:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Verifique se o registo do Azure Stack

Utilize estes passos para verificar que o Azure Stack com êxito é registrado com o Azure.

1. Inicie sessão para o Azure Stack [portal de administrador](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;região >. &lt;fqdn >*.
2. Selecione **mais serviços** > **gestão de Marketplace** > **adicionar a partir do Azure**.

Se vir uma lista de itens disponíveis do Azure (por exemplo, WordPress), a ativação foi concluída com êxito. No entanto, em ambientes desligados não verá itens do marketplace do Azure no mercado do Azure Stack.

> [!NOTE]
> Após a conclusão do registo, já não vai aparecer o aviso de Active Directory para não registar.

## <a name="renew-or-change-registration"></a>Renovar ou alterar o registo

### <a name="renew-or-change-registration-in-connected-environments"></a>Renovar ou alterar o registo em ambientes ligados

Terá de atualizar ou renovar o registo nas seguintes circunstâncias:

- Depois de renovar sua assinatura anual baseado em capacidades.
- Quando altera o modelo de faturação.
- Quando dimensionar as alterações (adicionar ou remover nós) para faturação com base na capacidade.

#### <a name="change-the-subscription-you-use"></a>Alterar a subscrição que utilizar

Se desejar alterar a subscrição que utilizar, tem de executar primeiro o **Remove-AzsRegistration** cmdlet, em seguida, certifique-se estiver conectado ao contexto correto do Azure PowerShell e, finalmente execute **AzsRegistration de conjunto**  com qualquer alterado parâmetros:

  ```powershell
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Alterar as funcionalidades de faturação do modelo ou de distribuição

Se quiser alterar o modelo de faturação ou as funcionalidades de distribuição para a sua instalação, pode chamar a função de Registro para definir os valores novos. Não é necessário primeiro de remover o registo atual:

  ```powershell
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Renovar ou alterar o registo em ambientes desligados

Terá de atualizar ou renovar o registo nas seguintes circunstâncias:

- Depois de renovar sua assinatura anual baseado em capacidades.
- Quando altera o modelo de faturação.
- Quando dimensionar as alterações (adicionar ou remover nós) para faturação com base na capacidade.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Remover o recurso de ativação do Azure Stack

Primeiro terá de remover o recurso de ativação do Azure Stack e, em seguida, o recurso de registo no Azure.  

Para remover o recurso de ativação no Azure Stack, execute os seguintes comandos do PowerShell no seu ambiente do Azure Stack:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Em seguida, para remover o recurso de registo no Azure, certifique-se estiver a utilizar um Azure ligado computador, inicie sessão para o contexto correto do Azure PowerShell e execute os comandos do PowerShell apropriados, conforme descrito abaixo.

Pode usar o token de registo utilizado para criar o recurso:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Em alternativa, pode utilizar o nome do registo:

  ```Powershell
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Voltar a registar utilizando passos desligados

Tem agora completamente registo anulado com êxito num cenário de desligado e terá de repetir os passos para registar um ambiente do Azure Stack num cenário de desligado.

## <a name="next-steps"></a>Passos Seguintes

[Transferir itens do marketplace do Azure](azure-stack-download-azure-marketplace-item.md)
