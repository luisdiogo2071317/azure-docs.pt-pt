---
title: Erros de quota do Azure | Microsoft Docs
description: Descreve como resolver erros de qouta do recurso.
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
ms.openlocfilehash: 6d9048ae531abedb89b70989ce1c962357c514cd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34357049"
---
# <a name="resolve-errors-for-resource-quotas"></a>Resolva os erros para quotas de recursos

Este artigo descreve os erros de quota que poderá encontrar durante a implementação de recursos.

## <a name="symptom"></a>Sintoma

Se implementar um modelo que cria os recursos que excedem as quotas do Azure, receberá um erro de implementação que se pareça com:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Em alternativa, poderá ver:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Causa

As quotas são aplicadas por grupo de recursos, subscrições, contas e outros âmbitos. Por exemplo, a sua subscrição pode ser configurada para limitar o número de núcleos para uma região. Se tentar implementar uma máquina virtual com mais núcleos do que a quantidade permitido, receberá um erro a indicar que a quota foi excedida.
Para informações sobre a quota completa, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="azure-cli"></a>CLI do Azure

Para a CLI do Azure, utilize o `az vm list-usage` comando para encontrar as quotas de máquina virtual.

```azurecli
az vm list-usage --location "South Central US"
```

Que devolve:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="powershell"></a>PowerShell

Para o PowerShell, utilize o **Get-AzureRmVMUsage** comando para encontrar as quotas de máquina virtual.

```powershell
Get-AzureRmVMUsage -Location "South Central US"
```

Que devolve:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

## <a name="solution"></a>Solução

Para pedir um aumento de quota, aceda ao portal do e um problema de suporte de ficheiros. No problema suporte, pedir um aumento da sua quota para a região na qual pretende implementar.

> [!NOTE]
> Lembre-se de que para os grupos de recursos, a quota para cada região individuais, não para a subscrição completa. Se precisar de implementar 30 núcleos nos EUA oeste, terá de voltar a pedir 30 núcleos do Gestor de recursos em EUA oeste. Se precisar de implementar 30 núcleos em qualquer uma das regiões aos quais têm acesso, deverá solicitar ao para 30 núcleos do Gestor de recursos em todas as regiões.
>
>

1. Selecione **subscrições**.

   ![Subscrições](./media/resource-manager-quota-errors/subscriptions.png)

2. Selecione a subscrição que tem uma quota maior.

   ![Selecionar subscrição](./media/resource-manager-quota-errors/select-subscription.png)

3. Selecione **utilização + quotas**

   ![Selecione e quotas de utilização](./media/resource-manager-quota-errors/select-usage-quotas.png)

4. No canto superior direito, selecione **pedir aumento**.

   ![Pedir aumento](./media/resource-manager-quota-errors/request-increase.png)

5. Preencha os formulários para o tipo de quota que precisa de aumentar.

   ![Preencha o formulário](./media/resource-manager-quota-errors/forms.png)