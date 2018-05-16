---
title: Configurar a cópia de segurança de HBase e Phoenix e replicação - Azure HDInsight | Microsoft Docs
description: Configure a cópia de segurança e de replicação de HBase e Phoenix.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: f43edaf16784e5ba5cc3d2b39df285582954210b
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="set-up-backup-and-replication-for-hbase-and-phoenix-on-hdinsight"></a>Configurar a cópia de segurança e de replicação de HBase e Phoenix no HDInsight

HBase suporta várias abordagens para guarding contra a perda de dados:

* Copiar o `hbase` pasta
* Exportar, em seguida, importar
* Tabelas de cópia
* Instantâneos
* Replicação

> [!NOTE]
> O Apache Phoenix armazena os metadados nas tabelas de HBase, para que os metadados de cópia de segurança quando a cópia de segurança as tabelas do catálogo de sistema HBase.

As secções seguintes descrevem o cenário de utilização para cada uma destas abordagens.

## <a name="copy-the-hbase-folder"></a>Copie a pasta de hbase

Com esta abordagem, copie todos os dados de HBase, sem a capacidade para selecionar um subconjunto de tabelas ou famílias de coluna. Abordagens subsequentes fornecem um maior controlo.

O HBase no HDInsight utiliza o armazenamento de predefinido selecionado quando criar o cluster, os blobs de armazenamento do Azure ou do Azure Data Lake Store. Em ambos os casos, o HBase armazena os ficheiros de dados e metadados no caminho seguinte:

    /hbase

* Uma conta de armazenamento do Azure a `hbase` pasta reside na raiz do contentor do blob:

    ```
    wasbs://<containername>@<accountname>.blob.core.windows.net/hbase
    ```

* No Azure Data Lake Store o `hbase` pasta reside no caminho da raiz que especificou quando aprovisionar um cluster. Este caminho de raiz, normalmente, tem um `clusters` pasta, com uma subpasta denominada após o cluster do HDInsight:

    ```
    /clusters/<clusterName>/hbase
    ```

Em ambos os casos, o `hbase` pasta contém todos os dados que HBase foi removida da cache no disco, mas não pode conter os dados em memória. Também pode utilizar esta pasta como uma representação exata dos dados HBase, tem de encerrar o cluster.

Depois de eliminar o cluster, pode deixar os dados no local, ou copiar os dados para uma nova localização:

* Crie uma nova instância de HDInsight apontar para a localização de armazenamento atual. É criada a nova instância com todos os dados existentes.

* Copiar o `hbase` pasta para um outro armazenamento de Azure blob contentor ou localização de Data Lake Store e, em seguida, inicie um novo cluster com os dados. Para o Storage do Azure, utilize [AzCopy](../../storage/common/storage-use-azcopy.md)e para utilização do Data Lake Store [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md).

## <a name="export-then-import"></a>Exportar, em seguida, importar

No cluster de HDInsight de origem, utilize o utilitário de exportação (incluído no HBase) para exportar dados de uma tabela de origem para o armazenamento de predefinido ligado. Pode, em seguida, copie a pasta exportada para a localização de armazenamento de destino e execute o utilitário de importação no cluster do HDInsight de destino.

Para exportar uma tabela, o SSH primeiro no nó principal do cluster do HDInsight de origem e, em seguida, execute o seguinte `hbase` comando:

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>"

Para importar uma tabela, o SSH para o nó principal do cluster do HDInsight de destino e, em seguida, execute o seguinte `hbase` comando:

    hbase org.apache.hadoop.hbase.mapreduce.Import "<tableName>" "/<path>/<to>/<export>"

Especifique o caminho completo de exportação para o armazenamento de predefinido ou para qualquer uma das opções de armazenamento ligado. Por exemplo, no armazenamento do Azure:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

No Azure Data Lake Store, a sintaxe é:

    adl://<accountName>.azuredatalakestore.net:443/<path>

Esta abordagem oferece granularidade de nível de tabela. Também pode especificar um intervalo de datas para as linhas incluir, que lhe permite executar o processo de forma incremental. Cada data é em milissegundos, desde a época de Unix.

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>" <numberOfVersions> <startTimeInMS> <endTimeInMS>

Tenha em atenção que tem de especificar o número de versões de cada linha para exportar. Para incluir todas as versões no intervalo de datas, defina `<numberOfVersions>` para um valor maior do que as versões possíveis de linha máximo, como 100000.

## <a name="copy-tables"></a>Tabelas de cópia

O utilitário de CopyTable copia dados a partir de uma tabela de origem, linha por linha, para uma tabela de destino existente com o mesmo esquema como origem. A tabela de destino pode estar no mesmo cluster ou outro cluster HBase.

Para utilizar CopyTable num cluster, SSH no nó principal do cluster do HDInsight de origem e, em seguida, executar esta `hbase` comando:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> <srcTableName>

Para utilizar CopyTable para copiar para uma tabela num cluster diferente, adicione o `peer` mudar com o endereço do cluster de destino:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> --peer.adr=<destinationAddress> <srcTableName>

O endereço de destino é constituído pelas três partes seguintes:

    <destinationAddress> = <ZooKeeperQuorum>:<Port>:<ZnodeParent>

* `<ZooKeeperQuorum>` é uma lista separada por vírgulas de nós de ZooKeeper, por exemplo:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net

* `<Port>` nas predefinições do HDInsight para 2181, e `<ZnodeParent>` é `/hbase-unsecure`, por isso, todo `<destinationAddress>` seria:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net:2181:/hbase-unsecure

Consulte [recolher manualmente a lista de quórum ZooKeeper](#manually-collect-the-zookeeper-quorum-list) neste artigo para obter detalhes sobre como obter estes valores para o seu cluster HDInsight.

O utilitário de CopyTable também suporta os parâmetros para especificar o intervalo de tempo de linhas para copiar e para especificar o subconjunto de famílias de coluna numa tabela para copiar. Para ver a lista completa de parâmetros suportados pelo CopyTable, execute CopyTable sem quaisquer parâmetros:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable

CopyTable analisa o conteúdo da tabela de origem completo que será copiado para a tabela de destino. Isto pode reduzir o desempenho do seu cluster HBase enquanto executa CopyTable.

> [!NOTE]
> Para automatizar a cópia dos dados entre as tabelas, consulte o `hdi_copy_table.sh` script no [Azure HBase Utils](https://github.com/Azure/hbase-utils/tree/master/replication) repositório no GitHub.

### <a name="manually-collect-the-zookeeper-quorum-list"></a>Recolher manualmente o quórum de ZooKeeper lista

Quando ambos os clusters do HDInsight estão na mesma rede virtual, conforme descrito anteriormente, resolução de nome de anfitrião interno é automática. Para utilizar CopyTable para clusters do HDInsight em duas redes virtuais separados ligados por um Gateway de VPN, terá de fornecer o anfitrião de endereços IP de nós de Zookeeper no quórum.

Para obter os nomes de anfitrião do quórum, execute o seguinte comando curl:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/configurations?type=hbase-site&tag=TOPOLOGY_RESOLVED" | grep "hbase.zookeeper.quorum"

O comando curl obtém um documento JSON com as informações de configuração de HBase e o comando de grep devolve apenas a entrada de "hbase.zookeeper.quorum", por exemplo:

    "hbase.zookeeper.quorum" : "zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net"

O valor de nomes de anfitrião do quórum é a cadeia completa para a direita dos dois pontos.

Para obter os endereços IP para que estes anfitriões, utilize o seguinte comando curl para cada anfitrião na lista anterior:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/hosts/<zookeeperHostFullName>" | grep "ip"

Neste comando curl, `<zookeeperHostFullName>` é o nome DNS completo de um anfitrião de ZooKeeper, tais como o exemplo `zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net`. A saída do comando contém o endereço IP para o anfitrião especificado, por exemplo:

    100    "ip" : "10.0.0.9",

Depois de recolher os endereços IP para todos os nós de ZooKeeper no seu quórum, reconstrua o endereço de destino:

    <destinationAddress>  = <Host_1_IP>,<Host_2_IP>,<Host_3_IP>:<Port>:<ZnodeParent>

No nosso exemplo:

    <destinationAddress> = 10.0.0.9,10.0.0.8,10.0.0.12:2181:/hbase-unsecure

## <a name="snapshots"></a>Instantâneos

Instantâneos permitem-lhe criar um ponto no tempo cópia de segurança de dados no seu arquivo de dados do HBase. Instantâneos têm overhead mínimo e concluir em segundos, porque uma operação de instantâneo eficazmente é uma operação de metadados capturar os nomes de todos os ficheiros de armazenamento em que instantâneas. No momento de um instantâneo, não existem dados reais são copiados. Instantâneos dependem da natureza imutável dos dados armazenados no HDFS, onde as atualizações, eliminações e inserções são todos representadas como novos dados. Pode restaurar (*clone*) um instantâneo no mesmo cluster ou exportar um instantâneo para outro cluster.

Para criar um instantâneo, o SSH ao nó principal do HBase do HDInsight cluster e iniciar o `hbase` shell:

    hbase shell

Na shell do hbase, utilize o comando de instantâneo com os nomes da tabela e deste instantâneo:

    snapshot '<tableName>', '<snapshotName>'

Para restaurar um instantâneo de por nome dentro do `hbase` shell, primeiro desativar a tabela, em seguida, restaurar o instantâneo e volte a ativar a tabela:

    disable '<tableName>'
    restore_snapshot '<snapshotName>'
    enable '<tableName>'

Para restaurar um instantâneo para uma nova tabela, utilize clone_snapshot:

    clone_snapshot '<snapshotName>', '<newTableName>'

Para exportar um instantâneo para o HDFS para utilização por outro cluster, crie primeiro o instantâneo, conforme descrito anteriormente e, em seguida, utilize o utilitário de ExportSnapshot. Executar este utilitário a partir de dentro da sessão SSH ao nó principal, não dentro de `hbase` shell:

     hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot <snapshotName> -copy-to <hdfsHBaseLocation>

O `<hdfsHBaseLocation>` pode ser a qualquer uma das localizações de armazenamento acessível para o cluster de origem e deve apontar para a pasta de hbase utilizada pelo seu cluster de destino. Por exemplo, se tiver uma conta de armazenamento do Azure secundária ligada ao seu cluster de origem e essa conta fornece acesso ao contentor utilizado pelo armazenamento de predefinido do cluster de destino, pode utilizar este comando:

    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot 'Snapshot1' -copy-to 'wasbs://secondcluster@myaccount.blob.core.windows.net/hbase'

Depois do instantâneo é exportado, SSH para o nó principal do cluster de destino e restaurar o instantâneo utilizando os restore_snapshot comando conforme descrito anteriormente.

Os instantâneos proporcionar uma cópia de segurança completa de uma tabela no momento do `snapshot` comando. Instantâneos não fornecem a capacidade para efetuar instantâneos incrementais pelo windows de tempo, nem para especificar subconjuntos de famílias de colunas para incluir no instantâneo.

## <a name="replication"></a>Replicação

Replicação do HBase pushes automaticamente transações de um cluster de origem para um cluster de destino, utilizando um mecanismo de assíncrono com overhead mínimo no cluster de origem. No HDInsight, pode configurar a replicação entre clusters onde:

* Os clusters de origem e destino estão na mesma rede virtual.
* Os clusters de origem e de destinos estão em diferentes redes virtuais ligadas por um gateway de VPN, mas ambos os clusters existem na mesma localização geográfica.
* O cluster de origem e clusters de destinos estão em diferentes redes virtuais ligadas por um gateway de VPN e cada cluster existe numa localização geográfica diferente.

Os passos gerais para configurar a replicação são:

1. No cluster de origem, crie as tabelas e preencher os dados.
2. No cluster de destino, crie tabelas de destino vazio com o esquema da tabela de origem.
3. Registe o cluster de destino como um elemento de rede para o cluster de origem.
4. Ative a replicação as tabelas de origem pretendido.
5. Copie dados existentes das tabelas de origem para as tabelas de destino.
6. Replicação copia automaticamente o novo modificações de dados para as tabelas de origem para as tabelas de destino.

Para ativar a replicação no HDInsight, aplica uma ação de Script ao cluster do HDInsight de origem em execução. Para obter instruções de ativação da replicação no seu cluster ou para testar replicação em clusters de exemplo criado em redes virtuais através de modelos de gestão de recursos do Azure, consulte [replicação do HBase configurar](apache-hbase-replication.md). Este artigo também inclui instruções para ativar a replicação dos metadados Phoenix.

## <a name="next-steps"></a>Passos Seguintes

* [Configurar replicação de HBase](apache-hbase-replication.md)
