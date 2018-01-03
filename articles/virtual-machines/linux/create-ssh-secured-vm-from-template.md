---
title: Criar uma VM com Linux no Azure a partir de um modelo | Microsoft Docs
description: Como utilizar o 2.0 CLI do Azure para criar uma VM com Linux a partir de um modelo do Resource Manager
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/18/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7bd5b8c0534a51c6b6c9e8871be513194d38788
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Como criar uma máquina virtual Linux com modelos Azure Resource Manager
Este artigo mostra como implementar rapidamente uma máquina virtual (VM) do Linux com modelos Azure Resource Manager e o 2.0 CLI do Azure. Também pode efetuar estes passos com a [CLI 1.0 do Azure](create-ssh-secured-vm-from-template-nodejs.md).


## <a name="templates-overview"></a>Descrição geral de modelos
Modelos Azure Resource Manager são ficheiros JSON que definem a infraestrutura e a configuração da sua solução do Azure. Ao utilizar um modelo, pode implementar repetidamente a solução durante o ciclo de vida da mesma e ter a confiança de que os recursos são implementados num estado consistente. Para saber mais sobre o formato do modelo e como pode construir, consulte o artigo [criar o primeiro modelo Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md). Para ver a sintaxe JSON para tipos de recursos, consulte [Define resources in Azure Resource Manager templates (Definir recursos nos modelos do Azure Resource Manager)](/azure/templates/).


## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Um grupo de recursos tem de ser criado antes de uma máquina virtual. O exemplo seguinte cria um grupo de recursos denominado *myResourceGroupVM* no *eastus* região:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual
O exemplo seguinte cria uma VM a partir [este modelo Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) com [criar a implementação do grupo az](/cli/azure/group/deployment#create). Autenticação de SSH só é permitida. Quando solicitado, forneça o valor da sua própria chave pública SSH, tais como o conteúdo do *~/.ssh/id_rsa.pub*. Se precisar de criar um par de chaves SSH, consulte [como criar e utilizar um par de chaves SSH para VMs com Linux no Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

No exemplo anterior, que especificou um modelo armazenado no GitHub. Também pode transferir ou criar um modelo e especifique o caminho local com o `--template-file` parâmetro.


## <a name="connect-to-virtual-machine"></a>Conectar à máquina virtual
Para SSH para a VM, obter o endereço IP público com [mostrar de vm az](/cli/azure/vm#show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

Pode, em seguida, SSH para a VM como habitualmente. Fornece-lhe próprio endereço IP público do comando anterior:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Passos Seguintes
Neste exemplo, criou uma VM com Linux básico. Para mais modelos de Gestor de recursos que incluem estruturas de aplicações ou criar ambientes mais complexas, procure o [Galeria de modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/).