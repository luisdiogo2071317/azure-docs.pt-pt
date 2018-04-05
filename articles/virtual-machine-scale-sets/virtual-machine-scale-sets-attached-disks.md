---
title: Discos de Dados Anexados a Conjuntos de Dimensionamento de Máquinas Virtuais do Azure | Microsoft Docs
description: Saiba como utilizar discos de dados anexados com conjuntos de dimensionamento de máquinas virtuais
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: negat
ms.openlocfilehash: ec11a2d66530129fb61d97681e6882b887c8654c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Conjunto de dimensionamento de máquinas virtuais do Azure e discos de dados anexados
Para expandir o armazenamento disponível, os [conjuntos de dimensionamento de máquinas virtuais](/azure/virtual-machine-scale-sets/) do Azure suportam instâncias de VM com discos de dados anexados. Pode anexar os discos de dados quando o conjunto de dimensionamento é criado ou a um conjunto de dimensionamento existente.

> [!NOTE]
>  Quando cria um conjunto de dimensionamento com discos de dados anexados, tem de montar e formatar os discos a partir de uma VM para poder utilizá-los (tal como acontece com VMs do Azure autónomas). Uma forma conveniente de executar este processo é utilizar uma Extensão de Script Personalizado que chama um script para particionar e formatar todos os discos de dados numa VM. Para obter exemplos deste processo, veja [CLI 2.0 do Azure](tutorial-use-disks-cli.md#prepare-the-data-disks) [Azure PowerShell](tutorial-use-disks-powershell.md#prepare-the-data-disks).


## <a name="create-and-manage-disks-in-a-scale-set"></a>Criar e gerir discos num conjunto de dimensionamento
Para obter informações detalhadas sobre como criar um conjunto de dimensionamento com discos de dados anexados, preparar e formatar ou adicionar e remover discos de dados, veja um dos seguintes tutoriais:

- [CLI 2.0 do Azure](tutorial-use-disks-cli.md)
- [Azure PowerShell](tutorial-use-disks-powershell.md)

O resto deste artigo descreve casos de utilização específicos, tais como clusters do Service Fabric que necessitam de discos de dados ou anexar discos de dados existentes com conteúdo a um conjunto de dimensionamento.


## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Criar um cluster do Service Fabric com discos de dados anexados
Cada [tipo de nó](../service-fabric/service-fabric-cluster-nodetypes.md) num cluster do [Service Fabric](/azure/service-fabric) em execução no Azure é suportado por um conjunto de dimensionamento de máquinas virtuais.  Ao utilizar um modelo do Azure Resource Manager, pode anexar discos de dados aos conjuntos de dimensionamento que compõem o cluster do Service Fabric. Pode utilizar um [modelo existente](https://github.com/Azure-Samples/service-fabric-cluster-templates) como um ponto de partida. No modelo, inclua uma secção _dataDisks_ no _storageProfile_ do(s) recursos _Microsoft.Compute/virtualMachineScaleSets_ e implemente o modelo. O exemplo seguinte anexa um disco de dados de 128 GB:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

Pode automaticamente criar partições, formatar e montar os discos de dados quando o cluster é implementado.  Adicione uma extensão de script personalizado ao _extensionProfile_ do _virtualMachineProfile_ do(s) conjunto(s) de dimensionamento.

Para preparar automaticamente o(s) disco(s) de dados num cluster do Windows, adicione o seguinte:

```json
{
    "name": "customScript",    
    "properties": {    
        "publisher": "Microsoft.Compute",    
        "type": "CustomScriptExtension",    
        "typeHandlerVersion": "1.8",    
        "autoUpgradeMinorVersion": true,    
        "settings": {    
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Para preparar automaticamente o(s) disco(s) de dados num cluster do Linux, adicione o seguinte:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```


## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Adicionar discos de dados pré-preenchidos a um conjunto de dimensionamento existente 
> Quando adicionar discos a um modelo de conjunto de dimensionamento existente, por predefinição, o disco é sempre criado vazio. Este cenário também inclui novas instâncias criadas pelo conjunto de dimensionamento. Este comportamento acontece porque a definição do conjunto de dimensionamento tem um disco de dados vazio. Para criar unidades de dados pré-preenchidas para um modelo de conjunto de dimensionamento existente, pode escolher entre duas opções:

* Copie dados da VM 0 da instância para o(s) disco(s) de dados nas outras VMs, executando um script personalizado.
* Crie uma imagem gerida com o disco do SO e o disco de dados (com os dados necessários) e crie um novo conjunto de dimensionamento com a imagem. Desta forma, cada nova VM criada tem um disco de dados que é fornecido na definição do conjunto de dimensionamento. Uma vez que esta definição se refere a uma imagem com um disco de dados que tem dados personalizados, cada máquina virtual no conjunto de dimensionamento tem estas alterações.

> A forma de criar uma imagem personalizada pode ser encontrada aqui: [Criar uma imagem gerida de uma VM generalizada no Azure](/azure/virtual-machines/windows/capture-image-resource/) 

> O utilizador tem de capturar a VM 0 da instância que tem os dados requeridos e, em seguida, utilizar o vhd dessa definição de imagem.


## <a name="additional-notes"></a>Notas adicionais
O suporte para o Azure Managed Disks e para discos de dados anexados a conjuntos de dimensionamento está disponível na versão da API [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) ou posterior da API Microsoft.Compute.

Na implementação inicial do suporte para discos anexados para conjuntos de dimensionamento, não pode anexar nem desanexar discos de dados a/de VMs individuais num conjunto de dimensionamento.

O suporte do portal do Azure para discos de dados anexados em conjuntos de dimensionamento é limitado inicialmente. Dependendo das suas necessidades, pode utilizar modelos do Azure, a CLI, o PowerShell, SDKs e a API REST para gerir discos anexados.


