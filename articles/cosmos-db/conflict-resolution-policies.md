---
title: Tipos de resolução de conflito e diretivas de resolução no Azure Cosmos DB
description: Este artigo descreve as categorias de conflito e diretivas de resolução de conflito no Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 1b2a122cc8a04d4f0044ecb0fe0341357bc29c0f
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514830"
---
# <a name="conflict-types-and-resolution-policies"></a>Tipos de conflito e diretivas de resolução

Entra em conflito e resolução de conflitos de políticas são aplicáveis se a sua conta do Cosmos está configurada com várias regiões de escrita.

Para contas do Cosmos configuradas com várias regiões de escrita, podem ocorrer conflitos de atualização quando vários gravadores atualizar simultaneamente no mesmo item em várias regiões. Conflitos de atualização são classificados em três tipos:

1. **Inserir conflitos:** estes conflitos podem ocorrer quando um aplicativo insere simultaneamente dois ou mais itens com o mesmo índice exclusivo (por exemplo, a propriedade de ID) de duas ou mais regiões. Neste caso, todas as escritas podem ter êxito inicialmente em seus respectivos regiões locais, mas com base na política de resolução de conflito que escolher, apenas um item com o ID original é finalmente confirmado.

1. **Substitua conflitos:** estes conflitos podem ocorrer quando uma aplicação atualizar um único item em simultâneo de duas ou mais regiões.

1. **Eliminar conflitos:** estes conflitos podem ocorrer quando uma aplicação em simultâneo elimina um item de uma região e atualiza-o partir de outra região.

## <a name="conflict-resolution-policies"></a>Políticas de resolução de conflitos

O cosmos DB oferece um mecanismo orientado por diretivas flexível para resolver conflitos de atualização. Pode selecionar as seguintes políticas de resolução de conflito de dois num contêiner de Cosmos:

- **Última-escrita-Wins (LWW):** esta política de resolução por predefinição, utiliza uma propriedade timestamp definidos pelo sistema (com base no protocolo de sincronização de hora de relógio). Em alternativa, ao utilizar a API de SQL, o Cosmos DB permite-lhe especificar qualquer outra propriedade numérica personalizada (também referida como o "caminho de resolução de conflito") para ser usado na resolução de conflitos.  

  Se dois ou mais conflitos de itens em inserem ou substituem as operações, o item que contém o valor mais alto para o "caminho de resolução de conflito" torna-se o "vencedor". Se vários itens têm o mesmo valor numérico para o caminho de resolução de conflito, a versão de "vencedor" selecionado é determinada pelo sistema. Todas as regiões são garantidas para convergir a um único vencedor e terminam cópia de segurança com a versão idêntica do item de compromisso. Se existirem conflitos de eliminação envolvidas, a versão eliminada wins sobre qualquer um dos insert sempre ou substituir conflitos, independentemente do valor do caminho de resolução de conflito.

  > [!NOTE]
  > Última-escrita-Wins é a política de resolução de conflito de predefinida e está disponível para contas SQL, tabela, MongoDB, Cassandra e Gremlin API.

  Para obter mais informações, consulte [exemplos usando LWW entram em conflito de políticas de resolução](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Personalizada:** esta política de resolução foi concebida para definida pelo aplicativo semântica para reconciliação de conflitos. Ao definir esta política no seu contentor do Cosmos, também tem de registar um procedimento de intercalação armazenado, o que é invocado automaticamente quando conflitos de atualização são detetados numa transação de base de dados no servidor. O sistema fornece exatamente garantir uma vez para a execução de um procedimento de mesclagem como parte do protocolo de compromisso.  

  No entanto, se configurar o seu contentor com a opção de resolução personalizados, mas qualquer uma falha ao registar um procedimento de intercalação no contentor, ou se o procedimento de intercalação lança uma exceção em tempo de execução, os conflitos são escritos para a feed de entra em conflito. Seu aplicativo, em seguida, terá de resolver manualmente os conflitos no feed-conflitos. Para obter mais informações, consulte [exemplos de como utilizar a política de resolução personalizados e como utilizar os conflitos de feed](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Política de resolução de conflito personalizados só está disponível para contas da API de SQL.

## <a name="next-steps"></a>Passos Seguintes

Em seguida pode saber como configurar políticas de resolução de conflito, utilizando os seguintes artigos:

* [Como utilizar a política de resolução de conflito LWW](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Como utilizar a política de resolução de conflito personalizado](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Como utilizar o feed-conflitos](how-to-manage-conflicts.md#read-from-conflict-feed)
