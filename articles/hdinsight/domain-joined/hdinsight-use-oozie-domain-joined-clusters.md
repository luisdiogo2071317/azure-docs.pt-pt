---
title: Fluxos de trabalho de Hadoop Oozie em Clusters de associados a um domínio de HDInsight
description: Utilizar Oozie de Hadoop no domínio do HDInsight baseado em Linux associados a um pacote de segurança empresarial. Saiba como definir um fluxo de trabalho do Oozie e submeter uma tarefa de Oozie.
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 928f6adbb348683a110f7da9b20efaae998290ca
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972218"
---
#<a name="run-apache-oozie-in-domain-joined-hdinsight-hadoop-clusters"></a>Execute o Apache Oozie em clusters do Hadoop de HDInsight associado a um domínio
Oozie é um sistema de fluxo de trabalho e a coordenação que gere as tarefas do Hadoop. Oozie está integrado com a pilha do Hadoop e suporta as seguintes tarefas:
- Apache MapReduce
- Apache Pig
- Apache Hive
- O Apache Sqoop

Também pode utilizar o Oozie para agendar tarefas que são específicas para um sistema, como programas de Java ou scripts de shell.

##<a name="prerequisites"></a>Pré-requisitos:
- Um cluster de Hadoop do HDInsight associados a um domínio. Consulte [clusters do HDInsight associados a um domínio de configurar](./apache-domain-joined-configure-using-azure-adds.md)

    > [!NOTE]
    > Para ver instruções detalhadas para utilizar o Oozie no domínio não associados ao ver clusters [isso](../hdinsight-use-oozie-linux-mac.md)

##<a name="connecting-to-domain-joined-cluster"></a>Ligar ao domínio associado a um cluster
Para obter mais informações sobre o SSH, veja [autenticação: HDInsight associado a um domínio](../hdinsight-hadoop-linux-use-ssh-unix.md).
- Ligar ao cluster do HDInsight com o SSH:
     ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```
Para verificar se a autenticação Kerberos foi concluída com êxito, utilize `klist` comando. Se não, utilizar `kinit` para iniciar a autenticação Kerberos.

- Inicie sessão no gateway do HDInsight para registar o token de oAuth necessário para aceder ao Azure Data Lake Store (ADLS)
     ```bash
     curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
     ```

    Um código de estado de resposta _200 OK_ indica o registo com êxito. Verifique o nome de utilizador e palavra-passe se for de uma resposta não autorizada (401) recebido.

## <a name="define-the-workflow"></a>Definir o fluxo de trabalho
Definições de fluxo de trabalho de Oozie são escritas no Hadoop processo de definição de idioma (hPDL), que é uma linguagem de definição do processo XML. Utilize os seguintes passos para definir o fluxo de trabalho:

-   Configuração da área de trabalho de utilizador de domínio:
 ```bash
hdfs dfs -mkdir /user/<DomainUser>
cd /home/<DomainUserPath>
cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
tar -xvf oozie-examples.tar.gz
hdfs dfs -put examples /user/<DomainUser>/
 ```
Substitua `DomainUser` com o nome de utilizador de domínio. Substitua `DomainUserPath` com o caminho do diretório raiz para o utilizador de domínio. Substitua `ClusterVersion` com a sua versão HDP do cluster.

-   Utilize a seguinte instrução para criar e editar um novo ficheiro:
 ```bash
nano workflow.xml
 ```

- Depois de abre o editor nano, introduza o seguinte XML como o conteúdo do ficheiro:
 ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <workflow-app xmlns="uri:oozie:workflow:0.4" name="map-reduce-wf">
       <credentials>
          <credential name="metastore_token" type="hcat">
             <property>
                <name>hcat.metastore.uri</name>
                <value>thrift://hn0-<clustername>.<Domain>.com:9083</value>
             </property>
             <property>
                <name>hcat.metastore.principal</name>
                <value>hive/_HOST@<Domain>.COM</value>
             </property>
          </credential>
          <credential name="hs2-creds" type="hive2">
             <property>
                <name>hive2.server.principal</name>
                <value>${jdbcPrincipal}</value>
             </property>
             <property>
                <name>hive2.jdbc.url</name>
                <value>${jdbcURL}</value>
             </property>
          </credential>
       </credentials>
       <start to="mr-test" />
       <action name="mr-test">
          <map-reduce>
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <prepare>
                <delete path="${nameNode}/user/${wf:user()}/examples/output-data/mrresult" />
             </prepare>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
                <property>
                   <name>mapred.mapper.class</name>
                   <value>org.apache.oozie.example.SampleMapper</value>
                </property>
                <property>
                   <name>mapred.reducer.class</name>
                   <value>org.apache.oozie.example.SampleReducer</value>
                </property>
                <property>
                   <name>mapred.map.tasks</name>
                   <value>1</value>
                </property>
                <property>
                   <name>mapred.input.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/input-data/text</value>
                </property>
                <property>
                   <name>mapred.output.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/output-data/mrresult</value>
                </property>
             </configuration>
          </map-reduce>
          <ok to="myHive2" />
          <error to="fail" />
       </action>
       <action name="myHive2" cred="hs2-creds">
          <hive2 xmlns="uri:oozie:hive2-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <jdbc-url>${jdbcURL}</jdbc-url>
             <script>${hiveScript2}</script>
             <param>hiveOutputDirectory2=${hiveOutputDirectory2}</param>
          </hive2>
          <ok to="myHive" />
          <error to="fail" />
       </action>
       <action name="myHive" cred="metastore_token">
          <hive xmlns="uri:oozie:hive-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
             </configuration>
             <script>${hiveScript1}</script>
             <param>hiveOutputDirectory1=${hiveOutputDirectory1}</param>
          </hive>
          <ok to="end" />
          <error to="fail" />
       </action>
       <kill name="fail">
          <message>Oozie job failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name="end" />
    </workflow-app>
 ```
Substitua `clustername` com o nome do cluster. 

Para guardar o ficheiro, selecione Ctrl + X, introduza `Y`e, em seguida, selecione **Enter**.

O fluxo de trabalho é dividido em duas partes:
*   Secção de credenciais: A secção de credenciais utiliza as credenciais que serão utilizados para autenticar o oozie ações:

    Neste exemplo, é utilizada a autenticação para ações do Hive. Para obter mais informações, consulte [isso]( https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

    O serviço de credencial permite as ações de oozie representar o utilizador para aceder aos serviços do Hadoop.

*   Secção de ação: Temos três ações aqui-mapreduce, hive ação de servidor 2 e uma ação de servidor 1 do hive:

    As execuções de map-reduce um exemplo de pacote de oozie para mapreduce que produz a contagem de palavras agregados.

    O servidor do hive 2 e ao hive ações de servidor 1 executa uma consulta simples na tabela de hivesample fornecida com o HDInsight.

    As ações de hive utilizam as credenciais definidas na secção de credenciais para autenticação com a palavra-chave `cred` do elemento de ação

- Utilize o comando seguinte para copiar o `workflow.xml` de ficheiros para `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:
     ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
     ```

    Substitua `domainuser` com seu nome de utilizador para o domínio.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Definir o ficheiro de propriedades para a tarefa de Oozie

1.  Utilize a seguinte instrução para criar e editar um novo arquivo de propriedades da tarefa:
     ```bash
    nano job.properties
     ```

2.   Depois de abre o editor nano, utilize o seguinte XML como o conteúdo do ficheiro:

    ```bash
        nameNode=adl://home
        jobTracker=headnodehost:8050
        queueName=default
        examplesRoot=examples
        oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
        hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
        hiveScript2=${nameNode}/user/${user.name}/countrowshive2.hql
        oozie.use.system.libpath=true
        user.name=[domainuser]
        jdbcPrincipal=hive/hn0-<ClusterShortName>.<Domain>.com@<Domain>.COM
        jdbcURL=[jdbcurlvalue]
        hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
        hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
    ```
    

   * Substitua `domainuser` com seu nome de utilizador para o domínio.
   * Substitua `ClusterShortName` com o nome abreviado para o cluster. Se for o clustername https://sechadoopcontoso.azurehdisnight.net, o `clustershortname` é primeiras seis letras para o cluster: sechad
   * Substitua `jdbcurlvalue` com o url do JDBC partir da configuração do hive. Por exemplo, jdbc:hive2: / / headnodehost:10001 /; transportMode = http.
    
   * Para guardar o ficheiro, selecione Ctrl + X, introduza `Y`e, em seguida, selecione **Enter**.

   * Este ficheiro de propriedades tem de existir localmente ao executar os trabalhos de oozie

## <a name="creating-custom-hive-scripts-for-the-oozie-job"></a>A criar scripts de hive personalizado para a tarefa de Oozie
Os scripts de 2 hive para o servidor do hive 1 e o servidor do hive 2 podem ser criados como o seguinte:
-   Ficheiro de 1 de servidor do Hive:
1.  Utilize a seguinte instrução para criar e editar um ficheiro para a ação de servidor 1 do hive:
    ```bash
    nano countrowshive1.hql
    ```

2.  Criar o script
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemake from hivesampletable limit 2;
    ```

3.  Guarde o ficheiro Hdfs
    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

-   Ficheiro de 2 de servidor do Hive:
1.  Utilize a seguinte instrução para criar e editar um campo para a ação de servidor 2 do hive:
    ```bash
    nano countrowshive2.hql
    ```

2.  Criar o script
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3.  Guarde o ficheiro Hdfs
    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submission-of-oozie-jobs"></a>Submissão de tarefas de Oozie
Submeter tarefas de oozie para clusters associados a um domínio é semelhante ao envio de trabalhos de oozie em clusters de associado ao domínio.

Para obter mais informações, consulte [submeter e gerir a tarefa](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-oozie-job-submission"></a>Resultados da submissão de tarefas do Oozie
Uma vez que o oozie tarefas são executadas em nome do utilizador, Yarn e Ranger de auditoria registos show as tarefas que está sendo executadas como o utilizador representado. A saída da CLI do trabalho oozie ser semelhante ao seguinte:


```bash
    Job ID : 0000015-180626011240801-oozie-oozi-W
    ------------------------------------------------------------------------------------------------
    Workflow Name : map-reduce-wf
    App Path      : adl://home/user/alicetest/examples/apps/map-reduce/wf.xml
    Status        : SUCCEEDED
    Run           : 0
    User          : alicetest
    Group         : -
    Created       : 2018-06-26 19:25 GMT
    Started       : 2018-06-26 19:25 GMT
    Last Modified : 2018-06-26 19:30 GMT
    Ended         : 2018-06-26 19:30 GMT
    CoordAction ID: -
    
    Actions
    ------------------------------------------------------------------------------------------------
    ID                      Status  Ext ID          ExtStatus   ErrCode
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@:start:    OK  -           OK      -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@mr-test    OK  job_1529975666160_0051  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive2    OK  job_1529975666160_0053  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive OK  job_1529975666160_0055  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@end    OK  -           OK      -
    -----------------------------------------------------------------------------------------------
```

*    A Ranger de registos de auditoria para o servidor do hive 2 oozie de mostra ações a executar a ação em nome do utilizador. O ranger e Yarn vista só é visível para o administrador de cluster.

## <a name="configuration-of-user-authorization-in-oozie"></a>Configuração de autorização de utilizador no Oozie
Oozie por si só tem uma configuração de autorização do utilizador, que pode impedir que os utilizadores a parar, cancelar trabalhos de outro utilizador. Esta opção estiver ativada, definindo a `oozie.service.AuthorizationService.security.enabled` para `true`. 

Podem encontrar mais detalhes sobre isso em Oozie documentação secção - [configuração de autorização de utilizador]( https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html):

Para componentes, tais como o servidor do hive 1 onde Plug-in do Ranger não está disponível/suportadas, a autorização de alta granularidade hdfs apenas é possível. Tudo bem autorização refinada só está disponível através de plug-ins do ranger.

## <a name="oozie-web-ui"></a>IU da web do Oozie
A IU da web de Oozie fornece uma vista baseada na web para o estado das tarefas de Oozie no cluster. Domínio associado clusters que tem de:

1. Adicionar uma [nó de extremidade](../hdinsight-apps-use-edge-node.md) e ative [a autenticação SSH Kerberos](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Siga os [IU da web do Oozie](../hdinsight-use-oozie-linux-mac.md) passos para ativar o túnel de SSH ao nó de extremidade e aceder à IU da web.

## <a name="next-steps"></a>Passos Seguintes
* [Utilizar o Oozie com o Hadoop para definir e executar um fluxo de trabalho no HDInsight do Azure baseado em Linux](../hdinsight-use-oozie-linux-mac.md)
* [Utilize o coordenador de Oozie baseados no tempo](../hdinsight-use-oozie-coordinator-time.md)
* [Executar consultas do Hive através do SSH em clusters do HDInsight associados a um domínio](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
