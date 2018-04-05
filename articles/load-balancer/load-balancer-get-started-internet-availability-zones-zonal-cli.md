---
title: Criar um padrão de Balanceador de público de carga com zonal front-end utilizando a CLI do Azure | Microsoft Docs
description: Saiba como criar um padrão de Balanceador de público de carga com zonal front-end utilizando a CLI do Azure
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: da18693f090a256bf69ea27e46e0ac010eb293b0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-frontend-using-azure-cli"></a>Criar um padrão de Balanceador de público de carga com zonal front-end utilizando a CLI do Azure

Os passos neste artigo através da criação de um público [padrão de Balanceador de carga](https://aka.ms/azureloadbalancerstandard) com zonal front-end. Ter um meio de front-end zonal qualquer fluxo de entrada ou saído é fornecido por uma único zona numa região. Pode criar um balanceador de carga com um front-end zonal utilizando um endereço IP público padrão zonal na sua configuração de front-end. Para compreender como funcionam os zonas de disponibilidade com o Balanceador de carga padrão, consulte [zonas padrão Balanceador de carga e a disponibilidade](load-balancer-standard-availability-zones.md). 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, certifique-se de que instalou a versão mais recente [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) e tem sessão iniciada numa conta do Azure com [início de sessão az](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login).

> [!NOTE]
> Suporte para as zonas de disponibilidade está disponível para famílias de tamanho VM, selecionados de recursos do Azure e regiões. Para obter mais informações sobre como começar a utilizar e os recursos do Azure, regiões e famílias de tamanho VM pode tentar zonas de disponibilidade com, consulte [zonas de descrição geral da disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Para obter suporte, pode contactar-nos no [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) ou [abrir um pedido de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 


## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o seguinte comando:

```azurecli-interactive
az group create --name myResourceGroupZLB --location westeurope
```

## <a name="create-a-public-standard-ip-address"></a>Criar um endereço IP padrão público

Crie um endereço IP público padrão zonal utilizando o seguinte comando:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroupZLB --name myPublicIPZonal --sku Standard --zone 1
```

## <a name="create-a-load-balancer"></a>Criar um balanceador de carga

Crie um padrão de Balanceador de público de carga com o IP público padrão que criou no passo anterior usando o seguinte comando:

```azurecli-interactive
az network lb create --resource-group myResourceGroupZLB --name myLoadBalancer --public-ip-address myPublicIPZonal --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Criar uma sonda LB na porta 80

Crie uma sonda de estado de funcionamento do Balanceador de carga utilizando o seguinte comando:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroupZLB --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Criar uma regra LB para a porta 80

Crie uma regra de Balanceador de carga utilizando o seguinte comando:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroupZLB --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [zonas padrão Balanceador de carga e a disponibilidade](load-balancer-standard-availability-zones.md).



