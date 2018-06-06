---
title: Compreender os efeitos de política do Azure
description: Definição de política do Azure tem vários efeitos que determinam como compatibilidade é gerida e reportada.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 23bbbe9cf86268f93ae1f8fcec9303efa8a673de
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796721"
---
# <a name="understanding-policy-effects"></a>Compreender os efeitos da política

Cada definição de política na política do Azure tem um efeito único que determina o que acontece durante a análise quando o **se** segmento a regra de política é avaliado como corresponde ao recurso que está a ser analisado. Os efeitos podem também se comportam de forma diferente se forem para um novo recurso, um recurso atualizado ou um recurso existente.

Existem cinco efeitos que são suportados numa definição de política:

- Acrescentar
- Auditoria
- AuditIfNotExists
- Negar
- DeployIfNotExists

## <a name="order-of-evaluation"></a>Ordem de avaliação

Quando é efetuado um pedido para criar ou atualizar um recurso através do Azure Resource Manager, a política processada várias os efeitos antes de manipulação de pedidos para o fornecedor de recursos adequados.
Se o fizer, impede o processamento desnecessária por um fornecedor de recursos quando um recurso não cumpre os controlos de concebida governação da política. Política cria uma lista de todas as definições de política que atribuído por uma política ou a atribuição de iniciativa, que se aplicam ao âmbito (menos exclusões) para o recurso e prepara a avaliar o recurso em relação a cada definição.

- **Acrescentar** é avaliado pela primeira vez. Acrescentar uma vez que foi possível alterar o pedido, uma alteração efetuada pelo acrescentar pode impedir uma auditoria ou negar o efeito de acionamento.
- **Negar** , em seguida, é avaliada. Através da avaliação de negação antes de auditoria, é evitado duplo o registo de um recurso indesejado.
- **Auditoria** , em seguida, é avaliado antes do pedido de ir para o fornecedor de recursos.

Depois do pedido é fornecido para o fornecedor de recursos e o fornecedor de recursos devolve um código de estado de êxito, **AuditIfNotExists** e **DeployIfNotExists** são avaliadas para determinar se seguimento é necessária o registo de compatibilidade ou ação.

## <a name="append"></a>Acrescentar

Acrescentar é utilizada para adicionar campos adicionais para o recurso pedido durante a criação ou atualização. Pode ser útil para adicionar etiquetas de recursos, tais como costCenter ou permitido especificar IPs para um recurso de armazenamento.

### <a name="append-evaluation"></a>Acrescentar avaliação

Conforme mencionado, acrescentar avalia antes do pedido durante a criação ou atualização de um recurso a ser processado por um fornecedor de recursos. Acrescentar adiciona campos para o recurso quando o **se** condição da regra de política. Se o efeito de acréscimo seria substituir um valor no pedido original com um valor diferente, em seguida, este age como um efeito de negar e rejeitar o pedido.

Quando uma definição de política utilizando o efeito de acréscimo é executada como parte de um ciclo de avaliação, efetue alterações aos recursos já existem. Em vez disso, marca qualquer recurso que cumpra o **se** condição como não conforme.

### <a name="append-properties"></a>Acrescentar propriedades

Só tem um efeito de acrescentar um **detalhes** matriz, o que é necessária. Como **detalhes** é uma matriz, pode levar a uma única **campo/valor** par ou múltiplos. Consulte [definição de política](policy-definition.md#fields) para obter a lista de campos aceitáveis.

### <a name="append-examples"></a>Acrescentar exemplos

Exemplo 1: Único **campo/valor** par acrescentar uma etiqueta.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "tags.myTag",
        "value": "myTagValue"
    }]
}
```

Exemplo 2: Vários **campo/valor** pares de acrescentar um conjunto de etiquetas.

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

Exemplo 3: Único **campo/valor** emparelhe utilizando um [alias](policy-definition.md#aliases) com uma matriz **valor** para definir regras IP numa conta de armazenamento.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

## <a name="deny"></a>Negar

Negar é utilizada para impedir que um pedido de recurso que não corresponde a padrões pretendidos através de uma definição de política e falha o pedido.

### <a name="deny-evaluation"></a>Avaliação de negação

Quando criar ou atualizar um recurso, negar impede que o pedido antes de a ser enviados para o fornecedor de recursos. O pedido é devolvido como 403 (proibido). No portal, o proibido pode ser visualizado como um Estado de implementação que foi impedido devido a atribuição de política.

Durante um ciclo de avaliação, definições de política com um efeito de negar que correspondem aos recursos estão marcadas como não conformes, mas é executada qualquer ação sobre esse recurso.

### <a name="deny-properties"></a>Propriedades de negação

O efeito de negação não tem propriedades adicionais para utilização no **, em seguida,** condição da definição de política.

### <a name="deny-example"></a>Exemplo de negação

Exemplo: Utilizar o efeito de negação.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Auditoria

O efeito de auditoria é utilizado para criar um evento de aviso no registo de auditoria quando um recurso de não conformidade será avaliado, mas não a interromperá se o pedido.

### <a name="audit-evaluation"></a>Avaliação de auditoria

O efeito de auditoria é o último a ser executado durante a criação ou atualização de um recurso antes do recurso é enviada para o fornecedor de recursos. Auditoria funciona com o mesmo para um pedido de recurso e um ciclo de avaliação e executa uma `Microsoft.Authorization/policies/audit/action` operação para o registo de atividade. Em ambos os casos, o recurso está marcado como não conformes.

### <a name="audit-properties"></a>Propriedades de auditoria

O efeito de auditoria não tem propriedades adicionais para utilização no **, em seguida,** condição da definição de política.

### <a name="audit-example"></a>Exemplo de auditoria

Exemplo: Utilizar o efeito de auditoria.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists permite auditoria num recurso que corresponda a **se** condição, mas não tem os componentes especificados no **detalhes** do **, em seguida,** condição.

### <a name="auditifnotexists-evaluation"></a>Avaliação AuditIfNotExists

AuditIfNotExists é executado depois de um fornecedor de recursos processou um pedido de criação ou atualização para um recurso e devolveu um código de estado de êxito. O efeito é acionado se existem não existem recursos relacionados ou se os recursos definidos pelo **ExistenceCondition** não avaliadas como verdadeiras. Quando é acionado o efeito, um `Microsoft.Authorization/policies/audit/action` operação para o registo de atividade é executada da mesma forma como o efeito de auditoria. Quando acionado, o recurso que satisfeitos a **se** condição é o recurso que está marcado como não conformes.

### <a name="auditifnotexists-properties"></a>Propriedades de AuditIfNotExists

O **detalhes** propriedade dos efeitos AuditIfNotExists tem todos os subproperties que definem os recursos relacionados para corresponder.

- **Tipo** [necessário]
  - Especifica o tipo de recurso relacionado para corresponder.
  - Começa por tentar obter um recurso, por baixo do **se** recursos de condição, em seguida, as consultas existentes no mesmo grupo de recursos como o **se** recursos de condição.
- **Nome** (opcional)
  - Especifica o nome exato do recurso para corresponder e faz com que a política para obter um recurso específico, em vez de todos os recursos do tipo especificado.
- **ResourceGroupName** (opcional)
  - Permite a correspondência do recurso relacionado para ser proveniente de um grupo de recursos diferentes.
  - Não é aplicável se **tipo** é um recurso que seria por baixo do **se** recursos de condição.
  - Predefinição é o **se** condição do grupo de recursos do recurso.
- **ExistenceScope** (opcional)
  - Valores permitidos são _subscrição_ e _ResourceGroup_.
  - Define o âmbito de onde obter o recurso relacionado para corresponder a partir de.
  - Não é aplicável se **tipo** é um recurso que seria por baixo do **se** recursos de condição.
  - Para _ResourceGroup_, limita-se para o **se** grupo de recursos do recurso de condição ou o grupo de recursos especificado na **ResourceGroupName**.
  - Para _subscrição_, consulta a subscrição completa para o recurso relacionado.
  - Predefinição é _ResourceGroup_.
- **ExistenceCondition** (opcional)
  - Se não for especificado, quaisquer relacionadas com recurso dos **tipo** satisfaça o efeito e não aciona a auditoria.
  - Utilize a mesma linguagem que a regra de política para o **se** condição, mas é avaliada em comparação com cada recurso relacionado individualmente.
  - Se quaisquer recursos relacionados correspondente avalia como VERDADEIRO, o efeito é satisfeito e não aciona a auditoria.
  - Pode utilizar [field()] para verificar equivalência com valores de **se** condição.
  - Por exemplo, poderia ser utilizado para validar que o recurso principal (no **se** condição) está a ser a mesma localização do recurso que o recurso relacionado correspondente.

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

Semelhante ao AuditIfNotExists, DeployIfNotExists executa uma implementação de modelo quando a condição for satisfeita.

### <a name="deployifnotexists-evaluation"></a>Avaliação DeployIfNotExists

DeployIfNotExists também é executada após um fornecedor de recursos processou um criar ou atualizar o pedido para um recurso e devolveu um código de estado de êxito. O efeito é acionado se existem não existem recursos relacionados ou se os recursos definidos pelo **ExistenceCondition** não avaliadas como verdadeiras. Quando é acionado o efeito, é executada uma implementação de modelo.

Durante um ciclo de avaliação, definições de política com um efeito de DeployIfNotExists que correspondem aos recursos estão marcadas como não conformes, mas é executada qualquer ação sobre esse recurso.

### <a name="deployifnotexists-properties"></a>Propriedades de DeployIfNotExists

O **detalhes** propriedade dos efeitos DeployIfNotExists tem todos os subproperties que definem os recursos relacionados para correspondência e a implementação do modelo para executar.

- **Tipo** [necessário]
  - Especifica o tipo de recurso relacionado para corresponder.
  - Começa por tentar obter um recurso, por baixo do **se** recursos de condição, em seguida, as consultas existentes no mesmo grupo de recursos como o **se** recursos de condição.
- **Nome** (opcional)
  - Especifica o nome exato do recurso para corresponder e faz com que a política para obter um recurso específico, em vez de todos os recursos do tipo especificado.
- **ResourceGroupName** (opcional)
  - Permite a correspondência do recurso relacionado para ser proveniente de um grupo de recursos diferentes.
  - Não é aplicável se **tipo** é um recurso que seria por baixo do **se** recursos de condição.
  - Predefinição é o **se** condição do grupo de recursos do recurso.
  - Se uma implementação de modelo é executada, é implementada no grupo de recursos deste valor.
- **ExistenceScope** (opcional)
  - Valores permitidos são _subscrição_ e _ResourceGroup_.
  - Define o âmbito de onde obter o recurso relacionado para corresponder a partir de.
  - Não é aplicável se **tipo** é um recurso que seria por baixo do **se** recursos de condição.
  - Para _ResourceGroup_, limita-se para o **se** grupo de recursos do recurso de condição ou o grupo de recursos especificado na **ResourceGroupName**.
  - Para _subscrição_, consulta a subscrição completa para o recurso relacionado.
  - Predefinição é _ResourceGroup_.
- **ExistenceCondition** (opcional)
  - Se não for especificado, quaisquer relacionadas com recurso dos **tipo** satisfaça o efeito e não aciona a implementação.
  - Utilize a mesma linguagem que a regra de política para o **se** condição, mas é avaliada em comparação com cada recurso relacionado individualmente.
  - Se quaisquer recursos relacionados correspondente avalia como VERDADEIRO, o efeito é satisfeito e não aciona a implementação.
  - Pode utilizar [field()] para verificar equivalência com valores de **se** condição.
  - Por exemplo, poderia ser utilizado para validar que o recurso principal (no **se** condição) está a ser a mesma localização do recurso que o recurso relacionado correspondente.
- **Implementação** [necessário]
  - Esta propriedade deve conter a implementação do modelo completo, teria de ser transmitido para o `Microsoft.Resources/deployments` colocar API. Para obter mais informações, consulte o [API de REST de implementações](/rest/api/resources/deployments).

  > [!NOTE]
  > Todas as funções dentro de **implementação** propriedade são avaliadas como componentes do modelo, não a política. A exceção é o **parâmetros** propriedade transmite os valores da política no modelo. O **valor** nesta secção sob um modelo de nome do parâmetro é utilizado para efetuar este valor transmitir (consulte _fullDbName_ no exemplo DeployIfNotExists).

### <a name="deployifnotexists-example"></a>Exemplo de DeployIfNotExists

Exemplo: Avalia bases de dados do SQL Server para determinar se transparentDataEncryption está ativado. Caso contrário, em seguida, ativá-la a uma implementação é executada.

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

## <a name="layering-policies"></a>Dispor políticas

Um recurso pode ser afetado por vários atribuições. Estes atribuições podem estar no mesmo âmbito (recurso específico, grupo de recursos, subscrição ou grupo de gestão) ou em âmbitos diferentes. Cada um destes atribuições também é provável que tenha um efeito diferente definido. Independentemente disso, a condição e o efeito de cada política (atribuído diretamente ou como parte de uma iniciativa) independentemente é avaliada. Por exemplo, se a política 1 tem uma condição que restringe a localização para a subscrição A partir a ser criada no 'westus' com o efeito de negar e a política 2 e que restringe os recursos no recurso grupo B (que está na subscrição A) de que está a ser criada no 'eastus' com a auditoria efeito são ambos atribuídos, o resultado resultante seria:

- Qualquer recurso já se encontra num grupo de recursos B 'eastus' é compatível com a política de 2, mas marcada como não conforme a política de 1.
- Qualquer recurso já se encontra num grupo de recursos B não 'eastus' será marcado como não conforme a política de 2 e iria também ser marcado como não compatível com a política 1 se não for 'westus'.
- Qualquer recurso novo na subscrição A não no 'westus' seria rejeitado pela política de 1.
- Qualquer recurso novo na subscrição A / grupo de recursos B 'westus' deverá ser marcado como não conformidade na política 2, mas seria possível criar (em conformidade com a política de 1 e 2 de política é de auditoria e não negar).

Se a política de 1 e a política de 2 tinha que tenha efeito de negar, a situação é alterado para:

- Qualquer recurso já se encontra num grupo de recursos B não 'eastus' será marcado como não conforme a política de 2.
- Qualquer recurso já se encontra num grupo de recursos B não 'westus' será marcado como não conforme a política de 1.
- Qualquer recurso novo na subscrição A não no 'westus' seria rejeitado pela política de 1.
- Qualquer recurso novo na subscrição A / B do grupo de recursos deverá ser negado (uma vez que a localização nunca foi satisfazer política 1 e a política de 2).

Como a cada atribuição é avaliada individualmente, não é uma oportunidade para um recurso para slip através de um intervalo devido a diferenças no âmbito. Por conseguinte, o resultado net de políticas de dispor ou sobreposição de política é considerado **cumulativa mais restritivas**. Por outras palavras, um recurso que pretende criar podia ser bloqueado devido a políticas sobrepostas e em conflito, tais como o exemplo acima, se a política de 1 e a política de 2 tinham um efeito de negação. Se ainda precisar do recurso a ser criados no âmbito de destino, reveja as exclusões em cada atribuição para garantir que as políticas corretas são afetar os âmbitos à direita.

## <a name="next-steps"></a>Passos Seguintes

Agora que tem uma compreensão mais aprofundada efeitos de definição de política, reveja os exemplos de política:

- Reveja mais exemplos em [amostras de política do Azure](json-samples.md).
