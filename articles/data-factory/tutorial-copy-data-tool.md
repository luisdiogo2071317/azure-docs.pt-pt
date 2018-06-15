---
title: Copiar dados utilizando a ferramenta Copiar Dados do Azure | Microsoft Docs
description: Crie uma fábrica de dados do Azure e, em seguida, utilize a ferramenta Copiar Dados para copiar dados do Armazenamento de blobs do Azure para uma base de dados SQL.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: d2f1d089c6a08a1dc90f82fd9d1c3cb2b6f6dc0a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30171810"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-the-copy-data-tool"></a>Copiar dados do Armazenamento de blobs do Azure para uma base de dados SQL com a ferramenta Copiar Dados
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Versão 1 - Disponibilidade geral](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versão 2 - Pré-visualização](tutorial-copy-data-tool.md)

Neste tutorial, irá utilizar o portal do Azure para criar uma fábrica de dados. Em seguida, utiliza a ferramenta Copiar Dados para criar um pipeline que copia dados do Armazenamento de blobs do Azure para uma base de dados SQL. 

> [!NOTE]
> Se não estiver familiarizado com o Azure Data Factory, veja [Introdução ao Azure Data Factory](introduction.md).
>
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utiliza a versão 1 do Data Factory, disponível de uma forma geral, veja [Introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


Neste tutorial, vai executar os seguintes passos:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Utilizar a ferramenta Copiar Dados para criar um pipeline.
> * Monitorizar o pipeline e execuções de atividades.

## <a name="prerequisites"></a>Pré-requisitos

* **Subscrição do Azure**: se não tem uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* **Conta de armazenamento do Azure**: utilize o Armazenamento de blobs como arquivo de dados de _origem_. Se não tem uma conta de armazenamento do Azure, veja as instruções apresentadas em [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* **Base de Dados SQL do Azure**: utilize uma base de dados SQL como arquivo de dados _sink_. Se não tem uma base de dados SQL, veja as instruções apresentadas em [Criar uma base de dados SQL](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Criar um blob e uma tabela SQL

Prepare o Armazenamento de blobs e a sua base de dados SQL para o tutorial ao executar os seguintes passos.

#### <a name="create-a-source-blob"></a>Criar um blob de origem

1. Inicie o **Bloco de Notas**. Copie o seguinte texto e guarde-o num ficheiro com o nome **inputEmp.txt** no seu disco:

    ```
    John|Doe
    Jane|Doe
    ```

2. Crie um contentor com o nome **adfv2tutorial** e carregue o ficheiro inputEmp.txt para o contentor. Pode utilizar várias ferramentas para executar estas tarefas, como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Criar uma tabela SQL sink

1. Utilize o seguinte script SQL para criar uma tabela com o nome **dbo.emp** na sua base de dados SQL:

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

2. Permita o acesso dos serviços do Azure ao SQL Server. Verifique se a definição **Permitir acesso aos serviços do Azure** está ativada para o servidor que tem o SQL Server em execução. Esta definição permite que o Data Factory grave dados na sua instância de SQL Server. Para verificar e ativar esta definição, execute estes passos:

    a. No lado esquerdo, selecione **Mais serviços** e, em seguida, selecione **Servidores SQL**.

    b. Selecione o seu servidor e, em seguida, selecione **DEFINIÇÕES** > **Firewall**.

    c. Na página **Definições de firewall**, defina a opção **Permitir acesso aos serviços do Azure** como **ATIVADO**.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. No menu da esquerda, selecione **+ Novo** > **Dados + Análise** > **Fábrica de Dados**: 
   
   ![Criação de nova fábrica de dados](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, em **Nome**, introduza **ADFTutorialDataFactory**. 
      
     ![Nova fábrica de dados](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   O nome da fábrica de dados tem de ser _globalmente exclusivo_. Poderá receber a seguinte mensagem de erro:
   
   ![Mensagem de erro de nova fábrica de dados](./media/tutorial-copy-data-tool/name-not-available-error.png)

   Se receber uma mensagem de erro relacionada com o valor do nome, introduza um nome diferente para a fábrica de dados. Por exemplo, utilize o nome _**oseunome**_**ADFTutorialDataFactory**. Para ter acesso às regras de nomenclatura para artefactos do Data Factory, veja [Regras de nomenclatura do Data Factory](naming-rules.md).
3. Selecione a **subscrição** do Azure na qual quer criar a nova fábrica de dados. 
4. Em **Grupo de Recursos**, efetue um destes passos:
     
    a. Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente.

    b. Selecione **Criar novo** e introduza o nome de um grupo de recursos. 
         
    Para saber mais sobre grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).

5. Em **Versão**, selecione **V2 (Pré-visualização)** para indicar a versão.
6. Em **Localização**, selecione a localização da fábrica de dados. Apenas são apresentadas as localizações suportadas na lista pendente. Os arquivos de dados (por exemplo, o Armazenamento do Azure e a Base de Dados SQL) e as computações (por exemplo, o Azure HDInsight) utilizados pela fábrica de dados podem estar noutras localizações e regiões.
7. Selecione **Afixar ao dashboard**. 
8. Selecione **Criar**.
9. No dashboard, o mosaico **A Implementar a Fábrica de Dados** mostra o estado do processo.

    ![Mosaico A Implementar o Data Factory](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Depois de concluída a criação, é apresentada a home page **Fábrica de Dados**.
   
    ![Home page da fábrica de dados](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Para iniciar a interface de utilizador (IU) do Azure Data Factory num separador à parte, selecione o mosaico **Criar e Monitorizar**. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Utilizar a ferramenta Copiar Dados para criar um pipeline

1. Na página **Vamos começar**, selecione o mosaico **Copiar Dados** para iniciar a ferramenta Copiar Dados. 

   ![Mosaico ferramenta Copiar Dados](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Na página **Propriedades**, em **Nome da tarefa**, introduza **CopyFromBlobToSqlPipeline**. Em seguida, selecione **Seguinte**. A IU do Data Factory cria um pipeline com o nome de tarefa especificado. 

    ![Página Propriedades](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Na página **Arquivo de dados de origem**, selecione **Armazenamento de Blobs do Azure** e, em seguida, selecione **Seguinte**. Os dados de origem encontram-se num Armazenamento de blobs. 

    ![Página de arquivo de dados de origem](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. Na página **Especificar a conta de Armazenamento de blobs do Azure**, siga os passos seguintes:

    a. Em **Nome da ligação**, introduza **AzureStorageLinkedService**.

    b. Selecione o nome da sua conta de armazenamento na lista pendente **Nome da conta de armazenamento**.

    c. Selecione **Seguinte**. 

    ![Especificar a conta de armazenamento](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

    Os serviços ligados ligam um arquivo de dados ou uma computação à fábrica de dados. Neste caso, vai criar um serviço ligado de armazenamento para ligar a sua conta de armazenamento ao arquivo de dados. O serviço ligado tem as informações de ligação que o Data Factory utiliza para ligar ao Armazenamento de blobs em runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) que contém os dados de origem. 

5. Na página **Escolher o ficheiro ou pasta de entrada**, execute os seguintes passos:
    
    a. Navegue para a pasta **adfv2tutorial/input**.

    b. Selecione o ficheiro **inputEmp.txt**.

    c. Selecione **Selecionar**. Em alternativa, pode fazer duplo clique no ficheiro **inputEmp.txt**.

    d. Selecione **Seguinte**. 

    ![Escolher o ficheiro ou pasta de entrada](./media/tutorial-copy-data-tool/choose-input-file-folder.png)

6. Na página **Definições do formato do ficheiro**, repare que a ferramenta deteta automaticamente os delimitadores de coluna e linha. Selecione **Seguinte**. Também pode pré-visualizar os dados e ver o esquema dos dados de entrada nesta página. 

    ![Definições do formato do ficheiro](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. Na página **Arquivo de dados de destino**, selecione **Base de Dados SQL do Azure** e depois selecione **Seguinte**.

    ![Arquivo de dados de destino](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. Na página **Especificar a base de dados SQL do Azure**, execute os seguintes passos: 

    a. Em **Nome da ligação**, introduza **AzureSqlDatabaseLinkedService**.

    b. Em **Nome do servidor**, selecione a sua instância de SQL Server.

    c. Em **Nome da base de dados**, selecione a sua base de dados SQL.

    d. Em **Nome de utilizador**, introduza o nome do utilizador.

    e. Em **Palavra-passe**, introduza a palavra-passe do utilizador.

    f. Selecione **Seguinte**. 

    ![Especificar a base de dados SQL](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

    Os conjuntos de dados têm de estar associados a um serviço ligado. O serviço ligado tem a cadeia de ligação que o Data Factory utiliza para ligar à base de dados SQL em runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) para os quais os dados são copiados.

9. Na página **Mapeamento da tabela**, selecione a tabela **[dbo].[emp]** e, em seguida, selecione **Seguinte**. 

    ![Mapeamento da tabela](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. Na página **Mapeamento de esquema**, repare que a primeira e segunda colunas no ficheiro de entrada estão mapeadas para as colunas **FirstName** e **LastName** da tabela **emp** .

    ![Página de mapeamento de esquema](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. Na página **Definições**, selecione **Seguinte**. 

    ![Página de definições](./media/tutorial-copy-data-tool/settings-page.png)
12. Na página **Resumo**, reveja as definições e depois selecione **Seguinte**.

    ![Página de resumo](./media/tutorial-copy-data-tool/summary-page.png)
13. Na **Página de implementação**, selecione **Monitorizar** para monitorizar o pipeline (tarefa).

    ![Página de implementação](./media/tutorial-copy-data-tool/deployment-page.png)
14. Tenha em atenção que o separador **Monitorização** à esquerda é selecionado automaticamente. A coluna **Ações** inclui ligações para ver os detalhes de execução da atividade e voltar a executar o pipeline. Selecione **Atualizar** para atualizar a lista. 

    ![Monitorizar execuções de pipeline](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. Para ver as execuções da atividade associadas à execução do pipeline, selecione a ligação **Ver Execuções de Atividade** na coluna **Ações**. Há apenas uma atividade (atividade copiar) no pipeline, pelo que só vai ver uma entrada. Para ver os detalhes da operação de cópia, selecione a ligação **Detalhes** (ícone de óculos) na coluna **Ações**. Para regressar à vista **Execuções do Pipeline**, selecione a ligação **Pipelines** na parte superior. Para atualizar a vista, selecione **Atualizar**. 

    ![Monitorização de execuções de atividade](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Selecione o separador **Editar** à esquerda para mudar para o modo de edição. Pode atualizar os serviços ligados, os conjuntos de dados e os pipelines criados através da ferramenta com o editor. Selecione **Código** para ver o código JSON da entidade que está atualmente aberta no editor. Para obter detalhes de edição sobre estas entidades na IU do Data Factory, veja [a versão do portal do Azure deste tutorial](tutorial-copy-data-portal.md).

    ![Separador Editor](./media/tutorial-copy-data-tool/edit-tab.png)
17. Certifique-se de que os dados estão inseridos na tabela **emp** na sua base de dados SQL.

    ![Verificar a saída do SQL](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Passos seguintes
O pipeline deste exemplo copia os dados do Armazenamento de blobs para uma base de dados SQL. Aprendeu a: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Utilizar a ferramenta Copiar Dados para criar um pipeline.
> * Monitorizar o pipeline e execuções de atividades.

Avance para o tutorial seguinte para saber como copiar dados do plano local para a cloud: 

> [!div class="nextstepaction"]
>[Copiar dados do plano local para a cloud](tutorial-hybrid-copy-data-tool.md)
