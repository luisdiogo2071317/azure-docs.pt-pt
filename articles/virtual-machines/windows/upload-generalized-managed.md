---
title: Criar uma VM do Azure gerida a partir de um VHD generalizado no local | Microsoft Docs
description: Carregar um VHD generalizado para o Azure e utilizá-la para criar novas VMs, no modelo de implementação Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/26/2018
ms.author: cynthn
ms.openlocfilehash: cf9255126adcec9a2d9d280211e46a6b5f93e14c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Carregar um VHD generalizado e utilizá-la para criar novas VMs no Azure

Este tópico explica como utilizar o PowerShell para carregar um VHD de uma VM generalizado para o Azure, criar uma imagem do VHD e crie uma nova VM a partir dessa imagem. Pode carregar um VHD exportado a partir de uma ferramenta de virtualização no local ou de outra nuvem. Utilizar [discos geridos](managed-disks-overview.md) para a nova VM simplifica a gestão de VM e fornece uma melhor disponibilidade quando a VM está colocada num conjunto de disponibilidade. 

Se pretender utilizar um script de exemplo, consulte [script para carregar um VHD para o Azure e criar uma nova VM de exemplo](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Antes de começar

- Antes de carregar qualquer VHD para o Azure, deve seguir [preparar um VHD do Windows ou o VHDX para carregar para o Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Reveja [planear a migração para discos geridos](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) antes de iniciar a migração para [discos geridos](managed-disks-overview.md).
- Este artigo requer o AzureRM versão do módulo 5.6 ou posterior. Executar ` Get-Module -ListAvailable AzureRM.Compute` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).


## <a name="generalize-the-source-vm-using-sysprep"></a>Generalize a VM de origem com o Sysprep

Sysprep remove todas as informações pessoais da conta, entre outras coisas e prepara a máquina para ser utilizado como uma imagem. Para obter detalhes sobre o Sysprep, consulte [como Sysprep de utilização: uma introdução](http://technet.microsoft.com/library/bb457073.aspx).

Certifique-se as funções de servidor em execução na máquina são suportadas pelo Sysprep. Para obter mais informações, consulte [suporte de Sysprep para funções de servidor](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Se estiver a executar o Sysprep antes de carregar o VHD para o Azure pela primeira vez, certifique-se de que tem [preparado VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) antes de executar o Sysprep. 
> 
> 

1. Iniciar sessão para a máquina virtual do Windows.
2. Abra a janela de linha de comandos como administrador. Altere o diretório para **%windir%\system32\sysprep**e, em seguida, execute `sysprep.exe`.
3. No **ferramenta de preparação de sistema** caixa de diálogo, selecione **introduza sistema Out-of-Box experiência (OOBE)**e certifique-se de que o **Generalize** caixa de verificação está selecionada.
4. No **as opções de encerramento**, selecione **encerramento**.
5. Clique em **OK**.
   
    ![Iniciar o Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Quando tiver concluído o Sysprep, encerrar a máquina virtual. Não reinicie a VM.


## <a name="get-the-storage-account"></a>Obter a conta de armazenamento

Necessita de uma conta de armazenamento no Azure para armazenar a imagem VM carregada. Pode utilizar uma conta de armazenamento existente ou crie um novo. 

Se planear utilizar o VHD para criar um disco gerido para uma VM, a localização da conta de armazenamento tem de ser igual a localização onde irá criar a VM.

Para mostrar as contas de armazenamento disponíveis, escreva:

```azurepowershell
Get-AzureRmStorageAccount | Format-Table
```

## <a name="upload-the-vhd-to-your-storage-account"></a>Carregar o VHD à sua conta de armazenamento

Utilize o [adicionar AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet para carregar o VHD para um contentor na sua conta de armazenamento. Neste exemplo carrega o ficheiro *myVHD.vhd* de *"discos de rígido C:\Users\Public\Documents\Virtual\"*  para uma conta de armazenamento com o nome *mystorageaccount* no o *myResourceGroup* grupo de recursos. O ficheiro será colocado no contentor com o nome *mycontainer* e o novo nome de ficheiro será *myUploadedVHD.vhd*.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Se tiver êxito, receberá uma resposta semelhante a isto:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Dependendo da sua ligação de rede e o tamanho do ficheiro VHD, este comando pode demorar algum tempo para concluir

### <a name="other-options-for-uploading-a-vhd"></a>Outras opções para carregar um VHD
 
Também pode carregar um VHD para a conta de armazenamento com um dos seguintes:

- [AZCopy](http://aka.ms/downloadazcopy)
- [Blob de cópia de armazenamento do Azure API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Blobs de carregamento de Explorador de armazenamento do Azure](https://azurestorageexplorer.codeplex.com/)
- [Referência de API de REST de serviço de importação/exportação de armazenamento](https://msdn.microsoft.com/library/dn529096.aspx)
-   Recomendamos que utilize o serviço de importação/exportação se estimado tempo de carregamento tem mais de 7 dias. Pode utilizar [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) para estimar o tempo de unidade de tamanho e a transferência de dados. 
    Importar/exportar podem ser utilizado para copiar para uma conta de armazenamento standard. Terá de copiar a partir de armazenamento standard para a conta do premium storage utilizando uma ferramenta como o AzCopy.

> [!IMPORTANT]
> Se estiver a utilizar o AzCopy carregar o VHD para o Azure, certifique-se de que definiu [/BlobType:page](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) antes de executar carregar o script. Se o destino for um blob e esta opção não for especificada, por predefinição, o AzCopy cria um blob de blocos.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Criar uma imagem gerida do VHD foi carregado 

Crie uma imagem gerida utilizando o VHD de SO generalizado. Substitua os valores pelas suas informações.


Em primeiro lugar, defina os alguns parâmetros:

```powershell
$location = "East US" 
$imageName = "myImage"
```

Crie a imagem utilizando o VHD de SO generalizado.

```powershell
$imageConfig = New-AzureRmImageConfig `
   -Location $location
$imageConfig = Set-AzureRmImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzureRmImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>Crie a VM

Agora que tem uma imagem, pode criar uma ou mais VMs novas partir da imagem. Este exemplo cria uma VM chamada *myVM* do *myImage*, no *myResourceGroup*.


```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -ImageName $imageName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>Passos Seguintes

Para iniciar sessão sua nova máquina virtual, navegue para a VM com o [portal](https://portal.azure.com), clique em **Connect**e abra o ficheiro RDP de ambiente de trabalho remoto. Utilize as credenciais da conta da máquina virtual original para iniciar sessão sua nova máquina virtual. Para obter mais informações, consulte [como ligar e iniciar sessão a uma máquina virtual do Azure com o Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

