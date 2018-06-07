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
ms.devlang: PowerShell
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/20/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: a093e60718881b2fe9ca70df7596e8963dc55d9f
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808048"
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

1. [Instale o PowerShell para a pilha do Azure](../azure-stack-powershell-install.md).

  ```PowerShell  
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ```

2. Se utilizar **serviços de Federação do Active Directory**, utilize o seguinte cmdlet:

  ```PowerShell
  # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAuidence endpoint for your environment>"

  # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    ```

3. Inicie sessão pilha do Azure como um operador. Para obter instruções, consulte [iniciar sessão na pilha do Azure como um operador](../azure-stack-powershell-configure-admin.md).

   ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

4. Adicione a imagem do Windows Server 2016 para o mercado de pilha do Azure.

    O **adicionar AzsPlatformimage** cmdlet utilizado para adicionar a imagem Especifica os valores utilizados pelos modelos Azure Resource Manager para fazer referência a imagem VM.
    
    Os valores incluem:
    
  - **publisher**  
    Por exemplo: `Microsoft`  
    O segmento de nome do publicador da imagem VM que os utilizadores utilizam quando implementam a imagem. Um exemplo é **Microsoft**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **offer**  
    Por exemplo: `WindowsServer`  
    O segmento de nome de oferta da imagem VM que os utilizadores utilizam quando implementam a imagem VM. Um exemplo é **WindowsServer**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **sku**  
    Por exemplo: `Datacenter2016`  
    O segmento de nome SKU da imagem de VM que os utilizadores utilizam quando implementam a imagem VM. Um exemplo é **Datacenter2016**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **Versão**  
    Por exemplo: `1.0.0`  
    A versão da imagem de VM que os utilizadores utilizam quando implementam a imagem VM. Esta versão está no formato  *\#.\#. \#*. Um exemplo é **1.0.0**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **osType**  
    Por exemplo: `Windows`  
    O osType da imagem tem de ser um **Windows** ou **Linux**. Substitua *fully_qualified_path_to_ISO* com o caminho para o ficheiro do Windows Server 2016 ISO que transferiu. 
  - **OSUri**  
    Por exemplo: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    Pode especificar um URI de armazenamento de BLOBs para um `osDisk`. O caso, deverá especificar a localização onde armazenou a imagem do que transferiu.

    Para obter mais informações, consulte a referência do PowerShell para o [adicionar AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) cmdlet.

    Abra o PowerShell com uma linha de comandos elevada e execute:

      ````PowerShell  
        Add-AzsPlatformimage -publisher "Microsoft" -offer "WindowsServer" -sku "Datacenter2016" -version "1.0.0” -OSType "Windows" -OSUri "<fully_qualified_path_to_ISO>"
      ````  

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
