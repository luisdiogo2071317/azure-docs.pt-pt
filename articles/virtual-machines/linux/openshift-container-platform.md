---
title: Implementar o OpenShift Container Platform no Azure | Documentos da Microsoft
description: Implemente o OpenShift Container Platform no Azure.
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
ms.openlocfilehash: a275df4567053149688694315ff24ac1ad7f711f
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43186919"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Implementar o OpenShift Container Platform no Azure

Pode utilizar um dos vários métodos para implementar o OpenShift Container Platform no Azure:

- Pode implementar manualmente os componentes de infraestrutura do Azure necessários e, em seguida, siga o OpenShift Container Platform [documentação](https://docs.openshift.com/container-platform/3.10/welcome/index.html).
- Também pode utilizar existente [modelo do Resource Manager](https://github.com/Microsoft/openshift-container-platform/) que simplifica a implementação do cluster OpenShift Container Platform.
- Outra opção consiste em utilizar o [oferta do Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Para todas as opções, é necessária uma subscrição de Red Hat. Durante a implantação, a instância de Red Hat Enterprise Linux está registada para a subscrição do Red Hat e anexada ao ID de agrupamento que contém a elegibilidade para o OpenShift Container Platform.
Certifique-se de que tem um nome de utilizador válido do Red Hat subscrição Manager (RHSM), a palavra-passe e o ID do conjunto. Pode verificar estas informações ao iniciar sessão para https://access.redhat.com.

## <a name="deploy-by-using-the-openshift-container-platform-resource-manager-template"></a>Implementar utilizando o modelo de Gestor de recursos do OpenShift Container Platform

Para implementar com o modelo do Resource Manager, use um ficheiro de parâmetros para fornecer os parâmetros de entrada. Para personalizar qualquer um dos itens de implementação que não são respondidos utilizando parâmetros de entrada, bifurcar o repositório do GitHub e altere os itens adequados.

Algumas opções de personalização comuns incluem, mas não sejam limitam a:

- Rede virtual CIDR (variável no azuredeploy. JSON)
- Tamanho de VM de bastion (variável no azuredeploy. JSON)
- Convenções de nomenclatura (variáveis no azuredeploy. JSON)
- Especificações de cluster do OpenShift, modificado através do ficheiro de anfitriões (deployOpenShift.sh)

### <a name="configure-the-parameters-file"></a>Configurar o ficheiro de parâmetros

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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "true"
        },
        "enableCockpit": {
            "value": "false"
        },
        "rhsmUsernamePasswordOrActivationKey": {
            "value": "usernamepassword"
        },
        "rhsmUsernameOrOrgId": {
            "value": "{RHSM Username}"
        },
        "rhsmPasswordOrActivationKey": {
            "value": "{RHSM Password}"
        },
        "rhsmPoolId": {
            "value": "{Pool ID}"
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
        "enableAzure": {
            "value": "true"
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

Substitua os itens entre parênteses, com as suas informações específicas.

### <a name="deploy-by-using-azure-cli"></a>Implementar com a CLI do Azure

> [!NOTE] 
> Requer que o seguinte comando da CLI do Azure 2.0.8 ou posterior. Pode verificar a versão da CLI com o `az --version` comando. Para atualizar a versão da CLI, veja [instalar o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

O exemplo seguinte implementa o cluster de OpenShift e todos os recursos relacionados num grupo de recursos com o nome myResourceGroup, com um nome de implementação de myOpenShiftCluster. O modelo é referenciado diretamente a partir do repositório do GitHub e um local parâmetros ficheiro azuredeploy. ini ao ficheiro é utilizado.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

A implantação se, pelo menos, 30 minutos a concluir, consoante o número total de nós implementados. O URL da consola do OpenShift e o nome DNS das impressões mestres OpenShift terminal quando a conclusão da implementação.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-by-using-the-openshift-container-platform-azure-marketplace-offer"></a>Implementar com a oferta do OpenShift Container Platform Azure Marketplace

A forma mais simples de implementar o OpenShift Container Platform no Azure está a utilizar o [oferta do Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Esta é a opção mais simples, mas ele também tem limitada capacidades de personalização. A oferta inclui três opções de configuração:

- **Pequenas**: implementa um cluster de não ser de elevada disponibilidade (HA) com um nó principal, uma infraestrutura nó, dois nós de aplicativo e um bastion nó. Todos os nós são os tamanhos de VM de DS2v2 padrão. Este cluster requer 10 total de núcleos e é ideal para testes em pequena escala.
- **Médio**: implementa um cluster do HA com três nós principais, dois nós de infraestrutura, quatro nós de aplicativo e o nó de um bastion. Todos os nós, exceto o nó de bastion são os tamanhos de VM de DS3v2 padrão. O nó de bastion é uma DS2v2 padrão. Este cluster necessita de 38 núcleos.
- **Grandes**: implementa um cluster do HA com três nós principais, dois nós de infraestrutura, seis nós de aplicativo e o nó de um bastion. Os nós do mestre e de infraestrutura são os tamanhos de VM de DS3v2 padrão. Os nós de aplicativo são os tamanhos de VM de DS4v2 padrão e o nó de bastion for um DS2v2 padrão. Este cluster requer 70 núcleos.

Configuração do fornecedor de soluções de Cloud do Azure é opcional para tamanhos de cluster médias e grandes. O tamanho de pequeno cluster não dá uma opção para configurar o fornecedor de soluções de Cloud do Azure.

## <a name="connect-to-the-openshift-cluster"></a>Ligue ao cluster do OpenShift

Quando a conclusão da implementação, ligar à consola do OpenShift com o seu browser, utilizando o `OpenShift Console Uri`. Em alternativa, pode ligar ao mestre de OpenShift utilizando o seguinte comando:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Limpar recursos

Utilize o [eliminação do grupo de az](/cli/azure/group#az_group_delete) comando para remover o grupo de recursos, o OpenShift cluster e todos os recursos relacionados quando já não forem necessários.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

- [Tarefas de pós-implementação](./openshift-post-deployment.md)
- [Resolver problemas de implementação do OpenShift no Azure](./openshift-troubleshooting.md)
- [Introdução ao OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
