---
title: Dimensionar os tamanhos de cluster - Azure HDInsight | Microsoft Docs
description: Dimensione um cluster do HDInsight para a carga de trabalho.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: ashish
ms.openlocfilehash: 8b76d7d0441a5c1c25ad17b73083ec0e4feef1fe
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="scale-hdinsight-clusters"></a>Dimensionamento de clusters do HDInsight

HDInsight fornece elasticidade, dando-lhe a opção de aumentar verticalmente e reduzir verticalmente o número de nós de trabalho nos seus clusters. Isto permite-lhe reduzir um cluster depois de horas ou no fim de semana e expandi-lo durante o horário de pico exigências de negócio.

Por exemplo, se tiver algumas processamento em lote que ocorre uma vez por dia ou de uma vez por mês, o cluster do HDInsight pode ser escalado para cima alguns minutos antes que o evento agendado para que será memória suficiente e poder de computação de CPU. Pode automatizar o dimensionamento com o cmdlet do PowerShell [ `Set–AzureRmHDInsightClusterSize` ](hdinsight-administer-use-powershell.md#scale-clusters).  Mais tarde, após o processamento é concluído e utilização novamente a ficar inativo, pode reduzir verticalmente o cluster do HDInsight para menos nós de trabalho.

* Dimensionar o seu cluster através de [PowerShell](hdinsight-administer-use-powershell.md):

    ```powershell
    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    ```
    
* Dimensionar o seu cluster através de [CLI do Azure](hdinsight-administer-use-command-line.md):

    ```
    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
    ```
    
* Dimensionar o seu cluster através de [portal do Azure](https://portal.azure.com), abra o painel de cluster do HDInsight, selecione **cluster de escala** no menu da esquerda e, em seguida, no painel do cluster de escala, digite o número de nós de trabalho, e Selecione guardar.

    ![Dimensionar o cluster](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Utilizar qualquer um destes métodos, pode dimensionar o cluster do HDInsight ou reduzir verticalmente dentro de minutos.

## <a name="scaling-impacts-on-running-jobs"></a>Impactos de dimensionamento em tarefas em execução

Quando lhe **adicionar** nós ao cluster do HDInsight em execução, quaisquer tarefas pendentes ou em execução não serão afetadas. Além disso, as novas tarefas podem ser em segurança submetidas enquanto o processo de dimensionamento está em execução. Se as operações de dimensionamento falharem por algum motivo, a falha é corretamente processada, mantendo o cluster num estado funcional.

No entanto, se são dimensionamento para baixo do seu cluster por **remover** nós, quaisquer tarefas pendentes ou em execução falharão ao concluir a operação de dimensionamento. Esta falha é devido a alguns dos serviços reiniciar durante o processo.

Para resolver este problema, pode aguardar tarefas a concluir antes de dimensionamento para baixo do seu cluster, manualmente terminar as tarefas ou volte a submeter as tarefas depois de ter concluído a operação de dimensionamento.

Para ver uma lista de pendente e tarefas em execução, pode utilizar a IU do YARN ResourceManager, seguindo estes passos:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, selecione **procurar**, selecione **Clusters do HDInsight**e, em seguida, selecione o cluster.
3. A partir do painel de cluster do HDInsight, selecione **Dashboard** no menu superior para abrir a IU do Ambari. Introduza as credenciais de início de sessão do cluster.
4. Clique em **YARN** na lista de serviços no menu da esquerda. Na página YARN, selecione **ligações rápidas** e coloque o cursor sobre o nó principal do Active Directory, em seguida, clique em **ResourceManager IU**.

    ![IU ResourceManager](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

Pode aceder diretamente a IU ResourceManager com `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

Pode ver uma lista de tarefas, juntamente com o respetivo estado atual. Captura de ecrã, há uma tarefa atualmente em execução:

![Aplicações de IU ResourceManager](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Para eliminar manualmente essa aplicação em execução, execute o seguinte comando a partir da shell de SSH:

```bash
yarn application -kill <application_id>
```

Por exemplo:

```bash
yarn application -kill "application_1499348398273_0003"
```

## <a name="rebalancing-an-hbase-cluster"></a>Reequilíbrio um cluster HBase

Servidores de região são equilibrados automaticamente dentro de alguns minutos após a conclusão da operação de dimensionamento. Para balancear manualmente os servidores de região, utilize os seguintes passos:

1. Ligar ao cluster do HDInsight através de SSH. Para obter mais informações, veja [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Inicie a shell de HBase:

        hbase shell

3. Utilize o seguinte comando para equilibrar manualmente os servidores de região:

        balancer

## <a name="scale-down-implications"></a>Reduzir verticalmente implicações

Como mencionado anteriormente, as tarefas pendentes ou em execução estão terminadas após a conclusão de um dimensionamento para baixo a operação. No entanto, existem outras potenciais implicações para dimensionamento pendentes que podem ocorrer.

## <a name="hdinsight-name-node-stays-in-safe-mode-after-scaling-down"></a>Nó de nome de HDInsight permanece no modo de segurança depois de dimensionamento para baixo

![Dimensionar o cluster](./media/hdinsight-scaling-best-practices/scale-cluster.png)

Se diminuir o cluster para baixo até o mínimo de nó de um trabalho, conforme mostrado na imagem anterior, HDFS poderá ficar bloqueado no modo de segurança, quando nós de trabalho são reiniciados, devido a aplicação de patches ou imediatamente após a operação de dimensionamento.

A causa principal é que o ramo de registo utiliza algumas `scratchdir` ficheiros e, por predefinição espera três réplicas de cada bloco, mas não existe apenas uma réplica possíveis se reduzir verticalmente para o nó de um trabalho mínimo. Como um consequence, os ficheiros no `scratchdir` ficar *under-replicadas*. Isto pode fazer com que o HDFS para se manter no modo de segurança quando os serviços são reiniciados, depois da operação de dimensionamento.

Quando uma escala baixo tentativa acontece, o HDInsight depende as interfaces de gestão do Ambari para primeiro desativar os nós de trabalho extra indesejável, que replica os seus blocos HDFS para outros nós de trabalho online, e, em seguida, reduzir o cluster de forma segura verticalmente. HDFS entra no modo de segurança durante a janela de manutenção e deveria fique assim que o dimensionamento estiver concluído. Neste momento é que pode ficar bloqueado, HDFS no modo de segurança.

HDFS está configurado com um `dfs.replication` definição de 3. Assim, os blocos de ficheiros scratch são under-replicados sempre que existem menos de três nós de trabalho online, porque não as esperados três cópias de cada bloco de ficheiro está disponível.

Pode executar um comando para colocar o HDFS fora do modo de segurança. Por exemplo, se souber que o motivo apenas o modo de segurança é porque os ficheiros temporários são under-replicados, pode deixar em segurança modo de segurança. Isto acontece porque os ficheiros under-replicados são ficheiros temporários scratch de ramo de registo.

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Depois de abandonar o fileparser modo de segurança, pode manualmente remover os ficheiros temporários, ou aguarde pela ramo de registo para, eventualmente, limpá-los automaticamente.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Erros de exemplo quando o modo de segurança está ativado

* H070 não é possível abrir a sessão de ramo de registo. org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **não é possível criar o diretório**  /tmp/ramo de registo do ramo de registo / 8 819c215c - 6d 87-4311-97c-4f0b9d2adcf0. **Nó de nome está em modo seguro**. Os blocos comunicados 75 tem 12 blocos adicionais para atingir o limiar 0.9900 de blocos totais 87. O número de datanodes em direto 10 atingiu o número mínimo 0. Modo de segurança será desligado automaticamente quando os limiares de tem sido atingidos.

* Bases de dados de mostrar H100 não é possível submeter a instrução: org.apache.thrift.transport.TTransportException: org.apache.http.conn.HttpHostConnectException: ligar ao hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername . Falha de Internal.cloudapp.NET/1.1.1.1]: **ligação foi recusada**

* Não foi possível H020 estabelecer ligação com hn0 hdisrv.servername.bx.internal.cloudapp .net: 10001: org.apache.thrift.transport.TTransportException: não foi possível criar a ligação de http para http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: ligar ao hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] falhou: ligação foi recusada: org.apache.thrift.transport.TTransportException: não foi possível criar a ligação de http para http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: ligar ao hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] falhou: **ligação foi recusada**

* Partir de registos do ramo de registo: avisar [principal]: servidor. HiveServer2 (HiveServer2.java:startHiveServer2(442)) – erro ao iniciar HiveServer2 21, de tentar novamente em 60 segundos java.lang.RuntimeException: erro ao aplicar a política de autorização na configuração do ramo de registo: org.apache.hadoop.ipc.RemoteException ( org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **não é possível criar o diretório** /tmp/hive/hive/70a42b8a-9437-466e-acbe-da90b1614374. **Nó de nome está em modo seguro**.
    Os blocos comunicados 0 tem 9 blocos adicionais para atingir o limiar 0.9900 de blocos totais 9.
    O número de datanodes em direto 10 atingiu o número mínimo 0. **Modo de segurança será desligado automaticamente quando os limiares de tem sido atingidos**.
    at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1324)

Pode rever os registos de nó de nome do `/var/log/hadoop/hdfs/` pasta, junto a hora quando o cluster foi ampliado, para ver quando entrou modo de segurança. Os ficheiros de registo são denominados `Hadoop-hdfs-namenode-hn0-clustername.*`.

A causa raiz dos erros anteriores é que Hive depende ficheiros temporários no HDFS durante a execução de consultas. Quando o HDFS introduz o modo de segurança, o ramo de registo não é possível executar consultas porque este não é possível escrever HDFS. Os ficheiros temporários no HDFS estão localizados na unidade local instalado no nó de trabalho individuais VMs e replicados para outros nós de trabalho em três réplicas, mínimo.

O `hive.exec.scratchdir` parâmetro no ramo de registo está configurado no `/etc/hive/conf/hive-site.xml`:

```xml
<property>
    <name>hive.exec.scratchdir</name>
    <value>hdfs://mycluster/tmp/hive</value>
</property>
```

### <a name="view-the-health-and-state-of-your-hdfs-file-system"></a>Ver o estado de funcionamento e o estado do seu sistema de ficheiros do HDFS

Pode ver um relatório de estado de cada nó de nome se nós estão no modo de segurança. Para ver o relatório, o SSH para cada nó principal e execute o seguinte comando:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode get
```

![Modo de segurança desativado](./media/hdinsight-scaling-best-practices/safe-mode-off.png)

> [!NOTE]
> O `-D` comutador é necessário porque o sistema de ficheiros no HDInsight é o Storage do Azure ou do Azure Data Lake Store. `-D` Especifica que os comandos forem executados no sistema de ficheiros local do HDFS.

Em seguida, pode ver um relatório que mostra os detalhes do Estado HDFS:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -report
```

Este comando resulta no seguinte num cluster de bom estado de funcionamento em que todos os blocos são replicados para o grau de esperado:

![Modo de segurança desativado](./media/hdinsight-scaling-best-practices/report.png)

HDFS suporta o `fsck` bloqueia o comando para verificar a existência inconsistências com vários ficheiros, como em falta para um ficheiro ou blocos under-replicados. Para executar o `fsck` comando contra o `scratchdir` ficheiros (disco scratch temporário):

```
hdfs fsck -D 'fs.default.name=hdfs://mycluster/' /tmp/hive/hive
```

Quando executado num sistema de ficheiros HDFS bom estado de funcionamento com nenhuma blocos under-replicados, poderá ver um resultado semelhante ao seguinte:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:07:01 UTC 2017
..Status: HEALTHY
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
 Minimally replicated blocks:   2 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          4
 Number of racks:               1
FSCK ended at Thu Jul 06 20:07:01 UTC 2017 in 3 milliseconds


The filesystem under path '/tmp/hive/hive' is HEALTHY
```

Em contrapartida, quando o `fsck` comando é executado num sistema de ficheiros HDFS com alguns blocos under-replicados, o resultado será semelhante ao seguinte:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:13:58 UTC 2017
.
/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info:  Under replicated BP-1867508080-10.0.0.21-1499348422953:blk_1073741826_1002. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s) and 0 decommissioning replica(s).
.
/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: CORRUPT blockpool BP-1867508080-10.0.0.21-1499348422953 block blk_1073741825

/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: MISSING 1 blocks of total size 26 B.Status: CORRUPT
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
  ********************************
  UNDER MIN REPL'D BLOCKS:      1 (50.0 %)
  dfs.namenode.replication.min: 1
  CORRUPT FILES:        1
  MISSING BLOCKS:       1
  MISSING SIZE:         26 B
  CORRUPT BLOCKS:       1
  ********************************
 Minimally replicated blocks:   1 (50.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       1 (50.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     0.5
 Corrupt blocks:                1
 Missing replicas:              2 (33.333332 %)
 Number of data-nodes:          1
 Number of racks:               1
FSCK ended at Thu Jul 06 20:13:58 UTC 2017 in 28 milliseconds


The filesystem under path '/tmp/hive/hive' is CORRUPT
```

Também pode ver o estado do HDFS na IU do Ambari, selecionando o **HDFS** serviço à esquerda ou com `https://<HDInsightClusterName>.azurehdinsight.net/#/main/services/HDFS/summary`.

![Estado do HDFS do Ambari](./media/hdinsight-scaling-best-practices/ambari-hdfs.png)

Também poderá ver um ou mais erros críticos no NameNodes Active Directory ou em modo de espera. Para ver o estado de funcionamento de blocos NameNode, selecione a ligação de NameNode junto ao alerta do.

![O estado de funcionamento do NameNode blocos](./media/hdinsight-scaling-best-practices/ambari-hdfs-crit.png)

Para limpar os ficheiros de scratch, que remove os erros de replicação de blocos, SSH para cada nó principal e execute o seguinte comando:

```
hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
```

> [!NOTE]
> Este comando pode causar uma falha do Hive se algumas tarefas ainda estão em execução.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode-due-to-under-replicated-blocks"></a>Como impedir a acumular-se no modo de segurança devido a blocos under-replicados HDInsight

Existem várias formas para impedir que o HDInsight a ser deixada no modo de segurança:

* Pare todas as tarefas do Hive antes de dimensionamento para baixo do HDInsight. Em alternativa, agende a escala para baixo de processo para evitar em conflito com a execução de tarefas do Hive.
* Limpar manualmente zero do Hive `tmp` ficheiros de diretório no HDFS antes de dimensionamento para baixo.
* Apenas reduzir verticalmente HDInsight para três nós de trabalho, mínimo. Evite vai como tão baixo como nó de um trabalho.
* Execute o comando para sair do modo de segurança, se necessário.

As secções seguintes descrevem estas opções.

#### <a name="stop-all-hive-jobs"></a>Parar todas as tarefas do Hive

Pare todas as tarefas do Hive antes de dimensionamento para baixo para o nó de um trabalho. Se estiver agendada a carga de trabalho, em seguida, execute o seu dimensionamento pendente depois de terminar o trabalho de ramo de registo.

Isto ajuda a minimizar o número de scratch ficheiros na pasta tmp (se aplicável).

#### <a name="manually-clean-up-hives-scratch-files"></a>Limpar manualmente os ficheiros de scratch do ramo de registo

Se o ramo de registo deixou atrás ficheiros temporários, em seguida, pode limpar manualmente os ficheiros antes de dimensionamento para baixo para evitar o modo de segurança.

1. Parar serviços do ramo de registo e lembre-se de que todas as consultas e as tarefas estão concluídas.

2. Listar o conteúdo a `hdfs://mycluster/tmp/hive/` diretório para ver se contém todos os ficheiros:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```
    
    Eis uma saída de exemplo quando existem ficheiros:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Se souber o que ramo de registo é feito com estes ficheiros, pode removê-los. Lembre-se de que o ramo de registo não tem quaisquer consultas em execução ao procurar na página IU do Yarn ResourceManager.

    Linha de comandos de exemplo para remover ficheiros de HDFS:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```
    
#### <a name="scale--hdinsight-to-three-worker-nodes"></a>HDInsight de dimensionamento para três nós de trabalho

Se a acumular no modo de segurança é um problema persistente e os passos anteriores não estiverem disponíveis, em seguida, pode pretender evitar o problema por apenas dimensionamento para baixo até três nós de trabalho. Isto poderá não ser o ideal, devido a restrições de custo, em comparação comparadas dimensionamento para baixo para um nó. No entanto, com apenas um nó de trabalho, HDFS não garante a três réplicas dos dados estão disponíveis para o cluster.

#### <a name="run-the-command-to-leave-safe-mode"></a>Execute o comando para sair do modo de segurança

A última opção é a observar a occasion raro na qual HDFS introduz o modo de segurança, em seguida, execute o comando de modo de segurança deixe. Depois de ter determinado que a razão HDFS entrou no modo de segurança devido aos ficheiros de ramo de registo que está a ser under-replicada, execute o seguinte comando para deixar o modo de segurança:

* HDInsight no Linux:

    ```bash
    hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
    ```
    
* HDInsight no Windows:

    ```bash
    hdfs dfsadmin -fs hdfs://headnodehost:9000 -safemode leave
    ```
    
## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure HDInsight](hadoop/apache-hadoop-introduction.md)
* [Dimensionamento de clusters](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Gerir clusters do HDInsight utilizando a IU da Web do Ambari](hdinsight-hadoop-manage-ambari.md)
