---
title: Invocar programas de Spark de Azure Data Factory | Microsoft Docs
description: Saiba como invocar programas de Spark de um Azure data factory, utilizando a atividade de MapReduce.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: f03c3b6e275c0bc97df9e687a20acf45956664d2
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Invocar programas de Spark do Azure Data Factory pipelines

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Atividade do ramo de registo](data-factory-hive-activity.md)
> * [Atividade do PIg](data-factory-pig-activity.md)
> * [Atividade de MapReduce](data-factory-map-reduce.md)
> * [Atividade de transmissão em fluxo do Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Atividade do Spark](data-factory-spark.md)
> * [Atividade de execução de lote de aprendizagem máquina](data-factory-azure-ml-batch-execution-activity.md)
> * [Atividade do recurso de atualização de aprendizagem máquina](data-factory-azure-ml-update-resource-activity.md)
> * [Atividade de procedimento armazenado](data-factory-stored-proc-activity.md)
> * [Atividade do Data Lake U-SQL de análise](data-factory-usql-activity.md)
> * [Atividade personalizada do .NET](data-factory-use-custom-activities.md)

> [!NOTE]
> Este artigo aplica-se versão 1 do Azure Data Factory, que é geralmente disponível. Se utilizar a versão 2 do serviço do Data Factory, o que está em pré-visualização, consulte [transformar dados utilizando a atividade do Apache Spark no Data Factory versão 2](../transform-data-using-spark.md).

## <a name="introduction"></a>Introdução
A atividade de Spark é uma do [atividades de transformação de dados](data-factory-data-transformation-activities.md) suportada pela fábrica de dados. Esta atividade executa o programa de Spark especificado no cluster do Spark no Azure HDInsight. 

> [!IMPORTANT]
> - A atividade de Spark não suporta clusters do HDInsight Spark que utilizam o Azure Data Lake Store como armazenamento principal.
> - A atividade de Spark suporta apenas existente (os seus próprios) clusters do HDInsight Spark. Não suporta um serviço de ligado de HDInsight a pedido.

## <a name="walkthrough-create-a-pipeline-with-a-spark-activity"></a>Instruções: Criar um pipeline com uma atividade do Spark
Eis os passos típicos para criar um pipeline de fábrica de dados com uma atividade de Spark: 

* Criar uma fábrica de dados.
* Crie um serviço ligado do Storage do Azure para ligar o seu armazenamento que está associado ao seu cluster do HDInsight Spark à fábrica de dados.
* Crie um serviço ligado do HDInsight para ligar o seu cluster do Spark no HDInsight a fábrica de dados.
* Crie um conjunto de dados refere-se ao serviço ligado de armazenamento. Atualmente, tem de especificar um conjunto de dados de saída para uma atividade, mesmo se não houver nenhuma saída que está a ser produzida. 
* Crie um pipeline com atividade de Spark refere-se ao serviço ligado do HDInsight que criou. A atividade está configurada com o conjunto de dados que criou no passo anterior como um conjunto de dados de saída. O conjunto de dados de saída é o que pauta a agenda (hora a hora, diariamente). Por conseguinte, tem de especificar o conjunto de dados de saída, apesar da atividade realmente não produz uma saída.

### <a name="prerequisites"></a>Pré-requisitos
1. Criar uma conta do storage para fins gerais ao seguir as instruções no [criar uma conta de armazenamento](../../storage/common/storage-create-storage-account.md#create-a-storage-account).

2. Criar um cluster do Spark no HDInsight ao seguir as instruções do tutorial [criar um cluster do Spark no HDInsight](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Associe a conta de armazenamento que criou no passo 1, com este cluster.

3. Transferir e rever o ficheiro de script de Python **test.py** localizado em [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).

4. Carregar **test.py** para o **pyFiles** pasta o **adfspark** contentor de armazenamento de Blobs. Crie o contentor e a pasta, caso não existam.

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Para criar uma fábrica de dados, siga estes passos:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

2. Selecione **novo** > **dados + análise** > **fábrica de dados**.

3. No **nova fábrica de dados** painel, em **nome**, introduza **SparkDF**.

   > [!IMPORTANT]
   > O nome do Azure Data Factory deve ser globalmente exclusivo. Se vir o erro "o nome da fábrica de dados SparkDF não está disponível", altere o nome do data factory. Por exemplo, utilize yournameSparkDFdate e criar a fábrica de dados novamente. Para obter mais informações sobre regras de nomenclatura, consulte [fábrica de dados: regras de nomenclatura](data-factory-naming-rules.md).

4. Em **subscrição**, selecione a subscrição do Azure onde pretende que a fábrica de dados ser criada.

5. Selecione um grupo de recursos existente ou crie um grupo de recursos do Azure.

6. Selecione o **afixar ao dashboard** caixa de verificação.

7. Selecione **Criar**.

   > [!IMPORTANT]
   > Para criar instâncias do Data Factory, tem de ser um membro do [contribuinte da fábrica de dados](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) função ao nível do grupo de recursos/subscrição.

8. Verá a fábrica de dados enquanto está a ser criado no dashboard do portal do Azure.

9. Depois de criar a fábrica de dados, consulte o **fábrica de dados** página, que mostra o conteúdo do data factory. Se não vir o **fábrica de dados** página, selecione o mosaico da fábrica de dados no dashboard.

    ![Painel Data Factory](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Criar serviços ligados
Neste passo, vai criar dois serviços ligados. Um serviço de cluster do Spark contém ligações para a fábrica de dados e o outro serviço liga o armazenamento à fábrica de dados. 

#### <a name="create-a-storage-linked-service"></a>Criar um serviço ligado ao Armazenamento
Neste passo, ligar-se a conta de armazenamento à fábrica de dados. Um conjunto de dados a que criar num passo posterior nestas instruções refere-se a este serviço ligado. O serviço ligado do HDInsight que definem no próximo passo refere-se demasiado para este serviço ligado. 

1. No **fábrica de dados** painel, selecione **autor e implementar**. É apresentado o Editor do Data Factory.

2. Selecione **novo arquivo de dados**e escolha **Storage do Azure**.

   ![Novo arquivo de dados](./media/data-factory-spark/new-data-store-azure-storage-menu.png)

3. O script JSON que utilizar para criar um armazenamento de serviço ligado é apresentado no editor.

   ![AzureStorageLinkedService](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

4. Substitua **nome da conta** e **chave da conta** com a chave de acesso e o nome da conta de armazenamento. Para saber como obter a chave de acesso de armazenamento, consulte como ver, copiar e voltar a gerar chaves de acesso de armazenamento no [gerir a sua conta de armazenamento](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

5. Para implementar o serviço ligado, selecione **implementar** na barra de comandos. Depois de implementar o serviço ligado com êxito, este desaparece na janela rascunho-1. Verá **AzureStorageLinkedService** na vista de árvore à esquerda.

#### <a name="create-an-hdinsight-linked-service"></a>Criar um serviço ligado do HDInsight
Neste passo, vai criar um serviço ligado do HDInsight para ligar o seu cluster do HDInsight Spark à fábrica de dados. O cluster do HDInsight é utilizado para executar o programa de Spark especificado na atividade Spark do pipeline neste exemplo. 

1. No Editor de fábrica de dados, selecione **mais** > **nova computação** > **cluster do HDInsight**.

    ![Criar serviço ligado do HDInsight](media/data-factory-spark/new-hdinsight-linked-service.png)

2. Copie e cole o fragmento seguinte na janela Rascunho-1. No editor de JSON, siga os passos seguintes:

    a. Especifique o URI para o cluster do HDInsight Spark. Por exemplo: `https://<sparkclustername>.azurehdinsight.net/`.

    b. Especifique o nome do utilizador que tem acesso ao cluster do Spark.

    c. Especifique a palavra-passe para o utilizador.

    d. Especifique o serviço ligado do Storage que está associado ao cluster do HDInsight Spark. Neste exemplo, é AzureStorageLinkedService.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - A atividade de Spark não suporta clusters do HDInsight Spark que utilizam o Azure Data Lake Store como armazenamento principal.
    > - A atividade de Spark suporta apenas existente (os seus próprios) clusters do HDInsight Spark. Não suporta um serviço de ligado de HDInsight a pedido.

    Para obter mais informações sobre o serviço ligado do HDInsight, consulte [serviço ligado de HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).

3. Para implementar o serviço ligado, selecione **implementar** na barra de comandos. 

### <a name="create-the-output-dataset"></a>Criar o conjunto de dados de saída
O conjunto de dados de saída é o que pauta a agenda (hora a hora, diariamente). Por conseguinte, tem de especificar um conjunto de dados de saída para a atividade do Spark no pipeline, apesar da atividade não produzir qualquer saída. A especificação de um conjunto de dados de entrada para a atividade é opcional.

1. No Editor de fábrica de dados, selecione **mais** > **novo conjunto de dados** > **Blob storage do Azure**.

2. Copie e cole o fragmento seguinte na janela Rascunho-1. O fragmento JSON define um conjunto de dados denominado **OutputDataset**. Além disso, especifica que os resultados são armazenados no contentor do blob denominado **adfspark** e na pasta denominada **pyFiles/saída**. Como mencionado anteriormente, este conjunto de dados é um conjunto de dados fictício. O programa de Spark neste exemplo não produzir qualquer saída. O **disponibilidade** secção especifica que o conjunto de dados de saída é produzido diariamente. 

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. Para implementar o conjunto de dados, selecione **implementar** na barra de comandos.


### <a name="create-a-pipeline"></a>Criar um pipeline
Neste passo, vai criar um pipeline com uma atividade HDInsightSpark. Atualmente, o conjunto de dados de saída é o que pauta a agenda, pelo que deve criar um conjunto de dados de saída, mesmo se a atividade não produzir qualquer saída. Se a atividade não incluir entradas, pode ignorar a criação do conjunto de dados de entrada. Por conseguinte, nenhum conjunto de dados de entrada é especificado neste exemplo.

1. No Editor de fábrica de dados, selecione **mais** > **novo pipeline**.

2. Substitua o script na janela rascunho-1 com o seguinte script:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Tenha em atenção os seguintes pontos:

    a. O **tipo** propriedade está definida como **HDInsightSpark**.

    b. O **rootPath** propriedade está definida como **adfspark\\pyFiles** onde adfspark é o contentor de blob e pyFiles é a pasta de ficheiros no contentor. Neste exemplo, o blob storage é aquele que está associado um cluster do Spark. Pode carregar o ficheiro para uma conta de armazenamento diferente. Se o fizer, crie um serviço ligado do Storage para ligar essa conta de armazenamento à fábrica de dados. Em seguida, especifique o nome do serviço ligado como um valor para o **sparkJobLinkedService** propriedade. Para obter mais informações sobre esta propriedade e outras propriedades suportadas pela atividade Spark, consulte [Spark propriedades da atividade](#spark-activity-properties).

    c. O **entryFilePath** propriedade está definida como **test.py**, que é o ficheiro de Python.

    d. O **getDebugInfo** propriedade está definida como **sempre**, que significa que os ficheiros de registo são sempre gerada (êxito ou falha).

    > [!IMPORTANT]
    > Recomendamos que não definir esta propriedade como `Always` num ambiente de produção, exceto se estiver a resolver um problema.

    e. O **produz** secção tem um conjunto de dados de saída. Tem de especificar um conjunto de dados de saída, mesmo que o programa de Spark não produzir qualquer saída. O conjunto de dados de saída pauta a agenda para o pipeline (hora a hora, diariamente). 

    Para obter mais informações sobre as propriedades suportadas pela atividade Spark, consulte a secção [Spark propriedades da atividade](#spark-activity-properties).

3. Para implementar o pipeline, selecione **implementar** na barra de comandos.

### <a name="monitor-a-pipeline"></a>Monitorizar um pipeline
1. No **fábrica de dados** painel, selecione **Monitor & Gerir** para iniciar a aplicação de monitorização no outro separador.

    ![Mosaico Monitorizar e Gerir](media/data-factory-spark/monitor-and-manage-tile.png)

2. Alterar o **hora de início** filtro na parte superior para **1/2/2017**e selecione **aplicar**.

3. Surge a janela de apenas uma atividade porque não existe apenas um dia entre o início (2017-02-01) e a hora de fim (2017-02-02) do pipeline. Confirme que o setor de dados está a ser o **pronto** estado.

    ![Monitorizar o pipeline](media/data-factory-spark/monitor-and-manage-app.png)

4. No **windows atividade** lista, selecione uma atividade executar para ver detalhes acerca do mesmo. Se existir um erro, consulte os detalhes acerca do mesmo no painel direito.

### <a name="verify-the-results"></a>Verificar os resultados

1. Inicie o bloco de notas do Jupyter para o cluster do HDInsight Spark acedendo a [este Web site](https://CLUSTERNAME.azurehdinsight.net/jupyter). Também pode abrir um dashboard do cluster para o HDInsight Spark do cluster e, em seguida, inicie o bloco de notas do Jupyter.

2. Selecione **novo** > **PySpark** para iniciar um novo bloco de notas.

    ![Novo bloco de notas Jupyter](media/data-factory-spark/jupyter-new-book.png)

3. Execute o seguinte comando ao copiar e colar o texto e premindo Shift + Enter no fim da segunda instrução:

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Confirme que vê os dados da tabela de AVAC. 

    ![Resultados de consulta do Jupyter](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
Para obter instruções detalhadas, consulte a secção [executar uma consulta de Spark SQL](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). 

### <a name="troubleshooting"></a>Resolução de problemas
Porque definida getDebugInfo como **sempre**, verá uma subpasta do registo na pasta pyFiles no contentor de blob. O ficheiro de registo na pasta de registo fornece informações adicionais. Este ficheiro de registo é especialmente útil quando existe um erro. Num ambiente de produção, deverá configurá-lo para **falha**.

Para resolução de problemas, execute os seguintes passos:


1. Aceda a `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Aplicação de IU do YARN](media/data-factory-spark/yarnui-application.png)

2. Selecione **registos** para um a execução tenta.

    ![Página de aplicações](media/data-factory-spark/yarn-applications.png)

3. Consulte as seguintes informações de erro adicionais na página de registo:

    ![Erro de registo](media/data-factory-spark/yarnui-application-error.png)

As secções seguintes fornecem informações sobre as entidades da fábrica de dados para utilizar um cluster do Spark e a atividade do Spark na fábrica de dados.

## <a name="spark-activity-properties"></a>Propriedades da atividade do Spark
Segue-se a definição de JSON de exemplo de um pipeline com uma atividade de Spark: 

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

A tabela seguinte descreve as propriedades JSON utilizadas na definição de JSON.

| Propriedade | Descrição | Necessário |
| -------- | ----------- | -------- |
| nome | Nome da atividade no pipeline. | Sim |
| descrição | Texto que descreve o que faz a atividade. | Não |
| tipo | Esta propriedade tem de ser definida para HDInsightSpark. | Sim |
| linkedServiceName | Nome do serviço ligado do HDInsight em que executa o programa de Spark. | Sim |
| rootPath | O contentor de blob e a pasta que contém o ficheiro de Spark. O nome de ficheiro é sensível às maiúsculas e minúsculas. | Sim |
| entryFilePath | Caminho relativo para a pasta raiz do Spark/pacote do código. | Sim |
| className | Classe de principal de Java/Spark da aplicação. | Não |
| Argumentos | Uma lista de argumentos da linha de comandos para o programa de Spark. | Não |
| proxyUser | A conta de utilizador para representar a execução do programa de Spark. | Não |
| sparkConfig | Especifique valores para as propriedades de configuração do Spark listados no [configuração Spark: propriedades da aplicação](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Não |
| getDebugInfo | Especifica se os ficheiros de registo do Spark são copiados para o armazenamento utilizado pelo cluster do HDInsight (ou) especificado por sparkJobLinkedService. Valores permitidos são None, sempre ou falha. O valor predefinido é nenhum. | Não |
| sparkJobLinkedService | O armazenamento ligado serviço que detém o Spark registos, dependências e ficheiro de tarefa. Se não especificar um valor para esta propriedade, o armazenamento associado o cluster do HDInsight é utilizado. | Não |

## <a name="folder-structure"></a>Estrutura de pastas
A atividade de Spark não suporta um script inline como Pig e fazer de atividades de ramo de registo. Tarefas do Spark também são mais extensíveis que as tarefas do Pig/Hive. Para tarefas do Spark, pode fornecer múltiplas dependências, tal como jar pacotes (colocadas no java CLASSPATH), ficheiros de Python (colocados a PYTHONPATH) e outros ficheiros.

Crie a seguinte estrutura de pasta no armazenamento de BLOBs referenciado pelo serviço ligado de HDInsight. Em seguida, carregue ficheiros dependentes para as subpastas adequadas na pasta raiz representado pelo **entryFilePath**. Por exemplo, carregar ficheiros do Python para a subpasta pyFiles e jar ficheiros para a subpasta v7 da pasta raiz. Em runtime, o serviço fábrica de dados espera que a seguinte estrutura de pasta no armazenamento de BLOBs: 

| Caminho | Descrição | Necessário | Tipo |
| ---- | ----------- | -------- | ---- |
| . | O caminho da raiz da tarefa do Spark no serviço ligado de armazenamento. | Sim | Pasta |
| &lt;definido pelo utilizador&gt; | O caminho que aponta para o ficheiro de entrada da tarefa de Spark. | Sim | Ficheiro |
| . / jars | Todos os ficheiros desta pasta são carregados e colocados a classpath Java do cluster. | Não | Pasta |
| ./pyFiles | Todos os ficheiros desta pasta são carregados e colocados PYTHONPATH do cluster. | Não | Pasta |
| . / ficheiros | Todos os ficheiros desta pasta são carregados e colocados no diretório de trabalho de executor. | Não | Pasta |
| . / arquiva | Todos os ficheiros desta pasta são descomprimidos. | Não | Pasta |
| . / registos | A pasta onde estão armazenados os registos do cluster do Spark.| Não | Pasta |

Eis um exemplo de armazenamento que contém dois ficheiros de tarefa do Spark no armazenamento de BLOBs referenciada pelo serviço ligado do HDInsight:

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
