---
title: Utilize nuvem init para adicionar um utilizador para uma VM com Linux no Azure | Microsoft Docs
description: "Como utilizar a cloud init para adicionar um utilizador para uma VM com Linux durante a criação com o 2.0 CLI do Azure"
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
ms.openlocfilehash: 728ffed27747cb298d5da312014a3c9e98b44f1e
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-add-a-user-to-a-linux-vm-in-azure"></a>Utilize nuvem init para adicionar um utilizador para uma VM com Linux no Azure
Este artigo mostra como utilizar [nuvem init](https://cloudinit.readthedocs.io) para adicionar um utilizador de uma máquina virtual ou máquina virtual (VM) conjuntos de dimensionamento (VMSS) no aprovisionamento de tempo no Azure. Este script de nuvem init é executado no primeiro arranque depois dos recursos foram aprovisionados através do Azure. Para obter mais informações sobre como nuvem init nativamente funciona no Azure e os distros suportados do Linux, consulte [descrição geral da nuvem init](using-cloud-init.md).

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Adicionar um utilizador para uma VM com init de nuvem
Uma das tarefas primeiro qualquer nova VM do Linux é adicionar um utilizador adicional para si próprio evitar a utilização de *raiz*. As chaves SSH são a melhor prática de segurança e facilidade de utilização. As chaves são adicionadas para o *~/.ssh/authorized_keys* ficheiro com este script de nuvem init.

Para adicionar um utilizador para uma VM com Linux, crie um ficheiro na sua shell atual com o nome *cloud_init_add_user.txt* e cole a seguinte configuração. Neste exemplo, crie o ficheiro na Shell na nuvem não no seu computador local. Pode utilizar qualquer editor que desejar. Introduza `sensible-editor cloud_init_add_user.txt` para criar o ficheiro e ver uma lista de editores disponíveis. Escolha #1 para utilizar o **nano** editor. Certifique-se de que o ficheiro de toda a nuvem-init é copiado corretamente, especialmente a primeira linha.  Tem de fornecer a sua própria chave pública (tais como o conteúdo do *~/.ssh/id_rsa.pub*) para o valor da `ssh-authorized-keys:` -tenha sido shortened aqui para simplificar o exemplo.

```yaml
#cloud-config
users:
  - default
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```
> [!NOTE] 
> O ficheiro de #cloud-config inclui o `- default` parâmetro incluído. Isto irá acrescentar o utilizador, para o utilizador de admin existente criado durante o aprovisionamento. Se criar um utilizador sem o `- default` parâmetro - o utilizador de admin gerado automaticamente criado pela plataforma do Azure será substituído. 

Antes de implementar esta imagem, tem de criar um grupo de recursos com o [criar grupo az](/cli/azure/group#create) comando. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Agora, crie uma VM com [az vm criar](/cli/azure/vm#create) e especifique o ficheiro de nuvem init com `--custom-data cloud_init_add_user.txt` da seguinte forma:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_add_user.txt \
  --generate-ssh-keys 
```

SSH para o endereço IP público da sua VM apresentado no resultado do comando anterior. Introduza os seus próprios **publicIpAddress** da seguinte forma:

```bash
ssh <publicIpAddress>
```

Para confirmar que o utilizador foi adicionado para a VM e os grupos especificados, ver o conteúdo do *etc/grupo* ficheiro da seguinte forma:

```bash
cat /etc/group
```

O resultado de exemplo seguinte mostra o utilizador a *cloud_init_add_user.txt* ficheiro foi adicionado para a VM e o grupo adequado:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Passos seguintes
Para obter exemplos adicionais nuvem-init das alterações de configuração, consulte o seguinte:
 
- [Adicionar um utilizador de Linux adicional para uma VM](cloudinit-add-user.md)
- [Executar um Gestor de pacote para atualizar pacotes existentes no primeiro arranque](cloudinit-update-vm.md)
- [Alterar o nome local do anfitrião de VM](cloudinit-update-vm-hostname.md) 
- [Instalar um pacote de aplicações, ficheiros de configuração de atualização e injetar chaves](tutorial-automate-vm-deployment.md)