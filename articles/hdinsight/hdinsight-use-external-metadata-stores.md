---
title: Utilizar arquivos de metadados externo - Azure HDInsight | Microsoft Docs
description: Utilize arquivos de metadados externo com clusters do HDInsight.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
tags: azure-portal
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jgao
ms.openlocfilehash: 2eadee1a8695450e2219031ea1a65ee3624f26b5
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Utilizar arquivos de metadados externo no Azure HDInsight

O metastore do Hive no HDInsight é uma parte essencial da arquitetura do Hadoop. Um metastore é o repositório de esquema central que pode ser utilizado por outras ferramentas de acesso de macrodados, tais como o Spark, consulta interativa (LLAP), Presto ou Pig. O HDInsight utiliza uma base de dados do SQL do Azure como o metastore do Hive.

![Arquitetura de arquivo de metadados de ramo de registo do HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Existem duas formas que pode configurar um metastore para os clusters do HDInsight:

* [Predefinição metastore](#default-metastore)
* [Metastore personalizado](#custom-metastore)

## <a name="default-metastore"></a>Predefinição metastore

Por predefinição, o HDInsight aprovisiona um metastore com cada tipo de cluster. Em vez disso, pode especificar um metastore personalizado. A predefinição metastore inclui as seguintes considerações:
- Sem custos adicionais. HDInsight aprovisiona um metastore com cada tipo de cluster sem qualquer custo adicional para si.
- Cada metastore predefinido faz parte do ciclo de vida do cluster. Quando eliminar um cluster que metastore e metadados também são eliminados.
- Não é possível partilhar o metastore predefinido com outros clusters.
- O metastore predefinido utiliza a BD do SQL do Azure básica, que tem um limite de (unidade de transação de base de dados) de DTU 5.
Este metastore predefinido é normalmente utilizada para cargas de trabalho é relativamente simples que não necessitam de vários clusters e não precisam de metadados preservados para além do ciclo de vida do cluster.


## <a name="custom-metastore"></a>Metastore personalizado

HDInsight também suporta metastores personalizadas, que são recomendadas para clusters de produção:
- Especifique os seus próprios SQL Database do Azure como o metastore.
- O ciclo de vida do metastore não está associado a um ciclo de vida de clusters, para que possa criar e eliminar clusters sem perda de metadados. Metadados, tais como os esquemas do Hive serão mantidas, mesmo depois de eliminar e voltar a criar o cluster do HDInsight.
- Um metastore personalizada permite-lhe ligar vários clusters e tipos de cluster para esse metastore. Por exemplo, um metastore única pode ser partilhado em clusters de consulta interativa, o ramo de registo e o Spark no HDInsight.
- Pagar o custo de um metastore (BD SQL do Azure), de acordo com o nível de desempenho que escolher.
- Pode dimensionar o metastore conforme necessário.


![Caso de utilização do armazenamento de metadados de ramo de registo do HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

<!-- Image – Typical shared custom Metastore scenario in HDInsight (?) -->



### <a name="select-a-custom-metastore-during-cluster-creation"></a>Selecione um metastore personalizado durante a criação do cluster

Pode apontar o cluster para uma base de dados de SQL do Azure criado anteriormente durante a criação do cluster, ou pode configurar a base de dados do SQL Server após a criação do cluster. Esta opção for especificada com o armazenamento > definições do Metastore ao criar um novo, Spark ou do Hadoop Hive interativo de cluster do portal do Azure.

![Portal do Azure de arquivo de metadados do Hive do HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

Também pode adicionar clusters adicionais um metastore personalizado do portal do Azure ou das configurações do Ambari (ramo de registo > Avançadas)

![Arquivo de metadados de ramo de registo do HDInsight Ambari](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Melhores práticas do metastore do ramo de registo

Seguem-se algumas geral ramo de registo do HDInsight metastore melhores práticas:

- Utilize um metastore personalizada sempre que possível, isto irá ajudar a recursos de computação separados (o cluster em execução) e os metadados (armazenado no metastore).
- Começar com uma camada de S2, que fornece 50 DTU e 250 GB de armazenamento. Se vir um estrangulamento, pode dimensionar a base de dados.
- Certifique-se de que o metastore criado para uma versão de cluster do HDInsight não é partilhado entre diferentes versões de cluster do HDInsight. Versões diferentes do ramo de registo utilizam esquemas diferentes. Por exemplo, não é possível partilhar um metastore com clusters do Hive 1.2 e 2.1 ramo de registo.
- Fazer cópias de segurança sua metastore personalizado periodicamente.
- Manter as suas metastore cluster do HDInsight na mesma região.
- Monitorize o metastore para o desempenho e disponibilidade utilizando ferramentas de monitorização de base de dados SQL do Azure, tal como o portal do Azure ou Log Analytics do Azure.

## <a name="oozie-metastore"></a>Metastore do Oozie

O Apache Oozie é um sistema de coordenação de fluxo de trabalho que gere as tarefas do Hadoop.  Oozie suporta tarefas do Hadoop para Apache MapReduce, Pig, Hive e outras pessoas.  Oozie utiliza um metastore para armazenar os detalhes sobre fluxos de trabalho atuais e concluídos. Para aumentar o desempenho ao utilizar o Oozie, pode utilizar a SQL Database do Azure como um metastore personalizado. O metastore também pode fornecer acesso aos dados da tarefa Oozie depois de eliminar o cluster.

Para obter instruções sobre como criar um metastore do Oozie com a SQL Database do Azure, consulte [Oozie de utilização para fluxos de trabalho](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Passos Seguintes

- [Configurar clusters no HDInsight com o Hadoop, Spark, Kafka e muito mais](./hdinsight-hadoop-provision-linux-clusters.md)
