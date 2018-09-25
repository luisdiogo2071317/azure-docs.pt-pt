---
title: Remediar recursos não compatíveis com o Azure Policy
description: Nesta explicação de procedimento orienta-o através de remediação de recursos que são incompatíveis para políticas no Azure Policy.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 747650bc47644cdca07f705f42d063c995ebe9bf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980258"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Remediar recursos não compatíveis com o Azure Policy

Recursos que são incompatíveis para um **deployIfNotExists** política pode ser colocada num Estado de conformidade por meio **remediação**. Remediação é conseguida ao instruindo a política para executar o **deployIfNotExists** efeito de política atribuída em seus recursos existentes. Nesta explicação de procedimento descreve os passos necessários para foi realizado.

## <a name="how-remediation-security-works"></a>Como funciona a segurança de remediação

Se política de executa o modelo **deployIfNotExists** definição de política, ele faz assim, usando um [identidade gerida](../../../active-directory/managed-identities-azure-resources/overview.md).
Política cria uma identidade gerida para cada atribuição para, mas tem de fornecer detalhes sobre quais funções para conceder a identidade gerida. Se a identidade gerida está em falta funções, é apresentado durante a atribuição de política ou uma iniciativa que contém a política. Quando utilizar o portal, política automaticamente concederá a identidade gerida as funções listadas assim que a atribuição é iniciada.

![Identidade gerida - função em falta](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Se um recurso modificado por **deployIfNotExists** está fora do âmbito da atribuição de política, identidade gerida da atribuição tem de ser por meio de programação concedida acesso ou a implementação de atualização irá falhar.

## <a name="configure-policy-definition"></a>Configurar a definição de política

A primeira etapa é definir as funções que **deployIfNotExists** precisa na definição de política para implementar o conteúdo do seu modelo incluído com êxito. Sob o **detalhes** propriedade, adicionar um **roleDefinitionIds** propriedade. Esta é uma matriz de cadeias de caracteres que correspondam a funções no seu ambiente.
Para obter um exemplo completo, consulte a [deployIfNotExists exemplo](../concepts/effects.md#deployifnotexists-example).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**roleDefinitionIds** utiliza o identificador de recurso completo e não tira o curto **roleName** da função. Para obter o ID para a função de "Contribuinte" no seu ambiente, utilize o seguinte código:

```azurecli-interactive
az role definition list --name 'Contributor'
```

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Contributor'
```

## <a name="create-a-remediation-task"></a>Criar uma tarefa de remediação

Durante a avaliação, a atribuição de política com **deployIfNotExists** efeito determina se existem recursos incompatíveis. Quando são encontrados recursos não compatíveis, os detalhes fornecidos no **remediação** página. Juntamente com a lista de políticas que tenham recursos em não conformidade é a opção para acionar uma **tarefas de remediação**. Este é o que cria uma implementação a partir da **deployIfNotExists** modelo.

Para criar uma **tarefas de remediação**, siga estes passos:

1. Inicie o serviço Azure Policy no portal do Azure ao clicar em **Todos os serviços** e, em seguida, ao pesquisar e selecionar **Policy**.

   ![Pesquisar política](../media/remediate-resources/search-policy.png)

1. Selecione **remediação** no lado esquerdo da página política do Azure.

   ![Selecione a remediação](../media/remediate-resources/select-remediation.png)

1. Todos os **deployIfNotExists** atribuições de política com recursos não compatíveis estão incluídas no **políticas para remediar** separador e os dados de tabela. Clique numa política com recursos que são incompatíveis. O **nova tarefa de remediação** é aberta a página.

   > [!NOTE]
   > Uma forma alternativa para abrir o **tarefas de remediação** página é a localização e clique na política da **conformidade** página, em seguida, clique no **criar tarefa de remediação** botão.

1. Na **nova tarefa de remediação** página, filtrar os recursos necessários para remediar utilizando o **âmbito** reticências e escolha os recursos subordinados de quais a política foi atribuída (incluindo para baixo para o recurso individual objetos). Além disso, utilizar o **localizações** pendente para filtrar ainda mais os recursos. Apenas os recursos listados na tabela serão remediados.

   ![Remediar - selecionar recursos](../media/remediate-resources/select-resources.png)

1. Iniciar a tarefa de atualização de uma vez que os recursos foram filtrados, clicando em **Remedeie**. A página de política de conformidade será aberta para o **tarefas de atualização** separador para mostrar o estado do progresso de tarefas.

   ![Remediar – progresso da tarefa](../media/remediate-resources/task-progress.png)

1. Clique nas **tarefas de remediação** da página de conformidade da política para obter detalhes sobre o progresso. A filtragem utilizado para a tarefa são mostradas juntamente com uma lista dos recursos a ser corrigida.

1. Partir do **tarefa remedation** página, clique com o botão direito num recurso para ver a implementação da tarefa de atualização ou o recurso. No final da linha, clique em **eventos relacionados com** para ver os detalhes, como uma mensagem de erro.

   ![Remediar - menu de contexto de tarefa de recursos](../media/remediate-resources/resource-task-context-menu.png)

Recursos implementados através de um **tarefas de remediação** será adicionado à **recursos implementados** separador na página de conformidade de política após um curto atraso.

## <a name="next-steps"></a>Passos Seguintes

- Reveja exemplos em [exemplos do Azure Policy](../samples/index.md)
- Reveja o [estrutura de definição de política](../concepts/definition-structure.md)
- Revisão [Noções básicas sobre os efeitos de política](../concepts/effects.md)
- Compreender como [criar políticas programaticamente](programmatically-create.md)
- Saiba como [obter dados de conformidade](getting-compliance-data.md)
- Rever o que é um grupo de gestão, com [Organizar os recursos com grupos de gestão do Azure](../../management-groups/overview.md)