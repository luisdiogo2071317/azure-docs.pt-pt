---
title: Criar um gateway de aplicação com uma firewall de aplicação web - CLI do Azure | Microsoft Docs
description: Saiba como criar um gateway de aplicação com uma firewall de aplicação web utilizando a CLI do Azure.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: victorh
ms.openlocfilehash: 87125b68c81af07d0ecd9693fdf7e2dc00a93324
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-cli"></a>Criar um gateway de aplicação com uma firewall de aplicação web utilizando a CLI do Azure

Pode utilizar a CLI do Azure para criar um [gateway de aplicação](application-gateway-introduction.md) com um [firewall de aplicações web](application-gateway-web-application-firewall-overview.md) (WAF) que utiliza um [conjunto de dimensionamento da máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). As utilizações WAF [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) regras para proteger a sua aplicação. Estas regras incluem a proteção contra ataques, tais como a injeção de SQL, ataques de scripts entre sites e hijacks de sessão. 

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação com WAF ativada
> * Criar um conjunto de dimensionamento de máquinas virtuais
> * Criar uma conta de armazenamento e configurar o diagnóstico

![Exemplo de firewall de aplicação Web](./media/application-gateway-web-application-firewall-cli/scenario-waf.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Criar um grupo de recursos do Azure com o nome *myResourceGroupAG* com [criar grupo az](/cli/azure/group#az_group_create).

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede

A rede virtual e sub-redes são utilizadas para fornecer conectividade de rede para o gateway de aplicação e os respetivos recursos associados. Criar a rede virtual denominada *myVNet* e sub-rede designada *myAGSubnet* com [az rede vnet criar](/cli/azure/network/vnet#az_network_vnet_create) e [sub-rede da vnet az rede criar](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Criar um endereço IP público com o nome *myAGPublicIPAddress* com [az público-ip da rede criar](/cli/azure/network/public-ip#az_network_public_ip_create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myBackendSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myAGSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway-with-a-waf"></a>Criar um gateway de aplicação com uma WAF

Pode utilizar [criar gateway de aplicação do rede az](/cli/azure/application-gateway#az_application_gateway_create) para criar o gateway de aplicação com o nome *myAppGateway*. Quando cria um gateway de aplicação utilizando a CLI do Azure, especifique as informações de configuração, tais como a capacidade, sku e definições de HTTP. O gateway de aplicação é atribuído a *myAGSubnet* e *myPublicIPSddress* que criou anteriormente.

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --capacity 2 \
  --sku WAF_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
az network application-gateway waf-config set \
  --enabled true \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --firewall-mode Detection \
  --rule-set-version 3.0
```

Pode demorar alguns minutos até o gateway de aplicação a ser criado. Depois de criado o gateway de aplicação, pode ver estas novas funcionalidades do mesmo:

- *appGatewayBackendPool* -um gateway de aplicação tem de ter, pelo menos, um conjunto de endereços de back-end.
- *appGatewayBackendHttpSettings* -Especifica que a porta 80 e um protocolo HTTP é utilizado para comunicação.
- *appGatewayHttpListener* -o serviço de escuta de predefinição associado *appGatewayBackendPool*.
- *appGatewayFrontendIP* -atribui *myAGPublicIPAddress* para *appGatewayHttpListener*.
- *rule1* - predefinição encaminhamento regra que está associada a *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>Criar um conjunto de dimensionamento de máquinas virtuais

Neste exemplo, crie um conjunto de dimensionamento de máquina virtual que fornece dois servidores para o conjunto de back-end no gateway de aplicação. As máquinas virtuais no conjunto de dimensionamento estão associadas a *myBackendSubnet* sub-rede. Para criar a escala definida, pode utilizar [az vmss criar](/cli/azure/vmss#az_vmss_create).

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

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/vhorne/samplescripts/master/install_nginx.sh"],"commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Criar uma conta de armazenamento e configurar o diagnóstico

Neste tutorial, o gateway de aplicação utiliza uma conta do storage para armazenar dados para efeitos de deteção e prevenção. Também pode utilizar a análise de registos ou Hub de eventos para registar dados. 

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Criar uma conta de armazenamento com o nome *myagstore1* com [criar conta de armazenamento az](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create).

```azurecli-interactive
az storage account create \
  --name myagstore1 \
  --resource-group myResourceGroupAG \
  --location eastus \
  --sku Standard_LRS \
  --encryption blob
```

### <a name="configure-diagnostics"></a>Configurar o diagnóstico

Configure diagnósticos para registar dados para os registos ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog e ApplicationGatewayFirewallLog. Substitute `<subscriptionId>` com o identificador de subscrição e, em seguida, configure o diagnóstico com [criar definições de diagnóstico do monitor az](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_create).

```azurecli-interactive
appgwid=$(az network application-gateway show --name myAppGateway --resource-group myResourceGroupAG --query id -o tsv)
storeid=$(az storage account show --name myagstore1 --resource-group myResourceGroupAG --query id -o tsv)
az monitor diagnostic-settings create --name appgwdiag --resource $appgwid \
  --logs '[ { "category": "ApplicationGatewayAccessLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayPerformanceLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayFirewallLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --storage-account $storeid
```

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Para obter o endereço IP público do gateway de aplicação, utilize [mostrar de ip público de rede az](/cli/azure/network/public-ip#az_network_public_ip_show). Copie o endereço IP público e cole-o na barra de endereço do browser.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![URL de base de teste no gateway de aplicação](./media/application-gateway-web-application-firewall-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação com WAF ativada
> * Criar um conjunto de dimensionamento de máquinas virtuais
> * Criar uma conta de armazenamento e configurar o diagnóstico

Para obter mais informações sobre gateways de aplicação e os recursos associados, avance para os artigos de procedimentos.