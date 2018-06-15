---
title: Início Rápido - Criar uma VM do Linux com a CLI 2.0 do Azure | Microsoft Docs
description: Neste início rápido, vai aprender a utilizar a CLI 2.0 do Azure para criar uma máquina virtual do Linux
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1c45f8f010d69337d21fce327933990a573988a4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187934"
---
# <a name="quickstart-create-a-linux-virtual-machine-with-the-azure-cli-20"></a>Início Rápido: criar uma máquina virtual do Linux com a CLI 2.0 do Azure

A CLI 2.0 do Azure é utilizada para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este início rápido mostra como utilizar a CLI 2.0 do Azure para implementar uma máquina virtual (VM) do Linux no Azure que executa o Ubuntu. Para ver a VM em ação, estabeleça o SSH para a VM e instale o servidor Web NGINX.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este guia de início rápido requer a execução da versão 2.0.30 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Criar a máquina virtual

Crie uma VM com o comando [az vm create](/cli/azure/vm#az_vm_create).

O seguinte exemplo cria uma VM com o nome *myVM*, adiciona uma conta de utilizador com o nome *azureuser* e gera chaves SSH, caso não existam, na localização predefinida da chave (*~/.ssh*). Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

São necessários alguns minutos para criar a VM e os recursos de suporte. O seguinte resultado de exemplo mostra que a operação de criação da VM foi concluída com êxito.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

Tenha em atenção o seu próprio `publicIpAddress` na saída da VM. Este endereço é utilizado para aceder à VM nos próximos passos.

## <a name="open-port-80-for-web-traffic"></a>Abrir a porta 80 para o tráfego da Web

Por predefinição, apenas as ligações SSH são abertas ao criar uma VM do Linux no Azure. Utilize [az vm open-port](/cli/azure/vm#az_vm_open_port) para abrir a porta TCP 80 para utilização com o servidor Web do NGINX:

```azurecli-interactive
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-virtual-machine"></a>Conectar à máquina virtual

Estabeleça o SSH para a VM como habitualmente. Substitua **publicIpAddress** pelo endereço IP público da VM, conforme indicado no resultado anterior da VM:

```bash
ssh azureuser@publicIpAddress
```

## <a name="install-web-server"></a>Instalar o servidor Web

Para ver a VM em ação, instale o servidor Web NGINX. Para atualizar as origens dos pacotes e instalar o pacote NGINX mais recente, execute os seguintes comandos na sua sessão SSH:

```bash
# update packages
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Quando terminar, utilize `exit` para fechar a sessão SSH.

## <a name="view-the-web-server-in-action"></a>Ver o servidor Web em ação

Com o NGINX instalado e a porta 80 agora aberta na VM da Internet, utilize um browser à sua escolha para ver a página de boas-vindas do NGINX predefinida. Utilize o endereço IP público da VM que obteve no passo anterior. O exemplo seguinte mostra o site predefinido do NGINX:

![Site predefinido do NGINX](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar o comando [az group delete](/cli/azure/group#az_group_delete) para remover o Grupo de Recursos, a VM e todos os recursos relacionados. Certifique-se de que encerrou a sessão SSH da VM e, em seguida, elimine os recursos da seguinte forma:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de início rápido, implementou uma máquina virtual simples, abriu uma porta de rede para o tráfego Web e instalou um servidor Web básico. Para saber mais sobre as máquinas virtuais do Azure, continue para o tutorial das VMs do Linux.


> [!div class="nextstepaction"]
> [Tutoriais das máquinas virtuais do Linux do Azure](./tutorial-manage-vm.md)
