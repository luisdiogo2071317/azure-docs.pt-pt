---
title: "Gerir a capacidade de computação no Azure SQL Data Warehouse (portal do Azure) | Microsoft Docs"
description: "Tarefas de portais do Azure para gerir poder de computação. Dimensionar os recursos de computação ao ajustar as DWUs. Ou, colocar em pausa e retomar a recursos de computação para reduzir os custos."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: f56e62576cae0c594f26bcddf44528032bd5ea69
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Gerir a capacidade de computação no Azure SQL Data Warehouse (portal do Azure)
Escala recursos de computação no Azure SQL Data Warehouse, utilizando o portal do Azure.

## <a name="scale-compute-power"></a>Capacidade de computação de escala
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Para alterar os recursos de computação:

1. Abra o [portal do Azure][Azure portal], abra a sua base de dados e clique em **escala**.

    ![Clique em escala][1]
2. No painel da escala, mova o controlo de deslize esquerda ou direita alterar a definição de DWU.

    ![Mover o controlo de deslize][2]
3. Clique em **Guardar**. É apresentada uma mensagem de confirmação. Clique em **Sim** para confirmar ou **não** para cancelar.

    ![Clicar em Guardar][3]


## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte [descrição geral da gestão][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png


<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
