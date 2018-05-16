---
title: Exemplo de Script da CLI do Azure – Criar e verificar uma máquina virtual num laboratório personalizado | Microsoft Docs
description: Este script da CLI do Azure cria uma máquina virtual num laboratório personalizado e verifica se está disponível.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2018
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: bd564cda7b4d5c2158b8499b48b8faa68309b461
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-azure-cli-to-create-and-verify-availability-of-a-virtual-machine-in-a-custom-lab"></a>Utilizar a CLI do Azure para criar e verificar a disponibilidade de uma máquina virtual num laboratório personalizado

Este script da CLI do Azure cria uma máquina virtual (VM) num laboratório personalizado. A VM criada com base numa imagem do marketplace com autenticação ssh. Em seguida, o script verifica se a VM está disponível para utilização. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/create-verify-virtual-machine-in-lab/create-verify-virtual-machine-in-lab.sh "Create and verify availability of a VM")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos:

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az lab vm create ](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-create) | Cria uma máquina virtual (VM) num laboratório personalizado. |
| [az lab vm show](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-show) | Apresenta o estado da VM num laboratório personalizado. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Para obter mais exemplos de scripts do PowerShell do Azure Lab Services, veja [Azure Lab Services CLI samples](../samples-cli.md) (Exemplos da CLI do Azure Lab Services).