---
title: Consulta do Hive através de controlador JDBC - Azure HDInsight | Microsoft Docs
description: Utilize o controlador JDBC partir de uma aplicação de Java para submeter consultas do Hive ao Hadoop no HDInsight. Liga através de programação e do cliente SQuirrel SQL.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: larryfr
ms.openlocfilehash: 9700408dad36591fc26aa159524f33965b9d1324
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31399420"
---
# <a name="query-hive-through-the-jdbc-driver-in-hdinsight"></a>Consulta do Hive através de controlador JDBC no HDInsight

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Saiba como utilizar o controlador JDBC partir de uma aplicação de Java para submeter consultas do Hive ao Hadoop no Azure HDInsight. As informações neste documento demonstra como estabelecer ligação através de programação e do cliente SQuirrel SQL.

Para obter mais informações sobre a Interface de JDBC do Hive, consulte [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Pré-requisitos

* Um Hadoop no cluster do HDInsight.

  > [!IMPORTANT]
  > O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, consulte [HDInsight 3.3 extinção](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL é uma aplicação de cliente JDBC.

* O [Java Developer Kit (JDK) versão 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) ou superior.

* [Apache Maven](https://maven.apache.org). Maven é um projeto construir o sistema para projetos de Java que é utilizado pelo projeto associado a este artigo.

## <a name="jdbc-connection-string"></a>Cadeia de ligação JDBC

Ligações de JDBC para um cluster do HDInsight no Azure são efetuadas mais 443 e o tráfego está protegido por SSL. Gateway público que os clusters manter-se por trás redireciona o tráfego para a porta que HiveServer2 é, na verdade, está a escutar. A cadeia de ligação seguinte mostra o formato a utilizar para o HDInsight:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Substitua `CLUSTERNAME` pelo nome do seu cluster do HDInsight.

## <a name="authentication"></a>Autenticação

Ao estabelecer a ligação, tem de utilizar o nome de administrador de cluster do HDInsight e a palavra-passe para autenticar para o gateway de cluster. Ao ligar a partir de clientes JDBC, tais como o SQuirreL SQL, é necessário introduzir o nome de administrador e a palavra-passe nas definições do cliente.

A partir de uma aplicação Java, tem de utilizar o nome e a palavra-passe ao estabelecer uma ligação. Por exemplo, o seguinte código Java abre uma nova ligação com a cadeia de ligação, o nome de administrador e a palavra-passe:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Estabelecer ligação com o cliente SQuirreL SQL

SQuirreL SQL é um cliente JDBC que pode ser utilizado para executar remotamente as consultas do Hive com o cluster do HDInsight. Os seguintes passos partem do princípio de que já tiver instalado o SQuirreL SQL.

1. Crie um diretório que contém os ficheiros. Por exemplo, `mkdir hivedriver`.

2. A partir de uma linha de comandos, utilize os seguintes comandos para copiar os ficheiros do cluster do HDInsight:

    ```bash
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/lib/log4j-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/lib/slf4j-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-*-1.2*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/httpclient-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/httpcore-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/libthrift-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/libfb*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-logging-*.jar .
    ```

    Substitua `USERNAME` com o nome de conta de utilizador do SSH para o cluster. Substitua `CLUSTERNAME` com o nome de cluster do HDInsight.

3. Inicie a aplicação de SQuirreL SQL. No lado esquerdo da janela, selecione **controladores**.

    ![Separador de controladores no lado esquerdo da janela](./media/apache-hadoop-connect-hive-jdbc-driver/squirreldrivers.png)

4. Partir dos ícones na parte superior do **controladores** caixa de diálogo, selecione o **+** ícone para criar um controlador.

    ![Ícones de controladores](./media/apache-hadoop-connect-hive-jdbc-driver/driversicons.png)

5. Na caixa de diálogo Adicionar controlador, adicione as seguintes informações:

    * **Nome**: ramo de registo
    * **URL do exemplo**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Caminho de classe extra**: Utilize o botão Adicionar para adicionar o todos os ficheiros jar transferida anteriormente
    * **Nome de classe**: org.apache.hive.jdbc.HiveDriver

   ![controlador caixa de diálogo Adicionar](./media/apache-hadoop-connect-hive-jdbc-driver/adddriver.png)

   Clique em **OK** para guardar estas definições.

6. No lado esquerdo da janela SQuirreL SQL, selecione **Aliases**. Em seguida, clique o **+** ícone para criar um alias de ligação.

    ![Adicionar novo alias](./media/apache-hadoop-connect-hive-jdbc-driver/aliases.png)

7. Utilize os seguintes valores para o **adicionar Alias** caixa de diálogo.

    * **Nome**: Hive no HDInsight

    * **Controlador**: Utilize o menu pendente para selecionar o **Hive** controlador

    * **URL**: `jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2`

        Substitua **CLUSTERNAME** pelo nome do cluster do HDInsight.

    * **Nome de utilizador**: O nome de conta de início de sessão do cluster para o seu cluster HDInsight. A predefinição é `admin`.

    * **Palavra-passe**: A palavra-passe para a conta de início de sessão do cluster.

 ![alias caixa de diálogo Adicionar](./media/apache-hadoop-connect-hive-jdbc-driver/addalias.png)

    > [!IMPORTANT] 
    > Utilize o **teste** botão para verificar se a ligação funciona. Quando **ligar à: Hive no HDInsight** é apresentada a caixa de diálogo, selecione **Connect** para executar o teste. Se o teste tiver êxito, verá um **ligação com êxito** caixa de diálogo. Se ocorrer um erro, consulte o artigo [resolução de problemas](#troubleshooting).

    Para guardar o alias de ligação, utilize o **Ok** na parte inferior do **adicionar Alias** caixa de diálogo.

8. Do **ligar ao** pendente na parte superior do SQuirreL SQL, selecione **do Hive no HDInsight**. Quando lhe for pedido, selecione **Connect**.

    ![caixa de diálogo de ligação](./media/apache-hadoop-connect-hive-jdbc-driver/connect.png)

9. Assim que estiver ligado, introduza a seguinte consulta para a caixa de diálogo de consulta SQL e, em seguida, selecione o **executar** ícone. A área de resultados deve mostrar os resultados da consulta.

        select * from hivesampletable limit 10;

    ![diálogo de consulta de SQL, incluindo os resultados](./media/apache-hadoop-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Ligar a partir de um exemplo de aplicação Java

Um exemplo de utilização de um cliente de Java para consulta do Hive no HDInsight está disponível em [ https://github.com/Azure-Samples/hdinsight-java-hive-jdbc ](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Siga as instruções no repositório para compilar e executar o exemplo.

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>Ocorreu um erro inesperado tentar abrir uma ligação de SQL

**Sintomas**: ao ligar a um cluster do HDInsight versão 3.3 ou superior, poderá receber um erro que ocorreu um erro inesperado. O rastreio da pilha para este erro começa com as seguintes linhas:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Causa**: Este erro é causado por um ficheiro de commons codec.jar versão mais antigo incluído com o SQuirreL.

**Resolução**: para corrigir este erro, utilize os seguintes passos:

1. Transferir o ficheiro jar do commons codec do cluster do HDInsight.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. Sair SQuirreL e, em seguida, avance para o diretório onde o SQuirreL está instalado no seu sistema. No diretório SquirreL, sob o `lib` diretório, substitua o codec.jar commons existente com o transferido a partir do cluster do HDInsight.

3. Reinicie o SQuirreL. Já não deve ocorrer o erro ao ligar ao ramo no HDInsight.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como utilizar JDBC para trabalhar com o Hive, utilize as hiperligações seguintes para explorar as outras formas de trabalhar com o Azure HDInsight.

* [Visualizar dados do Hive com o Microsoft Power BI no Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Visualizar dados de ramo de registo de consultas interativas com o Power BI no Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Utilize Zeppelin para executar consultas do Hive no Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Ligar o Excel para o HDInsight com o controlador ODBC do Microsoft Hive](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Ligar o Excel ao Hadoop com o Power Query](apache-hadoop-connect-excel-power-query.md).
* [Ligar ao Azure HDInsight e executar consultas do Hive, utilizando ferramentas do Data Lake para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Utilizar a ferramenta do Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carregar dados para o HDInsight](../hdinsight-upload-data.md)
* [Utilizar o Hive com o HDInsight](hdinsight-use-hive.md)
* [Utilizar o Pig com o HDInsight](hdinsight-use-pig.md)
* [Utilizar tarefas de MapReduce com o HDInsight](hdinsight-use-mapreduce.md)
