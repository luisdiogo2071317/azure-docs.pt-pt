---
title: Criar tabelas de armazenamento do Azure para a modificação de dados | Microsoft Docs
description: Criar tabelas de modificação de dados no armazenamento de tabelas do Azure.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 6c008175f01521ce4f96d13e58244dc72d9f6990
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661062"
---
# <a name="design-for-data-modification"></a>Estrutura de modificação de dados
Este artigo foca-se em considerações de design para otimizar as introduções, atualizações e elimina. Em alguns casos, terá de avaliar o compromisso entre as estruturas que otimizar para consulta contra estruturas otimizar a modificação de dados tal como o que fazer em estruturas de bases de dados relacionais (embora as técnicas para gerir os compromissos de design diferentes numa base de dados relacional). A secção [padrões de conceção de tabela](#table-design-patterns) descreve alguns padrões de conceção de detalhado para o serviço de tabela e realça algumas destas compromissos. Na prática, irá encontrar que muitas estruturas otimizadas para consultar entidades também funcionam bem para modificar as entidades.  

## <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>Otimizar o desempenho de inserção, atualização e operações de eliminação
Para atualizar ou eliminar uma entidade, deve conseguir identificá-lo utilizando o **PartitionKey** e **RowKey** valores. No que esta respeita, à sua escolha de **PartitionKey** e **RowKey** para modificar as entidades deve seguir critérios semelhantes à sua escolha para suportar o ponto de consultas porque pretende identificar entidades mais eficientemente possível. Não pretende utilizar uma análise ineficaz de partição ou tabela para localizar uma entidade para detetar o **PartitionKey** e **RowKey** valores tem de atualizar ou eliminá-lo.  

Os seguintes padrões na secção [padrões de conceção de tabela](#table-design-patterns) otimizar o desempenho ou a inserção, atualização de endereço e operações de eliminação:  

* [Um volume elevado eliminar padrão](table-storage-design-patterns.md#high-volume-delete-pattern) -ativar a eliminação de um grande volume de entidades armazenando todas as entidades para eliminação em simultâneo na sua própria tabela separada; eliminar as entidades por eliminar a tabela.  
* [Padrão de séries de dados](table-storage-design-patterns.md#data-series-pattern) -série de dados completa de loja uma entidade única para minimizar o número de pedidos que efetuar.  
* [Padrão de entidades Wide](table-storage-design-patterns.md#wide-entities-pattern) -utilizar várias entidades físicas para armazenar as entidades lógicas com mais do que 252 propriedades.  
* [Padrão de entidades grandes](table-storage-design-patterns.md#large-entities-pattern) -armazenamento de BLOBs de utilização para armazenar os valores de propriedade grandes.  

## <a name="ensure-consistency-in-your-stored-entities"></a>Certifique-se de consistência na sua entidades armazenadas
O fator chave que influencia à sua escolha de chaves para otimizar as modificações de dados é como garantir consistência utilizando transações atomic. Só pode utilizar um EGT a funcionar entidades armazenadas na mesma partição.  

Os seguintes padrões no artigo [padrões de conceção de tabela](table-storage-design-patterns.md) endereço gerir consistência:  

* [Padrão de índice secundário intra partição](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -armazenar várias cópias de cada entidade utilizando diferentes **RowKey** valores (a mesma partição) para ativar rápido e eficiente pesquisas e as ordens de ordenação alternada utilizando diferentes **RowKey** valores.  
* [Padrão de índice secundário partição entre](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - armazenar várias cópias de cada entidade a utilizar valores diferentes de RowKey em partições separadas ou em tabelas separadas para ativar rápido e eficiente pesquisas e ordenação alternada ordena utilizando diferentes**RowKey** valores.  
* [Padrão de transações eventualmente consistente](table-storage-design-patterns.md#eventually-consistent-transactions-pattern) -ativar o comportamento eventualmente consistente em limites de partição ou limites de sistema de armazenamento ao utilizar as filas do Azure.
* [Padrão de entidades do índice](table-storage-design-patterns.md#index-entities-pattern) -manter entidades de índice para ativar pesquisas eficiente que devolvem apresenta uma lista de entidades.  
* [Padrão de denormalization](table-storage-design-patterns.md#denormalization-pattern) -combinar dados relacionados com o em conjunto numa única entidade que lhe permite obter todos os dados que precisa com uma ponto único de consulta.  
* [Padrão de séries de dados](table-storage-design-patterns.md#data-series-pattern) -série de dados completa de loja uma entidade única para minimizar o número de pedidos que efetuar.  

Para obter informações sobre transações do grupo de entidade, consulte a secção [transações do grupo de entidade](table-storage-design.md#entity-group-transactions).  

## <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Certifique-se de que a estrutura para que as modificações eficiente facilita eficiente consultas
Em muitos casos, uma estrutura eficaz resulta consultas em modificações eficiente, mas deve sempre avaliar se for este o caso para o seu cenário específico. Alguns dos padrões no artigo [padrões de conceção de tabela](table-storage-design-patterns.md) explicitamente avaliar compromissos entre consultas e modificar as entidades e deve sempre ter em consideração o número de cada tipo de operação.  

Os seguintes padrões no artigo [padrões de conceção de tabela](table-storage-design-patterns.md) compromissos entre conceber consultas eficiente e estruturar para modificação de dados eficiente de endereços:  

* [Padrão de chave composta](table-storage-design-patterns.md#compound-key-pattern) -utilização composta **RowKey** valores para permitir que um cliente para procurar dados relacionados com um ponto único de consulta.  
* [Padrão de seguimento de registo](table-storage-design-patterns.md#log-tail-pattern) -obter o *n* entidades recentemente adicionadas a uma partição utilizando um **RowKey** valor ordena na data inversa e ordem de tempo.  
