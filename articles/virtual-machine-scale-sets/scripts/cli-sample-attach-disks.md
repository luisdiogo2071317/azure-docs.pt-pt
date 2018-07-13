---
title: Exemplos da CLI 2.0 do Azure - Anexar e utilizar discos de dados | Microsoft Docs
description: Exemplos da CLI 2.0 do Azure
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 63811e01145999077506914ab646576eeb511116
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308076"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Anexe e utilize discos de dados com um conjunto de dimensionamento de máquinas virtuais com a CLI 2.0 do Azure
Este script cria um conjunto de dimensionamento de máquinas virtuais e anexa e prepara discos de dados.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/use-data-disks/use-data-disks.sh "Create a virtual machine scale set with data disks")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Execute o seguinte comando para remover o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os seguintes comandos para criar um grupo de recursos, um conjunto de dimensionamento de máquinas virtuais e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Cria o conjunto de dimensionamento de máquinas virtuais e liga-o à rede virtual, à sub-rede e ao grupo de segurança de rede. É também criado um balanceador de carga para distribuir o tráfego para instâncias de VM individuais. Este comando também especifica a imagem da VM a ser utilizada e as credenciais administrativas.  |
| [az vmss disk attach](/cli/azure/vmss/disk#az_vmss_disk_attach) | Cria e anexa um disco de dados no conjunto de dimensionamento de máquinas virtuais. |
| [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) | Instala a Extensão do Script Personalizado do Azure para executar um script que prepara os discos de dados em cada instância da VM. |
| [az group delete](/cli/azure/ad/group#delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
Para obter mais informações sobre a CLI 2.0 do Azure, veja [documentação da CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/overview).

Pode encontrar exemplos adicionais do script do conjunto de dimensionamento de máquinas virtuais da CLI 2.0 do Azure na [documentação do conjunto de dimensionamento de máquinas virtuais do Azure](../cli-samples.md).