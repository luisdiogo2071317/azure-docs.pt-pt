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
ms.openlocfilehash: 690c5f07a2b0f7a88e16f0b0bbbaa9ca78e37317
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31429263"
---
1. Numa nova janela, inicie sessão no [portal do Azure](https://portal.azure.com/).
2. No menu esquerdo, clique em **Criar um recurso**, **Bases de Dados**e, em **Azure Cosmos DB** e **Criar**.
   
   ![Captura de ecrã do portal do Azure, com destaque para Mais Serviços e Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. No painel **Nova conta**, especifique **MongoDB** como a API e preencha a configuração pretendida para a conta do Azure Cosmos DB.
 
    * O **ID** tem de ser um nome exclusivo que vai utilizar para identificar a sua conta do Azure Cosmos DB. Pode conter apenas minúsculas, números, o caráter "-" e tem de ter entre três e 50 carateres.
    * A **Subscrição** é a sua subscrição do Azure. Será preenchida para si.
    * O **Grupo de Recursos** é o nome do grupo de recursos para a sua conta do Azure Cosmos DB. Selecione **Criar Novo** e, em seguida, introduza um novo nome de grupo de recursos para a sua conta. Para simplicidade, pode utilizar o mesmo nome do ID.
    * A **Localização** é a localização geográfica onde está localizada a sua instância da instância do Azure Cosmos DB. Escolha a localização mais próxima dos seus utilizadores.

    Em seguida, clique em **Criar**.

    ![A página da nova conta do Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. A criação da conta demora alguns minutos. Aguarde que o portal apresente a página **Parabéns! A sua conta do Azure Cosmos DB com a API do MongoDB está pronta**.

    ![O painel de Notificações de portal do Azure](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
