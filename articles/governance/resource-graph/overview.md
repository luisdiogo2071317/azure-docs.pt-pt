---
title: Descrição geral do Azure Resource Graph
description: O Azure Resource Graph é um serviço no Azure que permite uma consulta complexa de recursos à escala.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/22/2018
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: ed64f6317fefb9e82dbe14e806499965d926d434
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316204"
---
# <a name="what-is-azure-resource-graph"></a>O que é o Azure Resource Graph

O Azure Resource Graph é um serviço no Azure que foi desenvolvido para ampliar o Azure Resource Management, ao fornecer exploração de recursos eficiente e de alto desempenho, com a capacidade de consultar à escala todas as subscrições e grupos de gestão para que possa controlar de forma eficiente o seu ambiente. Estas consultas fornecem as seguintes funcionalidades:

- Capacidade de consultar recursos com filtragem, agrupamento e classificação complexos por propriedades de recursos.
- Capacidade de explorar de maneira iterativa os recursos com base nos requisitos de governação e de converter a expressão resultante numa definição de política.
- Capacidade de avaliar o impacto da aplicação de políticas num vasto ambiente de cloud.

Nesta documentação, vamos abordar cada funcionalidade de forma detalhada.

> [!NOTE]
> O Azure Resource Graph é utilizado pela nova experiência de navegação de “Todos os recursos” do portal do Azure. Foi concebido para ajudar os clientes com a necessidade de gerir os ambientes de grande escala.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Como é que o Resource Graph complementa o Azure Resource Manager?

Atualmente, o Azure Resource Manager envia dados para uma cache de recursos limitados que disponibiliza vários campos de recursos, mais especificamente: Nome do recurso, ID, Tipo, Grupo de Recursos, Subscrições e Localização. Antes, trabalhar com mais propriedades de recursos implicava fazer chamadas para cada fornecedor de recursos individuais e pedir os detalhes das propriedades de cada recurso.

Com o Azure Resource Graph, pode aceder a estas propriedades devolvidas pelos fornecedores de recursos sem a necessidade de fazer chamadas individuais para cada fornecedor de recursos.

## <a name="the-query-language"></a>Linguagem de consulta

Agora que compreende melhor o que é o Azure Resource Graph, vamos aprofundar como criar consultas.

É importante compreender que a linguagem de consulta do Azure Resource Graph se baseia na [Linguagem de Consulta do Azure Data Explorer](../../data-explorer/data-explorer-overview.md).

Em primeiro lugar, para obter detalhes sobre operações e funções que podem ser utilizadas com o Azure Resource Graph, veja [Linguagem de consulta do Resource Graph](./concepts/query-language.md). Para procurar recursos, veja [explorar recursos](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Permissões no Azure Resource Graph

Para utilizar o Resource Graph, tem de possuir os direitos adequados no [Controlo de acesso baseado em funções](../../role-based-access-control/overview.md) (RBAC) com acesso de leitura, pelo menos, aos recursos que pretende consultar. Sem, pelo menos, permissões `read` para o objeto do Azure ou o grupo de objetos, não seriam devolvidos resultados.

## <a name="running-your-first-query"></a>Executar a primeira consulta

O Resource Graph suporta a CLI do Azure e o Azure PowerShell. A consulta está estruturada da mesma forma para os dois idiomas. Saiba como ativar o Resource Graph na [CLI do Azure](first-query-azurecli.md#add-the-resource-graph-extension) e no [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Passos Seguintes

- Executar a primeira consulta com a [CLI do Azure](first-query-azurecli.md)
- Executar a primeira consulta com o [Azure PowerShell](first-query-powershell.md)
- Começar com as [Consultas de Introdução](./samples/starter.md)
- Melhorar a compreensão com as [Consultas Avançadas](./samples/advanced.md)