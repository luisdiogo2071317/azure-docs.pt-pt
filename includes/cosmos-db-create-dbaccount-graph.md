---
title: incluir ficheiro
description: incluir ficheiro
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 88751efdd5aaceddeed490c95d15d82a263fa81e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31429199"
---
1. Numa nova janela do browser, inicie sessão no [portal do Azure](https://portal.azure.com/).

2. Clique em **Criar um recurso** > **Bases de dados** > **Azure Cosmos DB**.
   
   ![Painel de "Bases de dados" portal do Azure](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. Na página **Nova conta**, introduza as definições para a nova conta do Azure Cosmos DB. 

    Definição|Valor sugerido|Descrição
    ---|---|---
    ID|*Introduzir um nome exclusivo*|Introduza um nome exclusivo para identificar esta conta do Azure Cosmos DB. Uma vez que *documents.azure.com* é anexado ao ID que indicar para criar o seu URI, utilize um ID exclusivo, mas identificável.<br><br>O ID pode conter apenas minúsculas, números, o caráter hífen (-) e tem de ter entre 3 e 50 carateres.
    API|Gremlin (gráfico)|A API determina o tipo de conta a criar. O Azure do Cosmos DB oferece cinco APIs que se adequam às necessidades da sua aplicação: SQL Server (base de dados do documento), Gremlin (base de dados do gráfico), MongoDB (base de dados do documento), Tabela do Azure e Cassandra, cada qual necessita atualmente de uma conta separada. <br><br>Selecione **Gremlin (gráfico)** porque neste guia de introdução que está a criar um gráfico que é consultável utilizando sintaxe Gremlin.<br><br>[Saiba mais sobre a Graph API](../articles/cosmos-db/graph-introduction.md)
    Subscrição|*A sua subscrição*|Selecione a subscrição do Azure que quer utilizar para esta conta do Azure Cosmos DB. 
    Grupo de Recursos|Criar novo<br><br>*Em seguida, introduza o mesmo nome exclusivo, conforme indicado acima ID*|Selecione **criar novo**, em seguida, introduza um novo nome de grupo de recursos para a sua conta. Para simplicidade, pode utilizar o mesmo nome do ID.
    Localização|*Selecione a região mais próxima dos seus utilizadores*|Selecione a localização geográfica na qual vai alojar a sua conta do Azure Cosmos DB. Utilize a localização mais próxima dos seus utilizadores para lhes dar o acesso mais rápido aos dados.
    Ativar redundância geográfica| Deixar em branco | Esta ação cria uma versão replicada da base de dados numa segunda região (emparelhada). Deixe em branco.  
    Afixar ao dashboard | Seleccionar | Selecione esta caixa para que a sua nova conta da base de dados seja adicionada ao dashboard do portal para facilitar o acesso.

    Em seguida, clique em **Criar**.

    ![O painel da nova conta do Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-create-new-account.png)

4. A criação da conta demora alguns minutos. Aguarde que o portal apresentar o **Parabéns! Foi criada a sua conta de base de dados do Azure Cosmos** página.

    ![O painel de Notificações de portal do Azure](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-graph-created.png)