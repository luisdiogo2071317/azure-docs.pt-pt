---
title: Tutorial - Gerir o tráfego da Web - CLI do Azure
description: Saiba como criar um gateway de aplicação com um conjunto de dimensionamento de máquinas virtuais para gerir o tráfego da Web com a CLI do Azure.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: f93ae2a4aaab2ae25a212b2f97c1f97e6b0e095e
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2019
ms.locfileid: "54160034"
---
# <a name="tutorial-manage-web-traffic-with-an-application-gateway-using-the-azure-cli"></a>Tutorial: Gerir o tráfego web com um gateway de aplicação com a CLI do Azure

O gateway de aplicação serve para gerir e proteger o tráfego da Web para os servidores que mantém. Pode utilizar a CLI do Azure para criar um [gateway de aplicação](overview.md) que utiliza um [conjunto de dimensionamento de máquinas virtuais](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) para servidores back-end, para gerir o tráfego da Web. Neste exemplo, o conjunto de dimensionamento contém duas instâncias de máquina virtual que foram adicionadas ao conjunto de back-end predefinido do gateway de aplicação.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar a rede
> * Criar um gateway de aplicação
> * Criar um conjunto de dimensionamento de máquinas virtuais com o conjunto de back-end predefinido

Se preferir, pode executar este tutorial com o [Azure PowerShell](tutorial-manage-web-traffic-powershell.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Crie um grupo de recursos com [az group create](/cli/azure/group#az-group-create). 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroupAG* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede 

Crie a rede virtual denominada *myVNet* e a sub-rede denominada *myAGSubnet* com [az network vnet create](/cli/azure/network/vnet#az-net). Em seguida, pode adicionar a sub-rede denominada *myBackendSubnet* que é necessária para os servidores de back-end, com [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create). Crie o endereço IP público denominado *myAGPublicIPAddress* com [az network public-ip create](/cli/azure/network/public-ip#az-network_public_ip_create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24

az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24

az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

Utilize [az network application-gateway create](/cli/azure/network/application-gateway#az-application-gateway-create) para criar o gateway de aplicação denominado *myAppGateway*. Quando cria um gateway de aplicação com a CLI do Azure, especifica informações de configuração, tais como a capacidade, o sku e as definições de HTTP. O gateway de aplicação é atribuído a *myAGSubnet* e *myPublicIPAddress* que criou anteriormente. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

 A criação do gateway de aplicação pode demorar vários minutos. Depois de criado o gateway de aplicação, irá ver estas novas funcionalidades:

- *appGatewayBackendPool* - um gateway de aplicação tem de ter, pelo menos, um conjunto de endereços de back-end.
- *appGatewayBackendHttpSettings* - especifica que a porta 80 e um protocolo HTTP são utilizados para a comunicação.
- *appGatewayHttpListener* - o serviço de escuta predefinido associado a *appGatewayBackendPool*.
- *appGatewayFrontendIP* - atribui *myAGPublicIPAddress* a *appGatewayHttpListener*.
- *rule1* - a regra de encaminhamento predefinida associada a *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>Criar um Conjunto de Dimensionamento de Máquinas Virtuais

Neste exemplo, vai criar um conjunto de dimensionamento de máquinas virtuais que fornece servidores para o conjunto de back-end no gateway de aplicação. As máquinas virtuais no conjunto de dimensionamento são associadas a *myBackendSubnet* e *appGatewayBackendPool*. Para criar o conjunto de dimensionamento, utilize [az vmss create](/cli/azure/vmss#az-vmss-create).

```azurecli-interactive
az vmss create \
  --name myvmss \
  --resource-group myResourceGroupAG \
  --image UbuntuLTS \
  --admin-username azureuser \
  --admin-password Azure123456! \
  --instance-count 2 \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --vm-sku Standard_DS2 \
  --upgrade-policy-mode Automatic \
  --app-gateway myAppGateway \
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>Instalar o NGINX

Agora, pode instalar o NGINX no conjunto de dimensionamento de máquinas virtuais, pelo que pode testar a conectividade HTTP do conjunto de back-end.

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
```

## <a name="test-the-application-gateway"></a>Testar o gateway de aplicação

Para obter o endereço IP público do gateway de aplicação, utilize [az network public-ip show](/cli/azure/network/public-ip#az-network_public_ip_show). Copie o endereço IP público e cole-o na barra de endereço do browser.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Testar o URL base no gateway de aplicação](./media/tutorial-manage-web-traffic-cli/tutorial-nginxtest.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não forem necessários, remova o grupo de recursos, o gateway de aplicação e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroupAG --location eastus
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar a rede
> * Criar um gateway de aplicação
> * Criar um conjunto de dimensionamento de máquinas virtuais com o conjunto de back-end predefinido

> [!div class="nextstepaction"]
> [Restringir o tráfego da Web com uma firewall de aplicações Web](./tutorial-restrict-web-traffic-cli.md)
