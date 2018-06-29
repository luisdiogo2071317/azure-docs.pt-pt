---
title: Utilizar a CLI do HDFS pré-visualização do Azure Data Lake armazenamento Gen2
description: Introdução ao HDFS CLI para pré-visualização do Data Lake armazenamento Gen2
services: storage
documentationcenter: ''
author: artemuwka
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: artek
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 75fb07120c78c45d422ee5017eac0afcf0e80859
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060823"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Utilizar a CLI do HDFS com o Data Lake armazenamento Gen2

Azure Data Lake armazenamento Gen2 Preview permite-lhe gerir e aceder aos dados, tal como faria com um [distribuídas ficheiro sistema Hadoop (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Se tiver um cluster do HDInsight anexado ou executar uma tarefa do Apache Spark utilizando Databricks do Azure para efetuar análises em dados armazenados no Azure Data Lake armazenamento Gen2 pode utilizar a interface de linha de comandos (CLI) para obter e manipular os dados carregados. O resto do artigo descreve as opções têm enquanto [equipa de armazenamento do Azure está a trabalhar na adição de suporte para o Explorador de armazenamento do Azure e o portal do Azure](https://azure.microsoft.com/roadmap/) -ter diversão!

## <a name="hdfs-cli-with-hdinsight"></a>CLI do HDFS com o HDInsight

O HDInsight fornece acesso ao sistema de ficheiros distribuído que está ligado localmente aos nós de computação. Este sistema de ficheiros pode ser acedido utilizando a shell que interage diretamente com o HDFS e outros sistemas de ficheiros que suporta o Hadoop. Seguem-se os comandos frequentemente utilizados e as ligações para recursos úteis.

>[!IMPORTANT]
>A faturação do cluster do HDInsight tem início quando o cluster é criado e termina quando é eliminado. Faturação é calculada por minuto, pelo que deve sempre eliminar o cluster quando já não está a ser utilizado (Saiba como [eliminar um cluster](../../hdinsight/hdinsight-delete-cluster.md)). No entanto, os dados armazenados no Azure Data Lake armazenamento Gen2 persistir, mesmo depois de um cluster do HDInsight é eliminado.

Para obter uma lista de ficheiros ou diretórios:

    hdfs dfs -ls <args>
Para criar um diretório:

    hdfs dfs -mkdir [-p] <paths>
Para eliminar um ficheiro ou diretório:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Vamos agora cluster do HDInsight Hadoop no Linux como um exemplo. Para utilizar a CLI do HDFS, primeiro tem de estabelecer [acesso remoto aos serviços](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Se escolher [SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) o código do PowerShell de exemplo seria ter o seguinte aspeto:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

É possível localizar a cadeia de ligação a "SSH + Cluster início de sessão" secção do painel de cluster do HDInsight no portal do Azure. Foram especificadas credenciais de SSH no momento da criação do cluster.

Para obter mais informações sobre a CLI do HDFS, consulte o [documentação oficial](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) e [HDFS permissões guia](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>CLI do HDFS com Databricks do Azure

O Databricks fornece um CLI de fácil utilização desenvolvida com a API de REST Databricks. O projeto open source estiver alojado num [GitHub](https://github.com/databricks/databricks-cli). Seguem-se os comandos comummente utilizados.

Para obter uma lista de ficheiros ou diretórios:

    dbfs ls [-l]
Para criar um diretório:

    dbfs mkdirs
Para eliminar um ficheiro:

    dbfs rm [-r]

Outra forma de interagir com Databricks são os blocos de notas. Enquanto um bloco de notas tem um idioma principal, pode misturar idiomas, especificando o idioma de % de comando mágica de idioma no início de uma célula. Especificamente, % ostrar permite-lhe executar o código de shell no seu bloco de notas muito semelhantes no exemplo HDInsight anteriores no artigo.

Para obter uma lista de ficheiros ou diretórios:

    %sh ls <args>
Para criar um diretório:

    %sh mkdir [-p] <paths>
Para eliminar um ficheiro ou diretório:

    %sh rm [-skipTrash] URI [URI ...]

Depois de iniciar o cluster do Spark no Azure Databricks, irá criar um novo bloco de notas. O script de exemplo do bloco de notas irão ter o seguinte aspeto:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Para obter mais informações sobre Databricks CLI, consulte o [documentação oficial](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Para obter mais informações sobre blocos de notas, consulte o [blocos de notas](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) secção da documentação.

## <a name="next-steps"></a>Passos Seguintes

- [Criar um cluster do HDInsight com Gen2 de armazenamento do Azure Data Lake](./quickstart-create-connect-hdi-cluster.md)
- [Utilizar uma conta de com capacidade de Gen2 de armazenamento do Azure Data Lake no Azure Databricks](./quickstart-create-databricks-account.md) 