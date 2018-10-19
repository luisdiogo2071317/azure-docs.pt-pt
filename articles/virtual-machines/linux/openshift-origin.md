---
title: Implementar OKD no Azure | Documentos da Microsoft
description: Implemente OKD no Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: e525bf0a5aa9bda7fdbbcefc4cb5b683c7fabc3b
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426231"
---
# <a name="deploy-okd-in-azure"></a>Implementar OKD no Azure

Pode utilizar uma das duas formas de implementar OKD (anteriormente conhecido como o OpenShift Origin) no Azure:

- Pode implementar manualmente todos os componentes de infraestrutura do Azure necessários e, em seguida, siga o OKD [documentação](https://docs.okd.io/3.10/welcome/index.html).
- Também pode utilizar existente [modelo do Resource Manager](https://github.com/Microsoft/openshift-origin) que simplifica a implementação do OKD cluster.

## <a name="deploy-by-using-the-okd-template"></a>Implementar utilizando o modelo OKD

Utilize o `appId` valor do principal de serviço que criou anteriormente para o `aadClientId` parâmetro.

O exemplo seguinte cria um ficheiro de parâmetros com o nome azuredeploy com todas as entradas necessárias.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

### <a name="deploy-by-using-azure-cli"></a>Implementar com a CLI do Azure


> [!NOTE] 
> Requer que o seguinte comando da CLI do Azure 2.0.8 ou posterior. Pode verificar a versão da CLI com o `az --version` comando. Para atualizar a versão da CLI, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

O exemplo seguinte implementa o cluster OKD e todos os recursos relacionados num grupo de recursos com o nome myResourceGroup, com um nome de implementação de myOpenShiftCluster. O modelo é referenciado diretamente a partir do repositório do GitHub, utilizando um ficheiro de parâmetros local com o nome azuredeploy.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

A implementação demora pelo menos 25 minutos para concluir, dependendo do número total de nós implementados. O URL da consola do OKD e o nome DNS das impressões mestres OpenShift terminal quando a conclusão da implementação.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-okd-cluster"></a>Ligar ao OKD cluster

Quando a conclusão da implementação, ligar à consola do OKD com o seu browser, utilizando o `OpenShift Console Uri`. Em alternativa, pode ligar ao mestre de OKD utilizando o seguinte comando:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Limpar recursos

Utilize o [eliminação do grupo de az](/cli/azure/group#az_group_delete) comando para remover o grupo de recursos, o OpenShift cluster e todos os recursos relacionados quando já não forem necessários.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

- [Tarefas de pós-implementação](./openshift-post-deployment.md)
- [Resolver problemas de implementação do OpenShift](./openshift-troubleshooting.md)
- [Guia de introdução OKD](https://docs.okd.io/latest/getting_started/index.html)
