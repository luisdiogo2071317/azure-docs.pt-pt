---
title: Especifique o grupo de recursos para as VMs no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como especificar um grupo de recursos para as VMs num laboratório no Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2019
ms.author: spelluru
ms.openlocfilehash: 94e5f5b29e93409df2373cf6c56e8185dc5373a2
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312979"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Especifique um grupo de recursos para máquinas virtuais do laboratório no Azure DevTest Labs
Como proprietário de um laboratório, pode configurar as máquinas virtuais do laboratório ser criado num grupo de recursos específico. Esta funcionalidade ajuda-o nos seguintes cenários: 

- Tiver menos grupos de recursos criados por laboratórios na sua subscrição.
- Ter os laboratórios de operar dentro de um conjunto fixo de grupos de recursos configurada por si
- Contornar as restrições e aprovações requeridas para a criação de grupos de recursos na sua subscrição do Azure.
- Consolidar todos os recursos do laboratório dentro de um grupo de recursos único para simplificar a controlar esses recursos e aplicar [políticas](../governance/policy/overview.md) geri-los ao nível do grupo de recursos.

Com esta funcionalidade, pode utilizar um script para especificar um novo ou um grupo de recursos existente na sua subscrição do Azure para todos os seu laboratório VMs. Atualmente, o DevTest Labs suporta esta funcionalidade através de uma API. 

## <a name="api-to-configure-a-resource-group-for-lab-virtual-machines"></a>API para configurar um grupo de recursos para máquinas virtuais do laboratório
Agora vamos examinar as opções que tem como proprietário de um laboratório ao utilizar esta API: 

- Pode escolher o **grupo de recursos do laboratório** todas as máquinas virtuais.
- Pode escolher uma **grupo de recursos existente** que não seja o grupo de recursos do laboratório para todas as máquinas virtuais.
- Pode introduzir um **novo grupo de recursos** nome todas as máquinas virtuais.
- Pode continuar com o comportamento existente, ou seja, um grupo de recursos é criado para cada VM no laboratório.
 
Esta definição aplica-se para novas máquinas de virtuais criadas no laboratório. As VMs mais antigas em seu laboratório que foram criadas em seus próprios grupos de recursos continuam a não são afetados. Ambientes criados no seu laboratório continuam a permanecer em seus próprios grupos de recursos.

### <a name="how-to-use-this-api"></a>Como utilizar esta API:
- Utilizar a versão de API **2018_10_15_preview** ao utilizar esta API. 
- Se especificar um novo grupo de recursos, certifique-se de que tenha **permissões de escrita em grupos de recursos** na sua subscrição. Sem permissões de escrita, criação de novas máquinas virtuais no resultado de grupo de recursos especificado numa falha. 
- Ao utilizar a API, passar o **completa de ID do grupo de recursos**. Por exemplo: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Certifique-se de que o grupo de recursos está na mesma subscrição que o laboratório. 

## <a name="use-powershell"></a>Utilizar o PowerShell 
Exemplo a seguir descreve como criar todas as máquinas virtuais do laboratório num novo grupo de recursos com um script do PowerShell.

```PowerShell
[CmdletBinding()]
Param(
    $subId,
    $labRg,
    $labName,
    $vmRg
)

az login | out-null

az account set --subscription $subId | out-null

$rgId = "/subscriptions/"+$subId+"/resourceGroups/"+$vmRg

"Updating lab '$labName' with vm rg '$rgId'..."

az resource update -g $labRg -n $labName --resource-type "Microsoft.DevTestLab/labs" --api-version 2018-10-15-preview --set properties.vmCreationResourceGroupId=$rgId

"Done. New virtual machines will now be created in the resource group '$vmRg'."
```

Invoca o script usando o seguinte comando (ResourceGroup.ps1 é o ficheiro que contém o anterior do script): 

```PowerShell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-azure-resource-manager-template"></a>Utilizar o modelo do Azure Resource Manager
Se estiver a utilizar o modelo Azure Resource Manager para criar um laboratório, utilize o **vmCreationResourceGroupId** na seção de propriedades de laboratório de modelo do Resource Manager, conforme mostrado no exemplo a seguir:

```json
        {
            "type": "microsoft.devtestlab/labs",
            "name": "[parameters('lab_name')]",
            "apiVersion": "2018_10_15_preview",
            "location": "eastus",
            "tags": {},
            "scale": null,
            "properties": {
                "vmCreationResourceGroupId": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>",
                "labStorageType": "Premium",
                "premiumDataDisks": "Disabled",
                "provisioningState": "Succeeded",
                "uniqueIdentifier": "000000000f-0000-0000-0000-00000000000000"
            },
            "dependsOn": []
        },
```


## <a name="next-steps"></a>Passos Seguintes
Consulte os seguintes artigos: 

- [Definir políticas para um laboratório](devtest-lab-get-started-with-lab-policies.md)
- [Perguntas mais frequentes](devtest-lab-faq.md)
