---
title: "Criar uma fábrica de dados do Azure com a IU do Azure Data Factory | Microsoft Docs"
description: "Este tutorial mostra-lhe como criar uma fábrica de dados com um pipeline que copia dados de uma pasta para outra pasta no Armazenamento de Blobs do Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: ebce4e0d137d2e56cc914efad357593af7abb255
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-data-factory-using-the-azure-data-factory-ui"></a>Criar uma fábrica de dados com a IU do Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versão 2 - Pré-visualização](quickstart-create-data-factory-portal.md)

Este início rápido descreve como utilizar a UI do Azure Data Factory para criar e monitorizar uma fábrica de dados. O pipeline que criar nesta fábrica de dados **copia** dados de uma pasta para outra pasta num armazenamento de blobs do Azure. Para ter acesso a um tutorial sobre como **transformar** dados com o Azure Data Factory, veja [Tutorial: Transformar dados com o Spark](tutorial-transform-data-spark-portal.md). 


> [!NOTE]
> Se não estiver familiarizado com o Azure Data Factory, veja [Introdução ao Azure Data Factory](data-factory-introduction.md) antes de começar o início rápido. 
>
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do Data Factory, que está disponível em geral (GA), veja [Data Factory version 1 - tutorial](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Versão 1 do Data Factory - tutorial).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Vídeo 
Ver este vídeo ajuda-o a compreender a IU do Data Factory: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Navegue para o [portal do Azure](https://portal.azure.com). 
2. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Nome não disponível - erro](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
    Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Só aparecem na lista pendente as localizações que o Data Factory suporta. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras localizações.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
    ![Home page da fábrica de dados](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a aplicação de interface de utilizador (IU) do Azure Data Factory num separador à parte. 
11. Na página de introdução, mude para o separador **Editar**, no painel do lado esquerdo, conforme mostrado na imagem abaixo: 

    ![Página Introdução](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-azure-storage-linked-service"></a>Criar o serviço ligado do Storage do Azure
Neste passo, vai criar um serviço ligado para ligar a sua conta de Armazenamento do Azure à fábrica de dados. O serviço ligado tem as informações de ligação utilizadas pelo serviço Data Factory em runtime para se ligar ao mesmo.

2. Clique em **Ligações** e, em seguida, clique no botão **Novo**, na barra de ferramentas. 

    ![Nova ligação](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
3. Na página **Novo Serviço Ligado**, selecione **Armazenamento de Blobs do Azure** e clique em **Continuar**. 

    ![Selecione o Armazenamento do Azure](./media/quickstart-create-data-factory-portal/select-azure-storage.png)
4. Na página **Novo Serviço Ligado**, siga os passos abaixo: 

    1. Introduza **AzureStorageLinkedService** em **Nome**.
    2. Selecione o nome da sua Conta de Armazenamento do Azure em **Nome da conta de armazenamento**.
    3. Clique em **Testar ligação** para confirmar que o serviço Data Factory se consegue ligar à conta de armazenamento. 
    4. Clique em **Guardar** para guardar o serviço ligado. 

        ![Definições do Serviço Ligado do Armazenamento do Azure](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 
5. Confirme que consegue ver **AzureStorageLinkedService** na lista de serviços ligados. 

    ![Serviço ligado do Armazenamento do Azure na lista](./media/quickstart-create-data-factory-portal/azure-storage-linked-service-in-list.png)

## <a name="create-datasets"></a>Criar conjuntos de dados
Neste passo, vai criar dois conjuntos de dados, **InputDataset** e **OutputDataset**. Estes conjuntos de dados são do tipo **AzureBlob**. Dizem respeito ao **serviço ligado do Armazenamento do Azure** que criou no passo anterior. 

O conjunto de dados de entrada representa a origem de dados na pasta de entrada. Na definição do conjunto de dados de entrada, vai especificar o contentor de blobs (**adftutorial**), a pasta (**input**) e o ficheiro (**emp.txt**) que contém os dados de origem. 

O conjunto de dados de saída representa os dados que são copiados para o destino. Na definição do conjunto de dados de saída, vai especificar o contentor de blobs (**adftutorial**), a pasta (**output**) e o ficheiro para o qual os dados vão ser copiados. Cada execução de um pipeline tem um ID exclusivo associado à mesma, que pode ser acedido com a variável de sistema **RunId**. O nome do ficheiro de saída é avaliado dinamicamente com base no ID da execução do pipeline.   

Nas definições do serviço ligado, vai especificar a conta de Armazenamento do Azure que contém os dados de origem. Nas definições do conjunto de dados de origem, vai especificar onde é que os dados de origem residem exatamente (contentor de blobs, pasta e ficheiro). Nas definições do conjunto de dados de sink, vai especificar para onde é que os dados vão ser copiados (contentor de blobs, pasta e ficheiro). 
 
1. Clique no botão **+ (mais)** e selecione **Conjunto de Dados**.

    ![Menu Novo conjunto de dados](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
2. Na página **Novo Conjunto de Dados**, selecione **Armazenamento de Blobs do Azure** e clique em **Concluir**. 

    ![Selecionar Armazenamento de Blobs do Azure](./media/quickstart-create-data-factory-portal/select-azure-blob-storage.png)
3. Na janela **Propriedades** do conjunto de dados, introduza **InputDataset** em **Nome**. 

    ![Definições gerais do conjunto de dados](./media/quickstart-create-data-factory-portal/dataset-general-page.png)
4. Mude para o separador **Ligação** e siga os passos abaixo: 

    1. Selecione **AzureStorageLinkedService** no serviço ligado. 
    2. Clique no botão **Procurar** do **caminho do ficheiro**. 
        ![Procurar o ficheiro de entrada](./media/quickstart-create-data-factory-portal/file-path-browse-button.png)
    3. Na janela **Escolher um ficheiro ou pasta**, navegue para a pasta **input** do contentor **adftutorial**, selecione **emp.txt** e clique em **Concluir**.

        ![Procurar o ficheiro de entrada](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    4. (Opcional) Clique em **Pré-visualizar os dados** para pré-visualizar os dados no ficheiro emp.txt.     
5. Repita os passos para criar o conjunto de dados de saída.  

    1. Clique no botão **+ (mais)**, no painel do lado esquerdo, e selecione **Conjunto de Dados**.
    2. Na página **Novo Conjunto de Dados**, selecione **Armazenamento de Blobs do Azure** e clique em **Concluir**.
    3. Especifique **OutputDataset** no nome.
    4. Introduza **adftutorial/output** na pasta. A atividade Copy cria a pasta de saída, se esta não existir. 
    5. Introduza `@CONCAT(pipeline().RunId, '.txt')` para o nome do ficheiro. Sempre que executar um pipeline, a execução do mesmo tem um ID exclusivo associado a si. A expressão concatena o ID de execução do pipeline com **.txt** para avaliar o nome do ficheiro de saída. Para obter a lista de variáveis e expressões do sistema suportadas, veja [System variables](control-flow-system-variables.md) (Variáveis do sistema) e [Expression language](control-flow-expression-language-functions.md) (Linguagem de expressões).

        ![Definições do conjunto de dados de saída](./media/quickstart-create-data-factory-portal/output-dataset-settings.png)

## <a name="create-a-pipeline"></a>Criar um pipeline 
Neste passo, vai criar e validar um pipeline com uma atividade **Copy** que utiliza os conjuntos de dados de entrada e saída. A atividade Copy copia dados do ficheiro especificado nas definições do conjunto de dados de entrada para o ficheiro especificado nas definições do conjunto de dados de saída. Se o conjunto de dados de entrada especifica apenas uma pasta (e não o nome do ficheiro), a atividade Copy copia todos os ficheiros na pasta de origem para o destino. 

1. Clique no botão **+ (mais)** e selecione **Pipeline**. 

    ![Menu Novo pipeline](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
2. Especifique **CopyPipeline** em **Nome**, na janela **Propriedades**. 

    ![Definições gerais do pipeline](./media/quickstart-create-data-factory-portal/pipeline-general-settings.png)
3. Na caixa de ferramentas **Atividades**, expanda **Fluxo de Dados** e arraste e largue a atividade **Copy** da caixa de ferramentas **Atividades** para a superfície do estruturador do pipeline. Também pode pesquisar por atividades na caixa de ferramentas **Atividades**. Especifique **CopyFromBlobToBlob** no **nome**.

    ![Definições gerais da atividade Copy](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
4. Mude para o separador **Origem** nas definições da atividade Copy e selecione **InputDataset** no **conjunto de dados de origem**.

    ![Definições de origem da atividade Copy](./media/quickstart-create-data-factory-portal/copy-activity-source-settings.png)    
5. Mude para o separador **Sink** nas definições da atividade Copy e selecione **OutputDataset** no **conjunto de dados de sink**.

    ![Definições de sink da atividade Copy](./media/quickstart-create-data-factory-portal/copy-activity-sink-settings.png)    
7. Clique em **Validar** para validar as definições do pipeline. Confirme que o pipeline foi confirmado com êxito. Para fechar a saída da validação, clique no botão **seta para a direita** (>>). 

    ![Validar o pipeline](./media/quickstart-create-data-factory-portal/pipeline-validate-button.png)

## <a name="test-run-the-pipeline"></a>Testar a execução do pipeline
Neste passo, vai testar a execução do pipeline antes de o implementar no Data Factory. 

1. Na barra de ferramentas do pipeline, clique em **Execução de Testes**. 
    
    ![Execuções de testes do pipeline](./media/quickstart-create-data-factory-portal/pipeline-test-run.png)
2. Confirme se vê o estado da execução do pipeline no separador **Saída** das definições do pipeline. 

    ![Saída da execução de testes](./media/quickstart-create-data-factory-portal/test-run-output.png)    
3. Confirme que vê um ficheiro de saída na pasta **output** do contentor **adftutorial**. Se a pasta de saída não existir, o serviço Data Factory cria-a automaticamente. 
    
    ![Verificar a saída](./media/quickstart-create-data-factory-portal/verify-output.png)


## <a name="trigger-the-pipeline-manually"></a>Acionar o pipeline manualmente
Neste passo, vai implementar entidades (serviços ligados, conjuntos de dados, pipelines) no Azure Data Factory. Em seguida, vai acionar manualmente uma execução de pipeline. Também pode publicar entidades do seu próprio repositório GIT do VSTS, que é abordado [noutro tutorial](tutorial-copy-data-portal.md?#configure-code-repository).

1. Antes de acionar um pipeline, tem publicar entidades no Data Factory. Para publicar, clique em **Publicar**, no painel do lado esquerdo. 

    ![Botão Publicar](./media/quickstart-create-data-factory-portal/publish-button.png)
2. Para acionar o pipeline manualmente, clique em **Acionar**, na barra de ferramentas, e selecione **Acionar Agora**. 
    
    ![Acionar agora](./media/quickstart-create-data-factory-portal/pipeline-trigger-now.png)

## <a name="monitor-the-pipeline"></a>Monitorizar o pipeline

1. Mude para o separador **Monitorizar**, no lado esquerdo. Utilize o botão **Atualizar** para atualizar a lista.

    ![Monitorizar o pipeline](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
2. Clique na ligação **Ver Execuções de Atividades**, em **Ações**. Vai ver o estado da execução da atividade Copy nesta página. 

    ![Execuções de atividades do pipeline](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
3. Para ver os detalhes da operação de cópia, clique na ligação **Detalhes** (imagem de óculos), na coluna **Ações** Para obter os detalhes das propriedades, veja [Copy Activity overview](copy-activity-overview.md) (Descrição geral da Atividade Copy). 

    ![Detalhes da operação de cópia](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
4. Confirme que vê um ficheiro novo na pasta **output**. 
5. Pode clicar na ligação **Pipelines** para regressar à vista **Execuções do Pipeline** a partir da vista **Execuções de Atividades**. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Acionar o pipeline com base numa agenda
Este passo é opcional neste tutorial. Pode criar um **acionador de agenda** para agendar a execução do pipeline periodicamente (hora a hora, diariamente, etc.). Neste passo, vai criar um acionador para ser executado a cada minuto até à data/hora que indicar como a data de fim. 

1. Mude para o separador **Editar**. 

    ![Mudar para o separador Editar](./media/quickstart-create-data-factory-portal/switch-edit-tab.png)
1. Clique em **Acionador**, no menu, e clique em **Novo/Editar**. 

    ![Menu Novo acionador](./media/quickstart-create-data-factory-portal/new-trigger-menu.png)
2. Na página **Adicionar Acionadores**, clique em **Escolher acionador...** e clique em **Novo**. 

    ![Adicionar acionadores - acionador novo](./media/quickstart-create-data-factory-portal/add-trigger-new-button.png)
3. Na página **Novo Acionador**, no campo **Fim** campo, selecione **Na Data**, especifique a hora de fim para alguns minutos depois da hora atual e clique em **Aplicar**. Existe um custo associado a cada execução de pipeline, pelo que deve especificar a hora de fim apenas para alguns minutos após a hora de início. Confirme que estão marcadas para o mesmo dia. No entanto, certifique-se de que há tempo suficiente para o pipeline ser executado entre a hora de publicação e a hora de fim. O acionador só entra em vigor depois de publicar a solução no Data Factory e não quando guarda o acionador na IU. 

    ![Definições do acionador](./media/quickstart-create-data-factory-portal/trigger-settings.png)
4. Selecione a opção **Ativado**, na página **Novo Acionador**, e clique em **Seguinte** 

    ![Definições do acionador - botão seguinte](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
5. Na página **Novo Acionador**, reveja a mensagem de aviso e clique em **Concluir**.

    ![Definições do acionador - botão concluir](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
6. Clique em **Publicar** para publicar as alterações no Data Factory. 

    ![Botão Publicar](./media/quickstart-create-data-factory-portal/publish-2.png)
8. Mude para o separador **Monitorizar**, no lado esquerdo. Clique em **Atualizar** para atualizar a lista. Pode ver que o pipeline é executado uma vez a cada minuto desde a hora de publicação até à hora de fim. Repare nos valores na coluna **Acionado Por**. A execução do acionador manual provém do passo (**Acionar Agora**) que realizou anteriormente. 

    ![Monitorizar execuções acionadas](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
9. Clique na seta para baixo junto a **Execuções do Pipeline** para mudar para a vista **Execuções Acionadas**. 

    ![Monitorizar execuções acionadas](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
10. Confirme que é criado um **ficheiro de saída** para todas as execuções de pipelines até à data/hora de fim especificada na pasta **output**. 

## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia dados de uma localização para outra localização num armazenamento de blobs do Azure. Leia os [tutoriais](tutorial-copy-data-portal.md) para saber como utilizar o Data Factory em mais cenários. 