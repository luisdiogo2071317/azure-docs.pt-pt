---
title: Implementar uma VM a partir dos VHDs para o Azure Marketplace | Documentos da Microsoft
description: Explica como registar uma VM a partir de um VHD implementadas no Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 2771549af29b3e717d117afb42de6db03fbee226
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639942"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Implementar uma VM a partir dos VHDs

Este artigo explica como registar-se de uma máquina virtual (VM) a partir de um Azure implementadas com o disco rígido virtual (VHD).  Ele lista as ferramentas necessárias e como utilizá-los para criar uma imagem de VM do utilizador, em seguida, implementá-la para o Azure através da [portal do Microsoft Azure](https://ms.portal.azure.com/) ou scripts do PowerShell. 

Depois de carregar os discos rígidos virtuais (VHDs) — o sistema de operativo VHD generalizado e zero ou mais VHDs de discos, à sua conta de armazenamento do Azure, pode registá-los como uma imagem de VM do utilizador. Em seguida, pode testar essa imagem. Uma vez que o sistema de operativo VHD é generalizado, é possível implementar diretamente a VM, fornecendo o URL de VHD.

Para mais informações sobre imagens VM, consulte as seguintes mensagens de blogue:

- [Imagem de VM](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [PowerShell de imagem VM "Como"](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)


## <a name="set-up-the-necessary-tools"></a>Configurar as ferramentas necessárias

Se ainda não o fez, instale o Azure PowerShell e CLI do Azure, com as instruções seguintes:

<!-- TD: Change the following URLs (in this entire topic) to relative paths.-->

- [Instalar o Azure PowerShell no Windows com o PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)
- [Instalar a CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="create-a-user-vm-image"></a>Criar uma imagem de VM do utilizador

Em seguida, irá criar uma imagem não gerida a partir de seu VHD generalizado.

#### <a name="capture-the-vm-image"></a>Capturar a imagem VM

Utilize as instruções no artigo sobre como capturar a VM que corresponde à sua abordagem de acesso:

-  PowerShell: [como criar uma imagem VM não gerida a partir de uma VM do Azure](../../../virtual-machines/windows/capture-image-resource.md)
-  CLI do Azure: [como criar uma imagem de uma máquina virtual ou VHD](../../../virtual-machines/linux/capture-image.md)
-  API: [capturar de máquinas virtuais -](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

### <a name="generalize-the-vm-image"></a>Generalize a imagem VM

Uma vez que tê gerou a imagem do utilizador a partir de um VHD generalizado anteriormente, também deve ser generalizado.  Mais uma vez, selecione o seguinte artigo que corresponde ao seu mecanismo de acesso.  (Pode ter já generalizada seu disco quando capturou isso.)

-  PowerShell: [generalizar a VM](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  CLI do Azure: [passo 2: imagem de VM de criar](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [generalizar de máquinas virtuais -](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="deploy-a-vm-from-a-user-vm-image"></a>Implementar uma VM a partir de uma imagem VM do utilizador

Em seguida, irá implementar uma VM a partir de uma imagem de VM do utilizador, através do portal do Azure ou PowerShell.

<!-- TD: Recapture following hilited images and replace with red-box. -->

### <a name="deploy-a-vm-from-azure-portal"></a>Implementar uma VM a partir do portal do Azure

Utilize o seguinte processo para implementar a sua VM de utilizador do portal do Azure.

1.  Inicie sessão no [Portal do Azure](https://portal.azure.com).

2.  Clique em **New** e procure **implementação do modelo**, em seguida, selecione **criar seu próprio modelo no Editor**.  <br/>
  ![Criar modelo de implementação do VHD no portal do Azure](./media/publishvm_021.png)

3. Copiar e colar isto [modelo JSON](./cpp-deploy-json-template.md) para o editor e clique em **guardar**. <br/>
  ![Guardar o modelo de implementação do VHD no portal do Azure](./media/publishvm_022.png)

4. Forneça os valores de parâmetro para apresentados **implementação personalizada** páginas de propriedades.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parâmetro**              |   **Descrição**                                                            |
   |  -------------              |   ---------------                                                            |
   | Nome de conta de armazenamento do utilizador   | Nome de conta de armazenamento onde está localizado um VHD generalizado                    |
   | Nome do contentor de armazenamento de usuário | Nome do contentor onde está localizado um VHD generalizado                          |
   | Nome DNS para o IP público      | Nome DNS de IP público                                                           |
   | Nome de utilizador administrador             | Nome de utilizador da conta de administrador para a nova VM                                  |
   | Palavra-passe de Administrador              | Palavra-passe de conta de administrador para a nova VM                                  |
   | Tipo de SO                     | Sistema de operativo da VM: `Windows` \| `Linux`                                    |
   | ID da subscrição             | Identificador da subscrição selecionada                                      |
   | Localização                    | Localização geográfica da implementação                                        |
   | Tamanho da VM                     | [Tamanho VM do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), por exemplo `Standard_A2` |
   | Nome do endereço IP público      | Nome do seu endereço IP público                                               |
   | Nome da VM                     | Nome da nova VM                                                           |
   | Nome da rede virtual        | Nome da rede virtual utilizada pela VM                                   |
   | Nome do NIC                    | Nome do cartão de interface de rede com a rede virtual               |
   | URL DE VHD                     | Concluir o URL de VHD de disco do SO                                                     |
   |  |  |
            
5. Depois de fornecer estes valores, clique em **Compra**. 

Azure começará a implementação: ele cria uma nova VM com o VHD não gerido especificado, no caminho de conta de armazenamento especificada.  Pode acompanhar o progresso no portal do Azure ao clicar em **máquinas virtuais** no lado esquerdo do portal.  Quando a VM tiver sido criada, o estado será alterado de `Starting` para `Running`. 


### <a name="deploy-a-vm-from-powershell"></a>Implementar uma VM a partir do PowerShell

Para implementar uma VM grande a partir da imagem de VM generalizada acabou de criar, utilize os seguintes cmdlets.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

<!-- TD: The following is a marketplace-publishing article and may be out-of-date.  TD: update and move topic.
For help with issues, see [Troubleshooting common issues encountered during VHD creation](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting) for additional assistance.
-->

## <a name="next-steps"></a>Passos Seguintes

Depois da VM é implementada, está pronto para [configurar a VM](./cpp-configure-vm.md).
