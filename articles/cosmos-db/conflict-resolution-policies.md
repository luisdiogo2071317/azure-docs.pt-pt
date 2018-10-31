---
title: Resolução de conflitos no Azure Cosmos DB
description: Este artigo descreve as categorias de conflito e diretivas de resolução de conflito no Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 2f219ed6c3155e8b7d3d978116e1748f6c115fea
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244116"
---
# <a name="conflict-types-and-resolution-policies"></a>Tipos de conflito e diretivas de resolução

Entra em conflito e resolução de conflitos de políticas são aplicáveis se a sua conta do Cosmos está configurada com várias regiões de escrita.

Para contas do Cosmos DB configuradas com várias regiões de escrita, podem ocorrer conflitos de atualização quando vários gravadores atualizar simultaneamente no mesmo item em várias regiões. Conflitos de atualização são classificados em três tipos:

1. **Inserir conflitos** pode ocorrer quando um aplicativo insere simultaneamente dois ou mais itens com o mesmo índice exclusivo (por exemplo, a propriedade de ID) de duas ou mais regiões. Neste caso, todas as escritas podem ter êxito inicialmente em seus respectivos regiões locais, mas com base na política de resolução de conflito que escolher, apenas um item com o ID original é finalmente confirmado.
2. **Substitua conflitos** pode ocorrer quando uma aplicação atualizar um único item em simultâneo de duas ou mais regiões.
3. **Eliminar conflitos** pode ocorrer quando uma aplicação em simultâneo elimina um item de uma região e atualiza-o partir de qualquer outra região.

## <a name="conflict-resolution-policies"></a>Políticas de resolução de conflito

O cosmos DB oferece um mecanismo orientado por diretivas flexível para a resolução de conflitos de atualização. Pode selecionar as seguintes políticas de resolução de conflito de dois num contêiner de Cosmos:

- **Última-escrita-Wins (LWW)** que, por predefinição, utiliza uma propriedade timestamp definidos pelo sistema (com base no protocolo de sincronização de hora de relógio). Em alternativa, ao utilizar a API de SQL, Cosmos DB permite-lhe especificar qualquer outra propriedade numérica personalizada (também referida como o "caminho de resolução de conflito") a ser utilizado para resolução de conflitos.  

Se dois ou mais conflitos de itens em inserem ou substituem as operações, o item que contém o valor mais alto para o "caminho de resolução de conflito" torna-se o "vencedor". Se vários itens têm o mesmo valor numérico para o caminho de resolução de conflito, a versão de "vencedor" selecionado é determinada pelo sistema. Todas as regiões são garantidas para convergir a um único vencedor e terminam cópia de segurança com a versão idêntica do item de compromisso. Se existirem eliminar conflitos envolvidas, a versão eliminada wins-se sempre através de inserções ou conflitos de replace, independentemente do valor do caminho de resolução de conflito.

> [!NOTE]
> LWW é a política de resolução de conflito de predefinida e está disponível para o SQL, tabela, MongoDB, Cassandra e APIs do Gremlin.

Para obter mais informações, consulte [exemplos usando LWW entram em conflito de políticas de resolução](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Custom** esta política destina-se a semântica definida pelo aplicativo para reconciliação de conflitos. Ao definir esta política no seu contentor do Cosmos, também tem de registar um procedimento armazenado de intercalação, que é invocado automaticamente quando conflitos de atualização são detetados numa transação de base de dados no lado do servidor. O sistema fornece exatamente garantir uma vez para a execução de um procedimento de mesclagem como parte do protocolo de compromisso.  

No entanto, se configurar o seu contentor com a opção de resolução personalizados, mas qualquer uma falha ao registar um procedimento de intercalação no contentor, ou se o procedimento de intercalação lança uma exceção em tempo de execução, os conflitos são escritos para a feed de entra em conflito. Seu aplicativo, em seguida, terá de resolver manualmente os conflitos no feed-conflitos. Para obter mais informações, consulte [exemplos de como utilizar a política de resolução personalizados e como utilizar os conflitos de feed](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

> [!NOTE]
> Política de resolução de conflito personalizados só está disponível para contas da API de SQL.

## <a name="next-steps"></a>Passos Seguintes

Em seguida pode saber como configurar políticas de resolução de conflito, utilizando os seguintes artigos:

* [Como utilizar a política de resolução de conflito LWW](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Como utilizar a política de resolução de conflito personalizado](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Como utilizar o feed-conflitos](how-to-manage-conflicts.md#read-from-conflict-feed)
