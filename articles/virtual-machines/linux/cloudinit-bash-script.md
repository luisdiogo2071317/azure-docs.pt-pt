---
title: Utilizar a cloud init para executar um script de bash numa VM com Linux no Azure | Microsoft Docs
description: "Como utilizar a cloud init para executar um script de bash numa VM com Linux durante a criação com o 2.0 CLI do Azure"
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
ms.openlocfilehash: 1fcc432e8437a7fd284a75aa40454848a2af3006
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-run-a-bash-script-in-a-linux-vm-in-azure"></a>Utilizar nuvem init para executar um script de bash numa VM com Linux no Azure
Este artigo mostra como utilizar [nuvem init](https://cloudinit.readthedocs.io) para executar um existente bash script numa máquina virtual (VM) Linux ou (VMSS) de conjuntos de dimensionamento de máquina virtual no aprovisionamento de tempo no Azure. Estes scripts de nuvem init executam no primeiro arranque depois dos recursos foram aprovisionados através do Azure. Para obter mais informações sobre como nuvem init nativamente funciona no Azure e os distros suportados do Linux, consulte [nuvem init descrição-geral](using-cloud-init.md)

## <a name="run-a-bash-script-with-cloud-init"></a>Executar um script de bash com init de nuvem
Com não é necessário para converter os scripts existentes na nuvem-config init na nuvem, o cloud init aceita vários tipos de entrada, um dos quais é um script de bash.

Se tiver utilizado a extensão do Azure do Linux personalizadas Script para executar os scripts, pode migrar deles para utilizar a cloud init. No entanto, as extensões do Azure tiver integrado relatórios de alerta de falhas de script, uma implementação de imagem de nuvem init não irá falhar se o script falhar.

Para ver esta funcionalidade em ação, crie um script de deteção simples para fins de teste. Como a nuvem-init `#cloud-config` ficheiro, este script deve ser local para onde irá executar os comandos de AzureCLI para aprovisionar a máquina virtual.  Neste exemplo, crie o ficheiro na Shell na nuvem não no seu computador local. Pode utilizar qualquer editor que desejar. Introduza `sensible-editor simple_bash.sh` para criar o ficheiro e ver uma lista de editores disponíveis. Escolha #1 para utilizar o **nano** editor. Certifique-se de que o ficheiro de toda a nuvem-init é copiado corretamente, especialmente a primeira linha.  

```bash
#!/bin/sh
echo "this has been written via cloud-init" + $(date) >> /tmp/myScript.txt
```

Antes de implementar esta imagem, tem de criar um grupo de recursos com o [criar grupo az](/cli/azure/group#create) comando. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Agora, crie uma VM com [az vm criar](/cli/azure/vm#create) e especifique o ficheiro de scripts de bash com `--custom-data simple_bash.sh` da seguinte forma:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data simple_bash.sh \
  --generate-ssh-keys 
```
## <a name="verify-bash-script-has-run"></a>Verifique se os scripts de bash foi executada
SSH para o endereço IP público da sua VM apresentado no resultado do comando anterior. Introduza os seus próprios **publicIpAddress** da seguinte forma:

```bash
ssh <publicIpAddress>
```

Mude para o **/tmp dos** diretório e certifique-se de que o ficheiro myScript.txt existe e tem o texto adequado dentro do mesmo.  Se não existir, pode verificar o **/var/log/cloud-init.log** para obter mais detalhes.  Procurar a entrada seguinte:

```bash
Running config-scripts-user using lock Running command ['/var/lib/cloud/instance/scripts/part-001']
```

## <a name="next-steps"></a>Passos seguintes
Para obter exemplos adicionais nuvem-init das alterações de configuração, consulte o seguinte:
 
- [Adicionar um utilizador de Linux adicional para uma VM](cloudinit-add-user.md)
- [Executar um Gestor de pacote para atualizar pacotes existentes no primeiro arranque](cloudinit-update-vm.md)
- [Alterar o nome local do anfitrião de VM](cloudinit-update-vm-hostname.md) 
- [Instalar um pacote de aplicações, ficheiros de configuração de atualização e injetar chaves](tutorial-automate-vm-deployment.md)