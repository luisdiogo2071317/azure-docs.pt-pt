---
title: Exemplo de Script da CLI do Azure – Restringir o tráfego da Web | Microsoft Docs
description: Exemplo de Script da CLI do Azure – Criar um gateway de aplicação com uma firewall de aplicações Web e um conjunto de dimensionamento de máquinas virtuais que utiliza regras OWASP para restringir o tráfego.
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
ms.openlocfilehash: 5864b77976db2ba953304a17c4622fdb6d4b7cdc
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
ms.locfileid: "33203583"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Restringir o tráfego da Web com a CLI do Azure

Este script cria um gateway de aplicação com uma firewall de aplicações Web que utiliza um conjunto de dimensionamento de máquinas virtuais para servidores back-end. A firewall de aplicações Web restringe o tráfego da Web com base nas regras OWASP. Depois de executar o script, pode testar o gateway de aplicação com o respetivo endereço IP público.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, o gateway de aplicação e todos os recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar a implementação. Cada item na tabela liga a documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az_net) | Cria uma rede virtual. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Cria uma sub-rede numa rede virtual. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/public-ip#az_network_public_ip_create) | Cria o endereço IP público do gateway de aplicação. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/application-gateway#az_application_gateway_create) | Cria um gateway de aplicação. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Cria um conjunto de dimensionamento de máquinas virtuais. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Cria uma conta de armazenamento. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) | Cria uma conta de armazenamento. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_show) | Obtém o endereço IP público do gateway de aplicação. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Pode encontrar exemplos adicionais de script da CLI do gateway de aplicação na [Documentação do gateway de aplicação do Azure](../cli-samples.md).
