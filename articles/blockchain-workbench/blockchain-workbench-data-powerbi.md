---
title: Utilizar dados de Azure Blockchain Workbench no Microsoft Power BI
description: Saiba como carregar e ver os dados de BD SQL do Azure Blockchain Workbench no Microsoft Power BI.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 2b909c0a8441010b87c913e5937d25c8127058f1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Utilizando os dados do Azure Blockchain Workbench com o Microsoft Power BI

Microsoft Power BI permite facilmente gerar relatórios de elevado desempenho de bases de dados de base de dados SQL utilizando o ambiente de trabalho do Power BI e, em seguida, publicá-los para [ https://www.powerbi.com ](http://www.powerbi.com).

Este artigo contém instruções passo a passo sobre como ligar à SQL Database do Blockchain Workbench Azure do dentro do Power BI desktop, criar um relatório e implementar o relatório no powerbi.com.

## <a name="prerequisites"></a>Pré-requisitos

* Transferir [Power BI Desktop](https://aka.ms/pbidesktopstore).

## <a name="connecting-powerbi-to-data-in-azure-blockchain-workbench"></a>A ligar o Power BI aos dados no Azure Blockchain Workbench

1.  Abra o ambiente de trabalho do Power BI.
2.  Selecione **obter dados**.

    ![Obter dados](media/blockchain-workbench-data-powerbi/get-data.png)
3.  Selecione **do SQL Server** da lista de tipos de origens de dados.

4.  Forneça o nome de servidor e base de dados na caixa de diálogo. Especifique se pretende importar os dados ou efetuar uma **DirectQuery**. Selecione **OK**.

    ![Selecionar o SQL Server](media/blockchain-workbench-data-powerbi/select-sql.png)

5.  Forneça as credenciais da base de dados para aceder ao Azure Blockchain Workbench. Selecione **base de dados** e introduza as suas credenciais.

    Se estiver a utilizar as credenciais criadas pelo processo de implementação do Azure Blockchain Workbench, o nome de utilizador é **dbadmin** e a palavra-passe é fornecido durante a implementação.

    ![Definições de base de dados SQL](media/blockchain-workbench-data-powerbi/db-settings.png)

6.  Uma vez à base de dados, o **navegador** caixa de diálogo apresenta as tabelas e vistas disponíveis na base de dados. As vistas foram concebidas para elaboração de relatórios e são todos os prefixo **vw**.

    ![Navegador](media/blockchain-workbench-data-powerbi/navigator.png)

7.  Selecione as vistas que pretende incluir. Para fins de demonstração, incluímos **vwContractAction**, que fornece detalhes sobre todas as ações ocorridas num contrato.

    ![Selecionar vistas](media/blockchain-workbench-data-powerbi/select-views.png)

Agora pode criar e publicar os relatórios tal como faria normalmente com o Power BI.

## <a name="next-steps"></a>Passos Seguintes

* [Vistas de base de dados no Azure Blockchain Workbench](blockchain-workbench-database-views.md)