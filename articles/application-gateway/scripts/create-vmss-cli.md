---
title: Exemplo de Script da CLI do Azure – Gerir o tráfego da Web | Microsoft Docs
description: Exemplo de Script da CLI do Azure – Gerir o tráfego da Web com um gateway de aplicação e um conjunto de dimensionamento de máquinas virtuais.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: b2485b65c54bdd5dcd523226e17c180bf1eb86a6
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734203"
---
# <a name="manage-web-traffic-using-the-azure-cli"></a>Gerir o tráfego da Web com a CLI do Azure

Este script cria um gateway de aplicação que utiliza um conjunto de dimensionamento de máquinas virtuais para servidores back-end. O gateway de aplicação pode então ser configurado para gerir o tráfego da Web. Depois de executar o script, pode testar o gateway de aplicação com o respetivo endereço IP público.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, o gateway de aplicação e todos os recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar a implementação. Cada item na tabela liga a documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Cria uma rede virtual. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnetwork) | Cria uma sub-rede numa rede virtual. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest) | Cria o endereço IP público do gateway de aplicação. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest) | Cria um gateway de aplicação. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az-vmss-create) | Cria um conjunto de dimensionamento de máquinas virtuais. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip) | Obtém o endereço IP público do gateway de aplicação. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Pode ver exemplos do script da CLI do gateway de aplicação adicionais na [Documentação da VM do Windows do Azure](../cli-samples.md).
