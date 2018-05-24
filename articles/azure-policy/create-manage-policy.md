---
title: Utilizar o Azure Policy para criar e gerir políticas para impor a conformidade organizacional
description: Utilize o Azure Policy para impor normas, corresponder a requisitos de conformidade regulamentar e de auditoria, controlar custos, manter a segurança e consistência de desempenho, bem como impor princípios de design a nível empresarial.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 2e04e08d22890246e2b68a55d79e82864201ef9d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Criar e gerir políticas para impor a conformidade

Compreender como criar e gerir políticas no Azure é importante para manter a conformidade com os seus padrões empresariais e contratos de nível de serviço. Neste tutorial, vai aprender a utilizar o Azure Policy para realizar algumas das tarefas mais comuns relacionadas com a criação, atribuição e gestão de políticas na sua organização, tais como:

> [!div class="checklist"]
> - Atribuir uma política para impor uma condição para os recursos que criar no futuro
> - Criar e atribuir uma definição de iniciativa para controlar a conformidade de vários recursos
> - Resolver um recurso negado ou em não conformidade
> - Implementar uma nova política numa organização

Se gostaria de atribuir uma política para identificar o estado atual de conformidade dos seus recursos existentes, os artigos de início rápido mostram como fazê-lo. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="assign-a-policy"></a>Atribuir uma política

O primeiro passo para impor a conformidade com o Azure Policy consiste em atribuir uma definição de política. Uma definição de política define sob que condição é imposta uma política e qual o efeito a ter. Neste exemplo, atribua uma definição de política incorporada chamada *Requer a versão do SQL Server 12.0*, para impor a condição de que todas as bases de dados SQL Server têm de ser v12.0 para estarem em conformidade.

1. Inicie o serviço Azure Policy no portal do Azure ao clicar em **Todos os serviços** e, em seguida, ao pesquisar e selecionar **Policy**.

   ![Pesquisar política](media/create-manage-policy/search-policy.png)

2. Selecione **Atribuições** no lado esquerdo da página Azure Policy. Uma atribuição é uma política que foi atribuída para ter lugar num âmbito específico.
3. Selecione **Atribuir Política** na parte superior da página **Política - Atribuições**.

   ![Atribuir uma definição de política](media/create-manage-policy/select-assign-policy.png)

4. Na página **Atribuir Política**, selecione o **Âmbito** ao clicar nas reticências e selecionando uma subscrição (obrigatório) e um grupo de recursos (opcional). Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta.  Em seguida, clique em **Selecionar** na parte inferior da página **Âmbito**.

   Este exemplo utiliza a **Subscrição Contoso**. A sua subscrição vai ser diferente.

5. Se quisesse excluir um ou mais grupos de recursos (se definisse apenas o âmbito como uma subscrição) ou recursos específicos dentro de um grupo de recursos (ambos os casos de definição de âmbito), podia configurar **Exclusões** a partir da atribuição de política. Por agora, deixe em branco.

6. Selecione as reticências de **Definição de política** para abrir a lista de definições disponíveis. Pode filtrar o **Tipo** de definição de política para *Incorporado* para ver todas e ler as respetivas descrições.

7. Selecione **Requer a versão do SQL Server 12.0**. Se não conseguir encontrar logo, escreva **require sql server** na caixa de pesquisa e prima ENTER ou clique fora da caixa de pesquisa. Clique em **Selecionar** na parte inferior da página **Definições Disponíveis** depois de ter encontrado e selecionado a definição de política.

   ![Localizar uma política](media/create-manage-policy/select-available-definition.png)

8. O **Nome da atribuição** é automaticamente preenchido com o nome da política que selecionou, mas pode alterá-lo. Para este exemplo, deixe *Requer a versão do SQL Server 12.0*. Também pode adicionar uma **Descrição** opcional. A descrição fornece detalhes sobre esta atribuição de política.

9. Clique em **Atribuir**.

## <a name="implement-a-new-custom-policy"></a>Implementar uma nova política personalizada

Agora que atribuiu uma definição de política incorporada, pode fazer mais com o Azure Policy. Em seguida, crie uma nova política personalizada para poupar ao garantir que as VMs criadas no seu ambiente não possam estar na série G. Desta forma, sempre que um utilizador na sua organização tenta criar uma VM na série G, o pedido é negado.

1. Selecione **Definições** em **CRIAÇÃO** no lado esquerdo da página Azure Policy.

   ![Definição em criação](media/create-manage-policy/definition-under-authoring.png)

2. Selecione **+ Definição de política** na parte superior da página. Esta ação abre a página **Definição de política**.
3. Escreva o seguinte:

   - O grupo de gestão ou a subscrição em que a definição de política é guardada. Selecione, utilizando o botão de reticências em **Localização da definição**.

     > [!NOTE]
     > Se pretende aplicar esta definição de política a várias subscrições, a localização tem de ser um grupo de gestão que contenha as subscrições às quais irá atribuir a política. O mesmo se aplica a uma definição de iniciativa.

   - O nome da definição de política - *Necessitar de SKUs de VM mais pequenas que a série G*
   - A descrição do que a definição de política é suposto fazer: *Esta definição de política impõe que todas as VMs criadas neste âmbito tenham SKUs mais pequenas que a série G para reduzir os custos.*
   - Escolha de entre as opções existentes ou crie uma nova categoria para esta definição de política.
   - Copie o seguinte código json e atualize-o de acordo com as suas necessidades com:
      - Os parâmetros de política.
      - As regras/condições de política, neste caso: o tamanho da SKU de VM igual à série G
      - O efeito de política, neste caso: **Negar**.

    Eis o aspeto que o seu json deverá ter. Cole o seu código revisto no portal do Azure.

    ```json
    {
        "policyRule": {
            "if": {
                "allOf": [{
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

    O valor da propriedade *field* na regra de política tem ser um dos seguintes: Nome, Tipo, Localização, Etiquetas ou um alias. Um exemplo de um alias poderá ser `"Microsoft.Compute/VirtualMachines/Size"`.

    Para ver mais exemplos do Azure Policy, veja [Modelos do Azure Policy](json-samples.md).

4. Selecione **Guardar**.

## <a name="create-a-policy-definition-with-rest-api"></a>Criar uma definição de política com a API REST

Pode criar uma política com a API REST para Definições de Política. A API REST permite-lhe criar e eliminar definições de política, bem como obter informações sobre as definições existentes.
Para criar uma definição de política, utilize o seguinte exemplo:

```http-interactive
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Incluir um corpo de pedido semelhante ao exemplo seguinte:

```json
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

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Para um ficheiro local, utilize o seguinte exemplo:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

Para criar uma definição de política com uma regra na mesma linha, utilize o seguinte exemplo:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
    "if": {
        "allOf": [{
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

```azurepowershell-interactive
$policy = '{
    "if": {
        "allOf": [{
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

$definition = New-AzureRmPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>Ver definições de política com o PowerShell

Para ver todas as definições de política na sua subscrição, utilize o seguinte comando:

```azurepowershell-interactive
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

```azurecli-interactive
az policy definition create --name 'denyCoolTiering' --description 'Deny cool access tiering for storage' --rules '{
    "if": {
        "allOf": [{
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

### <a name="view-policy-definitions-with-azure-cli"></a>Ver definições de política com a CLI do Azure

Para ver todas as definições de política na sua subscrição, utilize o seguinte comando:

```azurecli-interactive
az policy definition list
```

Devolve todas as definições de política disponíveis, incluindo políticas incorporadas. Cada política é devolvida no seguinte formato:

```json
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

Com uma definição de iniciativa, pode agrupar várias definições de política para alcançar um objetivo mais abrangente. Crie uma definição de iniciativa para garantir que os recursos dentro do âmbito da definição mantém-se em conformidade com as definições de política que constituem a definição de iniciativa.  Para obter mais informações sobre as definições de iniciativa, veja [Descrição geral do Azure Policy](azure-policy-introduction.md).

### <a name="create-an-initiative-definition"></a>Criar uma definição de iniciativa

1. Selecione **Definições** em **CRIAÇÃO** no lado esquerdo da página Azure Policy.

   ![Selecione as definições](media/create-manage-policy/select-definitions.png)

2. Selecione **+ Definição de Iniciativa** na parte superior da página para abrir a página **Definição de iniciativa**.

   ![Definição de iniciativa](media/create-manage-policy/initiative-definition.png)

3. Utilize as reticências de **Localização da definição** para selecionar uma subscrição na qual armazenar a definição.

4. Introduza o **Nome** e a **Descrição** da iniciativa.

   Este exemplo vai garantir que os recursos estão em conformidade com as definições de política sobre segurança. Portanto, o nome da iniciativa seria **Proteger-se** e a descrição seria: **Esta iniciativa foi criada para processar todas as definições de política associadas à proteção de recursos**.

5. Em **Categoria**, escolha de entre as opções existentes ou crie uma nova categoria.

6. Procure na lista de **Definições Disponíveis** (lado direito da página **Definição de iniciativa**) e selecione as definições de política que gostaria de adicionar a esta iniciativa. Para a iniciativa **Proteger-se**, adicione as seguintes definições de política incorporadas, clicando em **+** junto às informações de definição de política ou clicando numa linha de definição de política e, em seguida, na opção **+ Adicionar** na página de detalhes:
   - Requer a Versão do SQL Server 12.0
   - [Preview]: Monitor unprotected web applications in Security Center.
   - [Preview]: Monitor permissive network across in Security Center.
   - [Preview]: Monitor possible app Whitelisting in Security Center.
   - [Preview]: Monitor unencrypted VM Disks in Security Center.

   Depois de selecionar a definição de política na lista, esta é adicionada em **POLÍTICAS E PARÂMETROS**.

   ![Definições de iniciativa](media/create-manage-policy/initiative-definition-2.png)

7. Clique em **Guardar**.

### <a name="assign-an-initiative-definition"></a>Atribuir uma definição de iniciativa

1. Selecione **Definições** em **CRIAÇÃO** no lado esquerdo da página Azure Policy.
2. Localize a definição de iniciativa **Proteger-se** que criou anteriormente e selecione-a.
3. Selecione **Atribuir** na parte superior da página para abrir a página **Proteger-se: Atribuir Iniciativa**.

   ![Atribuir uma definição](media/create-manage-policy/assign-definition.png)

   Em alternativa, pode clicar com o botão direito do rato na linha selecionada ou clicar com o botão esquerdo do rato nas reticências no final da linha para abrir um menu de contexto.  Em seguida, selecione **Atribuir**.

   ![Fazer clique com o botão direito do rato numa linha](media/create-manage-policy/select-right-click.png)

4. Preencha a página **Proteger-se: Atribuir Iniciativa**, introduzindo as seguintes informações de exemplo. Pode utilizar as suas próprias informações.

   - Âmbito: a subscrição na qual guardou a iniciativa será a predefinição.  Pode alterar o âmbito para atribuir a iniciativa a um grupo de recursos na localização onde guardou a subscrição.
   - Exclusões: configure os recursos no âmbito para evitar que a atribuição de iniciativa seja aplicada aos mesmos.
   - Definição de iniciativa e Nome da atribuição: Proteger-se (pré-preenchida como o nome da iniciativa que está a ser atribuída).
   - Descrição: esta atribuição de iniciativa está adaptada para impor este grupo de definições de política.

5. Clique em **Atribuir**.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Excluir um recurso negado ou em não conformidade ao utilizar a Exclusão

Ao seguir o exemplo acima, depois de atribuir a definição de política para requerer a versão 12.0 do SQL Server, um servidor SQL criado com uma versão diferente da 12.0 seria negado. Nesta secção, tem um guia passo a passo para resolver uma tentativa negada de criar um SQL Server ao criar uma exclusão num único grupo de recursos. A exclusão impede a imposição da política (ou iniciativa) nesse recurso. No exemplo seguinte, qualquer versão do SQL Server será permitida num único grupo de recursos. Uma exclusão pode aplicar-se a um grupo de recursos ou pode restringir a exclusão a recursos individuais.

Uma implementação impedida devido a uma política ou iniciativa atribuída pode ser visualizada em duas localizações:

- No grupo de recursos visado pela implementação: selecione **Implementações** no lado esquerdo da página e clique no **Nome da Implementação** da implementação que falhou. O recurso que foi negado será apresentado com o estado _Proibido_. Para determinar a política ou iniciativa e a atribuição que negou o recurso, clique em **Falhou. Clique aqui para obter detalhes ->** na página de Descrição Geral da Implementação. Irá abrir uma janela no lado direito da página com as informações de erro. Em **Detalhes do Erro** estarão os GUIDs dos objetos da política relacionada.

   ![Implementação negada pela atribuição de política](media/create-manage-policy/rg-deployment-denied.png)

- Na página do Azure Policy: selecione **Conformidade** no lado esquerdo da página e clique na política **Requer a versão do SQL Server 12.0**. Na página que abre, verá um aumento na contagem de **Negar**. No separador **Eventos**, também verá quem fez a tentativa de implementação que foi negada pela política.

   ![Descrição geral de conformidade de uma política atribuída](media/create-manage-policy/compliance-overview.png)

Neste exemplo, Trent Baker, um dos especialistas em Virtualização da Contoso, estava a fazer o trabalho necessário. Temos de lhe conceder uma exceção, mas não queremos os SQL Servers que não são da versão 12.0 em qualquer grupo de recursos. Criámos um novo grupo de recursos, **SQLServers_Excluded** e irá agora conceder uma exceção a esta atribuição de política.

### <a name="update-assignment-with-exclusion"></a>Atualizar a atribuição com uma exclusão

1. Selecione **Atribuições** em **CRIAÇÃO** no lado esquerdo da página Azure Policy.
2. Procure por todas as atribuições de política e abra a atribuição *Requer a Versão do SQL Server 12.0*.
3. Defina a **Exclusão**, clicando nas reticências e selecionando o grupo de recursos a excluir, *SQLServers_Excluded* neste exemplo.

   ![Pedir exclusão](media/create-manage-policy/request-exclusion.png)

   > [!NOTE]
   > Consoante a política e o respetivo efeito, a exclusão também pode ser concedida a recursos específicos dentro de um grupo de recursos no âmbito da atribuição. Como neste tutorial foi utilizado um efeito **Negar**, não faria sentido definir a exclusão num recurso específico já existente.

4. Clique em **Selecionar** e em **Guardar**.

Nesta secção, resolveu a negação da tentativa de criar uma versão proibida do SQL Server ao criar uma exclusão num único grupo de recursos.

## <a name="clean-up-resources"></a>Limpar recursos

Se já acabou de trabalhar com os recursos deste tutorial, utilize os passos seguintes para eliminar quaisquer atribuições ou definições criadas acima:

1. Selecione **Definições** (ou **Atribuições** se estiver a tentar eliminar uma atribuição) em **CRIAÇÃO** no lado esquerdo da página Azure Policy.
2. Procure a nova definição de iniciativa ou de política (ou atribuição) que acabou de remover.
3. Clique com o botão direito do rato na linha ou selecione as reticências no fim da definição (ou atribuição) e selecione **Eliminar definição** (ou **Eliminar atribuição**).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial conseguiu realizar com êxito o seguinte:

> [!div class="checklist"]
> - Atribuir uma política para impor uma condição para os recursos que criar no futuro
> - Criar e atribuir uma definição de iniciativa para controlar a conformidade de vários recursos
> - Resolver um recurso negado ou em não conformidade
> - Implementar uma nova política numa organização

Para saber mais sobre as estruturas de definições de política, veja este artigo:

> [!div class="nextstepaction"]
> [Azure Policy definition structure](policy-definition.md) (Estrutura de definição do Azure Policy)