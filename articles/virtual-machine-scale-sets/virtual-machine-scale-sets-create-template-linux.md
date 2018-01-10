---
title: "Criar um conjunto de dimensionamento de máquinas virtuais Linux com um modelo do Azure | Microsoft Docs"
description: "Saiba como criar rapidamente um dimensionamento de máquinas virtuais Linux com um modelo do Azure Resource Manager que implementa uma aplicação de exemplo e configura regras de dimensionamento automático"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: b07bdd0739dabb05ef7012051b7ac28af3aaddaf
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-linux-virtual-machine-scale-set-with-an-azure-template"></a>Criar um conjunto de dimensionamento de máquinas virtuais Linux com um modelo do Azure
Um conjunto de dimensionamento de máquinas virtuais permite implementar e gerir um conjunto de máquinas virtuais idênticas e de dimensionamento automático. Pode dimensionar o número de VMs no conjunto de dimensionamento manualmente ou definir regras para dimensionar automaticamente com base na utilização de recursos, como CPU, exigência de memória ou tráfego de rede. Neste artigo de introdução, vai criar um conjunto de dimensionamento de máquinas virtuais Linux com um modelo do Azure Resource Manager. Também pode criar um conjunto de dimensionamento com a [CLI 2.0 do Azure](virtual-machine-scale-sets-create-cli.md), o [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) ou o [portal do Azure](virtual-machine-scale-sets-create-portal.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="define-a-scale-set-in-a-template"></a>Definir um conjunto de dimensionamento num modelo
Os modelos do Azure Resource Manager permitem implementar grupos de recursos relacionados. Os modelos são escritos em JavaScript Object Notation (JSON) e definem o ambiente de toda a infraestrutura do Azure para a sua aplicação. Num único modelo, pode criar o conjunto de dimensionamento de máquinas virtuais, instalar aplicações e configurar regras de dimensionamento automático. Com a utilização de parâmetros e variáveis, este modelo pode ser reutilizado para atualizar conjuntos de dimensionamento existentes ou criar conjuntos de dimensionamento adicionais. Pode implementar modelos através do portal do Azure, da CLI 2.0 do Azure ou do Azure PowerShell, ou a partir de pipelines de integração contínua/entrega contínua (CI/CD).

Para obter mais informações sobre modelos, veja [Descrição geral do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment)

Para criar um dimensionamento com um modelo, tem de definir os recursos adequados. As partes principais do tipo de recurso de conjunto de dimensionamento de máquinas virtuais são:

| Propriedade                     | Descrição da propriedade                                  | Valor de modelo de exemplo                    |
|------------------------------|----------------------------------------------------------|-------------------------------------------|
| tipo                         | Tipo de recurso do Azure a criar                            | Microsoft.Compute/virtualMachineScaleSets |
| nome                         | O nome do conjunto de dimensionamento                                       | myScaleSet                                |
| localização                     | A localização para criar o conjunto de dimensionamento                     | EUA Leste                                   |
| sku.name                     | O tamanho da VM para cada instância do conjunto de dimensionamento                  | Standard_A1                               |
| sku.capacity                 | O número de instâncias de VM a criar inicialmente           | 2                                         |
| upgradePolicy.mode           | Modo de atualização de instâncias de VM quando ocorrem alterações              | Automático                                 |
| imageReference               | A plataforma ou imagem personalizada a utilizar para as instâncias de VM | Canonical Ubuntu Server 16.04-LTS         |
| osProfile.computerNamePrefix | O prefixo de nome para cada instância de VM                     | myvmss                                    |
| osProfile.adminUsername      | O nome de utilizador para cada instância de VM                        | azureuser                                 |
| osProfile.adminPassword      | A palavra-passe para cada instância de VM                        | P@ssw0rd!                                 |

 O exemplo seguinte mostra a definição do recurso do conjunto de dimensionamento principal. Para personalizar um modelo do conjunto de dimensionamento, pode alterar o tamanho ou a capacidade inicial da VM ou utilizar uma plataforma diferente ou uma imagem personalizada.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

 Para manter o exemplo curto, a configuração da placa de interface de rede virtual (NIC) não é apresentada. Também não são apresentados componentes adicionais, como um balanceador de carga. Um modelo de conjunto de dimensionamento completo é apresentado [no fim deste artigo](#deploy-the-template).


## <a name="install-an-application"></a>Instalar uma aplicação
Quando implementa um conjunto de dimensionamento, as extensões de VM podem fornecer tarefas de automatização e configuração pós-implementação, tais como a instalação de uma aplicação. Os scripts podem ser transferidos a partir do armazenamento do Azure ou do GitHub, ou fornecidos para o portal do Azure no runtime da extensão. Para aplicar uma extensão ao conjunto de dimensionamento, adicione a secção *extensionProfile* ao exemplo de recurso anterior. Normalmente, o perfil de extensão define as seguintes propriedades:

- Tipo de extensão
- Publicador da extensão
- Versão da extensão
- Localização dos scripts de configuração ou instalação
- Comandos para executar nas instâncias de VM

O modelo [servidor HTTP de Python em Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) utiliza a Extensão de Script Personalizado para instalar o [Bottle](http://bottlepy.org/docs/dev/), uma arquitetura Web Python, e um servidor HTTP simples. 

Dois scripts são definidos em **fileUris** - *installserver.sh* e *workserver.py*. Estes ficheiros são transferidos a partir do GitHub e, em seguida, o *commandToExecute* executa `bash installserver.sh` para instalar e configurar a aplicação:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "AppInstall",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "fileUris": [
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
          ],
          "commandToExecute": "bash installserver.sh"
        }
      }
    }
  ]
}
```


## <a name="deploy-the-template"></a>Implementar o modelo
Pode implementar o modelo [Servidor HTTP de Python em Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) com o botão **Implementar no Azure** seguinte. Este botão abre o portal do Azure, carrega o modelo completo e pede-lhe alguns parâmetros, tais como um nome de conjunto de dimensionamento, uma contagem de instâncias e as credenciais de administrador.

[![Implementar o modelo no Azure](media/virtual-machine-scale-sets-create-template/deploy-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-vmss-bottle-autoscale%2Fazuredeploy.json)

Também pode utilizar a CLI 2.0 do Azure para instalar o servidor HTTP de Python em Linux com [az group deployment create](/cli/azure/group/deployment#create) da seguinte forma:

```azurecli-interactive
# Create a resource group
az group create --name myResourceGroup --location EastUS

# Deploy template into resource group
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/azuredeploy.json
```

Responda aos pedidos para fornecer um nome do conjunto de dimensionamento, uma contagem de instâncias e as credenciais de administrador para as instâncias de VM. A criação do conjunto de dimensionamento e dos recursos de apoio demora algum tempo.


## <a name="test-your-sample-application"></a>Testar o exemplo de aplicação
Para ver a sua aplicação em ação, obtenha o endereço IP público do balanceador de carga com [az network public-ip list](/cli/azure/network/public-ip#show) da seguinte forma:

```azurecli-interactive
az network public-ip list \
    --resource-group myResourceGroup \
    --query [*].ipAddress -o tsv
```

Introduza o endereço IP público do balanceador de carga num browser no formato *http://publicIpAddress:9000/do_work*. O balanceador de carga distribui o tráfego para uma das suas instâncias de VM, conforme mostra o exemplo seguinte:

![Página Web predefinida no NGINX](media/virtual-machine-scale-sets-create-template/running-python-app.png)


## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, pode utilizar [az group delete](/cli/azure/group#delete) para remover o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados da seguinte forma:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Passos seguintes
Neste artigo de introdução, criou um conjunto de dimensionamento Linux com um modelo do Azure e utilizou a Extensão de Script Personalizado para instalar um servidor Web de Python básico nas instâncias da VM. Para maior escalabilidade e automatização, expanda o conjunto de dimensionamento com os seguintes artigos de procedimentos:

- [Implementar uma aplicação em conjuntos de dimensionamento de máquinas virtuais](virtual-machine-scale-sets-deploy-app.md)
- Dimensionar automaticamente com a [CLI do Azure](virtual-machine-scale-sets-autoscale-cli.md), o [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) ou o [portal do Azure](virtual-machine-scale-sets-autoscale-portal.md)
- [Utilizar as atualizações automáticas de SO para as instâncias de VM do seu conjunto de dimensionamento](virtual-machine-scale-sets-automatic-upgrade.md)
