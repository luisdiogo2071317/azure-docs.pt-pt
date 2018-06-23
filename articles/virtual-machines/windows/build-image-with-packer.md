---
title: Como criar imagens de VM do Windows Azure com Packer | Microsoft Docs
description: Saiba como utilizar Packer criar imagens de máquinas virtuais do Windows no Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/29/2018
ms.author: iainfou
ms.openlocfilehash: b7f07ec8736086483f91746512f10118ee90762d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333169"
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>Como utilizar Packer para criar imagens de máquina virtual do Windows no Azure
Cada máquina virtual (VM) no Azure é criada a partir de uma imagem que define a distribuição do Windows e a versão do SO. Imagens podem incluir aplicações pré-instaladas e configurações. No Azure Marketplace disponibiliza várias imagens primeira e de terceiros para mais comuns SO e ambientes de aplicação, ou pode criar as suas próprias imagens personalizadas e adaptadas às suas necessidades. Este artigo fornece detalhes sobre como utilizar a ferramenta de open source [Packer](https://www.packer.io/) para definir e criar imagens personalizadas no Azure.


## <a name="create-azure-resource-group"></a>Criar grupo de recursos do Azure
Durante o processo de compilação, Packer cria temporários recursos do Azure baseia-se a VM de origem. Para essa VM de origem para utilização como uma imagem de captura, tem de definir um grupo de recursos. A saída do processo de compilação de Packer é armazenada neste grupo de recursos.

Crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```powershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzureRmResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>Criar as credenciais do Azure
Packer autentica com o Azure utilizando um principal de serviço. Um principal de serviço do Azure é uma identidade de segurança que pode utilizar com aplicações, serviços e ferramentas de automatização como Packer. Controlar e definir as permissões para as operações que pode efetuar o principal de serviço no Azure.

Criar um serviço principal com [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) e atribuir permissões para o principal de serviço criar e gerir os recursos com [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```powershell
$sp = New-AzureRmADServicePrincipal -DisplayName "AzurePacker" `
    -Password (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force)
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Para autenticar para o Azure, também terá de obter os IDs de inquilino e subscrição do Azure com [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription):

```powershell
$sub = Get-AzureRmSubscription
$sub.TenantId[0]
$sub.SubscriptionId[0]
```

Utilize estes dois IDs no próximo passo.


## <a name="define-packer-template"></a>Definir Packer modelo
Para criar imagens, crie um modelo como um ficheiro JSON. O modelo, é possível definir construtores e provisioners realizar o processo de compilação real. Packer tem um [construtor para o Azure](https://www.packer.io/docs/builders/azure.html) que permite-lhe definir os recursos do Azure, tais como as credenciais de principal de serviço criadas no precedente passo.

Crie um ficheiro denominado *windows.json* e cole o seguinte conteúdo. Introduza os seus próprios valores para o seguinte:

| Parâmetro                           | Onde obtê |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | ID de principal de serviço de vista com `$sp.applicationId` |
| *client_secret*                     | Palavra-passe especificada no `$securePassword` |
| *tenant_id*                         | O resultado da `$sub.TenantId` comando |
| *subscription_id*                   | O resultado da `$sub.SubscriptionId` comando |
| *object_id*                         | ID de objeto principal do serviço de vista com `$sp.Id` |
| *managed_image_resource_group_name* | Nome do grupo de recursos que criou no primeiro passo |
| *managed_image_name*                | Nome para a imagem de disco gerido que é criada |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "0831b578-8ab6-40b9-a581-9a880a94aab1",
    "client_secret": "P@ssw0rd!",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "object_id": "a7dfb070-0d5b-47ac-b9a5-cf214fff0ae2",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": true,
    "winrm_insecure": true,
    "winrm_timeout": "3m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "& $env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit",
      "while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 10  } else { break } }"
    ]
  }]
}
```

Este modelo cria uma VM do Windows Server 2016, instala o IIS e generaliza a VM com o Sysprep. A instalação do IIS mostra como pode utilizar o provisioner do PowerShell para executar os comandos adicionais. A imagem de Packer final inclui, em seguida, a instalação do software necessário e a configuração.


## <a name="build-packer-image"></a>Compilar Packer imagem
Se ainda não tiver Packer instalado no seu computador local, [siga as instruções de instalação Packer](https://www.packer.io/docs/install/index.html).

Compilar a imagem, especificando o Packer ficheiro do modelo da seguinte forma:

```bash
./packer build windows.json
```

Segue-se um exemplo de saída a partir dos comandos do anteriores:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the certificate’s URL ...
==> azure-arm:  -> Key Vault Name        : ‘pkrkvpq0mthtbtt’
==> azure-arm:  -> Key Vault Secret Name : ‘packerKeyVaultSecret’
==> azure-arm:  -> Certificate URL       : ‘https://pkrkvpq0mthtbtt.vault.azure.net/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate’s URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.55.35’
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version=“1.1.0.1” xmlns=“http://schemas.microsoft.com/powershell/2004/04"><Obj S=“progress” RefId=“0"><TN RefId=“0”><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N=“SourceId”>1</I64><PR N=“Record”><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Compute Name              : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

Demora alguns minutos para Packer criar a VM, execute os provisioners e limpar a implementação.


## <a name="create-a-vm-from-the-packer-image"></a>Criar uma VM a partir da imagem Packer
Agora pode criar uma VM a partir da imagem com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Os suporte recursos de rede são criados, caso ainda não existam. Quando lhe for pedido, introduza um nome de utilizador administrativo e a palavra-passe a ser criado na VM. O exemplo seguinte cria uma VM chamada *myVM* de *myPackerImage*:

```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Image "myPackerImage"
```

Se pretender criar VMs num grupo de recursos diferente ou região que a imagem de Packer, especifique o ID de imagem em vez de um nome de imagem. Pode obter o ID de imagem com [Get-AzureRmImage](/powershell/module/AzureRM.Compute/Get-AzureRmImage).

Demora alguns minutos para criar a VM a partir da sua imagem Packer.


## <a name="test-vm-and-webserver"></a>Testar a VM e o servidor Web
Obtenha o endereço IP público da sua VM com [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). O exemplo seguinte obtém o endereço IP para *myPublicIP* criado anteriormente:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIPAddress" | select "IpAddress"
```

Para ver a sua VM, que inclui a instalação do IIS do Packer provisioner, na ação, introduza o endereço IP público para um web browser.

![Site predefinido do IIS](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a>Passos Seguintes
Neste exemplo, Packer que utilizou para criar uma imagem VM com o IIS já instalado. Pode utilizar esta imagem de VM em conjunto com fluxos de trabalho de implementação existentes, tal como para implementar a sua aplicação para VMs criadas a partir da imagem com o Team Services, Ansible, Chef ou Puppet.

Para modelos de Packer de exemplo adicionais para outros distros do Windows, consulte [este repositório do GitHub](https://github.com/hashicorp/packer/tree/master/examples/azure).
