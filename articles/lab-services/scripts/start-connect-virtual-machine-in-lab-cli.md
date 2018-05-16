---
title: Exemplo de Script da CLI do Azure – Iniciar uma máquina virtual num laboratório personalizado | Microsoft Docs
description: Este script da CLI do Azure inicia uma máquina virtual num laboratório personalizado.
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
ms.openlocfilehash: c14328904f29f8160a1ccc1130fe6d2cfa629822
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-custom-lab"></a>Utilizar a CLI do Azure para iniciar uma máquina virtual num laboratório personalizado

Este script da CLI do Azure inicia uma máquina virtual (VM) num laboratório personalizado. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos:

| Comando | Notas |
|---|---|
| [az lab vm start ](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-start) | Inicia uma máquina virtual (VM) num laboratório personalizado. Esta operação pode demorar algum tempo. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Para obter mais exemplos de scripts do PowerShell do Azure Lab Services, veja [Azure Lab Services CLI samples](../samples-cli.md) (Exemplos da CLI do Azure Lab Services).