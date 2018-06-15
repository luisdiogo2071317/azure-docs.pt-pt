---
title: Copiar dados no local com a ferramenta Copiar Dados do Azure | Microsoft Docs
description: Crie uma fábrica de dados do Azure e utilize a ferramenta Copiar Dados para copiar dados de uma base de dados do SQL Server no local para um armazenamento de Blobs do Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: 85b721df1e666903c4966ca240c433ded01c06b7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30171908"
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-the-copy-data-tool"></a>Copiar dados de uma base de dados do SQL Server no local para o Armazenamento de blobs do Azure com a ferramenta Copiar Dados
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - Disponibilidade Geral](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versão 2 - Pré-visualização](tutorial-hybrid-copy-data-tool.md)

Neste tutorial, irá utilizar o portal do Azure para criar uma fábrica de dados. Utilize a ferramenta Copiar Dados para criar um pipeline que copia dados de uma base de dados SQL Server no local para o Armazenamento de blobs do Azure.

> [!NOTE]
> - Se não estiver familiarizado com o Data Factory, veja [Introdução ao Azure Data Factory](introduction.md).
>
> - Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utiliza a versão 1 do Data Factory, que está disponível em geral, veja a [Introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Neste tutorial, vai executar os seguintes passos:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Utilizar a ferramenta Copiar Dados para criar um pipeline.
> * Monitorizar o pipeline e execuções de atividades.

## <a name="prerequisites"></a>Pré-requisitos
### <a name="azure-subscription"></a>Subscrição do Azure
Antes de começar, se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/).

### <a name="azure-roles"></a>Funções do Azure
Para criar instâncias de fábricas de dados, a conta de utilizador que utiliza para iniciar sessão no Azure tem de ter atribuída a função *contribuidor* ou *proprietário* ou ser *administradora* da subscrição do Azure. 

Para ver as permissões que tem na subscrição, aceda ao portal do Azure. Selecione o nome de utilizador no canto superior direito, e, em seguida, selecione **Permissões**. Se tiver acesso a várias subscrições, selecione a subscrição apropriada. Para obter instruções de exemplo sobre como adicionar um utilizador a uma função, veja [Adicionar funções](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 e 2017
Neste tutorial, vai utilizar uma base de dados do SQL Server no local como um arquivo de dados de *origem*. O pipeline da fábrica de dados que vai criar neste tutorial copia dados desta base de dados do SQL Server no local (origem) para o Armazenamento de blobs (sink). Vai criar uma tabela com o nome **emp** na sua base de dados do SQL Server e inserir algumas entradas de exemplo na tabela. 

1. Inicie o SQL Server Management Studio. Se ainda não estiver instalado no seu computador, aceda a [Transferir o SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). 

2. Utilize as suas credenciais para se ligar à sua instância do SQL Server. 

3. Crie uma base de dados de exemplo. Na vista de árvore, clique com o botão direito do rato em **Bases de Dados** e selecione **Nova Base de Dados**. 
 
4. Na janela **Nova Base de Dados**, introduza um nome para a base de dados e selecione **OK**. 

5. Para criar a tabela **emp** e inserir alguns dados de exemplo na mesma, execute o script de consulta seguinte na base de dados. Na vista de árvore, clique com o botão direito do rato na base de dados que criou e selecione **Nova Consulta**.

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
Neste tutorial, utiliza uma conta de armazenamento do Azure para fins gerais (mais concretamente, o Armazenamento de blobs) como arquivo de dados de destino/sink. Se não tiver uma conta de armazenamento para fins gerais, veja [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account) para instruções sobre como criar uma. O pipeline da fábrica de dados que vai criar neste tutorial copia dados da base de dados do SQL Server no local (origem) para este Armazenamento de blobs (sink). 

#### <a name="get-the-storage-account-name-and-account-key"></a>Obter o nome e a chave da conta de armazenamento
Utilize o nome e a chave da sua conta de armazenamento neste tutorial. Para obter o nome e a chave da sua conta de armazenamento, siga os passos seguintes: 

1. Inicie sessão no [portal do Azure](https://portal.azure.com) com o seu nome de utilizador e a palavra-passe do Azure. 

2. No painel esquerdo, selecione **Mais serviços**. Utilize a palavra-chave **Armazenamento** para filtrar e selecione **Contas de armazenamento**.

    ![Procurar conta de armazenamento](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Na lista de contas de armazenamento, filtre para encontrar a sua conta de armazenamento, se necessário. Em seguida, selecione a sua conta de armazenamento. 

4. Na janela **Conta de armazenamento**, selecione **Chaves de acesso**.

    ![Chaves de acesso](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Nas caixas **Nome da conta de armazenamento** e **key1**, copie os valores e cole-os no Bloco de notas ou noutro editor, para utilizar mais adiante no tutorial. 

#### <a name="create-the-adftutorial-container"></a>Criar o contentor adftutorial 
Nesta secção, vai criar um contentor de blobs com o nome **adftutorial** no seu Armazenamento de blobs. 

1. Na janela **Conta de armazenamento**, mude para a **Descrição Geral** e selecione **Blobs**. 

    ![Selecionar a opção Blobs](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. Na janela **Serviço de Blob**, selecione **Contentor**. 

    ![Botão de contentor](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. Na janela **Novo Contentor**, na caixa **Nome**, introduza **adftutorial** e selecione **OK**. 

    ![Novo contentor](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Na lista de contentores, clique em **adftutorial**.

    ![Seleção de contentor](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Mantenha a janela **Contentor** de **adftutorial** aberta. Vai utilizá-la para verificar o resultado no final deste tutorial. O Data Factory cria automaticamente a pasta de saída neste contentor, pelo que não precisa de a criar.

    ![Janela do contentor](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. No menu da esquerda, selecione **Novo** > **Dados + Análise** > **Data Factory**. 
   
   ![Criação de nova fábrica de dados](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, em **Nome**, introduza **ADFTutorialDataFactory**. 
      
     ![Nova fábrica de dados](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   O nome da fábrica de dados tem de ser *globalmente exclusivo*. Se vir a seguinte mensagem de erro no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Para obter as regras de nomenclatura dos artefactos do Data Factory, veja [Regras de nomenclatura do Data Factory](naming-rules.md).
  
   ![Nome da nova fábrica de dados](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Selecione a **subscrição** do Azure na qual quer criar a fábrica de dados. 
4. Em **Grupo de Recursos**, efetue um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente.

      - Selecione **Criar novo** e introduza o nome de um grupo de recursos. 
         
      Para saber mais sobre grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).
5. Em **Versão**, selecione **V2 (Pré-visualização)**.
6. Em **Localização**, selecione a localização para a fábrica de dados. Só aparecem na lista pendente as localizações que são suportadas. Os arquivos de dados (por exemplo, o Armazenamento do Azure e a Base de Dados SQL) e as computações (por exemplo, o Azure HDInsight) que o Data Factory utiliza podem estar noutras localizações/regiões.
7. Selecione **Afixar ao dashboard**. 
8. Selecione **Criar**.
9. No dashboard, é apresentado o mosaico seguinte, com o estado **A Implementar o Data Factory**:

    ![Mosaico A Implementar o Data Factory](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
10. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
    ![Home page da fábrica de dados](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
11. Selecione **Criar e Monitorizar** para iniciar a interface de utilizador do Data Factory num separador à parte. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Utilizar a ferramenta Copiar Dados para criar um pipeline

1. Na página **Vamos começar**, selecione a página **Copiar Dados** para iniciar a ferramenta Copiar Dados. 

   ![Mosaico ferramenta Copiar Dados](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Na página **Propriedades** da ferramenta Copiar Dados, em **Nome da tarefa**, introduza **CopyFromOnPremSqlToAzureBlobPipeline**. Em seguida, selecione **Seguinte**. A ferramenta Copiar Dados cria um pipeline com o nome que especificar para este campo. 
    
   ![Nome da tarefa](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. Na página **Arquivo de dados de origem**, selecione **SQL Server** e, em seguida, selecione **Seguinte**. Poderá ter de deslocar para baixo para ver o **SQL Server** na lista. 

   ![Seleção do SQL Server](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Em **Nome da ligação**, introduza **SqlServerLinkedService**. Selecione a ligação **Criar Integration Runtime**. Tem de criar um runtime de integração autoalojado, transferi-lo para o seu computador e registá-lo no Data Factory. O runtime de integração autoalojado copia dados entre o ambiente no local e a cloud.

   ![Criar o runtime de integração autoalojado](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. Na caixa de diálogo **Criar Integration Runtime**, em **Nome**, introduza **TutorialIntegration Runtime**. Em seguida, selecione **Criar**. 

   ![Nome do runtime de Integração](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Selecione **Iniciar a configuração rápida neste computador**. Esta ação instala o runtime de integração no seu computador e regista-o no Data Factory. Em alternativa, pode utilizar a opção de configuração manual para transferir o ficheiro de instalação, executá-lo e utilizar a chave para registar o runtime de integração. 

    ![Ligação para iniciar a configuração rápida neste computador](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Execute a aplicação transferida. Pode ver o estado da configuração rápida na janela. 

    ![Estado da configuração rápida](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Confirme que **TutorialIntegrationRuntime** está selecionado para o campo **Integration Runtime**.

    ![Runtime de integração selecionado](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. Em **Especificar a base de dados do SQL Server no local**, siga os passos seguintes: 

    a. Em **Nome da ligação**, introduza **OnPremSqlLinkedService**.

    b. Em **Nome do servidor**, introduza o nome da instância do SQL Server no local.

    c. Em **Nome da base de dados**, introduza o nome da base de dados no local.

    d. Em **Tipo de autenticação**, selecione a autenticação adequada.

    e. Em **Nome de utilizador**, introduza o nome do utilizador com acesso ao SQL Server no local.

    f. Introduza a **palavra-passe** do utilizador. 
10. Na página **Selecionar tabelas a partir das quais copiar dados ou utilizar uma consulta personalizada**, selecione a tabela **[dbo].[emp]** da lista e selecione **Seguinte**. 

    ![Seleção de tabela emp](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. Na página **Arquivo de dados de destino**, selecione **Armazenamento de Blobs do Azure** e, em seguida, selecione **Seguinte**.

    ![Seleção do Armazenamento de blobs](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. Na página **Especificar a conta de Armazenamento de blobs do Azure**, siga os passos seguintes: 

    a. Em **Nome da ligação**, introduza **AzureStorageLinkedService**.

    b. Em **Nome da conta de armazenamento**, selecione a conta de armazenamento da lista pendente. 

    c. Selecione **Seguinte**.

    ![Especificar a conta de armazenamento](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. Na página **Escolher ficheiro ou pasta de saída**, em **Caminho da pasta**, introduza **adftutorial/fromonprem**. Criou o contentor **adftutorial** como parte dos pré-requisitos. Se a pasta de saída não existir, o Data Factory cria-a automaticamente. Também pode utilizar o botão **Procurar** para procurar no Armazenamento de blobs e nos respetivos contentores/pastas. Tenha em atenção que o nome do ficheiro de saída está definido como **dbo.emp** por predefinição.
        
    ![Escolher ficheiro ou pasta de saída](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. Na página **Definições do formato do ficheiro**, selecione **Seguinte**. 

    ![Página de definições do formato do ficheiro](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. Na página **Definições**, selecione **Seguinte**. 

    ![Página de definições](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. Na página **Resumo**, reveja os valores para todas definições e selecione **Seguinte**. 

    ![Página de resumo](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. Na página **Implementação**, selecione **Monitorização** para monitorizar o pipeline ou a tarefa que criou.

    ![Página de implementação](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. No separador **Monitorização**, pode ver o estado do pipeline que criou. Pode utilizar as ligações na coluna **Ações** para ver as execuções de atividades associadas à execução do pipeline e voltar a executar o pipeline. 

    ![Monitorizar execuções de pipeline](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Selecione a ligação **Ver Execuções de Atividade** na coluna **Ações** para ver as execuções de atividades associadas à execução do pipeline. Para ver os detalhes da operação de cópia, selecione a ligação **Detalhes** (ícone de óculos), na coluna **Ações**. Para regressar à vista **Execuções do Pipeline**, selecione **Pipelines** na parte superior.

    ![Monitorização de execuções de atividade](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Confirme que vê um ficheiro de saída na pasta **fromonprem** do contentor **adftutorial**. 
 
    ![Blob de saída](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Selecione o separador **Editar** à esquerda para mudar para o modo de edição. Pode atualizar os serviços ligados, conjuntos de dados e pipelines criados pela ferramenta, utilizando o editor. Selecione **Código** para ver o código JSON associado à entidade aberta no editor. Para obter detalhes sobre como editar estas entidades na IU do Data Factory, veja [a versão do portal do Azure deste tutorial](tutorial-copy-data-portal.md).

    ![Separador Editar](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia os dados de uma base de dados do SQL Server no local para o Armazenamento de blobs. Aprendeu a: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Utilizar a ferramenta Copiar Dados para criar um pipeline.
> * Monitorizar o pipeline e execuções de atividades.

Para obter uma lista dos arquivos de dados que o Data Factory suporta, veja [Arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).

Para saber como copiar dados em massa de uma origem para um destino, avance para o tutorial seguinte:

> [!div class="nextstepaction"]
>[Copiar dados em massa](tutorial-bulk-copy-portal.md)
