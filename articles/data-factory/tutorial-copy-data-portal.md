---
title: Utilizar o portal do Azure para criar um pipeline do Data Factory | Microsoft Docs
description: "Este tutorial disponibiliza instruções passo a passo para utilizar o portal do Azure para criar uma fábrica de dados com um pipeline. O pipeline utiliza a atividade Copy para copiar os dados de um armazenamento de blobs do Azure para uma base de dados SQL do Azure. "
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 424a5ec49018e969edbf90c374a9da7e1d22395d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Copiar dados do Blob do Azure para a Base de Dados SQL do Azure utilizando o Azure Data Factory
Neste tutorial, vai criar uma fábrica de dados com a interface de utilizador (IU) do Azure Data Factory. O pipeline nesta fábrica de dados copia os dados do Armazenamento de Blobs do Azure para a Base de Dados SQL do Azure. O padrão de configuração neste tutorial aplica-se à cópia a partir de um arquivo de dados baseado em ficheiros para um arquivo de dados relacional. Para obter uma lista dos arquivos de dados suportados como origens e sinks, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Se não estiver familiarizado com o Azure Data Factory, veja [Introdução ao Azure Data Factory](introduction.md).
>
> - Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Vai executar os seguintes passos neste tutorial:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Criar um pipeline com uma atividade Copy.
> * Testar a execução do pipeline
> * Acionar o pipeline manualmente
> * Acionar o pipeline com base numa agenda
> * Monitorizar o pipeline e execuções de atividades.

## <a name="prerequisites"></a>Pré-requisitos
* **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.
* **Conta de Armazenamento do Azure**. Utilize o armazenamento de blobs como arquivo de dados de **origem**. Se não tiver uma conta de armazenamento do Azure, veja o artigo [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account) para obter os passos para criar uma.
* **Base de Dados SQL do Azure**. Pode utilizar a base de dados como arquivo de dados **sink**. Se não tiver uma Base de Dados SQL do Azure, veja o artigo [Criar uma base de dados SQL do Azure](../sql-database/sql-database-get-started-portal.md) para obter os passos para criar uma.

### <a name="create-a-blob-and-a-sql-table"></a>Criar um blob e uma tabela SQL

Agora, prepare o Blob do Azure e a Base de Dados SQL do Azure para o tutorial ao efetuar os seguintes passos:

#### <a name="create-a-source-blob"></a>Criar um blob de origem

1. Inicie o Bloco de notas. Copie o texto seguinte e guarde-o como um ficheiro **emp.txt** no disco.

    ```
    John,Doe
    Jane,Doe
    ```

2. Crie um contentor com o nome **adftutorial** no armazenamento de Blobs do Azure. Crie uma pasta com o nome **input** neste contentor. Em seguida, carregue o ficheiro **emp.txt** para a pasta **input**. Utilize o portal do Azure ou ferramentas como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/) para realizar estas tarefas.

#### <a name="create-a-sink-sql-table"></a>Criar uma tabela SQL sink

1. Utilize o seguinte script SQL para criar a tabela **dbo.emp** na Base de Dados SQL do Azure.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Permita que os serviços do Azure acedam ao servidor SQL. Certifique-se de que a definição **Permitir acesso aos serviços do Azure** está **ATIVADA** para o servidor SQL do Azure para que o serviço Data Factory possa escrever dados no servidor SQL do Azure. Para verificar e ativar desta definição, execute os passos seguintes:

    1. Clique no hub **Mais serviços** à esquerda e clique em **Servidores SQL**.
    2. Selecione o seu servidor e clique em **Firewall** em **DEFINIÇÕES**.
    3. Na página **Definições de firewall**, clique em **ATIVAR** para **Permitir acesso aos serviços do Azure**.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Neste passo, vai criar uma fábrica de dados e iniciar a IU do Azure Data Factory para criar um pipeline nessa fábrica. 

1. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Página Nova fábrica de dados](./media/tutorial-copy-data-portal/name-not-available-error.png)
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
        Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Só aparecem na lista pendente as localizações que são suportadas. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras regiões.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.      
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-copy-data-portal/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-copy-data-portal/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Azure Data Factory num separador à parte.

## <a name="create-a-pipeline"></a>Criar um pipeline
Neste passo, vai criar um pipeline com uma Atividade Copy na fábrica de dados. A atividade Copy copia os dados do Armazenamento de Bobs do Azure para a Base de Dados SQL do Azure. No [Tutorial de início rápido](quickstart-create-data-factory-portal.md), seguiu os passos abaixo para criar um pipeline:

1. Criar o serviço ligado. 
2. Criar os conjuntos de dados de entrada e saída.
3. Por fim, criar um pipeline.

Neste tutorial, começa por criar o pipeline e, depois, vai criar serviços ligados e conjuntos de dados quando precisar dos mesmos para configurar o pipeline. 

1. Na página de introdução, clique no mosaico **Criar Pipeline**. 

   ![Mosaico Criar pipeline](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
3. Na janela **Propriedades** do pipeline, defina o **nome** do mesmo como **CopyPipeline**.

    ![Nome do pipeline](./media/tutorial-copy-data-portal/pipeline-name.png)
4. Na caixa de ferramentas **Atividades**, expanda a categoria **Fluxo de Dados** e arraste e largue a atividade **Copy** da caixa de ferramentas para a superfície do estruturador do pipeline. 

    ![Arrastar e largar a atividade Copy](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
5. No separador **Geral** da janela **Propriedades**, especifique **CopyFromBlobToSql** como o nome da atividade.

    ![Nome da atividade](./media/tutorial-copy-data-portal/activity-name.png)
6. Mude para o separador **Origem**. Clique em **+ Novo** para criar um conjunto de dados de origem. 

    ![Menu Novo conjunto de dados de origem](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
7. Na janela **Novo Conjunto de Dados**, selecione **Armazenamento de Blobs do Azure** e clique em **Concluir**. A origem de dados está num Armazenamento de Blobs do Azure, pelo que vai selecionar o Armazenamento de Blobs do Azure para o conjunto de dados de origem. 

    ![Selecionar Armazenamento de Blobs do Azure](./media/tutorial-copy-data-portal/select-azure-storage.png)
8. Verá um **separador** novo aberto na aplicação com o título **AzureBlob1**.

    ![Separador Azure Blob1 ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
9. No separador **Geral** da janela **Propriedades**, na parte inferior, especifique **SourceBlobDataset** no **nome**.

    ![Nome do conjunto de dados](./media/tutorial-copy-data-portal/dataset-name.png)
10. Mude para o separador **Ligação**, na janela Propriedades.   

    ![Separador Ligação](./media/tutorial-copy-data-portal/source-dataset-connection-tab.png)
11. Clique em **+ Novo**, junto à caixa de texto **Serviço ligado**. Os serviços ligados ligam um arquivo de dados ou uma computação à fábrica de dados. Neste caso, vai criar um serviço ligado do Armazenamento do Azure para ligar a sua conta de Armazenamento do Azure ao arquivo de dados. O serviço ligado tem as informações de ligação que o serviço Data Factory utiliza para se ligar ao armazenamento de blobs no runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) que contém os dados de origem. 

    ![Botão Novo serviço ligado](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
12. Na janela **Novo Serviço Ligado**, siga os passos abaixo: 

    1. Especifique **AzureStorageLinkedService** no campo **Nome**. 
    2. Selecione a sua conta de Armazenamento do Azure no campo **Nome da conta de Armazenamento**.
    3. Clique em **Testar ligação** para testar a ligação à conta de Armazenamento do Azure.  
    4. Clique em **Guardar** para guardar o serviço ligado.

        ![Novo Serviço Ligado do Armazenamento do Azure](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
13. Clique em **Procurar** no campo **Caminho do ficheiro**.  

    ![Botão Procurar para o ficheiro](./media/tutorial-copy-data-portal/file-browse-button.png)
14. Navegue para a pasta **adftutorial/input**, selecione o ficheiro **emp.txt** e clique em **Concluir**. Em alternativa, pode fazer duplo clique no emp.txt. 

    ![Selecionar o ficheiro de entrada](./media/tutorial-copy-data-portal/select-input-file.png)
15. Confirme que o **Formato de ficheiro** está definido como **Formato de texto** e que o **delimitador de colunas** está definido como **Vírgula (`,`)**. Se o ficheiro de origem utilizar delimitadores de linhas e colunas diferentes, pode clicar em **Detetar o Formato de Texto** no campo **Formato de ficheiro**. A ferramenta Copiar Dados deteta o formato de ficheiro e os delimitadores automaticamente por si. Contudo, pode substituir estes valores. Pode clicar em **Pré-visualizar dados** para pré-visualizar os dados nesta página.

    ![Detetar o formato de texto](./media/tutorial-copy-data-portal/detect-text-format.png)
17. Mude para o separador **Esquema**, na janela Propriedades e clique em **Importar Esquema**. Repare que a aplicação detetou duas colunas no ficheiro de origem. Está a importar o esquema aqui para que possa mapear colunas do arquivo de dados de origem para o arquivo de dados de sink. Se não precisar de mapear colunas, pode ignorar este passo. Neste tutorial, importe o esquema.

    ![Detetar o esquema de origem](./media/tutorial-copy-data-portal/detect-source-schema.png)  
19. Agora, mude para o **separador com o pipeline** ou clique no pipeline na **vista de árvore**, no lado esquerdo.  

    ![Separador do pipeline](./media/tutorial-copy-data-portal/pipeline-tab.png)
20. Confirme que **SourceBlobDataset** está selecionado no campo Conjunto de Dados de Origem, na janela Propriedades. Pode clicar em **Pré-visualizar dados** para pré-visualizar os dados nesta página. 
    
    ![Conjunto de dados de origem](./media/tutorial-copy-data-portal/source-dataset-selected.png)
21. Mude para o separador **Sink** e clique em **Novo** para criar um conjunto de dados de sink. 

    ![Menu conjunto de dados de sink novo](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
22. Na janela **Novo Conjunto de Dados**, selecione **Base de Dados SQL do Azure** e clique em **Concluir**. Neste tutorial, está a copiar dados para uma base de dados SQL do Azure. 

    ![Selecionar a Base de Dados SQL do Azure](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
23. No separador **Geral** da janela Propriedades, defina o nome como **OutputSqlDataset**. 
    
    ![Nome do conjunto de dados de saída](./media/tutorial-copy-data-portal/output-dataset-name.png)
24. Mude para o separador **Ligação** e clique em **Novo** em **Serviço ligado**. Os conjuntos de dados têm de estar associados a um serviço ligado. O serviço ligado tem a cadeia de ligação que o serviço Data Factory utiliza para se ligar à base de dados SQL do Azure no runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) para os quais os dados são copiados. 
    
    ![Botão Novo serviço ligado](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
25. Na janela **Novo Serviço Ligado**, siga os passos abaixo: 

    1. Introduza **AzureSqlDatabaseLinkedService** no campo **Nome**. 
    2. No campo **Nome do servidor**, selecione o seu servidor do SQL do Azure.
    4. No campo **Nome da base de dados**, selecione a sua base de dados SQL do Azure. 
    5. Introduza o nome do utilizador no campo **Nome de utilizador**. 
    6. Introduza a palavra-passe do utilizador no campo **Palavra-passe**. 
    7. Clique em **Testar ligação** para testar a ligação.
    8. Clique em **Guardar** para guardar o serviço ligado. 
    
        ![Serviço ligado da Base de Dados SQL do Azure novo](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

26. Selecione **[dbo].[emp]** em **Tabela**. 

    ![Selecionar a tabela emp](./media/tutorial-copy-data-portal/select-emp-table.png)
27. Mude para o separador **Esquema** e clique em Importar Esquema. 

    ![Importar esquema de destino](./media/tutorial-copy-data-portal/import-destination-schema.png)
28. Selecione a coluna **ID** e clique em **Eliminar**. A coluna ID é uma coluna de identidades na base de dados SQL, pelo que a atividade Copy não tem de inserir dados nessa coluna.

    ![Eliminar a coluna ID](./media/tutorial-copy-data-portal/delete-id-column.png)
30. Mude para o separador com o **pipeline** e confirme que **OutputSqlDataset** está selecionado em **Conjunto de Dado de Sink**.

    ![Separador do pipeline](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
31. Mude para o separador **Mapeamento**, na janela Propriedades, na parte inferior, e clique em **Importar Esquemas**. Repare que as duas primeiras colunas no ficheiro de origem estão mapeadas para os campos **FirstName** e **LastName** na base de dados SQL.

    ![Mapear esquemas](./media/tutorial-copy-data-portal/map-schemas.png)
33. Clique no botão **Validar** para validar o pipeline. Clique na **seta para a direita** para fechar a janela de validação.

    ![Saída da validação do pipeline](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
34. Clique no botão **Código**, no canto direito. Verá o código JSON associado ao pipeline. 

    ![Botão Código](./media/tutorial-copy-data-portal/code-button.png)
35. Verá o código JSON semelhante ao seguinte fragmento:  

    ```json
    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToSql",
                    "type": "Copy",
                    "dependsOn": [],
                    "policy": {
                        "timeout": "7.00:00:00",
                        "retry": 0,
                        "retryIntervalInSeconds": 20
                    },
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "SqlSink",
                            "writeBatchSize": 10000
                        },
                        "enableStaging": false,
                        "parallelCopies": 0,
                        "cloudDataMovementUnits": 0,
                        "translator": {
                            "type": "TabularTranslator",
                            "columnMappings": "Prop_0: FirstName, Prop_1: LastName"
                        }
                    },
                    "inputs": [
                        {
                            "referenceName": "SourceBlobDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "OutputSqlDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ]
                }
            ]
        }
    }
    ```

## <a name="test-run-the-pipeline"></a>Testar a execução do pipeline
Pode testar um pipeline antes de publicar artefactos (serviços ligados, conjuntos de dados e pipeline) no Data Factory (ou) no seu próprio repositório GIT do VSTS. 

1. Para testar a execução do pipeline, clique em **Execução de Testes**, na barra de ferramentas. Verá o estado da execução do pipeline no separador **Saída** da janela na parte inferior. 

    ![Botão Execução de Testes](./media/tutorial-copy-data-portal/test-run-output.png)
2. Confirme que os dados do ficheiro de origem são inseridos na base de dado SQL de destino. 

    ![Verificar a saída do SQL](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. Clique em **Publicar**, no painel do lado esquerdo. Esta ação publica as entidades (serviços ligados, conjuntos de dados e pipeline) que criou anteriormente no Azure Data Factory.

    ![Botão Publicar](./media/tutorial-copy-data-portal/publish-button.png)
4. Aguarde até ver a mensagem **Publicação com êxito**. Para ver mensagens de notificação, clique no separador **Mostrar Notificações**, na barra lateral do lado esquerdo. Clique em **X** para fechar a janela de notificações.

    ![Mostrar notificações](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Configurar o repositório de código
Pode publicar o código associado aos artefactos da sua fábrica de dados num repositório de código do Visual Studio Team System (VSTS). Neste passo, vai criar o repositório de código. 

Se não quiser trabalhar com o repositório de código do VSTS, pode ignorar este passo e continuar a publicação para o Data Factory, conforme fez no passo anterior. 

1. Clique em **Data Factory**, no canto do lado esquerdo (ou) na seta para baixo junto ao mesmo e clique em **Configurar Repositório de Código**. 

    ![Botão Código](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. Na página **Definições do Repositório**, siga os passos abaixo: 
    1. Selecione **GIT do Visual Studio Team Services** no campo **Tipo de Repositório**.
    2. Selecione a sua conta do VSTS no campo **Conta do Visual Studio Team Services**.
    3. Selecione um projeto na sua conta do VSTS no campo **Nome do Projeto**.
    4. Introduza **Tutorial2** no **nome do repositório de Git** que vai ser associado à fábrica de dados. 
    5. Confirme que a opção **Importar os recursos do Data Factory existentes para o repositório** está selecionada. 
    6. Clique em **Guardar** para guardar as definições. 

        ![Definições do repositório](./media/tutorial-copy-data-portal/repository-settings.png)
3. Confirme que **VSTS GIT** está selecionado no repositório.

    ![VSTS GIT selecionado](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. Noutro separador do browser, navegue para o repositório **Tutorial2**, onde verá dois ramos, **master** e **adf_publish**.

    ![Ramos master e adf_publish](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Confirme que os **ficheiros JSON** das entidades do Data Factory estão no ramo **master**.

    ![Ficheiros no ramo master](./media/tutorial-copy-data-portal/master-branch-files.png)
6. Verifique que os **ficheiros JSON** ainda não estão no ramo **adf_publish**. 

    ![Ficheiros no ramo adf_publish](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. Adicione uma **Descrição** para o **pipeline** e clique no botão **Guardar**, na barra de ferramentas. 

    ![Adicionar descrição para o pipeline](./media/tutorial-copy-data-portal/pipeline-description.png)
8. Agora, deverá ver um **ramo** com o seu nome de utilizador no repositório **Tutorial2**. A alteração que fez foi no seu próprio ramo, não no ramo master. Só pode publicar entidades deste último.

    ![O seu ramo](./media/tutorial-copy-data-portal/your-branch.png)
9. Passe o rato por cima do botão **Sincronizar** (mas não clique ainda), selecione a opção **Consolidar Alterações** e clique no botão **Sincronizar** para sincronizar as alterações com o ramo **master**. 

    ![Consolidar e sincronizar as alterações](./media/tutorial-copy-data-portal/commit-and-sync.png)
9. Na janela Sincronizar as alterações, realize as ações abaixo: 

    1. Confirme que **CopyPipeline** é apresentado na lista atualizada de pipelines.
    2. Confirme que **Publicar alterações após a sincronização** está selecionado. Se desmarcar esta opção, vai apenas sincronizar as alterações no seu ramo com o ramo master, não as publica no serviço Data Factory. Pode publicá-las mais tarde, ao clicar no botão **Publicar**. Se selecionar esta opção, as alterações são primeiro sincronizadas com o ramo master e, depois, publicadas no serviço Data Factory.
    3. Clique em **Sincronizar**. 

    ![Janela Sincronizar as alterações](./media/tutorial-copy-data-portal/sync-your-changes.png)
10. Agora, pode ver ficheiros no ramo **adf_publish** ramo do repositório **Tutorial2**. Também pode encontrar o modelo do Azure Resource Manager para a sua solução do Data Factory neste ramo.  

    ![Ficheiros no ramo adf_publish](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>Acionar o pipeline manualmente
Neste passo, vai acionar manualmente o pipeline que publicou no passo anterior. 

1. Clique em **Acionar**, na barra de ferramentas, e clique em **Acionar Agora**. 

    ![Menu Acionar agora](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Mude para o separador **Monitorizar**, no lado esquerdo. Verá uma execução de pipeline que é acionada por um acionador manual. Pode utilizar as ligações na coluna Ações para ver os detalhes das atividades e para voltar a executar o pipeline.

    ![Monitorizar execução do pipeline](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. Para ver as execuções de atividades associadas à execução do pipeline, clique na ligação **Ver Execuções de Atividades**, na coluna **Ações**. Neste exemplo, há apenas uma atividade, pelo que só vai ver uma entrada na lista. Para ver os detalhes da operação de cópia, clique na ligação **Detalhes** (ícone de óculos), na coluna **Ações**. Pode clicar em **Pipelines**, na parte superior, para mudar para a vista **Execuções do Pipeline**. Para atualizar a vista, clique em **Atualizar**.

    ![Ver execuções de atividades](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Verifique se são adicionadas mais duas linhas à tabela **emp** na base de dados SQL do Azure. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Acionar o pipeline com base numa agenda
Nesta agenda, vai criar um acionador de agendador para o pipeline. O acionador executa o pipeline na agenda especificada (hora a hora, diária, etc.). Neste exemplo, o acionador é definido para ser executado a cada minuto até à data/hora final especificada. 

1. Mude para o separador **Editar**, no lado esquerdo. 

    ![Separador Editar](./media/tutorial-copy-data-portal/edit-tab.png)
2. Clique em **Acionador** e selecione **Novo/Editar**. Se o pipeline não estiver ativo, mude para o mesmo. 

    ![Menu novo/editar do acionador](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. Na janela **Adicionar Acionadores**, clique em **Escolher acionador...** e clique em **+ Novo**. 

    ![Adicionar Acionadores - acionador novo](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. Na janela **Novo Acionador**, siga os passos abaixo: 

    1. Defina o **nome** como **RunEveryMinute**.
    2. Selecione **Na Data** em **Fim**. 
    3. Clique na lista pendente para **Fim em**.
    4. Selecione o **dia atual**. Por predefinição, o dia de fim está definido como o dia seguinte. 
    5. Atualize a parte dos **minutos**, de modo a que sejam alguns minutos depois da data/hora atual. O acionador só é ativado depois de publicar as alterações. Por conseguinte, se o definir para apenas poucos minutos de diferença e não o publicar até lá, não verá qualquer execução do acionador.  
    6. Clique em **Aplicar**. 

        ![Definir as propriedades do acionador](./media/tutorial-copy-data-portal/set-trigger-properties.png)
    7. Selecione a opção **Ativado**. Pode utilizar esta caixa de verificação para desativá-lo e ativá-lo mais tarde.
    8. Clique em **Seguinte**.

        ![Acionador ativado - seguinte](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Nenhuma execução de pipeline tem custos associados. Assim, defina a data de fim adequadamente. 
6. Na página **Parâmetros da Execução do Acionador**, reveja o aviso e clique em **Concluir**. O pipeline neste exemplo não tem nenhum parâmetro. 

    ![Parâmetros do pipeline](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
7. Clique em **Publicar** para publicar as alterações ao repositório. O acionador não é efetivamente ativado enquanto a publicação não for bem-sucedida. 

    ![Publicar o acionador](./media/tutorial-copy-data-portal/publish-trigger.png) 
8. Mude para o separador **Monitorizar**, do lado esquerdo, para ver as execuções do pipeline acionadas. 

    ![Execuções de pipeline acionadas](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
9. Para mudar da vista de execuções do pipeline para a vista de execuções do acionador, clique em Execuções do Pipeline e selecione Execuções do Acionador.
    
    ![Menu de execuções do acionador](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
10. Verá as execuções do acionador numa lista. 

    ![Lista de execuções do acionador](./media/tutorial-copy-data-portal/trigger-runs-list.png)
11. Confirme que estão inseridas duas linhas por minuto (em cada execução do pipeline) na tabela **emp** até à hora de fim especificada. 

## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia dados de uma localização para outra localização num armazenamento de blobs do Azure. Aprendeu a: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Criar um pipeline com uma atividade Copy.
> * Testar a execução do pipeline
> * Acionar o pipeline manualmente
> * Acionar o pipeline com base numa agenda
> * Monitorizar o pipeline e execuções de atividades.


Avance para o tutorial seguinte para saber como copiar dados do local para a cloud: 

> [!div class="nextstepaction"]
>[Copiar dados do local para a cloud](tutorial-hybrid-copy-portal.md)
