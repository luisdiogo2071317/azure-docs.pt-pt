---
title: Criar um gateway de aplicação com redirecionamento interno - CLI do Azure | Microsoft Docs
description: Saiba como criar um gateway de aplicação que redireciona o tráfego web interno para o conjunto adequado utilizando a CLI do Azure.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2017
ms.author: davidmu
ms.openlocfilehash: 20f54796f1dd088fa9d6446866151e6ce8ce03f7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-an-application-gateway-with-internal-redirection-using-the-azure-cli"></a>Criar um gateway de aplicação com redirecionamento interno utilizando a CLI do Azure

Pode utilizar a CLI do Azure para configurar [redirecionamento de tráfego da web](multiple-site-overview.md) quando cria um [gateway de aplicação](overview.md). Neste tutorial, é possível definir um conjunto de back-end com um conjunto de dimensionamento de máquinas virtuais. Em seguida, configure os serviços de escuta e as regras com base em domínios que possui para se certificar de que o tráfego web chega o conjunto adequado. Este tutorial parte do princípio de que possui vários domínios e utiliza exemplos *www.contoso.com* e *www.contoso.org*.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação
> * Adicionar serviços de escuta e a regra de redirecionamento
> * Criar um conjunto com o conjunto de back-end de dimensionamento de máquina virtual
> * Crie um registo CNAME no seu domínio

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

Criar a rede virtual denominada *myVNet* e a sub-rede denominada *myAGSubnet* utilizando [az rede vnet criar](/cli/azure/network/vnet#az_net). Em seguida, pode adicionar a sub-rede denominada *myBackendSubnet* que é necessário o conjunto de back-end de servidores utilizando [az rede vnet sub-rede](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Criar o endereço IP público com o nome *myAGPublicIPAddress* utilizando [az público-ip da rede criar](/cli/azure/public-ip#az_network_public_ip_create).

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


## <a name="add-listeners-and-rules"></a>Adicionar serviços de escuta e regras 

Um serviço de escuta é obrigatório para ativar o gateway de aplicação encaminhar o tráfego adequadamente para o conjunto de back-end. Neste tutorial, vai criar dois serviços de escuta para os dois domínios. Neste exemplo, os serviços de escuta são criados para os domínios de *www.contoso.com* e *www.contoso.org*.

Adicione os serviços de escuta de back-end que são necessárias para encaminhar o tráfego utilizando [az rede gateway de aplicação-serviço de escuta http criar](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).

```azurecli-interactive
az network application-gateway http-listener create \
  --name contosoComListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.com
az network application-gateway http-listener create \
  --name contosoOrgListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.org   
  ```

### <a name="add-the-redirection-configuration"></a>Adicionar a configuração de redirecionamento

Adicionar a configuração de redirecionamento que envia o tráfego de *www.consoto.org* para o serviço de escuta para *www.contoso.com* no gateway de aplicação com [az gateway de aplicação de rede criar a configuração de redirecionamento](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name orgToCom \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener contosoListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-routing-rules"></a>Adicionar regras de encaminhamento

As regras são processadas pela ordem em que são criados e o tráfego é direcionado através da primeira regra que corresponde ao URL enviados para o gateway de aplicação. Por exemplo, se tiver uma regra com um serviço de escuta básico e uma regra com uma escuta de vários site ambas na mesma porta, a regra com o serviço de escuta de vários site têm de estar listada antes da regra com o serviço de escuta básico por ordem para a regra de vários site a funcionar conforme esperado. 

Neste exemplo, pode criar duas novas regras e eliminar a regra predefinida que foi criada.  Pode adicionar a regra, utilizando [criar regra de gateway de aplicação de rede az](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoComRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoComListener \
  --rule-type Basic \
  --address-pool appGatewayBackendPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoOrgRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoOrgListener \
  --rule-type Basic \
  --redirect-config orgToCom
az network application-gateway rule delete \
  --gateway-name myAppGateway \
  --name rule1 \
  --resource-group myResourceGroupAG
```

## <a name="create-virtual-machine-scale-sets"></a>Criar conjuntos de dimensionamento de máquina virtual

Neste exemplo, crie um conjunto de dimensionamento de máquina virtual que suporta o conjunto de back-end que criou. O conjunto de dimensionamento que criar com o nome *myvmss* e contém duas instâncias de máquina virtual no qual instala NGINX.

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

Execute este comando na janela de shell:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-cname-record-in-your-domain"></a>Criar registo CNAME no seu domínio

Depois de criado o gateway de aplicação com o respetivo endereço IP público, pode obter o endereço DNS e utilizá-la para criar um registo CNAME no seu domínio. Pode utilizar [mostrar de ip público de rede az](/cli/azure/network/public-ip#az_network_public_ip_show) para obter o endereço DNS do gateway de aplicação. Copiar o *fqdn* valor o DNSSettings e utilizá-lo como o valor do registo CNAME que criar. A utilização de registos não é recomendada porque o VIP podem ser alterados quando o gateway de aplicação é reiniciado.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Introduza o nome de domínio na barra de endereço do seu browser. Tal como http://www.contoso.com.

![Testar o site da contoso no gateway de aplicação](./media/redirect-internal-site-cli/application-gateway-nginxtest.png)

Altere o endereço para o domínio, por exemplo http://www.contoso.org e deverá ver que o tráfego foi redirecionado para o serviço de escuta para www.contoso.com.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> * Configure a rede
> * Criar um gateway de aplicação
> * Adicionar serviços de escuta e a regra de redirecionamento
> * Criar um conjunto com o conjunto de back-end de dimensionamento de máquina virtual
> * Crie um registo CNAME no seu domínio