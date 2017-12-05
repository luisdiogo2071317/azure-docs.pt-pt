---
title: "Preparar a imagem de VM do Azure para utilização com a nuvem init | Microsoft Docs"
description: "Como preparar uma imagem de VM do Azure pré-existente para implementação com init de nuvem"
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
ms.openlocfilehash: 2eb7510d4e76e4996e83f351a62c0b025b487df2
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Preparar uma imagem de VM do Linux do Azure existente para utilização com init de nuvem
Este artigo mostra como colocar uma máquina virtual do Azure existente e prepará-la reimplementado e pronto a utilizar a cloud init. A imagem resultante pode ser utilizada para implementar uma nova máquina virtual ou conjuntos de dimensionamento de máquina virtual - o de que foi, em seguida, ser personalizado por nuvem init no momento da implementação.  Estes scripts de nuvem init executam no primeiro arranque depois dos recursos foram aprovisionados através do Azure. Para obter mais informações sobre como nuvem init nativamente funciona no Azure e os distros suportados do Linux, consulte [nuvem init descrição-geral](using-cloud-init.md)

## <a name="prerequisites"></a>Pré-requisitos
Este documento assume que já tem uma máquina virtual do Azure em execução com uma versão suportada do sistema operativo Linux. Que já tenha configurado a máquina de acordo com as suas necessidades, instalado todos os módulos necessários, processado todas as atualizações necessárias e ter sido testado para se certificar de que cumpre os requisitos. 

## <a name="preparing-rhel-74--centos-74"></a>A preparar RHEL 7.4 / CentOS 7.4
Tem de SSH para a VM com Linux e execute os seguintes comandos para instalar o init de nuvem.

```bash
sudo yum install -y cloud-init gdisk
sudo yum check-update cloud-init -y
sudo yum install cloud-init -y
```

Atualização do `cloud_init_modules` secção `/etc/cloud/cloud.cfg` para incluir os seguintes módulos:
```bash
- disk_setup
- mounts
```

Eis um exemplo do que um para fins gerais `cloud_init_modules` secção aspeto.
```bash
 cloud_config_modules:
 - mounts
 - locale
 - set-passwords
 - rh_subscription
 - yum-add-repo
 - package-update-upgrade-install
 - timezone
 - puppet
 - chef
 - salt-minion
 - mcollective
 - disable-ec2-metadata
 - runcmd
 - disk_setup
 - mounts
```
Precisa de um número de tarefas relacionadas com o aprovisionamento e processamento discos efémeras seja actualizado no `/etc/waagent.conf`. Execute os seguintes comandos para atualizar as definições apropriadas. 
```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
```
Permitir apenas Azure como uma origem de dados para o agente Linux do Azure através da criação de um novo ficheiro `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg` utilizando um editor à sua escolha com as seguintes linhas:

```bash
# This configuration file is provided by the WALinuxAgent package.
datasource_list: [ Azure ]
```

Se a imagem do Azure existente tem um ficheiro de comutação configurado e que pretende alterar a configuração do ficheiro de comutação para novas imagens utilizando init de nuvem, terá de remover o ficheiro de troca existente.

Para uma VM de RedHat baseada em imagens - siga as instruções em explicar documento VM de RedHat seguintes como [remova o ficheiro de troca](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Deployment_Guide/s2-swap-removing-file.html).

Para imagens de CentOS com swapfile ativado, pode executar o comando seguinte para desativar o swapfile:
```bash
sudo swapoff /mnt/resource/swapfile
```

Certifique-se de que a referência de swapfile é removida do `/etc/fstab` -deve ser algo semelhante a seguinte saída:
```text
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

Para guardar espaço e remova o ficheiro de troca pode executar o seguinte comando:
```bash
rm /mnt/resource/swapfile
```
## <a name="extra-step-for-cloud-init-prepared-image"></a>Passo adicional para a nuvem init preparado imagem
> [!NOTE]
> Se a imagem era anteriormente um **nuvem init** imagem preparada e configurada, terá de efetuar os passos seguintes.

Os seguintes três comandos são utilizados apenas se a VM estiver a personalizar para uma nova imagem de origem especializadas foi aprovisionada anteriormente por nuvem init.  Não é necessário executar estes se a imagem foi configurada com o agente Linux do Azure.

```bash
sudo rm -rf /var/lib/cloud/instances/* 
sudo rm -rf /var/log/cloud-init*
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>A finalizar o agente Linux definição 
Todas as imagens de plataforma do Azure têm o agente Linux do Azure instalado, independentemente se foi configurado por nuvem init ou não.  Execute o seguinte comando para concluir o desaprovisionamento o utilizador a partir do computador Linux. 

```bash
sudo waagent -deprovision+user -force
```

Para obter mais informações sobre os comandos de desaprovisionamento agente Linux do Azure, consulte o [agente Linux do Azure](agent-user-guide.md) para obter mais detalhes.

Sair da sessão SSH e, em seguida, na shell de deteção, execute os seguintes comandos de AzureCLI desalocar, generalize e criar uma nova imagem de VM do Azure.  Substitua `myResourceGroup` e `sourceVmName` com as informações adequadas ao refletir o seu sourceVM.

```bash
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Passos seguintes
Para obter exemplos adicionais nuvem-init das alterações de configuração, consulte o seguinte:
 
- [Adicionar um utilizador de Linux adicional para uma VM](cloudinit-add-user.md)
- [Executar um Gestor de pacote para atualizar pacotes existentes no primeiro arranque](cloudinit-update-vm.md)
- [Alterar o nome local do anfitrião de VM](cloudinit-update-vm-hostname.md) 
- [Instalar um pacote de aplicações, ficheiros de configuração de atualização e injetar chaves](tutorial-automate-vm-deployment.md)