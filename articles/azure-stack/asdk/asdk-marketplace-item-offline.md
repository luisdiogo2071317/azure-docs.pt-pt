---
title: Adicionar um item do marketplace pilha do Azure a partir de uma origem local | Microsoft Docs
description: Descreve como adicionar uma imagem de sistema operativo local para a pilha do Azure Marketplace.
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
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 676dcdea5912aed5504c386f01d6809eccf763d4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-a-local-source"></a>Tutorial: adicionar um item do marketplace pilha do Azure a partir de uma origem local

Como um operador de pilha do Azure, a primeira coisa que precisa de fazer para permitir aos utilizadores implementar uma Máquina Virtual (VM) é adicionar uma imagem VM para o mercado de pilha do Azure. Por predefinição, nada é publicado para o mercado de pilha do Azure, mas pode carregar imagens ISO da VM para que fiquem disponíveis para os seus utilizadores. Utilize esta opção se tiver implementado pilha do Azure num cenário desligado ou nos cenários com conectividade limitada.

Neste tutorial, pode transferir uma imagem de VM do Windows Server 2016 das avaliações de servidor Windows página e carregue-o para o mercado de pilha do Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar uma imagem de VM do Windows Server 2016
> * Certifique-se de que a imagem VM está disponível 
> * A imagem VM de teste

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

- Instalar o [módulos do Azure pilha compatível com o Azure PowerShell](asdk-post-deploy.md#install-azure-stack-powershell)
- Transferir o [ferramentas de pilha do Azure](asdk-post-deploy.md#download-the-azure-stack-tools)
- Transferir o [imagem de ISO de máquina virtual do Windows Server 2106](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016) da página avaliações do Windows Server

## <a name="add-a-windows-server-vm-image"></a>Adicionar uma imagem de VM do Windows Server
Pode publicar a imagem do Windows Server 2016 para o mercado de pilha do Azure através da adição de uma imagem ISO anteriormente transferida utilizando o PowerShell. 

Utilize esta opção se tiver implementado pilha do Azure num cenário desligado ou nos cenários com conectividade limitada.

1. Importar a pilha de Azure `Connect` e `ComputeAdmin` módulos do PowerShell incluídos no diretório de ferramentas de pilha do Azure, utilizando os seguintes comandos:

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.   
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   ```

2. Execute um dos seguintes scripts sessão no computador anfitrião ASDK consoante se implementou o ambiente de pilha do Azure utilizando o Azure Active Directory (Azure AD) ou os serviços de Federação do Active Directory (AD FS):

  - Os comandos para **implementações do Azure AD**: 

      ```PowerShell
      # To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "https://adminmanagement.local.azurestack.external"

      # To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "https://graph.windows.net/"
      
      # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
      Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" `
        -ArmEndpoint $ArmEndpoint

      Set-AzureRmEnvironment `
        -Name "AzureStackAdmin" `
        -GraphAudience $GraphAudience

      $TenantID = Get-AzsDirectoryTenantId `
      # Replace the AADTenantName value to reflect your Azure AD tenant name.
        -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
        -EnvironmentName AzureStackAdmin

      Add-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```

  - Os comandos para **implementações do AD FS**:
      
      ```PowerShell
      # To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "https://adminmanagement.local.azurestack.external"

      # To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "https://graph.local.azurestack.external/"

      # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
      Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" `
        -ArmEndpoint $ArmEndpoint

      Set-AzureRmEnvironment `
        -Name "AzureStackAdmin" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true

      $TenantID = Get-AzsDirectoryTenantId `
      -ADFS `
      -EnvironmentName "AzureStackAdmin" 

      Add-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```
   
3. Adicione a imagem do Windows Server 2016 para o mercado de pilha do Azure. (Substituir *fully_qualified_path_to_ISO* com o caminho para o ficheiro do Windows Server 2016 ISO que transferiu.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

## <a name="verify-the-marketplace-item-is-available"></a>Certifique-se de que o item do marketplace está disponível
Utilize estes passos para verificar que a nova imagem VM está disponível no mercado pilha do Azure.

1. Iniciar sessão para o [portal de administração ASDK](https://adminportal.local.azurestack.external).

2. Selecione **mais serviços** > **Marketplace gestão**. 

3. Certifique-se de que a imagem de VM do Windows Server 2016 Centro de dados foi adicionada com êxito.

   ![Imagem transferida a partir do Azure](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>A imagem VM de teste
Como um operador de pilha do Azure, pode utilizar o [portal de administrador](https://adminportal.local.azurestack.external) para criar um teste de VM para validar a imagem tem foram disponibilizada com êxito. 

1. Iniciar sessão para o [portal de administração ASDK](https://adminportal.local.azurestack.external).

2. Clique em **novo** > **computação** > **Datacenter do Windows Server 2016** > **criar**.  
 ![Criar a VM de imagem do marketplace](media/asdk-marketplace-item/new-compute.png)

3. No **Noções básicas** painel, introduza as seguintes informações e, em seguida, clique em **OK**:
  - **Nome**: 1 de vm de teste
  - **Nome de utilizador**: AdminTestUser
  - **Palavra-passe**: AzS TestVM01
  - **Subscrição**: aceitar a subscrição do fornecedor predefinido
  - **Grupo de recursos**: rg de vm de teste
  - **Localização**: local

4. No **escolher um tamanho** painel, clique em **A1 padrão**e, em seguida, clique em **selecione**.  

5. No **definições** painel, aceite as predefinições e clique em **OK**

6. No painel **Resumo**, clique em **OK** para criar a máquina virtual.  
> [!NOTE]
> A implementação da máquina virtual demora alguns minutos a concluir.

7. Para ver a nova VM, clique em **máquinas virtuais**, em seguida, procure **1 de vm de teste** e clique no respetivo nome.
    ![Imagem VM de teste primeiro apresentada](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Limpar recursos
Depois de ter com êxito com sessão iniciada para a VM e verificar se a imagem de teste está a funcionar corretamente, deve eliminar o grupo de recursos de teste. Isto irá libertar recursos limitados disponíveis para instalações de ASDK de nó único.

Quando já não é necessário, elimine o grupo de recursos, a VM e todos os recursos relacionados, seguindo estes passos:

1. Iniciar sessão para o [portal de administração ASDK](https://adminportal.local.azurestack.external).
2. Clique em **grupos de recursos** > **rg de vm de teste** > **eliminar grupo de recursos**.
3. Tipo **rg de vm de teste** como o nome do grupo de recursos e, em seguida, clique em **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Adicionar uma imagem de VM do Windows Server 2016
> * Certifique-se de que a imagem VM está disponível 
> * A imagem VM de teste

Avançar para o próximo tutorial para saber como criar uma oferta de pilha do Azure e o plano.

> [!div class="nextstepaction"]
> [Oferecer serviços IaaS de pilha do Azure](asdk-offer-services.md)
