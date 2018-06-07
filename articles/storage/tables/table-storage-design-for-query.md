---
title: Tabelas de armazenamento do Azure para consultas de design | Microsoft Docs
description: Criar tabelas de consultas no table storage do Azure.
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
ms.openlocfilehash: b8d2033b0b29caddf165f4b582c7d0578109480c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661074"
---
# <a name="design-for-querying"></a>Conceção para consultas
Soluções de serviço tabela poderão ser leitura intensiva, intensiva de escrita ou uma combinação dos dois. Este artigo incida nas questões a tenha em consideração quando está a conceber o seu serviço de tabela para suportar operações de leitura de forma eficiente. Normalmente, também é eficiente para operações de escrita de uma estrutura que suporta operações de leitura de forma eficiente. No entanto, existem considerações adicionais para tenha em consideração quando conceber para suportar operações, abordadas no artigo de escrita [Design de modificação de dados](table-storage-design-for-modification.md).

É um ponto de partida para conceber a sua solução de serviço tabela para que possa ler os dados de forma eficiente e peça "consultas de que a minha aplicação terá de executar para obter os dados que necessita do serviço tabela?"  

> [!NOTE]
> Com o serviço tabela, é importante obter o design correto adiantado porque é difícil e dispendiosas alterá-la mais tarde. Por exemplo, numa base de dados relacional é frequentemente possível para resolver problemas de desempenho simplesmente através da adição de índices para uma base de dados: não é uma opção com o serviço de tabela.  
> 
> 

Esta secção centra-se nos assuntos chaves que devem ser abordadas ao conceber as tabelas para consultas. Tópicos abrangidos nesta secção incluem:

* [Como à sua escolha de PartitionKey e RowKey tem impacto no desempenho das consultas](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Escolher um PartitionKey adequado](#choosing-an-appropriate-partitionkey)
* [Otimizar as consultas para o serviço tabela](#optimizing-queries-for-the-table-service)
* [Ordenação de dados no serviço tabela](#sorting-data-in-the-table-service)

## <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Como à sua escolha de PartitionKey e RowKey tem impacto no desempenho das consultas
Os exemplos seguintes partem do princípio de serviço tabela está a armazenar as entidades de empregado com a seguinte estrutura (omita a maioria dos exemplos de **Timestamp** propriedade para efeitos de clareza):  

| *Nome da coluna* | *Tipo de dados* |
| --- | --- |
| **PartitionKey** (nome do departamento de) |Cadeia |
| **RowKey** (Id de empregado) |Cadeia |
| **FirstName** |Cadeia |
| **LastName** |Cadeia |
| **idade** |Número inteiro |
| **EmailAddress** |Cadeia |

O artigo [descrição geral do armazenamento de Azure Table](table-storage-overview.md) descreve algumas das principais funcionalidades do serviço tabela do Azure que tenham uma influência direta na conceção para a consulta. Estes fazer com as seguintes diretrizes gerais para conceber consultas do serviço de tabela. Tenha em atenção que a sintaxe de filtro utilizada nos exemplos abaixo é a partir da API de REST do serviço tabela para obter mais informações, consulte [entidades de consulta](https://docs.microsoft.com/rest/api/storageservices/Query-Entities).  

* A ***ponto consulta*** é a pesquisa mais eficiente para utilizar e é recomendada a utilizar para pesquisas de elevado volume ou pesquisas que necessitam de latência mais baixa. Este tipo uma consulta pode utilizar os índices para localizar uma entidade individual de forma muito eficiente, especificando ambos os **PartitionKey** e **RowKey** valores. Por exemplo: $filter = (PartitionKey eq 'Vendas') e (RowKey eq '2')  
* Segundo melhor é um ***intervalo de consulta*** que utiliza o **PartitionKey** e os filtros num intervalo de **RowKey** valores para devolver mais de uma entidade. O **PartitionKey** valor identifica uma partição específica e o **RowKey** valores identificam um subconjunto de entidades dessa partição. Por exemplo: $filter = PartitionKey eq 'Dos vendas e do RowKey ge' e RowKey lt possível '  
* É melhor terceiro um ***partição analisar*** que utiliza o **PartitionKey** e filtros e que a outra propriedade de chave não podem devolver mais de uma entidade. O **PartitionKey** valor identifica uma partição específica, e a propriedade de valores select para um subconjunto de entidades dessa partição. Por exemplo: $filter = PartitionKey eq 'Vendas' e LastName eq 'Santos'  
* A ***tabela analisar*** não inclui o **PartitionKey** e é muito ineficaz porque este procura todas as partições que compõem a sua tabela por sua vez para qualquer entidades correspondentes. A operação executará uma análise da tabela, independentemente se pretende ou não utiliza o filtro de **RowKey**. Por exemplo: $filter = LastName eq 'Jones'  
* As consultas que devolvem várias entidades devolvem-las ordenados **PartitionKey** e **RowKey** ordem. Para evitar resorting as entidades no cliente, escolha um **RowKey** que define a sequência de ordenação mais comuns.  

Tenha em atenção que utilizar um "**ou**" para especificar um filtro com base no **RowKey** valores resultados de uma análise de partição e não é tratado como uma consulta de intervalo. Por conseguinte, deve evitar consultas que utilizam os filtros, tais como: $filter = PartitionKey eq 'Vendas' e (RowKey eq '121' ou RowKey eq '322')  

Para obter exemplos de código do lado do cliente que utilizam a biblioteca de clientes de armazenamento para executar consultas eficiente, consulte:  

* [Executar uma consulta de ponto de utilizar a biblioteca de clientes de armazenamento](table-storage-design-patterns.md#executing-a-point-query-using-the-storage-client-library)
* [Obter várias entidades com LINQ](table-storage-design-patterns.md#retrieving-multiple-entities-using-linq)
* [Projecção do lado do servidor](table-storage-design-patterns.md#server-side-projection)  

Para obter exemplos de código do lado do cliente que pode processar vários tipos de entidade armazenados na mesma tabela, consulte:  

* [Trabalhar com tipos de entidade heterogénea](table-storage-design-patterns.md#working-with-heterogeneous-entity-types)  

## <a name="choosing-an-appropriate-partitionkey"></a>Escolher um PartitionKey adequado
À sua escolha de **PartitionKey** deve balancear a necessidade de ativar a utilização de EGTs (para garantir consistência) com o requisito de distribuir as entidades por várias partições (para garantir que uma solução dimensionável).  

Em uma Alpine, pode armazenar todas as entidades numa partição única, mas isto pode limitar a escalabilidade da sua solução e impediriam que o serviço de tabela a ser capaz de pedidos de balanceamento de carga. Em outra Alpine, pode armazenar uma entidade por partição, que seria altamente dimensionável e que permite que o serviço tabela pedidos de balanceamento de carga, mas que iria impedi-lo da utilização de transações do grupo de entidade.  

Um ideal **PartitionKey** um que permite-lhe utilizar consultas eficiente e que tem partições suficientes para garantir a sua solução escalável. Normalmente, irá encontrar que as entidades terão uma propriedade adequada que distribui as entidades partições suficientes.

> [!NOTE]
> Por exemplo, um sistema que armazena informações sobre os utilizadores ou os funcionários, UserID pode ser uma boa PartitionKey. Pode ter várias entidades que utilizam um ID de utilizador especificado como a chave de partição. Cada entidade que armazena dados sobre um utilizador se encontra agrupada para uma única partição e, por isso, estas entidades estão acessíveis através de transações do grupo de entidade, enquanto altamente dimensionável.
> 
> 

Existem considerações adicionais na sua escolha de **PartitionKey** relacionadas com a forma como irá inserir, atualizar e eliminar entidades. Para obter mais informações, consulte [conceber tabelas de modificação de dados](table-storage-design-for-modification.md).  

## <a name="optimizing-queries-for-the-table-service"></a>Otimizar as consultas para o serviço tabela
O serviço tabela indexa automaticamente os seus entidades utilizando o **PartitionKey** e **RowKey** valores existentes num índice em cluster único, por conseguinte, o motivo que apontam consultas são mais eficiente para utilizar. No entanto, não existem nenhum índices diferente que no índice em cluster no **PartitionKey** e **RowKey**.

Muitas estruturas têm de cumprir os requisitos para ativar a pesquisa de entidades com base em vários critérios. Por exemplo, localizar entidades de empregado com base no correio eletrónico, id de empregado ou apelido. Os padrões descrito [padrões de conceção de tabela](table-storage-design-patterns.md) endereço estes tipos de requisito e descrevem formas de resolver o facto de que o serviço tabela fornecem índices secundários:  

* [Padrão de índice secundário intra partição](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -armazenar várias cópias de cada entidade utilizando diferentes **RowKey** valores (a mesma partição) para ativar rápido e eficiente pesquisas e as ordens de ordenação alternada utilizando diferentes **RowKey** valores.  
* [Padrão de índice secundário partição entre](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -armazenar várias cópias de cada entidade utilizando diferentes **RowKey** valores separar em partições ou separadas em tabelas para ativar as pesquisas de rápidos e eficientes e ordenação alternada as ordens utilizando diferentes **RowKey** valores.  
* [Padrão de entidades do índice](table-storage-design-patterns.md#index-entities-pattern) -manter entidades de índice para ativar pesquisas eficiente que devolvem apresenta uma lista de entidades.  

## <a name="sorting-data-in-the-table-service"></a>Ordenação de dados no serviço tabela
O serviço tabela devolve entidades ordenadas por ordem, com base no ascendente **PartitionKey** e, em seguida, **RowKey**. Estas chaves são valores de cadeia e para garantir que os valores numéricos ordenar corretamente, deverá convertê-los para um comprimento fixo e preenche-as com zeros. Por exemplo, se o valor de id de empregado utilizar como o **RowKey** é um valor de número inteiro, deve converter id de empregado **123** para **00000123**.  

Muitas aplicações têm requisitos para utilizar dados ordenados as ordens de diferentes: por exemplo, ordenação dos empregados pelo nome ou ao associar data. Como alternativa as ordens de ordenação para as entidades de endereços de padrões de seguintes:  

* [Padrão de índice secundário intra partição](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) - armazenar várias cópias de cada entidade utilizar valores diferentes de RowKey (na mesma partição) para ativar rápido e eficiente pesquisas e ordenação alternada ordena utilizando valores RowKey diferentes.  
* [Padrão de índice secundário partição entre](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - armazenar várias cópias de cada entidade utilizar valores diferentes de RowKey em separado partições em tabelas separadas para ativar rápido e eficiente pesquisas e ordenação alternada ordena utilizando valores RowKey diferentes .
* [Padrão de seguimento de registo](table-storage-design-patterns.md#log-tail-pattern) -obter o *n* entidades recentemente adicionadas a uma partição utilizando um **RowKey** valor ordena na data inversa e ordem de tempo.  

## <a name="next-steps"></a>Passos Seguintes

- [Padrões de conceção da tabela](table-storage-design-patterns.md)
- [Relações de modelação](table-storage-design-modeling.md)
- [Encriptar dados da tabela](table-storage-design-encrypt-data.md)
- [Estrutura de modificação de dados](table-storage-design-for-modification.md)
