---
title: Exemplos da CLI do Azure – Conjunto de dimensionamento de zona única | Microsoft Docs
description: Exemplos da CLI do Azure
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
ms.openlocfilehash: c359f50a8c2641ba6bc80ab6ef7f80c814d2704d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696971"
---
# <a name="create-a-single-zone-virtual-machine-scale-set-with-powershell"></a>Criar um conjunto de dimensionamento de máquinas virtuais de zona única com o PowerShell
Este script cria um conjunto de dimensionamento de máquinas virtuais com o Ubuntu numa única Zona de Disponibilidade. Depois de executar o script, pode aceder à máquina virtual através de RDP.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh "Create single-zone scale set")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Execute o seguinte comando para remover o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os seguintes comandos para criar um grupo de recursos, um conjunto de dimensionamento de máquinas virtuais e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/ad/group) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az vmss create](/cli/azure/vmss) | Cria o conjunto de dimensionamento de máquinas virtuais e liga-o à rede virtual, à sub-rede e ao grupo de segurança de rede. É também criado um balanceador de carga para distribuir o tráfego para instâncias de VM individuais. Este comando também especifica a imagem da VM a ser utilizada e as credenciais administrativas.  |
| [az group delete](/cli/azure/ad/group) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Pode encontrar exemplos adicionais de scripts da CLI do Azure para conjuntos de dimensionamento de máquinas virtuais na [documentação do conjunto de dimensionamento de máquinas virtuais do Azure](../cli-samples.md).
