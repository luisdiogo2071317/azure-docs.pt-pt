---
title: Utilizar arquivos de metadados externos - Azure HDInsight
description: Utilize arquivos de metadados externos com clusters do HDInsight.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: fb1401578237a92a6f164ec98e8dbcdbbb88be38
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595403"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Utilizar arquivos de metadados externos no Azure HDInsight

O metastore Hive no HDInsight é uma parte essencial da arquitetura de Hadoop. Um metastore é o repositório do esquema central que pode ser utilizado por outras ferramentas de acesso de grandes volumes de dados, como Spark, consulta interativa (LLAP), Presto ou Pig. HDInsight utiliza uma base de dados do SQL do Azure como o metastore Hive.

![Arquitetura de Store de metadados do Hive do HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Existem duas formas, que pode configurar um metastore para os seus clusters do HDInsight:

* [Metastore predefinida](#default-metastore)
* [Metastore personalizado](#custom-metastore)

## <a name="default-metastore"></a>Metastore predefinida

Por predefinição, o HDInsight aprovisiona um metastore com todos os tipos de cluster. Em vez disso, pode especificar um metastore personalizado. O metastore predefinida inclui as seguintes considerações:
- Sem custos adicionais. HDInsight aprovisiona um metastore com todos os tipos de cluster sem qualquer custo adicional para.
- Cada metastore predefinida faz parte do ciclo de vida do cluster. Ao eliminar um cluster que metastore e metadados também são eliminados.
- Não é possível partilhar o metastore predefinida com outros clusters.
- O metastore predefinida utiliza o básico BD SQL do Azure, que tem um limite relativamente 5 do DTU (unidade de transação de base de dados).
Este metastore predefinida é normalmente utilizado para cargas de trabalho relativamente simples, que não necessitam de vários clusters e não precisam de metadados preservados para além do ciclo de vida do cluster.


## <a name="custom-metastore"></a>Metastore personalizado

HDInsight também suporta metastores personalizada, que são recomendados para clusters de produção:
- Especificar sua própria base de dados do SQL do Azure como o metastore.
- O ciclo de vida do metastore não está associado a um ciclo de vida de clusters, para que possa criar e eliminar os clusters sem perder os metadados. Metadados, como seus esquemas de Hive persistirá mesmo depois de eliminar e voltar a criar o cluster do HDInsight.
- Um metastore personalizado permite-lhe ligar vários clusters e os tipos de cluster para esse metastore. Por exemplo, um metastore único pode ser compartilhado em clusters do Interactive Query, o Hive e o Spark no HDInsight.
- Paga pelo custo de um metastore (Azure SQL DB), de acordo com o nível de desempenho que escolher.
- Pode aumentar verticalmente o metastore conforme necessário.


![Caso de utilização do HDInsight Hive metadados Store](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

<!-- Image – Typical shared custom Metastore scenario in HDInsight (?) -->



### <a name="select-a-custom-metastore-during-cluster-creation"></a>Selecionar um metastore personalizado durante a criação do cluster

Pode apontar o seu cluster para uma base de dados de SQL do Azure criado anteriormente durante a criação do cluster, ou pode configurar a base de dados SQL após a criação do cluster. Esta opção é especificada com o armazenamento > definições de Metastore ao criar um novo Hadoop, Spark ou interativas do Hive cluster a partir do portal do Azure.

![Portal do Azure de Store de metadados do Hive do HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

Também pode adicionar clusters adicionais a um metastore personalizado no portal do Azure ou das configurações de Ambari (Hive > Avançadas)

![Ambari Store de metadados do Hive do HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Melhores práticas de metastore de Hive

Aqui estão alguns geral Hive do HDInsight metastore melhores práticas:

- Utilize um metastore personalizado sempre que possível, pois isso o ajudará a recursos de computação separada (seu cluster em execução) e metadados (armazenado no metastore).
- Comece com um escalão S2, que fornece 50 DTUS e 250 GB de armazenamento. Se vir um estrangulamento, pode dimensionar a base de dados.
- Certifique-se de que o metastore criada para uma versão de cluster do HDInsight não é compartilhado entre diferentes versões de cluster do HDInsight. Diferentes versões de Hive usam esquemas diferentes. Por exemplo, é possível partilhar um metastore com clusters do Hive 1.2 e 2.1 do Hive.
- Criar cópias de segurança seu metastore personalizado periodicamente.
- Manter os seus metastore e o cluster do HDInsight na mesma região.
- Monitorize o metastore para desempenho e disponibilidade usando ferramentas de monitorização de base de dados SQL do Azure, como o portal do Azure ou o Log Analytics do Azure.

## <a name="oozie-metastore"></a>Oozie Metastore

Apache Oozie é um sistema de coordenação do fluxo de trabalho que gere as tarefas do Hadoop.  Oozie suporta tarefas do Hadoop para Apache MapReduce, Pig, Hive e outras pessoas.  Oozie utiliza um metastore para armazenar detalhes sobre os fluxos de trabalho atuais e concluídos. Para aumentar o desempenho ao utilizar o Oozie, pode utilizar a base de dados do Azure SQL como um metastore personalizado. O metastore também pode fornecer acesso aos dados da tarefa de Oozie depois de eliminar o cluster.

Para obter instruções sobre como criar um metastore Oozie com o Azure SQL Database, consulte [utilizar Oozie para fluxos de trabalho](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Passos Seguintes

- [Configurar clusters no HDInsight com o Hadoop, Spark, Kafka e muito mais](./hdinsight-hadoop-provision-linux-clusters.md)
