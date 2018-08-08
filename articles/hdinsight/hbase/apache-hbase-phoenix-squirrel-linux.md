---
title: Utilizar Apache Phoenix e SQLLine com o HBase no HDInsight do Azure
description: Saiba como utilizar o Apache Phoenix no HDInsight. Além disso, saiba como instalar e configurar SQLLine no seu computador para ligar a um cluster do HBase no HDInsight.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/03/2018
ms.author: jasonh
ms.openlocfilehash: 349f1680cf754a44e2e4217ebde9d0d60479ebcf
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597468"
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Utilizar o Apache Phoenix com clusters de HBase baseado em Linux no HDInsight
Aprenda a usar [Apache Phoenix](http://phoenix.apache.org/) no Azure HDInsight e como utilizar SQLLine. Para obter mais informações sobre a Phoenix, consulte [Phoenix em 15 minutos ou menos](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Para a gramática de Phoenix, consulte [gramática Phoenix](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Para informações de versão de Phoenix sobre o HDInsight, consulte [quais são as novidades nas versões de cluster do Hadoop fornecidas pelo HDInsight](../hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Utilizar SQLLine
[SQLLine](http://sqlline.sourceforge.net/) é um utilitário de linha de comandos para executar o SQL.

### <a name="prerequisites"></a>Pré-requisitos
Antes de poder utilizar SQLLine, tem de ter os seguintes itens:

* **Um cluster do HBase no HDInsight**. Para criar um, veja [introdução ao Apache HBase no HDInsight](./apache-hbase-tutorial-get-started-linux.md).

Quando se liga a um cluster do HBase, tem de se conectar a uma das ZooKeeper VMs. Cada cluster do HDInsight tem três VMs ZooKeeper.

**Para obter o nome do anfitrião ZooKeeper**

1. Ao navegar para abrir o Ambari **https://\<nome do cluster\>. azurehdinsight.net**.
2. Para iniciar sessão, introduza o nome de utilizador HTTP (cluster) e a palavra-passe.
3. No menu da esquerda, selecione **ZooKeeper**. Três **servidor ZooKeeper** são apresentadas instâncias.
4. Selecione uma da **servidor ZooKeeper** instâncias. Sobre o **resumo** painel, encontrar o **nome do anfitrião**. Que é semelhante a *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Para utilizar SQLLine**

1. Ligar ao cluster utilizando SSH. Para obter mais informações, veja [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. No SSH, utilize os seguintes comandos para executar SQLLine:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Para criar uma tabela de HBase e inserir alguns dados, execute os seguintes comandos:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Para obter mais informações, consulte a [SQLLine manual](http://sqlline.sourceforge.net/#manual) e [gramática Phoenix](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a utilizar o Apache Phoenix no HDInsight. Para obter mais informações, veja estes artigos:

* [Descrição geral do HBase do HDInsight][hdinsight-hbase-overview].
  HBase é uma base de dados para os NoSQL de código-fonte aberto, Apache, que se incorporada no Hadoop que fornece acesso aleatório e consistência segura para grandes quantidades de dados não estruturados e semi-estruturados.
* [Aprovisionar clusters HBase numa rede Virtual do Azure][hdinsight-hbase-provision-vnet].
  Com a integração de rede virtual, HBase clusters podem ser implementados para a mesma rede virtual que seus aplicativos, para que os aplicativos podem comunicar diretamente com o HBase.
* [Configurar a replicação do HBase no HDInsight](apache-hbase-replication.md). Saiba como configurar a replicação do HBase em dois datacenters do Azure.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:apache-hbase-overview.md


