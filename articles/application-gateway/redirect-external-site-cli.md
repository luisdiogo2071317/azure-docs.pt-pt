---
title: Criar um gateway de aplicação com o redirecionamento de tráfego externo - CLI do Azure | Microsoft Docs
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
ms.date: 01/24/2018
ms.author: davidmu
ms.openlocfilehash: 6bcc5528398d8dd952e789eb2cecaa0eba856135
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-an-application-gateway-with-external-redirection-using-the-azure-cli"></a>Criar um gateway de aplicação com redirecionamento externo utilizando a CLI do Azure

Pode utilizar a CLI do Azure para configurar [redirecionamento de tráfego da web](multiple-site-overview.md) quando cria um [gateway de aplicação](overview.md). Neste tutorial, configurou uma regra que redireciona o tráfego da web que chega ao gateway de aplicação para um site externo e o serviço de escuta.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Configure a rede
> * Criar uma regra de serviço de escuta e o redirecionamento
> * Criar um gateway de aplicação

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

Criar a rede virtual denominada *myVNet* e a sub-rede denominada *myAGSubnet* utilizando [az rede vnet criar](/cli/azure/network/vnet#az_net). Criar o endereço IP público com o nome *myAGPublicIPAddress* utilizando [az público-ip da rede criar](/cli/azure/public-ip#az_network_public_ip_create). Estes recursos são utilizados para fornecer conectividade de rede para o gateway de aplicação e os respetivos recursos associados.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

Pode utilizar [criar gateway de aplicação do rede az](/cli/azure/application-gateway#create) para criar o gateway de aplicação com o nome *myAppGateway*. Quando cria um gateway de aplicação utilizando a CLI do Azure, especifique as informações de configuração, tais como a capacidade, sku e definições de HTTP. O gateway de aplicação é atribuído a *myAGSubnet* e *myPublicIPSddress* que criou anteriormente. 

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
  --frontend-port 8080 \
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

### <a name="add-the-redirection-configuration"></a>Adicionar a configuração de redirecionamento

Adicionar a configuração de redirecionamento que envia o tráfego de *www.consoto.org* para o serviço de escuta para *www.contoso.com* para o gateway de aplicação com [az gateway de aplicação de rede criar a configuração de redirecionamento](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name myredirect \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Temporary \
  --target-url "http://bing.com"
```

### <a name="add-a-listener-and-routing-rule"></a>Adicionar uma regra de encaminhamento e serviço de escuta

Um serviço de escuta é obrigatório para ativar o gateway de aplicação encaminhar o tráfego adequadamente. Criar o serviço de escuta com [az rede gateway de aplicação-serviço de escuta http criar](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create) com a porta de front-end criada com [criar gateway de aplicação de rede az front-end-porta](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). Uma regra é necessária para o serviço de escuta de saber para onde enviar o tráfego de entrada. Criar uma regra básica com o nome *redirectRule* utilizando [criar regra de gateway de aplicação de rede az](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name redirectPort
az network application-gateway http-listener create \
  --name redirectListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port redirectPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectListener \
  --rule-type Basic \
  --redirect-config myredirect
```

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Para obter o endereço IP público do gateway de aplicação, pode utilizar [mostrar de ip público de rede az](/cli/azure/network/public-ip#az_network_public_ip_show). Copie o endereço IP público e cole-o na barra de endereço do browser.

Deverá ver *bing.com* aparecem no seu browser.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> * Configure a rede
> * Criar uma regra de serviço de escuta e o redirecionamento
> * Criar um gateway de aplicação