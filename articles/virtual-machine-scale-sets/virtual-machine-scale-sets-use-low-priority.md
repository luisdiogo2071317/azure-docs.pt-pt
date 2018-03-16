---
title: "Criar um conjunto de dimensionamento do Azure que utiliza VMs de baixa prioridade (pré-visualização) | Microsoft Docs"
description: "Saiba como criar conjuntos de dimensionamento de máquina virtual do Azure que utilizam as VMs de prioridade baixa para guardar os custos"
services: virtual-machine-scale-sets
documentationcenter: 
author: mmccrory
manager: rajraj
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: memccror
ms.openlocfilehash: 9e4970ecc538caab537281931b89bfd57d994cfa
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>Prioridade baixa VMs em conjuntos de dimensionamento (pré-visualização)

Utilizando VMs de prioridade baixa em conjuntos de dimensionamento permite-lhe tirar partido da nossa capacidade unutilized em permite uma redução significativa de custos. Em qualquer altura ao Azure tem de voltar a capacidade, a infraestrutura do Azure irá expulsar VMs de prioridade baixa. Por conseguinte, VMs de prioridade baixa são ótimos para cargas de trabalho que podem processar interrupções como batch processar tarefas, ambientes de desenvolvimento/teste, computação grandes cargas de trabalho e muito mais.

A quantidade de capacidade unutilized disponível pode variar em tamanho, região, hora do dia e muito mais. Quando define a implementar VMs de prioridade baixa em escala, o Azure irá alocar as VMs se há capacidade disponível, mas não existe nenhum SLA para estas VMs. Um conjunto de dimensionamento de prioridade baixa é implementado num domínio único de falha e oferece que garantias de sem elevada disponibilidade.

> [!NOTE]
> Conjuntos de dimensionamento de prioridade baixa estão em pré-visualização e está preparado para os cenários de desenvolvimento e teste. 

## <a name="eviction-policy"></a>Política de expulsão

Quando definir a escala de prioridade baixa são expulso VMs, serão movidas para o estado parado (desalocado) por predefinição. Com esta política de expulsão, pode voltar a implementar instâncias expulsos, mas não existe nenhuma garantia de que a alocação será concluída com êxito. As VMs paradas irão contabilizada contra a sua quota de instância do conjunto de dimensionamento e será cobrada para os discos subjacentes. 

Se pretender que as suas VMs no seu conjunto ser eliminado quando estes são expulso de dimensionamento de prioridade baixa, pode definir a política de expulsão para eliminar na sua [modelo Azure Resource Manager](#use-azure-resource-manager-templates). Com a política de expulsão definida como eliminar, pode criar novas VMs ao aumentar a propriedade de contagem de instância de conjunto de dimensionamento. As VMs expulsos são eliminadas, juntamente com os respetivos discos subjacentes e, por conseguinte, não será cobrado para o armazenamento. Também pode utilizar a funcionalidade de dimensionamento automático de conjuntos de dimensionamento para automaticamente tentar compensar expulsos VMs, no entanto, não existe nenhuma garantia de que a alocação será concluída com êxito. Recomenda-se que apenas utilizar a funcionalidade de dimensionamento automático em conjuntos de dimensionamento de prioridade baixa quando definir a política de expulsão eliminar para evitar o custo dos discos e atingir os limites de quota. 

> [!NOTE]
> Durante a pré-visualização, poderá definir a política de expulsão utilizando [modelos Azure Resource Manager](#use-azure-resource-manager-templates). 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Implementar VMs de prioridade baixa numa escala define

Para implementar prioridade baixa VMs em conjuntos de dimensionamento, pode definir o novo *prioridade* sinalizador para *baixa*. Todas as VMs no seu conjunto de dimensionamento serão definidas em prioridade baixa. Para criar um conjunto com as VMs de prioridade baixa de dimensionamento, utilize um dos seguintes métodos:
- [CLI 2.0 do Azure](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Modelos Azure Resource Manager](#use-azure-resource-manager-templates)

## <a name="use-the-azure-cli-20"></a>Utilizar a CLI do Azure 2.0

O processo de criação de um conjunto com as VMs de prioridade baixa de dimensionamento é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-cli.md). Basta adicionar o ' – Prioridade ' parâmetro para a cli chamar e defina-o como *baixa* conforme mostrado no exemplo abaixo:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Low
```

## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell

O processo de criação de um conjunto com as VMs de prioridade baixa de dimensionamento é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-powershell.md).
Basta adicionar a '-Prioridade ' parâmetro para o [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) e defina-o como *baixa* conforme mostrado no exemplo abaixo:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Low"
```

## <a name="use-azure-resource-manager-templates"></a>Utilizar modelos Azure Resource Manager

O processo de criação de um conjunto de dimensionamento que utiliza a prioridade baixa VMs é o mesmo conforme detalhado no artigo Introdução ao obter para [Linux](virtual-machine-scale-sets-create-template-linux.md) ou [Windows](virtual-machine-scale-sets-create-template-windows.md). Adicionar a propriedade 'priority' para o *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* recurso de tipo no modelo e especifique *baixa* como o valor. Certifique-se de utilizar *2017-10-30-preview* versão da API ou superior. 

Para definir a política de expulsão para eliminação, adicione o parâmetro de 'evictionPolicy' e defina-o como *eliminar*.

O exemplo seguinte cria um conjunto nomeado de dimensionamento do baixa Linux *myScaleSet* no *Central EUA oeste*, que será *eliminar* as VMs na escala definido no expulsão:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_DS2_v2",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
       "priority": "Low",
       "evictionPolicy": "delete",
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
## <a name="next-steps"></a>Passos seguintes
Agora que criou um conjunto com as VMs de prioridade baixa de dimensionamento, experimente implementar nosso [modelo de dimensionamento automático com prioridade baixa](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Veja o [conjunto de dimensionamento de máquina virtual página de preços](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) para detalhes de preços.