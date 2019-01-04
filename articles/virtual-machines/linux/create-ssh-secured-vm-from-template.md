---
title: Criar uma VM do Linux no Azure a partir de um modelo | Documentos da Microsoft
description: Como utilizar a CLI do Azure para criar uma VM do Linux a partir de um modelo do Resource Manager
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 01/03/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c6e4e142025e40e77afee01d70de9cef68eca1f0
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54013201"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Como criar uma máquina virtual Linux com modelos Azure Resource Manager

Este artigo mostra-lhe como implementar rapidamente uma máquina virtual (VM) do Linux com modelos Azure Resource Manager e a CLI do Azure. Também pode efetuar estes passos com o [CLI clássica do Azure](create-ssh-secured-vm-from-template-nodejs.md).


Este artigo mostra-lhe como implementar rapidamente uma máquina virtual (VM) do Linux com modelos Azure Resource Manager e a CLI do Azure. 

## <a name="templates-overview"></a>Descrição geral de modelos
Modelos Azure Resource Manager são ficheiros JSON que definem a infraestrutura e a configuração da sua solução do Azure. Ao utilizar um modelo, pode implementar repetidamente a solução durante o ciclo de vida da mesma e ter a confiança de que os recursos são implementados num estado consistente. Para saber mais sobre o formato do modelo e como construí-lo, consulte [criar o primeiro modelo do Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md). Para ver a sintaxe JSON para tipos de recursos, consulte [Define resources in Azure Resource Manager templates (Definir recursos nos modelos do Azure Resource Manager)](/azure/templates/).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Um grupo de recursos tem de ser criado antes de uma máquina virtual. O exemplo seguinte cria um grupo de recursos chamado *myResourceGroupVM* no *eastus* região:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual
O exemplo seguinte cria uma VM a partir [este modelo do Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) com [criar a implementação do grupo az](/cli/azure/group/deployment#az_group_deployment_create). É permitida apenas a autenticação SSH. Quando solicitado, forneça o valor de sua própria chave pública SSH, como o conteúdo do *~/.ssh/id_rsa.pub*. Se precisar de criar um par de chaves SSH, veja [como criar e utilizar um par de chaves SSH para VMs do Linux no Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

No exemplo anterior, que especificou um modelo armazenado no GitHub. Também pode transferir ou criar um modelo e especifique o caminho local com o `--template-file` parâmetro.


## <a name="connect-to-virtual-machine"></a>Conectar à máquina virtual
Para encaminhar o SSH para a VM, obtenha o endereço IP público com [show de vm de az](/cli/azure/vm#az-vm-show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

Pode, em seguida, SSH à VM como habitualmente. Fornece-lhe próprio endereço IP público no comando anterior:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Passos Seguintes
Neste exemplo, criou uma VM do Linux básica. Para obter mais modelos do Resource Manager que incluem estruturas de aplicações ou crie ambientes mais complexos, procurar as [Galeria de modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/).

Para saber mais sobre a criação de modelos, veja a sintaxe JSON e propriedades para os tipos de recursos que implementou:

* [Networksecuritygroups](/azure/templates/microsoft.network/networksecuritygroups)
* [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Network/virtualnetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
* [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
