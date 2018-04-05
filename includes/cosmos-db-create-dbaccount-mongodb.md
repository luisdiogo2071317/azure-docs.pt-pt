---
title: Criar uma conta da API do MongoDB do Azure Cosmos DB
description: Descreve como criar uma conta da API do MongoDB do Azure Cosmos DB no portal do Azure
services: cosmos-db
author: mimig1
ms.service: cosmos-db
ms.topic: include
ms.date: 03/20/2018
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: 02ea0e011642313b885bc48ec48104fa2789da81
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
1. Numa nova janela, inicie sessão no [portal do Azure](https://portal.azure.com/).
2. No menu esquerdo, clique em **Criar um recurso**, **Bases de Dados**e, em **Azure Cosmos DB** e **Criar**.
   
   ![Captura de ecrã do portal do Azure, com destaque para Mais Serviços e Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. No painel **Nova conta**, especifique **MongoDB** como a API e preencha a configuração pretendida para a conta do Azure Cosmos DB.
 
    ![Captura de ecrã do painel Novo Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)

    * O **ID** tem de ser um nome exclusivo que vai utilizar para identificar a sua conta do Azure Cosmos DB. Pode conter apenas minúsculas, números, o caráter "-" e tem de ter entre três e 50 carateres.
    * A **Subscrição** é a sua subscrição do Azure. Será preenchida para si.
    * O **Grupo de Recursos** é o nome do grupo de recursos para a sua conta do Azure Cosmos DB.
    * A **Localização** é a localização geográfica onde está localizada a sua instância da instância do Azure Cosmos DB. Escolha a localização mais próxima dos seus utilizadores.

4. Clique em **Criar** para criar a conta.
5. Na barra de ferramentas, clique em **Notificações** para monitorizar o processo de implementação.

    ![Notificação de implementação iniciada](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  Quando a implementação estiver concluída, abra a conta nova no mosaico Todos os Recursos. 

    ![Conta do Azure Cosmos DB no mosaico Todos os Recursos](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
