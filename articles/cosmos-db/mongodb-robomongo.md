---
title: Utilizar Robomongo para o Azure Cosmos DB
description: 'Saiba como utilizar o Robomongo com um Azure Cosmos DB: API para a conta do MongoDB'
keywords: robomongo
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: 78f0158c9a80a60717b81b4788531c7efd979111
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863810"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Utilizar Robomongo com um Azure Cosmos DB: API para a conta do MongoDB
Para ligar ao Azure Cosmos DB: API para utilizar o Robomongo de conta de MongoDB, tem:

* Transferir e instalar [Robomongo](https://robomongo.org/)
* Ter o Azure Cosmos DB: API para a conta do MongoDB [cadeia de ligação](connect-mongodb-account.md) informações

## <a name="connect-using-robomongo"></a>Ligar com o Robomongo
Para adicionar o Azure Cosmos DB: API para a conta do MongoDB para ligações de MongoDB Robomongo, execute os seguintes passos.

1. Obter o Azure Cosmos DB: API para MongoDB informações de ligação de conta com as instruções [aqui](connect-mongodb-account.md).

    ![Captura de ecrã do painel de cadeia de ligação](./media/mongodb-robomongo/connectionstringblade.png)
2. Executar *Robomongo.exe*

3. Clique no botão de ligação sob **ficheiro** para gerir as suas ligações. Em seguida, clique em **criar** no **ligações MongoDB** janela, que abrirá a **definições de ligação** janela.

4. Na **definições de ligação** janela, escolha um nome. Em seguida, procure o **Host** e **porta** do suas informações de ligação no passo 1 e introduza-os em **endereço** e **porta**, respectivamente.

    ![Captura de ecrã da Robomongo gerir ligações](./media/mongodb-robomongo/manageconnections.png)
5. Sobre o **autenticação** separador, clique em **executar autenticação**. Em seguida, introduza a sua base de dados (a predefinição é *administrador*), **nome de utilizador** e **palavra-passe**.
Ambos **nome de utilizador** e **palavra-passe** podem ser encontrados nas informações de ligação no passo 1.

    ![Captura de ecrã do separador autenticação Robomongo](./media/mongodb-robomongo/authentication.png)
6. Na **SSL** separador, verificação **utilizar o protocolo SSL**, em seguida, altere o **método de autenticação** para **certificado autoassinado**.

    ![Captura de ecrã do separador de SSL Robomongo](./media/mongodb-robomongo/SSL.png)
7. Por fim, clique em **teste** para verificar a capacidade de ligar, em seguida, **guardar**.

## <a name="next-steps"></a>Passos Seguintes
* Explore o Azure Cosmos DB: API para MongoDB [amostras](mongodb-samples.md).
