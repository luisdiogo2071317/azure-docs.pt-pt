---
title: "Erros de modelo inválido do Azure | Microsoft Docs"
description: "Descreve como resolver erros de modelo inválido."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: 9626b3caaa7188a4e9a37f83d1fbf091951714f4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="resolve-errors-for-invalid-template"></a>Resolva os erros de modelo inválido

Este artigo descreve como resolver erros de modelo inválido.

## <a name="symptom"></a>Sintoma

Quando implementar um modelo, receberá um erro a indicar:

```
Code=InvalidTemplate
Message=<varies>
```

A mensagem de erro depende do tipo de erro.

## <a name="cause"></a>Causa

Este erro poderá resultar de vários tipos diferentes de erros. Estão normalmente relacionados um erro de sintaxe ou estrutural no modelo.

<a id="syntax-error" />

## <a name="solution-1---syntax-error"></a>Solução 1 - erro de sintaxe

Se receber uma mensagem de erro que indica a validação do modelo falha, pode ter um problema de sintaxe no seu modelo.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Este erro é fácil fazer como expressões de modelo podem ser intricate. Por exemplo, a atribuição de nome seguinte para uma conta de armazenamento tem um conjunto de parênteses Retos, três funções, três conjuntos de parênteses, um conjunto de plicas e uma propriedade:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Se não fornecer a sintaxe correspondente, o modelo produz um valor que é diferente da sua intenção.

Quando receber este tipo de erro, reveja cuidadosamente a sintaxe de expressão. Considere a utilização de um editor de JSON como [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) ou [Visual Studio Code](resource-manager-vs-code.md), que pode avisá-lo sobre os erros de sintaxe.

<a id="incorrect-segment-lengths" />

## <a name="solution-2---incorrect-segment-lengths"></a>Solução 2 - comprimentos de segmento incorreto

Outro modelo inválido erro ocorre quando o nome do recurso não está no formato correto.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Um recurso de nível de raiz tem de ter um menor segmento no nome do que o tipo de recurso. Cada segmento é diferenciado por uma barra. No exemplo seguinte, o tipo tem dois segmentos e o nome tem um segmento, pelo que tem um **nome válido**.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "myHostingPlanName",
  ...
}
```

Mas o exemplo seguinte é **não um nome válido** porque tem o mesmo número de segmentos que o tipo.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "appPlan/myHostingPlanName",
  ...
}
```

Para obter recursos subordinados, o tipo e o nome tem o mesmo número de segmentos. Este número de segmentos faz sentido porque o nome completo e o tipo para o elemento subordinado inclui o nome do principal e o tipo. Por conseguinte, o nome completo ainda tem um segmento menor que o tipo completo.

```json
"resources": [
    {
        "type": "Microsoft.KeyVault/vaults",
        "name": "contosokeyvault",
        ...
        "resources": [
            {
                "type": "secrets",
                "name": "appPassword",
                ...
            }
        ]
    }
]
```

Obter os segmentos direita pode ser tricky com tipos de Gestor de recursos que são aplicados através de fornecedores de recursos. Por exemplo, aplicar um bloqueio de recursos para um web site necessita de um tipo com quatro segmentos. Por conseguinte, o nome é três segmentos:

```json
{
    "type": "Microsoft.Web/sites/providers/locks",
    "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
    ...
}
```

<a id="parameter-not-valid" />

## <a name="solution-3---parameter-is-not-valid"></a>Solução 3 - parâmetro não é válido

Se fornecer um valor de parâmetro que não é um dos valores permitidos, receberá uma mensagem semelhante ao seguinte erro:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Duplo Verifique os valores permitidos no modelo e forneça um durante a implementação. Para obter mais informações sobre os valores de parâmetros permitidas, consulte [secção de parâmetros de modelos Azure Resource Manager](resource-manager-templates-parameters.md).

<a id="too-many-resource-groups" />

## <a name="solution-4---too-many-target-resource-groups"></a>Solução 4 - demasiados grupos de recursos de destino

Se especificar mais de cinco grupos de recursos de destino numa única implementação, receberá o erro. Considere consolidar o número de grupos de recursos na sua implementação, ou implementar alguns dos modelos como implementações separadas. Para obter mais informações, consulte [recursos do Azure de implementar a mais do que uma subscrição ou grupo de recursos](resource-manager-cross-resource-group-deployment.md).

<a id="circular-dependency" />

## <a name="solution-5---circular-dependency-detected"></a>Solução 5 - dependência circular detetada

Recebe este erro quando os recursos são dependentes entre si de uma forma que impede a implementação de iniciar. Uma combinação de interdependencies torna dois ou mais recursos de espera para outros recursos que também estão a aguardar. Por exemplo, resource1 depende resource3, resource2 depende resource1 e resource3 depende resource2. Normalmente, pode resolver este problema removendo dependências desnecessárias.

Para resolver uma dependência circular:

1. No seu modelo, localize o recurso identificado na dependência circular. 
2. Para esse recurso, examine o **dependsOn** propriedade e quaisquer utilizações do **referência** função para ver quais os recursos que depende. 
3. Examine esses recursos para ver quais os recursos que dependem. Siga as dependências até que tenha em atenção um recurso que depende o recurso original.
5. Para os recursos envolvidos na uma dependência circular, examine cuidadosamente todas as utilizações do **dependsOn** propriedade para identificar quaisquer dependências que não são necessários. Remova as dependências. Se não souber de que é necessária uma dependência, experimente removê-lo. 
6. Implementar novamente o modelo.

Remover os valores de **dependsOn** propriedade unidades pode provocar erros quando implementar o modelo. Se obtiver um erro, adicione a dependência no modelo. 

Se esse abordagem não resolver a dependência circular, considere mover a parte da sua lógica de implementação para recursos subordinados (por exemplo, as extensões ou definições de configuração). Configure os recursos subordinados implementar após os recursos envolvidos na uma dependência circular. Por exemplo, suponha que está a implementar duas máquinas virtuais, mas tem de definir propriedades em cada um que fazem referência a si. Pode implementá-las pela seguinte ordem:

1. vm1
2. vm2
3. Extensão vm1 depende vm1 e vm2. A extensão define os valores de vm1 que obtém a partir do vm2.
4. Extensão vm2 depende vm1 e vm2. A extensão define os valores de vm2 que obtém a partir do vm1.

A mesma abordagem funciona para aplicações do App Service. Considere mover valores de configuração para um recurso subordinado do recurso de aplicação. Pode implementar duas aplicações web pela seguinte ordem:

1. webapp1
2. webapp2
3. configuração para o webapp1 depende webapp1 e webapp2. Contém as definições de aplicação com valores de webapp2.
4. configuração para o webapp2 depende webapp1 e webapp2. Contém as definições de aplicação com valores de webapp1.
