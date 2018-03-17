---
title: Registar o ASDK com o Azure | Microsoft Docs
description: "Descreve como registar a pilha do Azure com o Azure para ativar relatórios de utilização e de sindicação do marketplace."
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
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 9e2dbc71f6424b87945e346a42c86d4cde7f740e
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="register-azure-stack-with-azure"></a>Registar a pilha do Azure com o Azure
Pode registar a instalação do Kit de desenvolvimento de pilha do Azure (ASDK) com o Azure para transferir os itens do marketplace a partir do Azure e configurar dados de comércio relatórios de volta à Microsoft. Registo é recomendado porque permite-lhe testar importante funcionalidade da pilha do Azure, como os relatórios de utilização e de sindicação do marketplace. Depois de registar pilha do Azure, a utilização é comunicada para comércio do Azure. Pode vê-lo sob a subscrição utilizada para o registo. No entanto, não são cobrados utilizadores ASDK para qualquer utilização que possam comunicam.


## <a name="register-azure-stack-with-azure"></a>Registar a pilha do Azure com o Azure 
Siga estes passos para registar o ASDK com o Azure.

> [!NOTE]
> Todos estes passos tem de ser executados a partir de um computador que tenha acesso ao ponto final com privilégios. Para ASDK, que é o computador de anfitrião do kit de desenvolvimento. 

Antes de utilizar estes passos para registar o ASDK com o Azure, certifique-se de que tem instalado o Azure PowerShell de pilha e transferir as ferramentas de pilha de Azure conforme descrito no [configuração pós-implementação](asdk-post-deploy.md) artigo. 

1. Abra uma consola do PowerShell como administrador.  

2. Os seguintes comandos do PowerShell para registar a sua instalação ASDK com o Azure (irá precisar de iniciar sessão na sua subscrição do Azure e a instalação de ASDK local):

    ```PowerShell
    # Add the Azure cloud subscription environment name. Supported environment names are AzureCloud or, if using a China Azure Subscription, AzureChinaCloud.
    Add-AzureRmAccount -EnvironmentName "AzureCloud"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
    Set-AzsRegistration `
        -CloudAdminCredential $CloudAdminCred `
        -PrivilegedEndpoint AzS-ERCS01 `
        -BillingModel Development

3. When the script completes, you should see this message: **Your environment is now registered and activated using the provided parameters.**

    ![](media/asdk-register/1.PNG) 

## Verify the registration was successful
Follow these steps to verify that the ASDK registration with Azure was successful.

1. Sign in to the [Azure Stack administration portal](https://adminportal.local.azurestack.external).

2. Click **Marketplace Management** > **Add from Azure**.

    ![](media/asdk-register/2.PNG) 

3. If you see a list of items available from Azure, your activation was successful.

    ![](media/asdk-register/3.PNG) 

## Next steps
[Add an Azure Stack marketplace item](asdk-marketplace-item.md)