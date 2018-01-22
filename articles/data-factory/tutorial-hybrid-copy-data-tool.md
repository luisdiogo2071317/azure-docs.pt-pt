---
title: Copiar dados no local utilizando a ferramenta Copiar Dados do Azure | Microsoft Docs
description: "Crie uma fábrica de dados do Azure e utilize a ferramenta Copiar Dados para copiar dados de uma base de dados do SQL Server no local para um armazenamento de Blobs do Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: aba8b13d47b2b9236a0d5cc868e53780e894c406
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-copy-data-tool"></a>Copiar dados de uma base de dados SQL Server no local para o Armazenamento de Blobs do Azure utilizando a ferramenta Copiar Dados
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versão 2 - Pré-visualização](tutorial-hybrid-copy-data-tool.md)

Neste tutorial, irá utilizar o portal do Azure para criar uma fábrica de dados. Utilize a ferramenta Copiar Dados para criar um pipeline que copia dados de uma base de dados do SQL Server no local para um armazenamento de blobs do Azure.

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
### <a name="azure-subscription"></a>Subscrição do Azure
Antes de começar, se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/).

### <a name="azure-roles"></a>Funções do Azure
Para criar instâncias de fábricas de dados, a conta de utilizador que utiliza para iniciar sessão no Azure tem de ter atribuída a função *contribuidor* ou *proprietário* ou ser *administradora* da subscrição do Azure. 

Para ver as permissões que a sua subscrição tem, aceda ao portal do Azure, selecione o seu nome de utilizador, no canto superior direito, e selecione **Permissões**. Se tiver acesso a várias subscrições, selecione a subscrição apropriada. Para obter instruções de exemplo sobre como adicionar um utilizador a uma função, veja o artigo [Add roles](../billing/billing-add-change-azure-subscription-administrator.md) (Adicionar funções).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 e 2017
Neste tutorial, vai utilizar uma base de dados do SQL Server no local como um arquivo de dados de *origem*. O pipeline da fábrica de dados que vai criar neste tutorial copia dados desta base de dados do SQL Server no local (origem) para o Armazenamento de blobs do Azure (sink). Vai criar uma tabela com o nome **emp** na sua base de dados do SQL Server e insira duas entradas de exemplo na mesma. 

1. Inicie o SQL Server Management Studio. Se ainda não estiver instalado no seu computador, aceda a [Transferir o SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). 

2. Utilize as suas credenciais para se ligar à sua instância do SQL Server. 

3. Crie uma base de dados de exemplo. Na vista de árvore, clique com o botão direito do rato em **Bases de Dados** e selecione **Nova Base de Dados**. 
 
4. Na janela **Nova Base de Dados**, introduza um nome para a base de dados e selecione **OK**. 

5. Para criar a tabela **emp** e inserir alguns dados de exemplo na mesma, execute o seguinte script de consulta na base de dados: na vista de árvore, clique com botão direito na base de dados que criou e, em seguida, selecione **Nova Consulta**.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
    ```

### <a name="azure-storage-account"></a>Conta de armazenamento do Azure
Neste tutorial, vai utilizar uma Conta de Armazenamento do Azure de fins gerais (mais concretamente, o Armazenamento de Blobs do Azure) como arquivo de dados de destino/sink. Se não tiver uma conta de armazenamento do Azure para fins gerais, veja [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account) para instruções sobre como criar uma. O pipeline da fábrica de dados que vai criar neste tutorial copia dados da base de dados do SQL Server no local (origem) para este armazenamento de Blobs do Azure (sink). 

#### <a name="get-storage-account-name-and-account-key"></a>Obter o nome e a chave da conta de armazenamento
Utilize o nome e a chave da sua conta de armazenamento do Azure neste tutorial. Obtenha o nome e a chave da sua conta de armazenamento seguindo os seguintes passos: 

1. Inicie sessão no [portal do Azure](https://portal.azure.com) com o seu nome de utilizador e sua palavra-passe do Azure. 

2. No painel do lado esquerdo, selecione **Mais Serviços**, utilize a palavra-chave **Armazenamento** para filtrar e selecione **Contas de Armazenamento**.

    ![Procurar contas de armazenamento](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Na lista das contas de armazenamento, filtre para encontrar a sua conta de armazenamento (se necessário) e selecione-a. 

4. Na janela **Conta de armazenamento**, selecione **Chaves de acesso**.

    ![Obter o nome e a chave da conta de armazenamento](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Nas caixas **Nome da conta de armazenamento** e **key1**, copie os valores e cole-os no Bloco de notas ou noutro editor, para utilizar mais adiante no tutorial. 

#### <a name="create-the-adftutorial-container"></a>Criar o contentor adftutorial 
Nesta secção, vai criar um contentor de blobs com o nome **adftutorial** no seu armazenamento de Blobs do Azure. 

1. Na janela **Conta de armazenamento**, mude para a **Descrição Geral** e selecione **Blobs**. 

    ![Selecionar a opção Blobs](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. Na janela **Serviço de Blob**, selecione **Contentor**. 

    ![Adicionar o botão de contentor](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. Na janela **Novo Contentor**, na caixa **Nome**, introduza **adftutorial** e selecione **OK**. 

    ![Introduzir o nome do contentor](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Na lista de contentores, clique em **adftutorial**.  

    ![Selecionar o contentor](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Mantenha a janela do **contentor** de **adftutorial** aberta. Vai utilizá-la para verificar o resultado no final deste tutorial. O Data Fabric cria automaticamente a pasta de saída neste contentor, pelo que não precisa de a criar.

    ![Janela do contentor](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Página Nova fábrica de dados](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
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

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Azure Data Factory num separador à parte. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Utilize a ferramenta Copiar Dados para criar um pipeline

1. Na página de introdução, clique no mosaico **Copiar Dados** para iniciar a ferramenta Copiar Dados. 

   ![Mosaico ferramenta Copiar Dados](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Na página **propriedades** da ferramenta Copiar Dados, especifique **CopyFromOnPremSqlToAzureBlobPipeline** como **Nome da tarefa**e clique em **Seguinte**. A ferramenta Copiar Dados cria um pipeline com o nome que especificar para este campo. 
    
   ![Página Propriedades](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. Na página **Arquivo de dados de origem**, selecione **SQL Server** e clique em **Seguinte**. Poderá ter de deslocar para baixo para ver o **SQL Server** na lista. 

   ![Página de arquivo de dados de origem](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Introduza **SqlServerLinkedService** para **Nome da ligação** e clique na ligação **Criar Integration Runtime**. Tem de criar um runtime de integração autoalojado, transferi-lo para o seu computador e registá-lo com o serviço Data Factory. O runtime de integração autoalojado copia dados entre o ambiente no local e a cloud do Azure.

   ![Ligação Criar runtime de integração](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. Na caixa de diálogo **Criar Integration Runtime**, introduza **TutorialIntegration Runtime** no campo **Nome** e clique em **Criar**. 

   ![Caixa de diálogo Criar runtime de integração](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Clique em **Iniciar a configuração rápida neste computador**. Esta ação instala o runtime de integração no seu computador e regista-o com o serviço Data Factory. Em alternativa, pode utilizar a opção de configuração manual para transferir o ficheiro de instalação, executá-lo e utilizar a chave para registar o runtime de integração. 

    ![Ligação Criar runtime de integração](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Execute a aplicação transferida. Pode ver o **estado** da configuração rápida na janela. 

    ![Estado da configuração rápida](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Confirme que o **TutorialIntegrationRuntime** está selecionado para o campo **Integration Runtime**.

    ![Runtime de integração selecionado](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. Em **Especificar a base de dados do SQL Server no local**, efetue os seguintes passos: 

    1. Introduza **OnPremSqlLinkedService** como **Nome da ligação**.
    2. Introduza o nome do SQL Server no local em **Nome do servidor**.
    3. Introduza o nome da base de dados no local em **Nome da base de dados**. 
    4. Selecione autenticação adequada para **Tipo de autenticação**.
    5. Introduza o nome do utilizador com acesso ao SQL Server no local em **Nome do utilizador**.
    6. Introduza a **palavra-passe** do utilizador. 
10. Na página **Selecionar tabelas a partir das quais copiar dados ou utilizar uma consulta personalizada**, selecione a tabela na lista **[dbo].[emp]** e clique em **Seguinte**. 

    ![Selecione a tabela emp](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. Na página **Arquivo de dados de destino**, selecione **Armazenamento de Blobs do Azure** e clique em **Seguinte**.

    ![Selecionar Armazenamento de Blobs do Azure](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. Na página **Especificar a conta de armazenamento de Blobs do Azure**, execute os seguintes passos: 

    1. Introduza **AzureStorageLinkedService** como **Nome da ligação**.
    2. Selecione a sua conta de Armazenamento do Azure a partir da lista pendente em **Nome da conta de armazenamento**. 
    3. Clique em **Seguinte**.

        ![Selecionar Armazenamento de Blobs do Azure](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. Na página **Escolher o ficheiro ou pasta de saída**, execute os seguintes passos: 
    
    1. Introduza **adftutorial/fromonprem** no **Caminho da pasta**. Criou o contentor **adftutorial** como parte dos pré-requisitos. Se a pasta de saída não existir, o serviço Data Factory cria-a automaticamente. Também pode utilizar o botão **Procurar** para navegar pelo armazenamento de Blobs e respetivos contentores/pastas. Tenha em atenção que o nome do ficheiro de saída está definido como **dbo.emp** por predefinição.
        
        ![Escolher ficheiro ou pasta de saída](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. Na página **Definições do formato do ficheiro**, clique em **Seguinte**. 

    ![Página de definições do formato do ficheiro](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. Na página **Definições**, clique em **Seguinte**. 

    ![Página de definições](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. Na página **Resumo**, reveja os valores para todas definições e clique em **Seguinte**. 

    ![Página de resumo](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. Na página **Implementação**, clique em **Monitorização** para monitorizar o pipeline ou tarefa que criou.

    ![Página de implementação](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. No separador **Monitorização**, pode ver o estado do pipeline que criou. As ligações na coluna **Ações** permitem-lhe ver as execuções de atividades associadas à execução do pipeline e voltar a executar o pipeline. 

    ![Execuções de pipeline](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Clique na ligação **Ver Execuções de Atividade** na coluna **Ações** para ver as execuções de atividade associadas à execução do pipeline. Para ver os detalhes da operação de cópia, clique na ligação **Detalhes** (ícone de óculos), na coluna **Ações**. Pode clicar em **Pipelines**, na parte superior, para regressar à vista de execuções de pipelines.

    ![Execuções de atividade](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Confirme que vê um ficheiro de saída na pasta **fromonprem** do contentor **adftutorial**.   
 
    ![Blob de saída](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Clique no separador **Editar** à esquerda para mudar para o modo de edição. Pode atualizar os serviços ligados, conjuntos de dados e pipelines criados pela ferramenta de utilizando o editor. Clique em **Código** para ver o código JSON associado à entidade aberta no editor. Para obter detalhes de edição sobre estas entidades na IU do Data Factory, veja [a versão do portal do Azure deste tutorial](tutorial-copy-data-portal.md).

    ![Separador Editar](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia os dados de uma base de dados do SQL Server no local para um armazenamento de Blobs do Azure. Aprendeu a: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Utilize a ferramenta Copiar Dados para criar um pipeline
> * Monitorizar o pipeline e execuções de atividades.

Para obter uma lista dos arquivos de dados que o Data Factory suporta, veja [Supported data stores](copy-activity-overview.md#supported-data-stores-and-formats) (Arquivos de Dados suportados).

Para aprender a copiar dados em massa de uma origem para um destino, avance para o tutorial seguinte:

> [!div class="nextstepaction"]
>[Copiar dados em massa](tutorial-bulk-copy-portal.md)