---
title: Restaurar um armazém de dados SQL do Azure - REST API | Microsoft Docs
description: Restaure um Azure SQL Data Warehouse com REST APIs.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 2e1874fdf7c11d98d369072739c5937caffe6e96
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="restore-an-azure-sql-data-warehouse-with-rest-apis"></a>Restaurar um armazém de dados SQL do Azure com as APIs REST
Restaure um Azure SQL Data Warehouse com REST APIs.

## <a name="before-you-begin"></a>Antes de começar
**Certifique-se a capacidade DTU.** Cada SQL Data Warehouse for alojado por um servidor lógico SQL (por exemplo, myserver.database.windows.net) que tem uma predefinição [quota de DTU](../sql-database/sql-database-what-is-a-dtu.md).  Antes, pode restaurar um SQL Data Warehouse, certifique-se de que o seu SQL server tem suficiente quota de DTU restantes para a base de dados a ser restaurado. Para pedir mais de DTU, pode [criar um pedido de suporte](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-an-active-or-paused-data-warehouse"></a>Restaurar um armazém de dados do Active Directory ou em pausa
Para restaurar um armazém de dados:

1. Obter a lista de pontos de restauro de base de dados utilizando a operação Get pontos de restauro de base de dados.
2. Iniciar o restauro utilizando o [criar pedido de restauro de base de dados](https://msdn.microsoft.com/library/azure/dn509571.aspx) operação.
3. Controlar o estado do seu restauro utilizando o [base de dados de estado da operação](https://msdn.microsoft.com/library/azure/dn720371.aspx) operação.

> [!NOTE]
> Depois do restauro foi concluída, pode configurar o armazém de dados recuperada, seguindo [configurar a base de dados após a recuperação](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="restore-a-deleted-data-warehouse"></a>Restaurar um armazém de dados eliminada
Para restaurar um armazém de dados eliminadas:

1. Listar todos os armazéns de dados eliminadas que possam ser restauradas utilizando o [lista que possam ser restaurada remover bases de dados](https://msdn.microsoft.com/library/azure/dn509562.aspx) operação.
2. Obter os detalhes para o armazém de dados eliminadas que pretende restaurar com a operação de [Get que possam ser restauradas removido da base de dados] [Get que possam ser restauradas removido da base de dados].
3. Iniciar o restauro utilizando o [criar pedido de restauro de base de dados](https://msdn.microsoft.com/library/azure/dn509571.aspx) operação.
4. Controlar o estado do seu restauro utilizando o [base de dados de estado da operação](https://msdn.microsoft.com/library/azure/dn720371.aspx) operação.

> [!NOTE]
> Para configurar o armazém de dados após o restauro foi concluída, consulte [configurar a base de dados após a recuperação](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as funcionalidades de continuidade do negócio das edições do SQL Database do Azure, leia o [descrição geral da continuidade de negócio da linha de base de dados do Azure SQL](../sql-database/sql-database-business-continuity.md).
