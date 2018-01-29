---
title: "Computação ambientes suportados pelo Azure Data Factory | Microsoft Docs"
description: "Saiba mais sobre os ambientes de computação que pode utilizar em pipelines do Azure Data Factory (por exemplo, o Azure HDInsight) para dados de transformação ou processo."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 410fb74d8f8ec6196bbd4cc19cc97704649b75c9
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Suportado pelo Azure Data Factory de ambientes de computação
> [!NOTE]
> Este artigo aplica-se à versão 1 do Azure Data Factory, que está geralmente disponível (GA). Se estiver a utilizar a versão 2 do serviço do Data Factory, o que está em pré-visualização, consulte [versão 2 serviços ligados de computação](../compute-linked-services.md).

Este artigo explica os ambientes de computação que pode utilizar para o processo ou de transformação de dados. Também fornece detalhes sobre as configurações diferentes (a pedido versus traga a sua própria) que suporta a fábrica de dados quando configurar serviços ligados com ligação estes computação ambientes para um Azure data factory.

A tabela seguinte fornece uma lista de ambientes de computação que são suportados pela fábrica de dados e as atividades que podem ser executadas nos mesmos. 

| Ambiente de computação                      | Atividades                               |
| ---------------------------------------- | ---------------------------------------- |
| [Cluster de Azure HDInsight a pedido](#azure-hdinsight-on-demand-linked-service) ou [seu próprio cluster do HDInsight](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [transmissão em fluxo do Hadoop](data-factory-hadoop-streaming-activity.md) |
| [O Azure Batch](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Atividades de Machine Learning: Execução de Lotes e Atualizar Recurso](data-factory-azure-ml-batch-execution-activity.md) |
| [Análise do Azure Data Lake](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Atividade de Procedimento Armazenado](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Versões do HDInsight suportadas na fábrica de dados
O Azure HDInsight suporta várias versões de cluster de Hadoop que pode implementar em qualquer altura. Cada versão suportada cria uma versão específica da distribuição Hortonworks Data Platform (HDP) e um conjunto de componentes da distribuição. 

Microsoft atualiza a lista de versões suportadas do HDInsight com as mais recentes componentes ecossistema do Hadoop e correções. Para obter informações detalhadas, consulte [as versões do HDInsight suportado](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> HDInsight baseado em Linux versão 3.3 foi extinguida 31 de Julho de 2017. Fábrica de dados versão 1 HDInsight a pedido ligado serviços aos clientes foram fornecidos até 15 de Dezembro de 2017, testar e atualizar para uma versão posterior do HDInsight. Será descontinuado 31 de Julho de 2018 HDInsight baseado em Windows.
>
> 

### <a name="after-the-retirement-date"></a>Após a data de retirada 

Após 15 de Dezembro de 2017:

- Pode já não é criar HDInsight baseado em Linux versão 3.3 (ou versões anteriores) serviço fábrica de dados versão 1 de ligado a clusters utilizando um HDInsight a pedido. 
- Se o [ **osType** e **versão** propriedades](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) não são ou não explicitamente especificado na definição de JSON para um serviço de ligado de HDInsight a pedido de versão 1 do existente fábrica de dados , o valor predefinido é alterado de **versão = 3.1, osType = Windows** para **versão =\<mais recente versão de predefinida HDI\>(https://docs.microsoft.com/azure/hdinsight/ osType hdinsight-Component-Versioning#hadoop-Components-Available-with-different-hdinsight-Versions), = Linux**.

Após a 31 de Julho de 2018:

- Já não pode criar uma versão de clusters do HDInsight baseados em Windows através da utilização de um serviço de ligado de HDInsight a pedido na versão 1 do Data Factory. 

### <a name="recommended-actions"></a>Ações recomendadas 

- Para garantir que pode utilizar os componentes de ecossistema do Hadoop e correções mais recentes, atualize o [ **osType** e **versão** propriedades](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) de afetados Data Factory versão 1 a pedido Definições de serviço ligado do HDInsight para versões mais recentes do HDInsight baseado em Linux (HDInsight 3.6). 
- Antes de 15 de Dezembro de 2017, teste Data Factory versão 1 do Hive, Pig, o MapReduce e o Hadoop atividades de transmissão em fluxo que referenciam o serviço ligado afetado. Certifique-se de que sejam compatíveis com o novo **osType** e **versão** valores predefinidos (**versão = 3.6**, **osType = Linux**) ou o versão do HDInsight e SO explícitos escreva que estiver a atualizar para. 
  Para saber mais sobre a compatibilidade, consulte [migrar de um cluster do HDInsight baseado em Windows para um cluster baseado em Linux](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) e [quais são os componentes do Hadoop e as versões disponíveis com o HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions). 
- Para continuar a utilizar um serviço ligado de HDInsight do Data Factory versão 1 a pedido para criar clusters do HDInsight baseados em Windows, defina explicitamente **osType** para **Windows** antes de 15 de Dezembro de 2017. Recomendamos que migre para clusters do HDInsight baseado em Linux antes de 31 de Julho de 2018. 
- Se estiver a utilizar um serviço de ligado de HDInsight a pedido para executar a versão 1 do Data Factory DotNet personalizada atividade, atualização de definição do JSON de atividade do DotNet personalizada em vez disso, utilizar do Azure Batch ligada serviço. Para obter mais informações, consulte [utilizar atividades personalizadas num pipeline Data Factory](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities). 

> [!Note]
> Se utilizar existentes, traga a sua própria cluster do HDInsight ligado dispositivo versão 1 de fábrica de dados ou um serviço ligado de HDInsight traga a sua própria e a pedido no Azure Data Factory versão 2, nenhuma ação é necessário. Os cenários, a política de suporte da versão mais recente de clusters do HDInsight já é imposta. 
>
> 


## <a name="on-demand-compute-environment"></a>Ambiente de computação a pedido
Uma configuração da pedido, o Data Factory gere completamente o ambiente de computação. Fábrica de dados cria automaticamente o ambiente de computação antes de uma tarefa for submetida para o processamento de dados. Quando a tarefa estiver concluída, o Data Factory remove o ambiente de computação. 

Pode criar um serviço ligado para um ambiente de computação a pedido. Utilize o serviço ligado para configurar o ambiente de computação e para controlar as definições de granulares para execução da tarefa, gestão de clusters e bootstrapping ações.

> [!NOTE]
> Atualmente, a configuração a pedido é suportada apenas para os clusters do HDInsight.
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Serviço ligado do Azure HDInsight a pedido
Fábrica de dados pode criar automaticamente um cluster do HDInsight a pedido baseado em Windows ou Linux para o processamento de dados. O cluster é criado na mesma região que a conta de armazenamento associado ao cluster. Utilizar o JSON **linkedServiceName** propriedade para criar o cluster.

Tenha em atenção o seguinte *chave* serviço ligado de pontos de HDInsight a pedido:

* O cluster do HDInsight a pedido não aparece na sua subscrição do Azure. O serviço Data Factory gere o cluster do HDInsight a pedido em seu nome.
* Os registos para tarefas que são executadas num cluster do HDInsight a pedido são copiados para a conta de armazenamento associado ao cluster do HDInsight. Para aceder a estes registos, no portal do Azure, visite o **detalhes da execução da atividade** painel. Para obter mais informações, consulte [monitorizar e gerir pipelines](data-factory-monitor-manage-pipelines.md).
* São-lhe cobrados apenas para o tempo que o cluster do HDInsight está a funcionar e as tarefas em execução.

> [!IMPORTANT]
> Normalmente demora *20 minutos* ou mais para aprovisionar um cluster do HDInsight a pedido.
>
> 

### <a name="example"></a>Exemplo
O JSON seguinte define um serviço ligado do HDInsight baseado em Linux da pedido. Fábrica de dados cria automaticamente um *baseado em Linux* cluster do HDInsight quando processa uma segmentação de dados. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.6",
            "osType": "Linux",
            "clusterSize": 1,
            "timeToLive": "00:05:00",            
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

> [!IMPORTANT]
> O cluster do HDInsight cria um *contentor predefinido* no armazenamento de Blobs do Azure que especificou no JSON **linkedServiceName** propriedade. Por predefinição, o HDInsight não elimina este contentor quando o cluster é eliminado. Num serviço ligado de HDInsight a pedido, um cluster do HDInsight é criado sempre que um setor tiver de ser processado, a menos que exista um cluster existente em direto (**timeToLive**). O cluster é eliminado quando o processamento é concluído. 
>
> Como são processadas mais setores, verá muitos contentores no armazenamento de Blobs. Se não necessitar dos contentores para as tarefas de resolução de problemas, pode querer eliminar contentores para reduzir o custo de armazenamento. Os nomes destes contentores seguem um padrão: `adf<your Data Factory name>-<linked service name>-<date and time>`. Pode utilizar uma ferramenta como o [Explorador de armazenamento do Microsoft](http://storageexplorer.com/) para eliminar contentores no armazenamento de Blobs.
>
> 

### <a name="properties"></a>Propriedades
| Propriedade                     | Descrição                              | Necessário |
| ---------------------------- | ---------------------------------------- | -------- |
| tipo                         | Defina a propriedade de tipo como **HDInsightOnDemand**. | Sim      |
| clusterSize                  | O número de nós de trabalho e dados do cluster. O cluster do HDInsight é criado com 2 nós principais, além do número de nós de trabalho que especificar para esta propriedade. Os nós são de tamanho Standard_D3, com 4 núcleos. Um cluster de nó de trabalho de 4 demora 24 núcleos (4\*4 = 16 núcleos para nós de trabalho plus 2\*4 = 8 núcleos de nós principais). Para obter detalhes sobre a camada de Standard_D3, consulte [clusters do Hadoop baseados em criar Linux no HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). | Sim      |
| timeToLive                   | O tempo de inatividade permitido para o cluster do HDInsight a pedido. Especifica o tempo durante o qual o cluster do HDInsight a pedido permanece alive quando executar uma atividade estiver concluída, se existirem outras tarefas ativas no cluster.<br /><br />Por exemplo, se uma atividade executar o processo demora 6 minutos e **timeToLive** está definido para 5 minutos, permanece o cluster ativo durante 5 minutos após os 6 minutos de processamento a execução da atividade. Se for executada outra atividade executada na janela de 6 minutos, é processada pelo mesmo cluster.<br /><br />Criar um cluster do HDInsight a pedido é uma operação dispendiosa (que pode demorar algum tempo). Utilize esta definição conforme necessário para melhorar o desempenho de uma fábrica de dados através da reutilização de um cluster do HDInsight a pedido.<br /><br />Se definir o **timeToLive** valor **0**, o cluster é eliminado, assim que a atividade executada depois de concluída. No entanto, se definir um valor elevado, o cluster poderão permanecer inativo, desnecessariamente, resultando em custos elevados. É importante definir o valor adequado com base nas suas necessidades.<br /><br />Se o **timeToLive** valor está definido corretamente, pipelines vários podem partilhar a instância de cluster do HDInsight a pedido. | Sim      |
| versão                      | A versão do cluster do HDInsight. Para as versões do HDInsight permitidas, consulte [as versões do HDInsight suportado](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). Se este valor não está especificado, o [mais recente versão de predefinida HDI](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions) é utilizado. | Não       |
| linkedServiceName            | O serviço ligado do Storage do Azure a ser utilizado pelo cluster a pedido para armazenar e processar dados. O cluster do HDInsight é criado na mesma região que esta conta de armazenamento.<p>Atualmente, não é possível criar um cluster do HDInsight a pedido que utiliza o Azure Data Lake Store como o armazenamento. Se pretende armazenar os dados de resultado do HDInsight em processamento no Data Lake Store, utilize a atividade de cópia para copiar os dados a partir do Blob storage para o Data Lake Store. </p> | Sim      |
| additionalLinkedServiceNames | Especifica as contas de armazenamento adicional para o serviço ligado de HDInsight. Fábrica de dados regista as contas de armazenamento em seu nome. Estas contas de armazenamento tem de ser na mesma região que o cluster do HDInsight. O cluster do HDInsight é criado na mesma região que a conta de armazenamento que é especificada pelo **linkedServiceName** propriedade. | Não       |
| osType                       | O tipo de sistema operativo. Valores permitidos são **Linux** e **Windows**. Se este valor não está especificado, **Linux** é utilizado.  <br /><br />Recomendamos vivamente a utilização de clusters do HDInsight baseado em Linux. A data de retirada para o HDInsight no Windows é de 31 de Julho de 2018. | Não       |
| hcatalogLinkedServiceName    | O nome do serviço ligado SQL do Azure que aponta para a base de dados do HCatalog. O cluster do HDInsight a pedido é criado utilizando a base de dados do SQL Server do metastore. | Não       |

#### <a name="example-linkedservicenames-json"></a>Example: LinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Propriedades avançadas
Granulares configuração de cluster do HDInsight a pedido, pode especificar as seguintes propriedades:

| Propriedade               | Descrição                              | Necessário |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Especifica os parâmetros de configuração de núcleos (Core-site.xml) para o cluster do HDInsight a ser criado. | Não       |
| hBaseConfiguration     | Especifica os parâmetros de configuração de HBase (hbase-site.xml) para o cluster do HDInsight. | Não       |
| hdfsConfiguration      | Especifica os parâmetros de configuração do HDFS (hdfs site.xml) para o cluster do HDInsight. | Não       |
| hiveConfiguration      | Especifica os parâmetros de configuração do ramo de registo (hive site.xml) para o cluster do HDInsight. | Não       |
| mapReduceConfiguration | Especifica os parâmetros de configuração do MapReduce (mapred site.xml) para o cluster do HDInsight. | Não       |
| oozieConfiguration     | Especifica os parâmetros de configuração do Oozie (oozie site.xml) para o cluster do HDInsight. | Não       |
| stormConfiguration     | Especifica os parâmetros de configuração de Storm (storm-site.xml) para o cluster do HDInsight. | Não       |
| yarnConfiguration      | Especifica os parâmetros de configuração YARN (yarn site.xml) para o cluster do HDInsight. | Não       |

#### <a name="example-on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Exemplo: Configuração de cluster do HDInsight a pedido com propriedades avançadas

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "version": "3.6",
      "osType": "Linux",
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Tamanhos de nó
Para especificar o tamanho de nós de ZooKeeper, dados e head, utilize as seguintes propriedades: 

| Propriedade          | Descrição                              | Necessário |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Define o tamanho de nó principal. O valor predefinido é **Standard_D3**. Para obter mais informações, consulte [especifique tamanhos de nó](#specify-node-sizes). | Não       |
| dataNodeSize      | Define o tamanho do nó de dados. O valor predefinido é **Standard_D3**. | Não       |
| zookeeperNodeSize | Define o tamanho do nó ZooKeeper. O valor predefinido é **Standard_D3**. | Não       |

#### <a name="specify-node-sizes"></a>Especifique os tamanhos de nó
Para os valores de cadeia que tem de especificar para as propriedades descritas na secção anterior, consulte [tamanhos de Máquina Virtual](../../virtual-machines/linux/sizes.md). Os valores têm de estar em conformidade com os cmdlets e APIs referenciado no [tamanhos de Máquina Virtual](../../virtual-machines/linux/sizes.md). O tamanho de nó de dados de grande (predefinição) tem 7 GB de memória. Isto poderá não ser suficiente para o seu cenário. 

Se pretender criar nós principais D4 tamanho e nós de trabalho, especifique **Standard_D4** como o valor para o **headNodeSize** e **dataNodeSize** propriedades: 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Se definir um valor incorreto para estas propriedades, poderá ver a mensagem seguinte:

  Falha ao criar o cluster. Exceção: não foi possível concluir a operação de criação do cluster. A operação falhou com o código "400". Estado do cluster não concluído: "Erro". Message: 'PreClusterCreationValidationFailure'. 
  
Se vir esta mensagem, certifique-se de que está a utilizar o cmdlet e os nomes de API da tabela no [tamanhos de Máquina Virtual](../../virtual-machines/linux/sizes.md).  

> [!NOTE]
> Atualmente, a fábrica de dados não suporta clusters do HDInsight que utilizam o Data Lake Store como o arquivo de primário. Utilize o Storage do Azure como o arquivo de principal para clusters do HDInsight. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Traga a sua própria ambiente de computação
Pode registar um ambiente de computação existente como um serviço ligado no Factory de dados. Gerir o ambiente de computação. O serviço fábrica de dados utiliza o ambiente de computação para executar atividades.

Este tipo de configuração é suportado para os ambientes de computação seguintes:

* O Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Servidor de SQL Server de base de dados do armazém de dados SQL do Azure, SQL do Azure

## <a name="azure-hdinsight-linked-service"></a>Serviço ligado de HDInsight do Azure
Pode criar um serviço ligado do HDInsight para registar o seu próprio cluster do HDInsight com o Data Factory.

### <a name="example"></a>Exemplo

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Propriedades
| Propriedade          | Descrição                              | Necessário |
| ----------------- | ---------------------------------------- | -------- |
| tipo              | Defina a propriedade de tipo como **HDInsight**. | Sim      |
| clusterUri        | O URI de cluster do HDInsight.        | Sim      |
| o nome de utilizador          | O nome da conta de utilizador a utilizar para ligar a um cluster do HDInsight existente. | Sim      |
| palavra-passe          | A palavra-passe da conta de utilizador.   | Sim      |
| linkedServiceName | O nome do serviço ligado do storage que se refere ao armazenamento de Blobs do utilizado pelo cluster do HDInsight. <p>Atualmente, não é possível especificar que uma Data Lake Store ligado serviço para esta propriedade. Se o cluster do HDInsight tem acesso ao Data Lake Store, podem aceder a dados no Data Lake Store do Hive ou Pig scripts. </p> | Sim      |

## <a name="azure-batch-linked-service"></a>Serviço de Batch ligado do Azure
Pode criar um serviço ligado de Batch para registar um conjunto do Batch de máquinas virtuais (VMs) para uma fábrica de dados. Pode executar o Microsoft .NET atividades personalizadas ao utilizar o Batch ou para o HDInsight.

Se estiver a utilizar o serviço Batch:

* Saiba mais sobre [Noções básicas do Azure Batch](../../batch/batch-technical-overview.md).
* Saiba mais sobre o [New-AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet. Utilize este cmdlet para criar uma conta do Batch. Em alternativa, pode criar a conta do Batch, utilizando o [portal do Azure](../../batch/batch-account-create-portal.md). Para obter informações detalhadas sobre como utilizar o cmdlet, consulte [utilizando o PowerShell para gerir uma conta do Batch](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx).
* Saiba mais sobre o [New-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet. Utilize este cmdlet para criar um conjunto do Batch.

### <a name="example"></a>Exemplo

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

Para o **accountName** propriedade, acrescentar **.\< nome de região\>**  para o nome da sua conta do batch. Por exemplo:

```json
"accountName": "mybatchaccount.eastus"
```

Outra opção consiste em fornecer o **batchUri** ponto final. Por exemplo:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Propriedades
| Propriedade          | Descrição                              | Necessário |
| ----------------- | ---------------------------------------- | -------- |
| tipo              | Defina a propriedade de tipo como **AzureBatch**. | Sim      |
| accountName       | O nome da conta do Batch.         | Sim      |
| accessKey         | A chave de acesso para a conta de Batch.  | Sim      |
| poolName          | O nome do conjunto de VMs.    | Sim      |
| linkedServiceName | O nome do serviço ligado do storage que está associada a este lote de serviço ligado. Este serviço ligado é utilizado para os ficheiros de testes que são necessários para executar a atividade e para armazenar os registos de execução da atividade. | Sim      |

## <a name="azure-machine-learning-linked-service"></a>Serviço de Machine Learning ligado do Azure
Pode criar um serviço ligado do Machine Learning para registar um lote de Machine Learning a classificação de ponto final de uma fábrica de dados.

### <a name="example"></a>Exemplo

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Propriedades
| Propriedade   | Descrição                              | Necessário |
| ---------- | ---------------------------------------- | -------- |
| Tipo       | Defina a propriedade de tipo como **AzureML**. | Sim      |
| mlEndpoint | O URL de classificação de lote.                   | Sim      |
| ApiKey     | API o modelo de área de trabalho publicado.     | Sim      |

## <a name="azure-data-lake-analytics-linked-service"></a>Serviço do Azure Data Lake Analytics ligado
Pode criar um serviço ligado do Data Lake Analytics para ligar um serviço de computação do Data Lake Analytics a um Azure data factory. A atividade U-SQL do Data Lake Analytics no pipeline refere-se a este serviço ligado. 

A tabela seguinte descreve as propriedades genéricas que são utilizadas na definição de JSON:

| Propriedade                 | Descrição                              | Necessário                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| tipo                 | Defina a propriedade de tipo como **AzureDataLakeAnalytics**. | Sim                                      |
| accountName          | O nome da conta do Data Lake Analytics.  | Sim                                      |
| dataLakeAnalyticsUri | Data Lake Analytics URI.           | Não                                       |
| subscriptionId       | O ID de subscrição do Azure.                    | Não<br /><br />(Se não for especificado, a subscrição da fábrica de dados é utilizada.) |
| resourceGroupName    | O nome do grupo de recursos do Azure.                | Não<br /><br /> (Se não for especificado, o grupo de recursos de fábrica de dados é utilizado.) |

### <a name="authentication-options"></a>Opções de autenticação
Para o seu serviço de análise do Data Lake ligadas, pode escolher entre a autenticação utilizando um principal de serviço ou uma credencial do utilizador.

#### <a name="service-principal-authentication-recommended"></a>Autenticação principal de serviço (recomendada)
Para utilizar a autenticação principal de serviço, registe-se uma entidade de aplicação no Azure Active Directory (Azure AD). Em seguida, conceda acesso do Azure AD para o Data Lake Store. Para obter passos detalhados, consulte [autenticação de serviço a serviço](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Tome nota dos seguintes valores que utilizar para definir o serviço ligado:
* ID da aplicação
* Chave da aplicação 
* ID do inquilino

Utilize a autenticação principal de serviço, especificando as seguintes propriedades:

| Propriedade                | Descrição                              | Necessário |
| :---------------------- | :--------------------------------------- | :------- |
| servicePrincipalId  | ID de cliente. da aplicação     | Sim      |
| servicePrincipalKey | Chave da aplicação.           | Sim      |
| inquilino              | As informações de inquilino (nome ou o inquilino ID de domínio) onde está localizada a sua aplicação. Para obter estas informações, coloque o cursor do rato no canto superior direito do portal do Azure. | Sim      |

**Exemplo: Autenticação principal do serviço**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="user-credential-authentication"></a>Autenticação de credenciais de utilizador
Para a autenticação de credenciais de utilizador para o Data Lake Analytics, especifique as seguintes propriedades:

| Propriedade          | Descrição                              | Necessário |
| :---------------- | :--------------------------------------- | :------- |
| Autorização | No Editor do Data Factory, selecione o **autorizar** botão. Introduza as credenciais que atribui o URL de autorização gerado automaticamente a esta propriedade. | Sim      |
| sessionId     | O ID de sessão OAuth da sessão de autorização do OAuth. Cada ID de sessão é exclusivo e pode ser utilizado apenas uma vez. Esta definição é gerada automaticamente quando utiliza o Editor do Data Factory. | Sim      |

**Exemplo: Autenticação de credenciais de utilizador**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Expiração do token
O código de autorização que gerou selecionando o **autorizar** botão expira após um intervalo de conjunto. 

Poderá ver a seguinte mensagem de erro quando expira o token de autenticação: 

  Erro de operação de credencial: invalid_grant - AADSTS70002: erro ao validar as credenciais. AADSTS70008: A concessão de acesso fornecida está expirada ou revogada. Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z

A tabela seguinte mostra expirações por tipo de conta de utilizador: 

| Tipo de utilizador                                | Expira após                            |
| :--------------------------------------- | :--------------------------------------- |
| As contas de utilizador que são *não* geridas pelo Azure AD (Hotmail, em direto e assim sucessivamente) | 12 horas.                                 |
| Contas de utilizador que *são* geridas pelo Azure AD | executar nos últimos 14 dias após o último setor. <br /><br />90 dias, se um setor que se baseia num serviço ligado com base em OAuth é executado, pelo menos, uma vez a cada 14 dias. |

Para evitar ou resolver este erro, a reautorização selecionando o **autorizar** botão quando o token expira. Em seguida, volte a implementar o serviço ligado. Também pode gerar valores para o **sessionId** e **autorização** propriedades através de programação utilizando o seguinte código:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Para obter mais informações sobre as classes de fábrica de dados que são utilizadas neste exemplo de código, consulte:
* [Classe de AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
* [Classe de AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* [Classe de AuthorizationSessionGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)

Adicione uma referência a Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll para o **WindowsFormsWebAuthenticationDialog** classe. 

## <a name="azure-sql-linked-service"></a>Serviço ligado SQL do Azure
Pode criar um serviço ligado SQL e utilizá-la com o [atividade de procedimento armazenado](data-factory-stored-proc-activity.md) para invocar um procedimento armazenado de um pipeline do Data Factory. Para obter mais informações, consulte [conector do SQL do Azure](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="azure-sql-data-warehouse-linked-service"></a>Serviço ligado do SQL Data Warehouse do Azure
Pode criar um serviço ligado do SQL Data Warehouse e utilizá-la com o [atividade de procedimento armazenado](data-factory-stored-proc-activity.md) para invocar um procedimento armazenado de um pipeline do Data Factory. Para obter mais informações, consulte [conector Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties).

## <a name="sql-server-linked-service"></a>Serviço ligado do SQL Server
Pode criar um serviço ligado do SQL Server e utilizá-la com o [atividade de procedimento armazenado](data-factory-stored-proc-activity.md) para invocar um procedimento armazenado de um pipeline do Data Factory. Para obter mais informações, consulte [conector do SQL Server](data-factory-sqlserver-connector.md#linked-service-properties).

