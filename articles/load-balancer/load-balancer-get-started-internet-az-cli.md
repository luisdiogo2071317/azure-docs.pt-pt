---
title: Criar um padrão de Balanceador de público de carga com redundância de zona IP público endereço front-end utilizando a CLI do Azure | Microsoft Docs
description: Saiba como criar um padrão de Balanceador de público de carga com redundância de zona IP público endereço front-end utilizando a CLI do Azure
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
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 1a430f5c6349741e5d04626158dc89d42169a15b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-frontend-using-azure-cli"></a>Criar um padrão de Balanceador de público de carga com redundância de zona front-end utilizando a CLI do Azure

Os passos neste artigo através da criação de um público [padrão de Balanceador de carga](https://aka.ms/azureloadbalancerstandard) com um front-end com redundância de zona utilizando um endereço IP público padrão.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="register-for-availability-zones-preview"></a>Registar-se na pré-visualização de zonas de disponibilidade

Se optar por instalar e utilizar a CLI localmente, este tutorial, necessita que está a executar a CLI do Azure versão 2.0.17 ou superior.  Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

> [!NOTE]
> Zonas de disponibilidade estão em pré-visualização e estão prontas para o seu desenvolvimento e teste cenários. O suporte está disponível para famílias de tamanho VM, selecionados de recursos do Azure e regiões. Para obter mais informações sobre como começar a utilizar e os recursos do Azure, regiões e famílias de tamanho VM pode tentar zonas de disponibilidade com, consulte [zonas de descrição geral da disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Para obter suporte, pode contactar-nos no [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) ou [abrir um pedido de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Certifique-se de que instalou a versão mais recente [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) e tem sessão iniciada numa conta do Azure com [início de sessão az](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o seguinte comando:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-public-ip-standard"></a>Criar um padrão de IP público

Crie um Public IP Standard utilizando o seguinte comando:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIP --sku Standard
```

## <a name="create-a-load-balancer"></a>Criar um balanceador de carga

Crie um padrão de Balanceador de público de carga com o IP público padrão que criou no passo anterior usando o seguinte comando:

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Criar uma sonda LB na porta 80

Crie uma sonda de estado de funcionamento do Balanceador de carga utilizando o seguinte comando:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Criar uma regra LB para a porta 80

Crie uma regra de Balanceador de carga utilizando o seguinte comando:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Passos Seguintes
- Saiba como [criar um IP público numa zona de disponibilidade](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



