---
title: Registar o ASDK com o Azure | Microsoft Docs
description: Descreve como registar a pilha do Azure com o Azure para ativar relatórios de utilização e de sindicação do marketplace.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 08a300d0e2d1565428f282a2073d91b5dd08c060
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37017004"
---
# <a name="azure-stack-registration"></a>Registo de pilha do Azure
Pode registar a instalação do Kit de desenvolvimento de pilha do Azure (ASDK) com o Azure para transferir os itens do marketplace a partir do Azure e configurar dados de comércio relatórios de volta à Microsoft. Registo é necessário para suportar a funcionalidade completa de pilha do Azure, incluindo sindicação do marketplace. Registo é recomendado porque permite-lhe testar importante funcionalidade da pilha do Azure, como os relatórios de utilização e de sindicação do marketplace. Depois de registar pilha do Azure, a utilização é comunicada para comércio do Azure. Pode vê-lo sob a subscrição utilizada para o registo. No entanto, não são cobrados utilizadores ASDK para qualquer utilização que possam comunicam.

Se não registar o seu ASDK, poderá ver um **ativação necessária** alerta de aviso que indica a registar o Kit de desenvolvimento de pilha do Azure. Este comportamento é esperado.

## <a name="prerequisites"></a>Pré-requisitos
Antes de utilizar estas instruções para registar o ASDK com o Azure, certifique-se de que tem instalado o Azure PowerShell de pilha e transferir as ferramentas de pilha de Azure conforme descrito no [configuração pós-implementação](asdk-post-deploy.md) artigo.

Além disso, o modo de idioma do PowerShell deve ser definido como **FullLanguageMode** no computador utilizado para registar o ASDK com o Azure. Para verificar se o modo de idioma atual está definido como completa, abra uma janela elevada do PowerShell e execute os seguintes comandos do PowerShell:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Certifique-se de que devolve o resultado **FullLanguageMode**. Se for devolvido qualquer outro modo de idioma, registo, terá de ser executado noutro computador ou o modo de idioma, terá de ser definido como **FullLanguageMode** antes de continuar.

## <a name="register-azure-stack-with-azure"></a>Registar a pilha do Azure com o Azure
Siga estes passos para registar o ASDK com o Azure.

> [!NOTE]
> Todos estes passos tem de ser executados a partir de um computador que tenha acesso ao ponto final com privilégios. Para ASDK, que é o computador de anfitrião do kit de desenvolvimento.

1. Abra uma consola do PowerShell como administrador.  

2. Execute os seguintes comandos do PowerShell para registar a sua instalação ASDK com o Azure. Terá de iniciar sessão na sua subscrição do Azure e a instalação de ASDK local. Se não tiver uma subscrição do Azure, ainda pode [criar uma conta do Azure gratuita aqui](https://azure.microsoft.com/free/?b=17.06). Registar o Azure pilha incorreu sem qualquer custo na sua subscrição do Azure.

  ```powershell
  # Add the Azure cloud subscription environment name. Supported environment names are AzureCloud or, if using a China Azure Subscription, AzureChinaCloud.
  Add-AzureRmAccount -EnvironmentName "AzureCloud"

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  #Register Azure Stack
  $AzureContext = Get-AzureRmContext
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01 `
      -BillingModel Development
  ```
3. O script estiver concluído, verá esta mensagem: **ambiente está agora registado e ativado utilizando os parâmetros fornecidos.**

    ![](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Certifique-se de que o registo foi efetuado com êxito
Siga estes passos para verificar se o registo ASDK com o Azure foi concluída com êxito.

1. Iniciar sessão para o [portal de administração do Azure pilha](https://adminportal.local.azurestack.external).

2. Clique em **Marketplace gestão** > **adicionar a partir do Azure**.

    ![](media/asdk-register/2.PNG)

3. Se vir uma lista de itens disponíveis a partir do Azure, a ativação foi efetuada com êxito.

    ![](media/asdk-register/3.PNG)

## <a name="next-steps"></a>Passos Seguintes
[Adicionar um item do marketplace pilha do Azure](.\.\azure-stack-marketplace.md)
