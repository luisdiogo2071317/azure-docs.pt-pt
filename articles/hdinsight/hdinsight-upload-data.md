---
title: Carregue dados para tarefas Hadoop no HDInsight
description: Saiba como carregar e aceder a dados para tarefas do Hadoop no HDInsight com a CLI clássica do Azure, o Explorador de armazenamento do Azure, Azure PowerShell, a linha de comandos do Hadoop ou Sqoop.
keywords: hadoop de ETL, colocar dados no hadoop, dados de carga do hadoop
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: 44aaccee436011bd7d27bec87515fde0e898732e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985984"
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Carregue dados para tarefas Hadoop no HDInsight

O Azure HDInsight fornece um sistema de ficheiros do distribuído do Hadoop de completo (HDFS) ao longo do armazenamento do Azure e Azure Data Lake Store. O Azure Data lake Store e o armazenamento são projetadas como uma extensão HDFS, fornecer uma experiência totalmente integrada aos clientes. Permitem que o conjunto completo de componentes no ecossistema do Hadoop para operar diretamente em dados que gere. O Azure Data Lake Store e o armazenamento são os sistemas de ficheiros distintos que estão otimizados para armazenamento de dados e computações nesses dados. Para obter informações sobre os benefícios da utilização do armazenamento do Azure, consulte [utilizar o armazenamento do Azure com o HDInsight] [ hdinsight-storage] e [Use Data Lake Store com o HDInsight](hdinsight-hadoop-use-data-lake-store.md).

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, tenha em atenção os seguintes requisitos:

* Um cluster do Azure HDInsight. Para obter instruções, consulte [introdução ao Azure HDInsight] [ hdinsight-get-started] ou [clusters do HDInsight criar](hdinsight-hadoop-provision-linux-clusters.md).
* Dados de conhecimento dos dois seguintes artigos:

    - [Utilizar o armazenamento do Azure com o HDInsight][hdinsight-storage]
    - [Use Data Lake Store com o HDInsight](hdinsight-hadoop-use-data-lake-store.md)

## <a name="upload-data-to-azure-storage"></a>Carregar dados para o armazenamento do Azure

### <a name="command-line-utilities"></a>Utilitários da linha de comandos
A Microsoft fornece os seguintes utilitários para trabalhar com o armazenamento do Azure:

| Ferramenta | Linux | SO X | Windows |
| --- |:---:|:---:|:---:|
| [CLI clássica do Azure][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] |✔ | |✔ |
| [Comandos de Hadoop](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Embora o CLI clássica do Azure, o Azure PowerShell e o AzCopy podem todos ser utilizados de fora do Azure, o comando de Hadoop só está disponível no HDInsight cluster. E o comando só permite o carregamento de dados do sistema de ficheiros local para o armazenamento do Azure.
>
>

#### <a id="xplatcli"></a>CLI clássica do Azure
A CLI clássica do Azure é uma ferramenta de várias plataformas que permite-lhe gerir serviços do Azure. Utilize os seguintes passos para carregar dados para o armazenamento do Azure:

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. [Instalar e configurar a CLI clássica do Azure para Mac, Linux e Windows](../cli-install-nodejs.md).
2. Abra um prompt de comando, bash ou outro shell e utilize o seguinte para autenticar a sua subscrição do Azure.

    ```cli
    azure login
    ```

    Quando lhe for pedido, introduza o nome de utilizador e palavra-passe para a sua subscrição.
3. Introduza o seguinte comando para listar as contas de armazenamento para a sua subscrição:

    ```cli
    azure storage account list
    ```

4. Selecione a conta de armazenamento que contém o blob que pretende trabalhar e, em seguida, utilize o seguinte comando para obter a chave para esta conta:

    ```cli
    azure storage account keys list <storage-account-name>
    ```

    Este comando devolve a **primário** e o **secundário** chaves. Copiar o **primário** valor de chave, como vai ser utilizada nos passos seguintes.
5. Utilize o seguinte comando para obter uma lista de contentores de BLOBs na conta de armazenamento:

    ```cli
    azure storage container list -a <storage-account-name> -k <primary-key>
    ```

6. Utilize os seguintes comandos para carregar e transferir ficheiros para o blob:

   * Para carregar um ficheiro:

        ```cli
        azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
        ```

   * Para transferir um ficheiro:

        ```cli
        azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>
        ```
    
> [!NOTE]
> Se sempre trabalha com a mesma conta de armazenamento, pode definir as seguintes variáveis de ambiente em vez de especificar a conta e da chave de cada comando:
>
> * **AZURE\_armazenamento\_conta**: O nome da conta de armazenamento
> * **AZURE\_armazenamento\_acesso\_chave**: A chave de conta de armazenamento
>
>

#### <a id="powershell"></a>O Azure PowerShell
O Azure PowerShell é um ambiente de script que pode utilizar para controlar e automatizar a implementação e gestão das cargas de trabalho no Azure. Para obter informações sobre como configurar a sua estação de trabalho para executar o Azure PowerShell, consulte [instalar e configurar o Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Para carregar um ficheiro local para o armazenamento do Azure**

1. Abra a consola do PowerShell do Azure com as instruções na [instalar e configurar o Azure PowerShell](/powershell/azure/overview).
2. Defina os valores das variáveis primeiros cinco no seguinte script:

    ```powershell
    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<ContainerName>"

    $fileName ="<LocalFileName>"
    $blobName = "<BlobName>"

    # Get the storage account key
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    # Create the storage context object
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    # Copy the file from local workstation to the Blob container
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
    ```

3. Cole o script na consola do Azure PowerShell para executá-lo para copiar o ficheiro.

Por exemplo os scripts do PowerShell criados para funcionar com o HDInsight, consulte [ferramentas do HDInsight](https://github.com/blackmist/hdinsight-tools).

#### <a id="azcopy"></a>AzCopy
O AzCopy é uma ferramenta de linha de comandos que foi concebida para simplificar a tarefa de transferência de dados para dentro e fora de uma conta de armazenamento do Azure. Pode utilizá-la como uma ferramenta autônoma ou incorporar essa ferramenta num aplicativo existente. [Baixe o AzCopy][azure-azcopy-download].

A sintaxe de AzCopy é:

```command
AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]
```

Para obter mais informações, consulte [AzCopy - carregar/transferir ficheiros para os Blobs do Azure][azure-azcopy].

Azcopy no Linux pré-visualização está disponível.  Ver [apresentamos o AzCopy no Linux pré-visualização](https://blogs.msdn.microsoft.com/windowsazurestorage/2017/05/16/announcing-azcopy-on-linux-preview/).

#### <a id="commandline"></a>Linha de comandos do Hadoop
A linha de comandos do Hadoop só é útil para armazenar dados em BLOBs de armazenamento do Azure quando os dados já estão presentes no nó principal do cluster.

Para utilizar o comando de Hadoop, primeiro tem de ligar ao nó principal através de um dos seguintes métodos:

* **HDInsight baseado em Windows**: [ligar através do ambiente de trabalho remoto](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **HDInsight baseado em Linux**: se conectar usando [SSH ou PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md).

Assim que estiver ligado, pode utilizar a sintaxe seguinte para carregar um ficheiro para o armazenamento.

```bash
hadoop -copyFromLocal <localFilePath> <storageFilePath>
```

Por exemplo, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Uma vez que é o sistema de ficheiros predefinido para o HDInsight no armazenamento do Azure, /example/data.txt é, na verdade, no armazenamento do Azure. Também pode consultar o ficheiro como:

    wasb:///example/data/data.txt

ou

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Para obter uma lista de outros comandos do Hadoop que trabalhar com arquivos, consulte [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> Nos clusters do HBase, a predefinição bloquear utilizado quando a escrita de dados é 256 KB de tamanho. Embora isso funciona bem quando utilizar APIs de HBase ou de REST APIs, usando o `hadoop` ou `hdfs dfs` comandos para escrever mais de ~ 12 GB de dados resulta num erro. Para obter mais informações, consulte a [exceção de armazenamento para escrita no blob](#storageexception) secção deste artigo.
>
>

### <a name="graphical-clients"></a>Clientes de gráficos
Também existem vários aplicativos que fornecem uma interface gráfica para trabalhar com o armazenamento do Azure. A tabela seguinte é uma lista de alguns desses aplicativos:

| Cliente | Linux | SO X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio Tools para o HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Explorador do Armazenamento do Azure](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Studio de armazenamento na cloud 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Explorador do Azure](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

#### <a name="visual-studio-tools-for-hdinsight"></a>Ferramentas do Visual Studio para o HDInsight
Para obter mais informações, consulte [navegar nos recursos ligados](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources).

#### <a id="storageexplorer"></a>Explorador de armazenamento do Azure
*Explorador de armazenamento do Azure* é uma ferramenta útil para inspecionar e alterar os dados em blobs. É uma ferramenta gratuita de código fonte aberto que pode ser transferida a partir [ http://storageexplorer.com/ ](http://storageexplorer.com/). O código-fonte está disponível a partir desta ligação também.

Antes de utilizar a ferramenta, deve saber a sua chave de conta e o nome da conta de armazenamento do Azure. Para obter instruções sobre como obter estas informações, consulte o "como: ver, copiar e voltar a gerar armazenamento de chaves de acesso" seção [criar, gerir ou eliminar uma conta de armazenamento][azure-create-storage-account].

1. Execute o Explorador de armazenamento do Azure. Se for a primeira vez que tem de executar o Explorador de armazenamento, que lhe for pedido para o **nome da conta _Storage** e **chave de conta de armazenamento**. Se tiver executado antes, utilize o **adicionar** botão para adicionar um novo nome da conta de armazenamento e a chave.

    Introduza o nome e chave para a conta de armazenamento utilizada pelo cluster do HDInsight e, em seguida, selecione **aberto & Guardar**.

    ![HDI.AzureStorageExplorer][image-azure-storage-explorer]
2. Na lista de contentores para a esquerda da interface, clique no nome do contentor que está associado ao seu cluster do HDInsight. Por padrão, ele é o nome do cluster do HDInsight, mas pode ser diferente se tiver introduzido um nome específico ao criar o cluster.
3. Na barra de ferramentas, selecione o ícone de carregamento.

    ![Barra de ferramentas com o ícone de carregamento realçado](./media/hdinsight-upload-data/toolbar.png)
4. Especifique um ficheiro para carregar e, em seguida, clique em **aberto**. Quando lhe for pedido, selecione **carregar** para carregar o ficheiro para a raiz do contentor de armazenamento. Se pretender carregar o ficheiro para um caminho específico, introduza o caminho no **destino** campo e, em seguida, selecione **carregar**.

    ![Caixa de diálogo de carregamento de arquivo](./media/hdinsight-upload-data/fileupload.png)

    Depois do ficheiro concluiu o carregamento, pode utilizá-lo a partir de tarefas no cluster do HDInsight.

### <a name="mount-azure-storage-as-local-drive"></a>Montar o armazenamento do Azure como unidade Local
Ver [armazenamento do Azure de montagem como unidade Local](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

### <a name="upload-using-services"></a>Carregar com serviços
#### <a name="azure-data-factory"></a>Azure Data Factory
O serviço Azure Data Factory é um serviço totalmente gerido para compor serviços de movimento de dados, processamento de dados e armazenamento de dados em pipelines de produção de dados simplificada, dimensionáveis e fiáveis.

Pode ser utilizado o Azure Data Factory para mover dados para o armazenamento do Azure ou para criar pipelines de dados que utilizam diretamente os recursos do HDInsight, como o Pig e Hive.

Para obter mais informações, consulte a [documentação do Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

#### <a id="sqoop"></a>O Apache Sqoop
Sqoop é uma ferramenta projetada para transferir dados entre o Hadoop e bases de dados relacionais. Pode usá-lo para importar dados de um sistema de gerenciamento de banco de dados relacional (RDBMS), como SQL Server, MySQL ou Oracle para o sistema de ficheiros distribuído Hadoop (HDFS), transformar os dados no Hadoop com o MapReduce ou Hive e, em seguida, exportar os dados de volta para um RDBMS.

Para obter mais informações, consulte [utilização Sqoop com o HDInsight][hdinsight-use-sqoop].

### <a name="development-sdks"></a>SDKs de desenvolvimento
O armazenamento do Azure também pode ser acessado com um SDK de linguagens de programação seguintes:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Para obter mais informações sobre como instalar os SDKs do Azure, consulte [transferências do Azure](https://azure.microsoft.com/downloads/)

### <a name="troubleshooting"></a>Resolução de problemas
#### <a id="storageexception"></a>Exceção de armazenamento para escrita em BLOBs
**Os sintomas**: ao utilizar o `hadoop` ou `hdfs dfs` comandos para escrever ficheiros de ~ 12 GB ou superior num cluster do HBase, pode encontrar o erro seguinte:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Causa**: HBase no HDInsight clusters predefinido para um tamanho de bloco de 256 KB, ao escrever no armazenamento do Azure. Enquanto ele trabalha para APIs do HBase ou de REST APIs, que resulta num erro ao utilizar o `hadoop` ou `hdfs dfs` utilitários da linha de comandos.

**Resolução**: Utilize `fs.azure.write.request.size` para especificar um tamanho de bloco maior. Pode fazer isso numa base por utilização com o `-D` parâmetro. O comando seguinte é um exemplo que usa este parâmetro com o `hadoop` comando:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

Também pode aumentar o valor de `fs.azure.write.request.size` globalmente através do Ambari. Os passos seguintes podem ser utilizados para alterar o valor na IU da Web do Ambari:

1. No seu browser, aceda à IU da Web do Ambari para o seu cluster. Isto é https://CLUSTERNAME.azurehdinsight.net, onde **CLUSTERNAME** é o nome do seu cluster.

    Quando lhe for pedido, introduza o nome de administrador e a palavra-passe para o cluster.
2. Do lado esquerdo do ecrã, selecione **HDFS**e, em seguida, selecione a **configurações** separador.
3. Na **filtro...**  , insira `fs.azure.write.request.size`. Esta ação apresenta o campo e o valor atual no meio da página.
4. Altere o valor de 262144 (256KB) para o novo valor. Por exemplo, 4194304 (4MB).

![Imagem da alteração do valor por meio da interface do Usuário de Web de Ambari](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Para obter mais informações sobre a utilização do Ambari, consulte [clusters do HDInsight gerir com a interface do Usuário da Web de Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Passos Seguintes
Agora que compreende como obter dados no HDInsight, leia os artigos seguintes para saber como executar uma análise:

* [Get started with Azure HDInsight (Introdução ao Azure HDInsight)][hdinsight-get-started]
* [Submeter tarefas do Hadoop de forma programática][hdinsight-submit-jobs]
* [Use Hive with HDInsight (Utilizar o Hive com o HDInsight)][hdinsight-use-hive]
* [Use Pig with HDInsight (Utilizar o Pig com o HDInsight)][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
