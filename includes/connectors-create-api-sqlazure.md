---
title: incluir ficheiro
description: incluir ficheiro
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/15/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 4ffda692da0ab7b63f7376c36dfab0bec914e334
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37138070"
---
* Se estiver a utilizar a SQL Database do Azure, siga os passos em [ligar à SQL Database do Azure](#connect-azure-sql-db). 

* Se estiver a utilizar o SQL Server, siga os passos em [ligar ao SQL Server](#connect-sql-server).

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Ligar à base de dados SQL do Azure

1. Quando o acionador do SQL Server ou a ação pede-lhe as informações da ligação, siga estes passos:

   1. Crie um nome para a ligação.

   2. Selecione o SQL server e, em seguida, selecione a base de dados. 

      É apresentada a lista de base de dados apenas depois de selecionar o SQL server.
 
   3. Forneça o nome de utilizador e palavra-passe para o servidor.

      Pode encontrar estas informações no portal do Azure com as propriedades de base de dados do SQL Server ou na sua cadeia de ligação: 
      
      "ID de utilizador = <*yourUserName*>"
      <br>
      "Palavra-passe = <*yourPassword*>"

   Este exemplo mostra as informações de ligação para um acionador, mas estes passos funcionam para ações demasiado.

   ![Criar a ligação de SQL Database do Azure](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)
   <br>
   Série de asteriscos (*) indica os valores necessários.

   | Propriedade | Valor | Detalhes | 
   |----------|-------|---------| 
   | Nome da Ligação | <*ligação My sql*> | O nome da ligação | 
   | Nome do Servidor SQL | <*Os meus do SQL Server*> | O nome para o SQL server |
   | Nome da Base de Dados SQL | <*Os meus-base de dados sql*>  | O nome da base de dados do SQL Server | 
   | Nome de utilizador | <*My sql username*> | O nome de utilizador para aceder à sua base de dados |
   | Palavra-passe | <*Os meus-sql-palavra-passe*> | A palavra-passe para aceder à sua base de dados | 
   |||| 

2. Quando tiver terminado, escolha **Create** (Criar).

3. Depois de criar a ligação, prossiga com [acionador adicionar SQL](#add-sql-trigger) ou [ação adicionar SQL](#add-sql-action).

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>Ligar ao SQL Server

Para poder selecionar o gateway, certifique-se de que já [configurar o data gateway](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection). Dessa forma, o gateway aparece na lista de gateways quando criar a ligação.

1. Quando o acionador do SQL Server ou a ação pede-lhe as informações da ligação, siga estes passos:

   1. No acionador ou ação, selecione **ligar através do gateway de dados no local** para que as opções de servidor SQL aparecem.

   2. Crie um nome para a ligação.

   3. Forneça o endereço para o SQL server, em seguida, forneça o nome da base de dados.
   
      Pode encontrar estas informações na sua cadeia de ligação: 
      
      * "Servidor = <*yourServerAddress*>"
      * "Base de dados = <*yourDatabaseName*>"

   4. Forneça o nome de utilizador e palavra-passe para o servidor.

      Pode encontrar estas informações na sua cadeia de ligação: 
      
      * "ID de utilizador = <*yourUserName*>"
      * "Palavra-passe = <*yourPassword*>"

   5. Se o SQL server utiliza o Windows ou a autenticação básica, selecione o tipo de autenticação.

   6. Selecione o nome do gateway de dados no local que criou anteriormente.
   
      Verifique se o gateway não aparecer na lista, que corretamente [configurar o gateway](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection).

   Este exemplo mostra as informações de ligação para um acionador, mas estes passos funcionam para ações demasiado.

   ![Criar a ligação do SQL Server](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)
   <br>
   Série de asteriscos (*) indica os valores necessários.

   | Propriedade | Valor | Detalhes | 
   |----------|-------|---------| 
   | Ligar através do gateway no local | Selecione esta opção primeiro para definições do SQL Server. | | 
   | Nome da Ligação | <*ligação My sql*> | O nome da ligação | 
   | Nome do Servidor SQL | <*Os meus do SQL Server*> | O nome para o SQL server |
   | Nome da Base de Dados SQL | <*Os meus-base de dados sql*>  | O nome da base de dados do SQL Server |
   | Nome de utilizador | <*My sql username*> | O nome de utilizador para aceder à sua base de dados |
   | Palavra-passe | <*Os meus-sql-palavra-passe*> | A palavra-passe para aceder à sua base de dados | 
   | Tipo de Autenticação | Windows ou básico | Opcional: O tipo de autenticação utilizado pelo seu SQL server | 
   | Gateways | <*gateway de dados My*> | O nome do gateway de dados no local | 
   |||| 

2. Quando tiver terminado, escolha **Create** (Criar). 

3. Depois de criar a ligação, prossiga com [acionador adicionar SQL](#add-sql-trigger) ou [ação adicionar SQL](#add-sql-action).
