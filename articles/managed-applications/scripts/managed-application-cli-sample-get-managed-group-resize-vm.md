---
title: Exemplo do Script da CLI do Azure – Obter um grupo de recursos gerido e redimensionar VMs | Microsoft Docs
description: Exemplo do Script da CLI do Azure – Obter um grupo de recursos gerido e redimensionar VMs
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 85d58538e15881308ee1f645f7ddd12ec27c94de
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423417"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Obter recursos num grupo de recursos gerido e redimensionar VMs com a CLI do Azure

Este script obtém os recursos a partir de um grupo de recursos gerido e redimensiona as VMs nesse grupo de recursos.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para implementar a aplicação gerida. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az managedapp list](https://docs.microsoft.com/cli/azure/managedapp#az-managedapp-list) | Lista as aplicações geridas. Forneça os valores de consulta para focar os resultados. |
| [az resource list](https://docs.microsoft.com/cli/azure/resource#az-resource-list) | Lista os recursos. Forneça um grupo de recursos e os valores de consulta para focar o resultado. |
| [az vm resize](https://docs.microsoft.com/cli/azure/vm#az-vm-resize) | Atualize um tamanho de máquina virtual. |


## <a name="next-steps"></a>Passos seguintes

* Para obter uma introdução às aplicações geridas, veja [Descrição geral da Aplicação Gerida do Azure](../overview.md).
* Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).
