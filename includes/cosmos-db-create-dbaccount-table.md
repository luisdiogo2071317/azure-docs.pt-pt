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
ms.openlocfilehash: 0a9f2aaa4bc6422d4e8a86373b5e578c5bd65b4c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38726138"
---
1. Numa nova janela do browser, inicie sessão no [portal do Azure](https://portal.azure.com/).
2. No menu esquerdo, clique em **Criar um recurso**, **Bases de Dados**e, em **Azure Cosmos DB** e **Criar**. 
   
   ![Captura de ecrã do portal do Azure, com destaque para Mais Serviços e Azure Cosmos DB](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)

3. Na página **Nova conta**, introduza as definições para a nova conta do Azure Cosmos DB. 
 
    Definição|Valor sugerido|Descrição
    ---|---|---
    ID|*Introduzir um nome exclusivo*|Introduza um nome exclusivo para identificar esta conta do Azure Cosmos DB. Uma vez que *documents.azure.com* é anexado ao ID que indicar para criar o seu URI, utilize um ID exclusivo, mas identificável.<br><br>O ID pode conter apenas minúsculas, números, o caráter hífen (-) e tem de ter entre 3 e 50 carateres.
    API|Tabela do Azure|A API determina o tipo de conta a criar. O Azure do Cosmos DB oferece cinco APIs que se adequam às necessidades da sua aplicação: SQL Server (base de dados do documento), Gremlin (base de dados do gráfico), MongoDB (base de dados do documento), Tabela do Azure e Cassandra, cada qual necessita atualmente de uma conta separada.<br><br>Selecione **Tabela do Azure** porque neste início rápido está a criar uma tabela que funciona com a API da Tabela.<br><br>[Saber mais sobre a API da Tabela](../articles/cosmos-db/table-introduction.md) |
    Subscrição|*Introduzir o mesmo nome exclusivo, conforme indicado no ID acima*|Selecione a subscrição do Azure que quer utilizar para esta conta do Azure Cosmos DB. 
    Grupo de Recursos|Criar novo<br><br>*Em seguida, introduza o mesmo nome exclusivo, conforme indicado no ID acima*|Selecione **Criar Novo** e, em seguida, introduza um novo nome de grupo de recursos para a sua conta. Para simplicidade, pode utilizar o mesmo nome do ID.
    Localização|*Selecione a região mais próxima dos seus utilizadores*|Selecione a localização geográfica na qual vai alojar a sua conta do Azure Cosmos DB. Utilize a localização mais próxima dos seus utilizadores para lhes dar o acesso mais rápido aos dados.
    Ativar redundância geográfica| Deixar em branco | Esta ação cria uma versão replicada da base de dados numa segunda região (emparelhada). Deixe em branco.  
    Afixar ao dashboard | Selecione | Selecione esta caixa para que a sua nova conta da base de dados seja adicionada ao dashboard do portal para facilitar o acesso.

    Em seguida, clique em **Criar**.

    ![A página da nova conta do Azure Cosmos DB](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png)

4. A criação da conta demora alguns minutos. Aguarde que o portal apresente a página **Parabéns! A sua conta do Azure Cosmos DB foi criada**.

    ![O painel de Notificações de portal do Azure](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png)
