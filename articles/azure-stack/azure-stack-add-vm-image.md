---
title: Adicionar e remover uma imagem VM para a pilha do Azure | Microsoft Docs
description: Adicionar ou remover a Windows ou Linux VM imagem personalizada sua organização para os inquilinos utilizarem.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 39708248160b029185b64ed927a453562e1003f2
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="make-a-virtual-machine-image-available-in-azure-stack"></a>Disponibilizar uma imagem de máquina virtual na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Na pilha do Azure, pode tornar imagens da máquina virtual disponíveis para os seus utilizadores. Essas imagens podem ser referenciadas pelas modelos Azure Resource Manager, ou pode adicioná-los para a IU do Azure Marketplace como um item do Marketplace. Pode utilizar uma imagem forma global do Azure Marketplace ou adicionar a sua imagem personalizada. Pode adicionar uma VM utilizando o portal ou o Windows PowerShell.

## <a name="add-a-vm-image-through-the-portal"></a>Adicionar uma imagem VM através do portal

> [!NOTE]
> Com este método, tem de criar o item do Marketplace em separado.

As imagens devem ser capazes de ser referenciado por um URI de armazenamento de Blobs. Preparar uma imagem de sistema operativo Windows ou Linux no formato VHD (não VHDX) e, em seguida, carregue a imagem para uma conta de armazenamento no Azure ou a pilha do Azure. Se a imagem já está carregada para o armazenamento de Blobs do Azure ou a pilha do Azure, pode ignorar o passo 1.

1. [Carregar uma imagem de VM do Windows Azure para implementações do Resource Manager](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) ou, para uma imagem do Linux, siga as instruções descritas em [máquinas virtuais Linux de implementar no Azure pilha](azure-stack-linux.md). Antes de carregar a imagem, é importante a ter em consideração os seguintes fatores:

   - Pilha do Azure suporta o formato VHD do disco fixo. O formato fixo estruturas de disco lógico forma linear dentro do ficheiro, pelo que esse X do deslocamento de disco é armazenado no desvio de blob X. Um rodapé pequeno no final do blob descreve as propriedades do VHD. Para confirmar se o disco está corrigido, utilize o [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) comando do PowerShell.  

    > [!IMPORTANT]
    >  Pilha do Azure não suporta VHDs de disco dinâmico. Redimensionar um disco dinâmico que está ligado a uma VM deixará a VM em estado de falha. Para atenuar este problema, elimine a VM sem eliminar o disco da VM, um blob VHD numa conta do storage. Converter o VHD de um disco dinâmico para um disco fixo e, em seguida, voltar a criar a máquina virtual.

   * É mais eficiente para carregar uma imagem para o armazenamento de Blobs do Azure pilha que para o Azure armazenamento de BLOBs porque demora menos tempo para enviar a imagem para o repositório de imagens de pilha do Azure.

   * Ao carregar o [imagem de VM do Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), certifique-se de que substitui o **iniciar sessão no Azure** passo com o [configurar o ambiente de PowerShell o operador de pilha do Azure](azure-stack-powershell-configure-admin.md) passo.  

   * Tome nota do blob storage URI onde carregar a imagem. Armazenamento de Blobs do URI tem o seguinte formato: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;*. vhd.

   * Para tornar o blob anonimamente acessível, vá para o contentor de blob de conta de armazenamento onde a imagem VM VHD foi carregada. Selecione **Blob**e, em seguida, selecione **política de acesso**. Opcionalmente, pode em vez disso, gerar uma assinatura de acesso partilhado do contentor e incluí-la como parte do URI de blob.

   ![Ir para blobs de conta de armazenamento](./media/azure-stack-add-vm-image/image1.png)

   ![Acesso de blob de conjunto para público](./media/azure-stack-add-vm-image/image2.png)

2. Inicie sessão no Azure pilha como operador. No menu, selecione **mais serviços** > **fornecedores de recursos**. Em seguida, selecione **computação** > **imagens da VM** > **adicionar**.

3. Em **adicionar uma imagem de VM**, introduza o publicador, oferta, SKU e versão da imagem de máquina virtual. Consulte estes segmentos de nome para a imagem VM em modelos do Resource Manager. Certifique-se de que seleciona o **osType** valor corretamente. Para **URI de Blob do disco de SO**, introduza o URI de Blob onde a imagem foi carregada. Em seguida, selecione **criar** para começar a criar a imagem de VM.

   ![Begin para criar a imagem](./media/azure-stack-add-vm-image/image4.png)

   Quando a imagem é criada com êxito, o estado de imagem VM mudar para **com êxito**.

4. Para disponibilizar a imagem de máquina virtual mais prontamente para consumo dos utilizadores na IU, é uma boa ideia [criar um item do Marketplace](azure-stack-create-and-publish-marketplace-item.md).

## <a name="remove-a-vm-image-through-the-portal"></a>Remover uma imagem de VM através do portal

1. Abra o portal de administração no [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

2. Selecione **gestão Marketplace**e, em seguida, selecione a VM que pretende eliminar.

3. Clique em **Eliminar**.

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>Adicionar uma imagem VM para o mercado utilizando o PowerShell

1. [Instale o PowerShell para a pilha do Azure](azure-stack-powershell-install.md).  

2. Inicie sessão pilha do Azure como um operador. Para obter instruções, consulte [iniciar sessão na pilha do Azure como um operador](azure-stack-powershell-configure-admin.md).

3. Abra o PowerShell com uma linha de comandos elevada e execute:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>” `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
  ````

  O **adicionar AzsPlatformimage** cmdlet Especifica os valores utilizados pelos modelos Azure Resource Manager para fazer referência a imagem VM. Os valores incluem:
  - **publisher**  
    Por exemplo: `Canonical`  
    O segmento de nome do publicador da imagem VM que os utilizadores utilizam quando implementam a imagem. Um exemplo é **Microsoft**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **offer**  
    Por exemplo: `UbuntuServer`  
    O segmento de nome de oferta da imagem VM que os utilizadores utilizam quando implementam a imagem VM. Um exemplo é **WindowsServer**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **sku**  
    Por exemplo: `14.04.3-LTS`  
    O segmento de nome SKU da imagem de VM que os utilizadores utilizam quando implementam a imagem VM. Um exemplo é **Datacenter2016**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **Versão**  
    Por exemplo: `1.0.0`  
    A versão da imagem de VM que os utilizadores utilizam quando implementam a imagem VM. Esta versão está no formato  *\#.\#. \#*. Um exemplo é **1.0.0**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **osType**  
    Por exemplo: `Linux`  
    O osType da imagem tem de ser um **Windows** ou **Linux**.  
  - **OSUri**  
    Por exemplo: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    Pode especificar um URI de armazenamento de BLOBs para um `osDisk`.  

    Para obter mais informações sobre o cmdlet Add-AzsPlatformimage, consulte o PowerShell Microsoft [documentação do módulo de operador de pilha do Azure](https://docs.microsoft.com/powershell/module/).

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>Adicionar uma imagem VM personalizada no Marketplace utilizando o PowerShell

1. [Instale o PowerShell para a pilha do Azure](azure-stack-powershell-install.md).

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

3. Inicie sessão pilha do Azure como um operador. Para obter instruções, consulte [iniciar sessão na pilha do Azure como um operador](azure-stack-powershell-configure-admin.md).

4. Crie uma conta do storage no global do Azure ou de pilha do Azure para armazenar a imagem VM personalizada. Para obter instruções, consulte [início rápido: carregar, transfira e blobs de lista no portal do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

5. Preparar uma imagem de sistema operativo Windows ou Linux no formato VHD (não VHDX), carregue a imagem para a sua conta do storage e obter o URI onde a imagem VM pode ser obtida através do PowerShell.  

  ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

6. (Opcionalmente) Pode carregar uma matriz de discos de dados como parte da imagem de VM. Crie os discos de dados utilizando o cmdlet New-DataDiskObject. Abra o PowerShell a partir de uma linha de comandos elevada e execute:

  ````PowerShell  
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
  ````

7. Abra o PowerShell com uma linha de comandos elevada e execute:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>” -OSType "<ostype>" -OSUri "<osuri>"
  ````

    Para obter mais informações sobre o cmdlet Add-AzsPlatformimage e o cmdlet New-DataDiskObject, consulte o PowerShell Microsoft [documentação do módulo de operador de pilha do Azure](https://docs.microsoft.com/powershell/module/).

## <a name="remove-a-vm-image-by-using-powershell"></a>Remover uma imagem de VM com o PowerShell

Quando já não necessita da imagem de máquina virtual que carregou, pode eliminar do Marketplace utilizando o cmdlet seguinte:

1. [Instale o PowerShell para a pilha do Azure](azure-stack-powershell-install.md).

2. Inicie sessão pilha do Azure como um operador.

3. Abra o PowerShell com uma linha de comandos elevada e execute:

  ````PowerShell  
  Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
  ````
  O **remover AzsPlatformImage** cmdlet Especifica os valores utilizados pelos modelos Azure Resource Manager para fazer referência a imagem VM. Os valores incluem:
  - **publisher**  
    Por exemplo: `Canonical`  
    O segmento de nome do publicador da imagem VM que os utilizadores utilizam quando implementam a imagem. Um exemplo é **Microsoft**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **offer**  
    Por exemplo: `UbuntuServer`  
    O segmento de nome de oferta da imagem VM que os utilizadores utilizam quando implementam a imagem VM. Um exemplo é **WindowsServer**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **sku**  
    Por exemplo: `14.04.3-LTS`  
    O segmento de nome SKU da imagem de VM que os utilizadores utilizam quando implementam a imagem VM. Um exemplo é **Datacenter2016**. Não inclua um espaço ou outros carateres especiais neste campo.  
  - **Versão**  
    Por exemplo: `1.0.0`  
    A versão da imagem de VM que os utilizadores utilizam quando implementam a imagem VM. Esta versão está no formato  *\#.\#. \#*. Um exemplo é **1.0.0**. Não inclua um espaço ou outros carateres especiais neste campo.  
    
    Para obter mais informações sobre theRemove AzsPlatformImage cmdlet, consulte o PowerShell Microsoft [documentação do módulo de operador de pilha do Azure](https://docs.microsoft.com/powershell/module/).

## <a name="next-steps"></a>Passos Seguintes

[Aprovisionar uma máquina virtual](azure-stack-provision-vm.md)