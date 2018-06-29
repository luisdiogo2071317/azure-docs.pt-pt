---
title: Utilizar pré-visualização do Azure Data Lake armazenamento Gen2 com clusters do HDInsight do Azure
description: Saiba como consultar dados a partir de pré-visualização do Azure Data Lake armazenamento Gen2 e armazenar os resultados da sua análise.
keywords: hdfs, dados estruturados, dados não estruturados, arquivo data lake, entrada de Hadoop, Hadoop saída, armazenamento de hadoop, hdfs entrada, saída hdfs, hdfs armazenamento, wasb do azure
services: hdinsight,storage
documentationcenter: ''
tags: azure-portal
author: jamesbak
manager: jahogg
ms.component: data-lake-storage-gen2
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: 2797c9f18364a2321bea885592690793271d8b8e
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062194"
---
# <a name="use-azure-data-lake-storage-gen2-preview-with-azure-hdinsight-clusters"></a>Utilizar pré-visualização do Azure Data Lake armazenamento Gen2 com clusters do HDInsight do Azure

Para analisar os dados em cluster do HDInsight, pode armazenar os dados em qualquer combinação de armazenamento do Azure, Gen1 de armazenamento do Azure Data Lake ou pré-visualização do Azure Data Lake armazenamento Gen2. Todas as opções de armazenamento permitem-lhe eliminar em segurança os clusters do HDInsight que são utilizados para o cálculo sem que haja perda de dados do utilizador.

O Hadoop suporta uma noção do sistema de ficheiros predefinido. O sistema de ficheiros predefinido implica um esquema e uma autoridade predefinidos. Também pode ser utilizado para resolver caminhos relativos. Durante o processo de criação de cluster do HDInsight, pode especificar um contentor do blob no Storage do Azure ou o armazenamento do Azure Data Lake como sistema de ficheiros predefinido. Em alternativa com o HDInsight 3.5, pode selecionar o armazenamento do Azure ou o armazenamento do Azure Data Lake como o sistema de ficheiros com algumas exceções.

Neste artigo, saiba como funciona o Gen2 de armazenamento do Azure Data Lake com clusters do HDInsight. Para obter mais informações sobre como criar um cluster do HDInsight, consulte [configurar HDInsight clusters com armazenamento do Azure Data Lake com o Hadoop, Spark, Kafka e muito mais](quickstart-create-connect-hdi-cluster.md).

O armazenamento do Azure é uma solução de armazenamento para fins gerais robusta que se integra perfeitamente no HDInsight. HDInsight pode utilizar o armazenamento do Azure Data Lake como o sistema de ficheiros para o cluster. Através de uma interface (HDFS sistema) de ficheiros distribuído Hadoop, o conjunto completo de componentes do HDInsight pode operar diretamente em ficheiros no armazenamento do Azure Data Lake.

Não recomendamos que utilize o sistema de ficheiros para armazenar dados empresariais. Depois de cada Utilize para reduzir o custo de armazenamento é uma boa prática a eliminar o sistema de ficheiros predefinido. Tenha em atenção que contentor predefinido contém aplicações e do sistema registos. Certifique-se de que obtém os registos antes de eliminar o contentor.

A partilha de um sistema de ficheiros em vários clusters não é suportada.

## <a name="hdinsight-storage-architecture"></a>Arquitetura de armazenamento do HDInsight

O diagrama seguinte apresenta uma vista abstrata da arquitetura de armazenamento do HDInsight relativamente à utilização do Armazenamento do Azure:

![Os clusters do Hadoop utilizam a API do HDFS para aceder e armazenar dados estruturados e não estruturados no Armazenamento de Blobs.](./media/use-hdi-cluster/HDI.ABFS.Arch.png "Arquitetura do Armazenamento do HDInsight")

O HDInsight fornece acesso ao sistema de ficheiros distribuído que está ligado localmente aos nós de computação. É possível aceder a este sistema de ficheiros ao utilizar o URI completamente qualificado, por exemplo:

    hdfs://<namenodehost>/<path>

Além disso, o HDInsight permite-lhe aceder a dados armazenados no armazenamento do Azure Data Lake. A sintaxe é:

    abfs[s]://<file_system>@<accountname>.dfs.core.widows.net/<path>

Seguem-se algumas considerações quando utilizar uma conta de armazenamento do Azure com clusters do HDInsight.

* **Os ficheiros nas contas de armazenamento que estão ligados a um cluster** tem o nome da conta e a chave associada ao cluster durante a criação. Esta configuração dá-lhe acesso total aos ficheiros no sistema de ficheiros.

* **Ficheiros públicos em contas de armazenamento que não estão ligados a um cluster** expor permissões só de leitura aos ficheiros no sistema de ficheiros.
  
  > [!NOTE]
  > Sistemas de ficheiros públicos permitem-lhe obter uma lista de todos os ficheiros disponíveis no sistema de ficheiros e metadados de acesso. Sistemas de ficheiros públicos permitem-lhe aceder aos ficheiros apenas se souber o URL exato. Para obter mais informações, consulte [restringir o acesso a contentores e blobs](http://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (as regras para contentores e blobs funcionem mesmo fore ficheiros e o sistema de ficheiros).
 
* **Sistemas de ficheiros privados em contas de armazenamento que não estão ligados a um cluster** não permitir acesso aos ficheiros no sistema de ficheiros, a menos que definir a conta do storage ao submeter as tarefas de WebHCat. Razões para esta restrição são explicados com mais tarde neste artigo.

As contas do Storage definidas durante o processo de criação e as respetivas chaves são armazenadas em %HADOOP_HOME%/conf/core-site.xml nos nós do cluster. O comportamento predefinido do HDInsight é utilizar as contas do Storage definidas no ficheiro core-site.xml. Pode modificar esta definição através do [Ambari](/hdinsight/hdinsight-hadoop-manage-ambari.md)

Várias tarefas de WebHCat, incluindo Hive, MapReduce, transmissão em fluxo do Hadoop e Pig, podem conter uma descrição das contas do Storage e metadados. (Esta abordagem atualmente funciona para o Pig com contas de armazenamento, mas não para os metadados.) Para obter mais informações, consulte [Utilizar um Cluster do HDInsight com Contas do Storage e Metastores Alternativos](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

## <a id="benefits"></a>Vantagens do Armazenamento do Azure

O custo de desempenho implícito de não ter clusters de cálculo e recursos de armazenamento colocalizados é atenuado pela forma como os clusters de cálculo são criados perto dos recursos da conta de armazenamento na região do Azure, onde a rede de alta velocidade permite que os nós de cálculo acedam aos dados de armazenamento do Azure de forma eficiente.

Existem várias vantagens associadas ao armazenamento de dados no armazenamento do Azure em vez do HDFS:

* **Partilha e reutilização de dados:** os dados no HDFS estão localizados dentro do cluster de cálculo. Apenas as aplicações que têm acesso ao cluster de cálculo podem utilizar os dados ao utilizar as APIs do HDFS. É possível aceder aos dados no armazenamento do Azure através das APIs do HDFS ou através das [APIs REST do Armazenamento de Blobs][blob-storage-restAPI]. Assim, pode-se utilizar um conjunto maior de ferramentas e aplicações (incluindo outros clusters do HDInsight) para produzir e consumir dados.
* **Arquivo de dados:** armazenar dados no armazenamento do Azure permite eliminar em segurança os clusters do HDInsight utilizados para o cálculo sem que haja perda de dados do utilizador.
* **Custo do armazenamento de dados:** armazenar dados no DFS a longo prazo é mais dispendioso do que armazenar dados no armazenamento do Azure, uma vez que o custo de um cluster de cálculo é superior ao custo de armazenamento do Azure. Além disso, uma vez que não é necessário recarregar os dados para cada geração de cluster de cálculo, também reduz os custos do carregamento de dados.
* **Aumento horizontal elástico:** embora o HDFS forneça um sistema de ficheiros ampliado horizontalmente, o dimensionamento é determinado pelo número de nós que cria para o cluster. A alteração do dimensionamento pode tornar-se um processo mais complexo do que depender das capacidades de dimensionamento elástico que obtém automaticamente no armazenamento do Azure.
* **Georreplicação:** o armazenamento do Azure pode ser georreplicado. Apesar de esta funcionalidade oferecer recuperação geográfica e redundância de dados, uma ativação pós-falha para a localização georreplicada afeta seriamente o desempenho e pode implicar custos adicionais. Para que seja a recomendação escolher a georreplicação de forma sensata e apenas se o valor dos dados justificar o custo adicional.
* **Gestão de ciclo de vida de dados:** todos os dados em qualquer sistema de ficheiros realiza a sua própria ciclo de vida de que está a ser muito importantes e frequentemente acedidos através de menor valor, inferior acedido através de arquivo ou eliminação. Storage do Azure fornece criação de camadas de dados e o ciclo de vida das políticas de gestão de camada de dados adequadamente para a sua fase do ciclo de vida.

Determinados pacotes e tarefas de MapReduce podem criar resultados intermédios que não quer realmente armazenar no armazenamento do Azure. Nesse caso, pode optar por armazenar os dados no HDFS local. Na verdade, o HDInsight utiliza o DFS para vários destes resultados intermédios nas tarefas do Hive e noutros processos.

> [!NOTE]
> A maioria dos comandos HDFS (por exemplo, `ls`, `copyFromLocal` e `mkdir`) continuarão a funcionar conforme esperado. Apenas os comandos que são específicos da implementação nativa do HDFS (que é conhecida como DFS), tal como `fschk` e `dfsadmin`, Mostrar comportamento diferente no armazenamento do Azure.

## <a name="create-an-data-lake-storage-file-system"></a>Criar um sistema de ficheiros de armazenamento do Data Lake

Para utilizar o sistema de ficheiros, tem primeiro de criar um [conta de armazenamento do Azure][azure-storage-create]. Como parte deste processo, deve especificar uma região do Azure onde será criada a conta de armazenamento. O cluster e a conta do Storage têm de estar alojados na mesma região. A base de dados do SQL Server do metastore do Hive e a base de dados do SQL Server do metastore do Oozie também têm de estar localizadas na mesma região.

Sempre que se encontre, cada blob que criar pertence a um sistema de ficheiros na sua conta de armazenamento do Azure Data Lake. 

O sistema de ficheiros de armazenamento do Data Lake predefinida armazena informações específicas do cluster como o histórico de tarefas e registos. Não partilhe um sistema de ficheiros de armazenamento do Data Lake predefinida com vários clusters do HDInsight. Isto pode danificar o histórico de tarefas. Recomenda-se para utilizar um sistema de ficheiros diferente para cada cluster e colocar os dados partilhados numa conta de armazenamento ligada especificada na implementação de todos os clusters relevantes em vez da conta do storage predefinida. Para obter mais informações sobre como configurar contas do Storage ligadas, consulte [Create HDInsight clusters (Criar clusters do HDInsight)][hdinsight-creation]. No entanto pode reutilizar um sistema de ficheiros de armazenamento predefinido depois do cluster do HDInsight original foi eliminado. Para clusters do HBase, pode manter os dados e o esquema da tabela do HBase ao criar um novo cluster do HBase com o contentor de blobs predefinido utilizado por um cluster do HBase que foi eliminado.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Utilizar o portal do Azure

Quando criar um cluster do HDInsight a partir do Portal, tem as opções (conforme ilustrado na captura de ecrã seguinte) para fornecer os detalhes da conta de armazenamento. Também pode especificar se pretende que uma conta de armazenamento adicional associados com o cluster e, se assim for, escolha a partir de qualquer uma das opções de armazenamento disponível para armazenamento adicional.

![origem de dados de criação do HDInsight hadoop](./media/use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> Não é suportado utilizar uma conta de armazenamento adicional numa localização diferente do cluster do HDInsight.

### <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell

Se lhe [instalado e configurado o Azure PowerShell][powershell-install], pode utilizar o seguinte código na linha de comandos do Azure PowerShell para criar uma conta de armazenamento e um contentor:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "WEST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
      -Name StorageAccountName `
      -Location $Location `
      -SkuName Standard_LRS `
      -Kind StorageV2 
      -HierarchialNamespace $True

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

> [!NOTE]
> Criar um contentor é synonymous com criação de um sistema de ficheiros no armazenamento do Azure Data Lake.

### <a name="use-azure-cli"></a>Utilizar a CLI do Azure

[!INCLUDE [use-latest-version](../../../includes/hdinsight-use-latest-cli.md)]

Se tiver [instalado e configurado a CLI do Azure](../../cli-install-nodejs.md), pode utilizar o seguinte comando para uma conta do Storage e um contentor.

    azure storage account create <storageaccountname> --type LRS --is-hns-enabled true

> [!NOTE]
> Durante a pré-visualização pública do Data Lake armazenamento Gen2 apenas `--type LRS` é suportada. Opções de proporcionar uma redundância adicional deixará de estar disponíveis em todo o programa de pré-visualização.

É-lhe pedido que especifique a região geográfica na qual foi criada a conta de armazenamento. Crie a conta do storage na mesma região que pretende criar o cluster do HDInsight.

Assim que a conta do Storage for criada, utilize o seguinte comando para obter as chaves da conta do Storage:

    azure storage account keys list <storageaccountname>

Para criar um contentor, utilize o seguinte comando:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

> [!NOTE]
> Criar um contentor é synonymous com criação de um sistema de ficheiros no armazenamento do Azure Data Lake.

## <a name="address-files-in-azure-storage"></a>Endereçar ficheiros no armazenamento do Azure

O esquema URI para aceder a ficheiros no armazenamento do Azure a partir do HDInsight é:

    abfs[s]://<FileSystem>@<AccountName>.dfs.core.widows.net/<path>

O esquema URI fornece acesso não encriptado (com o *abfs:* prefixo) e acesso encriptado de SSL (com *abfss*). Recomendamos que utilize *abfss* sempre que possível, mesmo quando aceder aos dados que se encontrem dentro da mesma região no Azure.

O &lt;FileSystem&gt; identifica o caminho do sistema de ficheiros de armazenamento do Azure Data Lake.
O &lt;AccountName&gt; identifica o nome de conta do Storage do Azure. É necessário um nome de domínio completamente qualificado (FQDN).

Se os valores de &lt;FileSystem&gt; nem &lt;AccountName&gt; foram especificadas, o sistema de ficheiros é utilizado. Para os ficheiros no sistema de ficheiros predefinido, pode utilizar um caminho relativo ou um caminho absoluto. Por exemplo, o *hadoop-examples.JAR mapreduce* ficheiro que vem com clusters do HDInsight pode ser referido utilizando um dos seguintes caminhos:

    abfs://myfilesystempath@myaccount.dfs.core.widows.net/example/jars/hadoop-mapreduce-examples.jar
    abfs:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> O nome de ficheiro é *hadoop-examples.jar* nos clusters do HDInsight versões 2.1 e 1.6.

O &lt;path&gt; é o nome do caminho do HDFS do ficheiro ou do diretório.

> [!NOTE]
> Ao trabalhar com ficheiros fora do HDInsight, a maioria dos utilitários não reconhece os ABFS formatar e em vez disso, espera um formato de caminho básico, tal como `example/jars/hadoop-mapreduce-examples.jar`.
> 

## <a name="use-additional-storage-accounts"></a>Utilizar contas de armazenamento adicionais

Ao criar um cluster do HDInsight, especifica a conta de armazenamento do Azure que quer associar ao mesmo. Além desta conta de armazenamento, pode adicionar mais contas de armazenamento da mesma subscrição do Azure ou de diferentes subscrições do Azure durante o processo de criação ou depois de um cluster ter sido criado. Para obter instruções sobre como adicionar mais contas do Storage, consulte [Create HDInsight clusters (Criar clusters do HDInsight)](/hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> Não é suportado utilizar uma conta de armazenamento adicional numa localização diferente do cluster do HDInsight.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a utilizar o armazenamento do Azure compatível com HDFS através do HDInsight. Esta abordagem permite-lhe criar soluções de aquisição de dados dimensionável, longo prazo, arquivo e utilizar o HDInsight para o desbloquear as informações no interior estruturados armazenados e os dados não estruturados.

Para obter mais informações, consulte:

* [O controlador de sistema de ficheiros do ABFS Hadoop para Gen2 de armazenamento do Azure Data Lake](abfs-driver.md)
* [Introdução ao armazenamento do Azure Data Lake](introduction.md)
* [Configurar clusters do HDInsight utilizando o armazenamento do Azure Data Lake com o Hadoop, Spark, Kafka e muito mais](quickstart-create-connect-hdi-cluster.md)
* [Ingestão de dados para utilizar o distcp de armazenamento do Azure Data Lake](use-distcp.md)

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: /hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: /storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/use-hdi-cluster/HDI.PowerShell.BlobCommands.png