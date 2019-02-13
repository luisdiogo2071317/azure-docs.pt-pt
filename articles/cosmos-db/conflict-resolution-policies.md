---
title: Tipos de resolução de conflito e diretivas de resolução com múltiplos escrever regiões no Azure Cosmos DB
description: Este artigo descreve as categorias de conflito e diretivas de resolução de conflito no Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 6b168efc2314e21f381d55f6ac79cda8b426e66d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110307"
---
# <a name="conflict-types-and-resolution-policies"></a>Tipos de conflito e políticas de resolução

Entra em conflito e resolução de conflitos de políticas são aplicáveis se a sua conta do Azure Cosmos DB está configurada com várias regiões de escrita.

Para contas do Azure Cosmos DB configuradas com várias regiões de escrita, podem ocorrer conflitos de atualização quando gravadores atualizar simultaneamente no mesmo item em várias regiões. Conflitos de atualização são classificados em três tipos:

* **Inserir conflitos**: Esses conflitos podem ocorrer quando um aplicativo insere simultaneamente dois ou mais itens com o mesmo índice exclusivo de duas ou mais regiões. Por exemplo, pode ocorrer este conflito com uma propriedade de ID. Todas as gravações, talvez seja bem-sucedida inicialmente em seus respectivos regiões locais. Mas, com base na política de resolução de conflito que escolher, apenas um item com o ID original é finalmente confirmado.

* **Substitua conflitos**: Esses conflitos podem ocorrer quando uma aplicação atualizar um único item em simultâneo de duas ou mais regiões.

* **Eliminar conflitos**: Esses conflitos podem ocorrer quando uma aplicação em simultâneo elimina um item de uma região e atualiza-o partir de outra região.

## <a name="conflict-resolution-policies"></a>Políticas de resolução de conflitos

O Azure Cosmos DB oferece um mecanismo orientado por diretivas flexível para resolver conflitos de atualização. Pode selecionar de entre duas políticas de resolução de conflitos num contentor do Azure Cosmos DB:

- **Wins (LWW) da última escrita**: Esta política de resolução, por predefinição, utiliza uma propriedade timestamp definidos pelo sistema. Baseia-se no protocolo de sincronização de hora de relógio. Se utilizar a API de SQL do Azure Cosmos DB, pode especificar qualquer outra propriedade personalizada numérica a ser utilizado para resolução de conflitos. Uma propriedade numérica personalizada é também referida como o caminho de resolução de conflito. 

  Se dois ou mais conflitos de itens em inserem ou substituem as operações, o item com o valor mais alto para o caminho de resolução de conflito torna-se o vencedor. O sistema determine o vencedor, se vários itens têm o mesmo valor numérico para o caminho de resolução de conflito. Todas as regiões são garantidas para convergir a um único vencedor e terminam cópia de segurança com a mesma versão do item de compromisso. Quando eliminar conflitos estão envolvidos, sempre a versão eliminada wins inserir ou substituir os conflitos. Este resultado ocorre não importa o valor do caminho de resolução de conflito.

  > [!NOTE]
  > Última escrever Wins é a política de resolução de conflito de predefinida. Está disponível para contas SQL, tabelas do Azure Cosmos DB, MongoDB, Cassandra e Gremlin API.

  Para obter mais informações, consulte [exemplos que usam LWW entram em conflito de políticas de resolução](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Custom**: Esta política de resolução destina-se a semântica definida pelo aplicativo para reconciliação de conflitos. Quando definir esta política no seu contentor do Azure Cosmos DB, também tem de registar um procedimento armazenado de intercalação. Este procedimento é invocado automaticamente quando está em conflito é detetadas numa transação de base de dados no servidor. O sistema fornece exatamente garantir uma vez para a execução de um procedimento de mesclagem como parte do protocolo de compromisso.  

  Existem dois pontos a serem lembrados se configurar o seu contentor com a opção de resolução personalizados. Se conseguir registar um procedimento de intercalação no contentor ou o procedimento de intercalação lança uma exceção em tempo de execução, os conflitos são escritos para a feed de entra em conflito. Seu aplicativo, em seguida, tem de resolver manualmente os conflitos em conflitos feed. Para obter mais informações, consulte [exemplos de como utilizar a política de resolução personalizados e como utilizar os conflitos de feed](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Política de resolução de conflito personalizado está disponível apenas para contas da API de SQL.

## <a name="next-steps"></a>Passos Seguintes

Saiba como configurar políticas de resolução de conflito. Consulte os seguintes artigos:

* [Como configurar vários mestres em seus aplicativos](how-to-multi-master.md)
* [Utilizar a política de resolução de conflito LWW](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Utilizar a política de resolução de conflito personalizado](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Utilize os conflitos de feed](how-to-manage-conflicts.md#read-from-conflict-feed)
