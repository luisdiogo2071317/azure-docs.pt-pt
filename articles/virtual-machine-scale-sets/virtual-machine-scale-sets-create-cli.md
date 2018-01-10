---
title: "Criar um Conjunto de Dimensionamento de Máquinas Virtuais com a CLI 2.0 do Azure | Microsoft Docs"
description: "Saiba como criar rapidamente um dimensionamento de máquinas virtuais com o Azure PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: 222bfa1c3070fa4634cf5c48d934a6387c48a4b0
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Criar um Conjunto de Dimensionamento de Máquinas Virtuais com a CLI 2.0 do Azure
Um conjunto de dimensionamento de máquinas virtuais permite implementar e gerir um conjunto de máquinas virtuais idênticas e de dimensionamento automático. Pode dimensionar o número de VMs no conjunto de dimensionamento manualmente ou definir regras para dimensionar automaticamente com base na utilização de recursos, como CPU, exigência de memória ou tráfego de rede. Neste artigo de introdução, vai criar um conjunto de dimensionamento de máquinas virtuais com a CLI 2.0 do Azure. Também pode criar um conjunto de dimensionamento com o [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md), ou com o [portal do Azure](virtual-machine-scale-sets-create-portal.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-scale-set"></a>Criar um conjunto de dimensionamento
Para poder criar um conjunto de dimensionamento, crie primeiro um grupo de recursos com [az group create](/cli/azure/group#create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Agora, crie um conjunto de dimensionamento de máquinas virtuais com [az vmss create](/cli/azure/vmss#create). O exemplo seguinte cria um conjunto nomeado de dimensionamento *myScaleSet* e gera chaves SSH, caso não existam:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys
```

A criação e configuração de todas as VMs e recursos do conjunto de dimensionamento demora alguns minutos.


## <a name="install-nginx-webserver"></a>Instalar o webserver NGINX
Para testar o conjunto de dimensionamento, utilize a Extensão de Script Personalizado para transferir e executar um script que instale o NGINX nas instâncias de VM. Esta extensão é útil para a configuração pós-implementação, instalação de software ou qualquer outra tarefa de gestão/configuração. Para obter mais informações, veja a [Descrição geral da Extensão de Script Personalizado](../virtual-machines/linux/extensions-customscript.md).

Aplique a Extensão de Script Personalizado que instala o NGINX da seguinte forma:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'
```


## <a name="allow-web-traffic"></a>Permitir tráfego Web
Para permitir que o tráfego alcance o servidor Web, crie uma regra de balanceador de carga com [az network lb rule create](/cli/azure/network/lb/rule#create). O exemplo seguinte cria uma regra com o nome *myLoadBalancerRuleWeb*:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroup \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```


## <a name="test-your-web-server"></a>Testar o servidor Web
Para ver o seu servidor Web em ação, obtenha o endereço IP público do seu balanceador de carga com [az network public-ip show](/cli/azure/network/public-ip#show). O exemplo seguinte obtém o endereço IP para *myScaleSetLBPublicIP* criada como parte do conjunto de dimensionamento:

```azurecli-interactive 
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query [ipAddress] \
  --output tsv
```

Introduza o endereço IP público do balanceador de carga num browser. O balanceador de carga distribui o tráfego para uma das suas instâncias de VM, conforme mostra o exemplo seguinte:

![Página Web predefinida no NGINX](media/virtual-machine-scale-sets-create-cli/running-nginx-site.png)


## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, pode utilizar [az group delete](/cli/azure/group#delete) para remover o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados da seguinte forma:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Passos seguintes
Neste artigo de introdução, criou um conjunto de dimensionamento básico e utilizou a Extensão de Script Personalizado para instalar um servidor Web de NGINX básico nas instâncias da VM. Para maior escalabilidade e automatização, expanda o conjunto de dimensionamento com os seguintes artigos de procedimentos:

- [Implementar uma aplicação em conjuntos de dimensionamento de máquinas virtuais](virtual-machine-scale-sets-deploy-app.md)
- Dimensionar automaticamente com a [CLI do Azure](virtual-machine-scale-sets-autoscale-cli.md), o [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) ou o [portal do Azure](virtual-machine-scale-sets-autoscale-portal.md)
- [Utilizar as atualizações automáticas de SO para as instâncias de VM do seu conjunto de dimensionamento](virtual-machine-scale-sets-automatic-upgrade.md)