---
title: Utilizar Robomongo para Azure Cosmos DB | Microsoft Docs
description: 'Saiba como utilizar Robomongo com uma base de dados do Azure Cosmos: API para a conta do MongoDB'
keywords: robomongo
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: b6d64d7d7b30d4175fb8c8bf6c98127465427d4e
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34795036"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Utilizar Robomongo com uma base de dados do Azure Cosmos: API para a conta do MongoDB
Para ligar a uma base de dados do Azure Cosmos: API para a conta de MongoDB com Robomongo, tem de:

* Transfira e instale [Robomongo](https://robomongo.org/)
* Tem a sua base de dados do Azure Cosmos: API para a conta do MongoDB [cadeia de ligação](connect-mongodb-account.md) informações

## <a name="connect-using-robomongo"></a>Ligar utilizando Robomongo
Para adicionar a base de dados do Azure Cosmos: API para a conta do MongoDB para ligações de MongoDB Robomongo, execute os seguintes passos.

1. Obter a sua base de dados do Azure Cosmos: API de informações de ligação de conta do MongoDB utilizando as instruções [aqui](connect-mongodb-account.md).

    ![Captura de ecrã do painel de cadeia de ligação](./media/mongodb-robomongo/connectionstringblade.png)
2. Executar *Robomongo.exe*

3. Clique no botão de ligação em **ficheiro** para gerir as suas ligações. Em seguida, clique em **criar** no **MongoDB ligações** janela, que será apresentado o **as definições de ligação** janela.

4. No **as definições de ligação** janela, escolha um nome. Em seguida, localize o **anfitrião** e **porta** das suas informações de ligação no passo 1 e insira-os em **endereço** e **porta**, respetivamente.

    ![Captura de ecrã das Robomongo gerir ligações](./media/mongodb-robomongo/manageconnections.png)
5. No **autenticação** separador, clique em **executar autenticação**. Em seguida, introduza a sua base de dados (predefinição é *Admin*), **nome de utilizador** e **palavra-passe**.
Ambos **nome de utilizador** e **palavra-passe** pode ser encontrado na suas informações de ligação no passo 1.

    ![Captura de ecrã do separador autenticação Robomongo](./media/mongodb-robomongo/authentication.png)
6. No **SSL** separador, verificação **protocolo SSL de utilização**, em seguida, altere o **método de autenticação** para **certificado autoassinado**.

    ![Captura de ecrã do separador Robomongo SSL](./media/mongodb-robomongo/SSL.png)
7. Por fim, clique em **teste** para verificar que é possível estabelecer ligação, em seguida, **guardar**.

## <a name="next-steps"></a>Passos Seguintes
* Explorar o Azure Cosmos DB: API para o MongoDB [amostras](mongodb-samples.md).
