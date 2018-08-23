---
title: Desenvolver modelos para o Azure Stack | Documentos da Microsoft
description: Conheça práticas recomendadas do modelo do Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: d09dec2f327d8b5911a4e55832ba106838c7ebc3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057682"
---
# <a name="azure-resource-manager-template-considerations"></a>Considerações sobre os modelos do Azure Resource Manager

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Ao desenvolver seu aplicativo, é importante garantir a portabilidade de modelo entre o Azure e o Azure Stack. Este artigo fornece considerações para o desenvolvimento do Azure Resource Manager [modelos](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf), pelo que pode o protótipo a implementação de aplicação e teste no Azure sem acesso a um ambiente do Azure Stack.

## <a name="resource-provider-availability"></a>Disponibilidade do fornecedor de recursos

O modelo que estiver a planear implementar apenas tem de utilizar os serviços do Microsoft Azure que já estão disponíveis ou em pré-visualização no Azure Stack.

## <a name="public-namespaces"></a>Espaços de nomes públicos

Como Azure Stack está hospedado no seu datacenter, tem espaços de nomes de ponto final de serviço diferentes que a nuvem pública do Azure. Como resultado, os pontos finais públicos codificado em modelos do Azure Resource Manager falharem ao tentar implementá-las para o Azure Stack. Dinamicamente que pode criar pontos finais de serviço com o *referência* e *concatenar* funções para recuperar os valores do fornecedor de recursos durante a implementação. Por exemplo, em vez de codificar *blob.core.windows.net* no seu modelo, recuperar o [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-simple-windows-vm/azuredeploy.json#L201) para definir dinamicamente o *osDisk.URI* ponto final:

     "osDisk": {"name": "osdisk","vhd": {"uri":
     "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
      '/',variables('OSDiskName'),'.vhd')]"}}

## <a name="api-versioning"></a>Controlo de versões de API

Versões de serviço do Azure podem ser diferentes entre o Azure e o Azure Stack. Cada recurso requer o **apiVersion** atributo, que define os recursos oferecidos. Para garantir a compatibilidade de versões de API no Azure Stack, as seguintes versões de API são válidas para cada fornecedor de recursos:

| Fornecedor de Recursos | apiVersion |
| --- | --- |
| Computação |`'2015-06-15'` |
| Rede |`'2015-06-15'`, `'2015-05-01-preview'` |
| Armazenamento |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| Serviço de Aplicações |`'2015-08-01'` |

## <a name="template-functions"></a>Funções de modelos

O Azure Resource Manager [funções](../../azure-resource-manager/resource-group-template-functions.md) fornecem capacidades necessárias para criar modelos dinâmicos. Por exemplo, pode usar funções para tarefas como:

* CONCATENAR ou remoção de cadeias de caracteres.
* Referenciar valores a partir de outros recursos.
* Fazendo a iteração em recursos para implementar várias instâncias.

Estas funções não estão disponíveis no Azure Stack:

* Ignorar
* tirar

## <a name="resource-location"></a>Localização do recurso

Modelos Azure Resource Manager utilizam um atributo de localização para colocar recursos durante a implementação. No Azure, localizações referem-se para uma região como oeste dos E.U.A. ou América do Sul. No Azure Stack, localizações são diferentes, como o Azure Stack é no seu datacenter. Para garantir que os modelos são transferíveis entre o Azure e o Azure Stack, deve fazer referência a localização do grupo de recursos à medida que implementa recursos individuais. Pode fazer isso usando `[resourceGroup().Location]` para garantir que todos os recursos herdam a localização do grupo de recursos. O resumo a seguir é um exemplo de como utilizar esta função durante a implementação de uma conta de armazenamento:

    "resources": [
    {
      "name": "[variables('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "[variables('apiVersionStorage')]",
      "location": "[resourceGroup().location]",
      "comments": "This storage account is used to store the VM disks",
      "properties": {
      "accountType": "Standard_GRS"
      }
    }
    ]

## <a name="next-steps"></a>Passos Seguintes

* [Implementar modelos com o PowerShell](azure-stack-deploy-template-powershell.md)
* [Implementar modelos com a CLI do Azure](azure-stack-deploy-template-command-line.md)
* [Implementar modelos com o Visual Studio](azure-stack-deploy-template-visual-studio.md)
