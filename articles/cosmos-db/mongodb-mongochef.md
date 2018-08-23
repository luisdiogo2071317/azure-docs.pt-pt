---
title: Utilizar Studio 3T (MongoChef) com o Azure Cosmos DB | Documentos da Microsoft
description: Saiba como utilizar o Studio 3T com uma conta de API do MongoDB do Azure Cosmos DB
keywords: mongochef, studio 3T
services: cosmos-db
author: slyons
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
ms.date: 01/18/2018
ms.author: sclyon
ms.openlocfilehash: af66f413055c01d9a4c3e94c7d3b9c5f1d5917b0
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42061466"
---
# <a name="azure-cosmos-db-use-studio-3t-with-a-mongodb-api-account"></a>Azure Cosmos DB: Utilize o Studio 3T com uma conta de API do MongoDB

Para ligar a uma conta de API do MongoDB do Azure Cosmos DB, tem de:

* Transfira e instale [Studio 3T](https://studio3t.com/) (anteriormente conhecido como o MongoChef)
* Ter o Azure Cosmos DB [cadeia de ligação](connect-mongodb-account.md) informações para a sua conta MongoDB

## <a name="create-the-connection-in-studio-3t"></a>Criar a ligação no Studio 3T
Para adicionar a sua conta do Azure Cosmos DB para o Gestor de ligações do Studio 3T, execute os seguintes passos:

1. Obter as informações de ligação do Azure Cosmos DB para a conta de API do MongoDB com as instruções a [ligar uma aplicação MongoDB ao Azure Cosmos DB](connect-mongodb-account.md) artigo.

    ![Captura de ecrã da página de cadeia de ligação](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Clique em **Connect** para abrir o Gerenciador de conexões, em seguida, clique em **nova ligação**

    ![Captura de ecrã do Gerenciador de conexões Studio 3T](./media/mongodb-mongochef/ConnectionManager.png)
3. Na **nova ligação** janela, no **servidor** separador, introduza o anfitrião (FQDN) da conta do Azure Cosmos DB e a porta.

    ![Captura de ecrã do Guia do servidor do Gerenciador de conexão do Studio 3T](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. Na **nova ligação** janela, no **autenticação** separador, selecione o modo de autenticação **Basic (MONGODB CR ou SCARM-SHA-1)** e introduza o nome de utilizador e palavra-passe.  Aceite o banco de dados de autenticação predefinido (administrador) ou fornecer seu próprio valor.

    ![Captura de ecrã do separador de autenticação Studio 3T ligação manager](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. No **nova ligação** janela, no **SSL** separador, verifique o **utilizar o protocolo SSL para ligar** caixa de verificação e o **aceitar o certificado SSL autoassinado do servidor**  botão de opção.

    ![Captura de ecrã do separador SSL Studio 3T ligação manager](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Clique nas **Testar ligação** botão para validar as informações de ligação, clique em **OK** para retornar à janela nova ligação e, em seguida, clique em **guardar**.

    ![Captura de ecrã da janela de ligação de teste do Studio 3T](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Utilizar Studio 3T para criar uma base de dados, uma coleção e documentos
Para criar uma base de dados, uma coleção e documentos, utilizar o Studio 3T, execute os seguintes passos:

1. Na **Gerenciador de conexões**, realce a ligação e clique em **Connect**.

    ![Captura de ecrã do Gerenciador de conexões Studio 3T](./media/mongodb-mongochef/ConnectToAccount.png)
2. Com o botão direito no anfitrião e escolha **Adicionar base de dados**.  Forneça um nome de base de dados e clique em **OK**.

    ![Captura de ecrã da opção Studio 3T Adicionar base de dados](./media/mongodb-mongochef/AddDatabase1.png)
3. A base de dados com o botão direito e escolher **adicionar coleção**.  Forneça um nome de coleção e clique em **criar**.

    ![Captura de ecrã da opção Studio 3T adicionar coleção](./media/mongodb-mongochef/AddCollection.png)
4. Clique nas **recolha** menu item, em seguida, clique em **Adicionar documento**.

    ![Captura de ecrã do item de menu do Studio 3T Adicionar documento](./media/mongodb-mongochef/AddDocument1.png)
5. Na caixa de diálogo Adicionar documento, cole o seguinte e, em seguida, clique em **Adicionar documento**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Adicione outro documento, desta vez com o seguinte conteúdo:

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Execute uma consulta de exemplo. Por exemplo, procure famílias com o nome da última 'Andersen' e devolver os pais e campos de estado.

    ![Captura de ecrã dos resultados de consulta Mongo Chef](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Passos Seguintes
* Explore a API de MongoDB do Azure Cosmos DB [amostras](mongodb-samples.md).
