---
title: Ligar ao SQL Server ou base de dados do SQL do Azure - as do Azure Logic Apps | Microsoft Docs
description: Como aceder e gerir bases de dados do SQL Server no local ou na nuvem ao automatizar fluxos de trabalho com Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/15/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: dccb91c782408a5fed5c3ef1b68f9918823ce402
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296293"
---
# <a name="connect-to-sql-server-or-azure-sql-database-from-azure-logic-apps"></a>Ligar ao SQL Server ou base de dados SQL do Azure a partir do Azure Logic Apps

Este artigo mostra como pode aceder aos dados na base de dados SQL do dentro de uma aplicação lógica com o conector do SQL Server. Dessa forma, pode automatizar tarefas, processos e fluxos de trabalho que gerem os seus dados do SQL Server e os recursos através da criação de aplicações lógicas. O conector funciona para ambos [do SQL Server no local](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) e para [SQL Database do Azure na nuvem](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview). 

Pode criar as logic apps, que são executados quando é acionada por eventos na sua base de dados do SQL Server ou em outros sistemas, tais como o Dynamics CRM Online. As logic apps também podem obter, insira e eliminar dados, juntamente com a execução de consultas SQL e procedimentos armazenados. Por exemplo, pode criar uma aplicação lógica que automaticamente verifica a existência de novos registos no Dynamics CRM Online, adiciona os itens na base de dados do SQL Server para quaisquer novos registos e, em seguida, envia alertas por e-mail.

Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. Se estiver familiarizado com as logic apps, reveja [que é o Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [início rápido: criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para obter informações técnicas específicas do conector, consulte o <a href="https://docs.microsoft.com/connectors/sql/" target="blank">referência de conector do SQL Server</a>.

## <a name="prerequisites"></a>Pré-requisitos

* A aplicação lógica em que precisa de acesso à base de dados SQL. Para iniciar a sua aplicação lógica com um acionador SQL, é necessário um [aplicação lógica em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Um [SQL database do Azure](../sql-database/sql-database-get-started-portal.md) ou um [base de dados do SQL Server](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) 

  As tabelas têm de ter dados para que a sua aplicação lógica pode devolver resultados ao chamar operações. Se criar uma base de dados do SQL do Azure, pode utilizar bases de dados de exemplo, que estão incluídos. 

* O nome do servidor SQL, nome de base de dados, o nome de utilizador e a palavra-passe. Precisa destas credenciais, para que podem autorizar a lógica de aceder ao seu SQL server. 

  * Para a base de dados SQL do Azure, pode encontrar estes detalhes na cadeia de ligação ou o portal do Azure nas propriedades de base de dados SQL:

    "Servidor = tcp: <*yourServerName*>. database.windows.net,1433;Initial catálogo = <*yourDatabaseName*>; Manter informações de segurança = False; ID de utilizador = <*yourUserName*>; Palavra-passe = <*yourPassword*>; MultipleActiveResultSets = False; Encriptar = True; TrustServerCertificate = False; Tempo limite da ligação = 30; "

  * Para o SQL Server, pode encontrar estes detalhes na cadeia de ligação: 

    "Servidor = <*yourServerAddress*>; base de dados = <*yourDatabaseName*>; Id de utilizador = <*yourUserName*>; Palavra-passe = <*yourPassword*>; "

* Antes de poder ligar as logic apps para sistemas no local, como o SQL Server, tem [configurar um gateway de dados no local](../logic-apps/logic-apps-gateway-install.md). Dessa forma, pode selecionar o gateway ao criar a ligação do SQL Server para a sua aplicação lógica.

<a name="add-sql-trigger"></a>

## <a name="add-sql-trigger"></a>Adicionar o acionador do SQL Server

No Azure Logic Apps, cada aplicação lógica tem de começar com uma [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que é desencadeado quando um evento específico acontece ou quando for cumprida uma condição específica. Sempre que o acionador é desencadeado, o motor de Logic Apps cria uma instância de aplicação lógica e inicia a executar o fluxo de trabalho da sua aplicação.

1. No portal do Azure ou Visual Studio, crie uma aplicação de lógica em branco, o qual abre o Designer de aplicações lógicas. Este exemplo utiliza o portal do Azure.

2. Na caixa de pesquisa, introduza "sql server" como o filtro. Na lista de acionadores, selecione o acionador do SQL Server que pretende. 

   Para este exemplo, selecione este acionador: **do SQL Server - quando um item é criado**

   ![Selecione "SQL Server - quando é criado um item" acionador](./media/connectors-create-api-sqlazure/sql-server-trigger.png)

3. Se lhe for pedida para detalhes de ligação, [criar a sua ligação de SQL agora](#create-connection). 
   Ou, se a ligação já existir, selecione o **nome da tabela** que pretende que o da lista.

   ![Selecionar a tabela](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

4. Definir o **intervalo** e **frequência** as propriedades que especificam a frequência a sua aplicação lógica verifica a tabela.

   Neste exemplo só verifica as tabelas selecionadas, mais nada. 
   Para fazer algo mais interessante, adicione as ações que executar as tarefas que pretende. 
   
   Por exemplo, para ver o novo item na tabela, poderá adicionar outras ações, tais como criar um ficheiro que tem campos da tabela e, em seguida, enviar alertas por e-mail. 
   Para mais informações sobre outras ações para este conector ou outros conectores, consulte o artigo [conectores Logic Apps](../connectors/apis-list.md).

5. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**. 

   Este passo permite e publica a sua aplicação lógica em direto no Azure automaticamente. 

<a name="add-sql-action"></a>

## <a name="add-sql-action"></a>Adicionar ação do SQL Server

No Azure Logic Apps, um [ação](../logic-apps/logic-apps-overview.md#logic-app-concepts) é um passo no fluxo de trabalho que se segue um acionador ou outra ação. Neste exemplo, a aplicação lógica começa com o [acionador de recorrência](../connectors/connectors-native-recurrence.md)e chama uma ação que obtém uma linha de uma base de dados do SQL Server.

1. No portal do Azure ou Visual Studio, abra a aplicação de lógica no Designer de aplicações lógicas. Este exemplo utiliza o portal do Azure.

2. No Designer de aplicação lógica, sob o acionador ou ação, escolha **novo passo** > **adicionar uma ação**.

   ![Selecione "Novo passo", "Adicionar uma ação"](./media/connectors-create-api-sqlazure/add-action.png)
   
   Para adicionar uma ação entre passos existentes, mova o rato sobre a seta para a ligação. 
   Escolha o sinal de adição (**+**) que é apresentado e, em seguida, escolha **adicionar uma ação**.

2. Na caixa de pesquisa, introduza "sql server" como o filtro. Na lista de ações, selecione qualquer ação do SQL Server que pretender. 

   Para este exemplo, selecione esta ação, o que obtém um único registo: **do SQL Server - linha Get**

   ![Introduza "sql server", selecione "SQL Server - linha Get"](./media/connectors-create-api-sqlazure/select-sql-get-row.png) 

3. Se lhe for pedida para detalhes de ligação, [criar a sua ligação de SQL agora](#create-connection). 
   Ou, se a ligação existe, selecione um **nome da tabela**e introduza o **ID de linha** para o registo de que pretende.

   ![Introduza o nome da tabela e o ID de linha](./media/connectors-create-api-sqlazure/table-row-id.png)
   
   Neste exemplo devolve apenas uma linha entre as tabelas selecionadas, mais nada. 
   Para ver os dados nesta linha, pode adicionar outras ações que crie um ficheiro com os campos da linha para revisão posterior e armazenar esse ficheiro numa conta de armazenamento na nuvem. Para saber mais sobre as restantes ações deste conector ou outros conectores, consulte [conectores Logic Apps](../connectors/apis-list.md).

4. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**. 

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Ligar à base de dados

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="process-data-in-bulk"></a>Processamento de dados em massa

Quando trabalha com conjuntos de resultados tão elevados que o conector não devolve todos os resultados ao mesmo tempo, em alternativa, um melhor controlo sobre o tamanho e a estrutura para conjuntos de resultados, pode utilizar *paginação*, que ajuda a gerir os resultados como conjuntos mais pequenos. 

[!INCLUDE [Set up pagination for results exceeding default page size](../../includes/connectors-pagination-bulk-data-transfer.md)]

### <a name="create-a-stored-procedure"></a>Criar um procedimento armazenado

Ao obter ou inserir várias linhas, a aplicação lógica pode itere através destes itens utilizando um [ *até ciclo* ](../logic-apps/logic-apps-control-flow-loops.md#until-loop) dentro estes [limites](../logic-apps/logic-apps-limits-and-config.md). No entanto, por vezes, a aplicação lógica tem de trabalhar com conjuntos de registos tão elevados como milhares ou milhões de linhas, o que pretende minimizar os custos de chamadas para a base de dados. 

Em vez disso, pode criar um <a href="https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine" target="blank"> *procedimento armazenado* </a> que é executado na sua instância do SQL Server e utiliza o **SELECIONAR - ORDER BY** instrução para organizar o forma como pretende que os resultados. Esta solução fornece-lhe mais controlo sobre o tamanho e a estrutura dos resultados da sua. A aplicação lógica chama o procedimento armazenado utilizando o conector do SQL Server **executar procedimento armazenado** ação. 

Para detalhes de solução, consulte estes artigos:

* <a href="https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx" target="_blank">Paginação de SQL para a transferência de dados em massa com Logic Apps</a>

* <a href="https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql" target="_blank">SELECIONE - ordenar por uma cláusula</a>

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Para obter informações técnicas sobre acionadores este conector, ações e limites, consulte o [detalhes de referência do conector](/connectors/sql/). 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre outros [conectores Logic Apps](../connectors/apis-list.md)

