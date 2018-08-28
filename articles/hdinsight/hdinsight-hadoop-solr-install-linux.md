---
title: Utilize a ação de Script para instalar o Solr num baseado em Linux HDInsight - Azure
description: Saiba como instalar o Solr nos clusters do Hadoop de HDInsight baseado em Linux utilizando as ações de Script.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: 205983344be8ae5bbe566a208ceb862b2e93cb8d
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43093102"
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Instalar e utilizar Solr nos clusters do HDInsight Hadoop

Saiba como instalar o Solr num Azure HDInsight ao utilizar a ação de Script. A Solr é uma plataforma de pesquisa poderosas e fornece capacidades de pesquisa de nível empresarial em dados geridos pelo Hadoop.

> [!IMPORTANT]
    > Os passos neste documento exigem um cluster do HDInsight que utilize o Linux. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

> [!IMPORTANT]
> O script de exemplo utilizado neste documento instala Solr 4.9 com uma configuração específica. Se quiser configurar o cluster de Solr com diferentes coleções, partições horizontais, esquemas, as réplicas, etc., tem de modificar o script e o Solr binários.

## <a name="whatis"></a>O que é o Solr

[Apache Solr](http://lucene.apache.org/solr/features.html) é uma plataforma de pesquisa empresarial que permite a poderosa pesquisa de texto completo em dados. Embora Hadoop permite armazenar e gerir grandes quantidades de dados, o Apache Solr fornece as capacidades de pesquisa para obter rapidamente os dados.

> [!WARNING]
> Componentes fornecidos com o cluster do HDInsight são totalmente suportadas pela Microsoft.
>
> Componentes personalizados, por exemplo, Solr, recebem suporte comercialmente razoável para ajudá-lo a resolver o problema. Suporte da Microsoft pode não ser capaz de resolver problemas com componentes personalizados. Terá de interagir com as Comunidades de código-fonte aberto para obter assistência. Por exemplo, há muitos sites de Comunidade que podem ser utilizados, como: [fórum do MSDN para o HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Também projetos Apache tem sites de projeto no [ http://apache.org ](http://apache.org), por exemplo: [Hadoop](http://hadoop.apache.org/).

## <a name="what-the-script-does"></a>O que faz o script

Este script faz as seguintes alterações para o cluster do HDInsight:

* Instala o Solr 4.9 em `/usr/hdp/current/solr`
* Cria um usuário **solrusr**, que é utilizado para executar o serviço de Solr
* Conjuntos **solruser** como proprietário do `/usr/hdp/current/solr`
* Adiciona uma [Upstart](http://upstart.ubuntu.com/) configuração inicia automaticamente o Solr.

## <a name="install"></a>Instalar o Solr com ações de Script

Um script de exemplo para instalar o Solr num cluster do HDInsight está disponível na seguinte localização:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

Para criar um cluster com o Solr instalado, utilize os passos a [clusters do HDInsight criar](hdinsight-hadoop-create-linux-clusters-portal.md) documento. Durante o processo de criação, utilize os seguintes passos para instalar o Solr:

1. Partir do __resumo do Cluster__ secção, settings__ select__Advanced, em seguida, __ações de Script__. Utilize as seguintes informações para preencher o formulário:

   * **NOME**: introduza um nome amigável para a ação de script.
   * **URI DO SCRIPT**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Marque esta opção
   * **Função de trabalho**: Marque esta opção
   * **ZOOKEEPER**: Marque esta opção para instalar no nó do Zookeeper
   * **PARÂMETROS**: deixe este campo em branco

2. Na parte inferior a **ações de Script** secção, utilize o **selecione** botão para guardar a configuração. Por último, utilize o **próxima** botão para retornar para o __resumo do Cluster__

3. Partir do __resumo do Cluster__ página, selecione __criar__ para criar o cluster.

## <a name="usesolr"></a>Como posso utilizar Solr no HDInsight

> [!IMPORTANT]
> Os passos nesta secção demonstram a funcionalidade básica do Solr. Para obter mais informações sobre como utilizar Solr, consulte a [Apache Solr site](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Dados de índice

Utilize os seguintes passos para adicionar dados de exemplo a Solr e, em seguida, consultá-los:

1. Ligar ao cluster do HDInsight com o SSH:

    > [!NOTE]
    > Substitua `sshuser` com o utilizador SSH para o cluster. Substitua `clustername` com o nome do cluster.

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Para obter mais informações, veja [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

     > [!IMPORTANT]
     > Passos mais adiante neste documento utilizam um túnel SSH para ligar à IU da web Solr. Para utilizar estes passos, tem de estabelecer um túnel SSH e, em seguida, configure o seu browser para utilizá-lo.
     >
     > Para obter mais informações, consulte a [Use SSH Tunneling túnel com o HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) documento.

2. Utilize os seguintes comandos para que dados de exemplo do índice Solr:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    O seguinte resultado é devolvido para a consola:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    O `post.jar` utilitário adiciona o **solr.xml** e **monitor.xml** documentos para o índice.
  
3. Utilize o seguinte comando para consultar a API de REST Solr:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Este comando procura **coleção1** para todos os documentos correspondentes **\*:\*** (codificado como \*% 3A\* na cadeia de consulta). O documento JSON seguinte é um exemplo da resposta:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }

### <a name="using-the-solr-dashboard"></a>Utilizar o dashboard de Solr

O dashboard de Solr é uma interface de Usuário que lhe permite trabalhar com o Solr por meio de seu navegador da web do web. O dashboard de Solr não está exposto diretamente na Internet do seu cluster do HDInsight. Pode utilizar um túnel SSH para aceder ao mesmo. Para obter mais informações sobre como utilizar um túnel SSH, consulte a [Use SSH Tunneling túnel com o HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) documento.

Depois de estabelecer um túnel SSH, utilize os seguintes passos para utilizar o dashboard de Solr:

1. Determine o nome de anfitrião para o nó principal primário:

   1. Utilize o SSH para ligar ao nó principal do cluster. Por exemplo, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       Para obter mais informações sobre como utilizar o SSH, consulte a [utilizar o SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Utilize o seguinte comando para obter o nome de anfitrião totalmente qualificado:

        ```bash
        hostname -f
        ```

        Este comando devolve um valor semelhante ao nome de anfitrião do seguinte:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Guarde o valor devolvido, uma vez que é utilizado mais tarde.

2. No seu browser, ligue ao **http://HOSTNAME:8983/solr/#/**, em que **nome do anfitrião** é o nome que determinou nos passos anteriores.

    O pedido é encaminhado através do túnel SSH para a web Solr da interface do Usuário no seu cluster. A página parece ser semelhante à seguinte imagem:

    ![Imagem do dashboard de Solr](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. A partir do painel esquerdo, utilize o **Seletor de núcleo** pendente para selecionar **coleção1**. Várias entradas-los aparecer abaixo **coleção1**.

4. Partir das entradas abaixo **coleção1**, selecione **consulta**. Utilize os seguintes valores para preencher a página de pesquisa:

   * Na **p** texto, digite  **\*:**\*. Esta consulta devolve todos os documentos que são indexados no Solr. Se quiser procurar uma cadeia específica dentro dos documentos, pode introduzir aqui essa cadeia de caracteres.
   * Na **wt** texto caixa, selecione o formato de saída. A predefinição é **json**.

     Por fim, selecione o **executar consulta** na parte inferior de cole a pesquisa.

     ![Utilize a ação de Script para personalizar um cluster](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     A saída devolve dois documentos que adicionou anteriormente para o índice. O resultado é semelhante para o documento JSON seguinte:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }

### <a name="starting-and-stopping-solr"></a>Iniciar e parar o Solr

Utilize os seguintes comandos para parar e iniciar o Solr manualmente:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Cópia de segurança de dados indexados

Utilize os seguintes passos para criar cópias de segurança Solr dados para o armazenamento predefinido para o seu cluster:

1. Ligar ao cluster através de SSH e, em seguida, utilize o seguinte comando para obter o nome de anfitrião para o nó principal:

    ```bash
    hostname -f
    ```

2. Utilize o seguinte comando para criar um instantâneo dos dados indexados. Substitua **HOSTNAME** com o nome devolvido pelo comando anterior:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    A resposta é semelhante ao seguinte XML:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Altere os diretórios para `/usr/hdp/current/solr/example/solr`. Há um subdiretório para cada coleção. Cada diretório de coleção contém um `data` diretório que contém o instantâneo para a coleção.

4. Para criar um arquivo compactado da pasta de instantâneos, utilize o seguinte comando:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Substitua o `snapshot.20150806185338855` valores com o nome do instantâneo para a sua coleção.

    Este comando cria um arquivo denominado **snapshot.20150806185338855.tgz**, que contém o conteúdo do **snapshot.20150806185338855** diretório.

5. Em seguida, pode armazenar o arquivo para armazenamento primário do cluster com o seguinte comando:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Para obter mais informações sobre como trabalhar com o Solr cópia de segurança e restauros, consulte [ https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups ](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Passos Seguintes

* [Instalar o Giraph nos clusters do HDInsight](hdinsight-hadoop-giraph-install-linux.md). Utilize a personalização de cluster para instalar o Giraph nos clusters do HDInsight Hadoop. Giraph permite-lhe efetuar o processamento através do Hadoop de gráficos e pode ser utilizado com o Azure HDInsight.

* [Instalar a Hue em clusters do HDInsight](hdinsight-hadoop-hue-linux.md). Utilize a personalização de cluster para instalar a Hue em clusters do HDInsight Hadoop. Hue é um conjunto de aplicações Web utilizado para interagir com um cluster do Hadoop.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
