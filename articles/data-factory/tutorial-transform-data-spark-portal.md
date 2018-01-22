---
title: Transformar dados com o Spark no Azure Data Factory | Microsoft Docs
description: "Este tutorial fornece instruções passo a passo para transformar dados através da Atividade do Spark no Azure Data Factory."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: shengc
ms.openlocfilehash: 8bd9382ed5a855368533c6bf2305682861c109c0
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Transformar dados na cloud através da atividade do Spark no Azure Data Factory
Neste tutorial, vai utilizar o portal do Azure para criar um pipeline do Data Factory que transforma dados com a Atividade do Spark e um serviço ligado do HDInsight a pedido. Vai executar os seguintes passos neste tutorial:

> [!div class="checklist"]
> * Criar uma fábrica de dados. 
> * Criar um pipeline com uma atividade Spark
> * Acionar uma execução de pipeline
> * Monitorizar a execução do pipeline.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [documentação da versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
* **Conta de Armazenamento do Azure**. Tem de criar um script Python e um ficheiro de entrada, e carregá-los para o armazenamento do Azure. A saída do programa Spark é armazenada nesta conta de armazenamento. O cluster do Spark a pedido utiliza a mesma conta de armazenamento como o respetivo armazenamento primário.  
* **Azure PowerShell**. Siga as instruções em [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/install-azurerm-ps).


### <a name="upload-python-script-to-your-blob-storage-account"></a>Carregar o script Python para a conta de Armazenamento de Blobs
1. Crie um ficheiro Python com o nome **WordCount_Spark.py** com o seguinte conteúdo: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Substitua **&lt;storageAccountName&gt;** pelo nome da sua conta de Armazenamento do Azure. Em seguida, guarde o ficheiro. 
3. No Armazenamento de Blobs do Azure, crie um contentor com o nome **adftutorial**, caso ainda não exista. 
4. Crie uma pasta com o nome **spark**.
5. Crie uma subpasta com o nome **script** na pasta **spark**. 
6. Carregue o ficheiro **WordCount_Spark.py** para a subpasta **script**. 


### <a name="upload-the-input-file"></a>Carregue o ficheiro de entrada
1. Crie um ficheiro com o nome **minecraftstory.txt** com algum texto. O programa Spark conta o número de palavras neste texto. 
2. Crie uma subpasta com o nome `inputfiles` na pasta `spark`. 
3. Carregue o ficheiro `minecraftstory.txt` para a subpasta `inputfiles`. 

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-transform-data-spark-portal/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/tutorial-transform-data-spark-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Nome não disponível - erro](./media/tutorial-transform-data-spark-portal/name-not-available-error.png)
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
      Alguns dos passos deste início rápido pressupõem que utiliza o nome: **ADFTutorialResourceGroup** para o grupo de recursos. Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Atualmente, o Data Factory V2 só permite criar fábricas de dados nas regiões E.U.A. Leste, E.U.A. Leste 2 e Europa Ocidental. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras regiões.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media//tutorial-transform-data-spark-portal/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
    ![Home page da fábrica de dados](./media/tutorial-transform-data-spark-portal/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a aplicação IU do Data Factory num separador à parte.

## <a name="create-linked-services"></a>Criar serviços ligados
Nesta secção, vai criar dois Serviços Ligados: 
    
- Um **Serviço Ligado do Armazenamento do Azure** que liga uma conta de Armazenamento do Azure à fábrica de dados. Este armazenamento é utilizado pelo cluster do HDInsight a pedido. Também contém o script Spark que vai ser executado. 
- Um **Serviço Ligado do HDInsight a Pedido**. O Azure Data Factory cria automaticamente um cluster do HDInsight, executa o programa Spark e, em seguida, elimina o cluster do HDInsight depois de estar inativo durante um período de tempo pré-configurado. 

### <a name="create-an-azure-storage-linked-service"></a>Criar um serviço ligado do Armazenamento do Azure

1. Na página **introdução**, mude para o separador **Editar**, no painel do lado esquerdo, conforme mostrado na imagem abaixo: 

    ![Mosaico Criar pipeline](./media/tutorial-transform-data-spark-portal/get-started-page.png)

2. Clique em **Ligações**, na parte inferior da janela, e clique em **+ Novo**. 

    ![Botão Nova ligação](./media/tutorial-transform-data-spark-portal/new-connection.png)
3. Na janela **Novo Serviço Ligado**, selecione **Armazenamento de Blobs do Azure** e clique em **Continuar**. 

    ![Selecionar Armazenamento de Blobs do Azure](./media/tutorial-transform-data-spark-portal/select-azure-storage.png)
4. Selecione o **nome da conta de armazenamento do Azure** e clique em **Guardar**. 

    ![Especificar a conta de armazenamento de blobs](./media/tutorial-transform-data-spark-portal/new-azure-storage-linked-service.png)


### <a name="create-an-on-demand-hdinsight-linked-service"></a>Crie um serviço ligado do HDInsight a pedido

1. Crie o botão **+ Novo** novamente para criar outro serviço ligado. 
2. Na janela **Novo Serviço Ligado**, selecione **Azure HDInsight** e clique em **Continuar**. 

    ![Selecione Azure HDInsight](./media/tutorial-transform-data-spark-portal/select-azure-hdinsight.png)
2. Na janela **Novo Serviço Ligado**, siga os passos abaixo: 

    1. Introduza **AzureHDInsightLinkedService** em **Nome**.
    2. Confirme que **HDInsight a pedido** está selecionado para **Tipo**.
    3. Selecione **AzureStorage1** para **Serviço Ligado do Storage do Azure**. Criou este serviço ligado anteriormente. Se utilizou um nome diferente, especifique o nome correto para este campo. 
    4. Selecione **spark** para **tipo de Cluster**.
    5. Introduza o **ID de principal de serviço** que tem permissão para criar um cluster do HDInsight. Este principal de serviço tem de ser membro da função de Contribuinte da subscrição ou do Grupo de recursos no qual o cluster é criado. Veja [Criar uma aplicação e um principal de serviço do Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) para obter mais detalhes.
    6. Introduzir a **chave de principal de Serviço**. 
    7. Selecione o mesmo grupo de recursos que utilizou ao criar a fábrica de dados para **Grupo de recursos**. O cluster do Spark é criado neste grupo de recursos. 
    8. Expandir **tipo de SO**.
    9. Introduza um **nome** para o **utilizador** do cluster. 
    10. Introduza a **palavra-passe** do utilizador. 
    11. Clique em **Guardar**. 

        ![Definições do serviço ligado do HDInsight](./media/tutorial-transform-data-spark-portal/azure-hdinsight-linked-service-settings.png)

> [!NOTE]
> O Azure HDInsight tem limitação do número total de núcleos que pode utilizar em cada região do Azure que suporta. Para o Serviço Ligado do HDInsight a Pedido, o cluster do HDInsight será criado na mesma localização do Armazenamento do Azure utilizado como o respetivo armazenamento primário. Certifique-se de que tem um número suficiente de quotas de núcleo para o cluster ser criado com êxito. Para obter mais informações, veja [Configurar clusters no HDInsight com o Hadoop, Spark, Kafka e muito mais](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). 

## <a name="create-a-pipeline"></a>Criar um pipeline

2. Clique no botão + (mais) e, clique em **Pipeline** no menu.

    ![Menu Novo pipeline](./media/tutorial-transform-data-spark-portal/new-pipeline-menu.png)
3. Na caixa de ferramentas **Atividades**, expanda **HDInsight** e arraste e largue a atividade **Spark** da caixa de ferramentas **Atividades** para a superfície do estruturador do pipeline. 

    ![Arrastar e largar atividade Spark](./media/tutorial-transform-data-spark-portal/drag-drop-spark-activity.png)
4. Nas propriedades da janela de atividade do **Spark** na parte inferior, efetue os seguintes passos: 

    1. mudar para o separador **HDI Cluster**.
    2. Selecione **AzureHDInsightLinkedService**, criado no passo anterior. 
        
    ![Especifique um serviço ligado do HDInsight](./media/tutorial-transform-data-spark-portal/select-hdinsight-linked-service.png)
5. Mude para o separador **Scripts/Jar** e siga os passos abaixo: 

    1. Selecione **AzureStorage1** para **Serviço Ligado da Tarefa**.
    2. Clique **Procurar no Armazenamento**. 
    3. Navegue para a **pasta spark/adftutorial/script**, selecione **WordCount_Spark.py** e clique em **Concluir**.      

    ![Especifique o script do Spark](./media/tutorial-transform-data-spark-portal/specify-spark-script.png)
6. Para validar o pipeline, clique no botão **Validar**, na barra de ferramentas. Clique no botão **seta para a direita** (>>) para fechar a janela de validação. 
    
    ![Botão Validar](./media/tutorial-transform-data-spark-portal/validate-button.png)
7. Clique em **Publicar**. O IU do Data Factory publica entidades (serviços ligados e pipeline) para o serviço do Azure Data Factory. 

## <a name="trigger-a-pipeline-run"></a>Acionar uma execução de pipeline
Clique em **Acionar**, na barra de ferramentas, e clique em **Acionar Agora**. 

![Acionar agora](./media/tutorial-transform-data-spark-portal/trigger-now-menu.png)

## <a name="monitor-the-pipeline-run"></a>Monitorizar a execução do pipeline.

1. Mude para o separador **Monitorizar**. Confirme que vê uma execução de pipeline. Demora aproximadamente de 20 minutos para criar um cluster do Spark. 

    ![Monitorizar execuções de pipeline](./media/tutorial-transform-data-spark-portal/monitor-tab.png)
2. Clique em **Atualizar** periodicamente para verificar o estado da execução do pipeline. 

    ![Estado da execução do pipeline](./media/tutorial-transform-data-spark-portal/pipeline-run-succeeded.png)
1. Para ver as execuções de atividades associadas à execução do pipeline, clique na ação **Ver Execuções de Atividades**, na coluna Ações. Pode clicar na ligação **Pipelines**, na parte superior, para regressar à vista de execuções de pipelines.

    ![Vista Execuções de Atividade](./media/tutorial-transform-data-spark-portal/activity-runs.png)

## <a name="verify-the-output"></a>Verificar a saída
Verifique se o ficheiro de saída é criado na pasta spark/otuputfiles/wordcount do contentor adftutorial. 

![Verificar a saída](./media/tutorial-transform-data-spark-portal/verity-output.png)

O ficheiro deve ter cada palavra do ficheiro de texto de entrada e o número de vezes que a palavra apareceu no ficheiro. Por exemplo: 

```
(u'This', 1)
(u'a', 1)
(u'is', 1)
(u'test', 1)
(u'file', 1)
```

## <a name="next-steps"></a>Passos seguintes
O pipeline, neste exemplo, transforma dados através da Atividade do Spark e um serviço de ligado de HDInsight a pedido. Aprendeu a: 

> [!div class="checklist"]
> * Criar uma fábrica de dados. 
> * Criar um pipeline com uma atividade Spark
> * Acionar uma execução de pipeline
> * Monitorizar a execução do pipeline.

Avance para o tutorial seguinte para saber como transformar dados executando o script Hive num cluster do Azure HDInsight que se encontra numa rede virtual. 

> [!div class="nextstepaction"]
> [Tutorial: transformar dados com o Hive na Rede Virtual do Azure](tutorial-transform-data-hive-virtual-network-portal.md).





