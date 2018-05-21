---
title: Criar um gateway de aplicação com um certificado - CLI do Azure | Microsoft Docs
description: Saiba como criar um gateway de aplicação e adicionar um certificado para a terminação de SSL com a CLI do Azure.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: victorh
ms.openlocfilehash: 2e72417b51e60f404f89b92a64678ce67b56a5e6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-the-azure-cli"></a>Criar um gateway de aplicação com HTTP para o redirecionamento de HTTPS utilizando a CLI do Azure

Pode utilizar a CLI do Azure para criar um [gateway de aplicação](application-gateway-introduction.md) com um certificado para a terminação de SSL. Uma regra de encaminhamento é utilizada para redirecionar o tráfego HTTP para a porta HTTPS no gateway de aplicação. Neste exemplo, também cria um [conjunto de dimensionamento da máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) para o conjunto de back-end do gateway de aplicação que contém duas instâncias de máquina virtual.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um certificado autoassinado
> * Configurar uma rede
> * Criar um gateway de aplicação com o certificado
> * Adicionar uma regra de serviço de escuta e o redirecionamento
> * Criar um conjunto com o conjunto de back-end predefinido de dimensionamento de máquina virtual

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli).

## <a name="create-a-self-signed-certificate"></a>Criar um certificado autoassinado

Para utilização em produção, deve importar um certificado válido assinado por um fornecedor fidedigno. Para este tutorial, crie um certificado autoassinado e um ficheiro pfx utilizando o comando de openssl.

```azurecli-interactive
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out appgwcert.crt
```

Introduza os valores que façam sentido para o seu certificado. Pode aceitar os valores predefinidos.

```azurecli-interactive
openssl pkcs12 -export -out appgwcert.pfx -inkey privateKey.key -in appgwcert.crt
```

Introduza a palavra-passe do certificado. Neste exemplo, *Azure123456!* está a ser utilizado.

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

## <a name="create-the-application-gateway"></a>Criar o gateway de aplicação

Pode utilizar [criar gateway de aplicação do rede az](/cli/azure/network/application-gateway#az_network_application_gateway_create) para criar o gateway de aplicação com o nome *myAppGateway*. Quando cria um gateway de aplicação utilizando a CLI do Azure, especifique as informações de configuração, tais como a capacidade, sku e definições de HTTP. 

O gateway de aplicação é atribuído a *myAGSubnet* e *myAGPublicIPAddress* que criou anteriormente. Neste exemplo, associar o certificado que criou e respetiva palavra-passe quando criar o gateway de aplicação. 

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
  --frontend-port 443 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress \
  --cert-file appgwcert.pfx \
  --cert-password "Azure123456!"

```

 Pode demorar alguns minutos até o gateway de aplicação a ser criado. Depois de criado o gateway de aplicação, pode ver estas novas funcionalidades do mesmo:

- *appGatewayBackendPool* -um gateway de aplicação tem de ter, pelo menos, um conjunto de endereços de back-end.
- *appGatewayBackendHttpSettings* -Especifica que a porta 80 e um protocolo HTTP é utilizado para comunicação.
- *appGatewayHttpListener* -o serviço de escuta de predefinição associado *appGatewayBackendPool*.
- *appGatewayFrontendIP* -atribui *myAGPublicIPAddress* para *appGatewayHttpListener*.
- *rule1* - predefinição encaminhamento regra que está associada a *appGatewayHttpListener*.

## <a name="add-a-listener-and-redirection-rule"></a>Adicionar uma regra de serviço de escuta e o redirecionamento

### <a name="add-the-http-port"></a>Adicionar a porta HTTP

Pode utilizar [criar gateway de aplicação de rede az front-end-porta](/cli/azure/network/application-gateway/frontend-port#az_network_application_gateway_frontend_port_create) para adicionar a porta HTTP para o gateway de aplicação.

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name httpPort
```

### <a name="add-the-http-listener"></a>Adicione o serviço de escuta HTTP

Pode utilizar [az rede gateway de aplicação-serviço de escuta http criar](/cli/azure/network/application-gateway/http-listener#az_network_application_gateway_http_listener_create) para adicionar o serviço de escuta com o nome *myListener* para o gateway de aplicação.

```azurecli-interactive
az network application-gateway http-listener create \
  --name myListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port httpPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-the-redirection-configuration"></a>Adicionar a configuração de redirecionamento

Adicionar o HTTP à configuração de redirecionamento de HTTPS para o gateway de aplicação com [criar gateway de aplicação de rede az redirecionamento-config](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name httpToHttps \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener appGatewayHttpListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-the-routing-rule"></a>Adicionar a regra de encaminhamento

Adicionar a regra de encaminhamento com o nome *rule2* com a configuração de redirecionamento para o gateway de aplicação com [criar regra de gateway de aplicação de rede az](/cli/azure/network/application-gateway/rule#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener myListener \
  --rule-type Basic \
  --redirect-config httpToHttps
```

## <a name="create-a-virtual-machine-scale-set"></a>Criar um conjunto de dimensionamento de máquinas virtuais

Neste exemplo, criar um conjunto nomeado de dimensionamento de máquina virtual *myvmss* que oferece funções de servidores para o conjunto de back-end no gateway de aplicação. As máquinas virtuais no conjunto de dimensionamento estão associadas *myBackendSubnet* e *appGatewayBackendPool*. Para criar a escala definida, pode utilizar [az vmss criar](/cli/azure/vmss#az_vmss_create).

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

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Para obter o endereço IP público do gateway de aplicação, pode utilizar [mostrar de ip público de rede az](/cli/azure/network/public-ip#az_network_public_ip_show). Copie o endereço IP público e cole-o na barra de endereço do browser.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Aviso seguro](./media/tutorial-http-redirect-cli/application-gateway-secure.png)

Para aceitar a segurança de aviso se utilizou um certificado autoassinado, selecione **detalhes** e, em seguida, **avance para a página Web**. O site NGINX protegido é apresentado como no exemplo seguinte:

![URL de base de teste no gateway de aplicação](./media/tutorial-http-redirect-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um certificado autoassinado
> * Configurar uma rede
> * Criar um gateway de aplicação com o certificado
> * Adicionar uma regra de serviço de escuta e o redirecionamento
> * Criar um conjunto com o conjunto de back-end predefinido de dimensionamento de máquina virtual

> [!div class="nextstepaction"]
> [Saiba mais sobre o que pode fazer com o gateway de aplicação](application-gateway-introduction.md)
