---
title: Utilizar dados de Azure Blockchain Workbench no Microsoft Excel
description: Saiba como carregar e ver os dados de BD SQL do Azure Blockchain Workbench no Microsoft Excel.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 70297bd0af6322d0f3ac2c719d1827e4bc5898cd
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="view-azure-blockchain-workbench-data-with-microsoft-excel"></a>Ver os dados do Azure Blockchain Workbench com o Microsoft Excel

Pode utilizar o Microsoft Excel para ver os dados na BD do SQL do Azure Blockchain Workbench. Este artigo fornece os passos que necessários para:

* Ligar à base de dados Blockchain Workbench a partir do Microsoft Excel
* Observe a tabelas de base de dados do Blockchain Workbench e vistas
* Carregar dados de vista do Blockchain Workbench para Excel

## <a name="connect-to-the-blockchain-workbench-database"></a>Ligar à base de dados Blockchain Workbench

Para ligar a uma base de dados do Blockchain Workbench:

1. Abra o Microsoft Excel.
2. No **dados** separador, escolha **obter dados**.
3. Selecione **do Azure** e, em seguida, selecione **de SQL Database do Azure**.

   ![Ligar à base de Dados SQL do Azure](media/blockchain-workbench-data-excel/connect-sql-db.png)

4. No **base de dados do SQL Server** caixa de diálogo:

    * Para **servidor**, introduza o nome do servidor Blockchain Workbench.
    * Para **base de dados (opcional)**, introduza o nome da base de dados.

   ![Forneça o servidor de base de dados e base de dados](media/blockchain-workbench-data-excel/provide-server-db.png)

5. No **base de dados do SQL Server** caixa de diálogo a barra de navegação, selecione **base de dados**. Introduza o **Username** e **palavra-passe**e, em seguida, selecione **Connect**.

    > [!NOTE]
    > Se estiver a utilizar as credenciais que criou durante o processo de implementação do Azure Blockchain Workbench, a **nome de utilizador** é `dbadmin`. O **palavra-passe** é aquela que criou quando implementou o Blockchain Workbench.
    
   ![Forneça credenciais para aceder à base de dados](media/blockchain-workbench-data-excel/provide-credentials.png)

## <a name="look-at-database-tables-and-views"></a>Observe a base de dados tabelas e vistas

Abre a caixa de diálogo do navegador de Excel depois de ligar à base de dados. Pode utilizar o navegador para observar as tabelas e vistas na base de dados. As vistas foram concebidas para relatórios e os respetivos nomes têm o prefixo **vw**.

   ![Pré-visualização do navegador de Excel de uma vista](media/blockchain-workbench-data-excel/excel-navigator.png)

## <a name="load-view-data-into-an-excel-workbook"></a>Dados de vista de carga para um livro do Excel

O exemplo seguinte mostra como pode carregar dados a partir de uma vista de um livro do Excel.

1. No **navegador** barra de deslocamento, selecione o **vwContractAction** vista. O **vwContractAction** pré-visualização mostra todas as ações relacionadas com um contrato na base de dados Blockchain Workbench.
2. Selecione **carga** para obter todos os dados na vista e colocá-la no seu livro do Excel.

   ![Dados carregados a partir de uma vista](media/blockchain-workbench-data-excel/view-data.png)

Agora que tem os dados carregados, pode utilizar funcionalidades do Excel para criar os seus próprios relatórios através de metadados e de transações da base de dados do Azure Blockchain Workbench.

## <a name="next-steps"></a>Passos Seguintes

* [Vistas de base de dados no Azure Blockchain Workbench](blockchain-workbench-database-views.md)