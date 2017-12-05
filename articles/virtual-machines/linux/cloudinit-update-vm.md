---
title: Utilizar a cloud init para atualizar e instalar pacotes de uma VM com Linux no Azure | Microsoft Docs
description: "Como utilizar a cloud init para atualizar e instalar pacotes numa VM com Linux durante a criação com o 2.0 CLI do Azure"
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
ms.openlocfilehash: 4209bc270a6d255c8512dd6ccd5551b556da5a6b
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Utilizar a cloud init para atualizar e instalar pacotes de uma VM com Linux no Azure
Este artigo mostra como utilizar [nuvem init](https://cloudinit.readthedocs.io) para atualizar pacotes um Linux da máquina virtual (VM) ou dimensionamento da máquina virtual define (VMSS) no aprovisionamento de tempo no Azure. Estes scripts de nuvem init executam no primeiro arranque depois dos recursos foram aprovisionados através do Azure. Para obter mais informações sobre como nuvem init nativamente funciona no Azure e os distros suportados do Linux, consulte [nuvem init descrição-geral](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>Atualizar uma VM com init de nuvem
Por motivos de segurança, poderá querer configurar uma VM para aplicar as atualizações mais recentes no primeiro arranque. Como nuvem init funciona em diferentes distros do Linux, é necessário especificar `apt` ou `yum` para o Gestor de pacotes. Em vez disso, é possível definir `package_upgrade` e permitir que o processo de nuvem init determinar o mecanismo de adequado para distro em utilização. Este fluxo de trabalho permite-lhe utilizar os scripts de nuvem init mesmo em distros.

Para ver o processo de atualização em ação, crie um ficheiro na sua shell atual com o nome *cloud_init_upgrade.txt* e cole a seguinte configuração. Neste exemplo, crie o ficheiro na Shell na nuvem não no seu computador local. Pode utilizar qualquer editor que desejar. Introduza `sensible-editor cloud_init_upgrade.txt` para criar o ficheiro e ver uma lista de editores disponíveis. Escolha #1 para utilizar o **nano** editor. Certifique-se de que o ficheiro de toda a nuvem-init é copiado corretamente, especialmente a primeira linha.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Antes de implementar esta imagem, tem de criar um grupo de recursos com o [criar grupo az](/cli/azure/group#create) comando. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Agora, crie uma VM com [az vm criar](/cli/azure/vm#create) e especifique o ficheiro de nuvem init com `--custom-data cloud_init_upgrade.txt` da seguinte forma:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

SSH para o endereço IP público da sua VM apresentado no resultado do comando anterior. Introduza os seus próprios **publicIpAddress** da seguinte forma:

```bash
ssh <publicIpAddress>
```

Execute a ferramenta de gestão de pacote e procurar atualizações. O exemplo seguinte utiliza `apt-get` numa VM com Ubuntu:

```bash
sudo apt-get upgrade
```

Como nuvem init verificadas e atualizações instaladas no arranque, não deverá haver nenhum atualizações a aplicar, conforme mostrado no seguinte exemplo de saída:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

Também pode ver que `httpd` foi instalado através da execução `yum history` e reveja o tipo de referência de saída `httpd`. 

## <a name="next-steps"></a>Passos seguintes
Para obter exemplos adicionais nuvem-init das alterações de configuração, consulte o seguinte:
 
- [Adicionar um utilizador de Linux adicional para uma VM](cloudinit-add-user.md)
- [Executar um Gestor de pacote para atualizar pacotes existentes no primeiro arranque](cloudinit-update-vm.md)
- [Alterar o nome local do anfitrião de VM](cloudinit-update-vm-hostname.md) 
- [Instalar um pacote de aplicações, ficheiros de configuração de atualização e injetar chaves](tutorial-automate-vm-deployment.md)