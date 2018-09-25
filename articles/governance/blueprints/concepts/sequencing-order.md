---
title: Compreender a sequência de implementação no Azure esquemas
description: Saiba mais sobre o ciclo de vida que atravessa um plano gráfico e os detalhes sobre cada fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: c09fb26d8375e08281241aaed3f6f6e30acc755b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955457"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Compreender a sequência de implementação no Azure esquemas

O Azure utiliza esquemas uma **ordem de sequenciamento** para determinar a ordem de criação de recursos ao processar a atribuição de um plano gráfico. Este artigo explica a ordem de sequenciamento do padrão que é utilizada, como personalizar a ordem e como a ordem de personalizados é processada.

Existem variáveis nos exemplos JSON que é necessário substituir com seus próprios valores:

- `{YourMG}` -Substituir pelo nome do seu grupo de gestão

## <a name="default-sequencing-order"></a>Ordem de sequenciamento do padrão

Se o plano gráfico não contém nenhuma diretiva para a ordem implementar artefactos ou a diretiva é nula, em seguida, é utilizada pela seguinte ordem:

- Nível de assinatura **atribuição de função** ordenados pelo nome do artefacto de artefactos
- Nível de assinatura **atribuição de política** ordenados pelo nome do artefacto de artefactos
- Nível de assinatura **modelo Azure Resource Manager** ordenados pelo nome do artefacto de artefactos
- **Grupo de recursos** artefatos (incluindo os artefactos de subordinados), ordenados pelo nome do marcador de posição

Dentro de cada **grupo de recursos** artefato que é processado, a ordem de sequência seguinte é utilizado para artefactos a ser criada dentro do grupo de recursos:

- Subordinado do grupo de recursos **atribuição de função** ordenados pelo nome do artefacto de artefactos
- Subordinado do grupo de recursos **atribuição de política** ordenados pelo nome do artefacto de artefactos
- Subordinado do grupo de recursos **modelo Azure Resource Manager** ordenados pelo nome do artefacto de artefactos

## <a name="customizing-the-sequencing-order"></a>Personalizar a sequência de sequenciamento

Ao redigir planos gráficos grandes, poderá ser necessário para um recurso a ser criada por uma ordem específica em relação a outro recurso. O padrão de uso mais comum disso é quando um plano gráfico inclui vários modelos do Azure Resource Manager. Planos gráficos lida com isso, permitindo que a ordem de sequenciamento ser definido.

Isso é feito definindo um `dependsOn` propriedade no JSON. Apenas o esquema (para grupos de recursos) e os objetos de artefacto suportam esta propriedade. `dependsOn` é uma matriz de cadeia de caracteres de nomes de artefacto o artefacto específico tem de ser criada antes de ser criado.

### <a name="example---blueprint-with-ordered-resource-group"></a>Exemplo - esquema com o grupo de recursos ordenada

Este é uma esquema de exemplo com um grupo de recursos que definiu uma ordem de sequenciamento personalizado ao declarar um valor para `dependsOn`, juntamente com um grupo de recursos padrão. Neste caso, o nome do artefacto **assignPolicyTags** serão processados antes do **ordenadas-rg** grupo de recursos.
**Standard-rg** serão processados por ordem de sequenciamento padrão.

```json
{
    "properties": {
        "description": "Example blueprint with custom sequencing order",
        "resourceGroups": {
            "ordered-rg": {
                "dependsOn": [
                    "assignPolicyTags"
                ],
                "metadata": {
                    "description": "Resource Group that waits for 'assignPolicyTags' creation"
                }
            },
            "standard-rg": {
                "metadata": {
                    "description": "Resource Group that follows the standard sequence ordering"
                }
            }
        },
        "targetScope": "subscription"
    },
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint",
    "type": "Microsoft.Blueprint/blueprints",
    "name": "mySequencedBlueprint"
}
```

### <a name="example---artifact-with-custom-order"></a>Exemplo - artefacto com ordem personalizado

Este é um artefacto de política de exemplo que depende de um modelo Azure Resource Manager. Por predefinição ordenação, um artefacto de política seria possível criar antes do modelo Azure Resource Manager. Isso permite que o artefacto de política aguardar que o modelo Azure Resource Manager a ser criada.

```json
{
    "properties": {
        "displayName": "Assigns an identifying tag",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "resourceGroup": "standard-rg",
        "dependsOn": [
            "customTemplate"
        ]
    },
    "kind": "policyAssignment",
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint/artifacts/assignPolicyTags",
    "type": "Microsoft.Blueprint/artifacts",
    "name": "assignPolicyTags"
}
```

## <a name="processing-the-customized-sequence"></a>A sequência personalizada de processamento

Durante o processo de criação, um tipo topológica é utilizado para criar o gráfico de dependência do esquema e os artefactos. Isto garante que os vários níveis de dependência entre grupos de recursos e os artefactos podem ser suportados.

Se uma dependência for declarada no plano gráfico ou um artefato que não alterar a ordem predefinida, em seguida, nenhuma alteração é feita para a ordem de sequenciamento. Exemplos disso são um grupo de recursos que depende de uma atribuição de política de subordinado de nível política ou recurso grupo "standard-rg" de subscrição que depende de atribuição de função do grupo de recursos "standard-rg" filho. Em ambos os casos, o `dependsOn` não tiver alterado o padrão ordem de sequenciamento e sem alterações seriam feitas.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [ciclo de vida de esquema](lifecycle.md)
- Compreender como utilizar [parâmetros estáticos e dinâmicos](parameters.md)
- Descubra como tornar a utilização de [plano gráfico de bloqueio do recurso](resource-locking.md)
- Saiba como [atualizar atribuições existentes](../how-to/update-existing-assignments.md)
- Resolver problemas durante a atribuição de um plano gráfico com [resolução de problemas gerais](../troubleshoot/general.md)