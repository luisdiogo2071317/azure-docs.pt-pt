---
title: "Início rápido: Aumentar horizontalmente computação no armazém de dados de SQL do Azure - portal do Azure | Microsoft Docs"
description: "Tarefas de portais do Azure para gerir poder de computação. Dimensionar os recursos de computação ao ajustar as DWUs. Ou, colocar em pausa e retomar a recursos de computação para reduzir os custos."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 6b86042ed6b95ba49fa2089ba36b1dbe9a61cc40
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Início rápido: Dimensionar de computação no Azure SQL Data Warehouse no portal do Azure

Dimensionar a computação no Azure SQL Data Warehouse no portal do Azure. [Aumentar horizontalmente computação](sql-data-warehouse-manage-compute-overview.md) para um melhor desempenho ou escala fazer uma cópia de computação para reduzir os custos. 

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de começar

Pode dimensionar um armazém de dados que já tem, ou utilize [início rápido: criar e ligar - portal](create-data-warehouse-portal.md) para criar um armazém de dados com o nome **mySampleDataWarehouse**.  Este guia de introdução dimensiona **mySampleDataWarehouse**.

## <a name="scale-compute"></a>Dimensionar computação

No SQL Data Warehouse, pode aumentar ou diminuir os recursos de computação ao ajustar unidades do data warehouse. O [criar e ligar - portal](create-data-warehouse-portal.md) criado **mySampleDataWarehouse** e inicializado-lo com 400 DWUs. Os seguintes passos ajustar as DWUs para **mySampleDataWarehouse**.

Para alterar as unidades de armazém de dados:

1. Clique em **bases de dados SQL** na página da esquerda do portal do Azure.
2. Selecione **mySampleDataWarehouse** do **bases de dados SQL** página. É aberto o armazém de dados.
3. Clique em **escala**.

    ![Clique em escala](media/quickstart-scale-compute-portal/click-scale.png)

2. No painel de escala, mova o controlo de deslize esquerda ou direita alterar a definição de DWU.

    ![Mover o controlo de deslize](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Clique em **Guardar**. É apresentada uma mensagem de confirmação. Clique em **Sim** para confirmar ou **não** para cancelar.

    ![Clicar em Guardar](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Passos Seguintes
Agora que tem aprendeu como dimensionar a computação para o seu armazém de dados. Para saber mais sobre o Azure SQL Data Warehouse, avance para o tutorial para carregar dados.

> [!div class="nextstepaction"]
>[Carregar dados para o SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
