---
title: "Criar um gateway de aplicação com o redirecionamento de URL com base no caminho - CLI do Azure | Microsoft Docs"
description: "Saiba como criar um gateway de aplicação com o URL com base no caminho redirecionado tráfego utilizando a CLI do Azure."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: davidmu
ms.openlocfilehash: 73fc20cf738f584008e7d8a019b8f7012dcacab1
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-application-gateway-with-url-path-based-redirection-using-the-azure-cli"></a>Criar um gateway de aplicação com o redirecionamento com base no caminho de URL a utilizar a CLI do Azure

Pode utilizar a CLI do Azure para configurar [regras de encaminhamento com base no caminho de URL](application-gateway-url-route-overview.md) quando cria um [gateway de aplicação](application-gateway-introduction.md). Neste tutorial, vai criar conjuntos de back-end utilizando [conjuntos de dimensionamento de máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Em seguida, crie as regras de encaminhamento de URL que certifique-se de que o tráfego web é redirecionado para o conjunto de back-end adequado.

Neste artigo, saiba como:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação
> * Adicionar serviços de escuta e regras de encaminhamento
> * Criar conjuntos de dimensionamento de máquina virtual para os conjuntos de back-end

O exemplo seguinte mostra o tráfego do site provenientes das portas 8080 e 8081 e que está a ser direcionado para os mesmo conjuntos de back-end:

![Exemplo de encaminhamento de URL](./media/tutorial-url-redirect-cli/scenario.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli).

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

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

Pode utilizar [criar gateway de aplicação do rede az](/cli/azure/application-gateway#create) para criar o gateway de aplicação com o nome myAppGateway. Quando cria um gateway de aplicação utilizando a CLI do Azure, especifique as informações de configuração, tais como a capacidade, sku e definições de HTTP. O gateway de aplicação é atribuído a *myAGSubnet* e *myPublicIPSddress* que criou anteriormente.  

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


### <a name="add-backend-pools-and-ports"></a>Adicionar conjuntos back-end e portas

Pode adicionar conjuntos de endereços de back-end com o nome *imagesBackendPool* e *videoBackendPool* para o gateway de aplicação utilizando [az gateway de aplicação-conjunto de endereços rede criar](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create). Adicione as portas front-end para os conjuntos utilizando [criar gateway de aplicação de rede az front-end-porta](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). 

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
  --name bport
az network application-gateway frontend-port create \
  --port 8081 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name rport
```

## <a name="add-listeners-and-rules"></a>Adicionar serviços de escuta e regras

### <a name="add-listeners"></a>Adicionar serviços de escuta

Adicione os serviços de escuta de back-end com o nome *backendListener* e *redirectedListener* que são necessárias para encaminhar o tráfego utilizando [az rede gateway de aplicação-serviço de escuta http criar](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port bport \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway http-listener create \
  --name redirectedListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port rport \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-the-default-url-path-map"></a>Adicionar o mapeamento de caminho de URL predefinido

Mapas de caminho de URL Certifique-se de que os URLs específicos são encaminhados para conjuntos back-end específico. Pode criar mapas de caminho de URL com o nome *imagePathRule* e *videoPathRule* utilizando [az rede gateway de aplicação url caminho-mapeamento criar](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) e [rede az Criar regra de mapa de caminho de url de gateway de aplicação](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create)

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name urlpathmap \
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
  --path-map-name urlpathmap \
  --paths /video/* \
  --address-pool videoBackendPool
```

### <a name="add-redirection-configuration"></a>Adicionar configuração de redirecionamento

Pode configurar o redirecionamento para o serviço de escuta com [criar gateway de aplicação de rede az redirecionamento-config](/cli/azure/application-gateway#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --gateway-name myAppGateway \
  --name redirectConfig \
  --resource-group myResourceGroupAG \
  --type Found \
  --include-path true \
  --include-query-string true \
  --target-listener backendListener
```

### <a name="add-the-redirection-url-path-map"></a>Adicionar o mapeamento de caminho do URL de redirecionamento

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name redirectpathmap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --redirect-config redirectConfig \
  --rule-name redirectPathRule
```

### <a name="add-routing-rules"></a>Adicionar regras de encaminhamento

As regras de encaminhamento associar os maps de caminho de URL com os serviços de escuta que criou. Pode adicionar as regras com o nome *defaultRule* e *redirectedRule* utilizando [criar regra de gateway de aplicação de rede az](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name defaultRule \
  --resource-group myResourceGroupAG \
  --http-listener backendListener \
  --rule-type PathBasedRouting \
  --url-path-map urlpathmap \
  --address-pool appGatewayBackendPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectedRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectedListener \
  --rule-type PathBasedRouting \
  --url-path-map redirectpathmap \
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
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Para obter o endereço IP público do gateway de aplicação, pode utilizar [mostrar de ip público de rede az](/cli/azure/network/public-ip#az_network_public_ip_show). Copie o endereço IP público e, em seguida, cole-o a barra de endereço do seu browser. Tal como *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm*, *http://40.121.222.19:8080/video/test.htm*, ou *http:// 40.121.222.19:8081/Images/Test.htm*.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![URL de base de teste no gateway de aplicação](./media/tutorial-url-redirect-cli/application-gateway-nginx.png)

Altere o URL para http://&lt;endereço ip&gt;: 8080/video/test.html, substituindo o IP de endereços para &lt;endereço ip&gt;, e deverá ver algo semelhante ao seguinte exemplo:

![Testar o URL de imagens no gateway de aplicação](./media/tutorial-url-redirect-cli/application-gateway-nginx-images.png)

Altere o URL para http://&lt;endereço ip&gt;: 8080/video/test.html, substituindo o IP de endereços para &lt;endereço ip&gt;, e deverá ver algo semelhante ao seguinte exemplo.

![URL de vídeo de teste no gateway de aplicação](./media/tutorial-url-redirect-cli/application-gateway-nginx-video.png)

Agora, altere o URL para http://&lt;endereço ip&gt;: 8081/images/test.htm, substituindo o IP de endereços para &lt;endereço ip&gt;, e deverá ver o tráfego redirecionado novamente para o conjunto de back-end de imagens em http://&lt;endereço ip&gt;: 8080/imagens.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação
> * Adicionar serviços de escuta e regras de encaminhamento
> * Criar conjuntos de dimensionamento de máquina virtual para os conjuntos de back-end

> [!div class="nextstepaction"]
> [Saiba mais sobre o que pode fazer com o gateway de aplicação](application-gateway-introduction.md)