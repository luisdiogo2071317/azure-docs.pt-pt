---
title: "Utilizar o Azure Policy para criar e gerir políticas para impor a conformidade organizacional | Microsoft Docs"
description: "Utilize o Azure Policy para impor normas, corresponder a requisitos de conformidade regulamentar e de auditoria, controlar custos, manter a segurança e consistência de desempenha, bem como impor princípios de design a nível empresarial."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/18/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: a3d47abcbf41133b9bc7194fd97f9b66a70003ff
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/03/2018
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Criar e gerir políticas para impor a conformidade

Compreender como criar e gerir políticas no Azure é importante para manter a conformidade com os seus padrões empresariais e contratos de nível de serviço. Neste tutorial, vai aprender a utilizar o Azure Policy para fazer algumas das tarefas mais comuns relacionadas com a criação, atribuição e gestão de políticas na sua organização, tais como:

> [!div class="checklist"]
> * Atribuir uma política para impor uma condição para os recursos que criar no futuro
> * Criar e atribuir uma definição de iniciativa para controlar a conformidade de vários recursos
> * Resolver um recurso negado ou em não conformidade
> * Implementar uma nova política numa organização

Se gostaria de atribuir uma política para identificar o estado atual de conformidade dos seus recursos existentes, os artigos de início rápido mostram como fazê-lo. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="assign-a-policy"></a>Atribuir uma política

O primeiro passo para impor a conformidade com o Azure Policy consiste em atribuir uma definição de política. Uma definição de política define sob que condição é imposta uma política e qual a ação a tomar. Neste exemplo, atribua uma definição de política incorporada chamada *Requer a Versão do SQL Server 12.0* para garantir a condição em que todas as bases de dados SQL Server sejam v12.0 para que estejam em conformidade.

1. Inicie o serviço Azure Policy no portal do Azure ao pesquisar e selecionar **Policy** no painel esquerdo.

   ![Pesquisar política](media/assign-policy-definition/search-policy.png)

2. Selecione **Atribuições** no painel esquerdo da página de Política do Azure. Uma atribuição é uma política que foi atribuída para ter lugar num âmbito específico.
3. Selecione **Atribuir Política** a partir da parte superior do painel **Atribuições**.

   ![Atribuir uma definição de política](media/create-manage-policy/select-assign-policy.png)

4. Na página **Atribuir Política**, clique em ![botão de Definição de política](media/assign-policy-definition/definitions-button.png), junto ao campo **Política**, para abrir a lista de definições disponíveis. Pode filtrar a definição de política **Tipo** para *BuiltIn* para ver todas e ler as suas descrições.

   ![Abrir definições de política disponíveis](media/create-manage-policy/open-policy-definitions.png)

5. Selecione **Requer a Versão do SQL Server 12.0**. Se não conseguir encontrar logo, escreva **Requer a Versão do SQL Server 12.0** na caixa de pesquisa e prima ENTER.

   ![Localizar uma política](media/create-manage-policy/select-available-definition.png)

6. O **Nome** apresentado é preenchido automaticamente, mas pode alterá-lo. Para este exemplo, utilize *Requer a Versão do SQL Server 12.0*. Também pode adicionar uma **Descrição** opcional. A descrição fornece detalhes sobre como esta atribuição de política certifica-se de que todos os servidores SQL criados neste ambiente estão na versão 12.0.

7. Altere o escalão de preço para **Standard** para se certificar de que a política é aplicada aos recursos existentes.

   Existem dois escalões de preços no Azure Policy – *Gratuito* e *Standard*. No Escalão gratuito, apenas pode impor políticas em recursos futuros, enquanto no Standard, também pode impô-los em recursos existentes para compreender melhor o seu estado de compatibilidade. Por o Azure Policy estar numa versão de Pré-visualização, ainda não foi lançado um modelo de preços, pelo que não vai receber uma fatura ao selecionar *Standard*. Para obter mais informações sobre preços, veja: [Azure Policy pricing (Preços do Azure Policy)](https://azure.microsoft.com/pricing/details/azure-policy).

8. Selecione o **Âmbito** - a subscrição (ou grupo de recursos) que registou anteriormente. Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos.

   Este exemplo utiliza a subscrição **Azure Analytics Capacity Dev**. A sua subscrição vai ser diferente.

10. Selecione **Atribuir**.

## <a name="implement-a-new-custom-policy"></a>Implementar uma nova política personalizada

Agora que atribuiu uma definição de política incorporada, pode fazer mais com o Azure Policy. Em seguida, crie uma nova política personalizada para poupar ao garantir que as VMs criadas no seu ambiente não possam estar na série G. Desta forma, sempre que um utilizador na sua organização tenta criar uma VM na série G, o pedido é negado.

1. Selecione **Definição** em **Criação** no painel esquerdo.

   ![Definição em criação](media/create-manage-policy/definition-under-authoring.png)

2. Selecione **+ Definição de Política**.
3. Escreva o seguinte:

   - O nome da definição de política - *Necessitar de SKUs de VM mais pequenas que a série G*
   - A descrição do que a definição e política é suposto fazer: esta definição de política impõe que todas as VMs criadas neste âmbito tenham SKUs mais pequenas que a série G para reduzir os custos.
   - A subscrição em que a definição de política reside. Neste caso, a definição de política reside em **Advisor Analytics Capacity Dev**. A sua lista de subscrições vai ser diferente.
   - Escolha das opções existentes ou crie uma nova categoria para esta definição de política.
   - Copie o seguinte código json e então atualize-o de acordo com as suas necessidades com:
      - Os parâmetros de política.
      - As regras/condições de política, neste caso: o tamanho da SKU de VM igual à série G
      - O efeito de política, neste caso: **Negar**.

    Aqui está com o que o seu json se deve parecer. Cole o seu código revisto no portal do Azure.

    ```json
{
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/sku.name",
            "like": "Standard_G*"
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
}
    ```

    O valor da *propriedade field* na regra de política deve ser um dos seguintes: Nome, Tipo, Localização, Marcas ou um alias. Por exemplo, `"Microsoft.Compute/VirtualMachines/Size"`.

    Para ver mais exemplos de código json, leia o artigo [Modelos para o Azure Policy](json-samples.md).

4. Selecione **Guardar**.

## <a name="create-a-policy-definition-with-rest-api"></a>Criar uma definição de política com a API REST

Pode criar uma política com a API REST para Definições de Política. A API REST permite-lhe criar e eliminar definições de política, bem como obter informações sobre as definições existentes.
Para criar uma definição de política, utilize o seguinte exemplo:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

```
Incluir um corpo de pedido semelhante ao exemplo seguinte:

```
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>Criar uma definição de política com o PowerShell

Antes de continuar com o exemplo do PowerShell, certifique-se de que tem instalada a versão mais recente do Azure PowerShell. Os parâmetros de política foram adicionados na versão 3.6.0. Se tiver uma versão anterior, os exemplos podem devolver um erro a indicar que o parâmetro não pode ser encontrado.

Pode criar uma definição de política ao utilizar o cmdlet `New-AzureRmPolicyDefinition`.

Para criar uma definição de política a partir de um ficheiro, passe o caminho para o ficheiro. Para um ficheiro externo, utilize o seguinte exemplo:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Para um ficheiro local, utilize o seguinte exemplo:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Para criar uma definição de política com uma regra na mesma linha, utilize o seguinte exemplo:

```
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

O resultado é armazenado num objeto `$definition`, que é utilizado durante a atribuição de política.
O exemplo seguinte cria uma definição de política que inclui os parâmetros:

```
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters
```

## <a name="view-policy-definitions"></a>Ver definições de política

Para ver todas as definições de política na sua subscrição, utilize o seguinte comando:

```
Get-AzureRmPolicyDefinition
```

Devolve todas as definições de política disponíveis, incluindo políticas incorporadas. Cada política é devolvida no seguinte formato:

```
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Criar uma definição de política com a CLI do Azure

Pode criar uma definição de política ao utilizar a CLI do Azure com o comando de definição de política.
Para criar uma definição de política com uma regra na mesma linha, utilize o seguinte exemplo:

```
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

## <a name="view-policy-definitions"></a>Ver definições de política

Para ver todas as definições de política na sua subscrição, utilize o seguinte comando:

```
az policy definition list
```

Devolve todas as definições de política disponíveis, incluindo políticas incorporadas. Cada política é devolvida no seguinte formato:

```
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Criar e atribuir uma definição de iniciativa

Com uma definição de iniciativa, pode agrupar várias definições de política para alcançar um objetivo mais abrangente. Crie uma definição de iniciativa para garantir que os recursos dentro do âmbito da definição mantém-se em conformidade com as definições de política que constituem a definição de iniciativa.  Consulte a [descrição geral do Azure Policy](./azure-policy-introduction.md) para mais informações sobre as definições de iniciativa.

### <a name="create-an-initiative-definition"></a>Criar uma definição de iniciativa

1. Selecione **Definições** em **Criação** no painel esquerdo.

   ![Selecione as definições](media/create-manage-policy/select-definitions.png)

2. Selecione **Definição de Iniciativas** no topo da página, esta seleção leva-o para o formulário de **Definição de Iniciativas**.
3. Introduza o nome e descrição da iniciativa.

   Neste exemplo, certifique-se de que os recursos estão em conformidade com as definições de política sobre a segurança. Portanto, o nome da iniciativa seria **Proteger-se** e a descrição seria: **Esta iniciativa foi criada para processar todas as definições de política associadas à proteção de recursos**.

   ![Definição de iniciativa](media/create-manage-policy/initiative-definition.png)

4. Procure na lista de **Definições Disponíveis** e selecione as definições de política que gostaria de adicionar a essa iniciativa. Para a nossa iniciativa de **Proteger-se**, **Adicionar** as seguintes definições de política integradas:
   - Requer a Versão do SQL Server 12.0
   - Monitorizar as aplicações web desprotegidas no Centro de Segurança.
   - Monitorizar a rede permissiva no Centro de Segurança.
   - Monitorizar possível colocação de aplicações em lista de permissões no Centro de Segurança.
   - Monitorizar Discos de VM sem encriptação no Centro de Segurança.

   ![Definições de iniciativa](media/create-manage-policy/initiative-definition-2.png)

   Depois de selecionar as definições de política da lista que vê em **Políticas e parâmetros**, conforme mostrado na imagem acima.

5. Utilize **Localização da definição** para selecionar uma subscrição na qual armazenar a definição. Selecione **Guardar**.

### <a name="assign-an-initiative-definition"></a>Atribuir uma definição de iniciativa

1. Vá à aba **Definições** em **Criação**.
2. Procure pela definição de iniciativa **Proteger-se** que criou.
3. Selecione a definição de iniciativa e selecione **Atribuir**.

   ![Atribuir uma definição](media/create-manage-policy/assign-definition.png)

4. Preencha o formulário **Atribuição** ao introduzir as seguintes informações de exemplo. Pode utilizar as suas próprias informações.
   - Nome: atribuição de proteção
   - Descrição: esta atribuição de iniciativa está adaptada para impor a este grupo as definições de política da subscrição **Azure Advisor Capacity Dev**.
   - Escalão de Preço: Standard
   - Âmbito onde pretende que esta atribuição aplicada: **Azure Advisor capacidade Dev**. Pode escolher a sua própria subscrição e grupo de recursos.

5. Selecione **Atribuir**.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Excluir um recurso negado ou em não conformidade ao utilizar a Exclusão

Ao seguir o exemplo acima, depois de atribuir a definição de política para requerer a versão 12.0 do SQL Server, um SQL Server criado com uma versão diferente seria negado. Nesta secção, tem um guia passo-a-passo para resolver uma tentativa negada de criar um servidor SQL de uma versão diferente ao pedir uma exclusão. A exclusão basicamente previne a imposição da política. Uma exclusão pode aplicar-se a um grupo de recursos, ou pode restringir a exclusão a recursos individuais.

1. Selecione **Atribuições** no painel esquerdo.
2. Procure por todas as atribuições de política e abra a atribuição *Requer a Versão do SQL Server 12.0*.
3. **Selecione** uma exclusão para recursos nos grupos de recursos em que está a tentar criar o SQL Server. Neste exemplo, exclua Microsoft.Sql/servers/databases: *azuremetrictest/testdb* e *azuremetrictest/testdb2*.

   ![Pedir exclusão](media/create-manage-policy/request-exclusion.png)

   Outras formas com que pode resolver um recurso negado incluem: entrar em contacto com quem estiver associado à política se tiver uma forte justificação para precisar do servidor SQL criado e editar diretamente a política se tiver acesso a esta.

4. Clique em **Atribuir**.

Nesta secção, resolveu a negação da sua tentativa de criar um SQL Server com a versão 12.0 ao pedir uma exclusão dos recursos.

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser continuar a trabalhar com os tutoriais subsequentes, não limpe os recursos criados neste guia. Se não planear continuar, utilize os passos seguintes para eliminar quaisquer atribuições ou definições criadas acima:

1. Selecione **Definições** (ou **Atribuições** se estiver a tentar eliminar uma atribuição) no painel esquerdo.
2. Procure pela nova iniciativa ou definição de política (ou atribuição) que acabou de criar.
3. Selecione as reticências no fim da definição ou atribuição, selecione **Eliminar Definição** (ou **Eliminar Atribuição**).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial conseguiu realizar com êxito o seguinte:

> [!div class="checklist"]
> * Atribuir uma política para impor uma condição para os recursos que criar no futuro
> * Criar e atribuir uma definição de iniciativa para controlar a conformidade de vários recursos
> * Resolver um recurso negado ou em não conformidade
> * Implementar uma nova política numa organização

Para saber mais sobre as estruturas de definições de política, veja este artigo:

> [!div class="nextstepaction"]
> [Estrutura de definição do Azure Policy](policy-definition.md)
