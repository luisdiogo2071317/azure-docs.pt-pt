---
title: Capturar uma imagem de VM do Linux no Azure com a CLI 2.0 | Documentos da Microsoft
description: Capture uma imagem de VM do Azure a utilizar para implementações em massa com a CLI 2.0 do Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2018
ms.author: cynthn
ms.openlocfilehash: ea202cad06130cfaaa134cad94ac08bede2f41a9
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867706"
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Como criar uma imagem de uma máquina virtual ou VHD

<!-- generalize, image - extended version of the tutorial-->

Para criar várias cópias de uma máquina virtual (VM) para utilizar no Azure, capture uma imagem de VM ou o VHD do SO. Para criar uma imagem, tem de remover as informações de conta pessoal que torna mais seguro implementar várias vezes. Nos passos seguintes desaprovisionar uma VM existente, desalocar e criar uma imagem. Pode utilizar esta imagem para criar VMs em qualquer grupo de recursos na sua subscrição.

Se quiser criar uma cópia da sua VM do Linux existente para a cópia de segurança ou de depuração, ou carregar um VHD especializado do Linux a partir de uma VM no local, veja [carregar e criar uma VM do Linux a partir de imagem de disco personalizada](upload-vhd.md).  

Também pode utilizar **Packer** para criar a configuração personalizada. Para obter mais informações sobre como utilizar o Packer, consulte [como utilizar o Packer para criar imagens de máquinas virtuais do Linux no Azure](build-image-with-packer.md).


## <a name="before-you-begin"></a>Antes de começar
Certifique-se de que cumpre os seguintes pré-requisitos:

* Precisa de uma VM do Azure criados no modelo de implementação do Resource Manager a utilizar discos geridos. Se ainda não criou uma VM do Linux, pode utilizar o [portal](quick-create-portal.md), o [CLI do Azure](quick-create-cli.md), ou [modelos do Resource Manager](create-ssh-secured-vm-from-template.md). Configure a VM conforme necessário. Por exemplo, [adicionar discos de dados](add-disk.md), aplicar atualizações e instalar aplicações. 

* Também tem de ter a versão mais recente [CLI do Azure 2.0](/cli/azure/install-az-cli2) instalado e de ter sessão iniciada para uma conta do Azure com [início de sessão az](/cli/azure/reference-index#az_login).

## <a name="quick-commands"></a>Comandos rápidos

Para uma versão simplificada neste tópico, para fins de teste, avaliar ou aprender sobre as VMs no Azure, consulte [criar uma imagem personalizada de uma VM do Azure com a CLI](tutorial-custom-images.md).


## <a name="step-1-deprovision-the-vm"></a>Passo 1: Desaprovisionar a VM
Desaprovisionar a VM, com o agente de VM do Azure, para eliminar ficheiros específicos da máquina e dados. Utilize o `waagent` comando com o *-desaprovisionamento + utilizador* parâmetro na sua VM do Linux de origem. Para obter mais informações, veja o [Guia de utilizador do Agente Linux do Azure](../extensions/agent-linux.md).

1. Ligar à sua VM do Linux com um cliente SSH.
2. Na janela do SSH, escreva o seguinte comando:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Apenas execute este comando numa VM que pretende capturar como uma imagem. Não garante que a imagem será removida de todas as informações confidenciais ou se é adequada para redistribuição. O *+ utilizador* parâmetro também remove a última conta de utilizador aprovisionado. Se pretender manter as credenciais da conta na VM, utilize *-desaprovisionamento* deixar a conta de utilizador no local.
 
3. Tipo **y** para continuar. Pode adicionar os **-forçar** parâmetro para evitar esta etapa de confirmação.
4. Depois do comando for concluído, escreva **sair**. Este passo fecha o cliente SSH.

## <a name="step-2-create-vm-image"></a>Passo 2: Criar a imagem de VM
Utilize a CLI 2.0 do Azure para marcar a VM como generalizado e capturar a imagem. Nos exemplos a seguir, substitua os nomes de parâmetros de exemplo pelos seus próprios valores. Os nomes de parâmetros de exemplo incluem *myResourceGroup*, *myVnet*, e *myVM*.

1. Desaloque a VM que desaprovisionado com [az vm deallocate](/cli//azure/vm#deallocate). O exemplo seguinte desaloca a VM com o nome *myVM* no grupo de recursos com o nome *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. Marque a VM como generalizado com [az vm generalize](/cli//azure/vm#generalize). O exemplo seguinte marca a VM com o nome *myVM* no grupo de recursos com o nome *myResourceGroup* como generalizada:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Agora, crie uma imagem do recurso VM com o [criar imagem de az](/cli/azure/image#az_image_create). O exemplo seguinte cria uma imagem designada *myImage* no grupo de recursos com o nome *myResourceGroup* usando o recurso da VM com o nome *myVM*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > A imagem é criada no mesmo grupo de recursos como a VM de origem. Pode criar VMs em qualquer grupo de recursos na sua subscrição a partir desta imagem. Da perspectiva de gestão, pode pretender criar um grupo de recursos específicos para os seus recursos da VM e imagens.
   >
   > Se gostaria de armazenar a imagem no armazenamento resiliente para a zona, terá de criá-la numa região que suporta [zonas de disponibilidade](../../availability-zones/az-overview.md) e incluir o `--zone-resilient true` parâmetro.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Passo 3: Criar uma VM a partir da imagem capturada
Criar uma VM com a imagem criada com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM com o nome *myVMDeployed* da imagem com o nome *myImage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>Criar a VM em outro grupo de recursos 

Pode criar VMs a partir de uma imagem em qualquer grupo de recursos na sua subscrição. Para criar uma VM no grupo de recursos diferente que a imagem, especifica o ID de recurso completo para a sua imagem. Uso [lista de imagens de az](/cli/azure/image#az_image_list) para ver uma lista de imagens. O resultado é semelhante ao seguinte exemplo:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

O exemplo seguinte utiliza [az vm criar](/cli/azure/vm#az_vm_create) para criar uma VM no grupo de recursos diferente que a imagem de origem especificando o ID de recurso de imagem:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>Passo 4: Verificar a implementação

Agora SSH para a máquina virtual que criou para verificar a implementação e comece a utilizar a nova VM. Para ligar através de SSH, encontrar o endereço IP ou FQDN da VM com [show de vm de az](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Passos Seguintes
Pode criar várias VMs a partir da imagem de VM de origem. Se precisar de efetuar alterações à sua imagem: 

- Crie uma VM a partir da sua imagem.
- Torne as atualizações ou alterações de configuração.
- Siga os passos novamente para desaprovisionar, desalocar, generalize e criar uma imagem.
- Utilize esta imagem de novo para as futuras Implantações. Se assim o desejar, elimine a imagem original.

Para obter mais informações sobre como gerir as suas VMs com a CLI, veja [CLI 2.0 do Azure](/cli/azure).
