---
title: Utilizar o Studio 3T (MongoChef) com o Azure Cosmos DB | Microsoft Docs
description: Saiba como utilizar o Studio 3T com uma conta do Azure Cosmos DB MongoDB API
keywords: mongochef, 3T do studio
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: anhoh
ms.openlocfilehash: 0341fbf668bbbc8f02e78bc1f6c7a00ecc939cc2
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="azure-cosmos-db-use-studio-3t-with-a-mongodb-api-account"></a>DB de Cosmos do Azure: Utilize o Studio 3T com uma conta de API do MongoDB

Para ligar a uma conta de API do Azure Cosmos DB MongoDB, tem de:

* Transfira e instale [Studio 3T](https://studio3t.com/) (anteriormente conhecida como MongoChef)
* Tem a sua base de dados do Azure Cosmos [cadeia de ligação](connect-mongodb-account.md) informações para a sua conta do MongoDB

## <a name="create-the-connection-in-studio-3t"></a>Criar a ligação no Studio 3T
Para adicionar a sua conta de base de dados do Azure Cosmos para o Gestor de ligações de 3T Studio, execute os seguintes passos:

1. Obter as informações de ligação de base de dados do Azure Cosmos para a sua conta de API do MongoDB utilizando as instruções no [ligar uma aplicação do MongoDB à base de dados do Azure Cosmos](connect-mongodb-account.md) artigo.

    ![Captura de ecrã da página de cadeia de ligação](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Clique em **Connect** para abrir o Gestor de ligações, em seguida, clique em **nova ligação**

    ![Captura de ecrã do Gestor de ligações de 3T Studio](./media/mongodb-mongochef/ConnectionManager.png)
3. No **nova ligação** janela, no **servidor** separador, introduza a porta e o anfitrião (FQDN) da conta de base de dados do Azure Cosmos.

    ![Captura de ecrã do separador de servidor de Gestor do Studio 3T ligação](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. No **nova ligação** janela, no **autenticação** separador, escolha o modo de autenticação **Basic (MONGODB CR ou SCARM-SHA-1)** e introduza o nome de utilizador e palavra-passe.  Aceite a BD de autenticação predefinido (administrador) ou forneça o seu próprio valor.

    ![Captura de ecrã do separador de autenticação de Gestor do Studio 3T ligação](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. No **nova ligação** janela, no **SSL** separador, verifique o **protocolo SSL utilizado para ligar** caixa de verificação e o **aceitar certificados SSL autoassinados do servidor**  botão de opção.

    ![Captura de ecrã do separador SSL do Studio 3T ligação manager](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Clique em de **Testar ligação** botão para validar as informações de ligação, clique em **OK** para regressar à janela de nova ligação e, em seguida, clique em **guardar**.

    ![Captura de ecrã da janela de ligação de teste de 3T do Studio](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Utilize Studio 3T para criar uma base de dados, coleção e documentos
Para criar uma base de dados, coleção e documentos utilizando Studio 3T, execute os seguintes passos:

1. No **Gestor de ligações**, realce a ligação e clique em **Connect**.

    ![Captura de ecrã do Gestor de ligações de 3T Studio](./media/mongodb-mongochef/ConnectToAccount.png)
2. Clique no anfitrião e escolha **Adicionar base de dados**.  Forneça um nome de base de dados e clique em **OK**.

    ![Captura de ecrã da opção Studio 3T Adicionar base de dados](./media/mongodb-mongochef/AddDatabase1.png)
3. A base de dados com o botão direito e selecione **adicionar coleção**.  Forneça um nome da coleção e clique em **criar**.

    ![Captura de ecrã da opção Studio 3T adicionar coleção](./media/mongodb-mongochef/AddCollection.png)
4. Clique em de **coleção** menu item, em seguida, clique em **Adicionar documento**.

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
7. Execute uma consulta de exemplo. Por exemplo, procure famílias com o apelido 'ou ' seja e devolver o principais e os campos de estado.

    ![Captura de ecrã Mongo Chef dos resultados da consulta](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Passos Seguintes
* Explore a API do Azure Cosmos DB MongoDB [amostras](mongodb-samples.md).
