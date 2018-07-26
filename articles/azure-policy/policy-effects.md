---
title: Noções básicas sobre o Azure Policy efeitos
description: Definição de política do Azure tem vários efeitos que determinam como conformidade é gerenciada e comunicada.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 17ad631e2441e4b8d6314557c17be143fd2f3de0
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248731"
---
# <a name="understanding-policy-effects"></a>Noções básicas sobre os efeitos de política

Cada definição de política na política do Azure tem um efeito único que determina o que acontece durante a verificação quando o **se** segmento da regra de política é avaliado de acordo com o recurso a ser analisado. Os efeitos podem também ter um comportamento diferente se forem para um novo recurso, um recurso atualizado ou um recurso existente.

Atualmente, existem cinco efeitos que são suportados numa definição de política:

- Acrescentar
- Auditoria
- AuditIfNotExists
- Negar
- DeployIfNotExists (disponível apenas para **incorporada** políticas)

## <a name="order-of-evaluation"></a>Ordem de avaliação

Quando é efetuado um pedido para criar ou atualizar um recurso através do Gestor de recursos do Azure, a política processada vários dos efeitos antes do pedido de processamento para o fornecedor de recursos apropriado.
Se o fizer, impede o processamento desnecessária por um fornecedor de recursos quando um recurso não cumpre os controlos de governação projetado de política. Política cria uma lista de todas as definições de política que atribuído, por uma política ou a atribuição de iniciativa, que se aplicam ao âmbito (menos exclusões) para o recurso e se prepara para avaliar o recurso em relação a cada definição.

- **Acrescentar** é avaliado primeiro. Acrescentar uma vez que pode alterar o pedido, uma alteração efetuada pelo acréscimo pode impedir que uma auditoria ou negar o efeito de acionar.
- **Negar** , em seguida, é avaliada. Avaliando negar antes de auditoria, registo duplo de um recurso indesejado é evitado.
- **Auditoria** , em seguida, é avaliado antes do pedido vai para o fornecedor de recursos.

Depois do pedido é fornecido para o fornecedor de recursos e o fornecedor de recursos devolve um código de estado de sucesso **AuditIfNotExists** e **DeployIfNotExists** são avaliadas para determinar se seguimento é necessária o registo de conformidade ou ação.

## <a name="append"></a>Acrescentar

Acrescentar é usado para adicionar campos adicionais para o recurso solicitado durante a criação ou atualização. Pode ser útil para adicionar etiquetas nos recursos, como costCenter ou permitido especificar IPs para um recurso de armazenamento.

### <a name="append-evaluation"></a>Acrescentar a avaliação

Conforme mencionado, acrescentar avalia antes do pedido obter processado por um fornecedor de recursos durante a criação ou atualização de um recurso. Acrescentar adiciona o campo (s) para o recurso quando o **se** é cumprida a condição de regra de política. Se o efeito de acréscimo poderia substituir um valor no pedido original com um valor diferente, em seguida, ele atua como um efeito de recusa e rejeitar o pedido.

Quando uma definição de política com o efeito de acréscimo é executada como parte de um ciclo de avaliação, não faz as alterações aos recursos já existentes. Em vez disso, ele marca a qualquer recurso que cumpra os **se** condição como não conforme.

### <a name="append-properties"></a>Propriedades de acréscimo

Um efeito de acréscimo apenas tem um **detalhes** matriz, o que é necessário. Como **detalhes** é uma matriz, pode demorar um único **campo/valor** par ou múltiplos. Consulte a [definição de política](policy-definition.md#fields) para obter a lista de campos aceitáveis.

### <a name="append-examples"></a>Exemplos de acréscimo

Exemplo 1: Único **campo/valor** par para acrescentar uma etiqueta.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "tags.myTag",
        "value": "myTagValue"
    }]
}
```

Exemplo 2: Múltiplos **campo/valor** pares para acrescentar um conjunto de marcas.

```json
"then": {
    "effect": "append",
    "details": [{
            "field": "tags.myTag",
            "value": "myTagValue"
        },
        {
            "field": "tags.myOtherTag",
            "value": "myOtherTagValue"
        }
    ]
}
```

Exemplo 3: Único **campo/valor** emparelhe com um [alias](policy-definition.md#aliases) com uma matriz **valor** para definir regras IP numa conta de armazenamento.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

## <a name="deny"></a>Negar

Negar serve para impedir que um pedido de recurso que não corresponde aos padrões desejados por meio de uma definição de política e o pedido de falha.

### <a name="deny-evaluation"></a>Avaliação de negação

Quando criar ou atualizar um recurso, negar impede que o pedido antes de a ser enviados para o fornecedor de recursos. O pedido é retornado como um erro 403 (proibido). No portal, o proibido pode ser visualizado como um Estado de implementação que foi impedido devido a atribuição de política.

Durante um ciclo de avaliação, definições de política com um efeito de recusa, que correspondem aos recursos são marcadas como não conforme, mas nenhuma ação é executada nesse recurso.

### <a name="deny-properties"></a>Propriedades de negação

O efeito de recusa não tem as propriedades adicionais para utilização no **, em seguida,** condição para a definição de política.

### <a name="deny-example"></a>Exemplo de negação

Exemplo: A utilizar o efeito de recusa.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Auditoria

O efeito de auditoria é utilizado para criar um evento de aviso no registo de atividades quando um recurso de conformidade for avaliado, mas não impede que o pedido.

### <a name="audit-evaluation"></a>Avaliação de auditoria

O efeito de auditoria é o último para serem executadas durante a criação ou atualização de um recurso antes do recurso é enviada para o fornecedor de recursos. Auditoria funciona da mesma para um pedido de recurso e um ciclo de avaliação e executa uma `Microsoft.Authorization/policies/audit/action` operação ao registo de atividades. Em ambos os casos, o recurso está marcado como não conforme.

### <a name="audit-properties"></a>Propriedades de auditoria

O efeito de auditoria não tem as propriedades adicionais para utilização no **, em seguida,** condição para a definição de política.

### <a name="audit-example"></a>Exemplo de auditoria

Exemplo: A utilizar o efeito de auditoria.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists permite a auditoria num recurso que corresponda a **se** condição, mas não tem os componentes especificados no **detalhes** do **, em seguida,** condição.

### <a name="auditifnotexists-evaluation"></a>Avaliação de AuditIfNotExists

AuditIfNotExists é executado depois de um fornecedor de recursos tratou de um pedido de criar ou atualizar para um recurso e devolveu um código de estado de êxito. O efeito é acionado se existirem não existem recursos relacionados ou se os recursos definidos pelo **ExistenceCondition** não avaliadas como verdadeiras. Quando o efeito é acionado, um `Microsoft.Authorization/policies/audit/action` operação para o registo de atividade é executada da mesma forma como o efeito de auditoria. Quando acionado, o recurso que satisfeitos os **se** condição é o recurso que está marcado como não conforme.

### <a name="auditifnotexists-properties"></a>Propriedades de AuditIfNotExists

O **detalhes** propriedade dos efeitos AuditIfNotExists tem todos os subproperties que definem os recursos relacionados para corresponder.

- **Tipo de** [necessário]
  - Especifica o tipo de recurso relacionado para corresponder.
  - Começa ao tentar obter um recurso por baixo da **se** recursos de condição, em seguida, consultas dentro do mesmo grupo de recursos como o **se** recursos de condição.
- **Nome** (opcional)
  - Especifica o nome exato do recurso para corresponder e faz com que a política para obter um recurso específico em vez de todos os recursos do tipo especificado.
- **ResourceGroupName** (opcional)
  - Permite a correspondência do recurso relacionado provenientes de um grupo de recursos diferente.
  - Não se aplica **tipo** é um recurso que seria por baixo da **se** recursos de condição.
  - A predefinição é o **se** grupo de recursos do recurso de condição.
- **ExistenceScope** (opcional)
  - Valores permitidos são _subscrição_ e _ResourceGroup_.
  - Define o âmbito de como obter o recurso relacionado para corresponder a partir de.
  - Não se aplica **tipo** é um recurso que seria por baixo da **se** recursos de condição.
  - Para _ResourceGroup_, seria o limite para o **se** grupo de recursos do recurso de condição ou o grupo de recursos especificado na **ResourceGroupName**.
  - Para _subscrição_, consulta da subscrição completa para os recursos relacionados.
  - A predefinição é _ResourceGroup_.
- **ExistenceCondition** (opcional)
  - Se não for especificado, quaisquer relacionadas com recursos de **tipo** satisfaça o efeito e não aciona a auditoria.
  - Utiliza o mesmo idioma como a regra de política para o **se** de condição, mas é avaliado relativamente a cada recurso relacionado individualmente.
  - Se todos os recursos relacionados correspondente for avaliada como true, o efeito é satisfeito e não aciona a auditoria.
  - Pode utilizar [field()] para verificar a equivalência com valores no **se** condição.
  - Por exemplo, poderia ser usado para validar que o recurso principal (no **se** condição) está na mesma localização de recursos que o recurso relacionado correspondente.

### <a name="auditifnotexists-example"></a>Exemplo de AuditIfNotExists

Exemplo: Avalia as máquinas virtuais para determinar se a extensão de Antimalware existe, em seguida, as auditorias quando em falta.

```json
{
    "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
    },
    "then": {
        "effect": "auditIfNotExists",
        "details": {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "existenceCondition": {
                "allOf": [{
                        "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                        "equals": "Microsoft.Azure.Security"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/extensions/type",
                        "equals": "IaaSAntimalware"
                    }
                ]
            }
        }
    }
}
```

## <a name="deployifnotexists"></a>DeployIfNotExists

Assim como AuditIfNotExists, DeployIfNotExists executa uma implementação de modelo quando a condição é cumprida.

> [!WARNING]
> Só está disponível para DeployIfNotExists **incorporada** políticas.

### <a name="deployifnotexists-evaluation"></a>Avaliação de DeployIfNotExists

DeployIfNotExists também é executado depois de um fornecedor de recursos manipulou uma criação ou atualização do pedido a um recurso e devolveu um código de estado de êxito. O efeito é acionado se existirem não existem recursos relacionados ou se os recursos definidos pelo **ExistenceCondition** não avaliadas como verdadeiras. Quando for acionado o efeito, é executada uma implementação do modelo.

Durante um ciclo de avaliação, definições de política com um efeito de DeployIfNotExists que correspondem aos recursos são marcadas como não conforme, mas nenhuma ação é executada nesse recurso.

### <a name="deployifnotexists-properties"></a>Propriedades de DeployIfNotExists

O **detalhes** propriedade dos efeitos DeployIfNotExists tem todos os subproperties que definem os recursos relacionados para correspondência e a implementação do modelo para executar.

- **Tipo de** [necessário]
  - Especifica o tipo de recurso relacionado para corresponder.
  - Começa ao tentar obter um recurso por baixo da **se** recursos de condição, em seguida, consultas dentro do mesmo grupo de recursos como o **se** recursos de condição.
- **Nome** (opcional)
  - Especifica o nome exato do recurso para corresponder e faz com que a política para obter um recurso específico em vez de todos os recursos do tipo especificado.
- **ResourceGroupName** (opcional)
  - Permite a correspondência do recurso relacionado provenientes de um grupo de recursos diferente.
  - Não se aplica **tipo** é um recurso que seria por baixo da **se** recursos de condição.
  - A predefinição é o **se** grupo de recursos do recurso de condição.
  - Se uma implementação do modelo for executada, é implementada no grupo de recursos deste valor.
- **ExistenceScope** (opcional)
  - Valores permitidos são _subscrição_ e _ResourceGroup_.
  - Define o âmbito de como obter o recurso relacionado para corresponder a partir de.
  - Não se aplica **tipo** é um recurso que seria por baixo da **se** recursos de condição.
  - Para _ResourceGroup_, seria o limite para o **se** grupo de recursos do recurso de condição ou o grupo de recursos especificado na **ResourceGroupName**.
  - Para _subscrição_, consulta da subscrição completa para os recursos relacionados.
  - A predefinição é _ResourceGroup_.
- **ExistenceCondition** (opcional)
  - Se não for especificado, quaisquer relacionadas com recursos de **tipo** satisfaça o efeito e não aciona a implementação.
  - Utiliza o mesmo idioma como a regra de política para o **se** de condição, mas é avaliado relativamente a cada recurso relacionado individualmente.
  - Se todos os recursos relacionados correspondente for avaliada como true, o efeito é satisfeito e não aciona a implementação.
  - Pode utilizar [field()] para verificar a equivalência com valores no **se** condição.
  - Por exemplo, poderia ser usado para validar que o recurso principal (no **se** condição) está na mesma localização de recursos que o recurso relacionado correspondente.
- **Implementação** [necessário]
  - Esta propriedade deve conter a implementação do modelo completo, como ele foi transmitido para o `Microsoft.Resources/deployments` colocar API. Para obter mais informações, consulte a [API do REST de implementações](/rest/api/resources/deployments).

  > [!NOTE]
  > Todas as funções dentro de **implementação** propriedade são avaliadas como componentes do modelo, não a política. A exceção é o **parâmetros** propriedade que passa os valores da política para o modelo. O **valor** nesta secção num modelo de nome do parâmetro é utilizado para efetuar este valor passando (consulte _fullDbName_ no exemplo DeployIfNotExists).

### <a name="deployifnotexists-example"></a>Exemplo de DeployIfNotExists

Exemplo: Avalia as bases de dados do SQL Server para determinar se transparentDataEncryption está ativado. Caso contrário, em seguida, é executada uma implementação para ativá-la.

```json
"if": {
    "field": "type",
    "equals": "Microsoft.Sql/servers/databases"
},
"then": {
    "effect": "DeployIfNotExists",
    "details": {
        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
        "name": "current",
        "existenceCondition": {
            "field": "Microsoft.Sql/transparentDataEncryption.status",
            "equals": "Enabled"
        },
        "deployment": {
            "properties": {
                "mode": "incremental",
                "template": {
                    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "fullDbName": {
                            "type": "string"
                        }
                    },
                    "resources": [{
                        "name": "[concat(parameters('fullDbName'), '/current')]",
                        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
                        "apiVersion": "2014-04-01",
                        "properties": {
                            "status": "Enabled"
                        }
                    }]
                },
                "parameters": {
                    "fullDbName": {
                        "value": "[field('fullName')]"
                    }
                }
            }
        }
    }
}
```

## <a name="layering-policies"></a>Políticas de disposição em camadas

Um recurso pode ser afetado por várias atribuições. Estas atribuições podem ser no mesmo âmbito (recurso específico, grupo de recursos, subscrição ou grupo de gestão) ou em âmbitos diferentes. Cada um destas atribuições também é provável que ter um efeito de diferente definido. Seja como for, a condição e o efeito de cada política (atribuído diretamente ou como parte de uma iniciativa) independente é avaliada. Por exemplo, se a diretiva 1 tem uma condição que restringe a localização do recurso para a subscrição A ser criado apenas no 'westus' com o efeito de recusa e política 2 tem uma condição que restringe apenas a localização do recurso para o grupo de recursos B (o que está na subscrição A) para ser criado no "eastus" com o efeito de auditoria são ambos atribuído, o resultado resultante seria::

- Qualquer recurso já no grupo de recursos B no "eastus" está em conformidade com a política de 2, mas marcado como não conforme a política de 1.
- Qualquer recurso já no grupo de recursos B não está em "eastus" será marcado como não conforme a política de 2 e também pode ser marcado não em conformidade com a diretiva 1 se não for 'westus'.
- De qualquer novo recurso na subscrição A não em 'westus' seria negado pela política de 1.
- De qualquer novo recurso na subscrição A / grupo de recursos B no 'westus' pode ser marcado como em não conformidade na política de 2, mas seria possível criar (em conformidade com a política de 1 e 2 de política é de auditoria e não negar).

Se a diretiva 1 e 2 de política que tinha efeitos de negações, situação mudará para:

- Qualquer recurso já no grupo de recursos B não está em "eastus" será marcado como não conforme a política de 2.
- Qualquer recurso já no grupo de recursos B não está no 'westus' será marcado como não conforme a política de 1.
- De qualquer novo recurso na subscrição A não em 'westus' seria negado pela política de 1.
- De qualquer novo recurso na subscrição A / B do grupo de recursos deve ser negado (uma vez que a localização do mesmo nunca foi possível satisfazer política 1 e a política de 2).

Como cada atribuição individualmente é avaliada, aqui não é uma oportunidade para um recurso de ortografia passar através de uma lacuna devido às diferenças no âmbito. Por conseguinte, o resultado líquido de políticas de disposição em camadas ou sobreposição de diretivas é considerado como estando **cumulativa mais restritivo**. Em outras palavras, um recurso que pretende que criou podia ser bloqueado devido a políticas conflitantes e sobrepostas, como o exemplo acima, se a política de 1 e 2 de política tiveram um efeito de recusa. Se ainda precisar do recurso a ser criadas no âmbito de destino, reveja as exclusões em cada atribuição para garantir que as políticas corretas estão a afetar os âmbitos certos.

## <a name="next-steps"></a>Passos Seguintes

Agora que tem uma compreensão mais aprofundada dos efeitos de definição de política, reveja os exemplos de política:

- Veja mais exemplos nas [amostras do Azure Policy](json-samples.md).
