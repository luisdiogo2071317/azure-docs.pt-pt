---
title: Utilizar init da nuvem para configurar um swapfile uma VM com Linux | Microsoft Docs
description: "Como utilizar a cloud init para configurar um swapfile uma VM com Linux durante a criação com o 2.0 CLI do Azure"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: a8ccec0dc8ff100c5d067cd50f2a6fa8cb4871fb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-configure-a-swapfile-on-a-linux-vm"></a>Utilizar init da nuvem para configurar um swapfile uma VM com Linux
Este artigo mostra como utilizar [nuvem init](https://cloudinit.readthedocs.io) para configurar o swapfile em várias distribuições em Linux. O swapfile tradicionalmente foi configurado pelo Linux agente (WALA) com base nas quais as distribuições necessário um.  Este documento será descrevem o processo de criação de swapfile a pedido durante o aprovisionamento vez utilizando init de nuvem.  Para obter mais informações sobre como nuvem init nativamente funciona no Azure e os distros suportados do Linux, consulte [nuvem init descrição-geral](using-cloud-init.md)

## <a name="create-swapfile-for-ubuntu-based-images"></a>Criar swapfile Ubuntu baseado em imagens
Por predefinição no Azure, as imagens de galeria Ubuntu não criar ficheiros de comutação. Ativar a configuração do ficheiro de troca durante a hora com a nuvem init de aprovisionamento de VM-consulte o [AzureSwapPartitions documento](https://wiki.ubuntu.com/AzureSwapPartitions) no Ubuntu wiki.

## <a name="create-swapfile-for-redhat-and-centos-based-images"></a>Criar swapfile para imagens baseada em VM de RedHat e CentOS

Criar um ficheiro na sua shell atual com o nome *cloud_init_swapfile.txt* e cole a seguinte configuração. Neste exemplo, crie o ficheiro na Shell na nuvem não no seu computador local. Pode utilizar qualquer editor que desejar. Introduza `sensible-editor cloud_init_swapfile.txt` para criar o ficheiro e ver uma lista de editores disponíveis. Escolha #1 para utilizar o **nano** editor. Certifique-se de que o ficheiro de toda a nuvem-init é copiado corretamente, especialmente a primeira linha.  

```yaml
#cloud-config
disk_setup:
ephemeral0:
table_type: gpt
layout: [66, [33,82]]
overwrite: True
fs_setup:
- device: ephemeral0.1
filesystem: ext4
- device: ephemeral0.2
filesystem: swap
mounts:
- ["ephemeral0.1", "/mnt"]
- ["ephemeral0.2", "none", "swap", "sw", "0", "0"]
```

Antes de implementar esta imagem, tem de criar um grupo de recursos com o [criar grupo az](/cli/azure/group#create) comando. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Agora, crie uma VM com [az vm criar](/cli/azure/vm#create) e especifique o ficheiro de nuvem init com `--custom-data cloud_init_swapfile.txt` da seguinte forma:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_swapfile.txt \
  --generate-ssh-keys 
```

## <a name="verify-swapfile-was-created"></a>Certifique-se de que foi criado swapfile
SSH para o endereço IP público da sua VM apresentado no resultado do comando anterior. Introduza os seus próprios **publicIpAddress** da seguinte forma:

```bash
ssh <publicIpAddress>
```

Assim que tiver SSH'ed para a vm, verifique se o swapfile foi criado

```bash
swapon -s
```

O resultado deste comando deve ter o seguinte aspeto:

```text
Filename                Type        Size    Used    Priority
/dev/sdb2  partition   2494440 0   -1
```

> [!NOTE] 
> Se tiver uma imagem do Azure existente que tenha um ficheiro de comutação configurado e que pretende alterar a configuração do ficheiro de comutação para novas imagens, deve remover o ficheiro de troca existente. Consulte 'Personalizar imagens de aprovisionar por nuvem init' documento para obter mais detalhes.

## <a name="next-steps"></a>Passos seguintes
Para obter exemplos adicionais nuvem-init das alterações de configuração, consulte o seguinte:
 
- [Adicionar um utilizador de Linux adicional para uma VM](cloudinit-add-user.md)
- [Executar um Gestor de pacote para atualizar pacotes existentes no primeiro arranque](cloudinit-update-vm.md)
- [Alterar o nome local do anfitrião de VM](cloudinit-update-vm-hostname.md) 
- [Instalar um pacote de aplicações, ficheiros de configuração de atualização e injetar chaves](tutorial-automate-vm-deployment.md)