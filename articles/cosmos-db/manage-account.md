---
title: Gerir uma conta do Azure Cosmos DB no portal do Azure | Documentos da Microsoft
description: Saiba como gerir a sua conta do Azure Cosmos DB no portal do Azure. Encontre um guia sobre como utilizar o Portal do Azure para ver, copiar, excluir e contas de acesso.
keywords: Azure Portal, do azure, do Microsoft azure
services: cosmos-db
author: kirillg
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: kirillg
ms.openlocfilehash: abcf51c6bd196c2ffb0bb35e2df161531a53972d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229395"
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Como gerir uma conta do Azure Cosmos DB
Saiba como definir a consistência global, trabalhar com chaves e eliminar uma conta do Azure Cosmos DB no portal do Azure.

## <a id="consistency"></a>Gerir as definições de consistência do Azure Cosmos DB
Selecionar o nível certo de consistência depende da semântica da sua aplicação. Familiarize-se com os níveis de consistência disponíveis no Azure Cosmos DB, leia [utilização de níveis de consistência para maximizar a disponibilidade e desempenho no Azure Cosmos DB][consistency]. O Azure Cosmos DB fornece consistência, disponibilidade e garantias de desempenho, em todos os níveis de consistência disponíveis para a sua conta de base de dados. Configurar a sua conta de base de dados com um nível de consistência de forte requer que os seus dados estão limitado a uma única região do Azure e globalmente disponível. Por outro lado, os níveis de consistência flexível - estagnação limitada, sessão ou eventual permitem-lhe associar qualquer número de regiões do Azure à sua conta de base de dados. Os passos seguintes mostram-lhe como selecionar o nível de consistência predefinido para a sua conta de base de dados.

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>Para especificar a consistência predefinida para uma conta do Azure Cosmos DB
1. Na [portal do Azure](https://portal.azure.com/), aceder à sua conta do Azure Cosmos DB.
2. Na página da conta, clique em **consistência predefinida**.
3. Na **consistência predefinida** página, selecione o novo nível de consistência e clique em **guardar**.
    ![Sessão de consistência predefinida][5]

## <a id="keys"></a>Ver, copiar e regenerar chaves de acesso e de palavras-passe
Quando cria uma conta do Azure Cosmos DB, o serviço gera duas chaves de acesso mestre (ou duas palavras-passe para contas de API do MongoDB) que podem ser utilizados para autenticação quando aceder a conta do Azure Cosmos DB. Ao fornecer duas chaves de acesso, o Azure Cosmos DB permite-lhe voltar a gerar as chaves sem interrupções na sua conta do Azure Cosmos DB. 

No [portal do Azure](https://portal.azure.com/), acesso a **chaves** página a partir do menu de recursos no **conta do Azure Cosmos DB** página para ver, copiar e regenerar as chaves de acesso que estão habituadas aceder à sua conta do Azure Cosmos DB. Para contas de API do MongoDB, aceder a **cadeia de ligação** página no menu de recursos para ver, copiar e regenerar as palavras-passe que são utilizadas para aceder à sua conta.

![Azure portal captura de ecrã, página chaves](./media/manage-account/keys.png)

> [!NOTE]
> O **chaves** página também inclui cadeias de ligação primária e secundária que podem ser utilizadas para ligar à sua conta do [ferramenta de migração de dados](import-data.md).
> 
> 

Chaves só de leitura também estão disponíveis nesta página. As leituras e consultas são operações só de leitura, enquanto cria, elimina, e substitui não é.

### <a name="copy-an-access-key-or-password-in-the-azure-portal"></a>Copiar uma chave de acesso ou a palavra-passe no portal do Azure
Sobre o **chaves** página (ou **cadeia de ligação** página para contas de API do MongoDB), clique no **cópia** botão à direita da chave ou palavra-passe que pretende copiar.

![Ver e copiar uma chave de acesso no portal do Azure, página Chaves](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys-and-passwords"></a>Regenerar chaves de acesso e de palavras-passe
Deve alterar as chaves de acesso (e palavras-passe para contas de API do MongoDB) à sua conta do Azure Cosmos DB periodicamente para ajudar a manter as suas ligações mais seguro. Duas chaves de acesso/as senhas são atribuídas para que possa manter as ligações à conta do Azure Cosmos DB através de uma chave de acesso enquanto volta a gerar a chave de acesso.

> [!WARNING]
> A regenerar as chaves de acesso afeta todas as aplicações que dependem da chave atual. Todos os clientes que utilizam a chave de acesso para aceder à conta do Azure Cosmos DB tem de ser atualizados para usar a nova chave.
> 
> 

Se tiver aplicações ou serviços em nuvem com a conta do Azure Cosmos DB, perderá as ligações se voltar a gerar chaves, a menos que reverta as suas chaves. Os passos seguintes descrevem o processo envolvido na implementar as chaves/palavras-passe.

1. Atualize a chave de acesso no código da aplicação para fazer referência a chave de acesso secundária da conta do Azure Cosmos DB.
2. Regenere a chave de acesso primária para a sua conta do Azure Cosmos DB. Na [portal do Azure](https://portal.azure.com/), aceder à sua conta do Azure Cosmos DB.
3. Na **conta do Azure Cosmos DB** página, clique em **chaves** (ou **cadeia de ligação** para MongoDB contas * *).
4. Sobre o **chaves**/**cadeia de ligação** página, clique no botão voltar a gerar, em seguida, clique em **Ok** para confirmar que pretende gerar uma nova chave.
    ![Voltar a gerar chaves de acesso](./media/manage-account/regenerate-keys.png)
5. Assim que tiver verificado que a nova chave está disponível para utilização (cerca de cinco minutos após a regeneração), atualize a chave de acesso no código da aplicação para fazer referência a nova chave de acesso primária.
6. Volte a gerar a chave de acesso secundária.
   
    ![Voltar a gerar chaves de acesso](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Pode demorar vários minutos antes de uma chave gerada recentemente pode ser utilizada para aceder à sua conta do Azure Cosmos DB.
> 
> 

## <a name="get-the-connection-string"></a>Obter a cadeia de ligação
Para obter a cadeia de ligação, faça o seguinte: 

1. Na [portal do Azure](https://portal.azure.com), aceder à sua conta do Azure Cosmos DB.
2. No menu de recursos, clique em **chaves** (ou **cadeia de ligação** para contas de API do MongoDB).
3. Clique nas **cópia** junto aos **cadeia de ligação primária** ou **cadeia de ligação secundária** caixa. 

Se estiver a utilizar a cadeia de ligação no [ferramenta de migração de bases de dados do Azure Cosmos DB](import-data.md), acrescentar o nome de base de dados para o fim da cadeia de ligação. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a> Eliminar uma conta do Azure Cosmos DB
Para remover uma conta do Azure Cosmos DB no portal do Azure que já não está a utilizar, o nome da conta com o botão direito e clique em **eliminar conta**.

![Como eliminar uma conta do Azure Cosmos DB no portal do Azure](./media/manage-account/deleteaccount.png)

1. Na [portal do Azure](https://portal.azure.com/), aceder à conta do Azure Cosmos DB que pretende eliminar.
2. Sobre o **conta do Azure Cosmos DB** página, a conta com o botão direito e, em seguida, clique em **eliminar conta**. 
3. Na página de confirmação resultante, escreva o nome da conta do Azure Cosmos DB para confirmar que pretende eliminar a conta.
4. Clique nas **eliminar** botão.

![Como eliminar uma conta do Azure Cosmos DB no portal do Azure](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Passos seguintes
Saiba como [começar com a sua conta do Azure Cosmos DB](https://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
