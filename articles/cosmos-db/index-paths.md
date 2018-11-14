---
title: Trabalhar com caminhos de índice no Azure Cosmos DB
description: Descrição geral de caminhos de índice no Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: rimman
ms.openlocfilehash: 0515397fb9ab0f05b4c763a2b05e9d986960bd91
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629297"
---
# <a name="index-paths-in-azure-cosmos-db"></a>Caminhos de índice no Azure Cosmos DB

Através de caminhos de índice no Azure Cosmos DB, pode optar por incluir ou excluir um caminho específico da indexação. Escolher caminhos específicos oferece desempenho melhorado de escrita e de armazenamento de índice mais baixo para cenários em que sabe que os padrões de consulta. Início de caminhos com a raiz de índice (`/`) o operador de caráter universal e, normalmente, terminar com o `?` operador de caráter universal. Este padrão indica que existem vários valores possíveis para o prefixo. Por exemplo, para servir a consulta `SELECT * FROM Families F WHERE F.familyName = "Andersen"`, tem de incluir um caminho de índice para `/familyName/?` na política de índice do contentor.

Caminhos de índice também podem utilizar o `*` operador de caráter universal para especificar o comportamento de caminhos recursivamente sob o prefixo. Por exemplo, `/payload/*` pode ser usado para excluir tudo sob a propriedade de payload da indexação.

## <a name="common-patterns-for-index-paths"></a>Padrões comuns para caminhos de índice

Aqui estão os padrões comuns para especificar caminhos de índice:

| **Caminho** | **Caso de utilização/descrição** |
| ---------- | ------- |
| /   | Caminho predefinido da coleção. Recursiva e aplica-se a árvore do documento inteiro.|
| / prop /?  | Caminho de índice necessário para servir consultas semelhante ao seguinte (com tipos de endereços ou intervalos de Hash, respectivamente):<br><br>SELECT FROM c coleção onde c.prop = "valor"<br><br>SELECT FROM c coleção onde c.prop > 5<br><br>SELECIONAR a partir de coleção c ORDER BY c.prop  |
| / prop / *  | Caminho de índice para todos os caminhos sob a etiqueta especificada. Funciona com as seguintes consultas<br><br>SELECT FROM c coleção onde c.prop = "valor"<br><br>SELECT FROM c coleção onde c.prop.subprop > 5<br><br>SELECT FROM c coleção onde c.prop.subprop.nextprop = "valor"<br><br>SELECIONAR a partir de coleção c ORDER BY c.prop |
| [] / propriedades / /?  | Caminho de índice necessário para servir a iteração e Junte-se a consultas em matrizes de escalares, como ["a", "b", "c"]:<br><br>SELECIONAR etiqueta de etiqueta IN collection.props etiqueta de onde = "valor"<br><br>SELECIONAR etiqueta de coleção c associação marca IN c.props onde Etiquetar > 5  |
| /Props/ [] /subprop/? | Caminho de índice necessário para servir a iteração e consultas de JUNÇÃO em relação a matrizes de objetos, como [{subprop: "a"}, {subprop: "b"}]:<br><br>SELECIONAR etiqueta de etiqueta IN collection.props onde tag.subprop = "valor"<br><br>SELECIONAR etiqueta de coleção c associação marca IN c.props onde tag.subprop = "valor" |
| / prop/subprop /? | Caminho de índice necessário para servir consultas (com tipos de endereços ou intervalos de Hash, respectivamente):<br><br>SELECT FROM c coleção onde c.prop.subprop = "valor"<br><br>SELECT FROM c coleção onde c.prop.subprop > 5  |

Quando definir os caminhos de índice personalizado, tem de especificar a regra de indexação predefinida para o item inteiro, assinalado com o caminho especial `/*`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a indexação no Azure Cosmos DB nos seguintes artigos:

- [Descrição geral de indexação](index-overview.md)
- [Políticas de indexação no Azure Cosmos DB](indexing-policies.md)
- [Tipos de índice no Azure Cosmos DB](index-types.md)
