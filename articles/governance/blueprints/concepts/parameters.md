---
title: Criação de esquemas dinâmicas através de parâmetros em esquemas do Azure
description: Saiba mais sobre os parâmetros de estáticos e dinâmicos e como usá-los cria planos gráficos dinâmicos.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e1a1d736eb9b22cd444a75b94d112abfe3fbe5af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993583"
---
# <a name="creating-dynamic-blueprints-through-parameters"></a>Criação de esquemas dinâmicas através de parâmetros

Um plano gráfico totalmente definido com vários artefactos (por exemplo, grupos de recursos, modelos de Gestor de recursos, as políticas ou atribuições de funções) oferece a criação rápida e consistente de aprovisionamento de objetos no Azure. Para ativar a utilização flexível desses padrões de design reutilizáveis e contentores, os esquemas do Azure suporta parâmetros. O parâmetro cria flexibilidade, tanto durante a definição e a atribuição, para alterar as propriedades os artefactos implementadas pelo plano gráfico.

Um exemplo simples é o artefacto de grupo de recursos. Quando é criado um grupo de recursos, ele tem dois valores necessários que tem de ser fornecidos: nome e localização. Ao adicionar um grupo de recursos ao seu blueprint, se não existirem parâmetros, definiria esse nome e localização para cada utilização do plano gráfico. Isso faria com que cada utilização de esquema para criar artefactos no mesmo grupo de recursos. Embora não um problema com o grupo de recursos, recursos dentro do grupo de recursos poderia se tornar duplicado e causa um conflito.

> [!NOTE]
> Não é um problema para dois esquemas diferentes incluir um grupo de recursos com o mesmo nome.
> Se um grupo de recursos incluído num plano gráfico já existir, o plano gráfico continua criar os artefactos relacionados nesse grupo de recursos. Isso poderia causar um conflito como dois recursos com o mesmo nome e tipo de recurso não pode existir dentro de uma subscrição.

Isso é onde parâmetros se adaptam. O valor para essas propriedades, no caso do grupo de recursos o nome e a propriedade de localização, esquemas, não defini-los durante a definição do esquema, mas em vez disso, definir seus valores durante a atribuição para uma subscrição. Isto torna possível reutilizar um plano gráfico que cria um grupo de recursos e outros recursos dentro de uma única subscrição, sem ter de conflito.

## <a name="blueprint-parameters"></a>Parâmetros de esquema

Através da API REST, os parâmetros podem ser criados no esquema, além de cada um dos artefatos suportados. Quando um parâmetro é criado sobre o plano gráfico, pode ser utilizado pelos artefactos nesse esquema. Por exemplo, poderá o prefixo para atribuição de nomes do grupo de recursos. O artefacto, em seguida, pode utilizar o parâmetro de esquema para criar um parâmetro de "principalmente dinâmico", como o parâmetro ainda poderia ser definido durante a atribuição, mas terá uma consistência que pode seguir as regras de nomenclatura da organização. Para obter os passos, consulte [parâmetros estáticos de definição - parâmetro de nível de esquema](#blueprint-level-parameter).

### <a name="using-securestring-and-secureobject-parameters"></a>Usando parâmetros secureString e secureObject

Enquanto um modelo do Resource Manager _artefacto_ suporta parâmetros do **secureString** e **secureObject** tipos, esquemas do Azure requer que cada um ligado com um cofre de chaves do Azure.
Isso impede que a prática de não segura de armazenar segredos, juntamente com o esquema e incentiva o emprego de padrões de seguros. Planos gráficos do Azure facilita isto através da deteção a inclusão de qualquer parâmetro seguro num modelo do Resource Manager _artefacto_ e pedir durante a atribuição do esquema para o Cofre de chaves seguintes propriedades por detetado seguro parâmetro:

- ID de recurso do Key Vault
- Nome secreto do Key Vault
- Versão de segredo do Key Vault

O Key Vault referenciado tem de existir na mesma subscrição como o esquema está a ser atribuído a e também tem de ter **ativar o acesso ao Azure Resource Manager para a implementação de modelo** configurado do Key Vault **acesso políticas** página. Para instruções sobre como ativar esta funcionalidade, consulte [Key Vault - ativar a implementação do modelo](../../../managed-applications/key-vault-access.md#enable-template-deployment).
Para obter mais informações sobre o Azure Key Vault, consulte [descrição geral do Cofre de chave](../../../key-vault/key-vault-overview.md).

## <a name="parameter-types"></a>Tipos de parâmetro

### <a name="static-parameters"></a>Parâmetros estáticos

Um valor de parâmetro definido na definição de um plano gráfico é chamado um **parâmetro estático**. Isto acontece porque cada utilização do plano gráfico irá implementar o artefacto usando esse valor estático. O exemplo de grupo de recursos, embora isso não faria sentido para o nome do grupo de recursos, poderá fazer sentido para a localização. Em seguida, cada atribuição do plano gráfico cria o grupo de recursos, tudo o que é chamado durante a atribuição, na mesma localização. Isto permite-lhe ser seletivo no que definir como necessários vs o que pode ser alterado durante a atribuição.

#### <a name="setting-static-parameters-in-the-portal"></a>Definir parâmetros estáticos no portal

1. Inicie o serviço de esquemas do Azure no portal do Azure ao clicar no **todos os serviços** e a procurar e selecionando **política** no painel esquerdo. Sobre o **política** página, clique em **esquemas**.

1. Selecione **definição de Blueprint** partir da página à esquerda.

1. Clique numa esquema existente e, em seguida, clique em **Editar esquema** ou clique em **+ criar esquema** e preencha as informações sobre o **Noções básicas** separador.

1. Clique em **seguinte: artefactos** ou clique nas **artefactos** separador.

1. Artefatos adicionados ao esquema, que tem opções de parâmetro exibir **parâmetros de X de Y preenchidos** no **parâmetros** coluna. Clique na linha de artefacto para editar os parâmetros de artefacto.

   ![Parâmetros de esquema](../media/parameters/parameter-column.png)

1. O **artefacto editar** página apresenta as opções de valor apropriadas para o artefacto clicado. Cada parâmetro no artefacto tem um título, uma caixa de valor e uma caixa de verificação. Definir a caixa desmarcada para torná-lo um **parâmetro estático**. No exemplo abaixo, apenas _localização_ é um **parâmetro estático** porque está desmarcada e _nome do grupo de recursos_ está marcada.

   ![Parâmetros de esquema estáticos](../media/parameters/static-parameter.png)

#### <a name="setting-static-parameters-from-rest-api"></a>Definir parâmetros estáticos a partir da API REST

Em cada URI da API REST, existem variáveis que são utilizadas que precisa substituir por seus próprios valores:

- `{YourMG}` -Substituir pelo nome do seu grupo de gestão
- `{subscriptionId}` -Substituir pelo seu ID de subscrição

##### <a name="blueprint-level-parameter"></a>Parâmetro de nível de esquema

Ao criar um plano gráfico através da REST API, é possível criar [esquema parâmetros](#blueprint-parameters). Para tal, utilize o seguinte formato de URI da API REST e o corpo:

- URI DA API REST

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint?api-version=2017-11-11-preview
  ```

- Corpo do Pedido

  ```json
  {
      "properties": {
          "description": "This blueprint has blueprint level parameters.",
          "targetScope": "subscription",
          "parameters": {
              "owners": {
                  "type": "array",
                  "metadata": {
                      "description": "List of AAD object IDs that is assigned Owner role at the resource group"
                  }
              }
          },
          "resourceGroups": {
              "storageRG": {
                  "description": "Contains the resource template deployment and a role assignment."
              }
          }
      }
  }
  ```

Depois de criar um parâmetro de nível de esquema, pode ser utilizado em artefatos adicionados a esse esquema.
O exemplo de REST API seguinte cria um artefacto de atribuição de função no plano gráfico e utiliza o parâmetro de nível de esquema.

- URI DA API REST

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/roleOwner?api-version=2017-11-11-preview
  ```

- Corpo do Pedido

  ```json
  {
      "kind": "roleAssignment",
      "properties": {
          "resourceGroup": "storageRG",
          "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
          "principalIds": "[parameters('owners')]"
      }
  }
  ```

Neste exemplo, o **principalIds** propriedade feita utilizar do **proprietários** esquema de parâmetro de nível ao fornecer um valor de `[parameters('owners')]`. Definir um parâmetro num artefacto com um parâmetro de nível de esquema ainda é um exemplo de um **parâmetro estático** que ele não pode ser definido durante a atribuição do esquema e serão esse valor em cada atribuição.

##### <a name="artifact-level-parameter"></a>Parâmetro de nível de artefacto

Criando **parâmetros estáticos** num artefato é semelhante, mas que assume um valor diretamente em vez de usar o `parameters()` função. O exemplo seguinte cria dois parâmetros estáticos, **tagName** e **tagValue**. O valor em cada um é fornecido diretamente e não utiliza uma chamada de função.

- URI DA API REST

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/policyStorageTags?api-version=2017-11-11-preview
  ```

- Corpo do Pedido

  ```json
  {
      "kind": "policyAssignment",
      "properties": {
          "description": "Apply storage tag and the parameter also used by the template to resource groups",
          "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
          "parameters": {
              "tagName": {
                  "value": "StorageType"
              },
              "tagValue": {
                  "value": "Premium_LRS"
              }
          }
      }
  }
  ```

### <a name="dynamic-parameters"></a>Parâmetros dinâmicos

O oposto de um parâmetro estático é um **parâmetro dinâmico**. Este é um parâmetro que não está definido no esquema, mas em vez disso, é definido durante a cada atribuição do esquema. O exemplo de grupo de recursos, isso faz sentido para o nome do grupo de recursos, fornecendo um nome diferente para cada atribuição do esquema.

#### <a name="setting-dynamic-parameters-in-the-portal"></a>Definir parâmetros dinâmicos no portal

1. Inicie o serviço de esquemas do Azure no portal do Azure ao clicar no **todos os serviços** e a procurar e selecionando **política** no painel esquerdo. Sobre o **política** página, clique em **esquemas**.

1. Selecione **definição de Blueprint** partir da página à esquerda.

1. Com o botão direito no plano de gráfico que pretende atribuir e selecione **atribuir esquema** ou clique no esquema que pretende atribuir, em seguida, clique nas **atribuir esquema** botão.

1. Sobre o **atribuir esquema** página, encontre o **parâmetros de artefacto** secção. Cada artefato com, pelo menos, um **parâmetro dinâmico** apresenta o artefacto e as opções de configuração. Forneça os valores necessários para os parâmetros antes de atribuir o plano gráfico. No exemplo abaixo, _Name_ é um **parâmetro dinâmico** que tem de ser definido para concluir a atribuição do esquema.

   ![Parâmetro de esquema dinâmico](../media/parameters/dynamic-parameter.png)

#### <a name="setting-dynamic-parameters-from-rest-api"></a>Definir parâmetros dinâmicos a partir da API REST

A definição **parâmetros dinâmicos** durante a atribuição é feito ao indicar o valor desejado diretamente. Em vez de usar uma função, como `parameters()`, o valor fornecido é uma cadeia de caracteres apropriada. Artefactos para um grupo de recursos são definidos com um "nome de modelo," e **name** e **localização** propriedades. Todos os outros parâmetros para cada artefato incluído é definida em **parâmetros** com um **\<nome\>** e **valor** par de chaves. Se o esquema estiver configurado para um parâmetro dinâmico, que não é fornecido durante a atribuição, a atribuição irá falhar.

- URI DA API REST

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Blueprint/blueprintAssignments/assignMyBlueprint?api-version=2017-11-11-preview
  ```

- Corpo do Pedido

  ```json
  {
      "properties": {
          "blueprintId": "/providers/Microsoft.Management/managementGroups/{YourMG}  /providers/Microsoft.Blueprint/blueprints/MyBlueprint",
          "resourceGroups": {
              "storageRG": {
                  "name": "StorageAccount",
                  "location": "eastus2"
              }
          },
          "parameters": {
              "storageAccountType": {
                  "value": "Standard_GRS"
              },
              "tagName": {
                  "value": "CostCenter"
              },
              "tagValue": {
                  "value": "ContosoIT"
              },
              "contributors": {
                  "value": [
                      "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
                      "38833b56-194d-420b-90ce-cff578296714"
                  ]
                },
              "owners": {
                  "value": [
                      "44254d2b-a0c7-405f-959c-f829ee31c2e7",
                      "316deb5f-7187-4512-9dd4-21e7798b0ef9"
                  ]
              }
          }
      },
      "identity": {
          "type": "systemAssigned"
      },
      "location": "westus"
  }
  ```

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [ciclo de vida de esquema](lifecycle.md)
- Aprenda a personalizar o [plano gráfico de ordem de sequenciamento](sequencing-order.md)
- Descubra como tornar a utilização de [plano gráfico de bloqueio do recurso](resource-locking.md)
- Saiba como [atualizar atribuições existentes](../how-to/update-existing-assignments.md)
- Resolver problemas durante a atribuição de um plano gráfico com [resolução de problemas gerais](../troubleshoot/general.md)