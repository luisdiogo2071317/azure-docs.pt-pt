---
title: Utilizar a CLI do HDFS com pré-visualização do Azure Data Lake Storage geração 2
description: Introdução à CLI do HDFS para a pré-visualização do Data Lake Storage geração 2
services: storage
author: artemuwka
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: artek
ms.component: data-lake-storage-gen2
ms.openlocfilehash: c5f11cbb12b727f5f308d7a71c51706fa8ec373f
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277091"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Utilizar a CLI do HDFS com o Data Lake Storage Gen2

Pré-visualização da geração 2 do armazenamento do Azure Data Lake permite-lhe gerir e aceder aos dados, tal como faria com um [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Se tiver um cluster do HDInsight anexado ou executar uma tarefa de Apache Spark com o Azure Databricks para realizar análises em dados armazenados numa conta de armazenamento do Azure, pode utilizar a interface de linha de comandos (CLI) para recuperar e manipular os dados carregados.

## <a name="hdfs-cli-with-hdinsight"></a>CLI do HDFS com o HDInsight

O HDInsight fornece acesso ao sistema de ficheiros distribuído que está ligado localmente aos nós de computação. Este sistema de ficheiros pode ser acedido através da utilização da shell que interage diretamente com o HDFS e outros sistemas de ficheiros que suporta o Hadoop. Seguem-se os comandos comumente utilizados e os links para recursos úteis.

>[!IMPORTANT]
>Faturação do cluster do HDInsight é iniciado depois de um cluster é criado e termina quando o cluster é eliminado. A faturação é rateada por minuto, pelo que deve sempre eliminar o cluster quando deixar de ser utilizado. Para saber como eliminar um cluster, consulte nosso [artigo sobre o tópico](../../hdinsight/hdinsight-delete-cluster.md). No entanto, os dados armazenados numa conta de armazenamento com o Data Lake Storage Gen2 ativada persistir até mesmo depois de eliminar um cluster do HDInsight.

Para obter uma lista de arquivos ou diretórios:

    hdfs dfs -ls <args>
Para criar um diretório:

    hdfs dfs -mkdir [-p] <paths>
Para eliminar um ficheiro ou diretório:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Vejamos agora o cluster do HDInsight Hadoop no Linux como exemplo. Para utilizar a CLI do HDFS, primeiro tem de estabelecer [acesso remoto aos serviços](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Se escolher [SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) o código do PowerShell de exemplo teria o aspeto da seguinte forma:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

A cadeia de ligação pode ser encontrada no "SSH + Cluster início de sessão" seção do painel do cluster do HDInsight no portal do Azure. Foram especificadas credenciais SSH no momento da criação do cluster.

Para obter mais informações sobre a CLI do HDFS, consulte a [documentação oficial](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) e o [guia de permissões do HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>CLI de HDFS com o Azure Databricks

O Databricks fornece uma CLI fácil de usar, criada sobre a API de REST do Databricks. O projeto de código-fonte aberto está hospedado no [GitHub](https://github.com/databricks/databricks-cli). Seguem-se os comandos comumente utilizados.

Para obter uma lista de arquivos ou diretórios:

    dbfs ls [-l]
Para criar um diretório:

    dbfs mkdirs
Para eliminar um ficheiro:

    dbfs rm [-r]

Outra forma de interação com o Databricks são blocos de notas. Embora um bloco de notas tenha um idioma primário, pode combinar vários idiomas, especificando o idioma de % de comando mágico de linguagem no início de uma célula. Especificamente, % sh permite-lhe executar o código de shell no seu bloco de notas muito parecido com o exemplo de HDInsight no início deste artigo.

Para obter uma lista de arquivos ou diretórios:

    %sh ls <args>
Para criar um diretório:

    %sh mkdir [-p] <paths>
Para eliminar um ficheiro ou diretório:

    %sh rm [-skipTrash] URI [URI ...]

Depois de iniciar o cluster do Spark no Azure Databricks, irá criar um novo bloco de notas. O exemplo de script de bloco de notas será semelhante ao seguinte:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Para obter mais informações sobre a CLI do Databricks, consulte a [documentação oficial](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Para obter mais informações nos blocos de notas, consulte a [blocos de notas](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) secção da documentação.

## <a name="next-steps"></a>Passos Seguintes

- [Criar um cluster do HDInsight com geração 2 de armazenamento do Azure Data Lake](./quickstart-create-connect-hdi-cluster.md)
- [Utilizar uma conta com capacidade de geração 2 de armazenamento do Azure Data Lake no Azure Databricks](./quickstart-create-databricks-account.md) 