---
title: SKU do Azure não está disponíveis erros | Microsoft Docs
description: Descreve como resolver o SKU não está disponível erro durante a implementação.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: 490c912a6abd6570c9bc74de8b86a516a8e6f807
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358766"
---
# <a name="resolve-errors-for-sku-not-available"></a>Resolver erros de SKU não está disponível

Este artigo descreve como resolver o **SkuNotAvailable** erro.

## <a name="symptom"></a>Sintoma

Quando implementar um recurso (normalmente, uma máquina virtual), receberá o seguinte código de erro e a mensagem de erro:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Causa

Recebe este erro quando o recurso SKU que selecionou (por exemplo, o tamanho da VM) não está disponível para a localização que selecionou.

## <a name="solution-1---powershell"></a>Solução 1 - PowerShell

Para determinar qual SKUs estão disponíveis numa região, utilize o [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) comando. Filtre os resultados por localização. Tem de ter a versão mais recente do PowerShell para este comando.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations -icontains "southcentralus"}
```

Os resultados incluem uma lista de SKUs para a localização e as restrições para esse SKU.

```powershell
ResourceType                Name      Locations Restriction                      Capability Value
------------                ----      --------- -----------                      ---------- -----
availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
virtualMachines      Standard_A0 southcentralus
virtualMachines      Standard_A1 southcentralus
virtualMachines      Standard_A2 southcentralus
```

## <a name="solution-2---azure-cli"></a>Solução 2 - CLI do Azure

Para determinar qual SKUs estão disponíveis numa região, utilize o `az vm list-skus` comando. Em seguida, pode utilizar `grep` ou um utilitário semelhante para filtrar os resultados.

```bash
$ az vm list-skus --output table
ResourceType      Locations           Name                    Capabilities                       Tier      Size           Restrictions
----------------  ------------------  ----------------------  ---------------------------------  --------  -------------  ---------------------------
availabilitySets  eastus              Classic                 MaximumPlatformFaultDomainCount=3
avilabilitySets   eastus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Aligned                 MaximumPlatformFaultDomainCount=3
```

## <a name="solution-3---azure-portal"></a>Solução 3 - portal do Azure

Para determinar qual SKUs estão disponíveis numa região, utilize o [portal](https://portal.azure.com). Inicie sessão no portal e adicione um recurso através da interface. Como definir os valores, consulte os SKUs disponíveis para esse recurso. Não é necessário concluir a implementação.

![SKUs disponíveis](./media/resource-manager-sku-not-available-errors/view-sku.png)

## <a name="solution-4---rest"></a>Solução 4 - REST

Para determinar qual SKUs estão disponíveis numa região, utilize a API REST para máquinas virtuais. Envie o pedido seguinte:

```HTTP 
GET
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
```

Devolve e regiões de SKUs disponíveis no seguinte formato:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

Se não conseguir encontrar um SKU adequado nessa região ou uma região alternativa que satisfaz a sua empresa, tem de submeter um [pedido SKU](https://aka.ms/skurestriction) para o suporte do Azure.
