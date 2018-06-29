---
title: Soluções de armazenamento do Azure para os serviços no HDInsight - Azure ML | Microsoft Docs
description: Saiba mais sobre as opções de armazenamento diferentes disponíveis com os serviços de ML no HDInsight
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: f5b9b180f8a6f825e4d91850ee72af19e6d09a4c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052968"
---
# <a name="azure-storage-solutions-for-ml-services-on-azure-hdinsight"></a>Soluções de armazenamento do Azure ML serviços no Azure HDInsight

Serviços de ML no HDInsight pode utilizar uma variedade de soluções de armazenamento para manter os dados, código ou objetos que contêm os resultados da análise. Estas incluem as seguintes opções:

- [Blob do Azure](https://azure.microsoft.com/services/storage/blobs/)
- [Armazenamento do Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/)
- [File storage do Azure](https://azure.microsoft.com/services/storage/files/)

Também tem a opção de aceder a várias contas de armazenamento do Azure ou contentores com o cluster do HDInsight. File storage do Azure é uma opção de armazenamento de dados conveniente para utilização no nó de extremidade que lhe permite montar uma partilha de ficheiros de armazenamento do Azure, por exemplo, o sistema de ficheiros do Linux. Mas partilhas de ficheiros do Azure podem ser montadas e utilizadas por qualquer sistema com um sistema operativo suportado como Windows ou Linux. 

Quando cria um cluster do Hadoop no HDInsight, especificou um **storage do Azure** conta ou um **arquivo Data Lake**. Um contentor de armazenamento específico da conta de que contém o sistema de ficheiros para o cluster que criou (por exemplo, o sistema de ficheiros distribuído Hadoop). Para obter mais informações e orientações, consulte:

- [Utilizar o armazenamento do Azure com o HDInsight](../hdinsight-hadoop-use-blob-storage.md)
- [Utilizar o Data Lake Store com clusters do HDInsight do Azure](../hdinsight-hadoop-use-data-lake-store.md)

## <a name="use-azure-blob-storage-accounts-with-ml-services-cluster"></a>Utilizar contas de armazenamento de Blobs do Azure com o cluster de serviços de ML

Se tiver especificado mais do que uma conta de armazenamento ao criar o cluster de ML serviços, as instruções seguintes explicam como utilizar uma conta secundária para operações de um cluster de serviços de ML e acesso a dados. Assumir as seguintes contas de armazenamento e o contentor: **storage1** e um contentor predefinido denominada **container1**, e **storage2** com **container2**.

> [!WARNING]
> Por motivos de desempenho, o cluster do HDInsight é criado no mesmo centro de dados como a conta de armazenamento primário que especificar. Não é suportada a utilização de uma conta de armazenamento numa localização diferente que o cluster do HDInsight.

### <a name="use-the-default-storage-with-ml-services-on-hdinsight"></a>Utilizar o armazenamento de predefinido com os serviços de ML no HDInsight

1. Utilizar um cliente SSH, ligar ao nó de extremidade do cluster. Para obter informações sobre como utilizar o SSH com clusters do HDInsight, consulte [utilizar o SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
  
2. Copie um ficheiro de exemplo, mysamplefile.csv, para o diretório de /share. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal mycsv.scv /share  

3. Mudar para o R Studio ou outra consola R e escrever o código de R ao definir o nó de nome para o **predefinido** e a localização do ficheiro que pretende aceder.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Todas as referências de ficheiro e diretório apontam para a conta de armazenamento `wasb://container1@storage1.blob.core.windows.net`. Este é o **predefinido a conta de armazenamento** que associado ao cluster do HDInsight.

### <a name="use-the-additional-storage-with-ml-services-on-hdinsight"></a>Utilizar o armazenamento adicional com os serviços de ML no HDInsight

Agora, suponha que pretende processar um ficheiro chamado mysamplefile1.csv que está localizado no /private diretório de **container2** no **storage2**.

No seu código de R, ponto de referência de nome de nó para o **storage2** conta de armazenamento.

    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mysamplefile1.csv")

Todas as referências de ficheiro e diretório agora apontam para a conta de armazenamento `wasb://container2@storage2.blob.core.windows.net`. Este é o **nome de nó** que tiver especificado.

Tem de configurar o /user RevoShare/<SSH username> diretório **storage2** da seguinte forma:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>

## <a name="use-an-azure-data-lake-store-with-ml-services-cluster"></a>Utilizar um Azure Data Lake Store com o cluster de serviços de ML 

Para utilizar o Data Lake Store com o cluster do HDInsight, terá de conceder o acesso de cluster a cada Azure Data Lake Store que pretende utilizar. Para obter instruções sobre como utilizar o portal do Azure para criar um cluster do HDInsight com uma conta do Azure Data Lake Store, como o armazenamento de predefinido ou como um arquivo de adicional, consulte [criar um cluster do HDInsight com o Data Lake Store utilizando o portal do Azure](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Em seguida, utilizar o arquivo no R script muito como fez uma conta do storage do Azure secundária, tal como descrito no procedimento anterior.

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Adicione o acesso de cluster a Lake os arquivos de dados do Azure
Aceder a um arquivo Data Lake, utilizando um Principal de serviço do Azure Active Directory (Azure AD) associado ao cluster do HDInsight.

1. Quando criar o cluster do HDInsight, selecione **identidade do AAD Cluster** do **origem de dados** separador.

2. No **identidade do AAD Cluster** caixa de diálogo em **selecione Principal de serviço do AD**, selecione **criar nova**.

Depois de dar um nome de Principal de serviço e criar uma palavra-passe para o mesmo, clique em **gerir o acesso ADLS** para associar o Principal de serviço com o Data Lake Store.

Também é possível adicionar o acesso do cluster para uma ou mais contas de Data Lake Store seguir a criação do cluster. Abra a entrada de portal do Azure para uma Data Lake Store e aceda à **Explorador de dados > acesso > Adicionar**. 

### <a name="how-to-access-the-data-lake-store-from-ml-services-on-hdinsight"></a>Como aceder o arquivo Data Lake a partir dos serviços de ML no HDInsight

Assim que tiver dado acesso a um Data Lake Store, pode utilizar o arquivo de cluster de ML serviços no HDInsight a forma como o faria com uma conta do storage do Azure secundário. A única diferença é que o prefixo **wasb: / /** alterações **adl: / /** da seguinte forma:


    # Point to the ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Os comandos seguintes são utilizados para configurar a conta de Data Lake Store com o diretório de RevoShare e adicione o ficheiro. csv de exemplo do exemplo anterior:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/mysamplefile.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-ml-services-on-hdinsight"></a>Utilizar o File storage do Azure com os serviços de ML no HDInsight

Há também uma opção de armazenamento de dados conveniente para utilização no nó de extremidade chamado [ficheiros do Azure] ((https://azure.microsoft.com/services/storage/files/). Permite-lhe montar uma partilha de ficheiros de armazenamento do Azure para o sistema de ficheiros do Linux. Esta opção pode ser útil para armazenar ficheiros de dados, os scripts de R e objetos de resultado que poderão ser necessário posteriormente, especialmente quando faz sentido utilizam o sistema de ficheiros nativo no nó de extremidade em vez de HDFS. 

Uma vantagem principal de ficheiros do Azure é que as partilhas de ficheiros podem ser montadas e utilizadas por qualquer sistema com um SO suportado, tais como o Windows ou Linux. Por exemplo, pode ser utilizado por outro cluster de HDInsight foi, ou alguém na sua equipa, pela VM do Azure ou mesmo um sistema local. Para obter mais informações, consulte:

- [How to use Azure File Storage with Linux (Como utilizar o Armazenamento de Ficheiros do Azure com o Linux)](../../storage/files/storage-how-to-use-files-linux.md)
- [Como utilizar o File storage do Azure no Windows](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral do cluster de ML serviços no HDInsight](r-server-overview.md)
* [Introdução ao cluster de ML serviços do Hadoop](r-server-get-started.md)
* [Opções de contexto do cluster de ML serviços no HDInsight de computação](r-server-compute-contexts.md)

