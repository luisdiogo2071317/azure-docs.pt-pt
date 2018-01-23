---
title: O Apache Phoenix no HDInsight - o Azure HDInsight | Microsoft Docs
description: 
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: a82ddc4a94688df87043ef93f24956efb93220c4
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="apache-phoenix-in-hdinsight"></a>O Apache Phoenix no HDInsight

[O Apache Phoenix](http://phoenix.apache.org/) é um open source, a camada de base de dados relacional paralelo em grande escala incorporada no [HBase](hbase/apache-hbase-overview.md). O Phoenix permite-lhe utilizar consultas de como o SQL através de HBase. O Phoenix utiliza controladores JDBC por baixo para permitir aos utilizadores criar, eliminar, alter tabelas, índices, vistas e sequências e linhas de upsert SQL individualmente e em massa. O Phoenix utiliza compilação nativo noSQL, em vez de utilizar o MapReduce para compilar consultas, permitindo a criação de aplicações de latência baixa por cima do HBase. O Phoenix adiciona coprocessors para suportar a execução com o código fornecido pelo cliente no espaço de endereço do servidor, ao executar o código colocalizado com os dados. Esta abordagem minimiza a transferência de dados de cliente/servidor.

O Apache Phoenix abre-se as consultas de macrodados não programadores que podem utilizar uma sintaxe de como o SQL Server em vez de programação. O Phoenix está altamente otimizado para o HBase, ao contrário de outras ferramentas, tal como [Hive](hadoop/hdinsight-use-hive.md) e Spark SQL. O benefício aos programadores está a escrever altamente performant consultas com muito menos código.
<!-- [Spark SQL](spark/apache-spark-sql-with-hdinsight.md)  -->

Quando submete uma consulta SQL, o Phoenix compila a consulta para chamadas nativas do HBase e executa a análise (ou estiver a planear) em paralelo para otimização. Esta camada de abstração liberta o Programador de escrever as tarefas do MapReduce, para se focarem em vez disso, a lógica de negócio e o fluxo de trabalho da sua aplicação em torno do armazenamento de macrodados do Phoenix.

## <a name="query-performance-optimization-and-other-features"></a>Otimização do desempenho de consulta e outras funcionalidades

O Apache Phoenix adiciona várias funcionalidades e melhoramentos de desempenho para consultas de HBase.

### <a name="secondary-indexes"></a>Índices secundários

O HBase com um índice único que esteja ordenado lexicographically na chave primária de linha. Estes registos só podem ser acedidos através da chave de linha. Acesso aos registos através de qualquer coluna que não seja a chave de linha requer a análise de todos os dados ao aplicar o filtro necessário. Um índice secundário, colunas ou expressões que são indexada uma chave de linha alternativo, permitindo pesquisas e intervalo analisa em que o índice.

Criar um índice secundário com o `CREATE INDEX` comando:

```sql
CREATE INDEX ix_purchasetype on SALTEDWEBLOGS (purchasetype, transactiondate) INCLUDE (bookname, quantity);
```

Esta abordagem pode produzir um aumento de desempenho significativas através de execução de consultas incluída num índice único. Este tipo de índice secundário é um **que abrangem índice**, que contém todas as colunas incluídas na consulta. Por conseguinte, não é necessária a pesquisa da tabela e o índice satisfaça a consulta de toda.

### <a name="views"></a>Vistas

As vistas de Phoenix fornecer uma forma de ultrapassar uma limitação de HBase, onde o desempenho começa a diminuir quando criar mais do que cerca de 100 tabelas físicas. Vistas de Phoenix ativar vários *tabelas virtuais* para partilhar uma tabela de HBase física subjacente.

Como criar uma vista de Phoenix é semelhante da sintaxe de vista de SQL Server standard. Uma diferença é que pode definir colunas para a vista, além das colunas herdada a partir da respetiva tabela base. Também pode adicionar novos `KeyValue` colunas.

Por exemplo, aqui é uma tabela física `product_metrics` com a definição dos seguinte:

```sql
CREATE  TABLE product_metrics (
    metric_type CHAR(1),
    created_by VARCHAR, 
    created_date DATE, 
    metric_id INTEGER
    CONSTRAINT pk PRIMARY KEY (metric_type, created_by, created_date, metric_id));
```

Defina uma vista através desta tabela com colunas adicionais:

```sql
CREATE VIEW mobile_product_metrics (carrier VARCHAR, dropped_calls BIGINT) AS
SELECT * FROM product_metrics
WHERE metric_type = 'm';
```

Para adicionar mais colunas mais tarde, utilize o `ALTER VIEW` instrução.

### <a name="skip-scan"></a>Análise de ignorar

Análise de ignorar utiliza uma ou mais colunas de um índice composto para localizar os valores distintos. Ao contrário de uma análise de intervalo, a análise de ignorar implementa intra-linha análise, a gerar resultados [um desempenho melhorado](http://phoenix.apache.org/performance.html#Skip-Scan). Durante a análise, o primeiro valor de correspondência é ignorado, juntamente com o índice até é encontrado o valor seguinte.

Utiliza uma análise de ignorar a `SEEK_NEXT_USING_HINT` enumeração do filtro de HBase. Utilizar `SEEK_NEXT_USING_HINT`, a análise de ignorar mantém um registo dos qual o conjunto de chaves ou intervalos de chaves, estão a ser procurado para cada coluna. A ignorar a análise, em seguida, utiliza uma chave que foi transmitida ao mesmo durante a avaliação de filtro e determina se é uma das combinações. Caso contrário, a análise de ignorar avalia a seguinte chave mais elevada para ir para.

### <a name="transactions"></a>Transações

Enquanto o HBase fornece ao nível da linha de transações, Phoenix integra [Tephra](http://tephra.io/) para adicionar suporte para transações entre-linha e de tabela cruzada com completo [ACID](https://en.wikipedia.org/wiki/ACID) semântica.

Como com as transações do SQL Server tradicionais, transações fornecidas através do Gestor de transações Phoenix permitem-lhe garantir que atómica é uma unidade de dados com êxito upserted, a reverter a transação se a operação de upsert falhar em qualquer tabela ativada para transações.

Para ativar transações Phoenix, consulte o [documentação do Apache Phoenix transação](http://phoenix.apache.org/transactions.html).

Para criar uma nova tabela com transações ativadas, defina o `TRANSACTIONAL` propriedade `true` num `CREATE` instrução:

```sql
CREATE TABLE my_table (k BIGINT PRIMARY KEY, v VARCHAR) TRANSACTIONAL=true;
```

Para alterar uma tabela existente, ser transacional, utilize a mesma propriedade num `ALTER` instrução:

```sql
ALTER TABLE my_other_table SET TRANSACTIONAL=true;
```

> [!NOTE]
> Não é possível voltar a mudar uma tabela transacional para a ser não transacional.

### <a name="salted-tables"></a>Tabelas salted

*Hotspotting do servidor de região* pode ocorrer quando são escritos registos com chaves sequenciais HBase. Embora pode ter vários servidores de região no seu cluster, as escritas estão todos a ocorrer em apenas um. Este concentração cria o problema de hotspotting onde, em vez da carga de trabalho de escrita, que está a ser distribuída por todos os servidores de região disponível, apenas um está a processar a carga. Uma vez que cada região tem um tamanho máximo predefinido, quando uma região atinge esse limite de tamanho, é dividida em duas regiões pequenas. Quando isso acontece, uma das regiões novas demora todos os novos registos, tornar-se a hotspots de novo.

Para atenuar este problema e melhorar o desempenho, dividir previamente tabelas para que todos os servidores de região igualmente são utilizados. O Phoenix fornece *salted tabelas*, de forma transparente a adicionar o byte salting para a chave de linha de uma tabela específica. A tabela é previamente split os limites de salt bytes para garantir que a distribuição de carga igual entre os servidores de região durante a fase inicial da tabela. Esta abordagem distribui a carga de trabalho de escrita em todos os servidores disponíveis região, melhorando a operação de escrita e desempenho de leitura. Para salt uma tabela, especifique o `SALT_BUCKETS` propriedade de tabela quando é criada a tabela:

```sql
CREATE TABLE Saltedweblogs (
    transactionid varchar(500) Primary Key,
    transactiondate Date NULL,
    customerid varchar(50) NULL,
    bookid varchar(50) NULL,
    purchasetype varchar(50) NULL,
    orderid varchar(50) NULL,
    bookname varchar(50) NULL,
    categoryname varchar(50) NULL,
    invoicenumber varchar(50) NULL,
    invoicestatus varchar(50) NULL,
    city varchar(50) NULL,
    state varchar(50) NULL,
    paymentamount DOUBLE NULL,
    quantity INTEGER NULL,
    shippingamount DOUBLE NULL) SALT_BUCKETS=4;
```

## <a name="enable-and-tune-phoenix-with-ambari"></a>Ativar e otimizar Phoenix com o Ambari

Um cluster HBase do HDInsight inclui o [Ambari IU](hdinsight-hadoop-manage-ambari.md) para alterações de configuração.

1. Para ativar ou desativar o Phoenix e para controlar as definições de tempo limite de consulta do Phoenix, inicie sessão na IU da Web do Ambari (`https://YOUR_CLUSTER_NAME.azurehdinsight.net`) com as suas credenciais de utilizador do Hadoop.

2. Selecione **HBase** da lista de serviços no menu da esquerda, em seguida, selecione o **folhas** separador.

    ![Configuração do Ambari HBase](./media/hdinsight-phoenix-in-hdinsight/ambari-hbase-config.png)

3. Localizar o **Phoenix SQL** secção de configuração para ativar ou desativar o phoenix e definir o tempo limite de consulta.

    ![Secção de configuração do Ambari Phoenix SQL](./media/hdinsight-phoenix-in-hdinsight/ambari-phoenix.png)

## <a name="see-also"></a>Consulte também

* [Utilizar o Apache Phoenix com clusters de HBase baseado em Linux no HDInsight](hbase/apache-hbase-phoenix-squirrel-linux.md)
