---
title: Copiar dados do SQL Server para o Armazenamento de Blobs com o Azure Data Factory | Microsoft Docs
description: "Saiba como copiar dados de um arquivo de dados no local para a cloud do Azure mediante a utilização de um integration runtime autoalojado no Azure Data Factory."
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
ms.date: 01/11/2018
ms.author: jingwang
ms.openlocfilehash: 64cd758e2f40ff2b18abbff1194a7e57389d8a54
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Tutorial: copiar dados de uma base de dados SQL Server no local para o Armazenamento de Blobs do Azure
Neste tutorial, vai utilizar a interface de utilizador (IU) do Azure Data Factory para criar um pipeline de fábrica de dados que copia dados de uma base de dados do SQL Server no local para o armazenamento de Blobs do Azure. Vai criar e utilizar um runtime de integração autoalojado, que move dados entre arquivos de dados no local e na cloud. 

> [!NOTE]
> Este artigo aplica-se à versão 2 do Azure Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [documentação da versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Este artigo não disponibiliza uma introdução detalhada do serviço Data Factory. Para obter mais informações, veja [Introdução ao Azure Data Factory](introduction.md). 

Neste tutorial, vai executar os seguintes passos:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Crie um integration runtime autoalojado.
> * Criar serviços ligados do SQL Server e do Armazenamento do Azure. 
> * Criar conjuntos de dados do SQL Server e dos Blobs do Azure.
> * Criar um pipeline com uma atividade de cópia para mover os dados.
> * Iniciar uma execução de pipeline.
> * Monitorizar a execução do pipeline.

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

5. Para criar a tabela **emp** e inserir alguns dados de exemplo na mesma, execute o script de consulta seguinte na base de dados:

   ```
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

6. Na vista de árvore, clique com o botão direito do rato na base de dados que criou e selecione **Nova Consulta**.

### <a name="azure-storage-account"></a>Conta de armazenamento do Azure
Neste tutorial, vai utilizar uma Conta de Armazenamento do Azure de fins gerais (mais concretamente, o Armazenamento de Blobs do Azure) como arquivo de dados de destino/sink. Se não tiver uma conta de armazenamento do Azure para fins gerais, veja [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account). O pipeline da fábrica de dados que vai criar neste tutorial copia dados da base de dados do SQL Server no local (origem) para este armazenamento de Blobs do Azure (sink). 

#### <a name="get-storage-account-name-and-account-key"></a>Obter o nome e a chave da conta de armazenamento
Utilize o nome e a chave da sua conta de armazenamento do Azure neste tutorial. Obtenha o nome e a chave da sua conta de armazenamento da seguinte forma: 

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
Neste passo, vai criar uma fábrica de dados e iniciar a IU do Azure Data Factory para criar um pipeline nessa fábrica. 

1. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-hybrid-copy-portal/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Página Nova fábrica de dados](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
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

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a IU do Azure Data Factory num separador à parte. 


## <a name="create-a-pipeline"></a>Criar um pipeline

1. Na página **Introdução** , clique em **Criar pipeline**. É criado um pipeline automaticamente por si. Verá o pipeline na vista de árvore e o respetivo editor aberto. 

   ![Página Introdução](./media/tutorial-hybrid-copy-portal/get-started-page.png)
2. No separador **Geral** da janela **Propriedades**, na parte inferior, e introduza **SQLServerToBlobPipeline** em **Nome**.

   ![Nome do pipeline](./media/tutorial-hybrid-copy-portal/pipeline-name.png)
2. Na caixa de ferramentas **Atividades**, expanda **Fluxo de Dados** e arraste e largue a atividade **Copy** na superfície de desenho do pipeline. Defina o nome da atividade como **CopySqlServerToAzureBlobActivity**.

   ![Nome da atividade](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)
3. Na janela Propriedades, mude para o separador **Origem** e clique em **+ Novo**.

   ![Novo conjunto de dados de origem - botão](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)
4. Na janela **Novo Conjunto de Dados**, procure **SQL Server**, selecione **SQL Server** e clique em **Concluir**. Verá um separador novo com o nome **SqlServerTable1**. Verá também o conjunto de dados **SqlServerTable1** na vista de árvore, do lado esquerdo. 

   ![Selecionar o SQL Server](./media/tutorial-hybrid-copy-portal/select-sql-server.png)
5. No separador **Geral** da janela Propriedades, introduza **SqlServerDataset** em **Nome**.
    
   ![Conjunto de Dados de Origem - nome](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)
6. Mude para o separador **Ligações** e clique em **+ Novo**. Neste passo, vai criar uma ligação para o arquivo de dados de origem (base de dados do SQL Server). 

   ![Conjunto de Dados de Origem - botão ligação nova](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)
7. Na janela **Novo Serviço Ligado**, clique em **Novo runtime de integração**. Nesta secção, vai criar um integration runtime autoalojado e vai associá-lo a um computador no local com a base de dados do SQL Server. O integration runtime autoalojado é o componente que copia os dados da base de dados SQL Server no seu computador para o armazenamento de Blobs do Azure. 

   ![Novo runtime de integração](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)
8. Na janela **Configuração do Runtime de Integração**, selecione **Rede Privada** e clique em **Seguinte**. 

   ![Selecionar rede privada](./media/tutorial-hybrid-copy-portal/select-private-network.png)
9. Introduza um nome para o runtime de integração e clique em **Seguinte**.  
    
   ![Runtime de Integração - nome](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)
10. Clique em **Clique aqui para iniciar a configuração rápida neste computador**, na secção **Opção 1: Configuração Rápida**. 

   ![Clicar na ligação de Configuração Rápida](./media/tutorial-hybrid-copy-portal/click-exress-setup.png)
11. Na janela **Configuração Rápida do Integration Runtime (Autoalojado)**, clique em **Fechar**. 

   ![Configuração do runtime de integração - êxito](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)
12. No browser, na janela **Configuração do Runtime de Integração**, clique em **Concluir**. 

   ![Configuração do runtime de integração - conclusão](./media/tutorial-hybrid-copy-portal/click-finish-integration-runtime-setup.png)
13. Na janela **Novo Serviço Ligado**, siga os passos abaixo:

    1. Introduza **SqlServerLinkedService** em **Nome**.
    2. Confirme que o runtime de integração autoalojado que criou anteriormente aparece em **Ligar através do runtime de integração**.
    3. Especifique o nome do SQL Server em **Nome do servidor**. 
    4. Especifique o nome da base de dados com a tabela **emp** no campo **Nome da base de dados**. 
    5. Selecione o **tipo de autenticação** adequado que o Data Factory deverá utilizar para se ligar à sua base de dados do SQL Server. 
    6. Introduza o **nome do utilizador** e a **palavra-passe**. Se precisar de utilizar um caráter de barra invertida (\\) no nome da sua conta de utilizador ou no nome do seu servidor, utilize o caráter de escape (\\) como prefixo. Por exemplo, utilize *omeudominion\\\\omeuutilizador*. 
    7. Clique em **Testar ligação**. Execute este passo para confirmar que o serviço Data Factory se consegue ligar à sua base de dados do SQL Server com o runtime de integração autoalojado que criou. 
    8. Para guardar o serviço ligado, clique em **Guardar**.

       ![Serviço ligado do SQL Server - definições](./media/tutorial-hybrid-copy-portal/sql-server-linked-service-settings.png)
14. Deverá regressar à janela com o conjunto de dados de origem aberto. Em **Ligação**, na janela **Propriedades**, siga os passos abaixo: 

    1. Confirme que vê **SqlServerLinkedService** em **Serviço ligado**. 
    2. Selecione **[dbo].[emp]** em **Tabela**.

        ![Conjunto de dados de origem - informações da ligação](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)
15. Mude para o separador com SQLServerToBlobPipeline (ou) clique em **SQLServerToBlobPipeline**, na vista de árvore. 

    ![Separador do pipeline](./media/tutorial-hybrid-copy-portal/pipeliene-tab.png)
16. Mude para o separador **Sink**, na janela **Propriedades** e clique em **+ Novo**. 

    ![Conjunto de dados de sink - botão novo](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)
17. Na janela **Novo Conjunto de Dados**, selecione **Armazenamento de Blobs do Azure** e clique em **Concluir**. Verá um novo separador aberto para o conjunto de dados. Também verá o conjunto de dados na vista de árvore. 

    ![Selecionar Armazenamento de Blobs do Azure](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)
18. Introduza **AzureBlobDataset** em **Nome**.

    ![Conjunto de dados de sink - nome](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)
19. Mude para o separador **Ligação**, na janela **Propriedades** e clique em **+ Novo**, em **Serviço ligado**. 

    ![Botão Novo serviço ligado](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)
20. Na janela **Novo Serviço Ligado**, siga os passos abaixo:

    1. Introduza **AzureStorageLinkedService** em **Nome**.
    2. Selecione a sua conta de Armazenamento do Azure em **Nome da conta de armazenamento**. 
    3. Clique em **Testar ligação** para testar a ligação à sua conta de Armazenamento do Azure.
    4. Clique em **Guardar**.

        ![Serviço ligado do Armazenamento do Azure - definições](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 
21.  Deverá regressar à janela com o conjunto de dados de sink aberto. No separador **Ligação**, siga os passos abaixo: 

        1. Confirme que **AzureStorageLinkedService** está selecionado em **Serviço ligado**.
        2. Introduza **adftutorial/fromonprem** na parte **folder** do **Caminho do ficheiro**. Se a pasta de saída não existe no contentor adftutorial, o serviço Data Factory cria-a automaticamente.
        3. Introduza `@CONCAT(pipeline().RunId, '.txt')` na parte **file name** do **Caminho do ficheiro**.

            ![Conjunto de dados de sink - ligação](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)
22. Mude para o separador com o pipeline aberto (ou) clique no **pipeline** na **vista de árvore**. Confirme que **AzureBlobDataset** está selecionado em **Conjunto de Dados de Sink**. 

    ![Conjunto de dados de sink selecionado ](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)
23. Para validar as definições do pipeline, clique em Validar, na barra de ferramentas do mesmo. Clique em **X**, no canto direito, para fechar o **Relatório de Validação de Pipeline**. 

    ![Validar o pipeline](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
1. Clique em **Publicar** para publicar as entidades que criou no serviço Azure Data Factory.

    ![Botão Publicar](./media/tutorial-hybrid-copy-portal/publish-button.png)
24. Aguarde até ver a mensagem **Publicação com êxito**. Também pode clicar na ligação **Mostrar Notificações**, no lado esquerdo, para ver o estado da publicação. Clique em **X** para fechar a janela de notificação. 

    ![Publicação com êxito](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)
    

## <a name="trigger-a-pipeline-run"></a>Acionar uma execução de pipeline
Clique em **Acionar**, na barra de ferramentas do pipeline, e clique em **Acionar Agora**.

![Acionar Agora](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>Monitorizar a execução do pipeline.

1. Mude para o separador **Monitorizar**. Verá o pipeline que acionou manualmente no passo anterior. 

    ![Execuções de pipeline](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
2. Para ver as execuções de atividades associadas à execução do pipeline, clique na ligação **Ver Execuções de Atividades**, na coluna **Ações**. Vai ver apenas execuções de atividades, porque só existe uma atividade no pipeline. Para ver os detalhes da operação de cópia, clique na ligação **Detalhes** (ícone de óculos), na coluna **Ações** Pode clicar em **Pipelines**, na parte superior, para regressar à vista de execuções de pipelines.

    ![Execuções de atividade](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>Verificar a saída
O pipeline cria automaticamente a pasta de saída com o nome *fromonprem* no contentor de blobs `adftutorial`. Confirme que consegue ver o ficheiro *dbo.emp.txt* na pasta de saída. 

1. No portal do Azure, na janela do contentor **adftutorial**, selecione **Atualizar** para ver a pasta de saída.

    ![Pasta de saída criada](media/tutorial-hybrid-copy-portal/fromonprem-folder.png)
2. Selecione `fromonprem` na lista de pastas. 
3. Confirme que vê um ficheiro com o nome `dbo.emp.txt`.

    ![Ficheiro de saída](media/tutorial-hybrid-copy-portal/fromonprem-file.png)


## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia dados de uma localização para outra localização no amazenamento de Bobs do Azure. Aprendeu a:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Crie um integration runtime autoalojado.
> * Criar serviços ligados do SQL Server e do Armazenamento do Azure. 
> * Criar conjuntos de dados do SQL Server e dos Blobs do Azure.
> * Criar um pipeline com uma atividade de cópia para mover os dados.
> * Iniciar uma execução de pipeline.
> * Monitorizar a execução do pipeline.

Para obter uma lista dos arquivos de dados que o Data Factory suporta, veja [Supported data stores](copy-activity-overview.md#supported-data-stores-and-formats) (Arquivos de Dados suportados).

Para aprender a copiar dados em massa de uma origem para um destino, avance para o tutorial seguinte:

> [!div class="nextstepaction"]
>[Copiar dados em massa](tutorial-bulk-copy-portal.md)
