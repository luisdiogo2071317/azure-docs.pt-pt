---
title: Utilizar o portal do Azure para criar um pipeline de fábrica de dados | Microsoft Docs
description: Este tutorial disponibiliza instruções passo-a-passo para utilizar o portal do Azure para criar uma fábrica de dados com um pipeline. O pipeline utiliza a atividade para copiar os dados de um Armazenamento de blobs do Azure para uma base de dados SQL.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 34c78a114c1d106c400a94941aa113153383e206
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30173343"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Copiar dados do Armazenamento de blobs do Azure para a base de dados SQL com o Azure Data Factory
Neste tutorial, vai criar uma fábrica de dados com a interface de utilizador (IU) do Azure Data Factory. O pipeline nesta fábrica de dados copia os dados do Armazenamento de blobs do Azure para uma base de dados SQL. O padrão de configuração neste tutorial aplica-se à cópia a partir de um arquivo de dados baseado em ficheiros para um arquivo de dados relacional. Para obter uma lista dos arquivos de dados suportados como origens e sinks, consulte a tabela de [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Se não estiver familiarizado com o Data Factory, veja [Introdução ao Azure Data Factory](introduction.md).
>
> - Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utiliza a versão 1 do Data Factory, que está disponível em geral, veja a [Introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Neste tutorial, vai executar os seguintes passos:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Criar um pipeline com uma atividade de cópia.
> * Testar a execução do pipeline.
> * Acionar o pipeline manualmente.
> * Acionar o pipeline com base numa agenda.
> * Monitorizar o pipeline e execuções de atividades.

## <a name="prerequisites"></a>Pré-requisitos
* **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.
* **Conta de armazenamento do Azure**. Utilize o Armazenamento de blobs como um arquivo dos dados de *origem*. Se não tiver uma conta de armazenamento, veja [Criar uma conta de armazenamento do Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) para seguir os passos para criar uma.
* **Base de Dados SQL do Azure**. Pode utilizar a base de dados como um arquivo de dados *sink*. Se não tiver uma base de dados SQL, veja [Criar uma base de dados SQL](../sql-database/sql-database-get-started-portal.md) para seguir os passos para criar uma.

### <a name="create-a-blob-and-a-sql-table"></a>Criar um blob e uma tabela SQL

Agora, prepare o Armazenamento de blobs e a Base de Dados SQL para o tutorial, ao efetuar os seguintes passos.

#### <a name="create-a-source-blob"></a>Criar um blob de origem

1. Inicie o Bloco de notas. Copie o texto seguinte e guarde-o como um ficheiro **emp.txt** no disco:

    ```
    John,Doe
    Jane,Doe
    ```

2. Crie um contentor com o nome **adftutorial** no Armazenamento de blobs. Crie uma pasta com o nome **input** neste contentor. Em seguida, carregue o ficheiro **emp.txt** para a pasta **input**. Utilize o portal do Azure ou ferramentas como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/) para realizar estas tarefas.

#### <a name="create-a-sink-sql-table"></a>Criar uma tabela SQL sink

1. Utilize o seguinte script SQL para criar a tabela **dbo.emp** na sua base de dados SQL:

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

2. Permita que os serviços do Azure acedam ao SQL Server. Certifique-se de que **Permitir acesso aos serviços do Azure** está **ATIVADO** para o SQL Server, para que o Data Factory possa escrever dados no SQL Server. Para verificar e ativar esta definição, execute estes passos:

    a. No lado esquerdo, selecione **Mais serviços** > **Servidores SQL**.

    b. Selecione o seu servidor e, em **DEFINIÇÕES**, selecione **Firewall**.

    c. Na página **Definições de firewall**, selecione **ATIVADO** em **Permitir acesso aos serviços do Azure**.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Neste passo, vai criar uma fábrica de dados e iniciar a IU do Data Factory para criar um pipeline na fábrica de dados. 

1. Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.
2. No menu da esquerda, selecione **Novo** > **Dados + Análise** > **Data Factory**. 
  
   ![Criação de nova fábrica de dados](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
3. Na página **Nova fábrica de dados**, em **Nome**, introduza **ADFTutorialDataFactory**. 
      
     ![Nova fábrica de dados](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser *globalmente exclusivo*. Se vir a seguinte mensagem de erro no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Para obter as regras de nomenclatura dos artefactos do Data Factory, veja [Regras de nomenclatura do Data Factory](naming-rules.md).
  
   ![Mensagem de erro](./media/tutorial-copy-data-portal/name-not-available-error.png)
4. Selecione a **subscrição** do Azure na qual quer criar a fábrica de dados. 
5. Em **Grupo de Recursos**, efetue um destes passos:
     
    a. Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente.

    b. Selecione **Criar novo** e introduza o nome de um grupo de recursos. 
         
    Para saber mais sobre grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md). 
6. Em **Versão**, selecione **V2 (Pré-visualização)**.
7. Em **Localização**, selecione uma localização para a fábrica de dados. Só aparecem na lista pendente as localizações que são suportadas. Os arquivos de dados (por exemplo, o Armazenamento do Azure e a Base de Dados SQL) e as computações (por exemplo, o Azure HDInsight) que a fábrica de dados utiliza podem estar noutras regiões.
8. Selecione **Afixar ao dashboard**. 
9. Selecione **Criar**. 
10. No dashboard, é apresentado o mosaico seguinte, com o estado **A Implementar o Data Factory**: 

    ![Mosaico A Implementar o Data Factory](media/tutorial-copy-data-portal/deploying-data-factory.png)
11. Depois de concluída a criação, vai ver a página **Fábrica de dados**, conforme mostrado na imagem.
   
    ![Home page da fábrica de dados](./media/tutorial-copy-data-portal/data-factory-home-page.png)
12. Selecione **Criar e Monitorizar** para iniciar a IU do Data Factory num separador à parte.

## <a name="create-a-pipeline"></a>Criar um pipeline
Neste passo, vai criar um pipeline com uma atividade de cópia na fábrica de dados. A atividade de cópia copia os dados do Armazenamento de blobs para a Base de Dados SQL. No [Tutorial de início rápido](quickstart-create-data-factory-portal.md), seguiu os passos abaixo para criar um pipeline:

1. Criar o serviço ligado. 
2. Criar os conjuntos de dados de entrada e saída.
3. Criar um pipeline.

Neste tutorial, vai começar pela criação do pipeline. Em seguida, vai criar serviços ligados e conjuntos de dados quando forem necessários para configurar o pipeline. 

1. Na página **Vamos começar**, selecione **Criar pipeline**. 

   ![Criar pipeline](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
2. Na janela **Propriedades** do pipeline, em **Nome**, introduza **CopyPipeline** como o nome do pipeline.

    ![Nome do pipeline](./media/tutorial-copy-data-portal/pipeline-name.png)
3. Na caixa de ferramentas **Atividades**, expanda a categoria **Fluxo de Dados** e arraste e largue a atividade **Copiar** da caixa de ferramentas para a superfície do estruturador do pipeline. 

    ![Atividade Copiar](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
4. No separador **Geral** da janela **Propriedades**, introduza **CopyFromBlobToSql** como o nome da atividade.

    ![Nome da atividade](./media/tutorial-copy-data-portal/activity-name.png)
5. Vá para o separador **Origem**. Selecione **+ Novo** para criar um conjunto de dados de origem. 

    ![Separador Origem](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
6. Na janela **Novo Conjunto de Dados**, selecione **Armazenamento de Blobs do Azure** e, em seguida, selecione **Concluir**. A origem de dados está num Armazenamento de blobs, pelo que vai selecionar o **Armazenamento de Blobs do Azure** para o conjunto de dados de origem. 

    ![Seleção de armazenamento](./media/tutorial-copy-data-portal/select-azure-storage.png)
7. Verá um separador novo aberto na aplicação, com o título **AzureBlob1**.

    ![Separador AzureBlob1 ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
8. No separador **Geral**, na parte inferior da janela **Propriedades**, em **Nome**, introduza **SourceBlobDataset**.

    ![Nome do conjunto de dados](./media/tutorial-copy-data-portal/dataset-name.png)
9. Vá para o separador **Ligação** da janela **Propriedades**. Junto à caixa de texto **Serviço ligado**, selecione **+ Novo**. 

    Os serviços ligados ligam um arquivo de dados ou uma computação à fábrica de dados. Neste caso, vai criar um serviço ligado de Armazenamento para ligar a sua conta de armazenamento ao arquivo de dados. O serviço ligado tem as informações de ligação que o Data Factory utiliza para ligar ao Armazenamento de blobs em runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) que contém os dados de origem. 

    ![Botão Novo serviço ligado](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
10. Na janela **Novo Serviço Ligado**, siga os passos seguintes: 

    a. Em **Nome**, introduza **AzureStorageLinkedService**. 

    b. Em **Nome da conta de armazenamento**, selecione a conta de armazenamento.

    c. Selecione **Testar ligação** para testar a ligação à conta de armazenamento.

    d. Selecione **Guardar** para guardar o serviço ligado.

    ![Novo serviço ligado](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
11. Junto a **Caminho do ficheiro**, selecione **Procurar**.

    ![Botão Procurar para o caminho do ficheiro](./media/tutorial-copy-data-portal/file-browse-button.png)
12. Vá para a pasta **adftutorial/input**, selecione o ficheiro **emp.txt** e, em seguida, selecione **Concluir**. Em alternativa, pode fazer duplo clique no **emp.txt**. 

    ![Selecionar o ficheiro de entrada](./media/tutorial-copy-data-portal/select-input-file.png)
13. Confirme que o **Formato de ficheiro** está definido como **Formato de texto** e que o **Delimitador de colunas** está definido como **Vírgula (`,`)**. Se o ficheiro de origem utilizar delimitadores de linhas e colunas diferentes, pode selecionar **Detetar o Formato de Texto** para o **Formato de ficheiro**. A ferramenta Copiar Dados deteta o formato de ficheiro e os delimitadores automaticamente por si. Contudo, pode substituir estes valores. Para pré-visualizar os dados nesta página, selecione **Pré-visualizar dados**.

    ![Detetar o formato de texto](./media/tutorial-copy-data-portal/detect-text-format.png)
14. Vá para o separador **Esquema** da janela **Propriedades** e selecione **Importar Esquema**. Repare que a aplicação detetou duas colunas no ficheiro de origem. Está a importar o esquema para aqui, para que possa mapear colunas do arquivo de dados de origem para o arquivo de dados sink. Se não precisar de mapear colunas, pode ignorar este passo. Neste tutorial, importe o esquema.

    ![Detetar o esquema de origem](./media/tutorial-copy-data-portal/detect-source-schema.png)  
15. Agora, vá para o separador com o pipeline ou selecione o pipeline no lado esquerdo.

    ![Separador do pipeline](./media/tutorial-copy-data-portal/pipeline-tab.png)
16. Em **Conjunto de Dados de Origem** da janela **Propriedades**, confirme que **SourceBlobDataset** está selecionado. Para pré-visualizar os dados nesta página, selecione **Pré-visualizar dados**. 
    
    ![Conjunto de dados de origem](./media/tutorial-copy-data-portal/source-dataset-selected.png)
17. Vá para o separador **Sink** e selecione **+ Novo** para criar um conjunto de dados sink. 

    ![Conjunto de dados sink](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
18. Na janela **Novo Conjunto de Dados**, selecione **Base de Dados SQL do Azure** e selecione **Concluir**. Neste tutorial, vai copiar dados para uma base de dados SQL. 

    ![Seleção de base de dados SQL](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
19. No separador **Geral** da janela **Propriedades**, em **Nome**, introduza **OutputSqlDataset**. 
    
    ![Nome do conjunto de dados de saída](./media/tutorial-copy-data-portal/output-dataset-name.png)
20. Vá para o separador **Ligação** e, junto a **Serviço ligado**, selecione **+ Novo**. Os conjuntos de dados têm de estar associados a um serviço ligado. O serviço ligado tem a cadeia de ligação que o Data Factory utiliza para ligar à base de dados SQL em runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) para os quais os dados são copiados. 
    
    ![Serviço ligado](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
21. Na janela **Novo Serviço Ligado**, siga os passos seguintes: 

    a. Em **Name**, introduza **AzureSqlDatabaseLinkedService**.

    b. Em **Nome do servidor**, selecione a sua instância de SQL Server.

    c. Em **Nome da base de dados**, selecione a sua base de dados SQL.

    d. Em **Nome de utilizador**, introduza o nome do utilizador.

    e. Em **Palavra-passe**, introduza a palavra-passe do utilizador.

    f. Selecione **Testar ligação** para testar a ligação.

    g. Selecione **Guardar** para guardar o serviço ligado. 
    
    ![Guardar novo serviço ligado](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

22. Em **Tabela**, selecione **[dbo].[emp]**. 

    ![Tabela](./media/tutorial-copy-data-portal/select-emp-table.png)
23. Vá para o separador **Esquema** e selecione **Importar Esquema**. 

    ![Selecione a importação do esquema](./media/tutorial-copy-data-portal/import-destination-schema.png)
24. Selecione a coluna **ID** e, em seguida, selecione **Eliminar**. A coluna **ID** é uma coluna de identidade na base de dados SQL, pelo que a atividade de cópia não tem de inserir dados nessa coluna.

    ![Eliminar a coluna ID](./media/tutorial-copy-data-portal/delete-id-column.png)
25. Vá para o separador com o pipeline e, em **Conjunto de Dados Sink**, confirme que **OutputSqlDataset** está selecionado.

    ![Separador do pipeline](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
26. Vá para o separador **Mapeamento**, na parte inferior da janela **Propriedades**, e selecione **Importar Esquemas**. Repare que as duas primeiras colunas no ficheiro de origem estão mapeadas para **FirstName** e **LastName** na base de dados SQL.

    ![Mapear esquemas](./media/tutorial-copy-data-portal/map-schemas.png)
27. Para validar o pipeline, selecione **Validar**. No canto superior direito, selecione a seta para a direita, para fechar a janela de validação.

    ![Saída da validação do pipeline](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
28. No canto superior direito, selecione **Código**. Verá o código JSON associado ao pipeline. 

    ![Botão Código](./media/tutorial-copy-data-portal/code-button.png)
29. Verá código JSON semelhante ao seguinte fragmento: 

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
Pode testar um pipeline antes de publicar artefactos (serviços ligados, conjuntos de dados e pipeline) no Data Factory ou no seu próprio repositório de Git do Visual Studio Team Services. 

1. Para testar a execução do pipeline, selecione **Execução de Testes**, na barra de ferramentas. Verá o estado da execução do pipeline no separador **Saída**, na parte inferior da janela. 

    ![Testar pipeline](./media/tutorial-copy-data-portal/test-run-output.png)
2. Confirme que os dados do ficheiro de origem são inseridos na base de dado SQL de destino. 

    ![Verificar a saída do SQL](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. No painel esquerdo, selecione **Publicar Tudo**. Esta ação publica as entidades (serviços ligados, conjuntos de dados e pipeline) que criou no Data Factory.

    ![Publicar](./media/tutorial-copy-data-portal/publish-button.png)
4. Aguarde até ver a mensagem **Publicação com êxito**. Para ver mensagens de notificação, na barra lateral do lado esquerdo, selecione o separador **Mostrar Notificações**. Para fechar a janela de notificações, selecione **Fechar**.

    ![Mostrar notificações](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Configurar o repositório de código
Pode publicar o código associado aos artefactos da sua fábrica de dados num repositório de código do Visual Studio Team Services. Neste passo, vai criar o repositório de código.  Para saber mais sobre a criação de visual com a integração de VSTS, veja [Criar com a integração de VSTS Git](author-visually.md#author-with-vsts-git-integration).

Se não quiser trabalhar com o repositório de código do Visual Studio Team Services, pode ignorar este passo. Pode continuar a publicar no Data Factory, tal como fez no passo anterior. 

1. No canto superior esquerdo, selecione **Data Factory** ou utilize a seta para baixo junto ao mesmo e selecione **Configurar Repositório de Código**. 

    ![Configurar o repositório de código](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. Na página **Definições do Repositório**, siga os passos seguintes:

    a. Em **Tipo de Repositório**, selecione **Git do Visual Studio Team Services**.

    b. Em **Conta do Visual Studio Team Services**, selecione a sua conta do Visual Studio Team Services.

    c. Em **Nome do Projeto**, selecione um projeto na sua conta do Visual Studio Team Services.

    d. Em **Nome do repositório de Git**, introduza **Tutorial2** como o repositório de Git a associar à fábrica de dados.

    e. Confirme que a caixa de verificação **Importar os recursos do Data Factory existentes para o repositório** está selecionada.

    f. Selecione **Guardar** para guardar as definições. 

    ![Definições do repositório](./media/tutorial-copy-data-portal/repository-settings.png)
3. Confirme que **VSTS GIT** está selecionado no repositório.

    ![Selecionar o GIT do VSTS](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. Num separador do browser, aceda ao repositório do **Tutorial2**. Verá dois ramos: **adf_publish** e **master** (principal).

    ![Ramos master e adf_publish](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Confirme que os ficheiros JSON das entidades do Data Factory estão no ramo **principal**.

    ![Ficheiros no ramo master](./media/tutorial-copy-data-portal/master-branch-files.png)
6. Verifique se os ficheiros JSON não estão já no ramo **adf_publish**. 

    ![Ficheiros no ramo adf_publish](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. Em **Descrição**, adicione uma descrição para o pipeline e selecione **Guardar** na barra de ferramentas. 

    ![Descrição do pipeline](./media/tutorial-copy-data-portal/pipeline-description.png)
8. Agora, deverá ver um ramo com o seu nome de utilizador no repositório **Tutorial2**. A alteração que fez foi no seu próprio ramo, não no ramo master. Só pode publicar entidades do ramo principal.

    ![O seu ramo](./media/tutorial-copy-data-portal/your-branch.png)
9. Passe o rato sobre o botão **Sincronizar** (não selecionar ainda), selecione a caixa de verificação **Consolidar Alterações** e selecione **Sincronizar** para sincronizar as alterações com o ramo principal. 

    ![Consolidar e sincronizar as alterações](./media/tutorial-copy-data-portal/commit-and-sync.png)
10. Na janela **Sincronizar as alterações**, efetue as seguintes ações: 

    a. Confirme que **CopyPipeline** é apresentado na lista atualizada de **Pipelines**.

    b. Confirme que **Publicar alterações após a sincronização** está selecionado. Se desmarcar esta caixa de verificação, só serão sincronizadas as alterações do seu ramo com o ramo principal. Não serão publicadas no Data Factory. Pode publicá-las mais tarde, ao clicar no botão **Publicar**. Se selecionar esta caixa de verificação, as alterações são primeiro sincronizadas com o ramo principal e, depois, publicadas no Data Factory.

    c. Selecione **Sincronizar**. 

    ![Sincronizar as alterações](./media/tutorial-copy-data-portal/sync-your-changes.png)
11. Agora, pode ver ficheiros no ramo **adf_publish** ramo do repositório **Tutorial2**. Também pode encontrar o modelo do Azure Resource Manager para a sua solução do Data Factory neste ramo. 

    ![A lista de ficheiros está no ramo adf_publish](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>Acionar o pipeline manualmente
Neste passo, vai acionar manualmente o pipeline que publicou no passo anterior. 

1. Selecione **Acionar** na barra de ferramentas e, em seguida, selecione **Acionar Agora**. Na página **Executar Pipeline**, selecione **Concluir**.  

    ![Acionar pipeline](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Vá para o separador **Monitorizar**, no lado esquerdo. Verá uma execução de pipeline que é acionada por um acionador manual. Pode utilizar as ligações na coluna **Ações** para ver os detalhes das atividades e para voltar a executar o pipeline.

    ![Monitorizar execuções de pipeline](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. Para ver as execuções de atividade associadas à execução do pipeline, selecione a ligação **Ver Execuções de Atividade** na coluna **Ações**. Neste exemplo, há apenas uma atividade, pelo que só vai ver uma entrada na lista. Para ver os detalhes da operação de cópia, selecione a ligação **Detalhes** (ícone de óculos) na coluna **Ações**. Selecione **Pipelines**, na parte superior, para regressar à vista **Execuções do Pipeline**. Para atualizar a vista, selecione **Atualizar**.

    ![Monitorização de execuções de atividade](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Verifique se são adicionadas mais duas linhas à tabela **emp** na base de dados SQL. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Acionar o pipeline com base numa agenda
Nesta agenda, vai criar um acionador de agenda para o pipeline. O acionador executa o pipeline na agenda especificada, como hora a hora ou diariamente. Neste exemplo, o acionador é definido para ser executado a cada minuto até à data/hora final especificada. 

1. Vá para o separador **Editar**, no lado esquerdo. 

    ![Separador Editar](./media/tutorial-copy-data-portal/edit-tab.png)
2. Selecione **Acionador** e selecione **Novo/Editar**. Se o pipeline não estiver ativo, aceda ao mesmo. 

    ![Opção Acionador](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. Na janela **Adicionar Acionadores**, selecione **Escolher acionador** e, em seguida, selecione **+ Novo**. 

    ![Botão Novo](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. Na janela **Novo Acionador**, siga os passos seguintes: 

    a. Em **Nome**, introduza **RunEveryMinute**.

    b. Em **Fim**, selecione **Na Data**.

    c. Em **Fim Em**, selecione da lista pendente.

    d. Selecione a opção **dia atual**. Por predefinição, o dia de fim está definido como o dia seguinte.

    e. Atualize a parte dos **minutos**, de modo a que sejam alguns minutos depois da data/hora atual. O acionador só é ativado depois de publicar as alterações. Se o definir para apenas poucos minutos de diferença e não o publicar até lá, não verá qualquer execução do acionador.

    f. Selecione **Aplicar**. 

    ![Propriedades do acionador](./media/tutorial-copy-data-portal/set-trigger-properties.png)

    g. Selecione a opção **Ativado**. Pode utilizar esta caixa de verificação para desativá-lo e ativá-lo mais tarde.

    h. Selecione **Seguinte**.

    ![Botão Ativado](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Está associado um custo a cada execução de pipeline, por isso, defina a data de fim adequadamente. 
5. Na página **Parâmetros da Execução do Acionador**, reveja o aviso e, em seguida, selecione **Concluir**. O pipeline neste exemplo não tem nenhum parâmetro. 

    ![Parâmetros de execução de acionador](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
6. Selecione **Sincronizar** para sincronizar as alterações do seu ramo com o ramo principal. Por predefinição, a opção **Publicar alterações após a sincronização** está selecionada. Ao selecionar **Sincronizar**, também publica as entidades atualizadas no Data Factory do ramo principal. O acionador não é ativado enquanto a publicação não for bem-sucedida.

    ![Sincronizar as alterações](./media/tutorial-copy-data-portal/sync-your-changes-with-trigger.png) 
7. Vá para o separador **Monitorizar**, do lado esquerdo, para ver as execuções do pipeline acionadas. 

    ![Execuções de pipeline acionadas](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
8. Para mudar da vista **Execuções do Pipeline** para a vista **Execuções do Acionador**, selecione **Execuções do Pipeline** e, em seguida, selecione **Execuções do Acionador**.
    
    ![Execuções de acionador](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
9. Verá as execuções do acionador numa lista. 

    ![Lista de execuções do acionador](./media/tutorial-copy-data-portal/trigger-runs-list.png)
10. Confirme que estão inseridas duas linhas por minuto (em cada execução do pipeline) na tabela **emp** até à hora de fim especificada. 

## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia dados de uma localização para outra localização no Armazenamento de blobs. Aprendeu a: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Criar um pipeline com uma atividade de cópia.
> * Testar a execução do pipeline.
> * Acionar o pipeline manualmente.
> * Acionar o pipeline com base numa agenda.
> * Monitorizar o pipeline e execuções de atividades.


Avance para o tutorial seguinte para saber como copiar dados do plano local para a cloud: 

> [!div class="nextstepaction"]
>[Copiar dados do plano local para a cloud](tutorial-hybrid-copy-portal.md)
