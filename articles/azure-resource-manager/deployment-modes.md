---
title: Modos de implementação do Azure Resource Manager | Documentos da Microsoft
description: Descreve como especificar se pretende utilizar um modo de implantação completas ou incrementais com o Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: c4347254df59c62085b2bfb195496bf479cf7b35
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344592"
---
# <a name="azure-resource-manager-deployment-modes"></a>Modos de implementação do Azure Resource Manager

Quando a implementação de recursos, especifica que a implementação é uma atualização incremental ou uma atualização completa.  A principal diferença entre esses dois modos é como o Resource Manager processa os recursos existentes no grupo de recursos que não estão no modelo. O modo predefinido é incremental.

## <a name="incremental-and-complete-deployments"></a>Implementações de incrementais e completas

Durante a implantação de recursos:

* No modo de conclusão, Gestor de recursos **elimina** recursos de que existem no grupo de recursos, mas não estão especificados no modelo.
* No modo de incremental, Gestor de recursos **deixa inalterado** recursos de que existem no grupo de recursos, mas não estão especificados no modelo.

Em ambos os modos, Gestor de recursos tenta criar todos os recursos especificados no modelo. Se o recurso já existe no grupo de recursos e as respetivas definições são iguais, a operação resulta numa nenhuma alteração. Se alterar os valores de propriedade para um recurso, o recurso é atualizado com os novos valores. Se tentar atualizar a localização ou o tipo de um recurso existente, a implementação falhar com um erro. Em vez disso, implementar um novo recurso com a localização ou escreva o que precisa.

Quando voltar a implementar um recurso no modo de incremental, especifique todos os valores de propriedade para o recurso, não apenas aqueles que estiver a atualizar. Se não especificar determinadas propriedades, o Resource Manager interpreta a atualização como substituir esses valores.

## <a name="example-result"></a>Resultado de exemplo

Para ilustrar a diferença entre os modos de incrementais e completas, considere o seguinte cenário.

**Grupo de recursos** contém:

* Recurso A
* Recurso B
* Recurso C

**Modelo** contém:

* Recurso A
* Recurso B
* Recurso D

Quando implementado num **incremental** modo, o grupo de recursos tem:

* Recurso A
* Recurso B
* Recurso C
* Recurso D

Quando implementado num **concluída** modo, C de recurso é eliminado. O grupo de recursos tem:

* Recurso A
* Recurso B
* Recurso D

## <a name="set-deployment-mode"></a>Definir o modo de implementação

Para definir o modo de implementação ao implementar com o PowerShell, utilize o `Mode` parâmetro.

```azurepowershell-interactive
New-AzureRmResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Para definir o modo de implementação ao implementar com a CLI do Azure, utilize o `mode` parâmetro.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Quando utilizar um [modelo ligado ou aninhado](resource-group-linked-templates.md), tem de definir o `mode` propriedade `Incremental`. Apenas os modelos de nível de raiz suportam o modo de implantação completa.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a criação de modelos do Resource Manager, veja [modelos Authoring Azure Resource Manager](resource-group-authoring-templates.md).
* Para saber mais sobre a implementação de recursos, veja [implementar uma aplicação com o modelo Azure Resource Manager](resource-group-template-deploy.md).
* Para ver as operações de um fornecedor de recursos, consulte [API REST do Azure](/rest/api/).
