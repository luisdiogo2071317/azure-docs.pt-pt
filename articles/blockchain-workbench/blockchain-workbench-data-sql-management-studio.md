---
title: Utilizar os dados do Azure Blockchain Workbench com o SQL Server Management Studio
description: Saiba como ligar ao base de dados da Azure Blockchain Workbench ' SQL do dentro do SQL Server Management Studio.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 1c9ee1e94ba1195db027c3edfab2e2a96f181ca3
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Utilizando os dados do Azure Blockchain Workbench com o SQL Server Management Studio

Microsoft SQL Server Management Studio fornece a capacidade de rapidamente escrever e testar consultas em relação a BD do SQL do Azure Blockhain Workbench. Esta secção contém instruções passo a passo sobre como ligar ao base de dados da Azure Blockchain Workbench ' SQL do dentro do SQL Server Management Studio.

## <a name="prerequisites"></a>Pré-requisitos

* Transferir [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>A ligar o SQL Server Management Studio aos dados no Azure Blockchain Workbench

1. Abra o SQL Server Management Studio e selecione **Connect**.
2. Selecione **motor de base de dados**.

    ![Motor de base de dados](media/blockchain-workbench-data-sql-management-studio/database-engine.png)

3. No **ligar ao servidor** caixa de diálogo, introduza o nome do servidor e as credenciais da sua base de dados.

    Se estiver a utilizar as credenciais criadas pelo processo de implementação do Azure Blockchain Workbench, o nome de utilizador será **dbadmin** e a palavra-passe será fornecido durante a implementação.

    ![Introduza as credenciais do SQL](media/blockchain-workbench-data-sql-management-studio/sql-creds.png)

 4. SQL Server Management Studio mostra uma lista de bases de dados, vistas de base de dados e procedimentos armazenados na base de dados do Azure Blockchain Workbench.

    ![Lista de base de dados](media/blockchain-workbench-data-sql-management-studio/db-list.png)

5. Para ver os dados associados a qualquer uma das vistas da base de dados, pode gerar automaticamente uma instrução select através dos seguintes passos.
6. Clique com o botão direito do rato em qualquer uma das vistas da base de dados no Explorador de objetos.
7. Selecione **Script vista como**.
8. Escolha **SELECIONAR**.
9. Selecione **nova janela do Editor de consultas**.
10. É possível criar uma nova consulta selecionando **nova consulta**.

## <a name="next-steps"></a>Passos Seguintes

* [Vistas de base de dados no Azure Blockchain Workbench](blockchain-workbench-database-views.md)