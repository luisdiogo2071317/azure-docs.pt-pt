---
title: Descrição geral do Azure Resource Graph
description: O Azure Resource Graph é um serviço no Azure que permite uma consulta complexa de recursos à escala.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d68183f4d0a928ac72f3f73ea5225ad174820cb7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162105"
---
# <a name="what-is-azure-resource-graph"></a>O que é o Azure Resource Graph

O Azure Resource Graph é um serviço no Azure que foi desenvolvido para ampliar o Azure Resource Management, ao fornecer exploração de recursos eficiente e de alto desempenho, com a capacidade de consultar à escala todas as subscrições e grupos de gestão para que possa controlar de forma eficiente o seu ambiente. Estas consultas fornecem as seguintes funcionalidades:

- Capacidade de consultar recursos com filtragem, agrupamento e classificação complexos por propriedades de recursos.
- Capacidade de explorar de maneira iterativa os recursos com base nos requisitos de governação e de converter a expressão resultante numa definição de política.
- Capacidade de avaliar o impacto da aplicação de políticas num vasto ambiente de cloud.

Nesta documentação, vamos abordar cada capacidade de forma detalhada.

> [!NOTE]
> O Azure Resource Graph é utilizado pela nova experiência de navegação de “Todos os recursos” do portal do Azure. Foi concebido para ajudar os clientes com a necessidade de gerir os ambientes de grande escala.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Como é que o Resource Graph complementa o Azure Resource Manager?

Atualmente, o Azure Resource Manager envia dados para uma cache de recursos limitados que expõe vários campos de recursos, mais especificamente: Nome do recurso, ID, Tipo, Grupo de Recursos, Subscrições e Localização. Hoje em dia, se quiser trabalhar com mais propriedades de recursos, terá de fazer chamadas para cada fornecedor de recursos individuais e pedir os detalhes das propriedade de cada recurso.

Com o Azure Resource Graph, pode aceder a estas propriedades devolvidas pelos fornecedores de recursos sem a necessidade de fazer chamadas individuais para cada fornecedor de recursos.

## <a name="the-query-language"></a>Linguagem de consulta

Agora que compreende melhor o que é o Azure Resource Graph, vamos aprofundar como criar consultas.

É importante compreender que a linguagem de consulta do Azure Resource Graph se baseia na [Linguagem de Consulta do Azure Data Explorer](../../data-explorer/data-explorer-overview.md).

Em primeiro lugar, para obter detalhes sobre operações e funções que podem ser utilizadas com o Azure Resource Graph, veja [Linguagem de consulta do Resource Graph](./concepts/query-language.md). Para procurar recursos, veja [explorar recursos](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Permissões no Azure Resource Graph

Para utilizar o Resource Graph, tem de possuir autorização no [Controlo de acesso baseado em funções](../../role-based-access-control/overview.md) (RBAC) com acesso de leitura, pelo menos, para os recursos que pretende consultar. Se não tiver permissões de `read` no grupo de gestão, na subscrição, no grupo de recursos ou no recurso individual, não será devolvido nada nos resultados de uma consulta do Resource Graph.

## <a name="running-your-first-query"></a>Executar a primeira consulta

O Resource Graph suporta a CLI do Azure e o Azure PowerShell. O componente de consulta está estruturado da mesma forma, independentemente da linguagem utilizada. O suporte para o Azure Resource Graph ainda não está disponível por predefinição em nenhum SDK, razão pela qual tem de ser carregado um módulo ou uma extensão para fornecer os comandos necessários.
Saiba como ativar o Resource Graph na [CLI do Azure](first-query-azurecli.md#add-the-resource-graph-extension) e no [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Passos seguintes

- Executar a primeira consulta com a [CLI do Azure](first-query-azurecli.md)
- Executar a primeira consulta com o [Azure PowerShell](first-query-powershell.md)
- Começar com as [Consultas de Introdução](./samples/starter.md)
- Melhorar a compreensão com as [Consultas Avançadas](./samples/advanced.md)