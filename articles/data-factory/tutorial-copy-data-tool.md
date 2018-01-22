---
title: Copiar dados utilizando a ferramenta Copiar Dados do Azure | Microsoft Docs
description: "Crie uma fábrica de dados do Azure e utilize a ferramenta Copiar Dados para copiar dados do Armazenamento de Blobs do Azure para a Base de Dados SQL do Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: a8c7035ebf462bb168147e9d8eb60742333ce8b8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-copy-data-tool"></a>Copiar dados do Blob do Azure para a Base de Dados SQL do Azure utilizando a ferramenta Copiar Dados
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versão 2 - Pré-visualização](tutorial-copy-data-tool.md)

Neste tutorial, irá utilizar o portal do Azure para criar uma fábrica de dados. Utilize a ferramenta Copiar Dados para criar um pipeline que copia dados de um armazenamento de blobs do Azure para uma base de dados SQL do Azure. 

> [!NOTE]
> - Se não estiver familiarizado com o Azure Data Factory, veja [Introdução ao Azure Data Factory](introduction.md).
>
> - Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


Vai executar os seguintes passos neste tutorial:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Utilize a ferramenta Copiar Dados para criar um pipeline
> * Monitorizar o pipeline e execuções de atividades.

## <a name="prerequisites"></a>Pré-requisitos

* **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.
* **Conta de Armazenamento do Azure**. Utilize o armazenamento de blobs como arquivo de dados de **origem**. Se não tiver uma conta de armazenamento do Azure, veja o artigo [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account) para obter os passos para criar uma.
* **Base de Dados SQL do Azure**. Pode utilizar a base de dados como arquivo de dados **sink**. Se não tiver uma Base de Dados SQL do Azure, veja o artigo [Criar uma base de dados SQL do Azure](../sql-database/sql-database-get-started-portal.md) para obter os passos para criar uma.

### <a name="create-a-blob-and-a-sql-table"></a>Criar um blob e uma tabela SQL

Agora, prepare o Blob do Azure e a Base de Dados SQL do Azure para o tutorial ao efetuar os seguintes passos:

#### <a name="create-a-source-blob"></a>Criar um blob de origem

1. Inicie o Bloco de notas. Copie o texto seguinte e guarde-o como ficheiro **input.txt** no disco.

    ```
    John|Doe
    Jane|Doe
    ```

2. Utilize ferramentas como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/) para criar o contentor **adfv2tutorial** e carregar o ficheiro **inputEmp.txt** para o contentor.

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

2. Permita que os serviços do Azure acedam ao servidor SQL. Certifique-se de que a definição **Permitir acesso aos serviços do Azure** está ativada para o servidor SQL do Azure para que o serviço Data Factory possa escrever dados no servidor SQL do Azure. Para verificar e ativar desta definição, execute os passos seguintes:

    1. Clique no hub **Mais serviços** à esquerda e clique em **Servidores SQL**.
    2. Selecione o seu servidor e clique em **Firewall** em **DEFINIÇÕES**.
    3. Na página **Definições de firewall**, clique em **ATIVAR** para **Permitir acesso aos serviços do Azure**.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Página Nova fábrica de dados](./media/tutorial-copy-data-tool/name-not-available-error.png)
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
      Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Apenas são apresentadas as localizações suportadas na lista pendente. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras localizações/regiões.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-copy-data-tool/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-copy-data-tool/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Azure Data Factory num separador à parte. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Utilize a ferramenta Copiar Dados para criar um pipeline

1. Na página de introdução, clique no mosaico **Copiar Dados** para iniciar a ferramenta Copiar Dados. 

   ![Mosaico ferramenta Copiar Dados](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Na página **propriedades** da ferramenta Copiar Dados, especifique **CopyFromBlobToSqlPipeline** como **Nome da tarefa**e clique em **Seguinte**. A IU do Data Factory cria um pipeline com o nome que especificar como nome da tarefa. 

    ![Ferramenta Copiar Dados – Página propriedades](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Na página **Arquivo de dados de origem**, selecione **Armazenamento de Blobs do Azure** e clique em **Seguinte**. A origem de dados está num armazenamento de blobs do Azure. 

    ![Página de arquivo de dados de origem](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. Na página **Especificar a conta de armazenamento de Blobs do Azure**, execute os seguintes passos: 
    1. Introduza **AzureStorageLinkedService** como **nome da ligação**.
    2. Selecione o **nome da conta de armazenamento** na lista pendente.
    3. Clique em **Seguinte**. 

        ![Especifique a conta de armazenamento de blobs](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

        Os serviços ligados ligam um arquivo de dados ou uma computação à fábrica de dados. Neste caso, vai criar um serviço ligado do Armazenamento do Azure para ligar a sua conta de Armazenamento do Azure ao arquivo de dados. O serviço ligado tem as informações de ligação que o serviço Data Factory utiliza para se ligar ao armazenamento de blobs no runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) que contém os dados de origem. 
5. Na página **Escolher o ficheiro ou pasta de entrada**, execute os seguintes passos:
    
    1. Navegue até à pasta **adfv2tutorial/input**.
    2. Selecione **inputEmp.txt**
    3. Clique em **Escolher**. Em alternativa, pode fazer duplo clique em **inputEmp.txt**. 
    4. Clique em **Seguinte**. 

    ![Escolher ficheiro ou pasta de entrada](./media/tutorial-copy-data-tool/choose-input-file-folder.png)
6. Na página **Definições do formato do ficheiro**, tenha em atenção que a ferramenta deteta automaticamente os delimitadores de linha e coluna e clique em **Seguinte**. Também pode pré-visualizar os dados e ver o esquema dos dados de entrada nesta página. 

    ![Página de definições do formato do ficheiro](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. Na página de **Arquivo de dados de destino**, selecione **Base de Dados SQL do Azure** e clique em **Seguinte**. 

    ![Página arquivo de dados de destino](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. Na página **Especificar base de dados SQL do Azure**, execute os seguintes passos: 

    1. Especifique **AzureSqlDatabaseLinkedService** como **Nome da ligação**. 
    2. Em **Nome do servidor**, selecione o seu servidor do SQL do Azure.
    3. Em **Nome da base de dados**, selecione a sua base de dados SQL do Azure.
    4. Especifique o nome do utilizador para o **Nome de utilizador**.
    5. Especifique a palavra-passe do utilizador para a **palavra-passe**.
    6. Clique em **Seguinte**. 

        ![Especificar a base de dados SQL do Azure](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

        Os conjuntos de dados têm de estar associados a um serviço ligado. O serviço ligado tem a cadeia de ligação que o serviço Data Factory utiliza para se ligar à base de dados SQL do Azure no runtime. O conjunto de dados especifica o contentor, a pasta e o ficheiro (opcional) para os quais os dados são copiados.
9.  Na página **Mapeamento de tabela** página, selecione **[dbo].[emp]** e clique em **Seguinte**. 

    ![Página de mapeamento de tabela](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. Na página **Mapeamento de esquema**, tenha em atenção que a primeira e segunda coluna no ficheiro de entrada está mapeada para as colunas **FirstName** e **LastName** da tabela **emp** . 

    ![Página de mapeamento de esquema](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. Na página **Definições**, clique em **Seguinte**. 

    ![Página de definições](./media/tutorial-copy-data-tool/settings-page.png)
12. Na página **Resumo**, reveja as definições e clique em **Seguinte**.

    ![Página de resumo](./media/tutorial-copy-data-tool/summary-page.png)
13. Na **Página de implementação**, clique em **Monitorização** para monitorizar o pipeline (tarefa).

    ![Página de implementação](./media/tutorial-copy-data-tool/deployment-page.png)
14. Tenha em atenção que o separador **Monitorização** à esquerda é selecionado automaticamente. Pode ver as ligações para ver os detalhes de execução da atividade e voltar a executar o pipeline na coluna **Ações**. Clique em **Atualizar** para atualizar a lista. 

    ![Monitorizar execuções de pipeline](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. Para ver as execuções de atividades associadas à execução do pipeline, clique na ligação **Ver Execuções de Atividades**, na coluna **Ações**. Há apenas uma atividade (copiar atividade) no pipeline, pelo que só vai ver uma entrada. Para ver os detalhes da operação de cópia, clique na ligação **Detalhes** (ícone de óculos), na coluna **Ações**. Para regressar à vista de execuções do pipeline, clique na ligação **Pipelines**, conforme mostrado na imagem. Para atualizar a vista, clique em **Atualizar**. 

    ![Monitorização de execuções de atividade](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Clique no separador **Editar** à esquerda para mudar para o modo de edição. Pode atualizar os serviços ligados, conjuntos de dados e pipelines criados pela ferramenta de utilizando o editor. Clique em **Código** para ver o código JSON associado à entidade aberta no editor. Para obter detalhes de edição sobre estas entidades na IU do Data Factory, veja [a versão do portal do Azure deste tutorial](tutorial-copy-data-portal.md).

    ![Separador Editor](./media/tutorial-copy-data-tool/edit-tab.png)
17. Certifique-se de que os dados são inseridos na tabela **emp** na base de dados SQL do Azure. 

    ![Verificar a saída do SQL](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia os dados de um armazenamento de blobs do Azure para uma base de dados SQL do Azure. Aprendeu a: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Utilize a ferramenta Copiar Dados para criar um pipeline
> * Monitorizar o pipeline e execuções de atividades.

Avance para o tutorial seguinte para saber como copiar dados do local para a cloud: 

> [!div class="nextstepaction"]
>[Copiar dados do local para a cloud](tutorial-hybrid-copy-data-tool.md)