---
title: Elemento de IU SizeSelector do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Compute.SizeSelector para o portal do Azure.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 9009d29e281ace179ad1dd2021c7cf35e3dc611a
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084812"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Elemento de IU Microsoft.Compute.SizeSelector
Um controlo para selecionar um tamanho de uma ou mais instâncias de máquina virtual.

## <a name="ui-sample"></a>Exemplo de IU

O utilizador verá um Seletor com valores predefinidos da definição de elemento.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

Depois de selecionar o controlo, o utilizador verá uma vista expandida das tamanhos disponíveis.

![Microsoft.Compute.SizeSelector expandido](./media/managed-application-elements/microsoft.compute.sizeselector-expanded.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": [],
    "numAvailabilityZonesRequired": 3,
    "zone": "3"
  },
  "options": {
    "hideDiskTypeFilter": false
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Observações
- `recommendedSizes` deve ter, pelo menos, um tamanho. O primeiro tamanho recomendado é utilizado como predefinição. A lista de tamanhos disponíveis não está ordenada pelo Estado recomendado. O utilizador pode selecionar essa coluna para ordenar pelo Estado recomendado.
- Se um tamanho recomendado não está disponível na localização selecionada, o tamanho automaticamente é ignorado. Em vez disso, o próximo tamanho recomendado é utilizado.
- `constraints.allowedSizes` e `constraints.excludedSizes` são opcionais, mas não podem ser utilizados em simultâneo. É possível determinar a lista de tamanhos disponíveis ao chamar [listar tamanhos de máquina virtual disponíveis para uma subscrição](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region). Qualquer dimensão não especificado no `constraints.allowedSizes` está oculto e qualquer dimensão não especificado na `constraints.excludedSizes` é apresentado.
- `osPlatform` tem de ser especificado e pode ser **Windows** ou **Linux**. É utilizado para determinar os custos de hardware das máquinas virtuais.
- `imageReference` for omitido para imagens originais, mas fornecido para imagens de terceiros. É utilizado para determinar os custos de software das máquinas virtuais.
- `count` é utilizado para definir o multiplicador adequado para o elemento. Suporta um valor estático, tal como **2**, ou como um valor dinâmico de outro elemento, `[steps('step1').vmCount]`. O valor predefinido é **1**.
- O `numAvailabilityZonesRequired` pode ser 1, 2 ou 3.
- Por predefinição, `hideDiskTypeFilter` é **falso**. O filtro do tipo de disco permite ao utilizador ver todos os tipos de disco ou apenas SSD.

## <a name="sample-output"></a>Resultado da amostra
```json
"Standard_D1"
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
