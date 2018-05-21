---
title: Ligar o Excel à Base de Dados SQL | Microsoft Docs
description: Saiba como ligar o Microsoft Excel à base de dados SQL do Azure na nuvem. Importe dados para o Excel para criação de relatórios e exploração de dados.
services: sql-database
keywords: ligar o excel ao sql, importar dados para o excel
author: joseidz
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 03/10/2017
ms.author: craigg
ms.openlocfilehash: 6f2894d65240580346b99d203f8289652d8e6618
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Ligar o Excel a uma base de dados SQL do Azure e criar um relatório

Ligar o Excel a uma base de dados do SQL Server na nuvem e importar dados e criar tabelas e gráficos com base nos valores na base de dados. Neste tutorial, irá configurar a ligação entre o Excel e uma tabela de base de dados, guardar o ficheiro que armazena os dados e as informações de ligação para o Excel e, em seguida, criar um gráfico dinâmico a partir dos valores da base de dados.

Precisa de uma base de dados SQL no Azure antes de começar. Se não tiver uma, consulte [Criar a sua primeira base de dados SQL](sql-database-get-started-portal.md) para obter uma base de dados com dados de exemplo e funcional, em apenas alguns minutos. Neste artigo, irá importar dados de exemplo para o Excel desse artigo, mas pode seguir passos semelhantes com os seus próprios dados.

Irá também precisar de uma cópia do Excel. Este artigo utiliza o [Microsoft Excel 2016](https://products.office.com/).

## <a name="connect-excel-to-a-sql-database-and-load-data"></a>Ligar o Excel a uma base de dados e a carga de dados SQL
1. Para ligar o Excel à base de dados SQL, abra o Excel e, em seguida, crie um novo livro ou abra um livro do Excel existente.
2. Na barra de menus na parte superior da página, selecione o **dados** separador, selecione **obter dados**, selecione a partir do Azure e, em seguida, selecione **de SQL Database do Azure**. 
   
   ![Selecione a origem de dados: ligar o Excel à base de dados SQL.](./media/sql-database-connect-excel/excel_data_source.png)
   
   É aberto o Assistente de Ligação de Dados.
3. Na caixa de diálogo **Ligar ao Servidor de Bases de Dados**, escreva o **Nome do servidor** da Base de Dados SQL à qual pretende ligar, no formato <*servername*>**. database.windows.net**. Por exemplo, **msftestserver.database.windows.net**. Opcionalmente, introduza o nome da base de dados. Selecione **OK** para abrir a janela de credenciais. 

   ![servidor name.png](media/sql-database-connect-excel/server-name.png)

1. No **base de dados do SQL Server** caixa de diálogo, selecione **base de dados** à esquerda lado e, em seguida, introduza no seu **nome de utilizador** e **palavra-passe** para o Servidor de base de dados do SQL Server que pretende ligar. Selecione **Connect** para abrir o **navegador**. 

  ![Escrever o nome do servidor e as credenciais de início de sessão](./media/sql-database-connect-excel/connect-to-server.png)
   
  > [!TIP]
  > Dependendo do ambiente de rede, poderá não conseguir ligar ou poderá perder a ligação se o servidor de base de dados SQL não permitir tráfego a partir do seu endereço IP de cliente. Aceda ao [portal do Azure](https://portal.azure.com/), clique em servidores SQL, clique no servidor, clique em firewall em definições e adicione o seu endereço IP de cliente. Consulte [Como configurar as definições da firewall](sql-database-configure-firewall-settings.md) para detalhes.
   
   
5. No **navegador**, selecione a base de dados para trabalhar com a lista, selecione as tabelas ou vistas que pretende trabalhar (Iremos escolher **vGetAllCategories**) e, em seguida, selecione **carga**para mover os dados da base de dados SQL Azure para a folha de cálculo do excel.
   
    ![Selecione uma base de dados e a tabela.](./media/sql-database-connect-excel/select-database-and-table.png)
   

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Importar os dados para o Excel e criar um gráfico dinâmico
Agora que estabeleceu a ligação, tem várias opções diferentes com como carregar os dados. Por exemplo, os seguintes passos criar um gráfico dinâmico com base nos dados encontrados na base de dados SQL. 

1. Siga os passos na secção anterior, mas desta vez, em vez de selecionar **carga**, selecione **carregar para** do **carga** pendente.
2. Em seguida, selecione como pretende ver estes dados no seu livro. Vamos escolher **PivotChart**. Também pode optar por criar uma **Nova folha de cálculo** ou **Adicionar estes dados ao Modelo de Dados**. Para mais informações sobre Modelos de Dados, consulte [Criar um modelo de dados no Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). 
   
    ![Escolher o formato para dados no Excel](./media/sql-database-connect-excel/import-data.png)
   
    A folha de cálculo tem agora um gráfico e uma tabela dinâmicos em branco.
2. Em **Campos PivotTable**, selecione todas as caixas de verificação para os campos que pretende visualizar.
   
    ![Configure o relatório de base de dados.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Se pretender ligar outros livros do Excel e folhas de cálculo para a base de dados, selecione o **dados** separador e selecione **origens recentes** para iniciar o **origens recentes** caixa de diálogo. A partir daí, escolha a ligação que criou na lista e, em seguida, clique em **abra**.
> ![Ligações recentes](media/sql-database-connect-excel/recent-connections.png)
 
## <a name="create-a-permanent-connection-using-odc-file"></a>Criar uma ligação permanente a utilizar ficheiro. odc
Para guardar os detalhes da ligação permanentemente, pode criar um ficheiro. odc e tornar esta ligação uma opção selecionável dentro do **as ligações existentes** caixa de diálogo. 

1. Na barra de menus na parte superior da página, selecione o **dados** separador e, em seguida, selecione **as ligações existentes** para iniciar o **as ligações existentes** caixa de diálogo. 
    1. Selecione **procure mais** para abrir o **selecionar origem de dados** caixa de diálogo.   
    2. Selecione o **+NewSqlServerConnection.odc** de ficheiros e, em seguida, selecione **abrir** para abrir o **Assistente de ligação de dados**.

    ![Nova Ligação](media/sql-database-connect-excel/new-connection.png)

2. No **Assistente de ligação de dados**, escreva o nome do servidor e as credenciais da sua base de dados SQL. Selecione **Seguinte**. 
    1. Selecione a base de dados que contenha os dados da lista pendente. 
    2. Selecione a tabela ou vista que estiver interessado em. Escolhemos vGetAllCategories.
    3. Selecione **Seguinte**. 

    ![Assistente de ligação de dados](media/sql-database-connect-excel/data-connection-wizard.png) 

3. Selecione a localização do ficheiro, o **nome de ficheiro**e o **nome amigável** no ecrã seguinte do Assistente de ligação de dados. Também pode optar por guardar a palavra-passe no ficheiro, apesar de isto potencialmente pode expor os dados para o acesso indesejável. Selecione **concluir** quando está preparado. 

    ![Guardar a ligação de dados](media/sql-database-connect-excel/save-data-connection.png)

4. Selecione como pretende importar os seus dados. Vamos escolher fazer uma tabela dinâmica. Também pode modificar as propriedades da ligação, selecione **propriedades**. Selecione **OK** quando está preparado. Se não foi possível optar por guardar a palavra-passe com o ficheiro, em seguida, será solicitado para introduzir as suas credenciais. 

    ![Importar dados](media/sql-database-connect-excel/import-data2.png)

5. Certifique-se de que a ligação nova foi guardada, expandindo o **dados** separador e selecionando **as ligações existentes**. 

    ![Ligação existente](media/sql-database-connect-excel/existing-connection.png)

## <a name="next-steps"></a>Passos Seguintes
* Saiba como [Ligar a Base de Dados SQL com o SQL Server Management Studio](sql-database-connect-query-ssms.md) para consulta e análise avançadas.
* Saiba mais sobre as vantagens dos [conjuntos elásticos](sql-database-elastic-pool.md).
* Saiba como [criar uma aplicação Web que estabelece ligação à Base de Dados SQL no back-end](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md).

