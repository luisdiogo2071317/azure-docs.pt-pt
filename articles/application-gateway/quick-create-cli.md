---
title: Início Rápido - Direcionar tráfego da Web com o Gateway de Aplicação do Azure - CLI do Azure | Microsoft Docs
description: Saiba como utilizar a CLI do Azure para criar um Gateway de Aplicação do Azure que direciona o tráfego da Web para máquinas virtuais num conjunto de back-end.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: quickstart
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 23ab97918d7def744f9ac3427faa3743b15124eb
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "42023154"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Início Rápido: Direcionar tráfego da Web com o Gateway de Aplicação do Azure - CLI do Azure

Com o Gateway de Aplicação do Azure, pode direcionar o tráfego da Web de aplicações para recursos específicos ao atribuir serviços de escuta a portas, criar regras e adicionar recursos a um conjunto de back-end.

Este início rápido mostra como utilizar a CLI do Azure para criar rapidamente o gateway de aplicação com duas máquinas virtuais no respetivo conjunto de back-end. Em seguida, vai testá-lo para se certificar de que está a funcionar corretamente.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este início rápido requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Terá de criar sempre recursos num grupo de recursos. Crie um grupo de recursos com [az group create](/cli/azure/group#az-group-create). 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroupAG* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede 

Terá de criar uma rede virtual para o gateway de aplicação conseguir comunicar com outros recursos. Pode criar uma rede virtual ao mesmo tempo que cria o gateway de aplicação. Neste exemplo, são criadas duas sub-redes: uma para o gateway de aplicação e a outra para as máquinas virtuais. 

Crie a rede virtual e a sub-rede com [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). Crie o endereço IP público com [az network public-ip create](/cli/azure/network/public-ip#az-public-ip-create).

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
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-backend-servers"></a>Criar servidores de back-end

Neste exemplo, vai criar duas máquinas virtuais para serem utilizadas como servidores de back-end para o gateway de aplicação. 

### <a name="create-two-virtual-machines"></a>Criar duas máquinas virtuais

Também vai instalar o NGINX nas máquinas virtuais para verificar se o gateway de aplicação foi criado com êxito. Pode utilizar o ficheiro de configuração cloud-init para instalar o NGINX e executar uma aplicação Node.js "Hello World" numa máquina virtual Linux. 

Na shell atual, crie um ficheiro com o nome cloud-init.txt e copie e cole a seguinte configuração na shell. Certifique-se de que copia o ficheiro cloud-init completo corretamente, especialmente a primeira linha:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

Crie as interfaces de rede com [az network nic create](/cli/azure/network/nic#az-network-nic-create). Crie as máquinas virtuais com [az vm create](/cli/azure/vm#az-vm-create).

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupAG \
    --name myNic$i \
    --vnet-name myVNet \
    --subnet myBackendSubnet
  az vm create \
    --resource-group myResourceGroupAG \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>Criar o gateway de aplicação

Crie um gateway de aplicação com [az network application-gateway create](/cli/azure/network/application-gateway#az-application-gateway-create). Quando cria um gateway de aplicação com a CLI do Azure, especifica informações de configuração, tais como a capacidade, sku e definições de HTTP. Os endereços IP privados das interfaces de rede são adicionados como servidores no conjunto de back-end do gateway de aplicação.

```azurecli-interactive
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$address1" "$address2"
```

A criação do gateway de aplicação pode demorar até 30 minutos. Depois de criado o gateway de aplicação, pode ver estas funcionalidades do mesmo:

- *appGatewayBackendPool* - um gateway de aplicação tem de ter, pelo menos, um conjunto de endereços de back-end.
- *appGatewayBackendHttpSettings* - especifica que a porta 80 e um protocolo HTTP são utilizados para a comunicação.
- *appGatewayHttpListener* - o serviço de escuta predefinido associado a *appGatewayBackendPool*.
- *appGatewayFrontendIP* - atribui *myAGPublicIPAddress* a *appGatewayHttpListener*.
- *rule1* - a regra de encaminhamento predefinida associada a *appGatewayHttpListener*.

## <a name="test-the-application-gateway"></a>Testar o gateway de aplicação

Não é preciso instalar o NGINX para criar o gateway de aplicação, mas instalou-o neste início rápido para verificar se o gateway de aplicação foi criado com êxito. Para obter o endereço IP público do gateway de aplicação, utilize [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show). Copie o endereço IP público e cole-o na barra de endereço do browser.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
``` 

![Testar o gateway de aplicação](./media/quick-create-cli/application-gateway-nginxtest.png)

Quando atualizar o browser, deverá ver o nome da outra VM.

## <a name="clean-up-resources"></a>Limpar recursos

Primeiro, explore os recursos que foram criados com o gateway de aplicação e, em seguida, quando já não for necessário, pode utilizar o comando [az group delete](/cli/azure/group#az-group-delete) para remover o grupo de recursos, o gateway de aplicação e todos os recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```
 
## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Gerir o tráfego da Web com um gateway de aplicação com a CLI do Azure](./tutorial-manage-web-traffic-cli.md)

