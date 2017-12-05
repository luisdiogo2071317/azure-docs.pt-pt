---
title: "Utilize init de nuvem para definir o nome de anfitrião para uma VM com Linux no Azure | Microsoft Docs"
description: "Como utilizar a cloud init para personalizar uma VM com Linux durante a criação com o 2.0 CLI do Azure"
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
ms.openlocfilehash: 7455748b2e08104dadfed8cfe41581a400539d4f
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-set-hostname-for-a-linux-vm-in-azure"></a>Utilize init de nuvem para definir o nome de anfitrião para uma VM com Linux no Azure
Este artigo mostra como utilizar [nuvem init](https://cloudinit.readthedocs.io) para configurar um nome de anfitrião específico numa máquina virtual ou máquina virtual (VM) conjuntos de dimensionamento (VMSS) no aprovisionamento de tempo no Azure. Estes scripts de nuvem init executam no primeiro arranque depois dos recursos foram aprovisionados através do Azure. Para obter mais informações sobre como nuvem init nativamente funciona no Azure e os distros suportados do Linux, consulte [nuvem init descrição-geral](using-cloud-init.md)

## <a name="set-the-hostname-with-cloud-init"></a>Definir o nome de anfitrião com init de nuvem
Por predefinição, o nome do anfitrião é o mesmo que o nome da VM quando criar uma nova máquina virtual no Azure.  Para executar um script de nuvem init para alterar este nome de anfitrião predefinido quando cria uma VM no Azure com [az vm criar](/cli/azure/vm#create), especifique o ficheiro de init de nuvem com o `--custom-data` mudar.  

Para ver o processo de atualização em ação, crie um ficheiro na sua shell atual com o nome *cloud_init_hostname.txt* e cole a seguinte configuração. Neste exemplo, crie o ficheiro na Shell na nuvem não no seu computador local. Pode utilizar qualquer editor que desejar. Introduza `sensible-editor cloud_init_hostname.txt` para criar o ficheiro e ver uma lista de editores disponíveis. Escolha #1 para utilizar o **nano** editor. Certifique-se de que o ficheiro de toda a nuvem-init é copiado corretamente, especialmente a primeira linha.  

```yaml
#cloud-config
hostname: myhostname
```

Antes de implementar esta imagem, tem de criar um grupo de recursos com o [criar grupo az](/cli/azure/group#create) comando. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Agora, crie uma VM com [az vm criar](/cli/azure/vm#create) e especifique o ficheiro de nuvem init com `--custom-data cloud_init_hostname.txt` da seguinte forma:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_hostname.txt \
  --generate-ssh-keys 
```

Depois de criado, a CLI do Azure mostra informações sobre a VM. Utilize o `publicIpAddress` para SSH para a VM. Introduza o seu próprio endereço da seguinte forma:

```bash
ssh <publicIpAddress>
```

Para ver o nome da VM, utilize o `hostname` comando da seguinte forma:

```bash
hostname
```

A VM deve reportar o nome do anfitrião como esse valor definido no ficheiro de nuvem init, conforme mostrado no seguinte exemplo de saída:

```bash
myhostname
```

## <a name="next-steps"></a>Passos seguintes
Para obter exemplos adicionais nuvem-init das alterações de configuração, consulte o seguinte:
 
- [Adicionar um utilizador de Linux adicional para uma VM](cloudinit-add-user.md)
- [Executar um Gestor de pacote para atualizar pacotes existentes no primeiro arranque](cloudinit-update-vm.md)
- [Alterar o nome local do anfitrião de VM](cloudinit-update-vm-hostname.md) 
- [Instalar um pacote de aplicações, ficheiros de configuração de atualização e injetar chaves](tutorial-automate-vm-deployment.md)