---
title: Gerir o Azure Cosmos DB no Explorador de Armazenamento do Azure
description: Saiba como gerir o Azure Cosmos DB no Explorador de Armazenamento do Azure.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: ''
author: Jejiang
manager: omafnan
editor: ''
tags: Azure Cosmos DB
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2018
ms.author: jejiang
ms.openlocfilehash: 18f580f1eae31c9bf3626e100217467bb48ca881
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Gerir o Azure Cosmos DB no Explorador de Armazenamento do Azure (Pré-visualização)

A utilização do Azure Cosmos DB no Explorador de Armazenamento do Azure permite aos utilizadores gerir as entidades do Azure Cosmos DB, manipular dados, atualizar os procedimentos e acionadores armazenados, bem como outras entidades do Azure, como blobs e filas de Armazenamento. Agora, pode utilizar a mesma ferramenta para gerir as diferentes entidades do Azure num único local. Neste momento, o Explorador de Armazenamento do Azure suporta contas do SQL, MongoDB, Graph e de Tabela.

Neste artigo, pode saber como utilizar o Explorador de Armazenamento para gerir o Azure Cosmos DB.


## <a name="prerequisites"></a>Pré-requisitos

Uma conta do Azure Cosmos DB para API do SQL<!--or MongoDB API-->. Se não tiver uma conta, pode criar uma no portal do Azure, conforme descrito em [Azure Cosmos DB: criar uma aplicação Web da SQL API com .NET e o portal do Azure](create-sql-api-dotnet.md).

## <a name="installation"></a>Instalação

Instale os bits mais recentes do Explorador de Armazenamento do Azure aqui: [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/), suportamos agora a versão para Windows, Linux e MAC.

## <a name="connect-to-an-azure-subscription"></a>Ligar a uma subscrição do Azure

1. Depois de instalar o **Explorador de Armazenamento do Azure**, clique no ícone de **plug-in** à esquerda, conforme mostrado na imagem seguinte:
       
   ![Ícone de plug-in](./media/storage-explorer/plug-in-icon.png)
 
2. Selecione **Adicionar uma Conta do Azure** e clique em **Iniciar sessão**.

   ![Ligar a uma subscrição do Azure](./media/storage-explorer/connect-to-azure-subscription.png)

2. Na caixa de diálogo **Início de sessão do Azure**, selecione **Iniciar sessão** e, em seguida, introduza as credenciais do Azure.

    ![Iniciar sessão](./media/storage-explorer/sign-in.png)

3. Selecione a sua subscrição na lista e clique em **Aplicar**.

    ![Aplicar](./media/storage-explorer/apply-subscription.png)

    O painel do Explorador atualiza e apresenta as contas na subscrição selecionada.

    ![Lista de contas](./media/storage-explorer/account-list.png)

    Ligou com êxito a **conta do Cosmos DB** à subscrição do Azure.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Ligar ao Azure Cosmos DB através de uma cadeia de ligação

Uma forma alternativa de ligar ao Azure Cosmos DB é utilizar uma cadeia de ligação. Utilize os seguintes passos para ligar através de uma cadeia de ligação.

1. Localize **Local e Ligada** na árvore à esquerda, clique com o botão direito do rato em **Contas do Cosmos DB** e selecione **Ligar ao Cosmos DB...**

    ![Ligar ao Cosmos DB através de uma cadeia de ligação](./media/storage-explorer/connect-to-db-by-connection-string.png)

2. Atualmente, apenas suporta a API do SQL e de Tabela. Escolha a API, cole a **Cadeia de Ligação**, introduza a **Etiqueta de conta**, clique em **Seguinte** para verificar o resumo e, em seguida, clique em **Ligar** para ligar a conta do Azure Cosmos DB. Para obter informações sobre como obter a cadeia de ligação, veja [Obter a cadeia de ligação](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Connection-string](./media/storage-explorer/connection-string.png)

## <a name="connect-to-azure-cosmos-db-by-using-local-emulator"></a>Ligar ao Azure Cosmos DB através de um emulador local
Utilize os seguintes passos para ligar ao Azure Cosmos DB pelo Emulador, só suporta atualmente a conta de SQL.
1. Instale o Emulador e inicie. Para saber como instalar o Emulador, veja [Emulador do Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator)
2. Localize **Local e Ligada** na árvore à esquerda, clique com o botão direito do rato em **Contas do Cosmos DB** e selecione **Ligar ao Emulador do Cosmos DB...**

    ![Ligar ao Cosmos DB pelo Emulador](./media/storage-explorer/emulator-entry.png)

3. Atualmente, apenas suporta a API do SQL. Cole a **Cadeia de Ligação**, introduza a **Etiqueta de conta**, clique em **Seguinte** para verificar o resumo e, em seguida, clique em **Ligar** para ligar a conta do Azure Cosmos DB. Para obter informações sobre como obter a cadeia de ligação, veja [Obter a cadeia de ligação](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Ligar ao Cosmos DB através da caixa de diálogo Emulador](./media/storage-explorer/emulator-dialog.png)



## <a name="azure-cosmos-db-resource-management"></a>Gestão de recursos do Azure Cosmos DB

Pode gerir uma conta do Azure Cosmos DB ao efetuar as seguintes operações:
* Abra a conta no portal do Azure
* Adicione o recurso à Lista de Acesso Rápido
* Procure e atualize os recursos
* Criar e eliminar bases de dados
* Criar e eliminar coleções
* Crie, edite, elimine e filtre documentos
* Faça a gestão de procedimentos armazenados, acionadores e funções definidas pelo utilizador

### <a name="quick-access-tasks"></a>Tarefas de acesso rápido

Ao clicar com o botão direito do rato numa subscrição no painel do Explorador, pode efetuar muitas tarefas de ação rápida:

* Clique com o botão direito do rato numa conta do Azure Cosmos DB ou numa base de dados, selecione **Abrir no Portal** e faça a gestão dos recursos no browser no portal do Azure.

     ![Abrir no Portal](./media/storage-explorer/open-in-portal.png)

* Também pode adicionar a conta do Azure Cosmos DB, a base de dados e a coleção ao **Acesso Rápido**.
* **Procurar a partir daqui** permite procurar por palavra-chave no caminho selecionado.

    ![procurar a partir daqui](./media/storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Gestão de bases de dados e coleções
#### <a name="create-a-database"></a>Criar uma base de dados 
-   Clique com o botão direito do rato na conta do Azure Cosmos DB, selecione **Criar Base de Dados**, introduza o nome da base de dados e prima **Enter** para concluir.
       
    ![Criar base de dados](./media/storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Eliminar uma base de dados
- Clique com o botão direito do rato na base de dados, clique em **Eliminar Base de Dados** e em **Sim** na janela de pop-up. O nó da base de dados é eliminado e a conta do Azure Cosmos DB é atualizada automaticamente.

    ![Eliminar base de dados1](./media/storage-explorer/delete-database1.png)  

    ![Eliminar base de dados2](./media/storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Criar uma coleção
1. Clique com o botão direito do rato na base de dados, selecione **Criar Coleção** e, em seguida, forneça as seguintes informações, como **ID da Coleção**, **Capacidade de armazenamento**, etc. Clique em **OK** para concluir. 

    ![Criar coleção1](./media/storage-explorer/create-collection.png)

    ![Criar coleção2](./media/storage-explorer/create-collection2.png) 

2. Selecione **Ilimitado** para poder especificar a chave de partição, em seguida, clique em **OK** para concluir.

    Se for utilizada uma chave de partição quando criar uma coleção, depois de concluída a criação, o valor da chave de partição não pode ser alterado na coleção. Para obter informações sobre definições de chaves de partição, veja [Estruturar a criação de partições](partition-data.md#designing-for-partitioning).

    ![Chave de partição](./media/storage-explorer/partitionkey.png)

#### <a name="delete-a-collection"></a>Eliminar uma coleção
- Clique com o botão direito do rato na coleção, clique em **Eliminar Coleção** e em **Sim** na janela de pop-up. 

    O nó da coleção é eliminado e a base de dados é atualizada automaticamente.

    ![Eliminar coleção](./media/storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Gestão de documentos

#### <a name="create-and-modify-documents"></a>Criar e modificar documentos
- Para criar um novo documento, abra **Documentos** na janela à esquerda, clique em **Novo Documento**, edite o conteúdo no painel direito e, em seguida, clique em **Guardar**. Também pode atualizar um documento existente e, em seguida, clicar em **Guardar**. As alterações podem ser eliminadas ao clicar em **Eliminar**.

    ![Documento](./media/storage-explorer/document.png)

#### <a name="delete-a-document"></a>Eliminar um documento
- Clique no botão **Eliminar** para eliminar o documento selecionado.

#### <a name="query-for-documents"></a>Consulta de documentos
- Edite o filtro de documentos ao introduzir uma [consulta SQL](sql-api-sql-query.md) e, em seguida, clique em **Aplicar**.

    ![Filtro de Documento](./media/storage-explorer/document-filter.png)



### <a name="graph-management"></a>Gestão de gráficos

#### <a name="create-and-modify-vertex"></a>Criar e modificar um vértice
1. Para criar um novo vértice, abra o **Graph** na janela esquerda, clique em **Vértice Novo**, edite o conteúdo e clique em **OK**.    
2. Para modificar um vértice existente, clique no ícone de caneta no painel direito.   

    ![Graph](./media/storage-explorer/vertex.png)

#### <a name="delete-a-graph"></a>Eliminar um gráfico
- Para eliminar um vértice, clique no ícone de reciclagem junto ao nome do vértice.

#### <a name="filter-for-graph"></a>Filtro para gráfico
- Edite o filtro de gráfico ao introduzir uma [consulta gremlin](gremlin-support.md) e, em seguida, clique em **Aplicar Filtro**.

    ![Filtro de Gráfico](./media/storage-explorer/graph-filter.png)

### <a name="table-management"></a>Gestão de tabelas

#### <a name="create-and-modify-table"></a>Criar e modificar tabela
1. Para criar uma nova tabela, abra **Entidades** na janela esquerda, clique em **Adicionar**, edite o conteúdo na caixa de diálogo **Adicionar Entidade**, adicione a propriedade ao clicar no botão **Adicionar Propriedade** e clique em **Inserir**.
2. Para modificar uma tabela, clique em **Editar**, modifique o conteúdo e clique em **Atualizar**.

    ![Tabela](./media/storage-explorer/table.png)

#### <a name="import-and-export-table"></a>Importar e exportar tabela
1. Para importar, clique no botão **Importar** e escolha uma tabela existente.
2. Para exportar, clique no botão **Exportar** e escolha um destino.

    ![Importação e Exportação de Tabela](./media/storage-explorer/table-import-export.png)

#### <a name="delete-entities"></a>Eliminar entidades
- Selecione as entidades e clique no botão **Eliminar**.

    ![Eliminação da tabela](./media/storage-explorer/table-delete.png)

#### <a name="query-table"></a>Tabela de consulta
- Clique no botão **Consulta**, introduza a condição de consulta e clique no botão **Executar Consulta**. Feche o painel de Consulta, ao clicar no botão **Fechar Consulta**.

    ![Consulta de Tabela](./media/storage-explorer/table-query.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Gerir procedimentos armazenados, acionadores e UDFs
* Para criar um procedimento armazenado, na árvore à esquerda, clique com o botão direito do rato em **Procedimento Armazenado**, selecione **Criar Procedimento Armazenado**, introduza um nome à esquerda, escreva os scripts do procedimento armazenado na janela à direita e, em seguida, clique em **Criar**. 
* Também pode editar procedimentos armazenados existentes ao fazer duplo clique, ao efetuar a atualização e, em seguida, ao clicar em **Atualizar** para guardar ou em **Eliminar** para cancelar a alteração.

    ![Procedimento armazenado](./media/storage-explorer/stored-procedure.png)
* As operações de **Acionadores** e **UDF** são semelhantes às dos **Procedimentos Armazenados**.

## <a name="next-steps"></a>Passos seguintes

* Veja o vídeo seguinte para ver como utilizar o Azure Cosmos DB no Explorador de Armazenamento do Azure: [Utilizar o Azure Cosmos DB no Explorador de Armazenamento do Azure](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Saiba mais sobre o Explorador de Armazenamento e como ligar mais serviços em [Introdução ao Explorador de Armazenamento (Pré-visualização)](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

