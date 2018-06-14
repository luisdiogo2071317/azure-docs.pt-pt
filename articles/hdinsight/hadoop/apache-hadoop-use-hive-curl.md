---
title: Utilizar o Hive do Hadoop com Curl no HDInsight - Azure | Microsoft Docs
description: Saiba como remotamente submeter tarefas do Pig para o HDInsight utilizando Curl.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: larryfr
ms.openlocfilehash: f602cf45165625ec252f2e29cb9b0e5ed878f3a8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32170259"
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Executar consultas do Hive com o Hadoop no HDInsight com REST

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Saiba como utilizar a API de REST de WebHCat para executar consultas do Hive com o Hadoop no Azure HDInsight cluster.

## <a name="prerequisites"></a>Pré-requisitos

* Um Hadoop baseado em Linux no clusters do HDInsight versão 3.4 ou superior.

  > [!IMPORTANT]
  > O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

* Um cliente REST. Este documento utiliza o Windows PowerShell e [Curl](http://curl.haxx.se/) exemplos.

    > [!NOTE]
    > O Azure PowerShell fornece cmdlets dedicados para trabalhar com o Hive no HDInsight. Para obter mais informações, consulte o [utilizar o Hive com o Azure PowerShell](apache-hadoop-use-hive-powershell.md) documento.

Este documento também utiliza o Windows PowerShell e [Jq](http://stedolan.github.io/jq/) para processar os dados JSON devolvidos de pedidos REST.

## <a id="curl"></a>Executar uma consulta do Hive

> [!NOTE]
> Quando utilizar cURL ou quaisquer outras comunicações REST com WebHCat, tem de autenticar os pedidos fornecendo um nome de utilizador e palavra-passe para o administrador de cluster do HDInsight.
>
> A API de REST está protegida por [autenticação básica](http://en.wikipedia.org/wiki/Basic_access_authentication). Para ajudar a garantir que as suas credenciais são enviadas de forma segura para o servidor, sempre efetue pedidos utilizando HTTP Secure (HTTPS).

1. Para definir o início de sessão do cluster que é utilizado pelos scripts neste documento, utilize um dos seguintes comandos:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Para definir o nome do cluster, utilize um dos seguintes comandos:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. Para verificar se consegue ligar ao cluster do HDInsight, utilize um dos seguintes comandos:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status)
    ```
    
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/status" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Receberá uma resposta semelhante para o seguinte texto:

    ```json
    {"status":"ok","version":"v1"}
    ```

    Os parâmetros utilizados neste comando são os seguintes:

    * `-u` -O nome de utilizador e palavra-passe utilizada para autenticar o pedido.
    * `-G` -Indica que este pedido é uma operação GET.

   O início do URL, `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`, é o mesmo para todos os pedidos. O caminho, `/status`, indica que o pedido está a devolver um Estado de WebHCat (também conhecido como Templeton) para o servidor. Também pode solicitar a versão do ramo de registo utilizando o seguinte comando:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/version/hive" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Este pedido devolve uma resposta semelhante ao seguinte texto:

    ```json
        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}
    ```

4. Utilize o seguinte para criar uma tabela com o nome **log4jLogs**:

    ```bash
    JOBID=`curl -s -u $LOGIN -d user.name=$LOGIN -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/rest" https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/hive | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{"user.name"="admin";"execute"="set hive.execution.engine=tez;DROP TABLE log4jLogs;CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED BY ' ' STORED AS TEXTFILE LOCATION '/example/data/;SELECT t4 AS sev,COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;";"statusdir"="/example/rest"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/hive" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    Este pedido utiliza o método POST, o que envia dados como parte do pedido para a API REST. Os seguintes valores de dados são enviados com o pedido:

     * `user.name` -O utilizador que está a executar o comando.
     * `execute` -As declarações HiveQL para executar.
     * `statusdir` -O diretório que o estado desta tarefa é escrito.

   As declarações de efetuar as seguintes ações:
   
   * `DROP TABLE` -Se a tabela já existir, é eliminado.
   * `CREATE EXTERNAL TABLE` -Cria uma nova tabela 'externa' no ramo de registo. As tabelas externas armazenam a definição de tabela no ramo de registo. Os dados for deixados na localização original.

     > [!NOTE]
     > As tabelas externas devem ser utilizadas ao espera os dados subjacentes ser atualizados por uma origem externa. Por exemplo, um processo de carregamento de dados automática ou outra operação de MapReduce.
     >
     > Remover uma tabela externa **não** eliminar os dados, a definição de tabela.

   * `ROW FORMAT` -Como estão formatados corretamente os dados. Os campos no registo de cada são separados por um espaço.
   * `STORED AS TEXTFILE LOCATION` -Onde os dados são armazenados (o diretório de dados de exemplo /) e de que esta está armazenada como texto.
   * `SELECT` -Seleciona uma contagem de todas as linhas onde coluna **t4** contém o valor **[erro]**. Esta declaração devolve um valor **3** porque existem três linhas que contêm este valor.

     > [!NOTE]
     > Tenha em atenção que os espaços entre declarações HiveQL são substituídos pelo `+` caráter quando utilizado com Curl. Delimitado por aspas valores que contêm um espaço, tais como o delimitador, não devem ser substituídos pelo `+`.

      Este comando devolve um ID de tarefa que pode ser utilizado para verificar o estado da tarefa.

5. Para verificar o estado da tarefa, utilize o seguinte comando:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    Se a tarefa estiver concluída, o estado é **com êxito**.

6. Depois do Estado da tarefa foi alterado para **com êxito**, pode obter os resultados da tarefa do armazenamento de Blobs do Azure. O `statusdir` parâmetro transmitido com a consulta contém a localização do ficheiro de saída; neste caso, `/example/rest`. Este endereço armazena os resultados no `example/curl` diretório no armazenamento de predefinido de clusters.

    Pode listar e transferir estes ficheiros utilizando o [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). Para obter mais informações sobre como utilizar a CLI do Azure com o Storage do Azure, consulte o [utilização do Azure CLI 2.0 com armazenamento do Azure](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) documento.

## <a id="nextsteps"></a>Passos seguintes

Para obter informações gerais sobre o Hive com o HDInsight:

* [Utilizar o Hive com o Hadoop no HDInsight](hdinsight-use-hive.md)

Para obter informações sobre outras formas pode trabalhar com o Hadoop no HDInsight:

* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)
* [Utilizar o MapReduce com o Hadoop no HDInsight](hdinsight-use-mapreduce.md)

Se estiver a utilizar com o Hive no Tez, consulte os seguintes documentos para obter informações de depuração:

* [Utilize a vista Ambari Tez no HDInsight baseado em Linux](../hdinsight-debug-ambari-tez-view.md)

Para obter mais informações sobre a API de REST utilizado neste documento, consulte o [WebHCat referência](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) documento.

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


