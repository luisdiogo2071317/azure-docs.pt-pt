---
title: Criar um gateway de aplicação com o URL com base no caminho regras de encaminhamento - CLI do Azure | Microsoft Docs
description: Saiba como criar URL caminho com base em regras de encaminhamento para uma aplicação gateway e máquina virtual de conjunto de dimensionamento utilizando a CLI do Azure.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: eeba6ce5bd082cb6b9c3266fcc95deb1785e8cce
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-url-path-based-routing-rules-using-the-azure-cli"></a>Criar um gateway de aplicação com o URL com base no caminho regras de encaminhamento utilizando a CLI do Azure

Pode utilizar a CLI do Azure para configurar [regras de encaminhamento com base no caminho de URL](application-gateway-url-route-overview.md) quando cria um [gateway de aplicação](application-gateway-introduction.md). Neste tutorial, vai criar conjuntos de back-end utilizando um [conjunto de dimensionamento da máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Em seguida, crie as regras de encaminhamento que certifique-se de que o tráfego web chega os servidores adequados nos agrupamentos de.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação com o mapa de URL
> * Criar conjuntos de dimensionamento de máquina virtual com os conjuntos de back-end

![Exemplo de encaminhamento de URL](./media/application-gateway-create-url-route-cli/scenario.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Criar um grupo de recursos utilizando [criar grupo az](/cli/azure/group#create).

O exemplo seguinte cria um grupo de recursos denominado *myResourceGroupAG* no *eastus* localização.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede 

Criar a rede virtual denominada *myVNet* e a sub-rede denominada *myAGSubnet* utilizando [az rede vnet criar](/cli/azure/network/vnet#az_net). Em seguida, pode adicionar a sub-rede denominada *myBackendSubnet* que é necessário para os servidores de back-end utilizando [az rede vnet sub-rede](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Criar o endereço IP público com o nome *myAGPublicIPAddress* utilizando [az público-ip da rede criar](/cli/azure/public-ip#az_network_public_ip_create).

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

## <a name="create-the-application-gateway-with-url-map"></a>Criar o gateway de aplicação com o mapa de URL

Pode utilizar [criar gateway de aplicação do rede az](/cli/azure/application-gateway#create) para criar o gateway de aplicação com o nome *myAppGateway*. Quando cria um gateway de aplicação utilizando a CLI do Azure, especifique as informações de configuração, tais como a capacidade, sku e definições de HTTP. O gateway de aplicação é atribuído a *myAGSubnet* e *myAGPublicIPAddress* que criou anteriormente. 

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

 Pode demorar alguns minutos até o gateway de aplicação a ser criado. Depois de criado o gateway de aplicação, pode ver estas novas funcionalidades do mesmo:

- *appGatewayBackendPool* -um gateway de aplicação tem de ter, pelo menos, um conjunto de endereços de back-end.
- *appGatewayBackendHttpSettings* -Especifica que a porta 80 e um protocolo HTTP é utilizado para comunicação.
- *appGatewayHttpListener* -o serviço de escuta de predefinição associado *appGatewayBackendPool*.
- *appGatewayFrontendIP* -atribui *myAGPublicIPAddress* para *appGatewayHttpListener*.
- *rule1* - predefinição encaminhamento regra que está associada a *appGatewayHttpListener*.


### <a name="add-image-and-video-backend-pools-and-port"></a>Adicionar a imagem e o vídeo conjuntos de back-end e porta

Pode adicionar conjuntos de back-end com o nome *imagesBackendPool* e *videoBackendPool* para o gateway de aplicação utilizando [az gateway de aplicação-conjunto de endereços rede criar](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create). Adicionar a porta de front-end para os conjuntos utilizando [criar gateway de aplicação de rede az front-end-porta](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). 

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name imagesBackendPool
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name videoBackendPool
az network application-gateway frontend-port create \
  --port 8080 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name port8080
```

### <a name="add-backend-listener"></a>Adicionar serviço de escuta de back-end

Adicione o serviço de escuta de back-end com o nome *backendListener* que necessários para encaminhar o tráfego utilizando [az rede gateway de aplicação-serviço de escuta http criar](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port port8080 \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-url-path-map"></a>Adicionar mapeamento de caminho de URL

Mapas de caminho de URL Certifique-se de que os URLs específicos são encaminhados para conjuntos back-end específico. Pode criar mapas de caminho de URL com o nome *imagePathRule* e *videoPathRule* utilizando [az rede gateway de aplicação url caminho-mapeamento criar](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) e [rede az Criar regra de mapa de caminho de url de gateway de aplicação](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create)

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name myPathMap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --address-pool imagesBackendPool \
  --default-address-pool appGatewayBackendPool \
  --default-http-settings appGatewayBackendHttpSettings \
  --http-settings appGatewayBackendHttpSettings \
  --rule-name imagePathRule
az network application-gateway url-path-map rule create \
  --gateway-name myAppGateway \
  --name videoPathRule \
  --resource-group myResourceGroupAG \
  --path-map-name myPathMap \
  --paths /video/* \
  --address-pool videoBackendPool
```

### <a name="add-routing-rule"></a>Adicionar regra de encaminhamento

A regra de encaminhamento associa os mapas de URL com o serviço de escuta que criou. Pode adicionar a regra com o nome *rule2* utilizando [criar regra de gateway de aplicação de rede az](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener backendListener \
  --rule-type PathBasedRouting \
  --url-path-map myPathMap \
  --address-pool appGatewayBackendPool
```

## <a name="create-virtual-machine-scale-sets"></a>Criar conjuntos de dimensionamento de máquina virtual

Neste exemplo, crie três conjuntos de dimensionamento de máquina virtual que suportam os três conjuntos de back-end que criou. Os conjuntos de dimensionamento que criar são denominados *myvmss1*, *myvmss2*, e *myvmss3*. Cada conjunto de dimensionamento contém duas instâncias de máquina virtual no qual instala NGINX.

```azurecli-interactive
for i in `seq 1 3`; do
  if [ $i -eq 1 ]
  then
    poolName="appGatewayBackendPool" 
  fi
  if [ $i -eq 2 ]
  then
    poolName="imagesBackendPool"
  fi
  if [ $i -eq 3 ]
  then
    poolName="videoBackendPool"
  fi
  az vmss create \
    --name myvmss$i \
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
    --backend-pool-name $poolName
done
```

### <a name="install-nginx"></a>Instalar o NGINX

```azurecli-interactive
for i in `seq 1 3`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/vhorne/samplescripts/master/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Para obter o endereço IP público do gateway de aplicação, pode utilizar [mostrar de ip público de rede az](/cli/azure/network/public-ip#az_network_public_ip_show). Copie o endereço IP público e cole-o na barra de endereço do browser. Tal como *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm*, ou *http://40.121.222.19:8080/video/test.htm*.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![URL de base de teste no gateway de aplicação](./media/application-gateway-create-url-route-cli/application-gateway-nginx.png)

Altere o URL para http://<ip-address>:8080/video/test.html até ao final do base URL e deverá ver algo semelhante ao seguinte exemplo:

![Testar o URL de imagens no gateway de aplicação](./media/application-gateway-create-url-route-cli/application-gateway-nginx-images.png)

Altere o URL para http://<ip-address>:8080/video/test.html e deverá ver algo semelhante ao seguinte exemplo.

![URL de vídeo de teste no gateway de aplicação](./media/application-gateway-create-url-route-cli/application-gateway-nginx-video.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação com o mapa de URL
> * Criar conjuntos de dimensionamento de máquina virtual com os conjuntos de back-end

Para obter mais informações sobre gateways de aplicação e os recursos associados, avance para os artigos de procedimentos.
